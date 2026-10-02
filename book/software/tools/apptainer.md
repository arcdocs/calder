# Apptainer

[Apptainer](https://apptainer.org/) is a containerisation platform that allows users to package software and its dependencies into portable, reproducible containers. Apptainer containers can be transferred between systems and run without requiring the software to be installed directly on the HPC system.

This page provides guidance on the general use of Apptainer on Calder. For detailed information about Apptainer, including advanced container usage and configuration, please refer to the [official Apptainer documentation](https://apptainer.org/docs/user/main/index.html).

## Apptainer on Calder

Apptainer is installed on Calder and is automatically available when you log in. No module needs to be loaded before using Apptainer. You can check the installed version with:

```bash
apptainer --version
```

```text
apptainer version 1.5.3-1.el9
```

## Running Apptainer containers

To run an Apptainer container, use:

```bash
apptainer run my_container.sif
```

You can also execute a specific command inside a container with `apptainer exec`:

```bash
apptainer exec my_container.sif command
```

For example, to check the Python version provided by a container:

```bash
apptainer exec my_container.sif python --version
```

Apptainer containers can be used within Slurm jobs in the same way as other applications. The resources required by the application should be requested from Slurm, while the container provides the required software environment.

### Using Apptainer with MPI

Apptainer can be used with MPI applications, including applications running across multiple compute nodes. MPI container usage requires additional configuration and depends on how both the container and the host system are built.

For guidance and examples, including the use of Apptainer with Slurm, see the [MPI section of the official Apptainer documentation](https://apptainer.org/docs/user/main/mpi.html).

## Building Apptainer containers

If you need to build an Apptainer container from a definition file, use:

```bash
apptainer build my_container.sif my_definition.def
```

Building containers may require additional privileges or access to a suitable build environment. If you cannot build a container directly on Calder, you can build it elsewhere and transfer the resulting `.sif` file to Calder.

For detailed information about definition files, building containers, and other container workflows, see the [official Apptainer documentation](https://apptainer.org/docs/user/main/index.html).

:::{note}
More information about containers and their use on HPC is available in the [HPC2 training course](https://arctraining.github.io/hpc2-software/course/containers.html).
:::
