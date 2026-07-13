# Software Available on NMTHPC

NMTHPC provides a wide range of scientific and research software. This page explains how to find and use available software.

## Environment Modules System

NMTHPC uses the **Environment Modules** system to manage software. Modules allow you to easily load and switch between different software packages and versions.

### Basic Module Commands

**List available modules**:
```bash
$ module avail
```
or 
```bash
$ module spider
```
**Search for specific software**:
```bash
$ module avail python
```
or
```bash
$ module spider python
```

**Load a module**:
```bash
$ module load python/3.12.5-xondaab
```

**List currently loaded modules**:
```bash
$ module list
```

**Unload a module**:
```bash
$ module unload python/3.12.5-xondaab
```

**Unload all modules**:
```bash
$ module purge
```

**Display module information**:
```bash
$ module show python/3.12.5-xondaab
```
or
```bash
$ module spider python/3.12.5-xondaab
```

**Get help with modules**:
```bash
$ module help
```

### Module Loading Best Practices

1. **Load modules in your job scripts**: Always load required modules in your SLURM scripts
2. **Specify versions**: Use specific versions for reproducibility (e.g., `python/3.12.5-xondaab` not just `python`)
3. **Check dependencies**: Some modules automatically load dependencies
4. **Clean environment**: Use `module purge` before loading modules to avoid conflicts

## Software Categories

### Compilers

**GCC (GNU Compiler Collection)**:
```bash
$ module load gcc/14.2.0-y5jrcb6
```

**Intel Compilers**:
```bash
$ module load intel-oneapi-compilers/2024.0.2-j5ujkki
```

### Programming Languages

**Python**:
```bash
$ module load python/3.12.5-xondaab
```

See Python website for detailed information.

**R**:
```bash
$ module load r/4.4.1-gcc-11.5.0-araotop
```

See [R](../software/r.md) for detailed information.

### Parallel Computing Libraries

**OpenMPI**:
```bash
$ module load openmpi/4.1.6-52xkn4g
```

Note - this version of openmpi currently uses the version of gcc (11.5.0) that was provided with the system (Rocky 9).

**Intel MPI**:
```bash
$ module load intel-mpi/2021.6
```

<!-- See [Using MPI with Fortran](../software/mpi_fortran.md) for MPI programming examples. --->

### GPU Computing


### Mathematical and Scientific Libraries

**BLAS/LAPACK** (Linear algebra):
```bash
$ module load openblas/0.3.28-x5wjuis
```

**HDF5** (Hierarchical data format):
```bash
$ module load hdf5/1.14.5-onz4pba
```

### Scientific Applications

**VASP** (Vienna Ab initio Simulation Package):
```bash
$ vasp/6.4.3-gcc-11.5.0-zraz3n2
```

See [VASP](../software/vasp.md) for computational chemistry examples.


## Python Package Management


### Using Miniforge/Conda

Miniforge provides a lightweight conda-based environment manager for creating and managing Python environments:
```bash
$ module load miniforge3
$ conda create -n myenv python=3.12.5
$ conda activate myenv
$ conda install numpy scipy matplotlib
```



## R Package Installation

Install R packages in your home directory:
```bash
$ module load r/4.4.1
$ R
> install.packages("ggplot2", repos="https://cloud.r-project.org")
```

See [R](../software/r.md) for more details. Note: we recommend installing R using conda environments.

## Compiling Your Own Software

You can compile software in your home directory:

```bash
$ module load gcc/14.2.0-y5jrcb6
$ ./configure --prefix=$HOME/software/myapp
$ make
$ make install
```

**Tips**:

- Install to `$HOME/software` or similar directory
- Load required modules before compiling
- Add installation directory to PATH in your `~/.bashrc`

## Requesting New Software

If you need software that's not currently installed:

1. Check if containers are an option
2. Try installing in your home directory
3. Contact HPC support at <hpc@nmthpc.atlassian.net> with:
   - Software name and version
   - Website or download link
   - Brief description of your use case
   - Whether it requires a license

We'll evaluate requests for system-wide installation.

## Module Files in Your Job Scripts

Always load required modules in your SLURM scripts:

```bash
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --ntasks=4
#SBATCH --time=01:00:00

# Load required modules
module purge
module load gcc/11.2.0
module load openmpi/4.1.4
module load python/3.11

# Run your application
python my_script.py
```

## Software Versions

```{note}
Software versions listed on this page are examples. Use `module avail` to see currently installed versions on NMTHPC.
```

## License-Restricted Software

Some software requires licenses:

- **MATLAB**: Check license availability
- **VASP**: Requires proof of license
- **Intel Compilers**: Site license available
- **Other commercial software**: Contact HPC support

If you have a license for commercial software, contact HPC support to inquire about installation.

## Frequently Asked Questions

### Why isn't my module loading?

- Check spelling: `module avail packagename`
- Module conflicts: Try `module purge` first
- Dependencies: Some modules require others to be loaded first

### How do I see which version of software is loaded?

```bash
$ module list
$ which python
$ python --version
```

### Can I use multiple versions of the same software?

Not simultaneously, but you can switch:
```bash
$ module unload python/3.10
$ module load python/3.11
```

### How do I make modules load automatically?

Add to your `~/.bashrc`:
```bash
module load python/3.11
```

```{warning}
Be cautious about loading modules automatically in `.bashrc`. This can cause issues with job scripts that need different module environments.
```

## Additional Resources

- [R](../software/r.md)

## Questions?

For questions about available software or module usage, contact <hpc@nmthpc.atlassian.net>.
