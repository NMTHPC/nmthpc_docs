# Accounts and System Login

This guide covers how to obtain an account on the NMT HPC cluster and how to connect to the system.

## Requesting an Account

### Eligibility

NMTHPC accounts are available to:
- New Mexico Tech faculty, staff, and students

### Account Request Process

1. Visit the NMT HPC account request portal (contact IT Services for the link)
2. Fill out the account request form with:
   - Your NMT credentials (if applicable)
   - Project description and computational needs
   - Faculty sponsor (for students and external collaborators)
3. Wait for approval notification via email
4. Once approved, you'll receive your login credentials and connection instructions

```{note}
We will be onboarding users to the HPC systems individually during the Spring 2026 semester. If you have been given access to the system, please contact NMTHPC support (<hpc@nmthpc.atlassian.net>) for any questions regarding system access and usage. We will begin a more general onboarding process over the course of the summer 2026 semester.
```

## Connecting to NMTHPC

NMTHPC is accessed via SSH (Secure Shell). The login process differs slightly between operating systems.

### Login Information

- **Hostname**: `nmthpc.id.nmt.edu` 
- **Username**: Your NMT 900 number.
- **Password**: Your institutional password.

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

## SSH Keys (Recommended)

SSH keys provide a more secure and convenient authentication method than passwords.

### Generating SSH Keys

**On Linux/macOS/WSL**:
```bash
$ ssh-keygen -t ed25519 -C "your_email@nmt.edu"
```

Press Enter to accept the default file location, then optionally enter a passphrase.

**On Windows (PowerShell)**:
```bash
$ ssh-keygen -t ed25519 -C "your_email@nmt.edu"
```

### Copying Your Public Key to NMTHPC

**Method 1: Using ssh-copy-id (Linux/macOS/WSL)**:
```bash
$ ssh-copy-id username@nmthpc.id.nmt.edu
```

**Method 2: Manual copy**:
1. Display your public key:
   ```bash
   $ cat ~/.ssh/id_ed25519.pub
   ```
2. Log in to NMTHPC with your password
3. Add the key to `~/.ssh/authorized_keys`:
   ```bash
   $ mkdir -p ~/.ssh
   $ chmod 700 ~/.ssh
   $ echo "your-public-key-content" >> ~/.ssh/authorized_keys
   $ chmod 600 ~/.ssh/authorized_keys
   ```

After setting up SSH keys, you can log in without entering your password each time.

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
**Important**: Login nodes are for light tasks only (editing files, submitting jobs, compiling code). Do NOT run computationally intensive jobs on login nodes. Use SLURM to submit jobs to compute nodes.
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
- If using SSH keys, ensure permissions are correct (`chmod 600 ~/.ssh/id_ed25519`)

### Too Many Authentication Failures

If you have many SSH keys, specify which one to use:
```bash
$ ssh -i ~/.ssh/id_ed25519 username@nmthpc.id.nmt.edu
```

## Getting Help

If you encounter issues logging in:
- Contact HPC support at **hpc@nmthpc.atlassian.net**
- Include error messages and what you've tried
- Specify your operating system and connection method
