# Leeds HPC User Documentation

This site provides documentation for users of the High Performance Computing (HPC) systems at the [University of Leeds](https://www.leeds.ac.uk).

**Calder** is the latest HPC system at the University of Leeds, following Aire, and launches in October 2026. It introduces new hardware, a new software stack, and an updated user environment, with increased compute and storage capacity compared with Aire. This site has been developed to provide documentation for both systems in one place, making it easier for existing Aire users to transition to Calder while keeping guidance for both systems available during the transition.

## Calder at a glance

Calder provides a significant increase in both compute and storage capacity compared with Aire:

| Resource | Aire | Calder | Increase |
| --- | ---: | ---: | ---: |
| CPU cores | 9,072 | 12,288 | **35%** |
| GPUs | 84 NVIDIA L40S | 56 NVIDIA H200 NVL | — |
| Scratch on Lustre (Disk based) | 3.7 PB | 6.3 PB | **70%** |
| Flash on Lustre (NVMe based) | 139 TB | 246 TB | **77%** |

Calder provides **3,216 additional CPU cores** compared with Aire, increasing the available CPU capacity by approximately 35%. Although Calder has fewer physical GPUs, its NVIDIA H200 NVL GPUs provide a newer and more capable GPU platform than the NVIDIA L40S GPUs available on Aire.

Calder also provides substantially more storage capacity, with approximately 70% more Lustre scratch storage and 77% more Lustre flash storage than Aire.

These figures describe overall system capacity. The performance of individual applications will vary depending on the workload and the hardware resources they use.

Where instructions or system characteristics differ between Aire and Calder, the documentation clearly identifies the relevant system. Where possible, common concepts and guidance are shared between the two systems to simplify the user experience.

## Moving from Aire to Calder

If you already use Aire, you can start exploring Calder without having to migrate your entire workflow at once. Your existing HPC account provides access to both systems, so you can begin by testing a representative job on Calder.

Many core HPC concepts remain the same, including Slurm job submission, command line usage and batch scripts. However, Calder has a different software stack, a hierarchical module system, different hardware, and InfiniBand networking. You may therefore need to adjust your software environment, job scripts or resource requests.

We recommend starting with a small test job before moving larger production workloads:

1. **Check your software:** Find out whether the software and versions you need are available on Calder.
2. **Review your job script:** Check the module commands, partition, CPU and memory requests, and any system specific settings.
3. **Test your workload:** Run a representative job and check its output, performance and resource usage.
4. **Move your workflow when ready:** Once you have confirmed that your application works as expected, you can start running your production workloads on Calder.

You do not need to migrate everything at once. If you are unsure how to adapt your workflow or choose appropriate resources, please contact the Research Computing team for advice.

## An evolving documentation site

This documentation is actively maintained and will continue to be updated as our HPC provision develops and as we learn from users. New software, features, guidance, and improvements will be added over time, so some sections may initially be more complete than others.

The documentation will also evolve alongside the Aire upgrade. Aire is being upgraded with a new software stack, a new module system, and other changes to its user environment. Once the Aire upgrade is complete, documentation for the upgraded Aire system will gradually be incorporated into this site.

The existing [Aire User Documentation](https://arcdocs.leeds.ac.uk/aire/) remains available during this transition. Over time, content will be migrated from the existing Aire documentation to this site. Once the Aire upgrade and documentation migration are complete, the existing Aire Docs site will be retired and this site will become the main documentation site for the University of Leeds HPC systems.

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

As this documentation is actively developed, we welcome your feedback and suggestions. If you find an error, notice missing information, or have suggestions for improving the documentation, please [raise an issue on GitHub](https://github.com/arcdocs/calder/issues) or submit a [Research IT ticket](https://bit.ly/arc-help).
:::
