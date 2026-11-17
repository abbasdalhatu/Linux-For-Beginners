---
---

# LESSON 02: LINUX DIRECTORY STRUCTURE

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Linux organizes its files and directories using a hierarchical structure.

This means directories can contain other directories, which can contain additional files and directories.

The entire structure begins at the root directory `/`.

Understanding this structure helps Linux administrators and ethical hackers locate configuration files, system logs, applications, user data, and security-related information.

## 2. The Linux Directory Tree

The Linux filesystem resembles an upside-down tree.

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
│   ├── kali
│   └── student
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

Explanation:

- `/` is the parent of the entire directory hierarchy.
- `/home` contains normal users' home directories.
- `/home/kali` contains files belonging to the `kali` user.
- `/etc` contains system-wide configuration.
- `/var` contains variable data such as logs and caches.

## 3. Important Directories Explained

### A. `/` – Root Directory

The root directory is the starting point of the Linux filesystem.

All directories and files in the standard hierarchy exist beneath it.

Command:

```bash
ls /
```

### B. `/home` – User Directories

This directory contains the home directories of normal users.

Example:

```text
/home/kali
/home/student
```

Each user normally has a personal directory where documents, downloads, and other files are stored.

Command:

```bash
ls /home
```

### C. `/root` – Root User's Home

This is the personal home directory of the root account.

It is not the same as `/`.

Command:

```bash
sudo ls -la /root
```

### D. `/etc` – Configuration Files

This directory contains system-wide configuration files.

Examples:

```text
/etc/passwd
/etc/group
/etc/hostname
/etc/hosts
/etc/fstab
```

These files provide information about users, groups, hostnames, networking, and filesystem mounting.

Command:

```bash
ls /etc
```

### E. `/var` – Variable Data

The `/var` directory stores data that changes during normal system operation.

Examples:

```text
/var/log
/var/cache
/var/tmp
/var/lib
```

Security professionals often inspect `/var/log` when investigating system events.

Command:

```bash
ls /var/log
```

### F. `/usr` – User Applications and Resources

The `/usr` directory contains many installed applications, executable programs, libraries, and shared resources.

Examples:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/share
```

On modern Linux systems, many traditional directories such as `/bin` and `/sbin` are symbolic links to locations under `/usr`.

Command:

```bash
ls /usr
```

### G. `/bin` – Essential Commands

This directory traditionally contains essential user commands.

Examples include:

```text
ls
cp
mv
cat
```

On modern Kali installations, `/bin` is commonly linked to `/usr/bin`.

Command:

```bash
ls -ld /bin
```

### H. `/sbin` – System Administration Commands

This directory traditionally contains system administration programs.

Examples include:

```text
fsck
reboot
```

On modern Kali installations, `/sbin` is commonly linked to `/usr/sbin`.

Command:

```bash
ls -ld /sbin
```

### I. `/boot` – Boot Files

This directory contains files required to start the operating system.

Examples include:

- Linux kernel images.
- Initial RAM filesystem images.
- Bootloader-related files.

Command:

```bash
ls /boot
```

### J. `/dev` – Device Files

Linux represents many hardware and virtual devices using special files.

Examples:

```text
/dev/null
/dev/zero
/dev/random
```

Command:

```bash
ls /dev
```

### K. `/proc` – Process and Kernel Information

`/proc` is a virtual filesystem that exposes information about running processes and the kernel.

It does not behave like an ordinary disk directory.

Examples:

```text
/proc/cpuinfo
/proc/meminfo
/proc/uptime
```

Commands:

```bash
cat /proc/cpuinfo
cat /proc/meminfo
```

### L. `/sys` – System and Device Information

`/sys` is another virtual filesystem.

It exposes information about devices, drivers, and kernel subsystems.

Command:

```bash
ls /sys
```

### M. `/tmp` – Temporary Files

This directory is used for temporary files created by applications and users.

Command:

```bash
ls -la /tmp
```

Temporary files may be removed automatically, depending on system configuration.

Do not store important permanent data here.

### N. `/media` – Removable Media

This directory is commonly used as a mount point for removable storage devices.

Examples include USB drives and other removable media.

Command:

```bash
ls /media
```

### O. `/mnt` – Temporary Mount Point

This directory is commonly used by administrators to mount filesystems manually.

Command:

```bash
ls /mnt
```

### P. `/opt` – Optional Software

This directory is commonly used for optional or third-party software packages.

Command:

```bash
ls /opt
```

### Q. `/run` – Runtime Data

This directory contains temporary runtime information created since the system booted.

Examples include process IDs, sockets, and service-related information.

Command:

```bash
ls /run
```

### R. `/srv` – Service Data

This directory is intended for data provided by services running on the system.

For example, a web server might store website data in a directory under `/srv`.

Command:

```bash
ls /srv
```

## 4. Important Directory Comparison

| Directory | Main Purpose |
|---|---|
| `/` | Filesystem starting point |
| `/home` | Normal users' personal directories |
| `/root` | Root user's personal directory |
| `/etc` | System configuration |
| `/var` | Variable system data |
| `/usr` | Applications and shared resources |
| `/boot` | Boot-related files |
| `/dev` | Device files |
| `/proc` | Process and kernel information |
| `/sys` | Device and kernel information |
| `/tmp` | Temporary files |
| `/opt` | Optional software |
| `/mnt` | Temporary mount point |
| `/media` | Removable media mount points |
| `/run` | Runtime information |
| `/srv` | Service data |

## 5. Understanding Directory Hierarchy Through an Example

Consider:

```text
/home/kali/Documents/security/notes.txt
```

Breakdown:

- `/` – Root directory.
- `home` – User directory location.
- `kali` – User's home directory.
- `Documents` – Personal documents directory.
- `security` – A subdirectory.
- `notes.txt` – A file.

This demonstrates how Linux organizes information using parent and child directories.

## 6. Why Directory Knowledge Matters in Ethical Hacking

Ethical hackers need to understand directory locations because security investigations often involve examining different parts of the operating system.

Examples:

1. `/etc` – Investigating system configuration.
2. `/var/log` – Reviewing system logs.
3. `/home` – Understanding user files and permissions.
4. `/proc` – Inspecting process and kernel information.
5. `/dev` – Understanding device interfaces.
6. `/usr/bin` – Locating installed executable programs.

Always access files according to your permissions and authorization.

## 7. Practical Laboratory

Perform the following exercises in Kali Linux.

### Exercise 1: Explore the Root Directory

```bash
ls -lah /
```

Identify at least ten directories.

### Exercise 2: Explore User Directories

```bash
ls -la /home
```

Identify your normal user's directory.

### Exercise 3: Explore Configuration Files

```bash
ls /etc
```

Then inspect:

```bash
cat /etc/hostname
```

### Exercise 4: Explore System Logs

```bash
ls -lah /var/log
```

Identify at least three entries.

### Exercise 5: Explore Kernel Information

```bash
cat /proc/meminfo
```

Identify the total memory information displayed.

### Exercise 6: Explore Device Files

```bash
ls /dev
```

Find:

```text
/dev/null
/dev/zero
/dev/random
```

### Exercise 7: Explore Application Directories

```bash
ls /usr
ls /usr/bin | head
```

Observe the installed application directories and executable names.

## 8. Knowledge Check

1. What is the purpose of the `/` directory?
2. What is stored inside `/home`?
3. What is the difference between `/root` and `/`?
4. What is the purpose of `/etc`?
5. Why is `/var/log` important in cybersecurity?
6. What is the purpose of `/boot`?
7. What kind of information does `/proc` provide?
8. What is the difference between `/mnt` and `/media`?
9. What is the purpose of `/dev`?
10. What is the difference between `/usr` and `/opt`?

## 9. Lesson Summary

Linux uses a hierarchical directory structure to organize system files, user data, applications, configuration files, and runtime information.

We examined the major directories and their responsibilities.

Understanding these locations is an essential skill for Linux system administration and ethical hacking.

**Next Lesson:** Root Directory and Home Directory.