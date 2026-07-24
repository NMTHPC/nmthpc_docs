# ORCA

This guide covers using ORCA for quantum chemistry calculations on the NMTHPC system.

## Loading ORCA

```bash
$ module avail orca
$ module load orca/6.1.1
```

**Verify installation**:
```bash
$ orca --version
$ which orca
```

This command displays the location of the ORCA executable. If ORCA is loaded successfully, it should return the path to the ORCA executable.

## Running ORCA

### Testing ORCA

A sample ORCA job is available for download to verify that ORCA is functioning correctly on NMTHPC.

| File | Description |
|------|-------------|
| [orca_test.tar.gz](https://github.com/user-attachments/files/30201358/orca_test.tar.gz) | Sample ORCA test job for validating the installation. |

Open a new Terminal window on your local computer (not the SSH session) and copy the test archive to your NMTHPC system home directory.

```bash
$ scp /path/to/orca_test.tar.gz your_username@nmthpc_hostname:~
```

> ***NOTE***
> 
> Replace:
> 
> - `/path/to/orca_test.tar.gz` with the location of the file on your laptop.
> 
> - `your_username` with your NMTHPC username.
> 
> - `nmthpc_hostname` with the hostname you normally SSH into.

The test archive should now be available on your NMTHPC system.

Connect to the NMTHPC cluster via SSH and extract the archive into your NMTHPC system.

```bash
$ tar -xzvf orca_test.tar.gz
```

This command extracts the contents of the compressed archive and creates a new directory named `orca_test` containing the test files.

Then, change your current working directory to the `orca_test` folder, where the sample input files and job script are located:

```bash
cd orca_test
```

List the files in the directory:

```bash
ls
```

You should see files similar to:

```text
h2o.inp
orca_test.sh
H2O_vac.gbw
```

Submit the test job to the Slurm scheduler:

```bash
sbatch orca_test.sh
```

After submitting the job, you should receive output similar to:

```text
Submitted batch job <job_id>
```

This should allow you to run a job on the cluster for the first time on the NMTHPC system. 

## Check the Output

After the job completes, verify that the output file was created:

```bash
ls
```

The output file `h2o.out` should be present.

Display the output file:

```bash
cat h2o.out
```

A successful ORCA calculation should display the ORCA banner followed by the calculation output and complete without runtime errors similar to:

```text
                                                  *****************
                                                  * O   R   C   A *
                                                  *****************

                              #########################################################
                              #                        -***-                          #
                              #          Department of theory and spectroscopy        #
                              #                                                       #
                              #                      Frank Neese                      #
                              #                                                       #
                              #     Directorship, Architecture, Infrastructure        #
                              #                    SHARK, DRIVERS                     #
                              #        Core code/Algorithms in most modules           #
                              #                                                       #
                              #        Max Planck Institute fuer Kohlenforschung      #
                              #                Kaiser Wilhelm Platz 1                 #
                              #                 D-45470 Muelheim/Ruhr                 #
                              #                      Germany                          #
                              #                                                       #
                              #                  All rights reserved                  #
                              #                        -***-                          #
                              #########################################################

                                                [Calculation Output]
```

This test verifies that the ORCA module is available and that you can successfully submit and run jobs on the NMTHPC cluster.

## Additional Resources

- [Official ORCA Documentation](https://www.faccts.de/orca/)
- [NMTHPC Software Overview](software_overview.md)

## Contact

For questions about ORCA or assistance with running jobs on NMTHPC, contact <hpc@nmthpc.atlassian.net>.
