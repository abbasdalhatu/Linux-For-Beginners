---
---

# LESSON 11: PARTITIONS AND FILESYSTEMS

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

When you install an operating system, the storage device must be organized so that the operating system can store and retrieve information.

Two important concepts make this possible:

1. Partitions
2. Filesystems

A partition divides a storage device into logical sections.

A filesystem organizes data inside a partition or another storage area.

Understanding these concepts is important for Linux administration, system installation, digital forensics, and cybersecurity.

## 2. What Is a Partition?

A partition is a logical division of a physical or virtual storage device.

Imagine a 500 GB disk divided into three sections:

```text
500 GB STORAGE DEVICE
+----------------------------+
|                            |
|       Partition 1          |
|          200 GB            |
|                            |
+----------------------------+
|                            |
|       Partition 2          |
|          200 GB            |
|                            |
+----------------------------+
|                            |
|       Partition 3          |
|          100 GB            |
|                            |
+----------------------------+
```

Each partition can be used for a different purpose.

For example:

- One partition can contain the operating system.
- Another can store personal files.
- Another can contain swap space.

A partition is not the same thing as a filesystem. A partition is a storage division; a filesystem is a structure used to organize data.

## 3. Why Do We Need Partitions?

Partitions help organize storage.

Common reasons include:

### Operating System Organization

Linux can use separate partitions for system files and user data.

### Data Management

Separating data can make administration and maintenance easier.

### Multiple Operating Systems

A computer can have separate partitions for different operating systems.

### Recovery and Maintenance

Separate partitions can help isolate certain system data and simplify some recovery operations.

### Swap Space

Linux can use a dedicated partition for swap space, although a swap file is also commonly used.

## 4. Understanding Partition Naming

Linux uses device names to identify disks and their partitions.

For traditional disk naming:

```text
/dev/sda
/dev/sda1
/dev/sda2
/dev/sda3
```

Explanation:

- `/dev/sda` represents the complete disk.
- `/dev/sda1` represents its first partition.
- `/dev/sda2` represents its second partition.
- `/dev/sda3` represents its third partition.

For NVMe devices:

```text
/dev/nvme0n1
/dev/nvme0n1p1
/dev/nvme0n1p2
```

The `p` separates the device name from the partition number.

Your Kali virtual machine may use either naming convention.

## 5. What Is a Filesystem?

A filesystem is a method used by an operating system to organize, store, retrieve, and manage files.

It controls how information is represented and accessed on a storage device.

A filesystem helps Linux manage:

- Files
- Directories
- File permissions
- Ownership
- File metadata
- Storage allocation
- Free space

Without a filesystem, Linux cannot use an ordinary storage partition as a normal organized collection of files and directories.

## 6. Common Linux Filesystems

Linux supports several filesystem types.

### EXT4

EXT4 means Fourth Extended Filesystem.

It is one of the most commonly used Linux filesystems.

Features:

- Supports large files and filesystems.
- Supports journaling.
- Provides good general-purpose performance.
- Commonly used for Linux installations.

### EXT3

EXT3 is an older Linux filesystem.

It introduced journaling compared with EXT2.

It is less commonly selected for new installations.

### EXT2

EXT2 is an older Linux filesystem without the standard journaling feature of EXT3 and EXT4.

It is mainly encountered in older systems and some specialized uses.

### XFS

XFS is a high-performance journaling filesystem.

It is commonly used for systems that handle large files and large storage volumes.

### Btrfs

Btrfs is a modern Linux filesystem with advanced features.

Some of its features include:

- Snapshots
- Checksums
- Subvolumes
- Copy-on-write
- Data integrity features

### FAT32

FAT32 is widely supported by different operating systems and devices.

It is commonly used for removable storage.

However, an individual file on FAT32 cannot exceed 4 GiB minus 1 byte.

### exFAT

exFAT is commonly used for flash drives and external storage devices.

It supports large files and is compatible with many modern operating systems.

## 7. Understanding Swap Space

Swap space is storage space that Linux can use as an extension of physical memory.

When memory pressure becomes high, Linux may move less-active memory pages from RAM into swap.

Swap can be implemented as:

- A dedicated swap partition.
- A swap file.

Example:

```text
RAM: 16 GB
SWAP: 4 GB
```

This is only an illustration, not a recommendation for every system.

Swap is not as fast as RAM and should not be treated as a replacement for sufficient physical memory.

Inspect swap usage:

```bash
swapon --show
```

You can also run:

```bash
free -h
```

## 8. Understanding Partition Tables

A partition table stores information about how a disk is divided into partitions.

Two important partition table types are:

### MBR

MBR means Master Boot Record.

Characteristics:

- Older partitioning scheme.
- Commonly supports up to four primary partitions.
- Supports extended partitions and logical partitions.
- Has a practical disk-size limitation of approximately 2 TiB with traditional 512-byte sectors.

### GPT

GPT means GUID Partition Table.

Characteristics:

- Modern partitioning scheme.
- Supports many partitions, depending on the operating system.
- Supports very large disks.
- Stores redundant partition information.
- Includes checksums for partition table integrity.

GPT is commonly used on modern computers with UEFI firmware.

## 9. Understanding Journaling

Some filesystems use a feature called journaling.

A journal records certain filesystem changes before they are fully applied.

This helps the filesystem recover more reliably after unexpected events such as:

- Power failure
- System crashes
- Unexpected shutdowns

EXT3, EXT4, and XFS support journaling.

Journaling improves recovery behavior, but it does not replace backups.

## 10. Inspecting Filesystems in Linux

Linux provides several commands for inspecting partitions and filesystems.

### Command 1: `lsblk -f`

```bash
lsblk -f
```

Displays:

- Device names
- Filesystem types
- UUIDs
- Labels
- Mount points

### Command 2: `df -Th`

```bash
df -Th
```

Displays mounted filesystems, their types, sizes, and available space.

### Command 3: `blkid`

```bash
sudo blkid
```

Displays filesystem identifiers and types for recognized block devices.

### Command 4: `findmnt`

```bash
findmnt
```

Displays mounted filesystems and their mount relationships.

### Command 5: `fdisk -l`

```bash
sudo fdisk -l
```

Displays disk and partition information.

Use these commands for inspection only during this lesson.

## 11. Understanding Filesystem Integrity

A filesystem can become inconsistent due to:

- Sudden power loss
- Hardware failure
- Storage errors
- Interrupted write operations

Linux provides filesystem checking utilities such as:

```bash
fsck
```

Different filesystem types may have their own checking tools.

Important safety rule:

Do not run filesystem repair commands on a mounted, active filesystem. Repair procedures must be selected according to the filesystem type and device state.

For this lesson, do not run repair commands. We will focus on inspection.

## 12. Why Partitions and Filesystems Matter in Cybersecurity

Understanding partitions and filesystems is useful in:

### Digital Forensics

Investigators examine partitions and filesystems to locate evidence.

### Incident Response

Security professionals inspect storage structures when investigating compromised systems.

### Data Recovery

Filesystem knowledge helps identify possible recovery methods.

### System Hardening

Administrators can organize storage to support security and operational requirements.

### Malware Investigation

Analysts may inspect filesystem metadata, unusual files, and suspicious storage activity.

## 13. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: Display Partitions and Filesystems

```bash
lsblk -f
```

Record:

- Disk name
- Partition names
- Filesystem types
- UUIDs
- Mount points

### Exercise 2: Display Mounted Filesystem Types

```bash
df -Th
```

Identify the filesystem type mounted at `/`.

### Exercise 3: Inspect Filesystem Identifiers

```bash
sudo blkid
```

Observe the UUID and filesystem type of each listed device.

### Exercise 4: Inspect the Mount Structure

```bash
findmnt
```

Observe how filesystems are connected to the Linux directory hierarchy.

### Exercise 5: Inspect the Partition Table

```bash
sudo fdisk -l
```

Identify:

- Disk size
- Partition count
- Partition types
- Partition sizes

Do not modify any partition.

### Exercise 6: Inspect Swap Space

```bash
swapon --show
free -h
```

Determine whether your Kali VM uses a swap partition, a swap file, or no active swap.

### Exercise 7: Compare Filesystem Information

Run:

```bash
lsblk -f
df -Th
```

Compare the results.

Notice that `lsblk -f` can show filesystem information for block devices, while `df -Th` focuses on mounted filesystems and their usage.

## 14. Knowledge Check

1. What is a partition?
2. What is a filesystem?
3. Explain the difference between a partition and a filesystem.
4. What is EXT4?
5. Mention three Linux-supported filesystem types.
6. What is swap space?
7. What does GPT stand for?
8. What is the difference between MBR and GPT?
9. What is filesystem journaling?
10. Which command displays mounted filesystem types?
11. Why should filesystem repair commands not be run carelessly on mounted filesystems?
12. Why is filesystem knowledge important in digital forensics?

## 15. Lesson Summary

A partition divides a storage device into logical sections, while a filesystem organizes data within a storage area.

We studied:

- Partitions
- EXT2, EXT3, EXT4
- XFS and Btrfs
- FAT32 and exFAT
- Swap space
- MBR and GPT
- Filesystem journaling
- Filesystem inspection commands

Important commands:

```bash
lsblk -f
df -Th
sudo blkid
findmnt
sudo fdisk -l
swapon --show
free -h
```

**Next Lesson:** Mounting and Unmounting Filesystems.