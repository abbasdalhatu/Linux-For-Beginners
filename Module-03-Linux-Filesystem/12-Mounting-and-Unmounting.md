---
---

# LESSON 12: MOUNTING AND UNMOUNTING FILESYSTEMS

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Linux uses a single directory hierarchy that begins at the root directory `/`.

Unlike Windows, where storage devices commonly receive separate drive letters such as `C:`, `D:`, and `E:`, Linux makes storage devices accessible through directories called mount points.

This process is called mounting.

For example, a USB flash drive can be connected to a Linux computer and mounted at:

```text
/media/kali/USB
```

Once mounted, its files become accessible through that directory.

Understanding mounting and unmounting is important for system administration, digital forensics, storage management, and cybersecurity.

## 2. What Is Mounting?

Mounting is the process of making a filesystem accessible through a directory in the Linux filesystem hierarchy.

A storage device may contain a filesystem, but Linux must mount it before users can normally access its files through the directory tree.

Example:

```text
Storage Device
      |
      v
Filesystem
      |
      v
Mount Point
      |
      v
Accessible Files and Directories
```

Imagine connecting a USB drive.

Linux identifies the device, and the filesystem can be mounted at a directory such as:

```text
/mnt/usb
```

You can then access its contents using:

```bash
ls /mnt/usb
```

## 3. What Is a Mount Point?

A mount point is a directory where a filesystem is attached to the Linux directory hierarchy.

Common mount-point locations include:

```text
/mnt
/media
```

### `/mnt`

Traditionally used for temporary or manually mounted filesystems.

Example:

```text
/mnt/usb
```

### `/media`

Commonly used by desktop environments for removable devices.

Example:

```text
/media/kali/USB
```

A mount point is an ordinary directory before a filesystem is mounted there.

Once another filesystem is mounted over it, the mounted filesystem's contents become visible through that directory.

## 4. Understanding the `mount` Command

The `mount` command is used to attach a filesystem to a mount point.

General syntax:

```bash
mount [options] device mount_point
```

Example:

```bash
sudo mount /dev/sdb1 /mnt/usb
```

Explanation:

- `sudo` requests administrative privileges.
- `mount` performs the mounting operation.
- `/dev/sdb1` represents the example partition.
- `/mnt/usb` is the mount point.

Important: `/dev/sdb1` is only an example. Never assume that it is your actual USB device.

Always identify the correct device first.

## 5. Creating a Mount Point

Before manually mounting a filesystem, you usually need an existing directory to use as the mount point.

Create one:

```bash
sudo mkdir -p /mnt/usb
```

The `-p` option creates missing parent directories if necessary and does not report an error when the directory already exists.

Inspect it:

```bash
ls -ld /mnt/usb
```

The directory is now available as a potential mount point.

## 6. Identifying Storage Devices Before Mounting

Before mounting a storage device, identify it correctly.

Use:

```bash
lsblk
```

For filesystem information:

```bash
lsblk -f
```

You can also inspect devices using:

```bash
sudo blkid
```

Example:

```text
NAME   FSTYPE  SIZE MOUNTPOINTS
sda           50G
├─sda1 ext4    49G /
└─sda2 swap     1G [SWAP]
sdb           16G
└─sdb1 vfat   16G
```

In this example:

- `/dev/sda` is the system disk.
- `/dev/sda1` contains the Linux filesystem.
- `/dev/sdb` is another disk.
- `/dev/sdb1` is a partition on that disk.

Do not use example device names without checking your actual system.

## 7. Understanding the `umount` Command

Unmounting safely detaches a mounted filesystem from the Linux directory hierarchy.

The command is:

```bash
umount
```

Example:

```bash
sudo umount /mnt/usb
```

After successful unmounting, the filesystem is no longer accessible through that mount point.

The correct command is `umount`, not `unmount`.

### Why Is Unmounting Important?

Unmounting helps ensure that pending filesystem operations are completed before a device is removed.

Removing a storage device without safely unmounting it can cause:

- Data loss
- Filesystem corruption
- Incomplete write operations
- Unexpected errors

Always unmount removable storage before physically disconnecting it.

## 8. Viewing Mounted Filesystems

Linux provides several commands for checking mounted filesystems.

### Command 1: `findmnt`

```bash
findmnt
```

Displays mounted filesystems and their relationships.

To inspect a particular mount point:

```bash
findmnt /
```

### Command 2: `df -h`

```bash
df -h
```

Displays used and available storage space for mounted filesystems.

### Command 3: `mount`

```bash
mount
```

Displays currently mounted filesystems.

For a more focused result:

```bash
findmnt -t ext4
```

This displays mounted filesystems of type EXT4.

## 9. Understanding `/etc/fstab`

Linux uses the file:

```text
/etc/fstab
```

to define filesystems that should be mounted automatically or otherwise configured during system startup.

The name `fstab` means filesystem table.

Inspect it:

```bash
cat /etc/fstab
```

A typical entry contains six fields:

```text
UUID=example-uuid / ext4 defaults 0 1
```

The fields are:

1. Filesystem specification – device, UUID, or another identifier.
2. Mount point – where the filesystem should be attached.
3. Filesystem type – such as ext4 or xfs.
4. Mount options – such as defaults.
5. Dump setting – traditionally used by the dump backup utility.
6. Filesystem check order – used by filesystem checking tools during startup.

Do not modify `/etc/fstab` during this lesson. An incorrect entry can cause boot problems.

## 10. Mounting by UUID

Linux can identify filesystems using UUIDs instead of relying only on device names.

For example:

```bash
sudo mount UUID=example-uuid /mnt/usb
```

Here, `example-uuid` is a placeholder, not a real UUID.

To find actual UUIDs:

```bash
sudo blkid
```

UUIDs can provide more consistent identification when device names change.

## 11. Read-Only Mounting

Sometimes a filesystem should be mounted without allowing ordinary write operations.

A read-only mount can be requested using:

```bash
sudo mount -o ro DEVICE MOUNT_POINT
```

For example, the placeholders can be replaced with a verified device and an existing mount point.

The `ro` option requests read-only mounting.

This can be useful when inspecting data without intending to modify it.

However, a normal read-only mount is not a guaranteed forensic write blocker. Strict forensic preservation requires suitable hardware or software write-blocking procedures and careful handling.

## 12. Mounting and Digital Forensics

Mounting is important during digital forensic investigations.

Investigators may need to inspect storage devices while preserving evidence.

Important practices include:

- Identify the correct storage device.
- Avoid writing to original evidence.
- Use appropriate write-blocking methods.
- Record relevant device information.
- Document mounting operations.
- Safely detach devices after inspection.

A read-only mount option can reduce accidental changes, but it should not replace proper forensic procedures.

## 13. Practical Laboratory

For this laboratory, we will practice mount-point management and filesystem inspection without mounting or modifying your system disk.

### Exercise 1: View Mounted Filesystems

```bash
findmnt
```

Observe the mount points displayed.

### Exercise 2: Inspect the Root Filesystem

```bash
findmnt /
df -h /
```

Identify the filesystem mounted at `/`.

### Exercise 3: Inspect Filesystem Types

```bash
df -Th
```

Observe the filesystem types used by your Kali VM.

### Exercise 4: Create a Practice Mount Point

```bash
sudo mkdir -p /mnt/linux-lab
```

Inspect it:

```bash
ls -ld /mnt/linux-lab
```

No storage device is mounted here yet.

### Exercise 5: Inspect the Filesystem Table

```bash
cat /etc/fstab
```

Identify the six fields in any entries that are present.

Do not edit this file.

### Exercise 6: Inspect Storage Devices

```bash
lsblk -f
sudo blkid
```

Observe the device names and UUIDs.

Do not mount an unknown device.

### Exercise 7: Remove the Empty Practice Mount Point

Because we did not mount a filesystem there, we can safely remove the empty directory:

```bash
sudo rmdir /mnt/linux-lab
```

If the directory is not empty, stop and inspect it instead of forcing removal.

## 14. Important Safety Rules

Before mounting or unmounting a filesystem:

1. Identify the correct device.
2. Verify its filesystem type.
3. Confirm that the mount point is correct.
4. Avoid modifying unknown partitions.
5. Never unmount an active system filesystem.
6. Do not remove a mounted removable device physically.
7. Be careful when editing `/etc/fstab`.

Never experiment with mounting random partitions or formatting devices.

## 15. Knowledge Check

1. What is mounting in Linux?
2. What is a mount point?
3. What is the purpose of `/mnt`?
4. What is the purpose of `/media`?
5. Which command mounts a filesystem?
6. Which command safely detaches a mounted filesystem?
7. Why should you unmount a USB drive before removing it?
8. What is the purpose of `/etc/fstab`?
9. What does the `ro` mount option mean?
10. What is a UUID?
11. Which command displays mounted filesystem information?
12. Why is write protection important in digital forensics?

## 16. Lesson Summary

Mounting makes a filesystem accessible through a directory in Linux.

Unmounting safely detaches it.

We learned:

- Mounting and unmounting.
- Mount points.
- The `mount` and `umount` commands.
- The `/mnt` and `/media` directories.
- Filesystem inspection.
- `/etc/fstab`.
- UUID-based mounting.
- Read-only mounting.
- Mounting in digital forensics.

Important commands:

```bash
mount
umount
findmnt
df -h
lsblk -f
sudo blkid
cat /etc/fstab
```

**Next Lesson:** Understanding `/proc` and `/sys`.