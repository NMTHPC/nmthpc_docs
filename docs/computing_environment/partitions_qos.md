# Partitions and Quality of Service (QOS)

This page explains the partition and Quality of Service (QOS) systems used on NMTHPC to manage access to computing resources.



## Partitions

Partitions are groups of nodes with similar characteristics. Think of them as different queues for different types of work.

### Viewing Available Partitions


```bash
$ sinfo
```

**Example output**:
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
comptest   up.  01:00:00      1.  idle node1 
cpu.std    up 2-00:00:00     16   idle node[2-16]
gpu        up 1-00:00:00      2   idle gpu[1-2]
cpu.hm     up 2-00:00:00      3   idle himem[1-3]
```

**Key columns**:

- `PARTITION`: Partition name (* indicates default)
- `AVAIL`: Availability status
- `TIMELIMIT`: Maximum job runtime
- `NODES`: Number of nodes
- `STATE`: Node state (idle, allocated, down, etc.)
- `NODELIST`: Which nodes are in this partition

### Common Partitions

```{note}
Use `sinfo` to see actual up-to-date partitions on NMTHPC.
```

#### Standard (cpu.std) Partition

**Purpose**: General-purpose CPU computing

**Characteristics**:

- Default partition
- CPU compute nodes
- Standard memory allocation
- Time limit: 2 days

**When to use**:

- Standard computational jobs
- MPI parallel applications
- CPU-intensive workloads

**Example job submission**:
```bash
#SBATCH --partition=cpu.std
#SBATCH --ntasks=16
#SBATCH --time=24:00:00
```

#### GPU Partition

**Purpose**: GPU computing, AI/ML model training

**Characteristics**:

- Nodes with NVIDIA H100 or NVIDIA H200 GPUs



**Example job submission**:
```bash
#SBATCH --partition=gpu
#SBATCH --gres=gpu:1
#SBATCH --time=06:00:00
```

See [Running Jobs on GPU Nodes](../using_nmthpc/gpu_jobs.md) for detailed guidance.

#### High Memory Partition

**Purpose**: Memory-intensive applications

**Characteristics**:

- Nodes with large RAM, for applications that need more memory than standard nodes

**Example job submission**:
```bash
#SBATCH --partition=cpu.hm 
#SBATCH --mem=12G
#SBATCH --time=24:00:00
```

### Specifying Partitions

**In job script**:
```bash
#SBATCH --partition=gpu
```

**On command line**:
```bash
$ sbatch --partition=gpu myjob.sh
```

**Interactive job**:
```bash
$ srun --partition=cpu.hm --pty bash
```

**Omit for default partition**:
```bash
# Uses default partition if not specified
$ sbatch myjob.sh
```

## Quality of Service (QOS)

QOS policies control job priority, resource limits, and scheduling behavior.

### Viewing QOS Information

**List available QOS**:
```bash
$ sacctmgr show qos
```

**Your account's QOS**:
```bash
$ sacctmgr show user $USER withassoc format=user,account,qos
```


### Resource Priority & QoS Matrix

The following table details the baseline priority values, preemption behaviors, and resource boundary allocations across all active QOS queues on NMTHPC.

| QOS NAME | PRIORITY | PARTITION(S) / HARDWARE | PREEMPTION | MAX CPU | MAX NODE | MAX GPU | MAX WALL |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **scaling_test** <br> `SCALING PHASE` | 150000 | `cpu.std` (node[2-16]) | - | - | - | - | 7-00:00:00 |
| **compile** | 110000 | `comptest` (node1) | - | 32 | 1 | - | 01:00:00 |
| **testing** | 100000 | `comptest` (node1) | - | 100 | 1 | - | 01:00:00 |
| **normal** | 90000 | `cpu.std` (node[2-16]) | scaling_test | 1024 | 4 | - | 2-00:00:00 |
| **hmem** | 90000 | `cpu.hm` (hm[1-3]) | - | 1536 | 2 | - | 7-00:00:00 |
| **h100** | 90000 | `gpu` (gpu[1-2]) | - | 256 | 2 | 2 | 12:00:00 |
| **long** | 70000 | `cpu.std` (node[2-16]) <br> `cpu.hm` (hm[1-3]) | QOS-long | 512 | 2 | - | 7-00:00:00 |
| **h100-long** | 70000 | `gpu` (gpu[1-2]) | - | 128 | 1 | 1 | 2-00:00:00 |
| **hmem-long** | 100 | `cpu.hm` (hm[1-3]) | - | 512 | 2 | - | 7-00:00:00 |

### Available QOS Levels Detailed

#### Scaling Test QOS (`scaling_test`)
- **Characteristics**: Highest priority queue tier dedicated to validation runs during scaling development tracking. 
- **Limits**: Maximum wall time up to 7 days (`7-00:00:00`).

#### Compile QOS (`compile`)
- **Characteristics**: Fast-tracked processing tier running strictly on isolated system hardware `node1`.
- **Limits**: Constrained to a maximum profile of 32 CPUs, 1 node footprint, and up to 1 hour (`01:00:00`) wall time duration window. Devoted for parsing large, complex builds.

#### Testing QOS (`testing`)
- **Characteristics**: High scheduling priority environment operating on local hardware infrastructure `node1`.
- **Limits**: Limited to 100 CPUs, 1 compute node, and a brief 1 hour (`01:00:00`) structural boundary runtime. Designed for swift prototyping evaluations.

#### Normal QOS (`normal`)
- **Characteristics**: Standard core execution assignment framework across the cluster's main standard architectural nodes (`cpu.std`).
- **Limits**: Cap bounds of 1024 CPUs, 4 total system node structures, and 2 days (`2-00:00:00`) limit runtime configurations. Susceptible to immediate priority preemption triggers initiated by operational `scaling_test` instances.

#### High Memory QOS (`hmem`)
- **Characteristics**: Default operational priority settings pinned directly over high-RAM operational configurations (`hm[1-3]`).
- **Limits**: Scaled limit allocations of up to 1536 CPUs, 2 system nodes, and up to 7 days (`7-00:00:00`) timeline boundaries.
 
### Specifying QOS

**In job script**:
```bash
#SBATCH --qos=normal
```

**On command line**:
```bash
$ sbatch --qos=long myjob.sh
```

## Resource Limits

### Partition Limits

Each partition has limits on:

**Time limits**: Maximum wall time for jobs
```bash
$ sinfo -o "%P %.11l"  # Show partition time limits
```

**Node limits**: Maximum nodes per job

**GPU limits**: Maximum GPUs per user or job

### QOS Limits

QOS policies limit:

- **Max jobs per user**: How many jobs you can have queued/running
- **Max CPUs per user**: Total CPUs across all your jobs
- **Max GPUs per user**: Total GPUs across all your jobs
- **Max wall time**: Longest allowed job duration
- **Max submit jobs**: How many jobs you can submit

### Checking Your Limits

**View your current usage**:
```bash
$ squeue -u $USER
```

**Count your running jobs**:
```bash
$ squeue -u $USER -t RUNNING | wc -l
```

**Total CPUs in use**:
```bash
$ squeue -u $USER -t RUNNING -o "%C" | tail -n +2 | awk '{sum+=$1} END {print sum}'
```

## Job Priority

Job priority determines the order in which pending jobs start when resources become available.

### Priority Factors

**Factors affecting priority**:

1. **QOS**: Higher QOS = higher priority
2. **Fair share**: Users with less recent usage get higher priority
3. **Job age**: Older pending jobs get priority boost
4. **Job size**: Smaller jobs may get priority to fill gaps
5. **Partition**: Some partitions have priority policies

### Viewing Job Priority

```bash
$ sprio
```

or for your jobs only:
```bash
$ sprio -u $USER
```

**Output columns**:

- `JOBID`: Job identifier
- `PRIORITY`: Overall priority score
- `AGE`: Priority from wait time
- `FAIRSHARE`: Priority from fair-share algorithm
- `QOS`: Priority from QOS

Higher numbers = higher priority

## Fair Share

NMTHPC uses fair-share scheduling to ensure equitable resource distribution.

### How Fair Share Works

- Tracks your recent resource usage
- Users with less recent usage get higher priority
- Encourages balanced resource sharing
- Usage "decays" over time (typically ~2 weeks)

### Checking Fair Share

```bash
$ sshare -u $USER
```

**Key fields**:

- `FairShare`: Your current fair-share factor (0.0-1.0)
- `Usage`: Your recent usage
- Higher FairShare = higher job priority

```{tip}
If your jobs are pending and you have high recent usage, your fair-share priority may be low. Other users with less recent usage will get priority. Wait times typically improve as your usage "decays."
```

## Best Practices

### Choosing the Right Partition

1. **Match hardware to needs**:
   - GPUs needed → GPU partition
   - High memory needed → High memory partition
   - Standard CPU work → Standard partition

2. **Consider time limits**:
   - Short jobs → Debug/test partition
   - Standard jobs → Standard partition
   - Very long jobs → Long QOS or special request

3. **Test first**:
   - Use debug partition for initial testing
   - Scale up to production partitions

### Optimizing Job Priority

1. **Request only what you need**:
   - Don't request excessive time or resources
   - Smaller resource requests = faster starts

2. **Be strategic with submissions**:
   - Submit jobs when you're ready to use results
   - Don't queue hundreds of jobs unless necessary

3. **Use appropriate QOS**:
   - Normal QOS for routine work
   - Special QOS only when truly needed

### Resource Request Strategy

```{warning}
Requesting more resources than you need:
- Wastes cluster resources
- Reduces your fair-share priority
- Makes jobs take longer to start
- Decreases efficiency metrics
```

**Do request**:

- Actual time needed + 20% buffer
- Memory based on test runs
- Cores your code can actually use

**Don't request**:

- Maximum time "just in case"
- All available memory "to be safe"
- All cores on a node if you'll use only a few

## Troubleshooting

### Job Won't Start

**Check partition availability**:
```bash
$ sinfo -p partitionname
```

**Check QOS limits**:
```bash
$ sacctmgr show qos format=Name,MaxWall,MaxTRES
```

**View pending reason**:
```bash
$ squeue -u $USER -o "%.18i %.30j %.20R"
```

### Hit Resource Limits

**Common limit messages**:

- `QOSMaxCpuPerUserLimit`: You're using max CPUs allowed
- `QOSMaxJobsPerUserLimit`: You have max jobs queued
- `QOSMaxGRESPerUser`: You're using max GPUs allowed

**Solutions**:

- Wait for running jobs to complete
- Cancel unnecessary jobs
- Request different QOS if appropriate
- Contact HPC support for special needs

### Job Priority Too Low

**Check fair-share**:
```bash
$ sshare -u $USER
```

**Check priority**:
```bash
$ sprio -u $USER
```

**Improve priority**:

- Wait for usage to decay
- Request smaller resource allocations
- Use appropriate QOS
- Submit fewer concurrent jobs

## Getting More Information

**Partition details**:
```bash
$ scontrol show partition partitionname
```

**QOS details**:
```bash
$ sacctmgr show qos qosname format=Name,Priority,MaxWall,MaxTRES
```

**Your account details**:
```bash
$ sacctmgr show user $USER withassoc format=user,account,partition,qos,defaultqos
```

## Questions?

For questions about partitions, QOS policies, or resource limits, contact <hpc@nmthpc.atlassian.net>.

For special resource requests or custom QOS, include:

- Why you need special resources
- How long you'll need them
- Estimated resource requirements
- Project timeline
