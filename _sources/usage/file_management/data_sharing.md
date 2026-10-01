# Data Sharing

It is important that you understand how the Linux system for controlling access to files works, and if you need to share files with another user, how to do this safely without opening up access to people you don't want to. This is one of the areas people most commonly run into problems with in Linux, so we have a guide to it here.

```{note}
Home directories are shared between Aire and Calder, so any permissions or access controls you set on your home or scratch directory apply consistently on both systems.
```

## Understanding permissions

There are 3 permissions: read (`r`), write (`w`) and execute (`x`). There are 3 levels to whom these permissions apply: owner, group, and all (everyone else). Permissions, group, and ownership can be changed with the `chmod`, `chgrp`, and `chown` commands respectively.

```{admonition} Historical note
:class: note
The default permissions on new files differed between our previous ARC3 and ARC4 systems: on ARC3, group and all had read and execute permissions on new files by default, whereas on ARC4 group and all had no permissions at all. If you're unsure which default currently applies to your files, check with `ls -l` or `getfacl` (see below) rather than assuming.
```

Users belong to groups; the `groups` command can be used to check which groups you belong to, and file/directory permissions can be organised around these groups. Only system administrators can create new groups or add users to them.

## Sharing files with specific users

Rather than changing the permissions available to an entire group, you can use access control lists (ACLs) to share files and directories with specific individual users:

- `setfacl` sets access control entries, and can be used to grant another user access to a specific file or directory.
- `getfacl` reports the current access control entries for a file or directory.
- `setfacl --recursive` applies a change through all subdirectories, and can also be used to set default permissions that are automatically applied to any new files and directories created afterwards.

To let another user read and execute (but not write to) files in your scratch directory:

```bash
cd /mnt/scratch/<username>
setfacl -m u:<target-username>:r-x .
```

Replace `<target-username>` with the username of the person you want to share the directory with.

To check what access controls are currently set on a directory:

```bash
cd /mnt/scratch/<username>
getfacl .
```

If you have an existing directory with subdirectories and want to recursively grant read, write, and execute permissions throughout:

```bash
cd /mnt/scratch/<username>
setfacl --recursive -m u:<target-username>:rwx .
```

The command above only applies to files and directories that already exist. To also apply these permissions by default to any new files and directories created afterwards, set a default ACL at the same time:

```bash
cd /mnt/scratch/<username>
setfacl --recursive -m u:<target-username>:rwx,d:u:<target-username>:rwx .
```

For more detailed examples and explanations of Linux file permissions, you can refer to this [Linux File Permissions Guide](https://www.redhat.com/en/blog/linux-file-permissions-explained).
