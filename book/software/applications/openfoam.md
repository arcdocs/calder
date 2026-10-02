# OpenFOAM

OpenFOAM is an open-source computational fluid dynamics (CFD) toolbox widely used for simulating fluid flow, turbulence, heat transfer, and other complex physical processes.

This page provides guidance on using OpenFOAM on Calder, including loading the available versions and submitting parallel jobs. For detailed information about OpenFOAM, its solvers, physical models, case setup, and advanced usage, please refer to the [official OpenFOAM documentation](https://www.openfoam.com/documentation).

## Available versions

The following versions of OpenFOAM are currently available on Calder:

| Version | Module |
| --- | --- |
| OpenFOAM 12 | `openfoam-org/12` |
| OpenFOAM v2512 | `openfoam/2512` |

Calder uses a hierarchical module system. Before loading OpenFOAM, load the Calder CPU environment and the required OpenMPI version:

```bash
module load calder/cpu
module load openmpi/5.0.10
```

You can then load the OpenFOAM version you require:

```bash
module load openfoam-org/12
```

or:

```bash
module load openfoam/2512
```

## Running OpenFOAM in parallel

OpenFOAM supports parallel execution using MPI. A parallel OpenFOAM case is divided into multiple subdomains using `decomposePar`, with each subdomain processed by an MPI process. After the simulation has completed, the results can be combined using `reconstructPar`.

The number of subdomains should normally match the number of MPI processes requested from Slurm. This is configured in the `system/decomposeParDict` file using the `numberOfSubdomains` entry.

### Single node

The following example runs an OpenFOAM case using 16 CPU cores on a single node:

```bash
#!/bin/bash
#SBATCH --job-name=openfoam
#SBATCH --time=01:00:00
#SBATCH --nodes=1
#SBATCH --ntasks=16
#SBATCH --mem=128G

module load calder/cpu
module load openmpi/5.0.10
module load openfoam/2512

# Decompose the case for parallel execution
decomposePar

# Run the simulation
mpirun -np $SLURM_NTASKS simpleFoam -parallel > log.simpleFoam

# Reconstruct the results
reconstructPar
```

Before submitting the job, make sure that `numberOfSubdomains` in `system/decomposeParDict` is set to `16`.

### Multiple nodes

OpenFOAM can also use multiple compute nodes. For example, the following job uses 64 CPU cores across two nodes:

```bash
#!/bin/bash
#SBATCH --job-name=openfoam
#SBATCH --time=04:00:00
#SBATCH --nodes=2
#SBATCH --ntasks=64
#SBATCH --ntasks-per-node=32
#SBATCH --mem=256G

module load calder/cpu
module load openmpi/5.0.10
module load openfoam/2512

# Decompose the case for parallel execution
decomposePar

# Run the simulation across the allocated nodes
mpirun -np $SLURM_NTASKS simpleFoam -parallel > log.simpleFoam

# Reconstruct the results
reconstructPar
```

For this example, set `numberOfSubdomains` in `system/decomposeParDict` to `64`.

:::{note}
The examples above use `simpleFoam` as an example solver. Replace it with the solver appropriate for your OpenFOAM case.

The number of MPI processes should match the number of subdomains specified in `decomposeParDict`. When changing the number of CPU cores or nodes requested, update `numberOfSubdomains` accordingly.

The amount of memory requested with `--mem` is specified per node. Choose a value appropriate for the size and memory requirements of your case.
:::
