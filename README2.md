## Introduction

This directory contains our pytorch implementation of Transformer-XL. Note that our state-of-the-art results reported in the paper were obtained by training the model on a large-scale TPU cluster, and our pytorch codebase currently does not support distributed training. Here we provide two sets of hyperparameters and scripts:
- `*large.sh` are for the SoTA setting with large models which might not be directly runnable on a local GPU machine.
- `*base.sh` are for the base models which can be run on a few GPUs.

The pytorch implementation produces similar results to the TF codebase under the same settings in our preliminary experiments.


## Prerequisite

- Pytorch 0.4: `conda install pytorch torchvision -c pytorch`


## Data Prepration

`bash getdata.sh`

## Training and Evaluation

#### Other options:

- `--batch_chunk`: this option allows one to trade speed for memory. For `batch_chunk > 1`, the program will split each training batch into `batch_chunk` sub-batches and perform forward and backward on each sub-batch sequentially, with the gradient accumulated and divided by `batch_chunk`. Hence, the memory usage will propertionally lower while the computation time will inversely higher. 
- `--div_val`: when using adaptive softmax and embedding, the embedding dimension is divided by `div_val` from bin $i$ to bin $i+1$. This saves both GPU memory and the parameter budget.
- `--fp16` and `--dynamic-loss-scale`: Run in pseudo-fp16 mode (fp16 storage fp32 math) with dynamic loss scaling. 
  - Note: to explore the `--fp16` option, please make sure the `apex` package is installed (https://github.com/NVIDIA/apex/).
- To see performance without the recurrence mechanism, simply use `mem_len=0` in all your scripts.


#### Other datasets:

- `Text8` character-level language modeling: check out `run_text8_base.sh`
- `lm1b` word-level language modeling: check out `run_lm1b_base.sh`
