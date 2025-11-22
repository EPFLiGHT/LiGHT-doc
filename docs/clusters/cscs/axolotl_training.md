# Axolotl Training on the CSCS

This tutorial describes how to launch training with [axolotl](https://github.com/axolotl-ai-cloud/axolotl). Axolotl is the pipeline that we use to finetune LLMs and VLMs.

This tutorial assumes that:

1. You have done the [setup to connect to the CSCS](cscs.md)
2. You have a working axolotl docker image with the correct version of transformers. If you wish to build an image with updated version, please check [this](cscs_docker.md); in this tutorial, we provide a Dockerfile to build axolotl with the latest version on GitHub.

## What is axolotl?

axolotl is a training pipeline that makes training and fine-tuning easier. axolotl configures training using a [YAML file](https://docs.axolotl.ai/docs/config-reference.html) that provides the training arguments, datasets and hyperparameters.

As a user, you need to do 2 things:

1. Put your datasets in a compatible format
2. Configure your training

## Setup

Create a TOML file to define the axolotl environment. On the CSCS, create a `~/.edf/axolotl.toml`:

```toml

# Put this or replace it with an updated axolotl image
image = "/capstor/store/cscs/swissai/a127/meditron/docker/axolotl.sqsh"

mounts = ["/capstor", "/iopsstor", "/users"]

writable = true

[annotations]
com.hooks.aws_ofi_nccl.enabled = "true"
com.hooks.aws_ofi_nccl.variant = "cuda12"

[env]
HF_HOME = "${SCRATCH}/hf"
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

## Formatting datasets in the right format

The first step is to convert your dataset into the right format. axolotl supports both datasets for LLMs and VLMs

### Text-only data

For text-only data, axolotl supports both pre-training and conversational data. We recommend storing the dataset in a [JSONL format](https://jsonlines.org/) where each line is a sample.

**Pre-training data:** Pre-training samples must be in this [format](https://docs.axolotl.ai/docs/dataset-formats/pretraining.html):

```json
{ "text" : "Lorem ipsum dolor sit amet, consectetur adipiscing elit." }
```

Pre-training data can be used when:

- You have big chunks of documents that describes the knowledge that you want to train on
- You have "low-quality" documents but a lot of them


**Conversational data:** Conversational samples must be in this [format](https://docs.axolotl.ai/docs/dataset-formats/conversation.html):

```json
{
    "messages": [
        { "role": "user", "content": "What is the capital of Switzerland?" } 
        { "role": "assistant", "content": "The capital of Switzerland is Bern" }
    ]
}
```

The `"messages"`, `"role"` and `"content"` are flexible and can be replaced by something else. For instance, you can also use the following format:

```json
{
    "conversations": [
        { "from": "user", "value": "What is the capital of Switzerland?" } 
        { "from": "assistant", "value": "The capital of Switzerland is Bern" }
    ]
}

```

Then in the axolotl configuration file, you will have to specify those fields correctly so that your dataset gets tokenized correctly.

### Multimodal data

TODO!

## Configure your training

The second step is to create a YAML configuration file that describes your training. 

Create a folder in your home directory to store your axolotl configurations:

```bash
mkdir -p ~/meditron/axolotl_config
cd ~/meditron/axolotl_config
```

Here is an example configuration file for fine-tuning the Apertus model. Store this file as `axolotl_apertus_8b.yaml`:

```yaml

base_model: swiss-ai/Apertus-8B-Instruct-2509
plugins:
  - axolotl.integrations.cut_cross_entropy.CutCrossEntropyPlugin

datasets:
  - path: /capstor/store/cscs/swissai/a127/path/to/conversations.jsonl
    type: chat_template
    split: train
    field_messages: conversations
    message_property_mappings:
      role: from
      content: value
  - path: /capstor/store/cscs/swissai/a127/path/to/pretrain.jsonl
    ds_type: json
    type: completion
    field: text


# This is the path where axolotl caches the prepared dataset
dataset_prepared_path: /capstor/store/cscs/swissai/a127/homes/$USER/axolotl_datasets/last_run_prepared

# Output directory where model checkpoints and logs will be saved
output_dir: /capstor/store/cscs/swissai/a127/meditron/models/tutorials/axolotl_apertus_8b

# Data loading and processing settings
shuffle_merged_datasets: true
dataset_processes: 64 # Avoid RAM OOM issues by lowering this value if needed

# If your model supports flash attention, enable it
flash_attention: true
flash_attn_rms_norm: true
flash_attn_fuse_qkv: false

# Enable/Disable sample packing
sample_packing: true
sequence_len: 2048
group_by_length: false
pad_to_sequence_len: true

# Gradient checkpointing settings: enable to save VRAM
gradient_checkpointing: true
gradient_checkpointing_kwargs:
  use_reentrant: false

# Control batch size and number of epochs
gradient_accumulation_steps: 2
micro_batch_size: 8
num_epochs: 1

# Learning rate scheduler and optimizer settings
optimizer: adamw_torch
optim_args:
  fused: true
learning_rate: 1.0e-5
warmup_ratio: 0.0
weight_decay: 0.05
lr_scheduler: cosine
cosine_min_lr_ratio: 0.1
max_grad_norm: 1.0

# Disable evaluation
evals_per_epoch: 0
eval_set_size: 0.0
eval_table_size: null

# Checkpointing and logging settings
resume_from_checkpoint: null
logging_steps: 1
saves_per_epoch: 2

# Model and tokenizer types (usually AutoModelForCausalLM and AutoTokenizer for causal LLMs)
tokenizer_type: AutoTokenizer
type: AutoModelForCausalLM

# Weights & Biases logging configuration
wandb_entity: 
wandb_log_model: 
wandb_name: Meditron-Apertus-8B
wandb_project: tutorial
wandb_watch: null

ddp_find_unused_parameters: true
deepspeed: /users/$USER/meditron/axolotl_config/deepspeed.json

```

In this example, we only use a conversational dataset and a pretrain dataset for training.

For the conversational dataset, we specify that the messages are stored in the `conversations` field, and that the `from` and `value` fields correspond to the role and content of each message respectively. Here is an example of how the data should look like:

```json
{
  "conversations": [
    {
      "from": "system",
      "value": "Answer this question truthfully"
    },
    {
      "from": "user",
      "value": "What is the capital of Switzerland?"
    },
    {
      "from": "assistant",
      "value": "The capital of Switzerland is Bern"
    }
  ]
}
```

The pretraining dataset is simply a JSONL file where each line contains a `text` field with the document to pretrain on:

```json
{ "text" : "Lorem ipsum dolor sit amet, consectetur adipiscing elit." }
```

To enable Deepspeed Zero-3 optimization, create a `deepspeed.json` file in the same folder with the following content:

```json
{
    "bf16": {
        "enabled": true
    },
    "zero_optimization": {
        "stage": 3,
        "offload_optimizer": {
          "device": "cpu",
          "pin_memory": true
        },
        "overlap_comm": false,
        "contiguous_gradients": true,
        "reduce_bucket_size": "auto",
        "stage3_prefetch_bucket_size": "auto",
        "stage3_param_persistence_threshold": "auto",
        "sub_group_size": 1e9,
        "stage3_max_live_parameters": 1e9,
        "stage3_max_reuse_distance": 1e9,
        "stage3_gather_16bit_weights_on_model_save": true
    },
    "gradient_accumulation_steps": "auto",
    "train_micro_batch_size_per_gpu": "auto",
    "gradient_clipping": 1.0,
    "wall_clock_breakdown": false,
    "activation_checkpointing": {
        "partition_activations": false,
        "contiguous_memory_optimization": false,
        "cpu_checkpointing": false
    },
    "flops_profiler": {
        "enabled": false
    },
    "aio": {
        "block_size": 1048576,
        "queue_depth": 8,
        "single_submit": false,
        "overlap_events": false
    }
}
```

## Launch training on the CSCS

Now that you have your dataset and configuration file ready, you can launch the training on the CSCS.

### Testing your configuration interactively

First launch a job in interactive mode to test your configuration:

```bash
srun --time=2:59:59 --partition normal -A a127 --environment=/users/$USER/.edf/axolotl.toml 
```

This will launch an interactive session with access to 4 GPUs. Inside the session, launch the training with:

```bash
torchrun --nproc_per_node=4 -m axolotl.cli.train /users/$USER/meditron/axolotl_config/axolotl_apertus_8b.yaml
```

Once you see something similar to the following, your training is running correctly:
```bash
{ "loss" : 2.34567, "epoch": 0.01, "step": 10, ... }
```
The logging above will be printed every `logging_steps` that you defined in the YAML configuration file.

When you are satisfied with your configuration, exit your interactive session with:
```bash
exit
```

### Launching a batch job

Now, you can create a batch job to launch the training. Create a SLURM script `launch_axolotl_apertus_8b.sh` with the following content:

```bash
#!/bin/bash
#SBATCH --job-name meditron-tutorial
#SBATCH --chdir /users/$USER/meditron/axolotl_config
#SBATCH --output /users/$USER/meditron/reports/R-%x.%j.out
#SBATCH --error /users/$USER/meditron/reports/R-%x.%j.err
#SBATCH --nodes 4               # number of Nodes
#SBATCH --ntasks-per-node 1     # number of MP tasks. IMPORTANT: torchrun represents just 1 Slurm task
#SBATCH --gres gpu:4        # Number of GPUs
#SBATCH --cpus-per-task 288     # number of CPUs per task.
#SBATCH --time 11:59:59       # maximum execution time (DD-HH:MM:SS)
#SBATCH --environment /users/$USER/.edf/axolotl.toml
#SBATCH -A a127

export WANDB_DIR=/capstor/store/cscs/swissai/a127/homes/$USER/wandb
export WANDB_API_KEY=<your_wandb_api_key>
export WANDB_MODE="online"

# Put Triton on a non-NFS directory
export TRITON_CACHE_DIR=/tmp/$USER/triton_cache

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
AXOLOTL_CONFIG_FILE=/users/$USER/meditron/axolotl_config/axolotl_apertus_8b.yaml

export HF_HOME=$SCRATCH/hf
export HF_TOKEN=<your_huggingface_token>
mkdir -p $HF_HOME

######################
######################
#### Set network #####
######################
MASTER_ADDR=$(scontrol show hostnames $SLURM_JOB_NODELIST | head -n 1)
MASTER_PORT=6300
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

export CMD="$LAUNCHER -m axolotl.cli.train $AXOLOTL_CONFIG_FILE"
echo $CMD
# srun error handling:
# --wait=60: wait 60 sec after the first task terminates before terminating all remaining tasks
SRUN_ARGS=" \
    --cpus-per-task $SLURM_CPUS_PER_TASK \
    --jobid $SLURM_JOB_ID \
    --wait 60 \
    -A a127 \
    "
# bash -c is needed for the delayed interpolation of env vars to work

srun $SRUN_ARGS bash -c "$CMD"
echo "END TIME: $(date)"
```

Make sure to replace the `$USER` with your username, and set your Weights & Biases and Hugging Face tokens.

You can increase the number of nodes by changing the `#SBATCH --nodes` parameter according to your needs.

Finally, launch the job with:

```bash
sbatch launch_axolotl_apertus_8b.sh
```

You can monitor the status of your job with:

```bash
squeue -u $USER
```

To get the logs of your job, check the files in the `~/meditron/reports/` folder.
```bash
cd ~/meditron/reports/
tail -f R-meditron-tutorial.<job_id>.err
```

That's it! You have successfully launched an axolotl training job on the CSCS.

## Further reading

- [Axolotl documentation](https://docs.axolotl.ai/)
- [CSCS documentation](https://docs.cscs.ch/)
