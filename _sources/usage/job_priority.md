# Job Priority

The Slurm scheduler manages resources and determines the order in which jobs are considered for execution. On Aire and Calder, job priority is influenced by several factors, including **fair share**, job age, job size, partition, Quality of Service (QoS), and other scheduling factors configured by the system.

Fair share is intended to balance resource usage between users and groups:

- When you use a larger share of the system's resources, your fair share generally decreases and your jobs receive lower priority.
- When you use fewer resources relative to your assigned share, your fair share generally increases and your jobs receive higher priority.
- The effect of previous resource usage decreases over time according to the scheduler's configured decay.

Fair share does not impose a fixed quota on users. Instead, it is one component of job priority that influences which jobs are scheduled first. The resources that contribute to fair share are determined by the system's Slurm configuration and may include CPUs, memory, GPUs, or other trackable resources.

## Understanding fair share

Slurm's fair share factor is a value between `0.0` and `1.0`. A higher value indicates a stronger fair share position and contributes more positively to job priority. The exact priority of a job is determined by fair share together with the other priority factors configured on the system. citeturn0search1

As a general guide:

| Fair share | General interpretation |
| ---: | --- |
| Close to `1.0` | Strong fair share position |
| Around `0.5` | Intermediate fair share position |
| Close to `0.0` | Weak fair share position |

These values are only a guide. The Fair Tree algorithm ranks users within the fair share hierarchy, so the significance of a particular value depends on the users, accounts, and shares configured on the system.

You can check your fair share information with:

```bash
sshare -l
```

The `FairShare` column shows the fair share factor. With `-l`, `sshare` also displays `Level FS`, which shows the fair share value at each level of the account hierarchy.

To see the priority components of your pending jobs, use:

```bash
sprio --user=$USER
```

The `FAIRSHARE` column shows the fair share component of each job's priority. You can also inspect a particular job:

```bash
sprio --jobs=123456
```

The `sprio` command can be used to see how fair share contributes to a job's overall priority alongside other factors such as age and job size.

:::{tip}
If a job is pending, use `squeue` to check its current state and pending reason:

```bash
squeue --job=123456
```

You can also request an estimated start time:

```bash
squeue --job=123456 --start
```

The estimated start time is not guaranteed and can change as the state of the system and the queue changes.
:::

## How fair share recovers

The effect of previous resource usage on fair share decreases over time according to Slurm's configured decay. This is controlled by parameters such as `PriorityDecayHalfLife`, so the time required for your fair share to recover depends on the scheduler configuration. citeturn0search1

:::{warning}
**Request reasonable resources for your jobs.**

Fair share is not something that Research IT can manually increase for an individual user simply because their jobs are waiting in the queue. If your fair share has been reduced by previous resource usage, it will recover through the normal scheduling process as the effect of that usage decays.
:::

Request enough CPU, memory, GPU, and wall time for your application to run successfully, but avoid substantially overestimating your requirements. Resource usage contributes to fair share according to the system's Slurm configuration, while unnecessarily large resource requests can also make a job harder to schedule.

If you are unsure how many CPUs, how much memory, how many GPUs, or how much wall time your application requires, please get in touch with us. We can help you make a sensible first estimate and refine your resource request based on your application's behaviour.

## What to expect when submitting jobs

Fair share is only one component of job priority. Slurm combines the enabled priority factors according to the system configuration, so a job with a stronger fair share does not necessarily run before every job with a weaker fair share. citeturn0search1

As a result:

- A job with a stronger fair share does not necessarily run immediately.
- A job submitted later can sometimes run before an older job if it has a sufficiently higher overall priority and the required resources are available.
- Job priority can change while a job is waiting in the queue.
- Small jobs may sometimes be scheduled around larger jobs when this allows the scheduler to use otherwise idle resources.
- Requesting different resources can affect both the job's priority and how easily it fits into available resources.

## Useful commands

| Command | Purpose |
| --- | --- |
| `sshare -l` | View fair share and usage information |
| `sprio --user=$USER` | View priority components for your pending jobs |
| `sprio --jobs=JOBID` | View priority components for a specific job |
| `squeue --user=$USER` | View your queued and running jobs |
| `squeue --job=JOBID --start` | View an estimated start time for a pending job |

For more information, see the official Slurm documentation for [Multifactor Job Priority](https://slurm.schedmd.com/priority_multifactor.html), [Fair Tree Fairshare](https://slurm.schedmd.com/fair_tree.html), [`sshare`](https://slurm.schedmd.com/sshare.html), and [`sprio`](https://slurm.schedmd.com/sprio.html).
