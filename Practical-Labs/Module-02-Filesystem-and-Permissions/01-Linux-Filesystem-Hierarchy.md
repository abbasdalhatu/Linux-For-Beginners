
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 01: LINUX FILESYSTEM HIERARCHY

### 1. Introduction

The Linux filesystem is the structure Linux uses to organize files, directories, devices, programs, and system information.

Unlike Windows, which commonly uses drive letters such as `C:\` and `D:\`, Linux organizes everything under one main directory called the **root directory**.

The root directory is represented by:

```bash
/
```

Think of `/` as the main trunk of a tree. Every other directory branches out from it.

### 2. Learning Objectives

By the end of this practical, you should be able to:

1. Explain the Linux filesystem hierarchy.
2. Identify important Linux system directories.
3. Navigate between system directories.
4. Understand the difference between `/` and `/root`.
5. Inspect directory structures using terminal commands.
6. Identify where important Linux files are stored.

### 3. Understanding the Root Directory

The root directory `/` is the starting point of the Linux filesystem.

Run:

```bash
cd /
pwd
ls -lah
```

Expected observation:

You should see directories such as:

```text
bin
boot
dev
etc
home
lib
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

These directories have different responsibilities.

### 4. Important Linux Directories

#### A. `/` – Root Directory

The highest-level directory in Linux.

Everything in the filesystem exists beneath it.

```bash
cd /
pwd
```

#### B. `/home` – User Home Directories

Stores personal directories belonging to ordinary users.

Example:

```text
/home/kali
/home/student
/home/user
```

Inspect it:

```bash
ls -lah /home
```

Navigate to your home directory:

```bash
cd ~
pwd
```

#### C. `/root` – Root User's Home

This is the home directory of the system administrator account called `root`.

Important distinction:

- `/` is the filesystem's top-level directory.
- `/root` is the root user's personal home directory.

Inspect it:

```bash
ls -ld /root
```

You may receive a permission-denied message when using an ordinary account. That is normal.

#### D. `/etc` – System Configuration

Stores system-wide configuration files.

Examples include:

```text
/etc/passwd
/etc/group
/etc/hostname
/etc/hosts
/etc/fstab
```

Inspect:

```bash
ls -lah /etc
```

Read the hostname configuration:

```bash
cat /etc/hostname
```

Inspect user account records:

```bash
cat /etc/passwd
```

Note: `/etc/passwd` contains account information, not users' actual passwords.

#### E. `/var` – Variable Data

Stores data that changes frequently.

Examples:

- System logs
- Application logs
- Package information
- Caches
- Mail queues

Important subdirectory:

```text
/var/log
```

Inspect:

```bash
ls -lah /var
ls -lah /var/log
```

Display recent system log messages:

```bash
journalctl -n 20 --no-pager
```

#### F. `/tmp` – Temporary Files

Stores temporary files used by applications and processes.

Inspect:

```bash
ls -lah /tmp
```

Create a temporary practice file:

```bash
touch /tmp/linux-lab-test.txt
ls -l /tmp/linux-lab-test.txt
```

Remove only the file you created:

```bash
rm /tmp/linux-lab-test.txt
```

#### G. `/usr` – User Programs and Resources

Contains many installed programs, libraries, and shared resources.

Important locations:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/share
/usr/local
```

Inspect:

```bash
ls /usr
ls /usr/bin | head
```

#### H. `/bin` – Essential User Commands

Traditionally contains essential commands used by ordinary users.

Examples include:

```text
ls
cp
mv
cat
```

On modern Kali and Debian systems, `/bin` may be a symbolic link to `/usr/bin`.

Inspect:

```bash
ls -ld /bin
```

#### I. `/sbin` – System Administration Commands

Traditionally contains commands used for system administration.

Inspect:

```bash
ls -ld /sbin
ls /sbin | head
```

On modern systems, `/sbin` may point to `/usr/sbin`.

#### J. `/boot` – Boot Files

Contains files required during the Linux boot process.

Examples:

- Linux kernel images
- Initial RAM filesystem images
- Bootloader-related files

Inspect:

```bash
ls -lah /boot
```

Do not modify or delete files in this directory during this practical.

#### K. `/dev` – Device Files

Linux represents many hardware devices and virtual devices through files in `/dev`.

Examples:

```text
/dev/null
/dev/zero
/dev/random
/dev/tty
```

Inspect:

```bash
ls -lah /dev
```

Check a specific device:

```bash
ls -l /dev/null
```

#### L. `/proc` – Process and Kernel Information

A virtual filesystem containing information about running processes and the kernel.

Examples:

```text
/proc/cpuinfo
/proc/meminfo
/proc/uptime
```

Inspect:

```bash
cat /proc/cpuinfo
cat /proc/meminfo
cat /proc/uptime
```

#### M. `/sys` – System and Hardware Information

Provides information about devices, drivers, and the kernel.

Inspect:

```bash
ls /sys
```

#### N. `/mnt` – Temporary Mount Points

Often used by administrators to attach filesystems manually.

Inspect:

```bash
ls -lah /mnt
```

#### O. `/media` – Removable Media

Commonly used for automatically mounted removable storage devices.

Examples include USB drives and external storage.

Inspect:

```bash
ls -lah /media
```

#### P. `/opt` – Optional Software

Often used for additional software installed outside the standard package-managed locations.

Inspect:

```bash
ls -lah /opt
```

#### Q. `/run` – Runtime Data

Contains temporary runtime information created after the system starts.

Inspect:

```bash
ls -lah /run
```

### 5. Filesystem Hierarchy Summary

| Directory | Main Purpose |
|---|---|
| `/` | Filesystem root |
| `/home` | Ordinary users' home directories |
| `/root` | Root user's home directory |
| `/etc` | Configuration files |
| `/var` | Logs and changing data |
| `/tmp` | Temporary files |
| `/usr` | Programs and shared resources |
| `/bin` | Essential user commands |
| `/sbin` | System administration commands |
| `/boot` | Boot-related files |
| `/dev` | Device files |
| `/proc` | Process and kernel information |
| `/sys` | Hardware and kernel interfaces |
| `/mnt` | Temporary mount points |
| `/media` | Removable media |
| `/opt` | Optional software |
| `/run` | Runtime information |

### 6. Practical Exercise: Explore the Linux Filesystem

Create a dedicated workspace:

```bash
mkdir -p ~/practical-labs/module02/lab01
cd ~/practical-labs/module02/lab01
```

Create a directory report:

```bash
{
    echo "LINUX FILESYSTEM EXPLORATION REPORT"
    echo "=================================="
    echo "Date: $(date)"
    echo "User: $(whoami)"
    echo "Hostname: $(hostname)"
    echo
    echo "ROOT DIRECTORY:"
    ls -lah /
    echo
    echo "HOME DIRECTORY:"
    ls -lah ~
    echo
    echo "CONFIGURATION DIRECTORY:"
    ls -lah /etc | head -20
    echo
    echo "LOG DIRECTORY:"
    ls -lah /var/log | head -20
    echo
    echo "SYSTEM INFORMATION:"
    cat /etc/os-release
} > filesystem-report.txt
```

Read your report:

```bash
cat filesystem-report.txt
```

### 7. Practical Exercise: Identify Important Files

Run these commands and observe the results:

```bash
file /etc/passwd
file /etc/hostname
file /proc/cpuinfo
file /dev/null
```

Notice that Linux files do not all represent ordinary text documents. Some represent devices or virtual system information.

### 8. Security Connection

Understanding the filesystem hierarchy is important in cybersecurity because sensitive information and security-relevant configuration are stored in different locations.

Examples:

| Location | Security Importance |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Protected password hashes |
| `/etc/ssh/` | SSH configuration |
| `/var/log/` | Security and system logs |
| `/home/` | User files and data |
| `/root/` | Administrator files |
| `/tmp/` | Temporary data that may require security monitoring |

For this lesson, inspect only files your account is authorized to access.

Never attempt to change system configuration or read protected credentials as part of this introductory exercise.

### 9. Knowledge Check

1. What is the purpose of `/` in Linux?
2. What is the difference between `/` and `/root`?
3. Which directory stores system configuration files?
4. Where are Linux system logs commonly stored?
5. What is the purpose of `/proc`?
6. Which directory contains device files?
7. What is the purpose of `/home`?
8. Why should you avoid deleting files inside `/boot`?
9. What is the difference between `/tmp` and `/var`?
10. Why is understanding the filesystem important to cybersecurity?

### 10. Lab Completion Checklist

- [ ] Navigated to `/`.
- [ ] Inspected the main filesystem directories.
- [ ] Explored `/home`, `/etc`, `/var`, and `/tmp`.
- [ ] Inspected `/proc` and `/dev`.
- [ ] Created the Module 02 Lab 01 workspace.
- [ ] Generated `filesystem-report.txt`.
- [ ] Identified important security-related directories.
- [ ] Completed the knowledge check.

### 11. Expected Result

Your practical workspace should contain:

```text
~/practical-labs/module02/lab01/
└── filesystem-report.txt
```

**Lab 01 Status:** Complete when all practical exercises and the checklist are finished.

**Next Lab:** Lab 02 – File Types and Inodes.
