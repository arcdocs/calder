# Leeds HPC Documentation

Welcome to the central documentation for tehe High Performance Computing (HPC) systems at the [University of Leeds](https://www.leeds.ac.uk).

**Calder**, our flagship HPC system launched in October 2026, operates alongside **Aire**. This site provides guidance for using **Aire** and **Calder**, including information about accessing the systems, running jobs, managing data and using software. Although the systems share many HPC concepts, some aspects of their hardware, software environments and configuration differ. Where instructions are system-specific, this documentation identifies the relevant system.

## Moving from Aire to Calder

If you already use Aire, you can explore Calder using your existing HPC account. You do not need to move all your workloads at once.

Many core HPC concepts remain the same, including command-line usage and Slurm job submission. However, you may need to adapt your software environment, job scripts or resource requests to account for differences between the systems.

Before running a workload on Calder:

1. **Check software availability:** Confirm that the software and versions you need are available on Calder. If software available on Aire is not yet available on Calder, continue using Aire for that workload. We are gradually adding more software modules and documentation.
2. **Review your job script:** Check module commands, partition names, CPU and memory requests, and any system-specific settings.
3. **Test your workload:** Run a representative job and check its output, performance and resource usage before moving production work.

If you are unsure how to adapt your workflow or choose appropriate resources, contact the Research Computing team for advice.

## An evolving documentation site

This documentation is maintained and updated as our HPC services develop. New software, features and guidance will be added over time.

Aire is also being upgraded to **Aire 2.0**, which will introduce a new software stack, a hierarchical module system and other changes to its user environment. Documentation for Aire 2.0 will be incorporated into this site as the upgrade and documentation migration progress.

The existing [Aire User Documentation](https://arcdocs.leeds.ac.uk/aire/) remains available during this transition. Content will gradually be migrated from the existing site. Once the Aire upgrade and documentation migration are complete, the old Aire Docs site will be retired and this site will become the main documentation site for the University of Leeds HPC systems.

:::{admonition} Using Aire and Calder
:class: note

During the transition, some guidance will apply to both systems, while other sections will provide separate instructions for Aire and Calder.

Where a command, job submission script, or hardware detail differs between systems, the documentation uses tabs to show the relevant information. Select the **Aire** or **Calder** tab to view the instructions for the system you are using.

For example, a job submission page may provide separate scripts for Aire and Calder while keeping the common Slurm concepts in one place.
:::

New users may want to start by reading the [Getting Started](getting_started/start) section.

:::{seealso}
To expand your expertise, you might be interested in exploring the [training courses](https://arc.leeds.ac.uk/courses/) offered by the [Research Computing](https://arc.leeds.ac.uk/) team. You can also see the [HPC Architecture](system/hpc_architecture.md) section for a basic introduction to the systems.
:::

:::{admonition} Give us feedback
:class: note

We welcome feedback and suggestions to help improve this documentation. If you find an error, notice missing information or have suggestions, please [raise an issue on GitHub](https://github.com/arcdocs/calder/issues) or submit a [Research IT ticket](https://bit.ly/arc-help).
:::
