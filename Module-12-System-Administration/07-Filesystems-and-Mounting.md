---
---


# Module 12 – Lesson 07: Filesystems and Mounting

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a filesystem is.
2. Understand how Linux organises filesystems.
3. Identify common Linux filesystem types.
4. Understand mounting and unmounting.
5. Inspect mounted filesystems.
6. Understand `/etc/fstab`.
7. Understand UUIDs and mount options.
8. Safely practise mounting a filesystem using a temporary disk image.

---

## 2. What Is a Filesystem?

A filesystem is a method used by an operating system to organise, store, retrieve, and manage files on a storage device.

Think of a filesystem as a library.

- Books represent files.
- Shelves represent directories.
- Labels help identify information.
- The library catalogue helps locate specific books.

Without a filesystem, the operating system would not have a standard way to organise and retrieve files.

### Real-world example

A university server may store:

- Student records.
- Examination results.
- Application files.
- Database records.
- Backup archives.

The filesystem organises these files so the operating system and applications can access them.

---

## 3. Common Linux Filesystem Types

Linux supports several filesystem types.

### A. ext4

`ext4` is one of the most commonly used Linux filesystems.

Features:

- Journaling support.
- Good general-purpose performance.
- Supports large files and filesystems.
- Widely supported by Linux distributions.

### B. XFS

XFS is a high-performance journaling filesystem.

It is commonly used in enterprise Linux environments.

### C. Btrfs

Btrfs is a modern Linux filesystem with advanced features.

Features include:

- Snapshots.
- Checksums.
- Subvolumes.
- Compression support.

### D. ext3

An older Linux journaling filesystem.

### E. FAT32

A filesystem commonly used for removable storage.

It has a maximum individual file size of approximately 4 GiB minus one byte.

### F. exFAT

A filesystem commonly used for flash storage and removable drives.

It supports files larger than the FAT32 individual-file limit.

### G. NTFS

NTFS is commonly used by Windows.

Linux can access NTFS filesystems using supported drivers.

---

## 4. Checking Filesystem Types

Linux provides several commands for inspecting filesystems.

### Command 1: lsblk

```bash
lsblk -f
```

Displays block devices, filesystem types, labels, UUIDs, and mount points.

### Command 2: df

```bash
df -Th
```

Displays mounted filesystems and their types.

### Command 3: blkid

```bash
sudo blkid
```

Displays filesystem identification information for recognised block devices.

### Command 4: findmnt

```bash
findmnt
```

Displays mounted filesystems and their relationships.

These commands are useful during storage administration and troubleshooting.

---

## 5. Understanding Mounting

Mounting is the process of making a filesystem accessible through a directory in the Linux filesystem hierarchy.

This directory is called a **mount point**.

For example:

A USB drive might be mounted at:

```text
/media/user/USB
```

After mounting, the files stored on the USB drive become accessible through that directory.

### Important concept

Linux does not normally assign drive letters like Windows.

Instead, filesystems are attached to directories within a single directory hierarchy.

Windows example:

```text
C:\
D:\
E:\
```

Linux example:

```text
/
/home
/media/usb
/mnt/data
```

---

## 6. Understanding Mount Points

A mount point is an existing directory where a filesystem becomes accessible.

Common mount point locations include:

| Mount Point | Purpose |
|---|---|
| `/` | Root filesystem |
| `/home` | User home directories |
| `/boot` | Boot-related files |
| `/mnt` | Common temporary mount location |
| `/media` | Common location for removable media |

Inspect mount points:

```bash
findmnt
```

Inspect the root filesystem:

```bash
findmnt /
```

---

## 7. The mount Command

The `mount` command attaches a filesystem to a directory.

General syntax:

```bash
sudo mount DEVICE MOUNT_POINT
```

Example:

```bash
sudo mount /dev/sdb1 /mnt/usb
```

This example assumes:

- `/dev/sdb1` is the correct filesystem device.
- `/mnt/usb` exists.
- The filesystem is supported.

**Warning:** Never copy this example blindly. Selecting the wrong device can expose or interfere with important storage.

Inspect devices before mounting anything.

### Display mounted filesystems

```bash
mount
```

A more readable alternative is:

```bash
findmnt
```

---

## 8. The umount Command

Unmounting detaches a filesystem from its mount point.

General syntax:

```bash
sudo umount MOUNT_POINT
```

Example:

```bash
sudo umount /mnt/usb
```

Notice that the command is spelled `umount`, not `unmount`.

### Why is unmounting important?

Before removing a USB drive, the operating system may still be writing information to it.

Safely unmounting helps ensure pending writes are completed and the filesystem is detached properly.

### Identify a busy mount point

If unmounting fails because the filesystem is busy, inspect which processes are using it.

For example:

```bash
sudo fuser -vm /mnt/usb
```

Do not force an unmount without understanding what is using the filesystem.

---

## 9. Understanding UUIDs

UUID stands for Universally Unique Identifier.

Linux filesystems can have UUIDs that identify them independently of device names.

Example:

```text
UUID="12345678-abcd-1234-abcd-123456789abc"
```

This is an illustrative UUID.

### Why use UUIDs?

Device names such as `/dev/sda1` may change depending on device detection order.

A filesystem UUID provides a more stable way to identify a filesystem.

### Display filesystem UUIDs

```bash
lsblk -f
```

Or:

```bash
sudo blkid
```

UUIDs are commonly used in persistent mount configurations.

---

## 10. Understanding /etc/fstab

The file:

```text
/etc/fstab
```

contains information about filesystems that Linux can mount automatically.

It is commonly used to configure filesystems that should be available during startup.

A typical entry has six fields:

```text
UUID=example-uuid /mnt/data ext4 defaults 0 2
```

The fields are:

| Field | Meaning |
|---|---|
| 1 | Device or filesystem identifier |
| 2 | Mount point |
| 3 | Filesystem type |
| 4 | Mount options |
| 5 | Dump utility setting |
| 6 | Filesystem check order |

### Understanding the example

```text
UUID=example-uuid /mnt/data ext4 defaults 0 2
```

- `UUID=example-uuid`: Identifies the filesystem.
- `/mnt/data`: Where it should be mounted.
- `ext4`: Filesystem type.
- `defaults`: Standard mount options.
- `0`: Dump utility setting.
- `2`: Filesystem check order.

This is an educational example, not an entry to copy into your actual configuration.

### Inspect fstab

```bash
cat /etc/fstab
```

**Important:** Incorrect entries in `/etc/fstab` can cause boot problems. Do not edit this file during this lesson.

---

## 11. Understanding Mount Options

Mount options control how a filesystem behaves.

Common options include:

| Option | Meaning |
|---|---|
| `defaults` | Uses standard default options |
| `ro` | Mount read-only |
| `rw` | Mount read-write |
| `noexec` | Prevents execution of programs from the mounted filesystem |
| `nosuid` | Ignores set-user-ID and set-group-ID bits |
| `nodev` | Prevents interpretation of device files |

### Example

```bash
sudo mount -o ro DEVICE MOUNT_POINT
```

This requests a read-only mount.

Replace the placeholders only when working with a verified device and mount point.

Read-only mounting can be useful when inspecting filesystems where modification is not desired, although it is not a substitute for forensic write protection.

---

## 12. Practical Lab: Creating and Mounting a Safe Filesystem Image

### Objective

Practise filesystem creation and mounting without modifying your Kali VM's real partitions.

We will create a small disk-image file inside your personal course lab.

The image behaves like a small virtual disk.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson07
cd ~/module12-lab/lesson07
```

### Step 2: Create a 64 MiB disk image

```bash
dd if=/dev/zero of=filesystem-lab.img bs=1M count=64 status=progress
```

This creates a regular file containing 64 MiB of zeroed data.

It does not overwrite a physical disk.

### Step 3: Create an ext4 filesystem inside the image

```bash
mkfs.ext4 -F filesystem-lab.img
```

The `-F` option allows the tool to create a filesystem in a regular file.

Do not replace the filename with a real disk device.

### Step 4: Create a mount point

```bash
mkdir -p ~/module12-lab/lesson07/mountpoint
```

### Step 5: Mount the image

```bash
sudo mount -o loop filesystem-lab.img ~/module12-lab/lesson07/mountpoint
```

The loop option allows a regular file to be used as a block device.

### Step 6: Verify the mount

```bash
findmnt ~/module12-lab/lesson07/mountpoint
```

You should see the filesystem associated with the image.

### Step 7: Create a test file

```bash
sudo touch ~/module12-lab/lesson07/mountpoint/test-file.txt
```

### Step 8: Add information to the test file

```bash
sudo sh -c 'echo "This is my Linux filesystem mounting lab." > ~/module12-lab/lesson07/mountpoint/test-file.txt'
```

### Step 9: Read the test file

```bash
sudo cat ~/module12-lab/lesson07/mountpoint/test-file.txt
```

You should see:

```text
This is my Linux filesystem mounting lab.
```

### Step 10: Inspect filesystem usage

```bash
df -h ~/module12-lab/lesson07/mountpoint
```

### Step 11: Unmount the filesystem

First leave the mount directory:

```bash
cd ~/module12-lab/lesson07
```

Then unmount it:

```bash
sudo umount ~/module12-lab/lesson07/mountpoint
```

### Step 12: Verify unmounting

```bash
findmnt ~/module12-lab/lesson07/mountpoint
```

No mounted filesystem should be returned for that mount point.

### Step 13: Inspect your image file

```bash
ls -lh filesystem-lab.img
```

The image remains in your lesson directory.

You may keep it for future exercises.

---

## 13. Cybersecurity Connection

Filesystem management is important in cybersecurity.

Security professionals use filesystem knowledge to:

- Investigate suspicious files.
- Inspect removable storage.
- Protect sensitive data.
- Understand filesystem permissions.
- Examine mounted devices.
- Reduce the risk of unauthorised execution.
- Preserve evidence during investigations.

For example, mounting a disk image read-only can help an investigator examine its contents without intentionally modifying the filesystem.

However, formal digital forensics may require specialised write blockers, validated acquisition tools, and evidence-integrity procedures.

---

## 14. Lesson Summary

You learned:

1. A filesystem organises information on storage devices.
2. Linux supports ext4, XFS, Btrfs, FAT32, exFAT, and NTFS.
3. Mounting makes a filesystem accessible through a directory.
4. Unmounting safely detaches a filesystem.
5. UUIDs identify filesystems.
6. `/etc/fstab` stores persistent mount configuration.
7. Mount options control filesystem behaviour.
8. A disk-image file can be used to practise filesystem operations safely.
9. `findmnt`, `lsblk`, and `df` help inspect filesystems.

---

## 15. Knowledge Check

1. What is a filesystem?
2. What is the purpose of ext4?
3. What is a mount point?
4. What does the `mount` command do?
5. What does `umount` do?
6. Why should a USB drive be unmounted before removal?
7. What is a UUID?
8. What is the purpose of `/etc/fstab`?
9. What is the difference between `ro` and `rw`?
10. Why can mounting a disk image be useful in cybersecurity?

---

## 16. Practical Completion Checklist

- [ ] I understand Linux filesystems.
- [ ] I can identify common filesystem types.
- [ ] I understand mounting and unmounting.
- [ ] I understand UUIDs.
- [ ] I inspected `/etc/fstab`.
- [ ] I understand common mount options.
- [ ] I created a 64 MiB filesystem image.
- [ ] I formatted the image as ext4.
- [ ] I mounted the image successfully.
- [ ] I created and read a test file.
- [ ] I safely unmounted the image.
- [ ] I understand the cybersecurity applications of filesystem management.

**Lesson 07 is complete when you understand the concepts and successfully mount, use, and unmount your filesystem image.**
