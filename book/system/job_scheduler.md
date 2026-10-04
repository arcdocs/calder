(page:job-scheduler)=
# Job Scheduler

A job scheduler, also known as a queueing system or workload manager, is software that manages jobs submitted to an HPC system. It allocates compute resources to jobs according to the system's scheduling policies, allowing many users to run workloads efficiently and fairly.

A job scheduler is particularly useful for HPC workloads because jobs may require substantial resources or run for long periods. Rather than waiting for a job to finish in an interactive terminal, you can submit it to the scheduler and allow it to run when the required resources become available.

## Why use a job scheduler?

The job scheduler helps to:

- **Use resources efficiently:** HPC systems are expensive to operate and consume significant amounts of energy. Scheduling jobs efficiently helps maximise system throughput and resource utilisation.
- **Save users time:** Long running or repetitive workloads can run without requiring you to monitor them continuously.
- **Share resources fairly:** The scheduler manages access to the available resources so that users and groups can share the system effectively.

## Which job scheduler do Aire and Calder use?

Both Aire and Calder use [Slurm](https://slurm.schedmd.com/), a widely used workload manager in HPC systems.

Slurm is configured differently at different HPC centres to meet local requirements. However, the main commands and options are generally the same or similar between systems.

:::{admonition} Used Slurm before?
:class: tip

If you have used Slurm on another HPC system, many of the commands and concepts will be familiar. However, always check the local documentation for the partitions, resource limits, and other configuration specific to Aire and Calder.
:::

## Submitting a job to the scheduler

To submit a batch job, first create a job script containing the commands needed to run your application. The script also specifies the resources required by the job, such as the number of CPU cores, memory, GPUs, and the maximum run time.

You can then submit the script using Slurm's [`sbatch`](https://slurm.schedmd.com/sbatch.html) command:

```bash
sbatch my_job.sh
```

Slurm will assign a job ID when the job is submitted. For example:

```text
Submitted batch job 123456
```

The job ID can be used to monitor the job and inspect its accounting information after it has completed.

For interactive jobs, use Slurm's [`srun`](https://slurm.schedmd.com/srun.html) command.

```{seealso}
For examples of job scripts and guidance on different types of jobs, see [Job Types and Examples](../usage/jobs.md).
```

## After a job is submitted

After a job is submitted, Slurm determines when it can run based on the resources requested, the availability of those resources, and the scheduling policies in place.

If the required resources are available and the job can be scheduled, it will start running. Otherwise, it will remain in the queue until resources become available and the job reaches a position where it can run.

### Checking the status of a job

Use [`squeue`](https://slurm.schedmd.com/squeue.html) to view jobs that are currently queued or running.

To see your own jobs:

```bash
squeue --user=$USER
```

You can also check a specific job by providing its job ID:

```bash
squeue --job=123456
```

The job's state is shown in the `ST` or `STATE` column. Common states include:

- `PD` (`PENDING`) - the job is waiting to run.
- `R` (`RUNNING`) - the job is currently running.
- `CG` (`COMPLETING`) - the job has finished its main work and is completing.
- `CD` (`COMPLETED`) - the job completed successfully.
- `F` (`FAILED`) - the job terminated unsuccessfully.
- `CA` (`CANCELLED`) - the job was cancelled.

For pending jobs, the `REASON` column can provide useful information about why the job has not started.

:::{tip}
If a job is waiting in the queue, do not assume that it is stuck. The scheduler may be waiting for the resources requested by the job to become available or for the job to reach a sufficiently high scheduling priority.
:::

### Checking completed jobs with `sacct`

Once a job has finished, it will normally no longer appear in `squeue`. Use [`sacct`](https://slurm.schedmd.com/sacct.html) to view accounting information for completed jobs and job steps.

For example:

```bash
sacct --job=123456
```

This provides information such as the job state, elapsed time, and exit code. You can request specific fields to make the output easier to interpret:

```bash
sacct --job=123456 --format=JobID,JobName,State,Elapsed,AllocCPUS,MaxRSS,ExitCode
```

Some useful fields include:

- `JobID` - the job or job step ID.
- `JobName` - the name of the job.
- `State` - the final state of the job.
- `Elapsed` - the amount of wall clock time for which the job ran.
- `AllocCPUS` - the number of CPUs allocated to the job.
- `MaxRSS` - the maximum resident memory used by the job or job step.
- `ExitCode` - the exit code returned by the job.

`sacct` is particularly useful after a job has completed because it can help you understand how the job ran and whether the resources requested were appropriate. For example, `MaxRSS` can help identify whether you requested substantially more memory than the job required.

For more information about the fields available in `sacct`, see the [Slurm `sacct` documentation](https://slurm.schedmd.com/sacct.html).

### Job output

Unless otherwise specified in the job script, Slurm writes the standard output and standard error from a batch job to an output file in the directory from which the job was submitted.

You can specify separate output and error files using:

```bash
#SBATCH --output=output_%j.out
#SBATCH --error=error_%j.err
```

Here, `%j` is replaced by the job ID.

### Cancelling a job

If you need to stop a queued or running job, use [`scancel`](https://slurm.schedmd.com/scancel.html) with the job ID:

```bash
scancel 123456
```

You can cancel all of your jobs with:

```bash
scancel --user=$USER
```

:::{note}
Use `scancel` carefully. Cancelling a running job will terminate it, and any work that has not been saved may be lost.
:::

## Choosing resources for your job

When submitting a job, request the resources that your application actually needs. These can include:

- CPU cores
- Memory
- GPUs
- Number of nodes
- Wall time

Requesting substantially more resources than your application requires can increase the time you wait for the job to start and can reduce overall system utilisation. Conversely, requesting too few resources may cause the job to fail or perform poorly.

The available resources and limits differ between Aire and Calder. Refer to the system specific guidance when selecting a partition and requesting resources.

```{seealso}
See [Job Types and Examples](../usage/jobs.md) for examples of resource requests and job submission scripts.
```
