# Data Transfer

This page describes recommended ways to transfer data to and from Aire and Calder, including using command-line tools and Globus.

## General guidance

- Aire and Calder login nodes provide connections to the campus network, JANET and other universities. Standard tools such as `scp` and `rsync` are available for transferring files.
- You can transfer individual files or directories. For a large number of small files, it may be more efficient to combine them into an archive before transferring them.
- You can transfer data between HPC and other machines on the University network, including desktop computers, workstations, departmental servers and storage.

:::{important}
Before a job starts, make sure its input data is available in the appropriate HPC storage area. If you need to transfer results elsewhere after a job finishes, write them to scratch during the job and transfer them afterwards.
:::

:::{warning}
Do not transfer data to or from HPC directly from a running compute job unless your workflow specifically requires it. Transfers can keep compute resources occupied while waiting for network operations. Where possible, stage input data before the job starts and transfer output data after it finishes.
:::

## Using SCP and rsync

The command-line tools `scp` and `rsync` are available on the login nodes. On Linux, you can use them from a terminal. On Windows, MobaXterm may provide a more convenient interface; on macOS, you could use Cyberduck or ForkLift.

Some graphical SCP clients may prompt for authentication repeatedly. If you need help transferring files with these tools, see the University IT article [KB0018323: How to transfer data to and from HPC](https://leeds.service-now.com/it?id=kb_article_view&table=kb_knowledge&sys_kb_id=dfcc76a9fb3b16909eaffefbaeefdc09&searchTerm=KB0018323). You may need to sign in with your University account to view it.

## Using Globus

Globus is the recommended option for transferring large datasets and files reliably between supported storage locations. It provides a web interface for selecting source and destination collections and managing transfers.

For information about University storage options, see the Library's [Storing and handling data](https://library.leeds.ac.uk/info/14062/research-data-management/65/storing-and-handling-data/3) guidance. For help transferring data between Globus collections and University storage services, see [KB0017543: Data transfer between Globus Collections, OneDrive, Microsoft Teams and SharePoint sites](https://it.leeds.ac.uk/it?id=kb_article_view&sys_kb_id=a15bba21fb943a909eaffefbaeefdcb5&table=kb_knowledge&searchTerm=KB0017543).

### Available HPC collections

The centrally managed Globus collections **Aire and Calder home directories**, **Aire scratch filesystem** and **Calder scratch filesystem** are available for transferring files in users' home directories and scratch directories on Aire and Calder. To use it:

1. Sign in to the [Globus web app](https://app.globus.org/).
2. In the **Collections** panel on the left, search for **Aire and Calder home directories** or **Aire scratch filesystem** or **Calder scratch filesystem**. Turn off all filters, including Recent Tasks, if the collection does not appear in the search results.
3. Select the collections you want to use, then click **Open in File Manager**. When using each Globus collection for the first time, you are required to authenticate with your University of Leeds account to access this collection, follow the instruction in your browser.
4. In File Manager, it takes you to your home directory if you open the **Aire and Calder home directories** collection, which is shared between Aire and Calder. When selecting **Calder scratch filesystem**, double click the `users` directory, then open the directory matching your HPC username. When selecting **Aire scratch filesystem**, find the directory matching your HPC username.
5. Select the files or directories you want to transfer, then choose the destination collection and navigate to the appropriate destination directory.
6. Start the transfer and monitor its progress in Globus.

:::{admonition} Bookmarking the collection
:class: note

To make future transfers easier, you can bookmark Globus collection. Once bookmarked, you can access it directly from File Manager without having to search for it again.
:::

### Other Globus resources

- [KB0017543: Data transfer between Globus Collections, OneDrive, Microsoft Teams and SharePoint sites](https://it.leeds.ac.uk/it?id=kb_article_view&sys_kb_id=a15bba21fb943a909eaffefbaeefdcb5&table=kb_knowledge&searchTerm=KB0017543) explains how to transfer data between Globus and University storage services.
- [KB0015444: Globus File Transfer Guide – How to Log In, Transfer Files Securely & Request Storage Access](https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0015444) introduces Globus and links to the Globus Data Transfer Service request form for enabling Globus on supported University storage.
- [How to log into Globus](https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0015522) explains how to authorise Globus to use your University of Leeds account.
- [Research Data Storage Service Provision](https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0018026) describes the University's research data storage service, including Globus-enabled storage.

If you want to connect Globus to your OneDrive account, you may need to request approval. See [Requesting access to applications to connect to your data (M365 Enterprise Apps)](https://it.leeds.ac.uk/it?id=kb_article_view&sysparm_article=KB0018501).

For more information about the Globus service itself, see the [Globus file transfer guide](https://docs.globus.org/guides/tutorials/manage-files/transfer-files/).
