# Running Jobs on GPU Nodes

This guide covers how to run GPU-accelerated jobs on NMTHPC's {{nmthpc_gpu_type}} GPU nodes.

## GPU Hardware Overview

NMTHPC features:

- **{{nmthpc_total_gpu_nodes}}** GPU nodes
- **{{nmthpc_gpu_type}}** GPUs
- High-bandwidth GPU memory
- NVLink or PCIe connectivity
- CUDA-capable architecture

## Requesting GPU Resources

### Interactive GPU Session

**Request a single GPU**:
```bash
$ srun --partition=gpu --gres=gpu:1 --mem=32G --time=02:00:00 --pty bash
```

**After allocation, verify GPU access**:
```bash
$ nvidia-smi
```

You should see information about the allocated GPU.

### Batch GPU Job

**Basic GPU job script**:
```bash
#!/bin/bash
#SBATCH --job-name=gpu_job
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=32G
#SBATCH --time=12:00:00
#SBATCH --output=gpu_job_%j.out

module load cuda/12.1

# Run GPU program
./my_gpu_program
```

### Requesting Multiple GPUs

**Single node, multiple GPUs**:
```bash
#SBATCH --gres=gpu:2      # Request 2 GPUs on one node
#SBATCH --mem=64G          # More memory for multi-GPU
```

**Multi-node GPU jobs** (if supported):
```bash
#SBATCH --nodes=2
#SBATCH --gres=gpu:2       # 2 GPUs per node = 4 GPUs total
#SBATCH --ntasks-per-node=2
```

```{note}
Check with HPC support for multi-node GPU capabilities and configuration on NMTHPC.
```

## GPU Monitoring

### Check GPU Status

**View GPU information**:
```bash
$ nvidia-smi
```

**Output includes**:

- GPU model and driver version
- Memory usage (used/total)
- GPU utilization percentage
- Running processes
- Temperature and power

### Continuous Monitoring

**Update every 2 seconds**:
```bash
$ watch -n 2 nvidia-smi
```

**Monitor specific metrics**:
```bash
$ nvidia-smi --query-gpu=timestamp,name,utilization.gpu,utilization.memory,memory.used,memory.total --format=csv -l 1
```

### Process-Level Monitoring

**Show GPU processes**:
```bash
$ nvidia-smi pmon
```

**GPU utilization over time**:
```bash
$ nvidia-smi dmon
```

## CUDA Programming

### Loading CUDA

**Load CUDA toolkit**:
```bash
$ module load cuda/12.1
```

**Verify CUDA installation**:
```bash
$ nvcc --version
$ which nvcc
```

### Compiling CUDA Code

**Simple CUDA compilation**:
```bash
$ module load cuda/12.1
$ nvcc -o my_program my_program.cu
```

**With optimization**:
```bash
$ nvcc -O3 -arch=sm_90 -o my_program my_program.cu
```

```{note}
H100 GPUs use compute capability 9.0 (`sm_90`). Check CUDA documentation for the exact architecture flag.
```

### CUDA Job Script Example

```bash
#!/bin/bash
#SBATCH --job-name=cuda_test
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --mem=16G
#SBATCH --time=01:00:00

module load cuda/12.1

# Compile
nvcc -o vector_add vector_add.cu

# Run
./vector_add

# Check GPU was used
nvidia-smi
```

## Deep Learning Frameworks

### PyTorch

**Interactive PyTorch session**:
```bash
$ srun --partition=gpu --gres=gpu:1 --mem=32G --time=02:00:00 --pty bash
$ module load cuda/12.1
$ module load python/3.11
$ python
>>> import torch
>>> print(torch.cuda.is_available())
True
>>> print(torch.cuda.device_count())
1
>>> print(torch.cuda.get_device_name(0))
NVIDIA H100
```

**PyTorch batch job**:
```bash
#!/bin/bash
#SBATCH --job-name=pytorch_train
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=8
#SBATCH --mem=64G
#SBATCH --time=24:00:00
#SBATCH --output=pytorch_%j.out

module load cuda/12.1
module load python/3.11

# Or use conda environment
# module load miniforge3
# source activate pytorch_env

python train.py --epochs 100 --batch-size 64
```

### TensorFlow

**TensorFlow job script**:
```bash
#!/bin/bash
#SBATCH --job-name=tensorflow_train
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --mem=64G
#SBATCH --time=48:00:00

module load cuda/12.1
module load python/3.11

# Verify GPU availability
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"

# Run training
python train_model.py
```

<!-- See [Training AI/ML Models on GPUs](../software/ai_ml_gpu.md) for comprehensive deep learning guidance. -->

### Multi-GPU Training

**PyTorch DataParallel**:
```python
import torch
import torch.nn as nn

# Model
model = MyModel()

# Use all available GPUs
if torch.cuda.device_count() > 1:
    print(f"Using {torch.cuda.device_count()} GPUs")
    model = nn.DataParallel(model)

model = model.cuda()
```

**SLURM script for multi-GPU**:
```bash
#!/bin/bash
#SBATCH --job-name=multi_gpu
#SBATCH --partition=gpu
#SBATCH --gres=gpu:2
#SBATCH --cpus-per-task=16
#SBATCH --mem=128G
#SBATCH --time=48:00:00

module load cuda/12.1
module load python/3.11

python train_multi_gpu.py
```



## Optimizing GPU Usage

### Check GPU Utilization

While your job runs, SSH to the compute node and check:

```bash
$ nvidia-smi
```

**Look for**:

- **GPU-Util**: Should be high (>80%) for compute-bound tasks
- **Memory-Usage**: Ensure you're not exceeding GPU memory
- **Processes**: Verify your process is using the GPU

### Common Issues and Solutions

**Low GPU utilization (<30%)**:

Possible causes:
- CPU bottleneck (increase `--cpus-per-task`)
- I/O bottleneck (optimize data loading)
- Small batch size (increase batch size)
- Data transfer overhead (use pinned memory, prefetching)

**Out of GPU memory**:

Solutions:
- Reduce batch size
- Use gradient accumulation
- Enable mixed precision training
- Use gradient checkpointing
- Request multiple GPUs and distribute model

**GPU not being used**:

Check:
- Code actually uses GPU (check with `nvidia-smi`)
- CUDA is loaded
- GPU-enabled version of software is loaded
- Code detects GPU correctly



## GPU Job Arrays

Run multiple GPU jobs as an array:

```bash
#!/bin/bash
#SBATCH --job-name=gpu_array
#SBATCH --output=logs/gpu_%A_%a.out
#SBATCH --array=1-10
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --mem=32G
#SBATCH --time=08:00:00

module load cuda/12.1
module load python/3.11

# Each task trains with different hyperparameters
CONFIG="config_${SLURM_ARRAY_TASK_ID}.yaml"

python train.py --config $CONFIG --gpu 0
```

See [Using SLURM Job Arrays](job_arrays.md) for more on job arrays.

## Best Practices

### Resource Requests

**CPU cores**: Request enough CPUs for data preprocessing

```bash
#SBATCH --cpus-per-task=8  # For data loading, preprocessing
```

Typically use 4-8 CPUs per GPU.

**Memory**: Request sufficient system RAM

```bash
#SBATCH --mem=64G  # System RAM, not GPU memory
```

GPU memory is fixed by hardware and doesn't need to be requested.

**Time limits**: GPU time is precious

- Test with short time limits first
- Request realistic time + 20% buffer
- Don't request maximum time "just in case"

### Code Optimization

**1. Batch size**: Maximize GPU memory usage
```python
# Increase batch size until GPU memory is ~90% full
batch_size = 64  # Tune this
```

**2. Data loading**: Don't bottleneck on CPU
```python
# PyTorch example
dataloader = DataLoader(
    dataset,
    batch_size=batch_size,
    num_workers=8,  # Match --cpus-per-task
    pin_memory=True  # Faster GPU transfer
)
```

**3. Minimize CPU-GPU transfers**:
```python
# Keep data on GPU when possible
data = data.cuda()
# Reuse GPU buffers
```

**4. Use built-in GPU operations**:
```python
# Good: GPU-optimized
torch.matmul(a, b)

# Bad: CPU fallback
numpy.matmul(a.cpu().numpy(), b.cpu().numpy())
```

### Monitoring During Development

**Add GPU logging to your code**:
```python
import torch

print(f"GPU available: {torch.cuda.is_available()}")
print(f"GPU count: {torch.cuda.device_count()}")
print(f"Current device: {torch.cuda.current_device()}")
print(f"Device name: {torch.cuda.get_device_name(0)}")

# During training
print(f"GPU memory allocated: {torch.cuda.memory_allocated() / 1e9:.2f} GB")
print(f"GPU memory reserved: {torch.cuda.memory_reserved() / 1e9:.2f} GB")
```

## Troubleshooting

### GPU Not Detected

**Check CUDA is loaded**:
```bash
$ module list
$ echo $CUDA_HOME
```

**Check GPU allocation**:
```bash
$ nvidia-smi
```

If no GPU shown, you're not on a GPU node or GPU wasn't allocated.

### CUDA Out of Memory

**Error**: `RuntimeError: CUDA out of memory`

**Solutions**:

1. **Reduce batch size**:
   ```python
   batch_size = 32  # Reduce from 64
   ```

2. **Clear GPU cache** (PyTorch):
   ```python
   torch.cuda.empty_cache()
   ```

3. **Use gradient accumulation**:
   ```python
   accumulation_steps = 4
   for i, (data, target) in enumerate(dataloader):
       output = model(data)
       loss = criterion(output, target) / accumulation_steps
       loss.backward()

       if (i + 1) % accumulation_steps == 0:
           optimizer.step()
           optimizer.zero_grad()
   ```

4. **Request multiple GPUs** and distribute model

### Slow Training

**Check GPU utilization**:
```bash
$ nvidia-smi dmon
```

If GPU util < 80%:

1. **Increase batch size**
2. **Add more data loading workers**:
   ```bash
   #SBATCH --cpus-per-task=16  # More CPUs for data loading
   ```
3. **Profile your code** to find bottlenecks

**Use profilers**:
```python
# PyTorch profiler
with torch.profiler.profile() as prof:
    model(input_batch)
print(prof.key_averages().table())
```

### Job Pending Long Time

GPU nodes are in high demand:

**Check wait reason**:
```bash
$ squeue -u $USER -o "%.18i %.30j %.20R"
```

**Reduce wait time**:

- Request fewer GPUs
- Request shorter time
- Submit during off-peak hours
- Use job arrays with `%` limiter

## Example: Complete GPU Training Workflow

```bash
#!/bin/bash
#SBATCH --job-name=model_training
#SBATCH --output=logs/train_%j.out
#SBATCH --error=logs/train_%j.err
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --cpus-per-task=8
#SBATCH --mem=64G
#SBATCH --time=24:00:00
#SBATCH --mail-type=END,FAIL
#SBATCH --mail-user=you@nmt.edu

# Exit on error
set -e

# Print job info
echo "Job started on $(hostname) at $(date)"
echo "Job ID: $SLURM_JOB_ID"
nvidia-smi

# Load modules
module purge
module load cuda/12.1
module load python/3.11

# Activate conda environment
source activate ml_env

# Verify GPU
python -c "import torch; print(f'GPU available: {torch.cuda.is_available()}')"

# Run training
python train.py \
    --data-dir /path/to/data \
    --output-dir results/$SLURM_JOB_ID \
    --epochs 100 \
    --batch-size 64 \
    --learning-rate 0.001 \
    --workers $SLURM_CPUS_PER_TASK

# Final GPU stats
nvidia-smi

echo "Job completed at $(date)"
```

## Additional Resources

<!-- - [Training AI/ML Models on GPUs](../software/ai_ml_gpu.md) --->
<!-- - [Python and Jupyter Notebooks](../software/python_jupyter.md) --->
- [Software Overview](../software/software_overview.md)

## Questions?

For questions about GPU computing on NMTHPC, contact <hpc@nmthpc.atlassian.net>.
