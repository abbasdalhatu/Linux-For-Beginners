# LESSON 05: ABSOLUTE AND RELATIVE PATHS

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

In Linux, there are two main ways to specify the location of a file or directory:

1. Absolute paths
2. Relative paths

Both methods help us locate files, but they work differently.

Understanding them is important when navigating Linux, managing files, writing Bash scripts, and performing cybersecurity tasks.

## 2. What Is an Absolute Path?

An absolute path specifies the complete location of a file or directory starting from the root directory `/`.

It does not depend on your current working directory.

Example:

```text
/home/kali/Documents/notes.txt
```

This is an absolute path because it begins with `/` and identifies the complete location of the file.

Other examples:

```text
/etc/passwd
/var/log/syslog
/usr/bin/python3
/home/kali/Downloads
```

No matter which directory you are currently working in, these paths refer to the same locations.

### Using an absolute path

Run:

```bash
cat /etc/hostname
```

This displays the hostname file without requiring you to navigate into `/etc`.

You can also navigate directly:

```bash
cd /home/kali/Documents
```

## 3. What Is a Relative Path?

A relative path specifies the location of a file or directory in relation to your current working directory.

It does not begin with the root directory `/`.

Example:

```text
Documents/notes.txt
```

This path means that `notes.txt` is inside a `Documents` directory located under your current working directory.

Its meaning depends on where you are when you execute the command.

### Example

Suppose your current directory is:

```text
/home/kali
```

You can access a file using:

```bash
cat Documents/notes.txt
```

The complete location is:

```text
/home/kali/Documents/notes.txt
```

The relative path is:

```text
Documents/notes.txt
```

## 4. Understanding the Difference

| Feature | Absolute Path | Relative Path |
|---|---|---|
| Starting point | Root directory `/` | Current working directory |
| Begins with `/` | Yes | No |
| Depends on current location | No | Yes |
| Example | `/home/kali/file.txt` | `file.txt` |
| Common use | Accessing known system locations | Working within a project directory |

## 5. Understanding Relative Path Symbols

Relative paths often use special symbols.

### A. Current Directory `.`

The single dot represents the current working directory.

Example:

```bash
ls .
```

You can also execute a script in the current directory using:

```bash
./script.sh
```

The `./` tells Linux to look for the script in the current directory.

### B. Parent Directory `..`

The double dot represents the parent directory.

Example:

```bash
cd ..
```

To access a file in the parent directory:

```bash
cat ../notes.txt
```

### C. Moving Multiple Levels

You can combine multiple parent directory references.

Example:

```bash
cd ../../
```

This moves two directory levels upward.

Another example:

```bash
cat ../../notes.txt
```

This attempts to access `notes.txt` two levels above your current directory.

## 6. Practical Example: Understanding Both Paths

Imagine the following directory structure:

```text
/home/kali/
├── Documents/
│   └── report.txt
├── Downloads/
└── Projects/
    └── Linux/
        └── lesson.md
```

Assume your current working directory is:

```text
/home/kali
```

The absolute path to `report.txt` is:

```text
/home/kali/Documents/report.txt
```

Its relative path is:

```text
Documents/report.txt
```

Now assume your current directory changes to:

```text
/home/kali/Projects/Linux
```

The absolute path remains:

```text
/home/kali/Documents/report.txt
```

But the relative path becomes:

```text
../../Documents/report.txt
```

This demonstrates why relative paths depend on your current working directory.

## 7. How to Identify an Absolute or Relative Path

Use this simple rule:

- If the path begins with `/`, it is an absolute path.
- If it does not begin with `/`, it is generally a relative path.

Examples:

| Path | Type |
|---|---|
| `/etc/hostname` | Absolute |
| `/home/kali/Documents` | Absolute |
| `Documents/file.txt` | Relative |
| `../Downloads` | Relative |
| `./script.sh` | Relative |
| `../../notes.txt` | Relative |

## 8. Why Paths Matter in Cybersecurity

Absolute and relative paths are important in security operations.

### A. Shell Scripting

Scripts often use paths to locate files and execute programs.

### B. File Investigation

Security professionals use absolute paths to locate important configuration files and logs.

### C. File Permissions

Understanding the exact file path helps administrators inspect and manage permissions correctly.

### D. Security Testing

Path handling is important when investigating file access controls and vulnerabilities in applications.

For example, directory traversal vulnerabilities occur when an application improperly handles user-supplied file paths.

One commonly discussed traversal pattern is:

```text
../../
```

In a properly authorized security lab, studying this concept helps you understand how poorly designed applications might access files outside their intended directories.

Never attempt to access files on systems without permission.

## 9. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: Display Your Current Directory

```bash
pwd
```

Write down the result.

### Exercise 2: Use an Absolute Path

```bash
cd /etc
pwd
```

Confirm that you are inside `/etc`.

### Exercise 3: Use a Relative Path

From `/etc`, run:

```bash
cd ..
pwd
```

Observe where you moved.

### Exercise 4: Access a File Using an Absolute Path

```bash
cat /etc/hostname
```

Explain why this is an absolute path.

### Exercise 5: Access a File Using a Relative Path

Navigate to `/etc`:

```bash
cd /etc
```

Then run:

```bash
cat hostname
```

Explain why `hostname` is a relative path here.

### Exercise 6: Use the Current Directory Symbol

```bash
ls .
```

Explain what `.` represents.

### Exercise 7: Use the Parent Directory Symbol

```bash
cd /var/log
cd ..
pwd
```

Identify your new directory.

### Exercise 8: Compare Both Path Types

Run:

```bash
cd ~
cat /etc/hostname
```

Then navigate to `/etc`:

```bash
cd /etc
cat hostname
```

Explain the difference between the two file references.

## 10. Knowledge Check

1. What is an absolute path?
2. What is a relative path?
3. Which directory does an absolute path begin from?
4. Why does a relative path depend on the current working directory?
5. Is `/etc/passwd` an absolute or relative path?
6. Is `Documents/notes.txt` an absolute or relative path?
7. What does `./` represent?
8. What does `../` represent?
9. Give an example of two different paths referring to the same file.
10. Why is understanding path handling important in cybersecurity?

## 11. Lesson Summary

An absolute path identifies a complete location starting from `/`.

A relative path identifies a location based on the current working directory.

We learned how to use both path types, how to recognize them, and how they support Linux administration, scripting, and cybersecurity investigations.

**Next Lesson:** Navigating the Filesystem.