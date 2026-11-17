---
---

# LESSON 10: LINUX STORAGE DEVICES

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

A storage device is hardware or virtual hardware used to store data.

Examples include:

- Hard Disk Drives (HDDs)
- Solid State Drives (SSDs)
- USB flash drives
- External hard drives
- Virtual machine disks
- Memory cards

Linux uses special device files to communicate with storage devices.

These device files are commonly located inside:

```text
/dev
```

For example:

```text
/dev/sda
/dev/sda1
/dev/nvme0n1
/dev/nvme0n1p1
```

The exact names depend on the storage hardware and Linux environment.

Understanding storage devices is important for Linux administration, digital forensics, and cybersecurity.

## 2. Understanding HDD and SSD

### Hard Disk Drive (HDD)

An HDD stores information using magnetic disks that rotate mechanically.

Characteristics:

- Contains moving mechanical parts.
- Usually cheaper per gigabyte.
- Generally slower than SSDs.
- Commonly used for large storage capacity.

### Solid State Drive (SSD)

An SSD stores information using flash memory.

Characteristics:

- Has no moving mechanical parts.
- Generally faster than HDDs.
- Provides faster boot and application loading.
- Commonly used in modern laptops.

Your Kali virtual machine may use a virtual disk file stored on your Windows computer's SSD.

The guest operating system sees a virtual storage device rather than directly managing your laptop's physical SSD.

## 3. Understanding `/dev`

Linux represents hardware devices using device files.

These files are commonly stored in:

```text
/dev
```

Examples:

| Device | Description |
|---|---|
| `/dev/sda` | A disk using the traditional SCSI-style naming scheme |
| `/dev/sda1` | First partition on that disk |
| `/dev/sdb` | Another disk |
| `/dev/nvme0n1` | An NVMe storage device |
| `/dev/nvme0n1p1` | First partition on that NVMe device |
| `/dev/sr0` | Common name for an optical drive |

These names are examples. Your Kali VM may use different device names.

## 4. Understanding Disk Naming

Linux uses different naming conventions for different storage device types.

### Traditional Disk Naming

Examples:

```text
/dev/sda
/dev/sdb
/dev/sdc
```

The letters distinguish disks.

Partitions may be represented as:

```text
/dev/sda1
/dev/sda2
/dev/sda3
```

For example:

- `/dev/sda` represents the entire disk.
- `/dev/sda1` represents its first partition.
- `/dev/sda2` represents its second partition.

### NVMe Disk Naming

NVMe devices use a different naming convention.

Example:

```text
/dev/nvme0n1
```

Breaking it down:

- `nvme` identifies the device type.
- `0` identifies the controller number.
- `n1` identifies the namespace.

Partitions are represented using `p` before the partition number:

```text
/dev/nvme0n1p1
/dev/nvme0n1p2
```

Do not assume that every Linux system uses `/dev/sda`.

## 5. The `lsblk` Command

The `lsblk` command displays information about block devices.

Run:

```bash
lsblk
```

Example output:

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sda      8:0    0   50G  0 disk
├─sda1   8:1    0   49G  0 part /
└─sda2   8:2    0    1G  0 part [SWAP]
```

Your output may be different.

Understanding the columns:

| Column | Meaning |
|---|---|
| NAME | Device name |
| MAJ:MIN | Major and minor device numbers |
| RM | Whether the device is removable |
| SIZE | Device or partition size |
| RO | Whether the device is read-only |
| TYPE | Device type |
| MOUNTPOINTS | Where the device is mounted |

The `disk` type represents a whole disk, while `part` represents a partition.

To display filesystem information as well:

```bash
lsblk -f
```

This can show filesystem types, labels, UUIDs, and mount points.

## 6. The `fdisk` Command

The `fdisk` utility can display and manage disk partition tables.

To inspect disks and partitions:

```bash
sudo fdisk -l
```

This displays information about available disks, including their sizes and partition layouts.

Important:

`fdisk` can modify partition tables. For this lesson, use it only with the listing option:

```bash
sudo fdisk -l
```

Do not create, delete, or modify partitions during this exercise.

## 7. The `blkid` Command

The `blkid` command displays information about block devices and their filesystem identifiers.

Run:

```bash
sudo blkid
```

Example output:

```text
/dev/sda1: UUID="example-uuid" TYPE="ext4"
/dev/sda2: UUID="another-uuid" TYPE="swap"
```

The actual UUIDs and filesystem types will differ.

Important terms:

- **UUID:** A unique identifier associated with a filesystem or storage device.
- **TYPE:** The filesystem type.
- **LABEL:** An optional human-readable name assigned to a filesystem.

UUIDs help Linux identify filesystems consistently.

## 8. Understanding Disk Capacity

Linux provides commands for checking storage capacity.

### `df`

The `df` command displays available and used space on mounted filesystems.

Run:

```bash
df -h
```

The `-h` option displays sizes in human-readable units such as GB and MB.

Example:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        49G   12G   35G  26% /
```

Your output will vary.

### `du`

The `du` command estimates how much storage files and directories use.

Example:

```bash
du -sh ~
```

This displays the total estimated disk usage of your home directory.

Difference:

- `df` checks filesystem space.
- `du` estimates space used by files and directories.

## 9. Understanding Virtual Disks in VMware

When Kali Linux runs inside VMware, it uses a virtual disk.

A virtual disk is a file or collection of files managed by the hypervisor.

For example, VMware may store a virtual disk using a `.vmdk` file.

The relationship looks like this:

```text
Physical SSD
     |
     v
Windows 11
     |
     v
VMware Workstation
     |
     v
Kali Virtual Machine
     |
     v
Virtual Disk
     |
     v
Linux Partitions and Filesystems
```

Kali sees its virtual disk as a storage device.

It does not normally need to know the exact physical SSD location where VMware stores that virtual disk.

**Important:** A virtual disk configured to allow growth may occupy less physical space initially than its maximum configured capacity. Always monitor free space on the Windows host.

## 10. Why Storage Knowledge Matters in Cybersecurity

Understanding storage devices is useful in several cybersecurity fields.

### Digital Forensics

Investigators examine storage devices to recover and analyze digital evidence.

### Incident Response

Security professionals inspect disks to identify suspicious files and possible evidence of compromise.

### System Administration

Administrators monitor disk capacity and identify storage problems.

### Penetration Testing Labs

Security students may use virtual disks to create isolated testing environments.

### Data Security

Understanding storage devices helps professionals protect sensitive information and avoid accidental data loss.

## 11. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: List Storage Devices

```bash
lsblk
```

Identify:

- Disk names.
- Disk sizes.
- Partitions.
- Mount points.

### Exercise 2: Display Filesystem Information

```bash
lsblk -f
```

Identify the filesystem types and UUIDs shown.

### Exercise 3: Inspect Disk Partitions

```bash
sudo fdisk -l
```

Identify the disk size and partition information.

Do not modify anything.

### Exercise 4: Display Block Device Identifiers

```bash
sudo blkid
```

Observe the filesystem types and UUIDs.

### Exercise 5: Check Available Storage

```bash
df -h
```

Identify the filesystem mounted at `/` and its available space.

### Exercise 6: Check Home Directory Storage Usage

```bash
du -sh ~
```

Record the approximate size of your home directory.

### Exercise 7: Inspect the `/dev` Directory

```bash
ls -lah /dev
```

Observe the different device entries.

### Exercise 8: Inspect NVMe or Traditional Disk Devices

Run:

```bash
ls /dev/sd*
```

Then:

```bash
ls /dev/nvme*
```

If either command reports that no matching file exists, that is not necessarily an error. Your VM may use a different naming scheme.

## 12. Important Safety Rules

Storage devices contain important operating system and user data.

During this lesson:

- Do not format any disk.
- Do not delete partitions.
- Do not run destructive `dd` commands.
- Do not experiment with `fdisk` modification commands.
- Do not mount or modify unknown storage devices.
- Always verify the device name before performing administrative storage operations.

A mistake involving the wrong disk can destroy important data.

## 13. Knowledge Check

1. What is a storage device?
2. What is the purpose of `/dev`?
3. What does `/dev/sda` represent?
4. What does `/dev/sda1` represent?
5. What is the difference between `/dev/sda` and `/dev/nvme0n1`?
6. Which command displays block devices?
7. Which command displays disk partition information?
8. What is a UUID?
9. What is the difference between `df` and `du`?
10. What is a virtual disk in VMware?

## 14. Lesson Summary

Linux uses device files to communicate with storage devices.

We learned about HDDs, SSDs, virtual disks, traditional disk naming, NVMe naming, and important storage inspection commands.

Commands covered:

```bash
lsblk
lsblk -f
sudo fdisk -l
sudo blkid
df -h
du -sh ~
```

These commands are useful for understanding storage devices and monitoring Linux systems.

**Next Lesson:** Partitions and Filesystems.