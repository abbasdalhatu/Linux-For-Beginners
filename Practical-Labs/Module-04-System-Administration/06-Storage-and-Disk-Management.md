
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 06: STORAGE AND DISK MANAGEMENT

### 1. Lab Overview

Storage management is the process of inspecting, organizing, monitoring, and maintaining the storage devices connected to a Linux system.

Linux uses filesystems to organize data on storage devices.

As a system administrator, you must understand how to:

- Identify storage devices.
- Inspect disk partitions.
- Monitor available disk space.
- Understand filesystems.
- Inspect mounted devices.
- Identify storage consumption.
- Understand mounting and unmounting.
- Investigate storage-related problems.

Storage management is also important in cybersecurity. A full disk can interrupt services, prevent logging, and affect system availability.

**Lab environment:** Kali Linux VM.

**Safety:** This lab is primarily read-only. Do not format disks, delete partitions, modify partition tables, or mount unfamiliar devices. Never experiment with disk-management commands on your Windows host.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain Linux storage concepts.
2. Identify available disks and partitions.
3. Inspect mounted filesystems.
4. Monitor disk space.
5. Investigate directory storage consumption.
6. Understand filesystem mounting.
7. Inspect disk usage and inode availability.
8. Generate a storage monitoring report.

### 3. Understanding Linux Storage

A storage device is hardware or virtual hardware used to store information.

Examples include:

- Hard Disk Drives (HDDs).
- Solid State Drives (SSDs).
- USB storage devices.
- Virtual machine disks.
- Network storage devices.

Linux represents storage devices using device files, commonly located in `/dev`.

Examples:

| Device | Description |
|---|---|
| `/dev/sda` | Common SATA/SCSI-style disk |
| `/dev/sda1` | First partition on that disk |
| `/dev/sda2` | Second partition |
| `/dev/nvme0n1` | NVMe storage device |
| `/dev/nvme0n1p1` | First partition on an NVMe device |
| `/dev/vda` | Common virtual disk device |

Device names depend on the hardware and virtual machine configuration.

### 4. Identify Storage Devices

Use `lsblk` to display block devices:

```bash
lsblk
```

Display additional information:

```bash
lsblk -f
```

Display device sizes and filesystem types:

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
```

Understand the columns:

| Column | Meaning |
|---|---|
| NAME | Device name |
| SIZE | Device capacity |
| TYPE | Disk, partition, or another device type |
| FSTYPE | Filesystem type |
| MOUNTPOINTS | Where the filesystem is mounted |

Example output:

```text
NAME       SIZE TYPE FSTYPE MOUNTPOINTS
sda         40G disk
├─sda1      39G part ext4   /
└─sda2       1G part swap   [SWAP]
```

Your actual output may be different.

### 5. Inspect Disk Partitions

A partition is a logical division of a storage device.

Inspect partition information:

```bash
sudo fdisk -l
```

This command displays information about available disks and their partitions.

You can also inspect block-device details:

```bash
sudo blkid
```

`blkid` displays information such as:

- Device identifiers.
- Filesystem types.
- UUIDs.
- Filesystem labels.

A UUID is a unique identifier used to identify a filesystem.

Inspect a particular device only if it exists:

```bash
sudo blkid /dev/sda1
```

Replace the device name with one actually shown by `lsblk`.

### 6. Understand Linux Filesystems

A filesystem determines how information is stored and organized on a storage device.

Common Linux filesystems include:

| Filesystem | Description |
|---|---|
| ext4 | Common Linux journaling filesystem |
| XFS | High-performance journaling filesystem |
| Btrfs | Copy-on-write filesystem with advanced features |
| FAT32 | Older widely compatible filesystem |
| exFAT | Common removable-storage filesystem |
| NTFS | Common Windows filesystem |

Inspect filesystem types:

```bash
lsblk -f
```

Display mounted filesystem types:

```bash
df -T
```

### 7. Monitor Available Disk Space

The `df` command displays filesystem disk usage.

Run:

```bash
df -h
```

The `-h` option displays sizes in human-readable units.

Example:

```text
Filesystem      Size  Used  Avail  Use%
/dev/sda1        40G   18G    20G   48%
```

Understand the columns:

| Column | Meaning |
|---|---|
| Filesystem | Mounted storage device |
| Size | Total filesystem size |
| Used | Space already occupied |
| Avail | Available space |
| Use% | Percentage of space used |

Inspect the root filesystem:

```bash
df -h /
```

Inspect filesystem types and space:

```bash
df -Th
```

### 8. Investigate Directory Storage Consumption

The `du` command helps identify how much storage directories and files consume.

Check the size of your home directory:

```bash
du -sh ~
```

Inspect the size of your practical-labs directory:

```bash
du -sh ~/practical-labs
```

Display sizes of individual directories:

```bash
du -h --max-depth=1 ~
```

Display the largest directories in your home directory:

```bash
du -h --max-depth=1 ~ | sort -hr | head
```

Understand the options:

- `-h` – Human-readable output.
- `-s` – Summary.
- `--max-depth=1` – Inspect one directory level.
- `sort -hr` – Sort sizes from largest to smallest.
- `head` – Display the first results.

These commands are useful when investigating why storage is running low.

### 9. Understand Inodes

An inode is a filesystem data structure that stores metadata about a file.

It can contain information such as:

- File permissions.
- Ownership.
- File size.
- Timestamps.
- Data block references.

A filesystem can run out of available inodes even when it still has free storage capacity.

Inspect inode usage:

```bash
df -i
```

Understand the columns:

| Column | Meaning |
|---|---|
| Inodes | Total inode capacity |
| IUsed | Inodes currently used |
| IFree | Available inodes |
| IUse% | Percentage of inodes used |

This is especially important on systems containing many small files.

### 10. Understand Mounted Filesystems

A mount point is a directory through which a filesystem becomes accessible.

For example, a separate storage device might be mounted at:

```text
/mnt/storage
```

Display currently mounted filesystems:

```bash
findmnt
```

Inspect the root filesystem:

```bash
findmnt /
```

Display mount information:

```bash
mount
```

Display filesystem usage:

```bash
df -h
```

These commands help administrators understand which storage devices are currently accessible.

### 11. Understand Mounting and Unmounting

Linux uses the `mount` command to attach a filesystem to a directory.

General syntax:

```bash
sudo mount DEVICE MOUNT_POINT
```

The `umount` command detaches a mounted filesystem.

General syntax:

```bash
sudo umount MOUNT_POINT
```

For example, a removable storage device might be mounted at `/mnt/usb`.

**Important:** Do not execute mounting or unmounting commands against your system disk or unfamiliar devices during this lab.

Before removing a storage device, make sure it is safely unmounted and that no processes are using it.

### 12. Understand /etc/fstab

The `/etc/fstab` file contains filesystem mount configuration.

Inspect it:

```bash
cat /etc/fstab
```

Typical entries can contain:

1. Filesystem identifier.
2. Mount point.
3. Filesystem type.
4. Mount options.
5. Dump setting.
6. Filesystem check order.

Example:

```text
UUID=example-uuid / ext4 defaults 0 1
```

This is an illustrative example, not a configuration to copy.

Incorrect changes to `/etc/fstab` can prevent a Linux system from booting correctly.

For this lab, inspect the file only. Do not modify it.

### 13. Storage Security and Troubleshooting

Storage problems can affect system availability and security.

Common problems include:

- Disk space exhaustion.
- Inode exhaustion.
- Unexpectedly large log files.
- Unusual files consuming storage.
- Failed filesystem mounts.
- Read-only filesystem errors.

Inspect storage usage:

```bash
df -h
```

Inspect inode availability:

```bash
df -i
```

Inspect large directories in your home directory:

```bash
du -h --max-depth=1 ~ | sort -hr | head
```

Inspect mount configuration:

```bash
cat /etc/fstab
```

Inspect recent storage-related kernel messages:

```bash
sudo dmesg -T | tail -n 30
```

Look for messages relating to:

- I/O errors.
- Filesystem errors.
- Device failures.
- Read-only filesystem transitions.
- Mount failures.

Do not run filesystem repair commands on mounted system partitions as an experiment.

### 14. Create a Storage Monitoring Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module04/lab06
```

Create the script:

```bash
nano ~/practical-labs/module04/lab06/storage-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module04/lab06/storage-report.txt"

{
    echo "LINUX STORAGE MONITORING REPORT"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "BLOCK DEVICES"
    lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
    echo

    echo "FILESYSTEM DISK USAGE"
    df -h
    echo

    echo "FILESYSTEM TYPES"
    df -Th
    echo

    echo "INODE USAGE"
    df -i
    echo

    echo "HOME DIRECTORY SIZE"
    du -sh "$HOME"
    echo

    echo "LARGEST HOME DIRECTORY ITEMS"
    du -h --max-depth=1 "$HOME" 2>/dev/null | sort -hr | head -n 10
    echo

    echo "MOUNT INFORMATION"
    findmnt
    echo

    echo "ROOT FILESYSTEM"
    findmnt /

} > "$REPORT" 2>&1

echo "Report generated: $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x ~/practical-labs/module04/lab06/storage-report.sh
```

Execute it:

```bash
~/practical-labs/module04/lab06/storage-report.sh
```

Read the report:

```bash
cat ~/practical-labs/module04/lab06/storage-report.txt
```

### 15. Practical Exercises

**Exercise 1 – Identify Storage Devices**

Run:

```bash
lsblk
lsblk -f
```

Identify your primary disk, partitions, and filesystem types.

**Exercise 2 – Inspect Partitions**

Run:

```bash
sudo fdisk -l
sudo blkid
```

Record the available storage devices and their identifiers.

**Exercise 3 – Monitor Disk Space**

Run:

```bash
df -h
df -Th
```

Identify the filesystem with the highest usage percentage.

**Exercise 4 – Investigate Directory Sizes**

Run:

```bash
du -sh ~
du -sh ~/practical-labs
du -h --max-depth=1 ~ | sort -hr | head
```

Identify the largest directory in your home directory.

**Exercise 5 – Inspect Inodes**

Run:

```bash
df -i
```

Record the inode usage percentage of your root filesystem.

**Exercise 6 – Inspect Mount Points**

Run:

```bash
findmnt
findmnt /
cat /etc/fstab
```

Identify the mount point and filesystem used by your root directory.

**Exercise 7 – Storage Diagnostics**

Run:

```bash
sudo dmesg -T | tail -n 30
```

Inspect the latest kernel messages for storage-related warnings or errors.

**Exercise 8 – Generate the Report**

Execute your storage monitoring script and inspect the generated report.

### 16. Lab Documentation

Create:

```bash
nano ~/practical-labs/module04/lab06/lab06-report.txt
```

Record:

```text
MODULE 04 – LAB 06
STORAGE AND DISK MANAGEMENT

1. Primary storage device:
2. Number of partitions:
3. Primary filesystem type:
4. Total root filesystem capacity:
5. Used root filesystem space:
6. Available root filesystem space:
7. Root filesystem usage percentage:
8. Inode usage percentage:
9. Largest directory identified:
10. What a mount point means:
11. What I learned:
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 17. Knowledge Check

1. What is a storage device?
2. What is a partition?
3. What is the purpose of `lsblk`?
4. What does `df -h` display?
5. What is the difference between `df` and `du`?
6. What is an inode?
7. Can a filesystem run out of inodes while still having free disk space?
8. What is a mount point?
9. What information is stored in `/etc/fstab`?
10. Why should administrators monitor disk usage?
11. Why is it dangerous to modify partition tables without proper preparation?
12. How can a full disk affect cybersecurity monitoring?

### 18. Lab Completion Checklist

- [ ] Identified storage devices.
- [ ] Inspected disk partitions.
- [ ] Identified filesystem types.
- [ ] Checked available disk space.
- [ ] Investigated directory sizes.
- [ ] Inspected inode usage.
- [ ] Examined mounted filesystems.
- [ ] Inspected `/etc/fstab`.
- [ ] Reviewed storage-related kernel messages.
- [ ] Generated the storage monitoring report.
- [ ] Completed lab documentation.

**LAB 06 STATUS: READY FOR PRACTICAL COMPLETION**
