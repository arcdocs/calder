(page:storage-filesystem)=
# Storage and Filesystem

Our HPC systems provide several storage options for different stages of a research workflow. Use your home directory for persistent files, shared scratch storage for datasets and job input/output, and temporary storage for data that only needs to exist while a job is running.

Storage quotas and paths differ between systems. Scratch and temporary storage are **not backed up**, so keep an independent copy of any data you cannot afford to lose.

## Storage at a glance

### Aire

| Storage type | Path | Environment variable | Quota | Backedup | Auto-Delete | Recommended Use |
| --- | --- | --- | --- | --- | --- | --- |
| Home | `/users/<username>` | `$HOME` (also: `~`) | 65 GB and 1.5M files | Yes | No | Scripts, configuration, notes, documentation |
| Scratch (Lustre disk) | `/mnt/scratch/` (also: `/scratch`) | `$SCRATCH` | 1 TB and 1.5M files | No | No | Large datasets and active project data |
| Flash (Lustre NVMe) | `/mnt/flash/tmp/job.<JOB-ID>`; (also: `/flash`) | `$TMP_SHARED` | 1 TB and 1.5M files | No | Yes, at job completion | I/O-intensive work, high-performance shared storage |
| Compute Nodes | `/tmp` (job-specific directories may also be provided) | `$TMP_LOCAL`, `$TMPDIR` | No fixed quota; limited by available space on the node | No | Yes, at job completion | Fast, node-local temporary data for single-node jobs |

### Calder

| Storage type | Path | Environment variable | Quota | Backedup | Auto-Delete | Recommended Use |
| --- | --- | --- | --- | --- | --- | --- |
| Home | `/users/<username>` | `$HOME` (also: `~`) | Shared with Aire | Yes | No | Scripts, configuration, notes, documentation |
| Scratch (Lustre disk) | `/scratch-calder/users/` | | 1 TB and 1.5M files | No | No | Large datasets and active project data |
| Flash (Lustre NVMe) | `/flash-calder/users/` | | 1 TB and 1.5M files | No | | I/O-intensive work, high-performance shared storage |

On Calder, quotas for both scratch filesystems are applied per Slurm project. The current limits are 1 TB and 1.5 million files per project on each filesystem. The `$SCRATCH` and `$TMP_SHARED` environment variables are not currently defined on Calder, so use the paths listed above. Aire 2.0 will also use project-based quotas; the current Aire system uses per-user quotas. For instructions on checking your quota and usage, see [Filesystem Quotas](page:filesystem-quotas). If you are unsure which project quota applies to your work, contact the Research Computing team.

The home directory is shared between Aire and Calder, so files stored in `/users/<username>` are available on both systems. This does **not** mean that the scratch filesystems are shared: use the appropriate path for the system on which you are working.

## Total filesystem capacity

The figures below describe the overall capacity of each filesystem, not the quota available to an individual user or project.

### Aire

| Filesystem | Total space | Total inodes |
| --- | ---: | ---: |
| Home directory | 106 TB | ~2.27 billion |
| Scratch on Lustre (`/mnt/scratch`) | 3.6 PB | ~3.0 billion |
| Flash on Lustre (`/mnt/flash`) | 139 TB | ~293 million |

### Calder

| Filesystem | Total space | Total inodes |
| --- | ---: | ---: |
| Home directory | Shared with Aire | Shared with Aire |
| Scratch on Lustre (`/scratch-calder`) | ~5.5 PB | ~2.30 billion |
| Flash on Lustre (`/flash-calder`) | ~167 TB | ~88.0 million |

:::{admonition} Aire 2.0 upgrade
:class: note

When Aire 2.0 launches, Aire's scratch and flash paths are expected to change to `/scratch-aire` and `/flash-aire` respectively, distinguishing them from Calder's filesystems. Check the updated documentation when the Aire 2.0 upgrade is complete.
:::

## Choosing and managing storage

- **Use your home directory for persistent, relatively small files.** It is backed up, but is not intended for high-I/O workloads.
- **Use disk-based scratch for large datasets and active work.** It is not backed up and files are not automatically deleted, so remove data you no longer need and keep important data elsewhere.
- **Use flash scratch for jobs with demanding I/O requirements.** It uses NVMe-based storage and is not backed up. Treat it as temporary job storage and check the relevant job environment and file lifetime before using it.
- **Use compute-node scratch for node-local temporary files.** It is not shared between nodes, so a file written to one node's `/tmp` is not automatically available on another node.
- **Do not treat any scratch area as your only copy of important data.** Copy important results to backed-up storage or an appropriate external storage location.
- **Monitor both space and file counts.** A quota can be reached by using too much storage or by creating too many files.

For more guidance, see [File and Data Management](../usage/file_management/start.md), including [Best Practices](page:best-practices) and [Filesystem Quotas](page:filesystem-quotas).

## When a filesystem becomes full

Per-user or per-project quotas help manage individual usage, but they do not guarantee that the filesystem as a whole will have free space. Quotas may be oversubscribed because not every user or project uses its full allowance at the same time. As a result, a filesystem can become critically full even when some users are within their quotas.

As filesystem usage approaches capacity, performance and reliability can be affected. At high utilisation, users may experience slower jobs, failed writes or incomplete output. If a filesystem reaches 100% usage, write operations may fail with `No space left on device`, jobs may fail, and files being written may be truncated or corrupted.

If a filesystem becomes critically full, Research IT may need to take urgent action to recover space and protect the service. This may include:

1. Temporarily suspending job scheduling so that additional jobs do not increase the pressure on the filesystem.
2. Emailing users to request urgent data cleanup.
3. Removing files to recover space, potentially without individual warning in an emergency.
4. Rebooting or otherwise recovering affected services if necessary.

:::{warning}
Emergency recovery actions may temporarily interrupt access to the system. In exceptional circumstances, immediate action may be necessary to protect the wider service, and files may be removed without warning. Keep independent copies of important data and respond promptly to Research IT notices.
:::

## A shared responsibility

Storage is a shared resource, and users play an important role in keeping it reliable for everyone. Please:

- Review and clean up data regularly, especially on scratch filesystems.
- Monitor your filesystem quotas and file counts.
- Follow the [Best Practices](page:best-practices) for data management and storage usage.
- Follow the [Rules and Regulations for using Aire](page:rules-regulations) where applicable.
- Respond promptly to system alerts and emails from Research IT.

Good storage management helps protect your research data and reduces the risk of disruption for the wider HPC community.
