# Frequently Asked Questions

This page answers common questions from users during the launch of Calder. If your question isn't answered here, please see [How to get support](../support/start.md).

## Accounts and Access

**Do I need to request a separate account to use Calder?**

No. There is a single HPC user group covering both systems, so there is no separate application process for Calder. If you already have an Aire account, you will be able to access Calder once it is available, and new applicants will get access to both systems through the existing [HPC account request form](request_account.md).

**Will my home directory, Slurm account, and quotas carry over from Aire?**

Yes. Home directories are shared between Aire and Calder, and the same onboarding process that currently creates your home directory, Slurm association, and Lustre quotas for Aire will also apply to Calder — there is nothing extra you need to do.

## Storage and Quotas

**Does my storage quota work the same way on Calder?**

Not quite. On Aire, Lustre quotas are applied per user. On Calder, Lustre quotas will instead be applied per Slurm project. The full details are still being finalised and will be published on the [Filesystem Quotas](page:filesystem-quotas) page once confirmed.

## Getting Support

**I'm having trouble logging in — what should I include when reporting the issue?**

Please share the exact steps you took, ideally by copying the commands and output directly from your terminal, or with a screenshot if copying isn't possible. This helps us diagnose the problem much faster than a general description.

**My job failed or isn't behaving as expected — what should I include when reporting it?**

Please include the job ID, along with the job's output and error files, and your job submission script. This information is essential for us to investigate what happened.

:::{seealso}
See [How to get support](../support/start.md) for details on raising an issue or submitting a Research IT ticket.
:::

## Shell Configuration

**What's the difference between `.bashrc` and `.bash_profile`, and which one should I use?**

`.bashrc` is sourced for every new shell that is spawned and should be used only for defining shell functions — not for updating your environment or running commands.

`.bash_profile` is executed at the start of each new login session and at the start of each batch job. If you want to tailor your environment — for example, setting custom path or environment variable values — this is the file to use. Any settings you add here are automatically exported to all child processes in that session or job.

```{warning}
Do not include `module load` commands in `.bash_profile`. Doing so can cause unintentional conflicts with modules you load in your job scripts. If you contact the [Research Computing](https://arc.leeds.ac.uk/) team for support and your `.bash_profile` contains `module load` commands, you will be asked to remove them first.
```
