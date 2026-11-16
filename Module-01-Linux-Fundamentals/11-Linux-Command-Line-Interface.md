# Module 01: Linux Fundamentals

# Lesson 11 – Linux Command Line Interface (CLI)

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 11 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

The Command Line Interface (CLI) is one of the most important ways to interact with a Linux operating system.

Unlike a Graphical User Interface (GUI), where users interact with windows, icons, menus, and buttons, the CLI allows users to communicate with the operating system by typing commands.

For example, instead of opening a file manager and clicking through folders, you can use:

```bash
ls
```

This command displays files and directories in your current location.

Linux administrators, developers, penetration testers, and cybersecurity professionals frequently use the command line because it provides speed, flexibility, automation, and direct access to many system-management functions.

In ethical hacking, understanding the CLI is essential because many security tools operate through the terminal.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Define the Command Line Interface.
2. Explain the difference between CLI and GUI.
3. Understand how Linux commands are structured.
4. Identify commands, options, arguments, and command output.
5. Execute basic Linux commands.
6. Understand command syntax.
7. Use command options and arguments.
8. Understand command chaining and pipes.
9. Use Linux help and manual pages.
10. Apply CLI knowledge to cybersecurity activities.

## 3. What Is a Command Line Interface?

A Command Line Interface (CLI) is a text-based interface that allows users to interact with an operating system by entering commands.

The user types a command, presses Enter, and the system processes the instruction.

### Example

```bash
pwd
```

Output example:

```text
/home/kali
```

The command displays the current working directory.

### How the CLI works

The general process is:

1. The user opens a terminal.
2. The shell displays a prompt.
3. The user types a command.
4. The shell interprets the command.
5. The operating system performs the requested operation.
6. The result is displayed in the terminal.

The shell acts as the command interpreter between the user and the operating system.

## 4. CLI vs GUI

There are two common ways to interact with an operating system.

- CLI: Command Line Interface.
- GUI: Graphical User Interface.

### Graphical User Interface

A GUI allows users to interact with a computer using visual elements such as:

- Windows.
- Icons.
- Menus.
- Buttons.
- Mouse pointers.

Example: Opening a folder using the Kali Linux file manager.

### Command Line Interface

A CLI allows users to interact with a computer using text commands.

Example:

```bash
cd Documents
```

This changes the current directory to `Documents`, if that directory exists in the current location.

### Comparison

| Feature | CLI | GUI |
|---|---|---|
| Interaction | Typed commands | Visual elements |
| Input | Keyboard | Mouse and keyboard |
| Speed | Efficient for repeated tasks | Convenient for visual tasks |
| Automation | Easy through scripts | Often requires additional automation |
| Learning | Requires command knowledge | Usually easier for beginners |
| Resource usage | Often lower | Usually higher |
| Precision | Commands can be very specific | Depends on available controls |

### Which is better?

Neither interface is universally better.

A GUI is convenient for everyday desktop activities, while the CLI is particularly useful for administration, automation, development, and cybersecurity.

Professional Linux users often combine both.

## 5. Understanding the Linux Terminal

A terminal is an application that provides access to a command-line environment.

In Kali Linux, you can open the terminal from the applications menu or use the keyboard shortcut configured by your desktop environment.

When the terminal opens, you may see a prompt similar to:

```text
kali@kali:~$
```

Let's understand its components.

| Component | Meaning |
|---|---|
| `kali` | Current username |
| `@` | Separates username and hostname |
| `kali` | Computer hostname |
| `~` | Current directory, usually the user's home directory |
| `$` | Ordinary user prompt indicator |

A root shell commonly uses `#` as its prompt indicator.

**Important:** Prompt symbols can be customised. Always verify your actual identity using `whoami` rather than relying only on the prompt.

## 6. Structure of a Linux Command

Linux commands follow a general structure:

```text
command [options] [arguments]
```

Let's understand each part.

### 6.1 Command

The command is the program or instruction you want to execute.

Example:

```bash
ls
```

Here, `ls` is the command.

### 6.2 Options

Options modify how a command behaves.

Example:

```bash
ls -l
```

The `-l` option requests a detailed listing.

### 6.3 Arguments

Arguments provide information about what the command should operate on.

Example:

```bash
ls Documents
```

Here, `Documents` is the argument.

### Complete example

```bash
ls -lah Documents
```

Explanation:

- `ls` is the command.
- `-l` requests detailed information.
- `-a` includes hidden entries.
- `-h` displays file sizes in a human-readable format.
- `Documents` is the directory argument.

This command displays a detailed listing of the Documents directory, including hidden entries.

## 7. Basic Linux Commands

Before learning advanced Linux administration and cybersecurity, you must understand basic commands.

### 7.1 `pwd` – Print Working Directory

Displays your current directory.

```bash
pwd
```

### 7.2 `ls` – List

Displays files and directories.

```bash
ls
```

Display hidden files:

```bash
ls -a
```

Display detailed information:

```bash
ls -l
```

Combine options:

```bash
ls -lah
```

### 7.3 `cd` – Change Directory

Moves from one directory to another.

```bash
cd Documents
```

Return to your home directory:

```bash
cd ~
```

Move one directory backward:

```bash
cd ..
```

Move to the root directory:

```bash
cd /
```

### 7.4 `mkdir` – Make Directory

Creates a new directory.

```bash
mkdir Projects
```

Create nested directories:

```bash
mkdir -p Projects/Linux/Lessons
```

### 7.5 `touch` – Create an Empty File

Creates an empty file if it does not exist.

```bash
touch notes.txt
```

If the file already exists, `touch` normally updates its timestamps rather than deleting its contents.

### 7.6 `cat` – Display File Contents

Displays the contents of a text file.

```bash
cat notes.txt
```

### 7.7 `cp` – Copy

Copies files or directories.

Copy a file:

```bash
cp notes.txt backup.txt
```

Copy a directory recursively:

```bash
cp -r Projects Projects-Backup
```

### 7.8 `mv` – Move or Rename

Rename a file:

```bash
mv notes.txt linux-notes.txt
```

Move a file into another directory:

```bash
mv linux-notes.txt Documents/
```

### 7.9 `rm` – Remove

Removes files.

```bash
rm old-notes.txt
```

Remove an empty directory:

```bash
rmdir EmptyFolder
```

**Warning:** Linux removal commands can permanently delete data. Always check the target before executing `rm`, especially when using recursive or forced options.

### 7.10 `clear` – Clear the Terminal

Clears the visible terminal screen.

```bash
clear
```

It does not delete files or erase the terminal's command history.

## 8. Understanding Command Options

Options change the behaviour of commands.

Linux commonly uses two option styles.

### Short options

Short options normally use a single hyphen.

Example:

```bash
ls -l
```

### Long options

Long options normally use two hyphens.

Example:

```bash
ls --all
```

### Combining options

Some short options can be combined.

```bash
ls -lah
```

This is equivalent to:

```bash
ls -l -a -h
```

Not every command supports combined options in the same way, so check its documentation.

### Finding available options

Use:

```bash
ls --help
```

This displays information about the command's supported options.

## 9. Command Arguments

Arguments tell a command which file, directory, or resource to work with.

Example:

```bash
mkdir Cybersecurity
```

`Cybersecurity` is the argument.

Another example:

```bash
cat /etc/os-release
```

Here:

- `cat` is the command.
- `/etc/os-release` is the file argument.

### Absolute and relative paths

An absolute path starts from the root directory `/`.

Example:

```bash
/home/kali/Documents/notes.txt
```

A relative path is interpreted from your current working directory.

Example:

```bash
Documents/notes.txt
```

Understanding paths is important because Linux commands often operate on files and directories.

## 10. Command Output and Errors

When you execute a Linux command, the system may produce normal output, an error message, or no visible output.

### Standard output (stdout)

Standard output is where normal command results are sent.

Example:

```bash
ls
```

### Standard error (stderr)

Standard error is where error messages are normally sent.

Example:

```bash
cat missing-file.txt
```

You may receive a message indicating that the file does not exist.

### Standard input (stdin)

Standard input is where a command receives input, commonly from the keyboard or another command.

Linux commonly represents these streams using file descriptors:

| Stream | File descriptor |
|---|---:|
| Standard input | 0 |
| Standard output | 1 |
| Standard error | 2 |

These streams are important for command chaining, scripting, and automation.

## 11. Command Chaining

Linux allows multiple commands to be combined in one command line.

### 11.1 Semicolon `;`

Runs commands sequentially, regardless of whether the previous command succeeds or fails.

```bash
pwd; whoami; date
```

### 11.2 AND operator `&&`

Runs the next command only if the previous command succeeds.

```bash
mkdir Test && cd Test
```

If the directory is created successfully, the shell moves into it.

### 11.3 OR operator `||`

Runs the next command only if the previous command fails.

```bash
cd MissingFolder || echo "Directory not found"
```

### Comparison

| Operator | Behaviour |
|---|---|
| `;` | Runs the next command regardless of success |
| `&&` | Runs the next command if the previous succeeds |
| `||` | Runs the next command if the previous fails |

These operators are useful for administration and automation.

## 12. Understanding Pipes

A pipe uses the `|` symbol to send the standard output of one command into the standard input of another command.

Example:

```bash
ls /etc | head
```

Explanation:

1. `ls /etc` lists entries in `/etc`.
2. The pipe passes the output to `head`.
3. `head` displays the first ten lines by default.

Another example:

```bash
ps aux | grep ssh
```

This searches the displayed process information for lines containing `ssh`.

Pipes allow Linux users to combine small utilities into more powerful workflows.

## 13. Command Redirection

Redirection changes where a command receives input or sends output.

### 13.1 Output redirection `>`

Sends standard output to a file, replacing its previous contents.

```bash
pwd > location.txt
```

### 13.2 Append redirection `>>`

Adds standard output to the end of a file.

```bash
date >> location.txt
```

### 13.3 Input redirection `<`

Uses a file as standard input.

```bash
sort < names.txt
```

### 13.4 Error redirection `2>`

Sends standard error to a file.

```bash
cat missing-file.txt 2> errors.txt
```

### 13.5 Redirecting both output streams

```bash
command > output.txt 2>&1
```

This sends standard output and standard error to the same file.

**Important:** Redirection can overwrite files. Review the destination before using `>`.

## 14. Getting Help in Linux

Linux provides several ways to learn how commands work.

### 14.1 `man`

Displays the manual page for a command.

Example:

```bash
man ls
```

Use the arrow keys to navigate.

Press `q` to exit.

### 14.2 `--help`

Displays a command's help information.

```bash
ls --help
```

### 14.3 `whatis`

Provides a short description of a command when its manual database is available.

```bash
whatis ls
```

### 14.4 `which`

Displays the path of a command found through the current PATH.

```bash
which bash
```

### 14.5 `type`

Explains how the shell interprets a command name.

```bash
type cd
```

This is useful because some commands, such as `cd`, are shell built-ins rather than separate executable files.

## 15. Why the CLI Is Important in Cybersecurity

Cybersecurity professionals use the CLI for system inspection, network analysis, automation, and security testing.

### System inspection

```bash
whoami
```

Identifies the current user.

```bash
uname -a
```

Displays kernel and system information.

```bash
ps aux
```

Displays running processes.

### Network inspection

```bash
ip addr
```

Displays network interface information.

```bash
ss -tuln
```

Displays listening TCP and UDP sockets.

### File inspection

```bash
ls -lah
```

Displays detailed file information, including hidden entries.

```bash
file notes.txt
```

Identifies the file type based on its contents and signatures.

### Log inspection

```bash
journalctl -n 20
```

Displays recent system journal entries.

These commands help administrators and security professionals understand the state of a Linux system.

## 16. Practical Lab: Mastering the Linux CLI

**Lab objective:** Practise command execution, navigation, file management, pipes, redirection, and help commands inside your Kali Linux virtual machine.

### Part A – Open the terminal

Open your Kali Linux VM.

Launch the terminal and execute:

```bash
whoami
```

Then:

```bash
pwd
```

Observe your username and current directory.

### Part B – Practise directory navigation

Run:

```bash
ls
```

Then:

```bash
cd Documents
```

If the directory exists, check your location:

```bash
pwd
```

Return home:

```bash
cd ~
```

### Part C – Create a practice directory

Run:

```bash
mkdir -p CLI-Practice/Files
```

Enter the directory:

```bash
cd CLI-Practice
```

Create three files:

```bash
touch file1.txt file2.txt file3.txt
```

List them:

```bash
ls -lah
```

### Part D – Add and display information

Write text into the first file:

```bash
echo "Learning Linux Command Line" > file1.txt
```

Display it:

```bash
cat file1.txt
```

Append another line:

```bash
echo "Linux is useful in cybersecurity." >> file1.txt
```

Display the updated file:

```bash
cat file1.txt
```

### Part E – Practise copying and renaming

Copy the first file:

```bash
cp file1.txt backup.txt
```

Rename the second file:

```bash
mv file2.txt renamed-file.txt
```

List the directory:

```bash
ls -l
```

### Part F – Practise pipes

Run:

```bash
ls -la | head
```

Then:

```bash
ps aux | head
```

Observe how one command passes its output to another.

### Part G – Practise command chaining

Run:

```bash
pwd; whoami; date
```

Then:

```bash
mkdir TestFolder && cd TestFolder
```

Verify your location:

```bash
pwd
```

Return to your practice directory:

```bash
cd ..
```

### Part H – Practise redirection

Run:

```bash
date > system-info.txt
```

Then:

```bash
uname -a >> system-info.txt
```

Display the result:

```bash
cat system-info.txt
```

### Part I – Practise help commands

Run:

```bash
ls --help
```

Then:

```bash
man pwd
```

Press `q` to exit the manual.

Finally:

```bash
type cd
```

Observe whether the command is a shell built-in.

### Part J – Clean up

After reviewing your work, return to your home directory:

```bash
cd ~
```

Remove the practice directory and its contents:

```bash
rm -r CLI-Practice
```

**Warning:** Before running recursive removal commands, verify the target path carefully. Never use `rm -rf` casually.

## 17. Important Terms

| Term | Meaning |
|---|---|
| CLI | Text-based interface for interacting with a computer |
| GUI | Graphical interface using visual elements |
| Terminal | Application that provides access to a command-line environment |
| Shell | Program that interprets commands |
| Command | Instruction executed by the shell |
| Option | Modifies command behaviour |
| Argument | Information supplied to a command |
| Absolute Path | Path beginning from the root directory |
| Relative Path | Path interpreted from the current directory |
| Pipe | Passes one command's output to another command |
| Redirection | Changes where input or output is sent |
| stdout | Standard output |
| stdin | Standard input |
| stderr | Standard error |

## 18. Knowledge Check

Answer these questions without looking at the lesson.

1. What is a Command Line Interface?
2. What is the difference between CLI and GUI?
3. What is the purpose of a Linux terminal?
4. What is a shell?
5. Explain the structure of a Linux command.
6. What is the difference between an option and an argument?
7. What does `pwd` do?
8. What is the purpose of `cd`?
9. What is the difference between `>` and `>>`?
10. What does the pipe symbol `|` do?
11. Explain the difference between `;`, `&&`, and `||`.
12. What is standard output?
13. What is standard error?
14. What is the purpose of the `man` command?
15. Why is CLI knowledge important in cybersecurity?

## 19. Practical Assignment

Complete the following tasks:

1. Define the Linux Command Line Interface.
2. Explain five differences between CLI and GUI.
3. Explain the structure of a Linux command.
4. Write five Linux commands and describe their purposes.
5. Explain the difference between absolute and relative paths.
6. Explain the purpose of command options and arguments.
7. Demonstrate how to create a directory and three files.
8. Explain how pipes work, using an example.
9. Explain the difference between output redirection and append redirection.
10. Describe three ways Linux CLI commands help cybersecurity professionals.

Save your assignment as:

`Linux-CLI-Assignment.md`

## 20. Exam-Ready Summary

The Command Line Interface (CLI) is a text-based method of interacting with an operating system. Linux users enter commands through a terminal, where a shell interprets them and requests the operating system to perform the required operations. A typical Linux command follows the structure `command [options] [arguments]`. Options modify command behaviour, while arguments identify the resources on which commands operate. Linux provides commands for navigation, file management, system inspection, and administration. Command chaining, pipes, and redirection allow multiple commands to work together. Help facilities such as `man` and `--help` provide documentation. The CLI is particularly important in cybersecurity because it supports efficient system administration, network inspection, automation, and security testing.

## 21. Lesson Conclusion

The Linux command line is a fundamental skill for anyone learning Linux administration or ethical hacking.

You have now learned how to execute commands, navigate directories, manage files, use command options, combine commands, redirect output, and access documentation.

These skills will become the foundation for more advanced Linux operations.

**Next Lesson:** Lesson 12 – Linux Terminal Fundamentals.