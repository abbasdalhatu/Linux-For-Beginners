
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 08: LINKS AND MOUNT POINTS

### 1. Introduction

Linux uses links to allow multiple directory entries to reference files.

There are two main types:

1. Hard links
2. Symbolic links (soft links)

Linux also uses mount points to make filesystems accessible within its directory hierarchy.

Understanding these concepts is important for:
- Linux system administration.
- Storage management.
- File recovery.
- Digital forensics.
- Filesystem security investigations.

### 2. Learning Objectives

By the end of this lab, you should be able to:

- Explain hard links and symbolic links.
- Create hard links using `ln`.
- Create symbolic links using `ln -s`.
- Identify links using `ls -l` and `stat`.
- Understand inode relationships.
- Identify broken symbolic links.
- Inspect mounted filesystems.
- Understand mount points and filesystem usage.
- Generate a filesystem investigation report.

### 3. Create Your Lab Workspace

```bash
mkdir -p ~/practical-labs/module02/lab08
cd ~/practical-labs/module02/lab08
```

Create a sample file:

```bash
echo "Linux filesystem investigation practice" > original.txt
```

Inspect:

```bash
ls -l
cat original.txt
```

### 4. Understanding Inodes

An inode is a filesystem data structure that stores metadata about a file.

It commonly contains:
- File type.
- Permissions.
- Owner information.
- Group information.
- File size.
- Timestamps.
- Pointers or references to file data.

A directory stores names and associates them with inode numbers.

Inspect the inode:

```bash
ls -i original.txt
```

Another method:

```bash
stat original.txt
```

Look for the inode number in the output.

### 5. Understanding Hard Links

A hard link is another directory entry that refers to the same inode as an existing file.

This means both filenames reference the same underlying file data.

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Inspect:

```bash
ls -li
```

You should notice that `original.txt` and `hardlink.txt` have the same inode number.

Check their contents:

```bash
cat original.txt
cat hardlink.txt
```

Both should display the same information.

### 6. Modify a Hard Link

Add information through the hard link:

```bash
echo "Additional information through hard link" >> hardlink.txt
```

Inspect the original:

```bash
cat original.txt
```

The new information should also appear.

Why?

Both filenames refer to the same inode and underlying file data.

### 7. Understand the Hard-Link Count

Inspect:

```bash
stat original.txt
```

Look for:

```text
Links: 2
```

The link count indicates how many hard directory entries refer to that inode.

Remove one filename:

```bash
rm hardlink.txt
```

Inspect:

```bash
stat original.txt
```

The link count should decrease.

The file data remains accessible through `original.txt`.

**Important limitations of hard links:**

- They normally cannot cross filesystem boundaries.
- Creating hard links to directories is generally restricted.
- They reference the same inode, not a separate copy of the data.

### 8. Understanding Symbolic Links

A symbolic link is a special file containing a pathname that points to another file or directory.

Create one:

```bash
ln -s original.txt symlink.txt
```

Inspect:

```bash
ls -l
```

Example:

```text
symlink.txt -> original.txt
```

Check:

```bash
cat symlink.txt
```

The command follows the link and displays the target file's contents.

### 9. Compare Inodes

Inspect both objects:

```bash
ls -li original.txt symlink.txt
```

You should observe different inode numbers.

Why?

A symbolic link is its own filesystem object. It stores a pathname that points to the target.

Inspect its metadata:

```bash
stat symlink.txt
```

Follow the symbolic link:

```bash
stat -L symlink.txt
```

The `-L` option tells `stat` to inspect the target rather than the symbolic link itself.

### 10. Create a Symbolic Link to a Directory

Create a directory:

```bash
mkdir project-data
```

Create a file inside:

```bash
echo "Project directory information" > project-data/info.txt
```

Create a symbolic link:

```bash
ln -s project-data project-link
```

Inspect:

```bash
ls -l
```

Access the target through the link:

```bash
cat project-link/info.txt
```

### 11. Understand Broken Symbolic Links

A symbolic link can become broken if its target is removed or moved.

Create a link:

```bash
ln -s original.txt broken-link.txt
```

Remove the target:

```bash
rm original.txt
```

Inspect:

```bash
ls -l
```

Try:

```bash
cat broken-link.txt
```

You should receive an error because the target no longer exists.

Check whether a symbolic link is broken:

```bash
test -L broken-link.txt && echo "Symbolic link exists"
test -e broken-link.txt || echo "Target is missing"
```

Explanation:

- `-L` checks whether the path is a symbolic link.
- `-e` checks whether the target exists.

Restore the original file:

```bash
echo "Restored practice file" > original.txt
```

### 12. Find Symbolic Links

List symbolic links in your current workspace:

```bash
find . -type l -ls
```

Find broken symbolic links:

```bash
find . -xtype l -print
```

The `-xtype l` test identifies symbolic links whose targets cannot be resolved.

### 13. Understanding Mount Points

A mount point is a directory where a filesystem becomes accessible.

Linux does not assign drive letters like Windows does.

Instead, filesystems are attached to directories within one unified directory hierarchy.

Examples include:

- `/` – Root filesystem.
- `/home` – May be a separate filesystem.
- `/boot` – May be mounted separately.
- `/mnt` – Common temporary mount location.
- `/media` – Common location for removable media.

### 14. Inspect Mounted Filesystems

Display mounted filesystems:

```bash
findmnt
```

Display filesystem types:

```bash
findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS
```

Inspect the root filesystem:

```bash
findmnt /
```

Inspect your home directory:

```bash
findmnt ~
```

Display disk usage:

```bash
df -h
```

Display filesystem types:

```bash
df -Th
```

### 15. Inspect Mount-Point Relationships

Use:

```bash
lsblk
```

For more detailed information:

```bash
lsblk -f
```

This displays information such as:

- Device names.
- Filesystem types.
- UUIDs.
- Labels.
- Mount points.

Inspect mounted filesystem configuration:

```bash
cat /proc/mounts
```

Inspect persistent mount configuration:

```bash
cat /etc/fstab
```

Do not edit `/etc/fstab` during this exercise.

### 16. Understand Mount Options

Mount options can influence filesystem behavior and security.

Examples include:

| Option | Purpose |
|---|---|
| `ro` | Read-only mount |
| `rw` | Read-write mount |
| `noexec` | Prevent normal execution of binaries from the mount |
| `nosuid` | Ignore SUID and SGID privilege effects |
| `nodev` | Do not interpret device files on the mount |

Inspect the options currently used:

```bash
findmnt -o TARGET,OPTIONS
```

These options are important when administrators configure removable storage or isolated data partitions.

### 17. Security Investigation

Links can affect how files are accessed.

For example, an application may access a symbolic link without realizing that the target points somewhere unexpected.

During an investigation, examine:

1. The link itself.
2. Its target.
3. Its inode.
4. Ownership and permissions.
5. Whether the target exists.
6. Whether the target is within the expected directory.

Inspect links:

```bash
find . -type l -exec ls -l {} \;
```

Inspect their targets:

```bash
find . -type l -exec readlink -f {} \;
```

Remember that `readlink -f` attempts to resolve the complete path. Broken or inaccessible targets may not resolve.

### 18. Create a Filesystem Investigation Report

Run:

```bash
cd ~/practical-labs/module02/lab08
```

Generate your report:

```bash
{
    echo "LINUX LINKS AND MOUNT POINTS REPORT"
    echo "==================================="
    echo "Date: $(date)"
    echo "User: $(whoami)"
    echo
    echo "FILES AND INODES:"
    ls -li
    echo
    echo "SYMBOLIC LINKS:"
    find . -type l -ls
    echo
    echo "BROKEN SYMBOLIC LINKS:"
    find . -xtype l -print
    echo
    echo "FILESYSTEM MOUNTS:"
    findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS
    echo
    echo "DISK USAGE:"
    df -h
    echo
    echo "BLOCK DEVICES:"
    lsblk -f
} > links-mounts-report.txt
```

Read:

```bash
cat links-mounts-report.txt
```

### 19. Lab Cleanup

Inspect the workspace:

```bash
ls -la
```

Remove the symbolic links created for this exercise:

```bash
rm -f symlink.txt broken-link.txt project-link
```

Remove the remaining practice objects:

```bash
rm -f original.txt hardlink.txt
rm -rf project-data
```

Do not remove files outside this dedicated lab directory.

Keep `links-mounts-report.txt`.

### 20. Knowledge Check

1. What is an inode?
2. What is a hard link?
3. What is a symbolic link?
4. Why do hard links normally share the same inode?
5. Why does a symbolic link have its own inode?
6. What happens when a symbolic link's target is deleted?
7. Which command creates a hard link?
8. Which command creates a symbolic link?
9. What is a mount point?
10. What does `findmnt` display?
11. What is the purpose of `noexec`?
12. Why should security investigators inspect symbolic-link targets?

### 21. Lab Completion Checklist

- [ ] Created the Lab 08 workspace.
- [ ] Inspected inode information.
- [ ] Created a hard link.
- [ ] Compared hard-link inode numbers.
- [ ] Examined the hard-link count.
- [ ] Created a symbolic link.
- [ ] Created a symbolic link to a directory.
- [ ] Investigated a broken symbolic link.
- [ ] Located symbolic links using `find`.
- [ ] Inspected mounted filesystems using `findmnt`.
- [ ] Inspected disk usage using `df`.
- [ ] Inspected block devices using `lsblk`.
- [ ] Generated `links-mounts-report.txt`.
- [ ] Cleaned up the practice objects.

### 22. Expected Workspace

```text
~/practical-labs/module02/lab08/
├── links-mounts-report.txt
├── original.txt
├── symlink.txt
├── broken-link.txt
├── project-data/
│   └── info.txt
└── project-link
```

Some listed practice objects may have been removed during cleanup. The audit report should remain.

**Lab 08 Status:** Complete when the practical exercises and checklist are finished.

**Next Lab:** Lab 09 – Filesystem Security Investigation.
