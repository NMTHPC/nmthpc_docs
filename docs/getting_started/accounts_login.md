# Accounts and System Login

This guide covers how to obtain an account on the NMT HPC cluster and how to connect to the system.

## Requesting an Account

### Eligibility

NMTHPC accounts are available to:
- New Mexico Tech faculty, staff, and students

### Account Request Process

1. Visit the [NMT HPC account Jira request portal](https://nmthpc.atlassian.net/servicedesk/customer/portal/2)
- If this is the first time you are Jira, it will ask to create an account
- You should use your NMT email to create this account 
2. Fill out the account request form with
3. Wait for an approval notification via email
4. The email notification will contain instructions for the next steps

```{note}
We will be onboarding users to the HPC systems individually during the Spring and Summer 2026 semesters. If you have been given access to the system, please create a support ticket through the [Jira support portal](https://nmthpc.atlassian.net/servicedesk/customer/portal/2) for any questions regarding system access and usage. We will begin a more general onboarding process over the course of the Fall 2026 semester.
```

## Connecting to NMTHPC

NMTHPC can only be accessed via SSH (Secure Shell), which is available through a terminal on most operating systems.

### Login Information

- **Hostname**: `nmthpc.id.nmt.edu` 
- **Username**: Your NMT 900 number.
- **Password**: Your academic lab password.

### From Linux or macOS

Open a terminal and use the `ssh` command:

```bash
$ ssh username@nmthpc.id.nmt.edu
```

Replace `username` with your actual 900 number. You will be prompted for your password.



### From Windows

Windows users have several options:

#### Option 1: Windows PowerShell or Command Prompt (Windows 10/11)

Modern Windows versions include OpenSSH:

```bash
$ ssh username@nmthpc.id.nmt.edu
```

#### Option 2: PuTTY

1. Download and install [PuTTY](https://www.putty.org/)
2. Launch PuTTY
3. Enter the hostname: `nmthpc.id.nmt.edu`
4. Click "Open"
5. Enter your username and password when prompted

#### Option 3: Windows Subsystem for Linux (WSL)

If you have WSL installed, use the same commands as Linux:

```bash
$ ssh username@nmthpc.id.nmt.edu
```

## First Login

Upon your first login, you'll be in the home directory and you will see a welcome message with recent announcements or system information. 

### Initial Setup

1. **Check your environment**:
   ```bash
   $ pwd  # Print working directory
   $ ls -la  # List files
   ```

2. **Verify available modules**:
   ```bash
   $ module avail
   ```

3. **Check filesystem access**:
   ```bash
   $ df -h ~  # Check home directory quota
   ```

## Login Nodes vs. Compute Nodes

```{warning}
**Important**: Login nodes are for light tasks only (editing files, submitting jobs, compiling simple code). Do NOT run computationally tasks on login nodes that takes more than one processor, large amounts of RAM, or a few second to run. Use SLURM to submit jobs to compute nodes and interactive jobs via srun to compile complex software packages.
```


See [Running Interactive Jobs](../using_nmthpc/interactive_jobs.md) and [Running Batch Jobs](../using_nmthpc/batch_jobs.md) for how to use compute resources properly.

## Troubleshooting Connection Issues

### Connection Refused or Timeout

- Verify you're on the NMT network or connected via VPN
- Check if you're using the correct hostname
- Ensure your account has been activated

### Permission Denied

- Double-check your username and password
- Verify your account hasn't expired

## Getting Help

If you encounter issues logging in:
- Contact HPC support by creating a ticket through the [Jira support portal](https://nmthpc.atlassian.net/servicedesk/customer/portal/2)
- Include error messages and what you've tried
- Specify your operating system and connection method
