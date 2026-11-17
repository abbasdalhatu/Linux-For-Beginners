---
---

# Lesson 07: The `cd` Command

## 1. Introduction

In Linux, files and directories are organized in a hierarchical structure.

To work efficiently, you need to know how to move from one directory to another.

The `cd` command allows you to change your current working directory.

`cd` stands for **Change Directory**.

For example, if your current location is:

```text
/home/kali
```

You can move into the Documents directory using:

```bash
cd Documents
```

Your new location becomes:

```text
/home/kali/Documents
```

The `cd` command is one of the most important commands for Linux navigation.

## 2. Basic Syntax

The general syntax is:

```bash
cd [OPTIONS] [DIRECTORY]
```

Where:

- `cd` is the command.
- `OPTIONS` modify its behaviour.
- `DIRECTORY` specifies where you want to move.

Examples:

```bash
cd Documents
cd /etc
cd /var/log
```

**Important:** `cd` is normally a Bash shell builtin because it must change the working directory of the current shell itself.

An external program cannot normally change the working directory of its parent shell.

## 3. Moving into a Directory

Suppose your home directory contains:

```text
/home/kali/
├── Desktop/
├── Documents/
├── Downloads/
└── Projects/
```

If your current directory is `/home/kali`, execute:

```bash
cd Documents
```

To confirm your new location:

```bash
pwd
```

Expected output:

```text
/home/kali/Documents
```

You can also move into nested directories:

```bash
cd Projects/Linux
```

This assumes that the `Projects/Linux` directory exists relative to your current location.

## 4. Understanding Absolute and Relative Paths

The `cd` command supports both absolute and relative paths.

### 4.1 Absolute Path

An absolute path starts from the root directory `/`.

Example:

```bash
cd /var/log
```

It works regardless of your current location, provided the destination exists and you have permission to access it.

### 4.2 Relative Path

A relative path is interpreted from your current working directory.

Example:

```bash
cd Documents
```

If your current directory is `/home/kali`, Linux interprets this as:

```text
/home/kali/Documents
```

Relative paths are convenient when navigating nearby directories.

## 5. Important `cd` Shortcuts

There are several shortcuts you should understand.

### 5.1 Move to Your Home Directory

```bash
cd ~
```

The `~` symbol represents your home directory.

You can also simply execute:

```bash
cd
```

Without an argument, Bash normally moves to your home directory.

### 5.2 Move to the Root Directory

```bash
cd /
```

The root directory is the top-level directory of the Linux filesystem.

### 5.3 Move One Directory Back

```bash
cd ..
```

The `..` symbol represents the parent directory.

Example:

If you are currently in:

```text
/home/kali/Documents
```

Executing:

```bash
cd ..
```

moves you to:

```text
/home/kali
```

### 5.4 Move Two Levels Up

```bash
cd ../..
```

Each `..` moves one directory level upward.

For example:

```text
/home/kali/Projects/Linux
```

Executing:

```bash
cd ../..
```

moves you to:

```text
/home/kali
```

### 5.5 Return to the Previous Directory

```bash
cd -
```

This switches you to the previous working directory.

Example:

```bash
cd /etc
cd /var/log
cd -
```

The final command takes you back to `/etc`.

This is useful when switching repeatedly between two directories.

## 6. Understanding `.` and `..`

Linux uses special directory references.

| Symbol | Meaning |
|---|---|
| `.` | Current directory |
| `..` | Parent directory |
| `~` | Home directory |
| `/` | Root directory |
| `-` | Previous working directory when used with `cd` |

Examples:

```bash
cd .
cd ..
cd ~
cd /
cd -
```

`cd .` keeps you in the current directory.

## 7. Using Tab Completion

Linux terminals support tab completion.

Instead of typing an entire directory name, you can type part of it and press the **Tab** key.

For example:

```bash
cd Doc
```

Press Tab.

If the name is unambiguous, Bash may complete it to:

```bash
cd Documents
```

If multiple entries match, pressing Tab twice may display possible completions.

Tab completion helps you:

- Work faster.
- Reduce typing mistakes.
- Avoid errors with long directory names.
- Navigate directories with spaces and unusual characters more easily.

## 8. Handling Directory Names with Spaces

Some directory names contain spaces.

For example:

```text
My Projects
```

You cannot safely treat this as one directory name without quoting or escaping the space.

Use quotation marks:

```bash
cd "My Projects"
```

Or escape the space with a backslash:

```bash
cd My\ Projects
```

Both methods tell the shell to treat the name as one argument.

## 9. Practical Lab: Basic Directory Navigation

Perform these tasks in your Kali Linux virtual machine.

### Step 1: Display Your Current Location

```bash
pwd
```

### Step 2: Move to Your Home Directory

```bash
cd ~
pwd
```

### Step 3: Move to the Root Directory

```bash
cd /
pwd
```

### Step 4: Move into `/etc`

```bash
cd /etc
pwd
```

### Step 5: Move into `/var/log`

```bash
cd /var/log
pwd
```

### Step 6: Move One Level Up

```bash
cd ..
pwd
```

### Step 7: Return to Your Home Directory

```bash
cd
pwd
```

### Step 8: Test the Previous Directory Shortcut

```bash
cd /etc
cd /var/log
cd -
pwd
```

Observe where you end up.

## 10. Practical Lab: Build a Navigation Practice Environment

Create a directory structure for practising relative navigation.

### Step 1: Return Home

```bash
cd ~
```

### Step 2: Create the Directory Structure

```bash
mkdir -p ~/cd-lab/Projects/Linux
mkdir -p ~/cd-lab/Projects/Python
mkdir -p ~/cd-lab/Documents/Notes
```

### Step 3: Enter the Linux Directory

```bash
cd ~/cd-lab/Projects/Linux
pwd
```

### Step 4: Move to the Parent Directory

```bash
cd ..
pwd
```

You should now be in:

```text
/home/kali/cd-lab/Projects
```

### Step 5: Enter the Python Directory

```bash
cd Python
pwd
```

### Step 6: Move Two Levels Up

```bash
cd ../..
pwd
```

### Step 7: Return to the Previous Directory

```bash
cd -
pwd
```

### Step 8: Return Home

```bash
cd ~
```

You can keep this practice structure for future exercises or remove it when finished.

## 11. Common `cd` Errors

### Error 1: No Such File or Directory

Example:

```bash
cd Documents
```

Output:

```text
bash: cd: Documents: No such file or directory
```

Possible causes:

- The directory does not exist.
- You are in the wrong parent directory.
- You typed the directory name incorrectly.
- You used the wrong capitalization.

Check your current location:

```bash
pwd
```

List available entries:

```bash
ls
```

### Error 2: Permission Denied

Example:

```bash
cd /root
```

A normal user may receive a permission-denied error because `/root` is restricted.

Do not assume that every directory is accessible to every user.

### Error 3: Incorrect Use of Spaces

Incorrect:

```bash
cd My Projects
```

Correct:

```bash
cd "My Projects"
```

### Error 4: Confusing Root and Home Directories

These commands are different:

```bash
cd /
cd ~
```

- `/` takes you to the root directory.
- `~` takes you to your home directory.

## 12. Cybersecurity Importance of `cd`

The `cd` command is important in cybersecurity because security tools and Linux commands often operate relative to the current directory.

### 12.1 Navigating Security Tool Directories

You may need to navigate to directories containing scripts, configuration files, logs, or reports.

Example:

```bash
cd /var/log
```

### 12.2 Working with Investigation Evidence

During an authorised investigation, you may create a dedicated directory for collected evidence and reports.

Example:

```bash
mkdir -p ~/security-investigation
cd ~/security-investigation
```

This helps keep your work organized.

### 12.3 Avoiding Destructive Mistakes

Before executing file-modification commands, verify your current location:

```bash
pwd
ls -la
```

This reduces the risk of accidentally modifying files in the wrong directory.

### 12.4 Understanding Relative Paths in Scripts

A script that references:

```bash
./results/report.txt
```

uses a path relative to its current working directory.

Changing directories can therefore affect where the script looks for files.

**Security rule:** Always understand the current working directory before running commands that create, modify, or delete files.

## 13. Important Commands Summary

| Command | Purpose |
|---|---|
| `cd Documents` | Enter Documents |
| `cd /etc` | Enter `/etc` |
| `cd ~` | Move to home directory |
| `cd` | Move to home directory in Bash |
| `cd /` | Move to root directory |
| `cd ..` | Move one level up |
| `cd ../..` | Move two levels up |
| `cd -` | Return to previous directory |
| `cd .` | Stay in current directory |
| `pwd` | Display current working directory |
| `ls` | List directory contents |

## 14. Knowledge Check

Answer these questions in your notebook.

1. What does `cd` stand for?
2. What is the main purpose of the `cd` command?
3. What is the difference between `cd /` and `cd ~`?
4. What does `cd ..` do?
5. What does `cd -` do?
6. What is the difference between an absolute and a relative path?
7. Why is `cd` implemented as a shell builtin?
8. How can you enter a directory whose name contains spaces?
9. What does the Tab key do during directory navigation?
10. Why should cybersecurity professionals check their working directory before executing file operations?

## 15. Practical Assignment

Complete these tasks in Kali Linux.

1. Display your current working directory.
2. Move to your home directory.
3. Move to `/etc`.
4. Move to `/var/log`.
5. Move one directory level up.
6. Use `cd -` to return to the previous directory.
7. Create a directory named `navigation-lab`.
8. Inside it, create `Linux`, `Security`, and `Networking` directories.
9. Navigate between these directories using relative paths.
10. Practise moving two levels up using `cd ../..`.
11. Create a directory whose name contains a space and enter it using quotation marks.
12. Explain why `cd` is important when working with cybersecurity tools and scripts.

## 16. Lesson Conclusion

The `cd` command allows you to navigate the Linux filesystem.

You have learned how to use absolute paths, relative paths, special directory references, previous-directory navigation, and tab completion.

These skills are essential for Linux administration, scripting, and ethical hacking.

**Key lesson:** Understand where you are and where you are going before executing commands.

---

**End of Lesson 07 — The `cd` Command**