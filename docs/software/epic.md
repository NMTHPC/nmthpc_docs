# EPIC

This guide covers using EPIC3 (Explicit Planetary Isentropic Coordinate) on the NMTHPC cluster. EPIC3 is as an atmospheric modeling application used to simulate planetary atmospheres and generate scientific output for analysis.

## Installing EPIC3

The EPIC3 installation archive is available in the shared software directory on NMTHPC.

Connect to the NMTHPC cluster via SSH and copy the archive to your home directory :

```bash
$ cp /home/shared_folder/epic/epic3_alpine_jul18_2024.tar .
```

```{note}
The `cp` command copies a file from one location to another. In this example, the period (`.`) at the end of the command represents your current working directory, so the archive will be copied there.
```

Verify that the archive was copied successfully:

```bash
$ ls
```

You should see:

```text
epic3_alpine_jul18_2024.tar
```

Extract the archive:

```bash
$ tar -xvf epic3_alpine_jul18_2024.tar
```

```{note}
The `tar` command extracts the contents of the archive into a new directory while preserving the original files.
```

Verify that the EPIC3 directory was created:

```bash
$ ls
```

You should see a directory similar to:

```text
epic3/
```

Change the directory into EPIC3:

```bash
$ cd epic3
```

Verify your current location:

```bash
$ pwd
```

The output should resemble:

```text
/home/your_username/epic3
```

## Starting an Interactive Session

Before running EPIC3, start an interactive Slurm session on a compute node. This provides the compute resources required to run MPI applications.

Request an interactive session:

```bash
$ srun -p comptest --qos=testing -N1 -n4 --cpus-per-task=1 --time=00:30:00 --pty bash
```

This command requests:

| Option | Description |
|--------|-------------|
| `-p comptest` | Uses the `comptest` partition for software testing. |
| `--qos=testing` | Uses the testing Quality of Service (QoS). |
| `-N1` | Requests one compute node. |
| `-n4` | Starts four MPI tasks (processes). |
| `--cpus-per-task=1` | Allocates one CPU core for each MPI task. |
| `--time=00:30:00` | Sets a maximum runtime of 30 minutes. |
| `--pty bash` | Starts an interactive Bash shell on the allocated compute node. |

After the command completes, your terminal prompt will change, indicating that you are now running on a compute node instead of the login node.

You can verify this by running:

```bash
$ hostname
```

The command should display the name of the compute node assigned to your job.

```{note}
When you are finished testing EPIC3, type `exit` to end the interactive session and release the allocated resources.
```

## Running EPIC3

### Testing EPIC3

A sample EPIC3 test is provided to verify that EPIC3 is installed correctly and can successfully run on the NMTHPC cluster.

| File | Description |
|------|-------------|
| [epic_test.tar.gz](https://github.com/user-attachments/files/30717189/epic_test.tar.gz) | Sample EPIC3 test case used to verify the installation. |

### Copy the Test Files

Open a new terminal window on your local computer (not your SSH session).

Transfer the test archive to your NMTHPC home directory using the `scp` (Secure Copy) command.

```bash
$ scp /path/to/epic_test.tar.gz your_username@nmthpc_hostname:~
```

```{note}
Replace:
- `/path/to/epic_test.tar.gz` with the location of the file on your local computer.
- `your_username` with your NMTHPC username.
- `nmthpc_hostname` with the hostname you normally use to connect to NMTHPC.
```

The test archive should now be available in your NMTHPC home directory.

### Extract the Test Files

Connect to the NMTHPC cluster using SSH.

Extract the archive:

```bash
$ tar -xzvf epic_test.tar.gz
```

This command extracts the compressed archive and creates a directory containing the EPIC3 test files.

Move into the test directory:

```bash
$ cd epic_test
```

List the files:

```bash
$ ls
```

You should see files similar to:

```text
epic000.nc
epic.nc
eigenvalues.txt
grs.txt
vertical.txt
zonal_wind.txt
```

### Run the Test

Load the required MPI module:

```bash
$ module load openmpi/5.0.5
```

**Verify installation**:
```bash
$ mpirun --version
$ which mpirun
```

Run the EPIC3 simulation:

```bash
$ mpirun -np 4 /home/900#/epic3/bin/mpi_epic.LINUX -itout 20 -itsave 5 -itback 10 epic000.nc
```

### Verify the Output

After the simulation completes, list the files in the directory:

```bash
$ ls
```

The simulation should produce output files similar to:

```text
epic.nc
epic000.nc
eigenvalues.txt
vertical.txt
zonal_wind.txt
grs.txt
```

The generated files contain the simulation results and can be used for further analysis.

You can display a text output file using:

```bash
$ cat eigenvalues.txt
```

## Additional Resources

- [Official EPIC Documentation](https://arxiv.org/abs/2509.16212)
- [NMTHPC Software Overview](software_overview.md)

## Contact

For questions about EPIC3 or assistance with running jobs on NMTHPC, contact <hpc@nmthpc.atlassian.net>.
