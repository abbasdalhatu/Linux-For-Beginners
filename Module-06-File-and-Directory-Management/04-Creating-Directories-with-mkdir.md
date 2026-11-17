---
---

# MODULE 06 – FILE AND DIRECTORY MANAGEMENT

## LESSON 04: CREATING DIRECTORIES WITH MKDIR

### 1. Introduction

Directories help Linux users organise files and information.

For example, a cybersecurity student might create separate directories for:

- Network scanning reports
- Penetration-testing projects
- Security investigation evidence
- Bash scripts
- Configuration backups
- Course notes

Linux provides a command called `mkdir` for creating directories.

`mkdir` means **Make Directory**.

### 2. Basic Syntax

The general syntax is:

```bash
mkdir [OPTIONS] DIRECTORY_NAME
```

Explanation:

- `mkdir` is the command.
- `OPTIONS` modify how the command works.
- `DIRECTORY_NAME` specifies the directory to create.

### 3. Creating a Single Directory

To create a directory named `projects`, execute:

```bash
mkdir projects
```

Verify its creation:

```bash
ls
```

You should see:

```text
projects
```

You can also use:

```bash
ls -ld projects
```

The `-d` option displays information about the directory itself rather than listing its contents.

### 4. Creating Multiple Directories

You can create several directories using one command.

Example:

```bash
mkdir documents downloads pictures
```

This creates three directories:

```text
documents/
downloads/
pictures/
```

Verify:

```bash
ls
```

This is useful when preparing a new project structure.

### 5. Creating Nested Directories with `-p`

Sometimes you need to create a directory inside another directory that does not exist yet.

For example:

```bash
mkdir projects/linux/security
```

This command may fail if `projects` or `projects/linux` does not exist.

To create the complete directory structure automatically, use `-p`.

```bash
mkdir -p projects/linux/security
```

The `-p` option creates missing parent directories.

It also prevents an error if the requested directory already exists.

Example structure:

```text
projects/
└── linux/
    └── security/
```

### 6. Understanding the `-v` Option

The `-v` option means verbose.

It displays a message describing each directory created.

Example:

```bash
mkdir -v reports
```

Possible output:

```text
mkdir: created directory 'reports'
```

You can combine options:

```bash
mkdir -pv projects/linux/labs
```

This creates missing parent directories and reports the creation activity.

### 7. Creating Directories with Absolute Paths

An absolute path begins from the root directory `/`.

Example:

```bash
mkdir -p /home/kali/security-labs
```

This creates the directory inside `/home/kali`, assuming you have permission.

You can also create a directory inside your home directory using:

```bash
mkdir -p ~/security-labs
```

The `~` symbol represents your home directory.

### 8. Creating Directories with Relative Paths

A relative path is interpreted from your current working directory.

Check your current location:

```bash
pwd
```

Suppose the result is:

```text
/home/kali
```

Now execute:

```bash
mkdir projects
```

Linux creates:

```text
/home/kali/projects
```

The location depends on where you are when you run the command.

### 9. Creating an Organised Project Structure

Imagine you are preparing a cybersecurity investigation project.

You want the following structure:

```text
investigation/
├── evidence/
├── reports/
├── scripts/
├── logs/
└── backups/
```

You can create the entire structure using:

```bash
mkdir -p investigation/{evidence,reports,scripts,logs,backups}
```

This uses Bash brace expansion.

Alternatively, use:

```bash
mkdir -p investigation/evidence investigation/reports investigation/scripts investigation/logs investigation/backups
```

Both approaches create the requested directories.

Verify the structure:

```bash
ls -R investigation
```

### 10. Important `mkdir` Options

| Option | Purpose |
|---|---|
| `-p` | Creates parent directories when necessary |
| `-v` | Displays information about created directories |
| `-m MODE` | Sets directory permissions during creation |

Example using `-m`:

```bash
mkdir -m 700 private
```

This requests permissions of `700` for the new directory.

The permissions mean:

- Owner: read, write, execute.
- Group: no permissions.
- Others: no permissions.

We will study Linux permissions in detail in Module 07.

### 11. Common Errors When Using `mkdir`

#### Error 1: Directory Already Exists

Command:

```bash
mkdir projects
```

Possible error:

```text
mkdir: cannot create directory 'projects': File exists
```

Solution:

```bash
mkdir -p projects
```

Use this when you want to ensure that the directory exists without an error if it is already present.

#### Error 2: Missing Parent Directory

Command:

```bash
mkdir projects/linux/security
```

Possible error:

```text
mkdir: cannot create directory: No such file or directory
```

Solution:

```bash
mkdir -p projects/linux/security
```

#### Error 3: Permission Denied

You may receive:

```text
mkdir: cannot create directory: Permission denied
```

This means your user does not have permission to create a directory at that location.

Choose a location where you have permission, such as:

```bash
mkdir -p ~/my-project
```

Do not automatically use `sudo` to overcome every permission error. First understand why access is restricted.

### 12. Practical Demonstration

Enter your Module 06 lab:

```bash
cd ~/module06-lab
```

Check your current directory:

```bash
pwd
```

Create a directory called `mkdir-practice`:

```bash
mkdir mkdir-practice
```

Enter it:

```bash
cd mkdir-practice
```

Create multiple directories:

```bash
mkdir documents reports scripts
```

Create nested directories:

```bash
mkdir -p projects/linux/security
```

Create another structure using brace expansion:

```bash
mkdir -p investigation/{evidence,logs,backups}
```

Display everything:

```bash
ls -R
```

Return to your lab directory:

```bash
cd ~/module06-lab
```

### 13. Cybersecurity Applications

The `mkdir` command is useful in cybersecurity because it helps organise investigation and testing activities.

**A. Penetration-testing organisation**

Create separate directories for reports, screenshots, scripts, and results.

**B. Evidence management**

Keep investigation evidence organised in dedicated directories.

**C. Log management**

Create directories for storing application and security logs.

**D. Automation**

Bash scripts can use `mkdir -p` to prepare directory structures automatically.

**E. Backup organisation**

Create separate locations for configuration backups and recovery files.

### 14. Safety Rules

1. Check your current directory before creating directories.
2. Avoid creating unnecessary directories inside system locations.
3. Use `mkdir -p` when creating nested structures.
4. Avoid changing permissions unnecessarily.
5. Keep cybersecurity project files organised.
6. Never use directory creation commands as a reason to modify protected system directories without authorisation.

### 15. Knowledge Check

Answer these questions in your notebook.

1. What does `mkdir` stand for?
2. Write the general syntax of the `mkdir` command.
3. What is the purpose of the `-p` option?
4. What does the `-v` option do?
5. How can you create three directories using one command?
6. What is the difference between an absolute path and a relative path when using `mkdir`?
7. Why might `mkdir projects/linux/security` fail?
8. How does `mkdir -p` solve the problem?
9. What does `mkdir -m 700 private` request?
10. Give two examples of how `mkdir` can help a cybersecurity professional.

### 16. Practical Exercises

Perform all exercises inside your Kali Linux VM.

**Exercise 1:** Enter your Module 06 lab.

```bash
cd ~/module06-lab
```

**Exercise 2:** Create a directory named `directory-management`.

**Exercise 3:** Inside it, create three directories:

- `documents`
- `reports`
- `scripts`

**Exercise 4:** Create this nested structure using `mkdir -p`:

```text
security/network/scanning
```

**Exercise 5:** Create the following structure using brace expansion:

```text
ethical-hacking/
├── reconnaissance/
├── scanning/
├── exploitation/
└── reporting/
```

**Exercise 6:** Use `mkdir -v` to create a directory named `verbose-test`.

**Exercise 7:** Use `ls -R` to display all the directories you created.

**Exercise 8:** Explain the difference between these commands:

```bash
mkdir projects/linux/labs
mkdir -p projects/linux/labs
```

### 17. Lesson Summary

In this lesson, you learned:

- The meaning and purpose of `mkdir`.
- How to create one or multiple directories.
- How to create nested directories.
- How to use `-p`, `-v`, and `-m`.
- How to use absolute and relative paths.
- How to organise cybersecurity projects.
- How to identify and resolve common directory creation errors.

**Next Lesson:** Creating Files with `touch`.

**End of Lesson 04.**