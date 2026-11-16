
# Module 12 – Lesson 06: Managing Storage and Disks

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux storage management.
2. Understand disks, partitions, and filesystems.
3. Identify storage devices.
4. Monitor disk capacity.
5. Inspect partitions and block devices.
6. Understand disk partitioning concepts.
7. Use safe commands to investigate storage devices.
8. Generate a storage inspection report.

---

## 2. What Is Storage Management?

Storage management is the process of organising, monitoring, maintaining, and controlling the storage devices connected to a computer.

Linux systems use storage devices to hold:

- Operating system files.
- Applications.
- User documents.
- Databases.
- Logs.
- Backups.
- Virtual machine files.

### Real-world example

Imagine a university server storing student records and examination results.

The server needs enough storage to:

- Store student information.
- Save uploaded assignments.
- Maintain database files.
- Store application logs.
- Keep backups.

A Linux administrator monitors storage to prevent the server from running out of space.

---

## 3. Understanding Storage Devices

A storage device is hardware used to store digital information.

Common storage devices include:

### A. HDD (Hard Disk Drive)

An HDD stores information using magnetic disks.

Characteristics:

- Mechanical moving components.
- Usually lower cost per storage capacity.
- Generally slower than SSDs.

### B. SSD (Solid State Drive)

An SSD stores information using flash memory.

Characteristics:

- No mechanical moving parts.
- Generally faster than HDDs.
- Common in modern laptops and servers.

### C. USB Flash Drive

A portable storage device connected through a USB port.

It is commonly used for:

- File transfers.
- Backups.
- Operating system installation media.

### D. Virtual Disks

Virtual machines use virtual disk files that behave like physical storage devices.

For example, your Kali Linux virtual machine may use a virtual disk managed by VMware.

Linux sees the virtual disk as a block device.

---

## 4. Understanding Block Devices

Linux represents many storage devices as block devices.

These devices allow information to be read and written in blocks.

Common device names include:

| Device | Meaning |
|---|---|
| `/dev/sda` | First SATA or SCSI-style disk |
| `/dev/sdb` | Second SATA or SCSI-style disk |
| `/dev/nvme0n1` | First NVMe disk |
| `/dev/vda` | First virtual disk using virtio |
| `/dev/loop0` | Loopback block device |

The actual names depend on the hardware and virtualisation environment.

### List block devices

```bash
lsblk
```

Display additional information:

```bash
lsblk -f
```

This can show:

- Device names.
- Filesystem types.
- Labels.
- UUIDs.
- Mount points.

### Example output

```text
NAME        SIZE TYPE MOUNTPOINTS
sda          40G disk
├─sda1       38G part /
└─sda2        2G part [SWAP]
```

This is an illustrative example, not a description of your actual VM.

---

## 5. Understanding Disk Partitions

A partition is a logical division of a physical or virtual storage device.

For example, a 500 GB disk could be divided into:

- 100 GB for the operating system.
- 300 GB for user data.
- 100 GB for backups.

These divisions are called partitions.

### Why use partitions?

Partitions help administrators:

- Separate operating system files from personal data.
- Organise storage.
- Manage different filesystems.
- Allocate storage for specific purposes.

### Partition naming examples

For a SATA-style disk:

```text
/dev/sda
/dev/sda1
/dev/sda2
```

For an NVMe disk:

```text
/dev/nvme0n1
/dev/nvme0n1p1
/dev/nvme0n1p2
```

The whole disk and its partitions are different block devices.

---

## 6. Understanding Filesystems

A filesystem determines how files and directories are organised on a storage device.

Linux supports several filesystem types.

| Filesystem | Description |
|---|---|
| ext4 | Common Linux filesystem |
| ext3 | Older journaling Linux filesystem |
| ext2 | Older Linux filesystem without journaling |
| XFS | High-performance journaling filesystem |
| Btrfs | Modern filesystem with advanced features |
| FAT32 | Commonly used on removable storage |
| exFAT | Designed for flash storage and large files |
| NTFS | Common Windows filesystem |

### Check filesystem information

```bash
lsblk -f
```

You can also use:

```bash
df -T
```

This displays mounted filesystems and their filesystem types.

---

## 7. Checking Disk Usage

One of the most important storage administration tasks is monitoring available disk space.

### Display disk usage

```bash
df -h
```

The `-h` option displays sizes in human-readable units.

### Display filesystem types

```bash
df -Th
```

### Check a particular directory

```bash
du -sh ~/module12-lab
```

### Find the largest directories in your home directory

```bash
du -sh ~/* 2>/dev/null
```

### Understanding df and du

| Command | Purpose |
|---|---|
| `df` | Displays filesystem space usage |
| `du` | Estimates space used by files and directories |

Remember:

- `df` examines filesystem capacity.
- `du` examines directory and file sizes.

---

## 8. Understanding Partition Tables

A partition table records how a disk is divided into partitions.

Two common partition table types are:

### A. MBR (Master Boot Record)

MBR is an older partitioning system.

Characteristics:

- Commonly limited to four primary partition entries.
- Uses extended partitions to work around that limitation.
- Generally limited to approximately 2 TiB with conventional 512-byte sectors.

### B. GPT (GUID Partition Table)

GPT is a modern partitioning system.

Characteristics:

- Supports many partitions.
- Supports very large disks.
- Includes partition-table backup information.
- Commonly used with modern UEFI systems.

### Inspect partition information

```bash
sudo fdisk -l
```

This displays information about detected disks and partitions.

It is an inspection command when used with `-l`.

Do not enter the interactive partitioning interface during this lesson.

---

## 9. Understanding Mount Points

Linux makes filesystems accessible through directories called mount points.

For example:

A USB drive might be mounted at:

```text
/media/user/USB
```

A Linux root filesystem is mounted at:

```text
/
```

### Display mounted filesystems

```bash
findmnt
```

### Display the root filesystem

```bash
findmnt /
```

### Display mount information

```bash
mount
```

Mounting makes a filesystem accessible within the Linux directory structure.

Unmounting disconnects that filesystem from its mount point.

**Important:** Never unplug an active storage device without safely unmounting it first.

---

## 10. Understanding Storage Monitoring

Administrators should monitor storage regularly.

Important indicators include:

### Disk capacity

How much storage is available?

```bash
df -h
```

### Inode usage

How many filesystem inodes are available?

```bash
df -i
```

### Large directories

Which directories consume the most space?

```bash
du -sh ~/*
```

### Device health

Physical storage devices may expose health information through SMART monitoring.

A commonly used tool is `smartctl`, which is part of smartmontools.

Availability depends on the system and virtualisation environment.

A virtual disk may not expose the physical drive's SMART information.

Do not install or configure additional storage tools for this lesson.

---

## 11. Practical Lab: Linux Storage Inspection

### Objective

Inspect the storage devices and filesystems visible to your Kali Linux virtual machine.

This lab is inspection-only.

Do not create partitions, format disks, resize filesystems, or delete storage devices.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson06
cd ~/module12-lab/lesson06
```

### Step 2: List block devices

```bash
lsblk
```

Identify the main disk and its partitions.

### Step 3: Inspect filesystem information

```bash
lsblk -f
```

Identify filesystem types and mount points.

### Step 4: Check disk capacity

```bash
df -h
```

Identify the filesystem containing your root directory.

### Step 5: Inspect filesystem types

```bash
df -Th
```

### Step 6: Inspect inode usage

```bash
df -i
```

### Step 7: Inspect your lab directory size

```bash
du -sh ~/module12-lab
```

### Step 8: Identify large directories in your home directory

```bash
du -sh ~/* 2>/dev/null
```

### Step 9: Inspect mounted filesystems

```bash
findmnt
```

### Step 10: Inspect the root mount

```bash
findmnt /
```

### Step 11: Inspect partition information

```bash
sudo fdisk -l
```

Read the output carefully.

Do not use interactive partitioning commands.

### Step 12: Generate your storage report

Run:

```bash
{
echo "LINUX STORAGE INSPECTION REPORT"
echo "==============================="
echo ""
echo "BLOCK DEVICES:"
lsblk
echo ""
echo "FILESYSTEM INFORMATION:"
lsblk -f
echo ""
echo "DISK USAGE:"
df -h
echo ""
echo "FILESYSTEM TYPES:"
df -Th
echo ""
echo "INODE USAGE:"
df -i
echo ""
echo "LAB DIRECTORY SIZE:"
du -sh ~/module12-lab
echo ""
echo "MOUNT INFORMATION:"
findmnt
} > storage-report.txt 2>&1
```

### Step 13: Read your report

```bash
less storage-report.txt
```

Press `q` to exit.

### Step 14: Verify the report

```bash
ls -lh storage-report.txt
wc -l storage-report.txt
```

You should now have a storage inspection report.

---

## 12. Cybersecurity Connection

Storage management is important for cybersecurity.

Storage devices may contain:

- Sensitive documents.
- User credentials.
- Database records.
- Authentication logs.
- Application data.
- Backup archives.

Security administrators must ensure that sensitive information is properly protected.

Important security considerations include:

1. Restricting access to sensitive files.
2. Monitoring unexpected storage growth.
3. Protecting backup files.
4. Using appropriate disk encryption.
5. Safely handling removable storage.
6. Monitoring storage devices for unusual activity.

For example, unexpected disk usage may indicate excessive logging, an application malfunction, or suspicious file creation.

It must be investigated rather than automatically treated as an attack.

---

## 13. Lesson Summary

You learned:

1. Storage management controls how Linux uses storage devices.
2. HDDs, SSDs, USB drives, and virtual disks are common storage devices.
3. Linux represents many storage devices as block devices.
4. Partitions divide storage devices into logical sections.
5. Filesystems organise files and directories.
6. MBR and GPT are partition-table formats.
7. Mount points provide access to filesystems.
8. `lsblk` displays block devices.
9. `df` displays filesystem usage.
10. `du` estimates directory sizes.
11. `fdisk -l` inspects partition information.
12. `findmnt` displays mounted filesystems.

---

## 14. Knowledge Check

1. What is storage management?
2. What is the difference between an HDD and an SSD?
3. What is a block device?
4. What is a partition?
5. What is the purpose of a filesystem?
6. What is the difference between MBR and GPT?
7. Which command lists block devices?
8. What is the difference between `df` and `du`?
9. What is a mount point?
10. Why is storage monitoring important for cybersecurity?

---

## 15. Practical Completion Checklist

- [ ] I understand Linux storage management.
- [ ] I understand HDDs, SSDs, and virtual disks.
- [ ] I inspected block devices using `lsblk`.
- [ ] I inspected filesystem information.
- [ ] I checked disk and inode usage.
- [ ] I inspected directory sizes.
- [ ] I inspected mounted filesystems.
- [ ] I inspected partition information safely.
- [ ] I generated `storage-report.txt`.

**Lesson 06 is complete when you understand the concepts and successfully generate your storage inspection report.**
