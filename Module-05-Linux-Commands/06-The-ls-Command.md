# Lesson 06: The `ls` Command

## 1. Introduction

Linux organizes files and directories in a hierarchical filesystem.

When working in Linux, you often need to know:

- What files exist in a directory.
- What directories are available.
- Which files are hidden.
- Who owns a file.
- What permissions a file has.
- When a file was last modified.

The `ls` command helps us inspect directory contents.

`ls` stands for **List**.

It is one of the most frequently used Linux commands.

## 2. Basic Syntax

The general syntax is:

```bash
ls [OPTIONS] [FILE_OR_DIRECTORY]
```

Where:

- `ls` is the command.
- `OPTIONS` modify how the output is displayed.
- `FILE_OR_DIRECTORY` specifies what you want to inspect.

To list the contents of your current directory:

```bash
ls
```

Example output:

```text
Desktop  Documents  Downloads  Music  Pictures  Public
```

Your output may differ depending on the files and directories in your system.

## 3. Listing a Specific Directory

You do not need to enter a directory before listing its contents.

For example:

```bash
ls /etc
```

This displays the contents of `/etc`.

You can also inspect:

```bash
ls /home
ls /var
ls /usr
```

This is useful when exploring a Linux filesystem.

## 4. Understanding Important `ls` Options

The `ls` command supports several useful options.

### 4.1 Long Listing: `ls -l`

```bash
ls -l
```

This displays detailed information about files and directories.

Example:

```text
-rw-r--r-- 1 kali kali 1200 Nov 14 10:30 notes.txt
drwxr-xr-x 2 kali kali 4096 Nov 14 10:35 Documents
```

Let's understand the output:

| Field | Meaning |
|---|---|
| `-rw-r--r--` | File type and permissions |
| `1` | Number of hard links |
| `kali` | Owner |
| `kali` | Group |
| `1200` | File size in bytes |
| `Nov 14 10:30` | Last modification time |
| `notes.txt` | File name |

The permission field will be studied in greater detail in the Linux permissions module.

### 4.2 Show Hidden Files: `ls -a`

Linux treats filenames beginning with a dot (`.`) as hidden by convention.

Examples:

```text
.bashrc
.profile
.ssh
```

To display hidden files:

```bash
ls -a
```

This also displays:

- `.` representing the current directory.
- `..` representing the parent directory.

### 4.3 Detailed Listing Including Hidden Files: `ls -la`

```bash
ls -la
```

This combines:

- `-l`: detailed information.
- `-a`: include hidden files.

It is particularly useful when inspecting configuration files.

### 4.4 Human-Readable File Sizes: `ls -lh`

```bash
ls -lh
```

This displays file sizes in easier-to-read units.

Example:

```text
-rw-r--r-- 1 kali kali 1.5K Nov 14 10:30 notes.txt
```

Instead of displaying only bytes, the output may use units such as:

- K: Kilobytes
- M: Megabytes
- G: Gigabytes

### 4.5 Sort by Modification Time: `ls -lt`

```bash
ls -lt
```

This sorts files by modification time, with the most recently modified entries first.

To reverse the order:

```bash
ls -ltr
```

### 4.6 Reverse the Listing: `ls -r`

```bash
ls -r
```

This reverses the normal sorting order.

### 4.7 Sort by File Size: `ls -lS`

```bash
ls -lS
```

This displays files in descending size order.

The largest files appear first.

To reverse the order:

```bash
ls -lSr
```

### 4.8 Display Directories with a Trailing Slash: `ls -F`

```bash
ls -F
```

This adds indicators to entries.

Common indicators include:

| Indicator | Meaning |
|---|---|
| `/` | Directory |
| `*` | Executable file |
| `@` | Symbolic link |
| `|` | Named pipe |

The exact indicators depend on the file type and system.

## 5. Understanding File Types in `ls -l`

The first character in the long listing identifies the file type.

Common file types include:

| Character | File type |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |
| `p` | Named pipe |
| `s` | Unix domain socket |

Example:

```text
-rw-r--r-- 1 kali kali 500 notes.txt
drwxr-xr-x 2 kali kali 4096 Documents
lrwxrwxrwx 1 kali kali 10 shortcut -> notes.txt
```

Notice:

- `notes.txt` begins with `-`.
- `Documents` begins with `d`.
- `shortcut` begins with `l`.

This helps you identify what kind of filesystem object you are inspecting.

## 6. Combining `ls` Options

Linux allows you to combine multiple options.

Examples:

```bash
ls -lah
```

Displays hidden files with detailed information and human-readable sizes.

```bash
ls -ltr
```

Displays detailed information sorted by modification time in reverse order.

```bash
ls -lSh
```

Displays detailed information sorted by file size, with human-readable sizes.

```bash
ls -la /etc
```

Displays all entries, including hidden ones, in `/etc`.

Understanding option combinations makes Linux administration faster.

## 7. Inspecting Files and Directories

You can use `ls` to inspect individual files and directories.

For example:

```bash
ls -l notes.txt
```

This displays detailed information about one file.

To inspect a directory itself rather than its contents:

```bash
ls -ld Documents
```

The `-d` option tells `ls` to list the directory entry itself.

This distinction is useful when checking directory ownership and permissions.

## 8. Practical Lab: Exploring Your Linux Filesystem

Perform these exercises in your Kali Linux virtual machine.

### Step 1: Display Your Current Location

```bash
pwd
```

### Step 2: List Your Current Directory

```bash
ls
```

### Step 3: Display Hidden Files

```bash
ls -a
```

Look for configuration files such as `.bashrc`.

### Step 4: Display Detailed Information

```bash
ls -l
```

Observe the permissions, owner, group, size, and modification time.

### Step 5: Combine Options

```bash
ls -lah
```

Study the output carefully.

### Step 6: Inspect the `/etc` Directory

```bash
ls /etc
```

Then:

```bash
ls -lah /etc
```

Observe the types of configuration files present.

### Step 7: Sort by File Size

```bash
ls -lSh
```

### Step 8: Sort by Modification Time

```bash
ls -lt
```

## 9. Practical Lab: Create Your Own Test Environment

We will create a small directory to practise listing files.

### Step 1: Create a Practice Directory

```bash
mkdir -p ~/ls-lab
```

### Step 2: Enter the Directory

```bash
cd ~/ls-lab
```

### Step 3: Create Test Files

```bash
touch file1.txt file2.txt file3.txt
```

### Step 4: Create a Hidden File

```bash
touch .hidden-file
```

### Step 5: Create a Subdirectory

```bash
mkdir Documents
```

### Step 6: List the Contents

```bash
ls
```

Notice that the hidden file is not displayed.

### Step 7: Display Everything

```bash
ls -a
```

You should now see `.hidden-file`.

### Step 8: Display Detailed Information

```bash
ls -lah
```

### Step 9: Inspect the Directory Itself

```bash
ls -ld Documents
```

### Step 10: Clean Up

When you have finished the lab, return home:

```bash
cd ~
```

You may remove the practice directory with:

```bash
rm -r ~/ls-lab
```

**Safety note:** The cleanup command permanently removes the specified practice directory and its contents. Verify the path before executing it. Never use this command on an unfamiliar or important directory.

## 10. Common `ls` Mistakes

### Mistake 1: Expecting Hidden Files to Appear

The normal command:

```bash
ls
```

does not display hidden files.

Use:

```bash
ls -a
```

### Mistake 2: Confusing File Size with Disk Usage

The size displayed by `ls -l` is the file's apparent size in bytes.

It is not necessarily the exact amount of disk space occupied by the file.

### Mistake 3: Assuming `ls -l` Shows Directory Contents Recursively

`ls -l` displays detailed information about entries in the specified directory.

To list directory contents recursively, use:

```bash
ls -R
```

Be careful when using recursive listing on large filesystem locations because the output can become very long.

### Mistake 4: Forgetting That Linux Is Case-Sensitive

These are different names:

```text
Report.txt
report.txt
REPORT.txt
```

Linux generally treats them as separate filenames.

## 11. Cybersecurity Importance of `ls`

The `ls` command is valuable during security assessments and Linux administration.

### 11.1 Inspecting Configuration Files

```bash
ls -lah /etc
```

This helps you identify configuration files and hidden entries.

### 11.2 Inspecting SSH Configuration

```bash
ls -lah ~/.ssh
```

This can help you identify SSH-related files.

Do not expose or share private keys.

### 11.3 Checking Permissions

```bash
ls -l
```

This helps you identify ownership and permission settings.

### 11.4 Finding Unusual Files

During an authorised investigation, you may inspect directories for unexpected files, symbolic links, or executables.

### 11.5 Investigating Large Files

```bash
ls -lhS
```

This can help identify unusually large files in a directory.

Remember that the presence of an unfamiliar file does not automatically mean it is malicious. Investigate it using additional evidence.

## 12. Important Commands Summary

| Command | Purpose |
|---|---|
| `ls` | List directory contents |
| `ls -l` | Detailed listing |
| `ls -a` | Include hidden entries |
| `ls -la` | Detailed listing including hidden entries |
| `ls -lh` | Human-readable file sizes |
| `ls -lt` | Sort by modification time |
| `ls -ltr` | Reverse modification-time order |
| `ls -lS` | Sort by file size |
| `ls -r` | Reverse listing order |
| `ls -F` | Add file-type indicators |
| `ls -ld` | Display directory information |
| `ls -R` | List contents recursively |

## 13. Knowledge Check

Answer these questions in your notebook.

1. What does the `ls` command do?
2. What is the purpose of `ls -l`?
3. Why do hidden files begin with a dot?
4. Which option displays hidden files?
5. What is the difference between `ls -la` and `ls -lh`?
6. What does the first character in `ls -l` output represent?
7. What does the character `d` represent?
8. What does `ls -lt` do?
9. How can you inspect a directory itself rather than its contents?
10. Why is `ls -lah` useful during a security investigation?

## 14. Practical Assignment

Complete the following tasks in Kali Linux.

1. Display the contents of your home directory.
2. Display all hidden files in your home directory.
3. Display detailed information about `/etc`.
4. Display file sizes in human-readable format.
5. Sort a directory listing by file size.
6. Sort a directory listing by modification time.
7. Create a directory named `security-practice`.
8. Create three text files inside it.
9. Create one hidden file.
10. Use `ls -lah` to inspect everything.
11. Use `ls -ld` to inspect the `security-practice` directory itself.
12. Explain three ways `ls` can help a cybersecurity professional.

## 15. Lesson Conclusion

The `ls` command is an essential Linux filesystem inspection tool.

It helps users explore directories, identify hidden files, inspect permissions, view file sizes, and understand filesystem contents.

For cybersecurity professionals, `ls` is especially useful when examining configuration directories, investigating files, and checking permissions.

**Key lesson:** Before working with files, learn how to inspect them properly.

---

**End of Lesson 06 — The `ls` Command**