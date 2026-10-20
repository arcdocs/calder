# Leeds HPC Documentation: Aire & Calder

Welcome to the central documentation for the University of Leeds High Performance Computing (HPC) systems. 

**Calder**, our flagship system (Launched Oct 2026), operates alongside **Aire**. This site is your primary guide for migrating workflows and leveraging our new H200 GPU architecture.

::: {grid} 1 2 2 3
:gutter: 2

^++^ **New to HPC?**
Start with our [Getting Started Guide](getting_started/start) to get your account and learn the basics.

^++^ **Moving to Calder?**
View the [Migration Checklist](#moving-from-aire-to-calder) and software compatibility table.

^++^ **Technical Specs**
Detailed hardware breakdown for [Calder](system/calder_hardware) and [Aire](system/aire_hardware).
:::

---

## System Comparison

While Calder has a smaller physical GPU count, the **H200 NVL** nodes offer significantly higher memory bandwidth and throughput for AI and simulation workloads.

| Resource | Aire (L40S) | Calder (H200 NVL) | Impact |
| :--- | :--- | :--- | :--- |
| **Compute Core Type** | Intel Ice Lake | Next-Gen Architecture | ~35% more throughput |
| **GPU Architecture** | Ada Lovelace | Hopper (H200) | High-bandwidth Memory (HBM3e) |
| **Interconnect** | 100Gbps | 400Gbps NDR InfiniBand | 4x faster node communication |
| **Total Scratch** | 3.7 PB | 6.3 PB | Larger datasets, faster I/O |

---

## Moving from Aire to Calder: The 60-Second Summary

Most Slurm commands are identical, but the **Software Stack** and **Partition Names** have changed.

### 1. Key Changes at a Glance
*   **Partitions:** You must target `calder_gpu` or `calder_cpu` (names to be confirmed via `sinfo`).
*   **Modules:** Calder uses a hierarchical Lmod system. Run `module spider <name>` to find software.
*   **Storage:** Use the new `$SCRATCH` paths for optimal performance on the NVMe tier.

### 2. Job Script Comparison
::::{tab-set}
::: {tab-item} Aire Script (Old)
```bash
#SBATCH --partition=gpu
#SBATCH --nodes=1
#SBATCH --gres=gpu:1
module load cuda/12.1
python my_code.py

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
