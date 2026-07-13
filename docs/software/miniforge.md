# Miniforge

Miniforge is a lightweight Conda distribution that provides Python package and environment management. It is the recommended way to create and manage Conda environments on NMTHPC.

## Python and R Support

Miniforge provides the Conda package and environment manager, allowing users to create isolated environments for Python, R, and many other scientific applications. Using Conda environments helps avoid dependency conflicts and makes workflows easier to reproduce.

## Why Use Miniforge?

**Benefits**:

- Create isolated environments for different projects
- Easy installation of packages and dependencies
- Manage different Python versions
- Avoid conflicts between package requirements
- Share reproducible environments with collaborators

## Loading Miniforge

```bash
$ module load miniforge3
```

Loading the `miniforge3` module makes the `conda` command available in your shell.

**Verify installation**:
```bash
$ conda --version
$ which conda
```


## Configuring Conda

Conda stores its configuration in a file named `.condarc` located in your home directory (`~/.condarc`). This file controls where Conda stores environments and downloaded packages.

View or edit the file with your preferred text editor:

```bash
$ nano ~/.condarc
```

After saving changes, future Conda commands will automatically use the updated configuration.

## Creating Environments

### Basic Environment

**Create environment with specific Python version**:
```bash
$ conda create -n myenv python=3.11
```

**Activate the environment**:
```bash
$ conda activate myenv
```

**Deactivate when done**:
```bash
$ conda deactivate
```

### Environment with Packages

**Create environment and install packages**:
```bash
$ conda create -n data_analysis python=3.11 numpy pandas matplotlib scipy
```

**Install additional packages later**:
```bash
$ conda activate data_analysis
$ conda install scikit-learn seaborn
```

### R Environment

Create an environment with R:

```bash
$ conda create -n r_env r-base
```

Activate it:

```bash
$ conda activate r_env
```

Start an R session:

```bash
$ R
```

Install packages from CRAN as usual:

```R
install.packages("ggplot2")
```

## Managing Packages

### Installing Packages

**From conda**:
```bash
$ conda install numpy scipy matplotlib
```

**From conda-forge** (larger package repository):
```bash
$ conda install -c conda-forge package_name
```

**From pip** (when package not in conda):
```bash
$ pip install package_name
```

```{tip}
Prefer `conda install` over `pip install` when possible. Conda handles dependencies better within conda environments.
```

### Listing Packages

**Packages in current environment**:
```bash
$ conda list
```

**Search for available packages**:
```bash
$ conda search package_name
```

### Updating Packages

**Update specific package**:
```bash
$ conda update numpy
```

**Update all packages**:
```bash
$ conda update --all
```

### Removing Packages

```bash
$ conda remove package_name
```

## Managing Environments

### List Environments

```bash
$ conda env list
```

or

```bash
$ conda info --envs
```

### Clone Environment

**Create copy of existing environment**:
```bash
$ conda create --name newenv --clone oldenv
```

### Remove Environment

```bash
$ conda env remove --name myenv
```

## Environment Files

### Export Environment

**Create reproducible environment file**:
```bash
$ conda activate myenv
$ conda env export > environment.yml
```

**environment.yml** contains all packages and versions.

### Create Environment from File

**On another system or for collaborators**:
```bash
$ conda env create -f environment.yml
```

### Minimal Environment File

**Manually create environment.yml**:
```yaml
name: myproject
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.11
  - numpy
  - pandas
  - matplotlib
  - scikit-learn
  - pip
  - pip:
    - some-pip-only-package
```

**Create from file**:
```bash
$ conda env create -f environment.yml
```

## Using Conda in Job Scripts

### Interactive Jobs

```bash
$ srun --pty bash
$ module load miniforge3
$ conda activate myenv
$ python my_script.py
```

### Batch Jobs

```bash
#!/bin/bash
#SBATCH --job-name=conda_job
#SBATCH --output=conda_%j.out
#SBATCH --ntasks=1
#SBATCH --mem=16G
#SBATCH --time=04:00:00

# Load Miniforge
module load miniforge3

# Activate environment
conda activate myenv

# Run Python script
python analysis.py
```

```{note}
Use `conda activate` after loading the Miniforge module to activate your environment.
```

## Common Environments for HPC

### Data Science Environment

```bash
$ conda create -n datascience python=3.11 \
    numpy pandas matplotlib seaborn scikit-learn \
    jupyter notebook ipython
```

### Machine Learning Environment

```bash
$ conda create -n ml python=3.11 \
    numpy pandas scikit-learn \
    pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
```

or for TensorFlow:
```bash
$ conda create -n tensorflow python=3.11 \
    numpy pandas matplotlib \
    tensorflow-gpu cudatoolkit=12.1
```

### Bioinformatics Environment

```bash
$ conda create -n bio python=3.11 \
    biopython pandas numpy matplotlib \
    -c bioconda -c conda-forge
```

### Scientific Computing Environment

```bash
$ conda create -n science python=3.11 \
    numpy scipy matplotlib \
    sympy numba h5py netcdf4
```

## Using Mamba

Mamba is a fast, drop-in replacement for Conda that can significantly reduce package installation time.

Install Mamba into your environment:

```bash
$ conda install -c conda-forge mamba
```

Then replace `conda` commands with `mamba`:

```bash
$ mamba install numpy pandas scipy
```

Most Conda commands work the same with Mamba.

## Best Practices

### Environment Location

By default, conda creates environments in `~/.conda/envs/`.

**Check environment size**:
```bash
$ du -sh ~/.conda/envs/*
```

**Clean up cached packages**:
```bash
$ conda clean --all
```

### Naming Conventions

Use descriptive names:

- `project_name`: For specific projects
- `python311`: For general Python 3.11 environment
- `ml_gpu`: For machine learning with GPU
- `analysis_2024`: For specific analysis work

### Performance Tips

**1. Use mamba** for faster package resolution:
```bash
$ conda install -c conda-forge mamba
$ mamba install numpy pandas  # Much faster than conda
```

**2. Specify channels in environment file** to avoid conflicts:
```yaml
channels:
  - conda-forge
  - defaults
```

**3. Pin versions** for reproducibility:
```yaml
dependencies:
  - python=3.11.5
  - numpy=1.24.3
  - pandas=2.0.2
```

## Troubleshooting

### Environment Not Found

**After creating environment**:
```bash
$ conda env list  # Make sure it was created
$ conda activate myenv
```

If activation fails:
```bash
$ source activate myenv  # Try source activate
```

### Package Conflicts

**Clear conda cache**:
```bash
$ conda clean --all
```

**Create fresh environment**:
```bash
$ conda deactivate
$ conda env remove -n problematic_env
$ conda create -n problematic_env python=3.11
```

**Install packages one at a time** to identify conflicts:
```bash
$ conda install numpy
$ conda install pandas
# etc.
```

### Conda is Slow

**Use mamba** instead:
```bash
$ conda install -c conda-forge mamba
$ mamba install package_name  # Much faster
```

**Use micromamba** (lightweight alternative):
```bash
# Ask HPC support about micromamba availability
```

### Out of Disk Space

**Check environment sizes**:
```bash
$ du -sh ~/.conda/envs/*
```

**Remove unused environments**:
```bash
$ conda env remove -n unused_env
```

**Clean package cache**:
```bash
$ conda clean --all
```

### Package Conflicts

If Conda cannot resolve package dependencies:

- Install multiple packages together in a single `conda install` command.
- Create a fresh environment instead of trying to repair a heavily modified one.
- Consider using `mamba` for faster dependency resolution.

**Contact HPC support** if you need more quota.

## Example Workflows

### Creating a New Project Environment

```bash
# Load miniforge
$ module load miniforge3

# Create environment
$ conda create -n myproject python=3.11

# Activate environment
$ conda activate myproject

# Install packages
$ conda install numpy pandas matplotlib scikit-learn jupyter

# Export for reproducibility
$ conda env export > environment.yml

# Test it works
$ python -c "import numpy, pandas; print('Success!')"
```

### Using Jupyter with Conda

```bash
# Create environment with Jupyter
$ conda create -n jupyter_env python=3.11 jupyter numpy pandas matplotlib

# Activate and start Jupyter
$ conda activate jupyter_env
$ jupyter notebook --no-browser --ip=0.0.0.0
```

<!-- See [Python and Jupyter Notebooks](python_jupyter.md) for detailed Jupyter instructions. --->

### Machine Learning Workflow

```bash
# Create ML environment
$ conda create -n pytorch_ml python=3.11
$ conda activate pytorch_ml

# Install PyTorch with GPU support
$ conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia

# Install additional packages
$ conda install pandas scikit-learn matplotlib seaborn tensorboard

# Verify GPU support
$ python -c "import torch; print(torch.cuda.is_available())"

# Export environment
$ conda env export > ml_environment.yml
```

## Conda Cheat Sheet

| Task | Command |
|------|---------|
| Load Miniforge | `module load miniforge3` |
| Create environment | `conda create -n myenv python=3.11` |
| Activate environment | `conda activate myenv` |
| Deactivate | `conda deactivate` |
| Install package | `conda install package` |
| List environments | `conda env list` |
| List packages | `conda list` |
| Export environment | `conda env export > env.yml` |
| Create from file | `conda env create -f env.yml` |
| Remove environment | `conda env remove -n myenv` |
| Clean cache | `conda clean --all` |

## Additional Resources

<!-- - [Python and Jupyter Notebooks](python_jupyter.md) --->
<!-- - [Training AI/ML Models on GPUs](ai_ml_gpu.md) --->
- [Official Conda Documentation](https://docs.conda.io/)
- [Conda Cheat Sheet PDF](https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html)

## Questions?

For questions about Miniforge on NMTHPC, contact <hpc@nmthpc.atlassian.net>.

