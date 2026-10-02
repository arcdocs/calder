(page:miniforge)=
# Miniforge

[Miniforge](https://github.com/conda-forge/miniforge) is a minimal distribution of the Conda and Mamba package and environment managers, configured to use the [conda-forge](https://conda-forge.org/) package repository. It allows users to create and manage isolated environments containing software and dependencies for their work.

This page provides guidance on the general use of Miniforge on Calder. For detailed information about Conda, Mamba, package management, and advanced environment management, please refer to the [Conda documentation](https://docs.conda.io/) and [Miniforge documentation](https://conda-forge.org/miniforge/).

:::{note}
For further guidance on creating and managing Conda environments on HPC, including Python and R environments, see [Managing Software Environments](page:environment-management).
:::

## Available version on Calder

The following version of Miniforge is currently available on Calder:

| Miniforge version | Conda version | Load command |
| --- | --- | --- |
| 26.1.1-3 | 26.1.1 | `module load miniforge3/26.1.1-3` |

The Miniforge release version `26.1.1-3` identifies the specific Miniforge distribution, while the Conda version provided by this release is `26.1.1`.

Calder uses a hierarchical module system. Before loading Miniforge, load the Calder CPU environment:

```bash
module load calder/cpu
module load miniforge3/26.1.1-3
```

## Creating a new environment

Conda environments provide isolated environments in which you can install software and Python packages without affecting other users or the system installation.

To create a new environment, for example one named `myenv`, run:

```bash
conda create --name myenv
```

To create an environment with a specific Python version:

```bash
conda create --name myenv python=3.12
```

You can also create an environment from a pre-defined `.yml` file:

```bash
conda env create --file environment.yml
```

For further guidance on creating and managing environments, see [Managing Software Environments](page:environment-management).

## Activating an environment

To activate an environment, use:

```bash
conda activate myenv
```

Once activated, packages and applications installed in the environment are available in your shell.

You can check which environment is currently active with:

```bash
conda info --envs
```

The active environment is marked with an asterisk (`*`).

## Deactivating an environment

To deactivate the current environment, use:

```bash
conda deactivate
```

## Submitting a job with Miniforge

Conda environments can be used within Slurm jobs to run applications and scripts with the required software and dependencies.

The following example requests 8 CPU cores and runs a Python script from the Conda environment `myenv`:

```bash
#!/bin/bash
#SBATCH --job-name=miniforge
#SBATCH --output=output_%j.out
#SBATCH --error=error_%j.err
#SBATCH --time=01:00:00
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8

module load calder/cpu
module load miniforge3/26.1.1-3

conda activate myenv

# Run your application
python my_script.py
```

The resources requested from Slurm should reflect the requirements of the application being run. For example, if the application is single threaded, requesting multiple CPUs may not provide any performance benefit.

:::{note}
Conda environments are normally created in your home directory unless a different location is specified. For large environments or environments containing many packages, consider the storage and quota guidance in [Managing Software Environments](page:environment-management).
:::
