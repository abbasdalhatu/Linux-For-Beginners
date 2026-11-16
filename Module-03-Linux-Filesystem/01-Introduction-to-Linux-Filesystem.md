# LESSON 01: INTRODUCTION TO THE LINUX FILESYSTEM

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

A filesystem is the method an operating system uses to organize, store, manage, and retrieve data on a storage device.

Think of a filesystem as a library.

- The library building represents the storage device.
- Bookshelves represent directories.
- Books represent files.
- Labels help you locate information.

Linux uses a hierarchical filesystem structure to organize its files and directories.

Understanding this structure is important for Linux administrators, system engineers, and ethical hackers.

## 2. What Is the Linux Filesystem?

The Linux filesystem is the organized structure through which Linux stores and accesses files, directories, and system information.

Unlike Windows, which commonly uses separate drive letters such as `C:`, `D:`, and `E:`, Linux organizes its filesystem under one main directory called the root directory.

The root directory is represented by:

```bash
/
```

Everything in the standard Linux directory hierarchy is located under `/`, including user files, applications, configuration files, and system resources.

## 3. Understanding Files

A file is a collection of information stored under a particular name.

Examples include:

- Text documents
- Images
- Videos
- Programs
- Configuration files
- System logs
- Scripts

Examples of Linux files:

```text
notes.txt
passwords.txt
script.sh
config.conf
image.png
```

Linux treats many resources as files, including devices and virtual system information.

## 4. Understanding Directories

A directory is a container used to organize files and other directories.

A directory can contain:

- Files
- Subdirectories
- Other directories

For example:

```text
home/
└── kali/
    ├── Documents/
    ├── Downloads/
    ├── Pictures/
    └── Projects/
```

Here, `kali` is a directory inside `home`, and it contains four other directories.

Directories make it easier to organize and locate information.

## 5. The Hierarchical Filesystem Structure

Linux uses a hierarchical structure that resembles an upside-down tree.

Example:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
│   └── kali
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sys
├── tmp
├── usr
└── var
```

The `/` directory is the starting point.

Every directory and file shown in this example exists somewhere beneath the root directory.

## 6. Important Linux Directories

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem hierarchy |
| `/home` | Contains normal users' home directories |
| `/root` | Home directory of the root account |
| `/etc` | System-wide configuration files |
| `/bin` | Essential user command programs; commonly merged into `/usr/bin` |
| `/sbin` | Essential system administration programs; commonly merged into `/usr/sbin` |
| `/boot` | Files needed for booting Linux |
| `/dev` | Device files |
| `/var` | Variable data such as logs and caches |
| `/tmp` | Temporary files |
| `/usr` | Applications, libraries, and shared resources |
| `/opt` | Optional third-party software |
| `/mnt` | Common temporary mount point |
| `/media` | Common mount point for removable media |
| `/proc` | Virtual filesystem exposing process and kernel information |
| `/sys` | Virtual filesystem exposing device and kernel information |

These directories have different responsibilities, and understanding them will help you navigate Linux effectively.

## 7. The Difference Between `/` and `/root`

These two paths are different.

**`/` – Root directory**

This is the highest directory in the Linux filesystem hierarchy.

**`/root` – Root user's home directory**

This is the personal home directory belonging to the root account.

Remember:

- `/` is the beginning of the filesystem.
- `/root` is a directory inside `/`.

## 8. Linux Filesystem vs Windows Filesystem

| Linux | Windows |
|---|---|
| Uses `/` as the hierarchy's starting point | Commonly uses drive letters such as `C:` |
| Uses `/home` for normal users' home directories | Uses `C:\Users` |
| Uses `/etc` for system configuration | Configuration is spread across several locations, including the Registry |
| Uses `/` as a path separator | Uses `\` as a path separator |
| Uses permissions and ownership to control access | Uses NTFS permissions and ownership |

## 9. Why Is the Linux Filesystem Important in Cybersecurity?

Understanding the filesystem is essential for ethical hacking and system security.

### A. Finding Configuration Files

Security professionals inspect configuration files to understand how systems are configured.

Example:

```bash
ls /etc
```

### B. Examining System Logs

Logs help security professionals investigate system activities and possible security incidents.

Example:

```bash
ls /var/log
```

### C. Understanding User Data

User directories may contain documents, scripts, and other personal files.

Example:

```bash
ls /home
```

### D. Investigating Running Processes

The `/proc` filesystem provides information about running processes and kernel details.

Example:

```bash
ls /proc
```

### E. Understanding Permissions

Filesystem permissions help protect sensitive files from unauthorized access.

We will study permissions in detail in Module 07.

## 10. Practical Laboratory

Perform these exercises inside your Kali Linux virtual machine.

### Exercise 1: Display the Root Directory

```bash
ls /
```

Observe the directories listed.

### Exercise 2: Display the Home Directory

```bash
ls /home
```

Identify your normal user's home directory.

### Exercise 3: Display System Configuration Files

```bash
ls /etc
```

Observe some of the configuration files and directories.

### Exercise 4: Display System Logs

```bash
ls /var/log
```

Identify some available log files.

### Exercise 5: Display the Current User

```bash
whoami
```

### Exercise 6: Display Your Current Directory

```bash
pwd
```

### Exercise 7: Display Files and Directories with Details

```bash
ls -lah /
```

Observe file permissions, ownership, sizes, and names.

## 11. Knowledge Check

Answer these questions:

1. What is a filesystem?
2. What is the Linux filesystem?
3. What is the root directory?
4. What is the difference between a file and a directory?
5. What does `/home` contain?
6. What is the purpose of `/etc`?
7. What is the difference between `/` and `/root`?
8. What information can be found inside `/var/log`?
9. Why is understanding the Linux filesystem important in cybersecurity?
10. What is the difference between the Linux and Windows filesystem structures?

## 12. Lesson Summary

In this lesson, we learned that the Linux filesystem organizes files and directories using a hierarchical structure.

We introduced the root directory, files, directories, and important system directories.

We also explored how filesystem knowledge helps cybersecurity professionals investigate system configurations, logs, user data, and running processes.

**Next Lesson:** Linux Directory Structure.