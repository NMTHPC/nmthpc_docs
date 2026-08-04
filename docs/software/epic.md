# EPIC

This guide covers using EPIC3 (Explicit Planetary Isentropic Coordinate) on the NMTHPC cluster. EPIC3 is as an atmospheric modeling application used to simulate planetary atmospheres and generate scientific output for analysis.

## Loading EPIC3

```bash
$ module avail epic3
$ module load epic3
```

**Verify installation**:
```bash
$ epic --version
$ which epic
```

This command displays the location of the ORCA executable. If ORCA is loaded successfully, it should return the path to the ORCA executable.

## Running EPIC3

### Testing EPIC3

A sample EPIC3 test is provided to verify that EPIC3 is installed correctly and can successfully run on the NMTHPC cluster.

| File | Description |
|------|-------------|
| `[epic_test.tar.gz](https://github.com/user-attachments/files/30717189/epic_test.tar.gz)` | Sample EPIC3 test case used to verify the installation. |

### Copy the Test Files

Open a new terminal window on your local computer (not your SSH session).

Transfer the test archive to your NMTHPC home directory using the `scp` (Secure Copy) command.

```bash
scp /path/to/epic_test.tar.gz your_username@nmthpc_hostname:~
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
tar -xzvf epic_test.tar.gz
```

This command extracts the compressed archive and creates a directory containing the EPIC3 test files.

Move into the test directory:

```bash
cd epic_test
```

List the files:

```bash
ls
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
module load openmpi/5.0.5
```

**Verify installation**:
```bash
$ mpirun --version
$ which mpirun
```

Run the EPIC3 simulation:

```bash
mpirun -np 4 /home/900#/epic3/bin/mpi_epic.LINUX -itout 20 -itsave 5 -itback 10 epic000.nc
```

### Verify the Output

After the simulation completes, list the files in the directory:

```bash
ls
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
cat eigenvalues.txt
```

## Additional Resources

- [Official EPIC Documentation](https://arxiv.org/abs/2509.16212)
- [NMTHPC Software Overview](software_overview.md)

## Contact

For questions about EPIC3 or assistance with running jobs on NMTHPC, contact <hpc@nmthpc.atlassian.net>.
