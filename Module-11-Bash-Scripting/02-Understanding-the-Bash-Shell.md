---
---


# Module 11 – Lesson 02: Understanding the Bash Shell

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a shell is.
2. Understand how Bash communicates with the Linux kernel.
3. Identify different types of Linux shells.
4. Understand interactive and non-interactive shells.
5. Work with Bash environment variables.
6. Understand the Bash command execution process.
7. Inspect your current shell using Linux commands.

---

## 2. What Is a Shell?

A **shell** is a program that provides an interface between a user and the operating system.

It accepts commands from the user, interprets them, and requests that the operating system perform the required operations.

In Linux, the shell allows users to interact with the system through a command-line interface (CLI).

### Simple Explanation

Imagine you want to create a directory.

You enter:

```bash
mkdir projects
```

The shell interprets this command and requests that Linux create the directory.

The shell acts as an intermediary between you and the operating system.

---

## 3. Understanding the Relationship Between User, Shell, and Kernel

Linux has several important components that work together.

### A. User

The person interacting with the computer.

### B. Shell

The program that interprets commands.

### C. Kernel

The core component of the Linux operating system.

The kernel manages:
- CPU resources.
- Memory.
- Hardware devices.
- Processes.
- Filesystems.
- System calls.

### How They Work Together

The process generally looks like this:

1. The user enters a command.
2. The shell reads the command.
3. The shell interprets the command.
4. The shell runs a program or requests an operating-system service.
5. The kernel performs the required operations when necessary.
6. The result is returned to the user.

**Important:** Not every shell command directly invokes the kernel. Some commands are shell built-ins, while others launch external programs.

---

## 4. What Is Bash?

Bash stands for **Bourne Again Shell**.

It is one of the most widely used command-line shells in Linux.

Bash provides features such as:

- Command execution.
- Variables.
- Functions.
- Loops.
- Conditional statements.
- Command history.
- Command completion.
- Input and output redirection.
- Pipes.
- Shell scripting.

These features make Bash useful for both everyday Linux administration and automation.

---

## 5. Different Types of Linux Shells

Linux supports several shell programs.

### A. Bash

Bourne Again Shell.

- Common on Linux systems.
- Supports scripting and automation.
- Compatible with many traditional shell scripts.

### B. Zsh

Z Shell.

- Provides advanced command completion.
- Supports extensive customization.
- Popular among developers and terminal enthusiasts.

### C. Fish

Friendly Interactive Shell.

- Designed for user-friendly interaction.
- Provides syntax highlighting.
- Offers convenient command suggestions.

### D. Sh

Bourne-style shell interface.

- Commonly associated with traditional Unix shell scripting.
- POSIX shell scripts generally aim for compatibility with shells implementing the POSIX standard.

### Comparison

| Shell | Main Feature |
|---|---|
| Bash | General-purpose scripting |
| Zsh | Customization and completion |
| Fish | User-friendly interactive experience |
| Sh | Traditional shell compatibility |

For this course, Bash will be our primary shell.

---

## 6. Interactive and Non-Interactive Shells

There are two important ways a shell can operate.

### A. Interactive Shell

An interactive shell allows users to enter commands directly.

Example:

```bash
pwd
```

You type the command and Bash displays the result.

Examples include:
- Opening the Kali terminal.
- Running commands manually.
- Working with command history.

### B. Non-Interactive Shell

A non-interactive shell executes commands without requiring the user to enter each command manually.

Example:

```bash
bash hello.sh
```

Bash reads the script and executes its instructions.

### Comparison

| Interactive Shell | Non-Interactive Shell |
|---|---|
| Accepts direct user input | Executes supplied commands or scripts |
| Commonly displays a prompt | Usually does not require a prompt |
| Supports interactive command history | May run without interactive history |
| Used for manual administration | Used for automation |

---

## 7. Identifying Your Current Shell

Linux provides commands for inspecting the current shell environment.

### Command 1: Display the SHELL Variable

```bash
echo "$SHELL"
```

This displays the user's configured login shell.

Example output:

```text
/bin/bash
```

**Important:** `$SHELL` usually identifies the configured login shell. It does not always identify the shell currently executing your commands.

### Command 2: Identify the Current Process

```bash
ps -p "$$" -o pid=,comm=,args=
```

This displays information about the current shell process.

### Command 3: Display the Bash Version

```bash
bash --version
```

This displays the installed Bash version.

### Command 4: Display the Current User

```bash
whoami
```

---

## 8. Understanding Environment Variables

An environment variable is a named value that a process can access while it runs.

Environment variables store information about the user's environment and system configuration.

Examples include:

| Variable | Purpose |
|---|---|
| `HOME` | User's home directory |
| `USER` | Current username |
| `SHELL` | Configured login shell |
| `PATH` | Directories searched for executable commands |
| `PWD` | Current working directory |
| `LANG` | Language and locale settings |

### Display an Environment Variable

Use:

```bash
echo "$HOME"
```

Display your username:

```bash
echo "$USER"
```

Display your PATH:

```bash
echo "$PATH"
```

### Display All Environment Variables

```bash
printenv
```

You can also use:

```bash
env
```

These commands display the environment variables available to the process.

---

## 9. Understanding the PATH Variable

The `PATH` variable is especially important.

It contains a list of directories that the shell searches when you enter an executable command without specifying its full path.

Example:

```bash
echo "$PATH"
```

You may see directories such as:

```text
/usr/local/bin:/usr/bin:/bin
```

The colon separates individual directories.

### Example

When you enter:

```bash
ls
```

Bash searches the directories listed in PATH to locate the executable, unless `ls` is otherwise resolved as a shell function, alias, or built-in.

To find the executable location, use:

```bash
command -v ls
```

Another useful command is:

```bash
type ls
```

### Security Importance

An unsafe PATH configuration can allow an attacker to cause a program to execute an unintended executable.

For this reason:
- Avoid adding untrusted directories to PATH.
- Be careful when using `.` in PATH.
- Inspect PATH when troubleshooting suspicious command behavior.

---

## 10. Shell Built-ins and External Commands

Bash commands can be divided into different categories.

### A. Shell Built-ins

These commands are implemented directly inside the shell.

Examples:

```bash
cd
echo
pwd
export
alias
```

You can inspect a command using:

```bash
type cd
```

### B. External Commands

These are separate executable programs stored on the filesystem.

Examples:

```bash
ls
cat
grep
mkdir
```

Inspect them using:

```bash
command -v ls
```

### Why Does This Matter?

Some commands behave differently depending on whether they are shell built-ins or external programs.

Understanding the difference helps with:
- Troubleshooting.
- Shell scripting.
- Command execution.
- Security investigations.

---

## 11. Understanding Command History

Bash can maintain a history of commands entered during interactive sessions.

Display your history:

```bash
history
```

Display the last 10 commands:

```bash
history 10
```

Search previous commands:

```bash
history | grep "mkdir"
```

### Useful Keyboard Shortcuts

| Shortcut | Function |
|---|---|
| Up Arrow | Previous command |
| Down Arrow | Next command |
| Ctrl + R | Search command history |
| Ctrl + A | Move to beginning of line |
| Ctrl + E | Move to end of line |
| Ctrl + C | Interrupt the foreground command |
| Ctrl + L | Clear the terminal display |

**Security Note:** Command history can contain sensitive information if users enter passwords, tokens, or other secrets directly into commands. Avoid putting secrets in command-line arguments.

---

## 12. Understanding Aliases

An alias is a shortcut for a command or group of commands.

Example:

```bash
alias ll='ls -lah'
```

Now you can enter:

```bash
ll
```

Instead of typing:

```bash
ls -lah
```

Display configured aliases:

```bash
alias
```

Remove an alias:

```bash
unalias ll
```

Aliases are useful in interactive terminals, but scripts should generally use explicit commands rather than relying on personal aliases.

---

## 13. Practical Laboratory

We will inspect the Bash environment inside Kali Linux.

### Step 1: Create the Lab Directory

```bash
mkdir -p ~/module11-lab/lesson02
cd ~/module11-lab/lesson02
```

### Step 2: Identify Your Shell

```bash
echo "$SHELL"
ps -p "$$" -o pid=,comm=,args=
```

### Step 3: Check Bash Version

```bash
bash --version
```

### Step 4: Inspect Environment Variables

```bash
echo "$HOME"
echo "$USER"
echo "$PATH"
echo "$PWD"
```

### Step 5: Display All Environment Variables

```bash
printenv
```

### Step 6: Inspect Commands

```bash
type cd
type ls
type echo
command -v grep
command -v mkdir
```

### Step 7: Explore Command History

```bash
history 10
```

### Step 8: Create a Temporary Alias

```bash
alias ll='ls -lah'
ll
```

Remove it:

```bash
unalias ll
```

---

## 14. Practical Challenge: Create a Shell Information Script

Create a script:

```bash
nano shell-info.sh
```

Enter:

```bash
#!/bin/bash

echo "========== SHELL INFORMATION =========="

echo "Configured login shell:"
echo "$SHELL"

echo "Current shell process:"
ps -p "$$" -o comm=

echo "Bash version:"
bash --version | head -n 1

echo "Current username:"
whoami

echo "Home directory:"
echo "$HOME"

echo "Current directory:"
pwd

echo "Command search path:"
echo "$PATH"

echo "========================================"
echo "SHELL INSPECTION COMPLETED"
```

Save and execute:

```bash
chmod +x shell-info.sh
./shell-info.sh
```

Observe the output and identify each piece of information.

---

## 15. Lesson Summary

In this lesson, we covered:

1. The meaning and purpose of a shell.
2. How users, shells, and the Linux kernel interact.
3. Bash, Zsh, Fish, and Sh.
4. Interactive and non-interactive shells.
5. Environment variables.
6. The PATH variable.
7. Shell built-ins and external commands.
8. Command history and aliases.
9. Basic shell inspection.

### Key Commands to Remember

| Command | Purpose |
|---|---|
| `echo "$SHELL"` | Display configured login shell |
| `ps -p "$$" -o comm=` | Identify current shell process |
| `bash --version` | Display Bash version |
| `printenv` | Display environment variables |
| `echo "$PATH"` | Display executable search path |
| `type command` | Identify command type |
| `command -v command` | Locate command resolution |
| `history` | Display command history |
| `alias` | Display aliases |

---

## 16. Knowledge Check

Answer these questions in your notebook:

1. What is a shell?
2. Explain the relationship between the user, shell, and kernel.
3. What does Bash stand for?
4. Name four Linux shells.
5. Explain the difference between an interactive and non-interactive shell.
6. What is an environment variable?
7. What is the purpose of the PATH variable?
8. Differentiate shell built-ins from external commands.
9. What does the `history` command do?
10. What is an alias?

### Practical Assessment

Successfully execute the `shell-info.sh` script and explain the purpose of at least five commands used in it.

**Lesson 02 is complete when you understand the concepts and finish the practical challenge.**
