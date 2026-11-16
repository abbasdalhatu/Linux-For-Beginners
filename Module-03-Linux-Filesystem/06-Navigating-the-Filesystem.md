# LESSON 06: NAVIGATING THE FILESYSTEM

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Filesystem navigation means moving between directories and locating files in a Linux operating system.

As a Linux user, you must know how to move between directories, inspect their contents, and locate important files.

This skill is essential for system administration, Bash scripting, and ethical hacking.

## 2. Understanding the Current Working Directory

The current working directory is the directory you are currently operating in.

Linux provides a command called `pwd` to display this location.

Example:

```bash
pwd
```

Possible output:

```text
/home/kali
```

This means your current location is `/home/kali`.

## 3. The `cd` Command

The `cd` command means **Change Directory**.

It allows you to move from one directory to another.

### A. Move to the Root Directory

```bash
cd /
```

This takes you to the root directory.

Verify your location:

```bash
pwd
```

### B. Move to Your Home Directory

```bash
cd ~
```

The tilde represents your home directory.

### C. Move to a Specific Directory

```bash
cd /etc
```

This takes you directly to `/etc`.

### D. Move to the Parent Directory

```bash
cd ..
```

This moves one directory level upward.

### E. Return to the Previous Directory

```bash
cd -
```

This switches back to the previous working directory.

It also displays the directory you moved to.

## 4. The `ls` Command

The `ls` command lists files and directories.

### A. List Current Directory Contents

```bash
ls
```

### B. List Files in a Specific Directory

```bash
ls /etc
```

### C. Display Detailed Information

```bash
ls -l
```

The long listing format displays:

- File permissions
- Number of links
- Owner
- Group
- File size
- Modification date
- File or directory name

### D. Display Hidden Files

```bash
ls -a
```

Linux hidden filenames normally begin with a dot (`.`).

### E. Combine Options

```bash
ls -lah
```

This displays hidden entries, detailed information, and human-readable file sizes.

## 5. Understanding Directory Navigation

Consider this directory structure:

```text
/home/kali/
├── Desktop/
├── Documents/
│   └── Cybersecurity/
├── Downloads/
├── Pictures/
└── Projects/
```

If your current directory is `/home/kali`, you can enter Documents using:

```bash
cd Documents
```

To enter Cybersecurity:

```bash
cd Documents/Cybersecurity
```

To return to `/home/kali`:

```bash
cd ~
```

## 6. Using `tree` to Visualize Directories

The `tree` command displays directories in a tree-like structure.

Example:

```bash
tree ~
```

If the command is not installed, install it:

```bash
sudo apt update
sudo apt install tree
```

You can limit the displayed depth:

```bash
tree -L 2 ~
```

This displays the home directory and up to two directory levels beneath it.

## 7. The `file` Command

The `file` command identifies the type of a file by examining its contents.

Example:

```bash
file /etc/hostname
```

Another example:

```bash
file /bin/ls
```

This is useful because a filename extension does not always accurately identify the actual file type.

## 8. Useful Navigation Shortcuts

| Command | Purpose |
|---|---|
| `pwd` | Displays current directory |
| `cd /` | Moves to root directory |
| `cd ~` | Moves to home directory |
| `cd ..` | Moves one level upward |
| `cd -` | Returns to previous directory |
| `ls` | Lists directory contents |
| `ls -l` | Displays detailed listing |
| `ls -a` | Displays hidden entries |
| `ls -lah` | Displays detailed human-readable listing |
| `tree` | Displays directory structure |
| `file filename` | Identifies file type |

## 9. Why Navigation Matters in Cybersecurity

Filesystem navigation helps security professionals investigate systems.

Examples:

- Navigate to `/var/log` to inspect system logs.
- Navigate to `/etc` to examine configuration files.
- Navigate to `/home` to inspect user directory structures.
- Navigate to `/proc` to examine process and kernel information.
- Navigate to `/usr/bin` to inspect installed executable programs.

A security professional must know where information is stored before investigating it.

Always respect file permissions and authorization.

## 10. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: Display Your Current Location

```bash
pwd
```

Record the output.

### Exercise 2: Navigate to Root

```bash
cd /
pwd
```

Confirm your location.

### Exercise 3: Navigate to `/etc`

```bash
cd /etc
pwd
```

Observe the location.

### Exercise 4: List Directory Contents

```bash
ls
```

Then run:

```bash
ls -lah
```

Compare the results.

### Exercise 5: Navigate to `/var/log`

```bash
cd /var/log
pwd
ls
```

Observe the available log entries.

### Exercise 6: Return to Your Home Directory

```bash
cd ~
pwd
```

Confirm that you have returned home.

### Exercise 7: Use the Previous Directory Shortcut

```bash
cd /etc
cd /var/log
cd -
```

Observe the directory displayed.

### Exercise 8: Visualize Your Home Directory

```bash
tree -L 2 ~
```

If `tree` is unavailable, install it first.

### Exercise 9: Identify File Types

```bash
file /etc/hostname
file /bin/ls
file /etc/passwd
```

Observe the descriptions.

## 11. Knowledge Check

1. What does filesystem navigation mean?
2. What is the purpose of `pwd`?
3. What does the `cd` command do?
4. What is the difference between `cd ~` and `cd /`?
5. What does `cd ..` do?
6. What does `cd -` do?
7. What is the purpose of `ls -lah`?
8. What is the purpose of the `tree` command?
9. What does the `file` command identify?
10. Why is filesystem navigation important in cybersecurity?

## 12. Lesson Summary

In this lesson, we learned how to navigate the Linux filesystem using `cd`, `pwd`, and `ls`.

We also explored directory visualization with `tree` and file identification with `file`.

These commands form the foundation of everyday Linux command-line work.

**Next Lesson:** Understanding File Types.