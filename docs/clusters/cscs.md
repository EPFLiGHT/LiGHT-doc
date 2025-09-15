# Connecting to CSCS

!!! danger "DO NOT RUN ON LOGIN NODE"

    When you establish a direct connection using `ssh` you connect to the login node. Everyone is on that node and as such **YOU SHOULD NEVER RUN ANY
	JOBS DIRECTLY ON THE LOGIN NODE**. If you want to run a process, *like a training*, you can run it on a [dedicated allocated job](#launching-job)


## Pre-setup (access to the CSCS)

Please ask Michael or Annie to add you to the CSCS project. Once you have been added, check your mail for the invitation link. You will to have to create an account.

## Connect to the login node

To connect to the login node, you will need to refresh your key every 24 hours. To refresh your keys, you need to execute the following script. Make sure to replace `$CSCS_USERNAME` with your CSCS username and the `$CSCS_PASSWORD` with your CSCS password. 

```
#!/bin/bash

# This script sets the environment properly so that a user can access CSCS
# login nodes via ssh. 

#    Copyright (C) 2023, ETH Zuerich, Switzerland
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU General Public License as published by
#    the Free Software Foundation, version 3 of the License.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU General Public License for more details.
#
#    You should have received a copy of the GNU General Public License
#    along with this program.  If not, see <http://www.gnu.org/licenses/>.
#
#    AUTHORS Massimo Benini


USERNAME=$CSCS_USERNAME
PASSWORD=$CSCS_PASSWORD
#read -p "Username : " USERNAME
#read -s -p "Password: " PASSWORD

function ProgressBar {
# Process data
    let _progress=(${1}*100/${2}*100)/100
    let _done=(${_progress}*4)/10
    let _left=40-$_done
# Build progressbar string lengths
    _fill=$(printf "%${_done}s")
    _empty=$(printf "%${_left}s")

# 1.2 Build progressbar strings and print the ProgressBar line
# 1.2.1 Output example:
# 1.2.1.1 Progress : [########################################] 100%
printf "\rSetting the environment : [${_fill// /#}${_empty// /-}] ${_progress}%%"
}

#Variables
_start=1
#This accounts as the "totalState" variable for the ProgressBar function
_end=100

#Params
MFA_KEYS_URL="https://sshservice.cscs.ch/api/v1/auth/ssh-keys/signed-key"

#Detect OS
OS="$(uname)"
case "${OS}" in
  'Linux')
    OS='Linux'
    ;;
  'FreeBSD')
    OS='FreeBSD'
    ;;
  'WindowsNT')
    OS='Windows'
    ;;
  'Darwin')
    OS='Mac'
    ;;
  *) ;;
esac

#OS validation
if [ "${OS}" != "Mac" ] && [ "${OS}" != "Linux" ]; then
  echo "This script works only on Mac-OS or Linux. Abording."
  exit 1
fi

#Read Inputs
echo
read -s -p "Enter OTP (6-digit code): " OTP
echo

if [ -z "${PASSWORD}" ]; then
    echo "Password is empty."
    exit 1
fi

if ! [[ "${OTP}" =~ ^[[:digit:]]{6} ]]; then
    echo "OTP is not valid, OTP must contains only six digits."
    exit 1
fi

ProgressBar 25 "${_end}"
echo "  Authenticating to the SSH key service..."

HEADERS=(-H "Content-Type: application/json" -H "accept: application/json")
KEYS=$(curl -s -S --ssl-reqd \
    "${HEADERS[@]}" \
    -d "{\"username\": \"$USERNAME\", \"password\": \"$PASSWORD\", \"otp\": \"$OTP\"}" \
    "$MFA_KEYS_URL")

if [ $? != 0 ]; then
    exit 1
fi

ProgressBar 50 "${_end}"
echo "  Retrieving the SSH keys..."

DICT_KEY=$(echo ${KEYS} | cut -d \" -f 2)
if [ "${DICT_KEY}" == "payload" ]; then
   MESSAGE=$(echo ${KEYS} | cut -d \" -f 6)
   ! [ -z "${MESSAGE}" ] && echo "${MESSAGE}"
   echo "Error fetching the SSH keys. Aborting."
   exit 1
fi

PUBLIC=$(echo ${KEYS} | cut -d \" -f 4)
PRIVATE=$(echo ${KEYS} | cut -d \" -f 8)

#Check if keys are empty:
if [ -z "${PUBLIC}" ] || [ -z "${PRIVATE}" ]; then
    echo "Error fetching the SSH keys. Aborting."
    exit 1
fi

ProgressBar 75 "${_end}"
echo "  Setting up the SSH keys into your home folder..."

#Check ~/.ssh folder and store the keys
echo ${PUBLIC} | awk '{gsub(/\\n/,"\n")}1' > ~/.ssh/cscs-key-cert.pub || exit 1
echo ${PRIVATE} | awk '{gsub(/\\n/,"\n")}1' > ~/.ssh/cscs-key || exit 1

#Setting permissions:
chmod 644 ~/.ssh/cscs-key-cert.pub || exit 1
chmod 600 ~/.ssh/cscs-key || exit 1

#Format the keys:
if [ "${OS}" = "Mac" ]
then
  sed -i '' -e '$ d' ~/.ssh/cscs-key-cert.pub || exit 1
  sed -i '' -e '$ d' ~/.ssh/cscs-key || exit 1
else [ "${OS}" = "Linux" ]
  sed '$d' ~/.ssh/cscs-key-cert.pub || exit 1
  sed '$d' ~/.ssh/cscs-key || exit 1
fi

ProgressBar 100 "${_end}"
echo "  Completed."

exit_code_passphrase=1
read -n 1 -p "Do you want to add a passphrase to your key? [y/n] (Default y) " reply; 
if [ "$reply" != "" ];
 then echo;
fi
if [ "$reply" = "${reply#[Nn]}" ]; then
      while [ $exit_code_passphrase != 0 ]; do
        ssh-keygen -f ~/.ssh/cscs-key -p
        exit_code_passphrase=$?
      done
fi

if (( $exit_code_passphrase == 0 ));
  then
    SUBSTRING=", using the passphrase you have set:";
  else
     SUBSTRING=":";
fi     

eval `ssh-agent -s`
ssh-add -t 1d ~/.ssh/cscs-key
```

We strongly suggest to store this script in a file as you will have to execute it every day. If you don't want to have your login ID stored in a script, you can comment out the lines:

```
#read -p "Username : " USERNAME
#read -s -p "Password: " PASSWORD
```

and remove the lines:

```
USERNAME=$CSCS_USERNAME
PASSWORD=$CSCS_PASSWORD
```

## Setup your ssh config

Add the following lines to the `~/.ssh/config` file:

```
Host ela
    HostName ela.cscs.ch
    User $CSCS_USERNAME
    ForwardAgent yes
    ForwardX11 yes
    forwardX11Trusted yes
	IdentityFile ~/.ssh/cscs-key


Host todi
    HostName todi.cscs.ch
    User $CSCS_USERNAME
    ProxyJump ela
    ForwardAgent yes
    ForwardX11 yes
    forwardX11Trusted yes
	IdentityFile ~/.ssh/cscs-key

Host clariden
    HostName clariden.cscs.ch
    User $CSCS_USERNAME
    ProxyJump ela
    ForwardAgent yes
    ForwardX11 yes
    forwardX11Trusted yes
    IdentityFile ~/.ssh/cscs-key
```

To connect to the cluster, run the following:

```
# Your terminal

ssh clariden
```

This opens a terminal on the CSCS login node. 

## Setup Github

To operate on private repositories on GitHub. You can either generate a SSH key pairs or use a GitHub personal access token (GitHub PAT). We recommand doing the second option but both options are viable.

To generate a GitHub PAT, follow those [instructions](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#creating-a-personal-access-token-classic). Make sure that this PAT is stored somewhere

For this tutorial, we are gonna use the `MultiMeditron` training pipeline setup. Clone the MultiMeditron repository in your user directory:

```
# CSCS login node

mkdir /users/$CSCS_USERNAME/meditron
cd /users/$CSCS_USERNAME/meditron

# Use the HTTPS command if you used github personal access token and the SSH based if you have added SSH key-pairs
git clone https://github.com/OpenMeditron/MultiMeditron.git
git clone git@github.com:OpenMeditron/MultiMeditron.git
```

When GitHub asks for your password, input the PAT that you have generated in this step.

## Create a personal folder in the capstor partition

```
# CSCS login node

mkdir /capstor/store/cscs/swissai/a127/homes/$CSCS_USERNAME
```

This personal folder on the capstor will be mainly used to store your huggingface home and your big files that don't fit in your users personal folder

## Setup the environment on the cluster

> *__IMPORTANT NOTE__:* NEVER RUN ANY COMPUTE JOB ON THE LOGIN NODE (or else you will slow down everyone on the cluster). THE LOGIN NODE SHOULD ONLY BE USED TO SCHEDULE JOB.

The terminal will spawn you into the `/users/$CSCS_USERNAME` directory.

When running job, you will need to execute your job inside docker images. This is done by using `.toml` files that specify which docker image, environment variables are gonne be set when running the job. Create a folder `.edf` in `/users/$CSCS_USERNAME`:

```
# CSCS login node

mkdir /users/$CSCS_USERNAME/.edf
```


Create a `.edf/multimodal.toml` file:

```
image = "/capstor/store/cscs/swissai/a127/meditron/docker/multimeditron_latest.sqsh"
mounts = ["/capstor", "/iopsstor", "/users"]

writable = true

workdir = "/users/$CSCS_USERNAME/meditron/MultiMeditron"

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

Notice 2 things:

* We specify the path to the `.sqsh` file in the `image` attribute. This is the image used by the job that stores all of the dependencies.
* We specify the path to the MultiMeditron repo in the `workdir` attribute. This is the directory where we spawn when the job is launched.

Note that for other types of job, you will probably require a different image and a different working directory.

## Launching job

There are 2 types of job that you can launch:

- Interactive using `srun` (which gives you a terminal)
- Non-interactive using `sbatch` (which schedule a job)

### Interactive job

On the login node, you can launch an interactive job by executing the following command:

```
srun --time=1:29:59 --partition debug -A a127 --environment=/users/$CSCS_USERNAME/.edf/multimodal.toml --pty bash
```

Here is a breakdown of the command:

- `--time` is the maximum running time of the job (here, the job runs for 1h30 before it gets killed)
- `--partition debug` is the node partition in which the job executed. As of 14/08/2025, there are 3 partitions:

    - `normal`: with a maximum running time of 12 hours and no limit on the number of distributed nodes. This partition is the partition used for non-interactive jobs and long interactive jobs
    - `debug`: with a maximum running time of 1h30 with only one node. This partition is meant for interactive jobs
    - `xfer`: this partition is meant for data transfer and doesn't claim any GPU

To check if you have been allocated a node, run the following command in another terminal:

```
# CSCS login node

squeue --me --start
```

This command will give you a dynamic estimation of the scheduled time (may change as people pass you in the priority queue). Note that this command doesn't output anything if your job has been allocated.

Once you have been allocated a job, you will have a terminal inside the allocated node. Make sure that your `bash prompt` is of the form `$CSCS_USERNAME@nidxxxxxx` (and __not__ `[clariden][$CSCS_USERNAME@clariden-lnxxx]`. Run:

```
# CSCS job node

nvidia-smi
```

to make sure you have 4 GPUs and that you have the driver installed.

Once you are done with the job. Type `exit` to exit the terminal to exit the terminal. This will cancel your job.

### Non-interactive job

To launch a non-interactive job, you need to create a sbatch script. Create a file called `sbatch_train.sh`:

```
#!/bin/bash
#SBATCH --job-name demo-job
#SBATCH --output /users/$CSCS_USERNAME/meditron/reports/R-%x.%j.out
#SBATCH --error /users/$CSCS_USERNAME/meditron/reports/R-%x.%j.err
#SBATCH --nodes 1         # number of Nodes
#SBATCH --ntasks-per-node 1     # number of MP tasks. IMPORTANT: torchrun represents just 1 Slurm task
#SBATCH --gres gpu:4        # Number of GPUs
#SBATCH --cpus-per-task 288     # number of CPUs per task.
#SBATCH --time 0:59:59       # maximum execution time (DD-HH:MM:SS)
#SBATCH --environment /users/$CSCS_USERNAME/.edf/multimodal.toml
#SBATCH -A a127

export WANDB_DIR=/capstor/store/cscs/swissai/a127/homes/$CSCS_USERNAME/wandb
export WANDB_MODE="offline"
export HF_TOKEN=<insert your token>

PRELUDE="cd /users/$CSCS_USERNAME/meditron/multimodal/MultiMeditron/ && source setup.sh"

export CUDA_LAUNCH_BLOCKING=1
echo "START TIME: $(date)"
# auto-fail on any errors in this script
set -eo pipefail
# logging script's variables/commands for future debug needs
set -x
######################
### Set enviroment ###
######################
GPUS_PER_NODE=4
echo "NODES: $SLURM_NNODES"
######## Args ########
export HF_HOME=/capstor/store/cscs/swissai/a127/homes/$CSCS_USERNAME/hf_home

######################
######################
#### Set network #####
######################
MASTER_ADDR=$(scontrol show hostnames $SLURM_JOB_NODELIST | head -n 1)
MASTER_PORT=6200
######################
# note that we don't want to interpolate `\$SLURM_PROCID` till `srun` since otherwise all nodes will get
# 0 and the launcher will hang
#
# same goes for `\$(hostname -s|tr -dc '0-9')` - we want it to interpolate at `srun` time

LAUNCHER="
  torchrun \
  --nproc_per_node $GPUS_PER_NODE \
  --nnodes $SLURM_NNODES \
  --node_rank \$SLURM_PROCID \
  --rdzv_endpoint $MASTER_ADDR:$MASTER_PORT \
  --rdzv_backend c10d \
  --max_restarts 0 \
  --tee 3 \
  "

export CMD="$LAUNCHER train.py --config config/config_alignment.yaml"

echo $CMD

# srun error handling:
# --wait=60: wait 60 sec after the first task terminates before terminating all remaining tasks
SRUN_ARGS=" \
  --cpus-per-task $SLURM_CPUS_PER_TASK \
  --jobid $SLURM_JOB_ID \
  --wait 60 \
  -A a127 \
  --reservation=sai-a127
  "
# bash -c is needed for the delayed interpolation of env vars to work
srun $SRUN_ARGS bash -c "$PRELUDE && $CMD"
echo "END TIME: $(date)"
```

Make sure to replace all the `$CSCS_USERNAME` by your username and the `$HF_TOKEN` with your huggingface token. Pay attention to the following parameters:

- `#SBATCH --job-name demo-job` sets the job name to `demo-job`
- `#SBATCH --nodes 1` means that we are claiming one node (of 4 GPUs). You should increase this if you are launching bigger jobs
- `#SBATCH --output /users/$CSCS_USERNAME/meditron/reports/R-%x.%j.out` and `#SBATCH --error /users/$CSCS_USERNAME/meditron/reports/R-%x.%j.err` mean that this will create a folder `/users/$CSCS_USERNAME/meditron/reports` that stores all the job logs
- Note that here, we execute a training of MultiMeditron with `config/config_alignment.yaml`, thus you need to make sure that the paths of the dataset are correct
- Note that the part which follows the `#SBATCH` commands will be executed on every node

To queue your job, run:

```
# CSCS login node

sbatch sbatch_train.sh
```
 

You can check if your job has been allocated GPUs by running:

```
# CSCS login node

squeue --me
```
This command gives you the `JOBID` of the job you have launched

Once the job enters the `R` state (for running), the job is running. You can check the logs of your job by going into the `reports` directory:

```
# Login node

cd /users/$CSCS_USERNAME/meditron/reports/
tail -f R-%x.%j.err
```

where you need to replace `R-%x.%j.err` by the actual report name.


You can either let the job finishes or cancels the job.

```
# Login node

scancel $JOBID
```

where `$JOBID`is the `JOBID` that you get when running `squeue --me`

