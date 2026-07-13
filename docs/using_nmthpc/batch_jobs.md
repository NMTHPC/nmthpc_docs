# Running Batch Jobs and SLURM Basics

Batch jobs are the primary way to run computational work on NMTHPC. This guide covers SLURM batch job basics and best practices.

## What are Batch Jobs?

Batch jobs:

- Run without user interaction
- Are queued and run when resources are available
- Can run overnight, over weekends, or for extended periods
- Are defined by shell scripts with SLURM directives
- Are ideal for production computational work

## Basic SLURM Workflow

1. **Write a job script** with resource requests and commands
2. **Submit the job** to the queue with `sbatch`
3. **Monitor the job** with `squeue` and `sacct`
4. **Review output** when job completes

## Your First Batch Job

### Simple Job Script

Create a file named `simple_job.sh`:

```bash
#!/bin/bash
#SBATCH --job-name=my_first_job
#SBATCH --output=output_%j.txt
#SBATCH --ntasks=1
#SBATCH --time=00:10:00
#SBATCH --mem=1G

# Print some information
echo "Job started on $(hostname) at $(date)"
echo "Job ID: $SLURM_JOB_ID"
echo "Running on $SLURM_NNODES node(s)"

# Do some work
sleep 30
echo "Hello from NMTHPC!"

# Finish
echo "Job finished at $(date)"
```

### Submit the Job

```bash
$ sbatch simple_job.sh
Submitted batch job 12345
```

### Check Job Status

```bash
$ squeue -u $USER
```

### View Output

After job completes:
```bash
$ cat output_12345.txt
```

## SLURM Script Components

### The Shebang

```bash
#!/bin/bash
```

Must be the first line. Specifies the shell interpreter.

### SLURM Directives

Lines starting with `#SBATCH` are SLURM directives:

```bash
#SBATCH --option=value
```

**Common directives**:

```bash
#SBATCH --job-name=my_job           # Job name
#SBATCH --output=output_%j.txt      # Output file (%j = job ID)
#SBATCH --error=error_%j.txt        # Error file (separate from output)
#SBATCH --ntasks=4                  # Number of tasks (MPI processes)
#SBATCH --cpus-per-task=1           # CPUs per task (for threading)
#SBATCH --nodes=1                   # Number of nodes
#SBATCH --mem=16G                   # Memory per node
#SBATCH --time=04:00:00             # Time limit (HH:MM:SS)
#SBATCH --partition=cpu.std         # Partition name
#SBATCH --mail-type=END,FAIL        # Email notifications
#SBATCH --mail-user=you@nmt.edu     # Your email
```

### Environment and Commands

After directives, add your actual work:

```bash
# Load modules
module load python/3.11

# Set environment variables
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

# Change to working directory (usually already there)
cd $SLURM_SUBMIT_DIR

# Run your program
python my_script.py
```

## Resource Requests

### CPU Resources

**Single task**:
```bash
#SBATCH --ntasks=1
```

**Multiple tasks** (for MPI):
```bash
#SBATCH --ntasks=16          # 16 MPI processes
```

**Multithreaded application**:
```bash
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8    # 8 threads
```

**Hybrid MPI + OpenMP**:
```bash
#SBATCH --ntasks=4           # 4 MPI processes
#SBATCH --cpus-per-task=4    # 4 threads each = 16 total CPUs
```

### Memory Requests

**Total memory per node**:
```bash
#SBATCH --mem=32G            # 32 GB total
```

**Memory per CPU**:
```bash
#SBATCH --mem-per-cpu=4G     # 4 GB per CPU
```

```{tip}
Use `--mem` for most cases. Use `--mem-per-cpu` when memory needs scale with CPU count.
```

### Time Limits

**Format: Days-Hours:Minutes:Seconds**

```bash
#SBATCH --time=01:00:00      # 1 hour
#SBATCH --time=04:30:00      # 4.5 hours
#SBATCH --time=2-00:00:00    # 2 days
#SBATCH --time=7-12:00:00    # 7.5 days
```

```{warning}
Always specify a realistic time limit. Jobs are killed when time expires. Add ~20% buffer to your estimate.
```

### Partition Selection

```bash
#SBATCH --partition=cpu.std   # Use standard partition
#SBATCH --partition=gpu       # Use GPU partition
```

See [Partitions and QOS](../computing_environment/partitions_qos.md) for available partitions.

## Complete Job Script Examples

### Serial Job (Single CPU)

```bash
#!/bin/bash
#SBATCH --job-name=serial_job
#SBATCH --output=serial_%j.out
#SBATCH --ntasks=1
#SBATCH --mem=8G
#SBATCH --time=02:00:00

module load python/3.11

python my_script.py input.txt output.txt
```

### Parallel Job (MPI)

```bash
#!/bin/bash
#SBATCH --job-name=mpi_job
#SBATCH --output=mpi_%j.out
#SBATCH --ntasks=16
#SBATCH --mem-per-cpu=2G
#SBATCH --time=04:00:00

module load gcc/11.2.0
module load openmpi/4.1.4

mpirun ./my_mpi_program
```

<!-- See [Using MPI with Fortran](../software/mpi_fortran.md) for MPI examples. --->

### Multithreaded Job (OpenMP)

```bash
#!/bin/bash
#SBATCH --job-name=openmp_job
#SBATCH --output=openmp_%j.out
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --mem=32G
#SBATCH --time=03:00:00

module load gcc/11.2.0

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

./my_openmp_program
```

### Python Job with Miniforge

```bash
#!/bin/bash
#SBATCH --job-name=python_analysis
#SBATCH --output=analysis_%j.out
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4
#SBATCH --mem=16G
#SBATCH --time=06:00:00

module load miniforge3

conda activate myenv

python analysis.py --input data.csv --output results.txt
```


## Job Submission and Management

### Submitting Jobs

**Submit script**:
```bash
$ sbatch myjob.sh
Submitted batch job 12345
```

**Submit with command-line options** (overrides script):
```bash
$ sbatch --time=01:00:00 --mem=8G myjob.sh
```

**Submit from specific directory**:
```bash
$ cd /path/to/workdir
$ sbatch myjob.sh
```

### Monitoring Jobs

**View your jobs**:
```bash
$ squeue -u $USER
```

**Detailed job information**:
```bash
$ scontrol show job 12345
```

**Job history**:
```bash
$ sacct -j 12345
```

See [Monitoring Resources](../computing_environment/monitoring_resources.md) for comprehensive monitoring guide.

### Canceling Jobs

**Cancel specific job**:
```bash
$ scancel 12345
```

**Cancel all your jobs**:
```bash
$ scancel -u $USER
```

**Cancel jobs by name**:
```bash
$ scancel --name=myjob
```

## Job Dependencies

### Running Jobs in Sequence

**Job 2 starts after Job 1 completes**:
```bash
$ JOB1=$(sbatch --parsable job1.sh)
$ sbatch --dependency=afterok:$JOB1 job2.sh
```

**Dependency types**:

- `after:jobid`: Start after jobid starts
- `afterok:jobid`: Start after jobid completes successfully
- `afternotok:jobid`: Start if jobid fails
- `afterany:jobid`: Start after jobid completes (any exit status)

**Example workflow**:
```bash
$ JOB1=$(sbatch --parsable preprocessing.sh)
$ JOB2=$(sbatch --parsable --dependency=afterok:$JOB1 analysis.sh)
$ JOB3=$(sbatch --parsable --dependency=afterok:$JOB2 postprocessing.sh)
```

## Job Environment Variables

SLURM sets useful environment variables in your job:

```bash
$SLURM_JOB_ID              # Job ID
$SLURM_JOB_NAME            # Job name
$SLURM_SUBMIT_DIR          # Directory where sbatch was run
$SLURM_NTASKS              # Number of tasks
$SLURM_CPUS_PER_TASK       # CPUs per task
$SLURM_NNODES              # Number of nodes
$SLURM_NODELIST            # List of allocated nodes
$SLURM_ARRAY_TASK_ID       # Array task ID (for job arrays)
```

**Using in scripts**:
```bash
echo "Running on $SLURM_NNODES nodes"
echo "Output directory: $SLURM_SUBMIT_DIR/output_$SLURM_JOB_ID"
```

## Output and Error Files

### Default Behavior

By default, SLURM creates:
```
slurm-JOBID.out  # Combined stdout and stderr
```

### Custom Output Files

**Separate output and error**:
```bash
#SBATCH --output=output_%j.txt
#SBATCH --error=error_%j.txt
```

**Include job name and ID**:
```bash
#SBATCH --output=%x_%j.out    # %x = job name, %j = job ID
```

**Output to subdirectory**:
```bash
#SBATCH --output=logs/job_%j.out
```

Make sure the directory exists first:
```bash
$ mkdir -p logs
$ sbatch myjob.sh
```

### Viewing Output While Job Runs

**Follow output in real-time**:
```bash
$ tail -f slurm-12345.out
```

**Last 50 lines**:
```bash
$ tail -50 slurm-12345.out
```

## Best Practices

### Resource Requests

**1. Test first with small jobs**:
```bash
# Test job
#SBATCH --time=00:30:00
#SBATCH --mem=4G
```

**2. Request what you need + buffer**:
```bash
# If test used 12 GB and 3 hours:
#SBATCH --mem=16G          # 33% buffer
#SBATCH --time=04:00:00    # 33% buffer
```

**3. Don't over-request**:

- Wastes resources
- Lowers priority
- Longer queue times

### Job Organization

**1. Use descriptive names**:
```bash
#SBATCH --job-name=protein_fold_1a2b
```

**2. Organize output files**:
```bash
mkdir -p logs results
#SBATCH --output=logs/%x_%j.out
```

**3. Document your scripts**:
```bash
#!/bin/bash
# Purpose: Analyze RNA-seq data from experiment XYZ
# Author: Your Name
# Date: 2024-01-15
```

### Error Handling

**1. Check for errors in script**:
```bash
#!/bin/bash
#SBATCH directives...

# Exit on any error
set -e

# Check if input file exists
if [ ! -f input.dat ]; then
    echo "Error: input.dat not found"
    exit 1
fi

# Run program
./my_program input.dat
```

**2. Validate output**:
```bash
# Check if output was created
if [ ! -f output.dat ]; then
    echo "Error: output.dat not created"
    exit 1
fi
```

### Email Notifications

**Get notified of job events**:
```bash
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH --mail-user=yourname@nmt.edu
```

**Notification types**:

- `BEGIN`: Job starts
- `END`: Job completes successfully
- `FAIL`: Job fails
- `ALL`: All events
- `TIME_LIMIT`: Job reaches time limit

## Troubleshooting

### Job Fails Immediately

**Check output file**:
```bash
$ cat slurm-12345.out
```

**Common causes**:

- Module not loaded
- Input file not found
- Wrong path to executable
- Typo in script

### Job Killed - Out of Memory

**Check with sacct**:
```bash
$ sacct -j 12345 --format=JobID,State,MaxRSS,ReqMem
```

If `MaxRSS` is close to `ReqMem`:

**Solution**: Increase memory request
```bash
#SBATCH --mem=32G  # Increased from 16G
```

### Job Killed - Time Limit

**Check time used**:
```bash
$ sacct -j 12345 --format=JobID,Elapsed,Timelimit,State
```

**Solution**: Increase time limit
```bash
#SBATCH --time=08:00:00  # Increased from 4 hours
```

### Job Pending Forever

**Check reason**:
```bash
$ squeue -u $USER -o "%.18i %.30j %.20R"
```

**Common reasons and solutions**:

- `Resources`: Wait or reduce request
- `Priority`: Your fair-share is low (wait)
- `QOSMaxCpuPerUserLimit`: Cancel or wait for running jobs
- `PartitionNodeLimit`: Requested too many nodes

See [Partitions and QOS](../computing_environment/partitions_qos.md) for more information.

## Advanced Topics

### Job Arrays

For running many similar jobs, see [Using SLURM Job Arrays](job_arrays.md).

### GPU Jobs

For GPU computing, see [Running Jobs on GPU Nodes](gpu_jobs.md).

### Parallel Programming

For MPI and parallel programming:

<!-- - [Using MPI with Fortran](../software/mpi_fortran.md) --->

## Job Script Template

**Save this as `template.sh`**:

```bash
#!/bin/bash
#SBATCH --job-name=CHANGEME
#SBATCH --output=logs/%x_%j.out
#SBATCH --error=logs/%x_%j.err
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=4G
#SBATCH --time=01:00:00
#SBATCH --partition=std.cpu
#SBATCH --mail-type=END,FAIL
#SBATCH --mail-user=you@nmt.edu

# Exit on error
set -e

# Print job information
echo "Job started on $(hostname) at $(date)"
echo "Job ID: $SLURM_JOB_ID"
echo "Working directory: $(pwd)"

# Load modules
module purge
module load python/3.11

# Your commands here
python my_script.py

# Finish
echo "Job completed at $(date)"
```

**Make logs directory**:
```bash
$ mkdir -p logs
```

## Summary

**Key SLURM Commands**:

| Task | Command |
|------|---------|
| Submit job | `sbatch script.sh` |
| View queue | `squeue -u $USER` |
| Job details | `scontrol show job JOBID` |
| Job history | `sacct -j JOBID` |
| Cancel job | `scancel JOBID` |
| Job efficiency | `seff JOBID` |

**Next Steps**:

- [Using SLURM Job Arrays](job_arrays.md) for many similar jobs
- [Running Jobs on GPU Nodes](gpu_jobs.md) for GPU computing
- [Monitoring Resources](../computing_environment/monitoring_resources.md) for detailed monitoring

## Questions?

For questions about batch jobs or SLURM, contact <hpc@nmthpc.atlassian.net>.
