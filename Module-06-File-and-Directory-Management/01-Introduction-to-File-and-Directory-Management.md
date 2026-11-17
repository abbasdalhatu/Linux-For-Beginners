---
---

# MODULE 06 – FILE AND DIRECTORY MANAGEMENT

## LESSON 01: INTRODUCTION TO FILE AND DIRECTORY MANAGEMENT

### 1. Introduction

File and directory management is one of the most important skills in Linux system administration.

Linux stores information in files and organises those files into directories. As a Linux user, system administrator, or ethical hacker, you must understand how to create, locate, organise, copy, move, inspect, and remove files.

For example, during an ethical hacking exercise, you may need to:

- Create a directory for a penetration-testing project.
- Save network scan results into files.
- Organise captured information.
- Copy configuration files for backup.
- Inspect log files.
- Remove temporary files after completing a lab.

All these activities require file and directory management.

### 2. What Is a File?

A file is a named collection of information stored on a computer.

A file may contain:

- Text
- Images
- Videos
- Audio
- Source code
- System configuration
- Application data
- Logs and reports

Examples of Linux files:

```text
notes.txt
report.pdf
script.sh
config.conf
network-scan.log
```

Each file has information associated with it, such as:

- Filename
- File type
- Size
- Permissions
- Owner
- Group
- Timestamps

### 3. What Is a Directory?

A directory is a location used to organise and contain files and other directories.

A directory is similar to a folder in Windows.

For example:

```text
home/
└── kali/
    ├── Documents/
    │   └── notes.txt
    ├── Downloads/
    │   └── report.pdf
    └── Projects/
        └── network-lab/
            └── results.txt
```

In this example:

- `home` is a directory.
- `kali` is inside `home`.
- `Documents`, `Downloads`, and `Projects` are directories.
- `notes.txt`, `report.pdf`, and `results.txt` are files.

Directories help users organise information logically.

### 4. Difference Between Files and Directories

| Feature | File | Directory |
|---|---|---|
| Purpose | Stores information | Organises files and directories |
| Example | `notes.txt` | `Documents` |
| Can contain files? | Normally no | Yes |
| Can contain directories? | No | Yes |
| Linux representation | File type `-` for a regular file | File type `d` |

To identify file types, Linux provides commands such as:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 kali kali 1200 Nov 14 10:00 notes.txt
drwxr-xr-x 2 kali kali 4096 Nov 14 10:05 Documents
```

The first character identifies the type:

- `-` = Regular file
- `d` = Directory
- `l` = Symbolic link

We will explore file types in the next lesson.

### 5. Why File and Directory Management Matters

File management is important for several reasons.

**A. Organisation**

It allows users to arrange information into meaningful folders.

**B. Data protection**

Copying important files creates backups that can help prevent data loss.

**C. System administration**

Administrators manage configuration files, application files, logs, and system directories.

**D. Cybersecurity**

Security professionals inspect logs, organise investigation evidence, and protect sensitive information.

**E. Automation**

Linux scripts frequently create directories, generate reports, move files, and process information automatically.

### 6. Windows vs Linux File Management

Both Windows and Linux allow users to manage files, but their command-line environments differ.

| Activity | Windows CMD | Linux |
|---|---|---|
| Show current directory | `cd` | `pwd` |
| List files | `dir` | `ls` |
| Create directory | `mkdir` | `mkdir` |
| Create empty file | `type nul > file.txt` | `touch file.txt` |
| Copy file | `copy` | `cp` |
| Move file | `move` | `mv` |
| Delete file | `del` | `rm` |
| Remove directory | `rmdir` | `rmdir` |

Linux also provides powerful tools for searching, filtering, and managing large numbers of files.

### 7. Important Linux File Management Commands

We will study these commands throughout this module.

| Command | Purpose |
|---|---|
| `mkdir` | Creates directories |
| `touch` | Creates empty files or updates timestamps |
| `cp` | Copies files and directories |
| `mv` | Moves or renames files and directories |
| `rm` | Removes files and directories |
| `rmdir` | Removes empty directories |
| `cat` | Displays or combines file contents |
| `less` | Views file contents page by page |
| `more` | Views file contents page by page |
| `head` | Displays the beginning of a file |
| `tail` | Displays the end of a file |
| `file` | Identifies file types |
| `stat` | Displays detailed file information |

Do not try to memorise every command immediately. We will practise each one individually.

### 8. Practical Demonstration

Open your Kali Linux virtual machine.

Check your current directory:

```bash
pwd
```

List its contents:

```bash
ls
```

Create a directory for this module's practical exercises:

```bash
mkdir -p ~/module06-lab
```

Enter the directory:

```bash
cd ~/module06-lab
```

Confirm your location:

```bash
pwd
```

You should see a path ending in:

```text
/module06-lab
```

Now create a subdirectory:

```bash
mkdir files
```

List the contents:

```bash
ls
```

You should see:

```text
files
```

You have now created your first directory structure for this module.

### 9. Important Safety Rules

When managing files in Linux, remember:

1. Always confirm your current directory before performing file operations.
2. Understand a command before executing it.
3. Avoid using `rm -rf` carelessly because it can permanently remove large amounts of data.
4. Keep backups of important files.
5. Do not modify system configuration files unless you understand the consequences.
6. Practise destructive operations only inside your dedicated lab directory.

### 10. Knowledge Check

Answer these questions in your notebook.

1. What is a file?
2. What is a directory?
3. State four differences between files and directories.
4. Why is file management important in Linux?
5. Give five examples of file-management commands.
6. What does the first character in `ls -l` output represent?
7. Explain two ways file management is useful in cybersecurity.
8. What is the difference between Windows CMD file management and Linux file management?

### 11. Practical Exercises

Complete the following tasks in your Kali Linux VM.

**Exercise 1:** Display your current working directory.

**Exercise 2:** List all files and directories in your home directory.

**Exercise 3:** Create a directory named `module06-lab` if it does not already exist.

**Exercise 4:** Enter the directory and create another directory named `documents`.

**Exercise 5:** Display the directory structure using:

```bash
ls -R ~/module06-lab
```

### 12. Lesson Summary

In this lesson, you learned:

- What files are.
- What directories are.
- How Linux organises information.
- The difference between files and directories.
- Why file management is important in system administration and cybersecurity.
- Common Linux file-management commands.
- Basic safety practices when working with files.

**Next Lesson:** Understanding Linux File Types.

**End of Lesson 01.**