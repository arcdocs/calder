# Modules and Applications

This page provides an overview of how software applications are managed on Aire and Calder.

## Module system explained

The HPC systems use a *module* system to manage software applications, a common approach on HPC systems. Modules make it possible to use multiple applications and versions without conflicts, while ensuring that centrally installed software is configured and optimised for the system.

Calder uses a hierarchical module system: the software you can see depends on the CPU or GPU environment and the compiler or MPI stack currently loaded. Aire uses a flat module system, where `module avail` lists all available software directly. See [Using modules on Calder](#using-modules-on-calder) for details.

## Installing and managing modules

Popular software applications are installed and maintained centrally by the [Research Computing](https://arc.leeds.ac.uk/) team. This saves time, ensures the software is optimised for the HPC hardware, and prevents conflicts between applications that provide files or libraries with the same name. Access to licensed software can also be restricted to the appropriate user groups.

```{admonition} Requesting new modules
:class: note
If software is likely to be useful to multiple users, you can [request a new software installation](page:request-software). Requests are assessed for suitability as a shared application. Most open-source software can be incorporated, and commercial software can be supported where suitable licences are available.
```

(using-modules-on-calder)=

## Using modules on Calder

Calder uses a hierarchical module system to provide software environments for both CPU and GPU workloads.

### Recommended module configuration

After logging in, we recommend setting the module path to use only the Calder software stack:

```bash
export MODULEPATH=/uol/apps/calder/modules
```

This reduces the likelihood of accidentally loading software from another system and helps ensure you are working with the intended Calder software environment.

:::{note}
Integration of the software environments across Aire and Calder is ongoing, so the separation between the two is not yet fully refined.

Even without modifying `MODULEPATH`, Calder modules will still be available. However, you may see software from multiple environments, which can make it difficult to determine exactly what is being loaded.

To avoid confusion, we recommend setting the `MODULEPATH` as shown above.
:::

### Discovering available software

Once the module path has been configured, you can view the top-level environments by running:

```bash
module avail
```

You should see something similar to:

```text
------------- /uol/apps/calder/modules ------------

calder/cpu calder/hopper (D)
```

The `(D)` indicates the default environment.

`module avail` only shows software that is currently available within the active layer of the module hierarchy. Initially, only the top-level environments are visible:

- `calder/cpu`
- `calder/hopper`

After loading one of these environments, additional software becomes available. For example:

```bash
module load calder/cpu
module avail
```

will reveal software built for the CPU environment. Similarly:

```bash
module load calder/hopper
module avail
```

will reveal software built for the Hopper GPU environment. As you load additional compiler, MPI, or software modules, further layers of compatible software become available.

### Using `module spider`

If you want to explore all software available in the module tree, we recommend using:

```bash
module spider
```

Unlike `module avail`, this command searches the entire hierarchy and displays software that may not yet be visible in the current environment.

Once you find a package of interest, you can obtain instructions for loading it:

```bash
module spider <module-name>
```

For example:

```bash
module spider gromacs
```

This will display the required modules and loading sequence needed to access the requested software.

## Using modules on Aire

On Aire, users can list, load, and unload modules using the `module` command:

List currently available modules:

```bash
module avail
```

Load a specific module:

```bash
module load <module-name>
```

Unload a module (useful for avoiding module conflicts and for workflow separation):

```bash
module unload <module-name>
```
