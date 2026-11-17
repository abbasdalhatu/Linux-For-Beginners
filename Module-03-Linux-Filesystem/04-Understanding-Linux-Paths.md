---
---

# LESSON 04: UNDERSTANDING LINUX PATHS

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

A path is the location of a file or directory within a filesystem.

Think of a path as a home address. Just as an address tells you where someone lives, a Linux path tells the operating system where a file or directory is located.

For example:

```text
/home/kali/Documents/notes.txt
```

This path identifies a file called `notes.txt` inside the `Documents` directory of the `kali` user's home directory.

Understanding paths is essential for navigating Linux, managing files, writing Bash scripts, and performing cybersecurity investigations.

## 2. What Is a Linux Path?

A Linux path describes the location of a file or directory.

Linux uses the forward slash `/` to separate directory names.

Example:

```text
/home/kali/Downloads/file.txt
```

Breakdown:

- `/` – Root directory.
- `home` – User directory location.
- `kali` – User's home directory.
- `Downloads` – Downloads directory.
- `file.txt` – File name.

Each component helps identify the file's location.

## 3. Understanding Path Separators

Linux uses:

```text
/
```

Windows commonly uses:

```text
\
```

Examples:

**Linux:**

```text
/home/kali/Documents/report.txt
```

**Windows:**

```text
C:\Users\Student\Documents\report.txt
```

Do not confuse the two separators when working with commands or scripts.

## 4. Understanding Parent and Child Directories

Directories can contain other directories.

Example:

```text
/home/kali/
└── Documents/
    └── Cybersecurity/
        └── notes.txt
```

Here:

- `kali` is the parent of `Documents`.
- `Documents` is the parent of `Cybersecurity`.
- `Cybersecurity` is the parent of `notes.txt`'s containing directory.

A parent directory is the directory immediately above another directory in the hierarchy.

A child directory is a directory contained inside another directory.

## 5. Understanding Special Path Symbols

Linux provides special symbols that make navigating the filesystem easier.

### A. Forward Slash `/`

Represents the root directory.

```bash
cd /
```

### B. Tilde `~`

Represents the current user's home directory.

```bash
cd ~
```

### C. Single Dot `.`

Represents the current working directory.

Example:

```bash
ls .
```

This lists the contents of the current directory.

### D. Double Dot `..`

Represents the parent directory.

Example:

```bash
cd ..
```

This moves one directory level upward.

### E. Multiple Parent References

You can use multiple `..` components to move several levels upward.

Example:

```bash
cd ../../
```

This moves two directory levels upward, provided those levels exist.

## 6. Displaying Your Current Path

The `pwd` command means **Print Working Directory**.

It displays the full path of your current location.

Example:

```bash
pwd
```

Possible output:

```text
/home/kali
```

This tells you that your current working directory is `/home/kali`.

## 7. Listing Files Using Paths

The `ls` command can display files and directories at specific locations.

Examples:

```bash
ls /
ls /home
ls /etc
ls /var/log
```

You do not need to enter a directory before listing its contents.

You can also use:

```bash
ls -lah /home/kali
```

This displays detailed information, including hidden entries.

## 8. Accessing Files Using Paths

You can access a file directly by specifying its path.

Example:

```bash
cat /etc/hostname
```

This displays the contents of the system hostname file.

Another example:

```bash
cat /etc/os-release
```

This displays information about the installed Linux distribution.

## 9. Why Linux Paths Matter in Cybersecurity

Understanding paths helps security professionals locate important files and investigate system activity.

Examples:

| Path | Security relevance |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Protected password hash information |
| `/var/log` | System and application logs |
| `/home` | User files and directories |
| `/proc` | Process and kernel information |
| `/usr/bin` | Installed executable programs |

Some files contain sensitive information. Access them only when authorized.

## 10. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: Display Your Current Path

```bash
pwd
```

Write down the output.

### Exercise 2: Move to the Root Directory

```bash
cd /
pwd
```

Confirm that the output is `/`.

### Exercise 3: Move to Your Home Directory

```bash
cd ~
pwd
```

Confirm that you are in your personal home directory.

### Exercise 4: Move One Level Up

```bash
cd ..
pwd
```

Observe how your location changes.

### Exercise 5: Use the Current Directory Symbol

```bash
ls .
```

Explain what the dot represents.

### Exercise 6: Use the Parent Directory Symbol

```bash
ls ..
```

Observe the contents of the parent directory.

### Exercise 7: Access a File Using Its Full Path

```bash
cat /etc/hostname
```

Explain why you do not need to navigate to `/etc` first.

### Exercise 8: Display Distribution Information

```bash
cat /etc/os-release
```

Identify the distribution name and version information.

## 11. Knowledge Check

1. What is a path in Linux?
2. Which symbol separates directory names in Linux paths?
3. What does `/` represent?
4. What does `~` represent?
5. What does `.` represent?
6. What does `..` represent?
7. What is the purpose of `pwd`?
8. What is the difference between a parent and child directory?
9. Give an example of a Linux file path.
10. Why are Linux paths important in cybersecurity?

## 12. Lesson Summary

A Linux path identifies the location of a file or directory.

We learned about path separators, parent and child directories, and special symbols such as `/`, `~`, `.`, and `..`.

We also practiced using paths to navigate the filesystem and access system information.

**Next Lesson:** Absolute and Relative Paths.