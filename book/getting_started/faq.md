# Frequently Asked Questions

This page answers common questions from users during the launch of Calder. If your question is not answered here, please see [How to get support](../support/start.md).

## Aire and Calder

**What is the difference between Aire and Calder, and when should I use each system?**

Calder is the newer HPC system and provides increased compute and storage capacity compared with Aire. It has 12,288 CPU cores across 96 standard CPU nodes, compared with 9,072 CPU cores across the standard and high-memory CPU nodes on Aire. Calder also provides 6.3 PB of Lustre scratch storage and 246 TB of Lustre flash storage, compared with 3.7 PB and 139 TB respectively on Aire.

The two systems also have different hardware and system configurations. Calder uses a newer AMD EPYC 9555 CPU platform and InfiniBand networking, while Aire uses AMD EPYC 9634 processors and Omni-Path networking.

For CPU workloads, Calder is particularly well suited to jobs that require substantial computational resources or parallel execution across multiple nodes. Calder has a minimum CPU allocation of 8 cores, and its InfiniBand network provides a high-performance interconnect for workloads that require communication between nodes.

Calder also provides NVIDIA H200 NVL GPUs, while Aire provides NVIDIA L40S GPUs. The H200 has 141 GB of HBM3e memory and 4.8 TB/s of memory bandwidth, making it well suited to demanding scientific computing, HPC, and AI/ML workloads. The L40S has 48 GB of GDDR6 memory and is well suited to a range of AI, machine learning, inference, visualisation, and other GPU workloads. The most appropriate system therefore depends on the requirements of your application.

As a general guide:

- **Use Calder** for new or demanding CPU workloads, particularly large multi-node parallel jobs, and for workloads that benefit from the capabilities of the H200 GPUs.
- **Use Aire** when your workload has specific requirements that are better supported by its existing hardware or software environment, or while your workflow is being migrated to Calder.
- **If you are unsure which system is most appropriate**, please contact us. We can help you assess your application's requirements and identify a suitable system.

Both systems use Slurm as their job scheduler, but partitions, resource limits, software environments, and some hardware-specific features differ between them. Always refer to the system-specific documentation when submitting a job.

**Will my existing Aire job scripts work on Calder?**

Many Slurm directives and commands will be familiar, but you should review your scripts before using them on Calder. Module names and the module system, partitions, resource limits, and hardware-specific settings may differ. Test your script with a representative job before moving a production workload.

**Is my software available on Calder?**

Check the Calder software documentation for available applications, versions and module names. Calder uses a hierarchical module system, so the commands required to load software may differ from Aire. Some software versions or dependencies may also differ between systems. If you cannot find the software you need, please contact us.

**Do I need to move my data to Calder?**

Home directories are shared between Aire and Calder. However, storage arrangements and quotas differ between systems, and you should check the [Filesystem Quotas](page:filesystem-quotas) documentation before moving data or planning a workflow. In particular, Lustre quotas on Aire are applied per user, while Calder quotas are applied per Slurm project. Check which storage location your job uses and whether the data is accessible from both systems before assuming that files are available in the same location.

**Can I continue using Aire while trying Calder?**

Yes. You can test representative workloads on Calder while continuing to use Aire for existing workflows. You do not need to migrate everything at once. Before moving a production workflow, check that the required software and data are available, test the job, and confirm that its results and performance meet your needs.

## Accounts and Access

**Do I need to request a separate account to use Calder?**

No. There is a single HPC user group covering both systems, so there is no separate application process for Calder. If you already have an Aire account, you will be able to access Calder once it is available, and new applicants will get access to both systems through the existing [HPC account request form](request_account.md).

**Will my home directory, Slurm account, and quotas carry over from Aire?**

Yes. Home directories are shared between Aire and Calder, and the same onboarding process that currently creates your home directory, Slurm association, and Lustre quotas for Aire will also apply to Calder. There is nothing extra you need to do.

## Storage and Quotas

**Does my storage quota work the same way on Calder?**

Not quite. On Aire, Lustre quotas are applied per user. On Calder, Lustre quotas will instead be applied per Slurm project. The full details are still being finalised and will be published on the [Filesystem Quotas](page:filesystem-quotas) page once confirmed.

## Getting Support

**I'm having trouble logging in. What should I include when reporting the issue?**

Please share the exact steps you took, ideally by copying the commands and output directly from your terminal, or with a screenshot if copying is not possible. This helps us diagnose the problem much faster than a general description.

**My job failed or isn't behaving as expected. What should I include when reporting it?**

Please include:

- The **job ID**.
- Your **job submission script**.
- The job's **output and error files**.
- Any relevant commands and error messages.
- The output from [`sacct`](https://slurm.schedmd.com/sacct.html) for the job, where available.

For example:

```bash
sacct --job=123456 --format=JobID,JobName,State,Elapsed,AllocCPUS,MaxRSS,ExitCode
```

This can provide useful information about the final state of the job, the resources allocated to it, its elapsed time, memory usage, and exit code. Including this information when reporting a failed or unexpected job can help us identify resource or application issues more quickly.

If the job is still queued or running, [`squeue`](https://slurm.schedmd.com/squeue.html) can be used to check its current state:

```bash
squeue --job=123456
```

For more information about investigating jobs, see the [Job Scheduler](../system/job_scheduler.md) documentation.

:::{seealso}
See [How to get support](../support/start.md) for details on raising an issue or submitting a Research IT ticket.
:::

## Shell Configuration

**What's the difference between `.bashrc` and `.bash_profile`, and which one should I use?**

`.bashrc` is sourced for every new shell that is spawned and should be used only for defining shell functions, not for updating your environment or running commands.

`.bash_profile` is executed at the start of each new login session and at the start of each batch job. If you want to tailor your environment, for example by setting custom path or environment variable values, this is the file to use. Any settings you add here are automatically exported to all child processes in that session or job.

```{warning}
Do not include `module load` commands in `.bash_profile`. Doing so can cause unintentional conflicts with modules you load in your job scripts. If you contact the [Research Computing](https://arc.leeds.ac.uk/) team for support and your `.bash_profile` contains `module load` commands, you will be asked to remove them first.
```
