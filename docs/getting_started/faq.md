# Frequently Asked Questions

This page answers common questions about using the NMT HPC cluster.

## Account and Access

### How do I get an account on NMTHPC?

See our [Accounts and System Login](accounts_login.md) page for detailed instructions on requesting an account.

### I forgot my password. How do I reset it?

Your password is your NMT academic lab password. Please refer [this page](https://www.nmt.edu/itc/newlablogins.php) for more information on NMT academic lab logins.

### Can I access NMTHPC from off campus?

Yes, but you'll need to connect through the NMT VPN first. Contact NMT IT Services for VPN access and configuration instructions.

### How long do NMTHPC accounts remain active?

Student accounts are reviewed annually. Faculty and staff accounts remain active as long as you're affiliated with NMT. Inactive accounts may be deactivated after extended periods of non-use.

## Storage and Data

### Where should I store my files?

NMTHPC has two main filesystems. See [Nodes and Filesystems](../computing_environment/nodes_filesystems.md) for more details.

### What are the storage quotas?

The home directory (`/home/username`) attached the login node has a storage quota of 1 Tb, where `username` is your NMT 900 number. The parallel BeeGFS server (`/data/username/`) will soon have a storage quota of 2 Tb.  

You can check your current storage usage on your home directory with:
- `df -h /home/username/` or
- `df -h $HOME`


### How do I back up my data?

The NMTHPC `home` directory is regularly backed up. The `data` directory is not backed up and is only designed for short term data storage while running models. You're responsible for backing up important data to other locations. 

## Running Jobs

### What's the difference between login nodes and compute nodes?

**Login nodes** are for:

- Editing files
- Compiling simple codes (using no more than 1 processor)
- Submitting jobs
- Managing files

**Compute nodes** are for:

- Running simulations
- Data analysis
- Compiling complex codes
- Any computationally intensive work

Never run heavy computations on login nodes. Use SLURM to submit jobs to compute nodes.

### How do I run a job?

There are two main ways:

1. **Interactive jobs**: For compiling, testing and development
   `$ srun --ntasks=1 --mem=2G --time=00:10:00 --mpi=pmi2 --pty --partition=compile --qos=compile bash`
- For more information see [Running Interactive Jobs](../using_nmthpc/interactive_jobs.md)

2. **Batch jobs**: For production runs
   `$ sbatch myjob.sh`
- For more information see [Running Batch Jobs](../using_nmthpc/batch_jobs.md)

### How do I check the status of my job?

`$ squeue -u $USER`


For more detailed information:

`$ scontrol show job JOBID`

See [Monitoring Resources](../computing_environment/monitoring_resources.md) for more commands.

### Why is my job pending?

Common reasons include:

- **Resources**: The requested resources (CPUs, GPUs, memory) aren't currently available
- **Priority**: Other jobs have higher priority
- **Limits**: You've reached your maximum number of running jobs
- **QOS**: Quality of Service restrictions

Check with:
- `$ squeue -u $USER`


The \`REASON\` column shows why a job is pending.

### How do I cancel a job?

`$ scancel JOBID`

To cancel all your jobs:

`$ scancel -u $USER`


## Software

### What software is available on NMTHPC?

Use the \`module\` system to see available software:

`$ module avail`

See [Software Available on NMTHPC](../computing_environment/software.md) for a comprehensive list.

### How do I load software?

`$ module load softwarename`

Example:

`$ module load python/3.12.5-gcc-11.5.0-xondaab`

### Can I install my own software?

Yes! You can:

- Install Python packages in your home directory with \`pip install --user\`
- Create Anaconda environments
- Compile software in your home directory

See specific software guides in the [Software and Examples](../software/anaconda.md) section.

### I need software that's not installed. What should I do?

Contact HPC support by creating a ticket through the [Jira support portal](https://nmthpc.atlassian.net/servicedesk/customer/portal/2) with:

- Software name and version
- Why you need it
- Link to the software website

## Best Practices

### How can I be a good NMTHPC user?

- Don't run jobs on login nodes
- Request only the resources you need
- Test your software and jobs on a laptop or desktop before using the THPC system
- Test on the NMTHPC system with short jobs before submitting long runs
- Clean up old data you no longer need
- Monitor your jobs and cancel failed ones
- Acknowledge NMTHPC in publications

### How should I structure my workflow?

1. **Develop and test** on your local machine when possible
2. **Transfer** only necessary data to NMTHPC
3. **Test** with small jobs or interactive sessions
4. **Scale up** to full production runs
5. **Transfer** results back to your local machine
6. **Clean up** intermediate files

### What are some common mistakes to avoid?

- Running computations on login nodes
- Not specifying enough memory or time
- Not checking job status after submission
- Ignoring error messages
- Requesting more resources than needed

## Getting Help

### How do I contact HPC support?

Contact HPC support by creating a ticket through the [Jira support portal](https://nmthpc.atlassian.net/servicedesk/customer/portal/2)

Include in your request:

- Detailed description of the issue
- Job IDs (if applicable)
- Error messages
- What you've already tried

### What information should I include in a support request?

- **For job issues**: Job ID, error messages, SLURM script
- **For software issues**: Software name, version, exact commands you ran
- **For login issues**: Error messages, your operating system
- **For data transfer issues**: Transfer method, file sizes, error messages

