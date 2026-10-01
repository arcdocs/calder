# Job Types and Examples

Users must always specify a time limit, either at submission or in their job script. The [official Slurm documentation](https://slurm.schedmd.com/documentation.html) is maintained by SchedMD, the developers of Slurm, and so provides a good starting point for anyone who wants to explore the topic in more depth.

Below, each job type is introduced alongside example job scripts demonstrating its use.

## Batch jobs

The vast majority of jobs will be batch jobs. These are submitted via a job script, which is a shell script (file ending `.sh`), containing the commands to run your job, alongside instructions for the job scheduler (Slurm), detailing the resources required. Slurm will then allocate that job in a position in the job queue, depending on a fair share policy. This policy is not first-come-first-served; resources are allocated fairly between different faculties and users. At a bare minimum, the job script must specify how long the job needs to run for. Unless otherwise directed, Slurm will default to the following settings:

- 1 CPU core
- 1GB memory
- Use of standard compute node pool (No GPU access)

```{note}
On Calder's standard compute partition, the minimum CPU request is 8 cores — if you request fewer, Slurm will allocate 8 cores regardless. This minimum doesn't apply to the GPU partition, where CPU and memory are instead allocated proportionally to the number of GPUs requested (see [GPU jobs](#gpu-jobs) below).
```

### Writing job scripts

We encourage users to write their job submission scripts using text editor tools such as: `nano` (recommended for beginners), `vim`, and `emacs`.

The basic approach to create a new job submission file on HPC would be `nano job_submit.sh` or `vim job_submit.sh`. This opens the new empty file in the text editor ready for you to write its contents.

:::{warning}
Job scripts written on Windows computers contain different invisible line ending characters that lead to job submission failures such as `/bin/bash^M: bad interpreter: No such file or directory`. You can use the command `dos2unix job_script.sh` on the login nodes to convert your script to the correct line endings.
:::

A simple demonstration script is provided below. In this example named `job_script1.sh` we are requesting an hour of time, 1GB of memory, and a single CPU core to run an example binary file `example.bin` in the current directory.

```bash
#!/bin/bash
#SBATCH --job-name=simple_job   # Job name
#SBATCH --time=01:00:00         # Request runtime (hh:mm:ss)
#SBATCH --mem=1G                # Request memory
#SBATCH --ntasks=1              # Number of tasks
#SBATCH --cpus-per-task=1       # Number of cores per task

# Load any necessary modules
module load <module_name>

# Execute your application
./example.bin
```

The job script specified above includes a number of lines that request some amount of compute resource for our job. This is defined by the syntax `#SBATCH <option>`. These lines are commented out of the shell script but are read by the scheduler to determine how much compute resource is required and thus how to fit the job into the queue. A {ref}`submission-options` is available below.

### Using the queue

To submit the job script, we use the command `sbatch`:

```bash
$ sbatch job_script1.sh
Submitted batch job 42
```

This returns some text to confirm our job has been submitted and provides us with the job's unique ID number (in this case 42). Arguments can also be submitted to the queue upon job submission, for example:

```bash
$ sbatch -t 1:00:00 job_script1.sh
Submitted batch job 43
```

Commands passed to Slurm in this way will override those in the job script.

We can view the queue status using `squeue`. Use the option `--me` to filter for your own jobs:

```bash
$ squeue --me
JOBID   PARTITION   NAME        USER     ST  TIME NODES  NODELIST(REASON)
42      nodes       simple_job  exuser   R   0:25 10     node[01-10]
43      nodes       other_job   exuser   PD  0:00 16     (resources)
```

Currently running jobs are identified with an `R`; jobs still in the queue show `PD`.

Jobs can be cancelled using `scancel <JOBID>` (in our case, `scancel 42`). Users are only able to cancel their own jobs.

### Serial job examples

This example requests 1 CPU core, 1 hour of runtime, and 32GB of memory for a serial job. It writes standard output and error messages to separate files, using `%j` to include the job ID in each filename for easier troubleshooting.

```bash
#!/bin/bash
#SBATCH --job-name=serial_job         # Descriptive job name
#SBATCH --output=output_%j.out        # Output file (%j = job ID)
#SBATCH --error=error_%j.err          # Error file (%j = job ID)
#SBATCH --time=01:00:00                # Request 1 hour of runtime
#SBATCH --mem=32G                     # Request 32GB of memory
#SBATCH --ntasks=1                     # Request one task
#SBATCH --cpus-per-task=1              # Request one CPU core

# Load any necessary modules
module load <module_name>

# Run the job
./example_aire.bin
```

:::{note}
On Calder's standard compute partition, the minimum CPU request is 8 cores. A serial job will still run on only one core, so the remaining allocated cores will be unused. Calder is therefore better suited to threaded jobs that can use all 8 or more requested cores; see the Threaded subsection below.
:::

## Parallel jobs

Parallel jobs include those run over multiple cores within the same node (SMP, typically via OpenMP) or across multiple nodes via a message passing interface (MPI) such as Open MPI.

:::{note}
Note that in order to use more than one CPU (or core), your program must be specifically written to use a parallel programming model. Just requesting more CPUs in Slurm will not make the program use more than 1 CPU; the extra CPUs you reserved will sit idle.
:::

### Threaded

Here we request 16 cores within a single node for 2 hours. Note that jobs will need to be compiled for execution on multiple threads (e.g., via OpenMP) or run on multithreading-capable software; the below example is for a binary compiled for OpenMP.

::::{tab-set}

:::{tab-item} Aire

```bash
#!/bin/bash
#SBATCH --job-name=threaded_job
#SBATCH --time=02:00:00
#SBATCH --nodes=1
#SBATCH --ntasks=1          # Number of tasks for OpenMP
#SBATCH --cpus-per-task=16  # Number of CPU cores per task

# Load any necessary modules
module load <module_name>

# Tell OpenMP how many resources it has been given
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

# Run the job
./example_aire.bin
```

:::

:::{tab-item} Calder

```bash
#!/bin/bash
#SBATCH --job-name=threaded_job
#SBATCH --time=02:00:00
#SBATCH --nodes=1
#SBATCH --ntasks=1          # Number of tasks for OpenMP
#SBATCH --cpus-per-task=16  # Number of CPU cores per task

# Load any necessary modules
module load <module_name>

# Tell OpenMP how many resources it has been given
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

# Run the job
./example_calder.bin
```

:::
::::

:::{note}
To optimise performance, it is sometimes worth exploring additional OpenMP options such as:

```bash
export OMP_PLACES=cores
export OMP_PROC_BIND=close
```

These settings can help improve thread placement and binding, potentially speeding up your code.
:::

### MPI

These are jobs that run across multiple nodes using a Message Passing Interface (MPI). In the following example, we request 256 MPI processes across 2 nodes, with 128 tasks per node:

::::{tab-set}

:::{tab-item} Aire

```bash
#!/bin/bash
#SBATCH --job-name=MPI_job
#SBATCH --time=04:00:00
#SBATCH --mem=256G              # Request 256GB memory per node
#SBATCH --ntasks=256            # Number of MPI processes
#SBATCH --nodes=2               # Number of nodes
#SBATCH --ntasks-per-node=128   # Number of tasks per node

# Load any necessary modules, e.g. MPI
module load openmpi

# Run the job
srun ./example_aire.bin
```

:::

:::{tab-item} Calder

```bash
#!/bin/bash
#SBATCH --job-name=MPI_job
#SBATCH --time=04:00:00
#SBATCH --mem=256G              # Request 256GB memory per node
#SBATCH --ntasks=256            # Number of MPI processes
#SBATCH --nodes=2               # Number of nodes
#SBATCH --ntasks-per-node=128   # Number of tasks per node

# Load any necessary modules, e.g. MPI
module load openmpi

# Run the job using Slurm's native launcher
srun ./example_calder.bin
```

:::
::::

```{tip}
On both Aire and Calder, `srun` is Slurm's native launcher and is generally recommended for launching MPI applications. It uses the resources allocated by Slurm and works well for multi-node jobs. The systems use different network fabrics: Aire uses Omni-Path and Calder uses InfiniBand. Check the system-specific software and MPI guidance when optimising an application.

Some applications that invoke `mpiexec` or `mpirun` internally may report errors such as "There are not enough slots available" despite the requested resources having been allocated. Where the application allows the MPI launcher to be configured, consider using `srun` instead.

If you are unsure which launcher your application expects, consult the software documentation or contact Research IT.
```

## Job arrays

Situations often arise when you want to run many almost identical jobs simultaneously, perhaps running the same program many times but changing the input data or some argument or parameter. One possible solution is to write a Python or Perl script to create all the job scripts, and then write a BASH script to execute them. This is very time consuming and might end up submitting many more jobs to the queue than you actually need to. Thankfully, this problem is made much easier via Slurm's job array feature:

- Only a single `sbatch` command is issued, and only a single `scancel` command would be required to cancel all jobs
- Only a single entry appears after checking `squeue`
- It is much easier for the user to keep track of your jobs

The easiest way to think of a job array is as a job script with a built-in FOR loop. It makes use of an environment variable created by Slurm - `$SLURM_ARRAY_TASK_ID`. The example script below runs 100 jobs in a Conda environment, with input and output files determined by the array index `$SLURM_ARRAY_TASK_ID`. Job arrays are compatible with many workflows, making them a flexible option for parameter sweeps or batch processing.

::::{tab-set}

:::{tab-item} Aire

```bash
#!/bin/bash
#SBATCH --job-name=task_array_job
#SBATCH --time=01:00:00
#SBATCH --array=1-100%10             # Run job array with indices 1 to 100, allowing up to 10 jobs to run concurrently
#SBATCH --output=arrayjob_%A_%a.out  # Save output to a file named with job ID (%A) and array index (%a)

# Load any necessary modules
# e.g. using a conda environment
module load miniforge
conda activate my_environment

# Run the job, passing in the input and output filenames
python -i $SCRATCH/input/input.$SLURM_ARRAY_TASK_ID -o $SCRATCH/results/out.$SLURM_ARRAY_TASK_ID
```

:::

:::{tab-item} Calder

```bash
#!/bin/bash
#SBATCH --job-name=task_array_job
#SBATCH --time=01:00:00
#SBATCH --array=1-100%10             # Run job array with indices 1 to 100, allowing up to 10 jobs to run concurrently
#SBATCH --output=arrayjob_%A_%a.out  # Save output to a file named with job ID (%A) and array index (%a)

# Load any necessary modules
# e.g. using a conda environment
module load miniforge
conda activate my_environment

# Run the job, passing in the input and output filenames
python -i $SCRATCH/input/input.$SLURM_ARRAY_TASK_ID -o $SCRATCH/results/out.$SLURM_ARRAY_TASK_ID
```

:::
::::

This array job would be submitted as normal using `sbatch task_array_script.sh`. In this example, input files would be read from the `input` directory, and take the form input.1, input.2, input.3 etc. The program would create output files out.1, out.2, out.3 etc in the `results` directory.

### Task arrays (multiple tasks in one job)

Unlike job arrays, which submit many independent jobs, a task array refers to running multiple tasks within a single Slurm job allocation. This is common for MPI jobs or parallel programs where tasks need to communicate.

Slurm uses the `--ntasks` option to specify the number of tasks. All tasks share the same environment and resources, making this ideal for tightly coupled workloads.

::::{tab-set}

:::{tab-item} Aire

```bash
#!/bin/bash
#SBATCH --job-name=task_example
#SBATCH --output=task_%j.out
#SBATCH --error=task_%j.err
#SBATCH --ntasks=10 # 10 tasks in one job
#SBATCH --cpus-per-task=1
#SBATCH --time=01:00:00
#SBATCH --mem=20G

module load openmpi
echo "Running MPI job with $SLURM_NTASKS tasks"
srun ./my_mpi_program
```

:::

:::{tab-item} Calder

```bash
#!/bin/bash
#SBATCH --job-name=task_example
#SBATCH --output=task_%j.out
#SBATCH --error=task_%j.err
#SBATCH --ntasks=10 # 10 tasks in one job
#SBATCH --cpus-per-task=1
#SBATCH --time=01:00:00
#SBATCH --mem=20G

module load openmpi
echo "Running MPI job with $SLURM_NTASKS tasks"
srun ./my_mpi_program
```

:::
::::

## Interactive jobs

It is also possible to run jobs in interactive mode. This means rather than queueing for your job to run on the batch system you request resource for an interactive session via the shell.

:::{warning}
We strongly discourage users from using interactive sessions for interactive code development using platforms like jupyter, spyder or Rstudio. Code development should be done before deploying code on the HPC.
:::

Interactive jobs can be started using the `srun` command, as follows:

```bash
[username@login1 ~]$ srun -t 01:00:00 --pty /bin/bash
[username@node016 ~]$
```

Note that it is required to specify a requested runtime, but the scheduler will otherwise adopt the same default resources as for batch jobs. If the requested resources are available, the session will start immediately. If not, the session will immediately exit. Note that once the session has started the prompt changes to show that the interactive session is now running on a specific compute node (in this case, `node016`).

:::{note}
If you have made changes to your environment on the login nodes these will not be preserved when connecting to an interactive session and you will be required to rerun `module load` commands. Connecting to an interactive session creates a new login shell, which means any commands in your .bashrc are executed when you connect.
:::

(gpu-jobs)=

## GPU jobs

Both Aire and Calder include general-purpose GPU facilities for GPU-accelerated code. The GPU model, partition, and therefore the exact `--gres` value needed to request one, differs between the two systems:

- **Aire**: GPU nodes are equipped with NVIDIA L40S GPUs, 3 per node (gres type `nvidia_l40`), requested via the `gpu` partition. Login nodes are equipped with entry-level NVIDIA A2 GPUs, which is useful for configuration purposes - note that login nodes should not be used to run jobs in order to skip the queue or work around restrictions for interactive jobs.
- **Calder**: GPU nodes are equipped with NVIDIA H200 GPUs, 8 per node (gres type `nvidia_h200`), requested via the `calder-gpu-hopper` partition. GPU nodes are named `calder-gpu-h200-001` onwards.

### Requesting use of GPU nodes

GPUs can be requested with `--partition=<partition>` and `--gres=gpu:<type>:<number>`, where `<partition>` and `<type>` identify the GPU partition and model available on the system (the `gpu` partition and `nvidia_l40` type on Aire; the `calder-gpu-hopper` partition and `nvidia_h200` type on Calder) and `<number>` is the number of GPUs requested. For example, a basic submission script requesting a single GPU might look like:

::::{tab-set}

:::{tab-item} Aire

```bash
#!/bin/bash 
#SBATCH --time=01:00:00                # Request runtime (hh:mm:ss)             
#SBATCH --partition=gpu                # Request the GPU partition
#SBATCH --gres=gpu:nvidia_l40:1        # Request a single GPU

# Load any necessary modules
# module load <module_name>

# Run the job
./example.bin
```

:::

:::{tab-item} Calder

```bash
#!/bin/bash 
#SBATCH --time=01:00:00                   # Request runtime (hh:mm:ss)             
#SBATCH --partition=calder-gpu-hopper     # Request the GPU partition
#SBATCH --gres=gpu:nvidia_h200:1          # Request a single GPU

# Load any necessary modules
# module load <module_name>

# Run the job
./example.bin
```

:::
::::

```{note}
On Calder, if you only request a number of GPUs (without separately requesting `--cpus-per-task` or `--mem`), the CPU cores and memory allocated on the GPU node are proportional to the number of GPUs requested. For example, requesting a single GPU with no other resources specified gives you 1 GPU plus 1/8 of the node's CPU cores and memory, since there are 8 GPUs per node. GPU usage is accounted for and counts towards fair share in the same way as CPU and memory usage.

Additionally, GPU jobs on Calder are limited to a maximum of 16 GPUs per job and per user at any one time, across a maximum of 2 nodes.
```

GPUs are also available in interactive mode. For example, requesting an interactive session with two GPUs:

::::{tab-set}

:::{tab-item} Aire

```bash
[username@login1[aire] ~]$ srun -t 01:00:00 -p gpu --gres=gpu:nvidia_l40:2 --pty /bin/bash
[username@gpu007[aire] ~]$
```

:::

:::{tab-item} Calder

```bash
[username@login1[calder] ~]$ srun -t 01:00:00 -p calder-gpu-hopper --gres=gpu:nvidia_h200:2 --pty /bin/bash
[username@calder-gpu-h200-001 ~]$
```

:::
::::

Here, note again that the prompt has changed to tell us that we are now running on a dedicated GPU node.

To use the GPU hardware you will need to ensure the NVIDIA CUDA toolkit module is loaded into your environment, *before* compiling or running code. This can be done explicitly with `module load cuda`, or via an environment manager such as conda (via miniforge). Again note that changes made to the environment on login nodes will not carry over to compute nodes, so this will need to be done in the job submission script or after starting an interactive session.

### AI/ML jobs on GPU

This example shows how to run a Python job in a Conda environment. To request GPUs, make sure to specify the GPU partition in your Slurm script with `--partition=gpu` on Aire (or `--partition=calder-gpu-hopper` on Calder). Then, request the number of GPUs you need using `--gres=gpu:<type>:N`, where `N` is the number of GPUs; for instance, request 1 GPU with `--gres=gpu:nvidia_l40:1` on Aire (or `--gres=gpu:nvidia_h200:1` on Calder), or 2 GPUs with `--gres=gpu:nvidia_l40:2` on Aire (or `--gres=gpu:nvidia_h200:2` on Calder).

::::{tab-set}

:::{tab-item} Aire

```bash
#!/bin/bash
#SBATCH --job-name=ml_job                # Job name
#SBATCH --time=01:00:00                  # Request runtime (hh:mm:ss)
#SBATCH --partition=gpu                  # Request GPU partition
#SBATCH --gres=gpu:nvidia_l40:1          # Request 1 GPU

# Load any necessary modules, e.g. Miniforge
# Activate conda environment
module load miniforge
conda activate my_ML_environment

# Run the job
python my_ML_script.py
```

:::

:::{tab-item} Calder

```bash
#!/bin/bash
#SBATCH --job-name=ml_job                # Job name
#SBATCH --time=01:00:00                  # Request runtime (hh:mm:ss)
#SBATCH --partition=calder-gpu-hopper    # Request GPU partition
#SBATCH --gres=gpu:nvidia_h200:1         # Request 1 GPU

# Load any necessary modules, e.g. Miniforge
# Activate conda environment
module load miniforge
conda activate my_ML_environment

# Run the job
python my_ML_script.py
```

:::
::::

:::{tip}
Requesting 1 GPU on Aire defaults to using 1 CPU core and 1GB memory for your job unless you request more. If you need more CPU cores and memory, you need to request them separately using additional SBATCH directives. On one Aire GPU node, there are 24 CPU cores and 256GB memory total, divided among 3 GPUs (approximately 8 cores and 85GB memory available per GPU). 

On Calder, if not explicitly specified, CPU and memory on the GPU node scale automatically and proportionally with the number of GPUs requested. on one Calder GPU node, there are 128 CPU cores and 1.5TB memory total, divided among 8 GPUs (16 cores and approximately 192GB memory per GPU by default).
:::

````{admonition} Using the Flash storage
Aire provides temporary Flash storage (`$TMP_SHARED`) for high I/O performance during job execution. This NVMe storage has a quota of 1TB and 1.5M files per job, making it ideal for I/O-intensive workloads like ML/AI. Data is automatically purged when the job ends.

```bash
#!/bin/bash
#SBATCH --job-name=gpu_flash             # Job name
#SBATCH --time=01:00:00                  # Request runtime (hh:mm:ss)
#SBATCH --partition=gpu                  # Request GPU partition
#SBATCH --gres=gpu:nvidia_l40:1          # Request 1 GPU
#SBATCH --cpus-per-task=4                # Request 4 CPU cores
#SBATCH --mem-per-cpu=8G                 # Request 8GB memory per CPU core

# Flash storage path is automatically set as $TMP_SHARED
echo "Flash storage path: $TMP_SHARED"

# Copy input data to Flash storage
cp -r /path/to/input/data $TMP_SHARED/

# Load GPU environment
module load miniforge
conda activate my_ML_environment

# Run GPU job using local data
python my_ML_script.py --data $TMP_SHARED/data

# Copy results back to permanent storage
cp -r $TMP_SHARED/results /path/to/permanent/storage/

# Flash storage ($TMP_SHARED) is automatically cleaned after the job ends
```

The availability and configuration of equivalent temporary storage on Calder will be documented when its hardware details are confirmed.
````

## Large-memory jobs

Calder does not have high memory compute nodes. If you need more memory than is available on a single standard compute node, you can use a high memory node on Aire by requesting the relevant partition with `--partition=himem`.

High memory nodes can be useful for applications that require a large amount of memory but do not benefit from, or cannot use, MPI parallelism across multiple nodes. Keeping the application on a single node may also avoid the performance overhead associated with inter node communication.

Here, we request a high memory node to run a threaded application using OpenMP. Note that the `--mem` option specifies the amount of memory requested *per node*.

```bash
#!/bin/bash
#SBATCH --job-name=large_memory_job
#SBATCH --time=04:00:00
#SBATCH --partition=himem    # Request high-memory node
#SBATCH --mem=1024G          # Request 1024GB memory
#SBATCH --nodes=1
#SBATCH --ntasks=1           # Number of tasks for OpenMP
#SBATCH --cpus-per-task=32   # Number of CPU cores per task

# Tell OpenMP how many resources it has been given
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

# Run the job
./example.bin
```

## Job dependencies

It is possible to submit a job which will only start once another job has reached a particular state. This is useful when multiple stages of a workflow must run in sequence. Dependency conditions include `afterok`, which starts the dependent job only if the initial job completes successfully, and `afterany`, which starts the dependent job once the initial job finishes, regardless of outcome (including failure or timeout).

Submitting an initial job using `sbatch job1.sh` returns a job ID number `<JOBID>`. A second job submitted with a dependency, `job2.sh`, will remain in the queue until the dependency condition is satisfied.

```bash
$ sbatch --dependency=afterok:<JOBID> job2.sh
```

A dependency on more than one job can be specified by separating job IDs with a colon. The dependent job will start only when all specified jobs satisfy the condition.

```bash
$ sbatch --dependency=afterok:<JOBID_1>:<JOBID_2> job3.sh
```

A dependent job can also be submitted from within a job script. In this case, `$SLURM_JOB_ID` is set automatically to the job ID of the running job.

```bash
#!/bin/bash
#SBATCH --job-name=initial_job
#SBATCH --time=01:00:00
#SBATCH --mem=1G
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1

# Run the job
./example.bin

# Submit a dependent job
sbatch --dependency=afterany:$SLURM_JOB_ID job2.sh
```

(submission-options)=

## List of Slurm submission options

Below is a list of currently available options for submissions to Slurm. This list is likely to grow as we add and update functionality.

| Option | Description | Default |
| :---- | :--- | :---- |
| `--time=d-hr:min:s`<br>`-t d-hr:min:s` | The requested wall clock time (amount of real time needed by the job). Failure to include this parameter will result in an error message. | Required |
| `--mem-per-cpu=<size>[units]` | Amount of memory requested **per CPU**. Units are specified with K, M, G, i.e., request 1GB with `--mem=1G`. | 1GB |
| `--mem=<size>[units]` | Total amount of memory allocated **per node**. If the job spans multiple nodes, this value is applied to each node individually. For example, if `--mem=32G` is specified and the job uses 2 nodes, each node will be allocated 32GB of memory, resulting in a total of 64GB across the job. | |
| `--cpus-per-task=<number>`<br>`-c <number>` | Request a number of CPU cores per task, for threaded applications. By default, Slurm will allocate a single processor per task. On Calder's standard compute partition, requests are rounded up to a minimum of 8 cores. | 1 (8 on Calder's standard partition) |
| `--nodes=<number>`<br>`-N <number>` | Number of nodes requested (for MPI jobs) | 1 |
| `--ntasks=<number>`<br>`-n <number>` | Specifies the total number of tasks (processes) to be launched for the job, typically for MPI-based applications. Each task usually corresponds to an MPI process. For example, `--ntasks=8` will launch 8 processes across the allocated resources. | 1 |
| `--ntasks-per-node=<number>` | Requests a number of tasks to be executed on each node. If used with `--ntasks`, `--ntasks` will take precedence, and `--ntasks-per-node` will be treated as a *maximum* count of tasks per node. | Must be specified for MPI jobs |
| `--job-name=<jobname>`<br>`-J <jobname>` | Specify a job name. | Name of the submission script. |
| `--output=<filename>`<br>`-o <filename>` | Write standard output to the specified file. Use `%j` to include the job ID, for example `--output=task_%j.out`. | `slurm-<jobid>.out` |
| `--error=<filename>`<br>`-e <filename>` | Write standard error to the specified file. Use `%j` to include the job ID, for example `--error=task_%j.err`. | Standard output |
| `--partition=<name>`<br>`-p <name>` | Requests use specific partitions, for example, `gpu` and `himem`. | |
| `--gres=gpu:<type>:<number>` | Requests the use of GPU nodes, specifying the GPU type and number of requested GPUs. This needs to be used with the GPU partition (`gpu` on Aire, `calder-gpu-hopper` on Calder). On Aire, use type `nvidia_l40` (each GPU node has 3 GPUs). On Calder, use type `nvidia_h200` (each GPU node has 8 GPUs, with a maximum of 16 GPUs per job/user and 2 nodes per job). | |
| `--array=<start>-<stop>`<br>`-a <start>-<stop>` | Produce an array of sub-tasks (loop) from `<start>` to `<stop>`, using the `$SLURM_ARRAY_TASK_ID` variable to identify the individual sub-tasks.  | |
| `--mail-type=BEGIN,END` | Request an email to be sent at the beginning and end of a job to the owner. | |
| `--mail-user=username@leeds.ac.uk` | Specify user email for `--mail-type` option. | User's email address |
| `--help`<br>`-h`| Display a list of help information, then exit. | |
