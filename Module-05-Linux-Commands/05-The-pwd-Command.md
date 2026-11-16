# Lesson 05: The `pwd` Command

## 1. Introduction

In Linux, the terminal always operates from a particular location in the filesystem. This location is called the **current working directory**.

The `pwd` command allows us to identify that location.

`pwd` stands for **Print Working Directory**.

It is one of the simplest but most important Linux commands because it helps us understand where we are before executing other commands.

## 2. What Is a Working Directory?

A working directory is the directory in which your shell is currently operating.

For example, imagine your Linux filesystem contains:

```text
/
├── home
│   └── kali
│       ├── Documents
│       ├── Downloads
│       └── Projects
├── etc
├── var
└── usr
```

If your terminal is currently inside `/home/kali/Documents`, that is your current working directory.

When you execute a command using a relative path, Linux interprets that path in relation to your current working directory.

This is why knowing your current location is important.

## 3. Basic Syntax

The general syntax is:

```bash
pwd [OPTION]
```

To display your current working directory:

```bash
pwd
```

Example output:

```text
/home/kali
```

This means your shell is currently operating inside the `/home/kali` directory.

**Important:** Your output may be different depending on where your terminal is currently located.

## 4. Understanding the Output

Consider this command:

```bash
pwd
```

Output:

```text
/home/kali/Documents
```

Let's break it down:

- `/` represents the root directory.
- `home` is a directory inside the root directory.
- `kali` is a directory inside `/home`.
- `Documents` is a directory inside `/home/kali`.

The complete path tells us exactly where we are.

## 5. Logical and Physical Paths

The `pwd` command supports two important options:

### 5.1 Logical Path: `pwd -L`

The logical option displays the path as maintained by the shell.

```bash
pwd -L
```

This can preserve symbolic links in the displayed path.

### 5.2 Physical Path: `pwd -P`

The physical option displays the actual filesystem location, resolving symbolic links.

```bash
pwd -P
```

To understand the difference, imagine this structure:

```text
/home/kali/pwd-lab/
├── real-folder/
└── link-folder -> real-folder
```

If you enter the symbolic link:

```bash
cd ~/pwd-lab/link-folder
```

You can compare:

```bash
pwd -L
pwd -P
```

The logical path may show:

```text
/home/kali/pwd-lab/link-folder
```

The physical path may show:

```text
/home/kali/pwd-lab/real-folder
```

Exact behavior can depend on your shell and environment.

## 6. Understanding `pwd` as a Shell Builtin

In Bash, `pwd` is normally available as a shell builtin.

A shell builtin is a command implemented directly inside the shell rather than requiring a separate executable.

Check how your shell identifies `pwd`:

```bash
type pwd
```

You may see:

```text
pwd is a shell builtin
```

You can also inspect the Bash help information:

```bash
help pwd
```

To locate an external version, try:

```bash
command -v pwd
```

On many Linux systems, the external GNU Coreutils version is located at:

```text
/usr/bin/pwd
```

You can inspect its manual page with:

```bash
man pwd
```

The exact location and availability of an external executable may vary by distribution.

## 7. Absolute Paths and Relative Paths

The `pwd` command becomes more useful when you understand two types of paths.

### 7.1 Absolute Path

An absolute path starts from the root directory `/`.

Example:

```text
/home/kali/Documents
```

It identifies a location independently of your current directory.

### 7.2 Relative Path

A relative path describes a location in relation to your current working directory.

For example, if your current directory is:

```text
/home/kali
```

You can enter Documents using:

```bash
cd Documents
```

The command uses a relative path.

Use `pwd` to confirm your location before and after navigation.

## 8. Practical Lab: Exploring Your Current Location

Perform these commands inside your Kali Linux virtual machine.

### Step 1: Display Your Current Location

```bash
pwd
```

Observe the output.

### Step 2: Move to Your Home Directory

```bash
cd ~
pwd
```

The `~` symbol represents your home directory.

### Step 3: Move to the Root Directory

```bash
cd /
pwd
```

Expected output:

```text
/
```

### Step 4: Move to the System Log Directory

```bash
cd /var/log
pwd
```

You should see:

```text
/var/log
```

### Step 5: Compare Logical and Physical Paths

```bash
pwd -L
pwd -P
```

Observe whether the outputs are identical.

## 9. Practical Lab: Understanding Symbolic Links

A symbolic link is a special filesystem object that points to another file or directory.

Let's create a safe practice environment inside your home directory.

### Step 1: Create a Practice Directory

```bash
mkdir -p ~/pwd-lab/real-folder
```

### Step 2: Create a Symbolic Link

```bash
ln -s ~/pwd-lab/real-folder ~/pwd-lab/link-folder
```

### Step 3: Enter the Symbolic Link

```bash
cd ~/pwd-lab/link-folder
```

### Step 4: Display the Logical Path

```bash
pwd -L
```

### Step 5: Display the Physical Path

```bash
pwd -P
```

Compare the results.

You have now seen how logical and physical paths can differ.

### Step 6: Return Home

```bash
cd ~
```

The practice directory is safe to keep or remove later.

## 10. Common Mistakes and Problems

### Mistake 1: Confusing `pwd` with `ls`

These commands perform different tasks.

- `pwd` displays your current directory.
- `ls` displays the contents of a directory.

### Mistake 2: Assuming Every Terminal Starts in the Same Directory

Your terminal may start in your home directory, a project directory, or another location.

Always check:

```bash
pwd
```

### Mistake 3: Forgetting the Current Directory

A relative path depends on where you are.

For example:

```bash
rm example.txt
```

This command targets `example.txt` in the current working directory.

If you are in the wrong directory, you may affect the wrong file.

### Mistake 4: Working in a Directory That Has Been Deleted

If another process deletes the directory your shell is currently using, commands such as `pwd` may report an error or an unusual result.

A simple recovery is to move to a valid directory:

```bash
cd ~
pwd
```

## 11. Cybersecurity Importance of `pwd`

The `pwd` command is useful in cybersecurity and ethical hacking for several reasons.

### 11.1 Avoiding File Operation Mistakes

Before modifying or deleting files, confirm your location:

```bash
pwd
```

### 11.2 Understanding Tool Locations

When working with security tools, scripts, configuration files, and reports, knowing the current directory helps prevent confusion.

### 11.3 Understanding Relative Paths

Scripts and programs sometimes access files through relative paths.

If the working directory changes, those paths may point somewhere else.

### 11.4 Safe Command Execution

Before running a command that modifies files, check your current directory and understand exactly which files the command will affect.

**Security rule:** Never run an unfamiliar or destructive command without understanding its target and checking your current location.

## 12. Important Commands Summary

| Command | Purpose |
|---|---|
| `pwd` | Display current working directory |
| `pwd -L` | Display logical working directory |
| `pwd -P` | Display physical working directory |
| `cd ~` | Move to home directory |
| `cd /` | Move to root directory |
| `type pwd` | Identify how the shell resolves `pwd` |
| `help pwd` | Display Bash builtin help |
| `man pwd` | Open the manual page |

## 13. Knowledge Check

Answer these questions in your notebook.

1. What does `pwd` stand for?
2. What is a current working directory?
3. What is the difference between an absolute path and a relative path?
4. What does `pwd -L` display?
5. What does `pwd -P` display?
6. What is a symbolic link?
7. How can you check whether `pwd` is a Bash builtin?
8. Why should a cybersecurity professional check the current directory before executing file operations?
9. What does the `~` symbol represent in Linux?
10. What could happen if you execute a relative-path command from the wrong directory?

## 14. Practical Assignment

Complete the following tasks in your Kali Linux virtual machine.

1. Display your current working directory.
2. Move to your home directory and confirm your location.
3. Move to `/etc` and confirm your location.
4. Move to `/var/log` and confirm your location.
5. Compare the output of `pwd -L` and `pwd -P`.
6. Create the symbolic-link practice environment from Section 9.
7. Enter the symbolic link and compare its logical and physical paths.
8. Use `type pwd` to determine whether your shell recognizes it as a builtin.
9. Open the manual page for `pwd`.
10. Write down three reasons why `pwd` is useful in cybersecurity.

## 15. Lesson Conclusion

The `pwd` command is a fundamental Linux navigation tool.

It tells us where the shell is currently operating and helps us understand how relative paths are interpreted.

Although simple, it is extremely useful when managing files, navigating Linux systems, writing scripts, and performing cybersecurity tasks.

**Key lesson:** Before working with files or executing commands that modify the filesystem, know where you are.

---

**End of Lesson 05 — The `pwd` Command**