# LESSON 01 – INTRODUCTION TO LINUX COMMANDS

## 1. Introduction

Linux commands are instructions entered into a command-line interface to make the operating system perform specific tasks.

Instead of interacting with everything through graphical applications, Linux allows users to control the system by typing commands into a terminal.

You can use Linux commands to:

- Navigate directories.
- Create and delete files.
- Manage users and permissions.
- Install software.
- Monitor running processes.
- Inspect network connections.
- Investigate system logs.
- Perform security assessments.

For an ethical hacker, understanding Linux commands is essential because many cybersecurity tools operate through the terminal.

## 2. What Is a Linux Command?

A Linux command is an instruction given to the shell to perform an operation.

Example:

```bash
pwd
```

This command displays the current working directory.

Another example:

```bash
whoami
```

This displays the username of the currently logged-in user.

When you enter a command, the shell interprets it and arranges for the requested operation to be performed.

## 3. Understanding the Command-Line Interface (CLI)

CLI means Command-Line Interface.

It is a way of interacting with a computer by entering text commands rather than relying entirely on graphical elements.

A terminal emulator provides a window where you can interact with a shell such as Bash.

The relationship is:

1. Terminal emulator: Provides the interface for typing commands.
2. Shell: Interprets commands and manages their execution.
3. Operating system: Provides the services needed to perform operations.
4. Kernel: Manages hardware and core system resources.

## 4. Anatomy of a Linux Command

Consider this example:

```bash
ls -l /home
```

It contains three important parts:

| Component | Meaning |
|---|---|
| `ls` | Command name |
| `-l` | Option |
| `/home` | Argument |

### Command

The command identifies the program or operation to execute.

Example:

```bash
ls
```

### Option

An option changes how a command behaves.

Example:

```bash
ls -l
```

The `-l` option requests a detailed listing.

### Argument

An argument provides information about what the command should operate on.

Example:

```bash
ls /home
```

Here, `/home` is the directory being listed.

## 5. Basic Linux Commands

### 5.1 `pwd` – Print Working Directory

Displays your current location in the filesystem.

```bash
pwd
```

Example output:

```text
/home/kali
```

### 5.2 `ls` – List Directory Contents

Displays files and directories.

```bash
ls
```

For a detailed listing:

```bash
ls -l
```

To include hidden files:

```bash
ls -a
```

To combine options:

```bash
ls -la
```

### 5.3 `whoami` – Current Username

```bash
whoami
```

### 5.4 `date` – Display Date and Time

```bash
date
```

### 5.5 `uname` – System Information

```bash
uname
```

For detailed information:

```bash
uname -a
```

### 5.6 `hostname` – Computer Name

```bash
hostname
```

### 5.7 `clear` – Clear the Terminal

```bash
clear
```

### 5.8 `history` – Command History

```bash
history
```

## 6. Why Linux Commands Are Important in Ethical Hacking

Linux commands are useful throughout cybersecurity operations.

### System Information Gathering

```bash
uname -a
whoami
hostname
ip addr
```

### File Investigation

```bash
ls -la
find ~ -type f
file filename.txt
```

### Process Investigation

```bash
ps aux
```

### Log Investigation

```bash
journalctl -n 20
```

### Network Investigation

```bash
ip addr
ip route
```

These commands help you understand your own Linux system and investigate authorised environments.

## 7. Command Case Sensitivity

Linux commands and filenames are generally case-sensitive.

For example:

```bash
pwd
```

is different from:

```bash
PWD
```

Similarly, these can represent three different filenames:

```text
report.txt
Report.txt
REPORT.txt
```

Always pay attention to uppercase and lowercase characters.

## 8. Understanding Command Errors

For example:

```bash
pwdd
```

You may receive:

```text
command not found
```

Common causes include:

- Typing mistakes.
- Missing software.
- Incorrect command options.
- Incorrect file or directory paths.
- Insufficient permissions.

Read error messages carefully. They often provide useful information about the problem.

## 9. Practical Laboratory

Open your Kali Linux terminal and perform these exercises.

### Exercise 1 – Identify Your Environment

```bash
whoami
hostname
pwd
uname -a
date
```

Write down what each command tells you.

### Exercise 2 – Explore Your Home Directory

```bash
cd ~
pwd
ls
ls -la
```

Observe the difference between `ls` and `ls -la`.

### Exercise 3 – Explore Command History

```bash
history
```

Identify at least five commands you previously executed.

### Exercise 4 – Practise Command Combinations

```bash
whoami; hostname; pwd
```

Notice how all three commands execute in sequence.

Now try:

```bash
pwd && ls
```

The second command executes only if the first command succeeds.

## 10. Important Terms

| Term | Definition |
|---|---|
| Command | An instruction given to the shell |
| CLI | Command-Line Interface |
| Terminal | Interface used to interact with a shell |
| Shell | Program that interprets commands |
| Option | Changes a command's behaviour |
| Argument | Information supplied to a command |
| Command history | Record of previously entered commands |
| Case-sensitive | Uppercase and lowercase are treated differently |

## 11. Lesson Summary

In this lesson, you learned:

1. What Linux commands are.
2. What a command-line interface is.
3. The relationship between a terminal and a shell.
4. The basic structure of Linux commands.
5. Common commands used for system information.
6. How to explore files and directories.
7. Why Linux commands are important in ethical hacking.
8. How to recognise basic command errors.

## 12. Knowledge Check

Answer these questions in your notebook:

1. What is a Linux command?
2. What does CLI stand for?
3. What is the difference between a terminal and a shell?
4. Identify the command, option and argument in `ls -l /home`.
5. What does `pwd` display?
6. What is the purpose of `whoami`?
7. Why is Linux case-sensitive?
8. What does the `history` command do?
9. Give four examples of Linux commands useful for system investigation.
10. Why should ethical hackers understand Linux commands?

## 13. Practical Assignment

Use your Kali terminal to:

- Identify your username.
- Identify your hostname.
- Display your current directory.
- Display detailed system information.
- List all files, including hidden files, in your home directory.
- Display your command history.

Record each command and its output in your notebook.

**End of Lesson 01 – Introduction to Linux Commands.**      