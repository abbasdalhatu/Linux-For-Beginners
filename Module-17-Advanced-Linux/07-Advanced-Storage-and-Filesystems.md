---
---


# MODULE 17 – ADVANCED LINUX
## LESSON 07: ADVANCED STORAGE AND FILESYSTEMS

---

## 1. Introduction to Linux Storage

Linux uses a structured storage system to organize, access, and manage data.

Storage administration involves understanding:

- Physical storage devices.
- Disk partitions.
- Filesystems.
- Mount points.
- Disk usage.
- Filesystem permissions.
- Storage monitoring.
- Disk health.
- Logical Volume Management (LVM).
- Swap space.

As a Linux administrator or cybersecurity professional, you must understand how storage works because system reliability, performance, and data security depend on it.

### Learning objectives

By the end of this lesson, you should be able to:

1. Identify storage devices.
2. Understand disk partitions.
3. Explain Linux filesystems.
4. Inspect mounted filesystems.
5. Monitor disk usage.
6. Understand mount points.
7. Explain filesystem metadata.
8. Understand LVM.
9. Understand swap space.
10. Perform safe storage inspections.

---

## 2. Understanding Storage Devices

Linux represents storage devices as files inside `/dev`.

Common storage devices include:

| Device | Description |
|---|---|
| `/dev/sda` | First SATA/SCSI-style disk |
| `/dev/sdb` | Second SATA/SCSI-style disk |
| `/dev/nvme0n1` | NVMe storage device |
| `/dev/vda` | Virtual disk |
| `/dev/loop0` | Loopback device |
| `/dev/sr0` | Optical drive |

Device names depend on the hardware and virtual environment.

### Inspect storage devices

```bash
lsblk
```

Display filesystem information:

```bash
lsblk -f
```

Display device information:

```bash
sudo fdisk -l
```

This command lists detected disks and partition tables.

### Important distinction

A storage device and a partition are not the same.

Example:

```text
/dev/sda
/dev/sda1
/dev/sda2
```

- `/dev/sda` represents the entire disk.
- `/dev/sda1` represents its first partition.
- `/dev/sda2` represents its second partition.

---

## 3. Understanding Disk Partitions

Partitioning divides a physical or virtual storage device into separate logical sections.

For example:

```text
Disk: /dev/sda
|
|-- Partition 1: /dev/sda1
|-- Partition 2: /dev/sda2
|-- Partition 3: /dev/sda3
```

Partitions can be used for:

- Operating system files.
- User data.
- Swap space.
- Recovery environments.
- Separate application data.

### Common partition table formats

| Format | Description |
|---|---|
| MBR | Master Boot Record |
| GPT | GUID Partition Table |

### MBR

MBR is an older partitioning scheme.

Traditional MBR supports:

- Four primary partition entries.
- Extended and logical partitions.
- A maximum disk size of approximately 2 TiB with conventional 512-byte sectors.

### GPT

GPT is a newer partitioning scheme.

Advantages include:

- Support for very large disks.
- More partition entries.
- Improved partition metadata.
- Redundant partition table information.

### Inspect partition tables

```bash
sudo fdisk -l
```

You can also inspect a specific device:

```bash
sudo fdisk -l /dev/sda
```

Replace `/dev/sda` with a device that actually exists on your system.

**Safety:** These are inspection commands. Do not create, delete, or resize partitions during this lesson.

---

## 4. Understanding Linux Filesystems

A filesystem determines how data is organized and stored on a storage device.

Different filesystems provide different capabilities.

### Common Linux filesystems

| Filesystem | Description |
|---|---|
| ext4 | Widely used Linux filesystem |
| ext3 | Older journaling filesystem |
| ext2 | Older filesystem without journaling |
| XFS | High-performance journaling filesystem |
| Btrfs | Copy-on-write filesystem |
| ZFS | Advanced storage and data-integrity platform |
| FAT32 | Widely compatible filesystem |
| exFAT | Common removable-storage filesystem |
| NTFS | Common Windows filesystem |

### A. ext4

The fourth extended filesystem is commonly used in Linux.

Features:

- Journaling.
- File permissions.
- Large filesystem support.
- Reliable general-purpose storage.

### B. XFS

XFS is designed for high-performance storage and large files.

It is commonly used in servers and enterprise environments.

### C. Btrfs

Btrfs provides advanced capabilities such as:

- Snapshots.
- Copy-on-write.
- Checksums.
- Subvolumes.
- Data compression.

### D. NTFS

NTFS is the standard filesystem used by modern Windows installations.

Linux can access NTFS volumes through supported drivers and filesystem tools.

### Inspect filesystem types

```bash
lsblk -f
```

You can also use:

```bash
df -T
```

---

## 5. Understanding Mount Points

Linux organizes storage through a single directory hierarchy.

A mount point is a directory where a filesystem becomes accessible.

For example:

```text
/
|
|-- home
|-- boot
|-- var
|-- mnt
|-- media
```

A separate storage device can be mounted under `/mnt` or `/media`.

### Display mounted filesystems

```bash
findmnt
```

Display the root filesystem:

```bash
findmnt /
```

Display mounted filesystem information:

```bash
mount
```

### Understanding mount output

Example:

```text
/dev/sda1 on / type ext4 (rw,relatime)
```

This tells you:

- `/dev/sda1`: Source device.
- `/`: Mount point.
- `ext4`: Filesystem type.
- `rw`: Read/write mount mode.
- `relatime`: Access-time update behaviour.

### Important mount options

| Option | Meaning |
|---|---|
| `ro` | Read-only |
| `rw` | Read/write |
| `noexec` | Prevent direct execution of programs |
| `nosuid` | Ignore set-user-ID and set-group-ID permissions |
| `nodev` | Do not interpret device files on that mount |

These options can improve security when applied appropriately.

---

## 6. Monitoring Disk Usage

Linux provides several commands for monitoring storage capacity.

### A. df

The `df` command displays filesystem space usage.

```bash
df -h
```

The `-h` option displays human-readable sizes.

Example output:

```text
Filesystem      Size  Used  Avail  Use%
/dev/sda1        50G   20G    28G   42%
```

### B. du

The `du` command estimates disk usage for files and directories.

```bash
du -sh ~
```

Display usage for directories inside your home directory:

```bash
du -h --max-depth=1 ~
```

### C. ncdu

`ncdu` provides an interactive disk-usage interface.

Check whether it is installed:

```bash
command -v ncdu
```

If it is not installed, you may install it:

```bash
sudo apt update
sudo apt install ncdu
```

Run it against your home directory:

```bash
ncdu ~
```

### Important difference

| Command | Purpose |
|---|---|
| `df` | Reports filesystem capacity and available space |
| `du` | Estimates space used by files and directories |
| `lsblk` | Displays storage devices and partitions |

---

## 7. Understanding Filesystem Inodes

An inode is a filesystem data structure that stores metadata about a file.

It contains information such as:

- File permissions.
- Ownership.
- File size.
- Timestamps.
- Links.
- Pointers to file data blocks.

An inode does not normally store the filename itself. Directory entries associate filenames with inode numbers.

### Display inode information

```bash
ls -i
```

Display inode usage:

```bash
df -i
```

### Example

```text
123456 report.txt
```

Here:

- `123456` is the inode number.
- `report.txt` is the filename.

### Why inodes matter

A filesystem can have free disk space but still be unable to create new files if it has exhausted its available inodes.

This is especially relevant when a system contains millions of small files.

---

## 8. Understanding Hard Links and Symbolic Links

Linux supports two important types of file links.

### A. Hard links

A hard link is another directory entry referring to the same inode.

Create a hard link:

```bash
ln original.txt hardlink.txt
```

Both names refer to the same underlying file data.

### B. Symbolic links

A symbolic link points to another file or directory through a pathname.

Create a symbolic link:

```bash
ln -s original.txt symlink.txt
```

### Differences

| Feature | Hard link | Symbolic link |
|---|---|---|
| References | Same inode | Pathname |
| Can cross filesystems | No | Yes |
| Can point to directories | Usually restricted | Yes |
| Can become broken | Not in the usual pathname sense | Yes |
| Inode | Same as target | Different inode |

### Inspect links

```bash
ls -li
```

Display a symbolic link's target:

```bash
readlink symlink.txt
```

---

## 9. Understanding Logical Volume Management (LVM)

Logical Volume Management provides a flexible method of managing storage.

Instead of relying entirely on fixed partitions, LVM allows administrators to create and resize logical volumes using available storage.

### LVM architecture

LVM has three major components.

**1. Physical Volume (PV)**

A disk or partition prepared for LVM.

**2. Volume Group (VG)**

A storage pool created from one or more physical volumes.

**3. Logical Volume (LV)**

A virtual storage volume created from a volume group.

### Architecture diagram

```text
Physical Storage
       |
       v
Physical Volume (PV)
       |
       v
Volume Group (VG)
       |
       v
Logical Volume (LV)
       |
       v
Filesystem
       |
       v
Mount Point
```

### Inspect LVM configuration

Check available LVM commands:

```bash
command -v pvs
command -v vgs
command -v lvs
```

If LVM tools are installed, inspect:

```bash
sudo pvs
```

```bash
sudo vgs
```

```bash
sudo lvs
```

These commands inspect existing LVM configuration.

### Why LVM matters

LVM is useful for:

- Flexible storage allocation.
- Expanding logical volumes.
- Managing multiple storage devices.
- Creating storage snapshots.
- Server storage administration.

Do not create or modify LVM volumes in this lesson.

---

## 10. Understanding Swap Space

Swap is storage space used by Linux as an extension of physical memory management.

When memory pressure increases, the kernel may move selected memory pages to swap.

Swap can be implemented as:

- A swap partition.
- A swap file.

### Inspect memory and swap

```bash
free -h
```

Display active swap:

```bash
swapon --show
```

Display memory information:

```bash
cat /proc/meminfo
```

### Example output

```text
Mem:     8.0Gi
Swap:    2.0Gi
```

The values above are illustrative, not expected values for your VM.

### Important concepts

- RAM is physical memory.
- Swap is storage-backed memory support.
- Swap is significantly slower than RAM.
- Swap does not replace adequate physical memory.

---

## 11. Disk Health and Storage Monitoring

Storage devices can experience errors or degradation.

Monitoring helps administrators identify problems before they cause data loss.

### A. Inspect kernel storage messages

```bash
sudo dmesg | grep -iE 'disk|nvme|sda|error|fail'
```

### B. Inspect block devices

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
```

### C. Inspect device health tools

Check whether SMART tools are installed:

```bash
command -v smartctl
```

If available, inspect detected devices:

```bash
sudo smartctl --scan
```

SMART information is hardware-dependent and may not be exposed by a virtual machine.

For VMware virtual disks, the guest operating system may not have direct access to the physical drive's complete health information.

### Storage warning signs

- Increasing I/O errors.
- Unexpected filesystem read-only behaviour.
- Frequent device resets.
- Unusual disk latency.
- Repeated filesystem errors.
- Unexpected storage disappearance.

Always back up important data before performing storage repairs.

---

## 12. Practical Lab: Storage Inspection

We will inspect your Kali VM without changing its partitions or filesystems.

### Step 1: Create a laboratory directory

```bash
mkdir -p ~/module17-lab/lesson07
cd ~/module17-lab/lesson07
```

### Step 2: Identify storage devices

```bash
lsblk
```

### Step 3: Inspect filesystem types

```bash
lsblk -f
```

### Step 4: Inspect partitions

```bash
sudo fdisk -l
```

### Step 5: Inspect mounted filesystems

```bash
findmnt
```

### Step 6: Inspect filesystem capacity

```bash
df -h
```

### Step 7: Inspect inode usage

```bash
df -i
```

### Step 8: Inspect home directory usage

```bash
du -sh ~
```

### Step 9: Inspect RAM and swap

```bash
free -h
```

### Step 10: Inspect active swap devices

```bash
swapon --show
```

### Step 11: Inspect available LVM information

```bash
sudo pvs
sudo vgs
sudo lvs
```

If the commands are unavailable or no LVM volumes exist, record that result. It does not mean your storage is broken.

---

## 13. Practical Project: Build a Storage Report

Create a Bash script that collects storage information.

### Step 1: Create the script

```bash
nano storage-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson07/storage-report.txt"

{
    echo "=================================="
    echo "        LINUX STORAGE REPORT"
    echo "=================================="

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "STORAGE DEVICES:"
    lsblk

    echo
    echo "FILESYSTEM INFORMATION:"
    lsblk -f

    echo
    echo "DISK CAPACITY:"
    df -h

    echo
    echo "INODE USAGE:"
    df -i

    echo
    echo "HOME DIRECTORY USAGE:"
    du -sh "$HOME"

    echo
    echo "MEMORY AND SWAP:"
    free -h

    echo
    echo "ACTIVE SWAP:"
    swapon --show

    echo
    echo "=================================="
    echo "          REPORT COMPLETE"
    echo "=================================="

} > "$REPORT"

echo "Storage report saved to: $REPORT"
```

### Step 2: Make it executable

```bash
chmod +x storage-report.sh
```

### Step 3: Run the script

```bash
./storage-report.sh
```

### Step 4: Read the report

```bash
cat ~/module17-lab/lesson07/storage-report.txt
```

### Step 5: Verify the report

```bash
ls -lh ~/module17-lab/lesson07/
```

Your report should contain storage devices, filesystem details, disk capacity, inode usage, memory, and swap information.

---

## 14. Storage Security Best Practices

Storage security is essential for protecting sensitive information.

### A. Apply least privilege

Only authorized users should have access to sensitive files.

### B. Encrypt sensitive storage

Encryption helps protect data if a storage device is lost or stolen.

Linux supports technologies such as LUKS for block-device encryption.

### C. Maintain backups

Use regular backups and periodically test restoration.

### D. Monitor storage capacity

Low disk space can cause service failures and prevent applications from writing data.

### E. Monitor filesystem errors

Investigate repeated storage errors instead of ignoring them.

### F. Secure removable storage

Do not automatically trust unknown USB storage devices.

### G. Use appropriate mount options

Options such as `noexec`, `nosuid`, and `nodev` can reduce certain risks when applied to suitable filesystems.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is a Linux storage device?
2. Explain the difference between a disk and a partition.
3. What is the difference between MBR and GPT?
4. What is a filesystem?
5. Explain the purpose of ext4.
6. What is a mount point?
7. What is the difference between `df` and `du`?
8. What is an inode?
9. Explain hard links and symbolic links.
10. What are the three major components of LVM?
11. What is swap space?
12. What does `lsblk -f` display?
13. Why can a filesystem run out of inodes while still having free disk space?
14. Explain two Linux storage security practices.

---

## 16. Important Commands Summary

| Command | Purpose |
|---|---|
| `lsblk` | List block devices |
| `lsblk -f` | Display filesystem details |
| `fdisk -l` | Inspect partition tables |
| `findmnt` | Display mounted filesystems |
| `df -h` | Display disk capacity |
| `df -i` | Display inode usage |
| `du -sh DIRECTORY` | Estimate directory usage |
| `free -h` | Display memory and swap |
| `swapon --show` | Display active swap |
| `mount` | Display mounted filesystems |
| `ls -i` | Display inode numbers |
| `readlink FILE` | Display symbolic-link target |
| `pvs` | Display LVM physical volumes |
| `vgs` | Display LVM volume groups |
| `lvs` | Display LVM logical volumes |
| `dmesg` | Display kernel messages |
| `smartctl` | Inspect supported storage health information |

---

## 17. Lesson Summary

In this lesson, you studied advanced Linux storage and filesystem administration.

You learned:

- Storage devices.
- Disk partitions.
- MBR and GPT.
- Linux filesystems.
- Mount points.
- Disk capacity monitoring.
- Inodes.
- Hard and symbolic links.
- Logical Volume Management.
- Swap space.
- Storage health monitoring.
- Storage security.

You also built a storage-reporting script.

### Completion checklist

- [ ] Understand Linux storage devices.
- [ ] Understand partitions.
- [ ] Understand filesystems.
- [ ] Inspect mounted filesystems.
- [ ] Monitor disk capacity.
- [ ] Understand inode usage.
- [ ] Understand hard and symbolic links.
- [ ] Understand LVM architecture.
- [ ] Inspect swap space.
- [ ] Build the storage-report script.
- [ ] Understand storage security.

**END OF LESSON 07**
