---
---

# Lesson 14: Filesystem Inspection and Troubleshooting

## 1. Introduction

Linux provides several commands that allow system administrators and cybersecurity professionals to inspect filesystems, monitor disk usage, identify storage problems, and investigate suspicious files.

Filesystem inspection means examining how storage is organised and how much space is being used.

Troubleshooting means identifying and resolving problems affecting files, directories, partitions, or storage devices.

In ethical hacking, these skills are useful when:
- Investigating suspicious files.
- Checking available disk space.
- Identifying unusually large directories.
- Inspecting mounted filesystems.
- Investigating storage-related system errors.
- Examining files during security investigations.

## 2. Checking Disk Space with `df`

The `df` command displays information about mounted filesystems and their available space.

Basic command:

```bash
df
```

Human-readable output:

```bash
df -h
```

The `-h` option displays sizes in readable units such as KB, MB, and GB.

Example:

```bash
df -h
```

Important columns:

| Column | Meaning |
|---|---|
| Filesystem | Storage device or filesystem |
| Size | Total filesystem size |
| Used | Space already occupied |
| Avail | Available space |
| Use% | Percentage of space used |
| Mounted on | Where the filesystem is accessible |

**Why it matters:** A filesystem that reaches 100% usage can cause applications to fail, prevent logs from being written, and affect system stability.

## 3. Checking Directory Sizes with `du`

The `du` command estimates how much disk space files and directories occupy.

Check the size of your home directory:

```bash
du -sh ~
```

Check the size of a particular directory:

```bash
du -sh /var/log
```

Check the sizes of directories inside your current location:

```bash
du -h --max-depth=1 .
```

Options:

- `-s`: Displays a summary.
- `-h`: Uses human-readable units.
- `--max-depth=1`: Limits directory depth to one level.

## 4. Identifying Large Files

Large files can consume significant storage space.

Find files larger than 100 MB in your home directory:

```bash
find ~ -type f -size +100M
```

Find files larger than 500 MB:

```bash
find ~ -type f -size +500M
```

Understand the options:

- `find`: Searches for files and directories.
- `~`: Starts the search in your home directory.
- `-type f`: Searches for regular files.
- `-size +100M`: Selects files larger than 100 MB.

**Security relevance:** Investigators may use file-size searches to locate unusually large archives, backups, or files that need further examination. File size alone does not prove malicious activity.

## 5. Inspecting Mounted Filesystems

The `findmnt` command displays mounted filesystems in a readable tree.

```bash
findmnt
```

Display filesystem types:

```bash
findmnt -t ext4
```

Display the filesystem containing your current directory:

```bash
findmnt -T .
```

You can also inspect mounted filesystems using:

```bash
mount
```

The `findmnt` command is often easier to read when investigating mount points.

## 6. Checking Storage Devices

The `lsblk` command displays block devices such as disks and partitions.

```bash
lsblk
```

Display filesystem information:

```bash
lsblk -f
```

Example output might contain:

```text
NAME        FSTYPE   SIZE MOUNTPOINTS
sda                  50G
├─sda1      ext4     49G /
└─sda2      swap      1G [SWAP]
```

The actual output on your Kali VM will differ.

Important columns:

- NAME: Device or partition name.
- FSTYPE: Filesystem type.
- SIZE: Device or partition size.
- MOUNTPOINTS: Where it is mounted.

## 7. Checking File and Directory Permissions

Incorrect permissions can prevent users and applications from accessing files.

Inspect a file:

```bash
ls -l /etc/passwd
```

Inspect a directory:

```bash
ls -ld /etc
```

Display detailed file metadata:

```bash
stat /etc/passwd
```

The `stat` command can show:
- File size.
- File type.
- Permissions.
- Owner and group.
- Access time.
- Modification time.
- Metadata change time.

Remember:

- Access time (atime): When file contents were last accessed, subject to filesystem and mount options.
- Modification time (mtime): When file contents were last modified.
- Change time (ctime): When file metadata last changed.

## 8. Checking Filesystem Information

The `file` command helps identify the type of a file.

```bash
file /etc/passwd
```

Inspect another file:

```bash
file /bin/ls
```

The `file` command examines file content and metadata patterns to identify its likely type.

It does not prove that a file is safe or authentic.

## 9. Checking System Logs for Storage Problems

Linux logs can provide useful information when investigating filesystem problems.

Inspect recent system messages:

```bash
sudo journalctl -p warning -n 30
```

View recent kernel messages:

```bash
sudo dmesg | tail -30
```

Search kernel messages for storage-related terms:

```bash
sudo dmesg | grep -iE 'disk|filesystem|I/O error|ext4'
```

These commands may reveal warnings or errors involving storage devices, filesystem drivers, or disk operations.

Do not assume every warning indicates a serious problem. Investigate the surrounding messages and system context.

## 10. Common Filesystem Problems

### Problem 1: No Space Left on Device

Possible causes:
- Large files consuming storage.
- Excessive log files.
- Too many temporary files.
- Applications generating unnecessary data.

Useful commands:

```bash
df -h
du -sh ~
```

### Problem 2: Permission Denied

Possible causes:
- Insufficient permissions.
- Incorrect file ownership.
- Restricted directory access.

Useful commands:

```bash
ls -l filename
stat filename
whoami
```

Do not solve every permission problem by applying `chmod 777`. That gives everyone broad access and can introduce security risks.

### Problem 3: Filesystem Not Mounted

Possible causes:
- Device not connected.
- Incorrect mount configuration.
- Missing mount point.
- Filesystem or device errors.

Useful commands:

```bash
lsblk -f
findmnt
```

### Problem 4: Input/Output Errors

Possible causes:
- Storage device problems.
- Filesystem errors.
- Virtual disk problems.
- Connection or hardware issues.

Useful command:

```bash
sudo dmesg | tail -50
```

## 11. Understanding `fsck`

`fsck` stands for Filesystem Check.

It is used to check and, where appropriate, repair filesystem inconsistencies.

Example syntax:

```bash
sudo fsck /dev/sdXN
```

Here, `/dev/sdXN` is only a placeholder for a correctly identified partition.

**Important safety rules:**

1. Identify the correct filesystem before doing anything.
2. Never guess a device name.
3. Do not run filesystem repairs on a mounted filesystem.
4. Use a suitable recovery environment when necessary.
5. Back up important data before attempting repairs.
6. Understand the repair options before approving changes.

For this lesson, learn what `fsck` does, but do not run it on your Kali system.

## 12. Practical Lab: Filesystem Inspection

Perform these exercises inside your Kali Linux virtual machine.

### Exercise 1: Inspect disk usage

```bash
df -h
```

Identify:
- The filesystem containing `/`.
- Its total size.
- Its used space.
- Its available space.

### Exercise 2: Inspect directory sizes

```bash
du -sh ~
du -sh /var/log
du -h --max-depth=1 ~
```

Compare the sizes.

### Exercise 3: Find large files

```bash
find ~ -type f -size +10M
```

This searches your home directory for files larger than 10 MB.

### Exercise 4: Inspect mounted filesystems

```bash
findmnt
findmnt -T /
```

Identify the filesystem containing your root directory.

### Exercise 5: Inspect storage devices

```bash
lsblk
lsblk -f
```

Identify your virtual disk and its partitions.

### Exercise 6: Inspect file metadata

```bash
stat /etc/passwd
file /etc/passwd
ls -l /etc/passwd
```

Compare the information provided by each command.

### Exercise 7: Inspect system messages

```bash
sudo dmesg | tail -20
```

Look for messages related to storage or filesystems.

## 13. Knowledge Check

Answer these questions in your notebook:

1. What is filesystem inspection?
2. What is the purpose of the `df -h` command?
3. What is the difference between `df` and `du`?
4. Which command helps identify files larger than 100 MB?
5. What is the purpose of `findmnt`?
6. What information does `lsblk -f` display?
7. What is the difference between file modification time and metadata change time?
8. What does the `fsck` command do?
9. Why should filesystem repair tools not be run on a mounted filesystem?
10. Mention three common filesystem problems.

## 14. Exam-Ready Summary

Filesystem inspection is the process of examining Linux storage devices, mounted filesystems, file permissions, and disk usage. Linux provides commands such as `df`, `du`, `find`, `findmnt`, `lsblk`, `stat`, and `file` to investigate filesystem conditions. These commands help administrators and cybersecurity professionals identify storage problems, inspect suspicious files, and maintain system reliability.

## 15. Lesson Conclusion

You have learned how to inspect Linux filesystems, monitor storage usage, locate large files, examine permissions, inspect system logs, and understand basic filesystem troubleshooting.

These skills are important for Linux system administration, digital forensics, and ethical hacking.

**Next Lesson:** Module 03 Assessment.