# Building images for the CSCS

This tutorial assumes that you followed the CSCS setup tutorial at [cscs.md](cscs.md)

## Claim a job to build docker image

Connect to the CSCS login node:

```bash
ssh clariden
```

Claim a job:

```bash
# On the login node

srun --time 11:59:59 -p normal -A a127 --pty bash
```

Make sure to put a big timeout as building docker images can take a lot of time

## Configure podman storage

In order to use podman on alps, you need to create a valid container storage configuration file at `$HOME/.config/containers/storage.conf`. In pratice you need to crete the following file at `/users/<USERNAME>/.config/containers/storage.conf`. 

```toml
[storage]
driver = "overlay"
runroot = "/dev/shm/$USER/runroot"
graphroot = "/dev/shm/$USER/root"

[storage.options.overlay]
mount_program = "/usr/bin/fuse-overlayfs-1.13"
```

## Building the image

Create your Dockerfile and name it `Dockerfile`. For example, this is the Dockerfile used to run axolotl:

```dockerfile
FROM nvcr.io/nvidia/pytorch:24.07-py3

# setup
RUN apt-get update && apt-get install python3-pip python3-venv -y
RUN pip install --upgrade pip setuptools

# Axolotl installs
RUN MAX_JOBS=24 pip install flash-attn==2.6.2 --no-build-isolation
RUN pip install trl==0.9.6
RUN pip install fschat@git+https://github.com/lm-sys/FastChat.git@27a05b04a35510afb1d767ae7e5990cbd278f8fe
RUN pip install deepspeed==0.14.4

RUN MAX_JOBS=8 TORCH_CUDA_ARCH_LIST="9.0" pip install -v -U git+https://github.com/facebookresearch/xformers.git@main#egg=xformers
RUN pip install transformers@git+https://github.com/huggingface/transformers.git@026a173a64372e9602a16523b8fae9de4b0ff428

COPY axolotl/ /workspace/axolotl
WORKDIR /workspace/axolotl
RUN pip install -e .
```

Note that this Dockerfile assumes that you have the [axolotl repository](https://github.com/axolotl-ai-cloud/axolotl) cloned in `path/to/root_directory`

Run:

```bash
# Recommended: change the tag from my_awesome_image to a more meaningful one

podman build -f /path/to/root_directory/Dockerfile -t my_awesome_image path/to/root_directory
```

You can change the base docker image (the `FROM` value) to have a more recent version of the CUDA driver. You can check the available tags [here](https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch/tags).

**IMPORTANT NOTE:** Note that the CSCS cluster uses an ARM64 architecture. Make sure that you are building your packages for this architecture. Some libraries like VLLM may be trickier to install as they rely on low-level optimization.

## Export the docker image to a .sqsh file

Run the following command, you can change the `my_custom_image.sqsh` file name to a more meaningful one.

```bash
enroot import -o /capstor/store/cscs/swissai/a127/meditron/docker/my_custom_image.sqsh podman://localhost/my_awesome_image:latest
```

Set the correct permissions:

```bash
setfacl -b /capstor/store/cscs/swissai/a127/meditron/docker/my_custom_image.sqsh
chmod +r /capstor/store/cscs/swissai/a127/meditron/docker/my_custom_image.sqsh
```

## Use your new Docker image

To use your new Docker image, create a new toml file in `$HOME/.edf` (in this example we name it `example.toml`):

```toml
image = "/capstor/store/cscs/swissai/a127/meditron/docker/my_custom_image.sqsh"
mounts = ["/capstor", "/iopsstor", "/users"]

writable = true

# Uncomment this to set a particular working directory
# workdir = "/path/to/workdir"

[annotations]
com.hooks.aws_ofi_nccl.enabled = "true"
com.hooks.aws_ofi_nccl.variant = "cuda12"

[env]
CUDA_CACHE_DISABLE = "1"
NCCL_NET = "AWS Libfabric"
NCCL_CROSS_NIC = "1"
NCCL_NET_GDR_LEVEL = "PHB"
FI_CXI_DISABLE_HOST_REGISTER = "1"
FI_MR_CACHE_MONITOR = "userfaultfd"
FI_CXI_DEFAULT_CQ_SIZE = "131072"
FI_CXI_DEFAULT_TX_SIZE = "32768"
FI_CXI_RX_MATCH_MODE = "software"
FI_CXI_SAFE_DEVMEM_COPY_THRESHOLD = "16777216"
FI_CXI_COMPAT = "0"
```

Note the line starting with `image =`

Then, to claim an interactive job with this image:

```bash
srun --time=1:29:59 --partition debug -A a127 --environment=$HOME/.edf/example.toml --pty bash
```

For non interactive job. Look for the `--environment` argument in the srun arguments and change it to your desired `.toml` file (here `example.toml`
