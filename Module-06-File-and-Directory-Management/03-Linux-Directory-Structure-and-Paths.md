---
---

# MODULE 06 – FILE AND DIRECTORY MANAGEMENT

## LESSON 03: LINUX DIRECTORY STRUCTURE AND PATHS

### 1. Introduction

Linux uses a hierarchical filesystem structure to organise files and directories.

Unlike Windows, which commonly uses drive letters such as `C:\` and `D:\`, Linux organises its filesystem from one main starting point called the **root directory**.

The root directory is represented by:

```bash
/
```

Every file and directory in a typical Linux filesystem exists under this root hierarchy.

Understanding this structure is important because Linux users, administrators, and cybersecurity professionals regularly navigate system directories to locate configuration files, inspect logs, manage applications, and investigate security issues.

### 2. Understanding the Linux Root Directory

The root directory `/` is the highest-level directory in the Linux filesystem.

Display its contents using:

```bash
ls /
```

You may see directories such as:

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
tmp
usr
var
```

Each directory has a particular purpose.

### 3. Important Linux Directories

#### A. `/bin` – Essential User Commands

This directory traditionally contains essential commands needed for basic system operation.

Examples include:

```text
ls
cp
mv
cat
```

On modern Kali and Debian-based systems, `/bin` may be a symbolic link to `/usr/bin`.

#### B. `/boot` – Boot Files

Contains files required during the system boot process.

Examples include:

- Linux kernel images
- Initial RAM filesystem images
- Bootloader-related files

Inspect it with:

```bash
ls /boot
```

#### C. `/dev` – Device Files

Contains device interfaces represented as filesystem objects.

Examples include:

```text
/dev/null
/dev/zero
/dev/tty
```

Inspect it with:

```bash
ls /dev
```

#### D. `/etc` – Configuration Files

Contains system-wide configuration files.

Examples include:

```text
/etc/passwd
/etc/hostname
/etc/hosts
/etc/fstab
```

Inspect it with:

```bash
ls /etc
```

Cybersecurity importance:

Security professionals may inspect configuration files to understand system settings, user accounts, and network configuration.

Do not modify system configuration files during basic practice.

#### E. `/home` – User Home Directories

Contains personal directories belonging to ordinary users.

For example:

```text
/home/kali
/home/student
```

The home directory stores personal files, downloads, documents, and projects.

Display your home directory using:

```bash
echo $HOME
```

#### F. `/lib` – Essential Shared Libraries

Contains essential shared libraries and kernel-related components needed by system programs.

On modern systems, `/lib` may be linked to a corresponding directory under `/usr`.

#### G. `/media` – Removable Media Mount Points

Commonly used for automatically mounted removable storage devices.

Examples include USB drives and removable media.

#### H. `/mnt` – Temporary Mount Point

Traditionally used by administrators to mount filesystems temporarily.

#### I. `/opt` – Optional Software

Often used for additional software packages that are installed outside the main distribution-managed application layout.

#### J. `/proc` – Process and Kernel Information

A virtual filesystem that exposes information about running processes and the kernel.

For example:

```bash
ls /proc
```

Inspect CPU information:

```bash
cat /proc/cpuinfo
```

Inspect memory information:

```bash
cat /proc/meminfo
```

Important: `/proc` does not behave like an ordinary directory containing permanent files. Much of its content is generated dynamically by the kernel.

#### K. `/root` – Root User's Home Directory

The home directory of the root administrator account.

It is different from `/`, which is the filesystem root.

#### L. `/run` – Runtime Data

Contains temporary runtime information about the system since boot.

#### M. `/sbin` – System Administration Commands

Traditionally contains important system administration utilities.

On modern distributions, `/sbin` may be linked to `/usr/sbin`.

#### N. `/tmp` – Temporary Files

Used by applications and users to store temporary data.

Files in this directory may be removed automatically according to system policies.

Do not store important permanent information here.

#### O. `/usr` – User Applications and Resources

Contains many user-space programs, libraries, documentation, and shared resources.

Common subdirectories include:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/share
/usr/local
```

#### P. `/var` – Variable Data

Stores data that changes during system operation.

Examples include:

```text
/var/log
/var/cache
/var/tmp
/var/lib
```

The `/var/log` directory is particularly important in cybersecurity because it commonly contains system and application logs.

Inspect it using:

```bash
ls /var/log
```

### 4. Linux Directory Structure Summary

| Directory | Main Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/bin` | Essential user commands |
| `/boot` | Boot files |
| `/dev` | Device interfaces |
| `/etc` | System configuration |
| `/home` | Ordinary users' home directories |
| `/lib` | Essential libraries |
| `/media` | Removable media mount points |
| `/mnt` | Temporary mount point |
| `/opt` | Optional software |
| `/proc` | Process and kernel information |
| `/root` | Root user's home directory |
| `/run` | Runtime system data |
| `/sbin` | System administration commands |
| `/tmp` | Temporary files |
| `/usr` | Applications and shared resources |
| `/var` | Variable data and logs |

### 5. Understanding Linux Paths

A path tells Linux where a file or directory is located.

There are two main types of paths:

1. Absolute paths
2. Relative paths

#### A. Absolute Paths

An absolute path gives the complete location of a file or directory, starting from `/`.

Example:

```text
/home/kali/Documents/notes.txt
```

Another example:

```text
/etc/hostname
```

Absolute paths do not depend on your current working directory.

You can display your current directory using:

```bash
pwd
```

#### B. Relative Paths

A relative path describes a location in relation to your current working directory.

For example, if your current directory is:

```text
/home/kali
```

Then:

```bash
cd Documents
```

moves you into:

```text
/home/kali/Documents
```

Relative paths depend on your current location.

### 6. Special Path Symbols

Linux uses special symbols to represent common directory locations.

| Symbol | Meaning |
|---|---|
| `/` | Root directory |
| `~` | Current user's home directory |
| `.` | Current directory |
| `..` | Parent directory |
| `-` | Previous working directory when used with `cd -` |

Examples:

Display your home directory:

```bash
cd ~
```

Move into the parent directory:

```bash
cd ..
```

Return to the previous directory:

```bash
cd -
```

Display your current directory:

```bash
pwd
```

### 7. Understanding the Difference Between `/` and `/root`

These two paths are different.

`/` represents the root of the entire filesystem.

`/root` is the home directory of the root administrator account.

Example:

```bash
cd /
pwd
```

Output:

```text
/
```

Now:

```bash
cd /root
pwd
```

If your account has permission to access it, the output will be:

```text
/root
```

An ordinary user may not have permission to enter `/root`.

### 8. Navigating the Filesystem

Practise these commands:

Display the current directory:

```bash
pwd
```

List the root directory:

```bash
ls /
```

List the configuration directory:

```bash
ls /etc
```

Enter your home directory:

```bash
cd ~
```

Enter the `/tmp` directory:

```bash
cd /tmp
```

Return to your home directory:

```bash
cd ~
```

Display your current location:

```bash
pwd
```

### 9. Cybersecurity Applications

Understanding Linux directories is essential in cybersecurity.

**A. Configuration investigation**

The `/etc` directory contains configuration files that can help an analyst understand system settings.

**B. Log investigation**

The `/var/log` directory commonly stores logs that help identify suspicious activity.

**C. Process investigation**

The `/proc` filesystem exposes information about running processes and kernel activity.

**D. User investigation**

The `/home` directory contains ordinary users' personal directories.

**E. System investigation**

Directories such as `/boot`, `/dev`, `/usr`, and `/var` help analysts understand how the Linux operating system is organised.

### 10. Important Safety Notes

1. Do not delete files from system directories.
2. Do not modify `/etc` configuration files unless you understand the consequences.
3. Do not experiment with writing to device files inside `/dev`.
4. Remember that `/proc` and `/sys` expose virtual system information.
5. Always check your current directory before performing file operations.

### 11. Knowledge Check

Answer these questions in your notebook.

1. What is the Linux root directory?
2. What is the difference between `/` and `/root`?
3. What is the purpose of `/etc`?
4. What type of information can be found in `/var/log`?
5. What is the purpose of `/home`?
6. What does the `/proc` directory contain?
7. Explain the difference between an absolute path and a relative path.
8. What does `~` represent?
9. What is the difference between `.` and `..`?
10. Why is understanding the Linux directory structure important in cybersecurity?

### 12. Practical Exercises

Perform these exercises in your Kali Linux VM.

**Exercise 1:** Display the root directory contents.

```bash
ls /
```

**Exercise 2:** Display the contents of `/etc`.

```bash
ls /etc
```

**Exercise 3:** Display your home directory.

```bash
echo $HOME
```

**Exercise 4:** Display your current working directory.

```bash
pwd
```

**Exercise 5:** Navigate to `/tmp` using an absolute path.

```bash
cd /tmp
```

**Exercise 6:** Return to your home directory using `~`.

```bash
cd ~
```

**Exercise 7:** Display the contents of `/var/log`.

```bash
ls /var/log
```

**Exercise 8:** Inspect your system's memory information.

```bash
cat /proc/meminfo
```

**Exercise 9:** Create a directory inside your home directory called `directory-practice`.

```bash
mkdir -p ~/directory-practice
```

**Exercise 10:** Enter the directory and display its absolute path.

```bash
cd ~/directory-practice
pwd
```

### 13. Lesson Summary

In this lesson, you learned:

- The Linux hierarchical filesystem.
- The purpose of important system directories.
- The difference between `/` and `/root`.
- Absolute and relative paths.
- Special path symbols such as `~`, `.`, and `..`.
- How to navigate Linux directories.
- Why directory knowledge is important for cybersecurity investigations.

**Next Lesson:** Creating Directories with `mkdir`.

**End of Lesson 03.**