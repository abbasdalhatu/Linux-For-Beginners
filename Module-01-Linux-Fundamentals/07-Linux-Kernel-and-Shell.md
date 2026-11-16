# Module 01: Linux Fundamentals

# Lesson 07 – Linux Kernel and Shell

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 07 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

Every Linux operating system has two important components that help users interact with the computer and manage its resources: the **kernel** and the **shell**.

The kernel is the core of the operating system. It manages hardware, memory, processes, devices, and communication between software and hardware.

The shell provides an interface through which users can interact with the operating system by entering commands.

For example, when you open a Kali Linux terminal and type:

```bash
ls
```

The shell interprets the command, starts the appropriate program, and displays the result.

Understanding the kernel and shell is essential for Linux administration, software development, and ethical hacking.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Define the Linux kernel.
2. Explain the responsibilities of the kernel.
3. Understand how the kernel communicates with hardware.
4. Define a shell.
5. Identify different types of Linux shells.
6. Understand how commands are executed.
7. Differentiate between a kernel, shell, and terminal.
8. Perform basic shell operations in Kali Linux.
9. Explain the importance of kernels and shells in cybersecurity.

## 3. What Is the Linux Kernel?

The Linux kernel is the central component of a Linux-based operating system.

It acts as an intermediary between applications and computer hardware.

Applications cannot normally access hardware resources directly. Instead, they request services from the kernel.

For example, when a program needs to read a file from a storage device, it requests the operation through the operating system interface. The kernel handles the necessary filesystem and device operations.

### Main responsibilities of the Linux kernel

**1. Process management**

The kernel manages running programs and processes.

It is responsible for:

- Creating processes.
- Scheduling CPU time.
- Managing process execution.
- Terminating processes.
- Managing process communication.

**2. Memory management**

The kernel manages RAM and virtual memory.

It handles:

- Allocating memory to processes.
- Releasing unused memory.
- Virtual memory management.
- Memory protection.
- Managing memory access between processes.

**3. Device management**

The kernel communicates with hardware through device drivers.

Examples include:

- Keyboards.
- Mouse devices.
- Storage devices.
- Network interfaces.
- Graphics hardware.

**4. Filesystem management**

The kernel provides access to filesystems and manages operations such as:

- Creating files.
- Reading files.
- Writing files.
- Deleting files.
- Managing file permissions.

**5. Network management**

The kernel provides networking functionality, including:

- Network communication.
- IP networking.
- Packet processing.
- Network interface management.
- Socket operations.

**6. Security and access control**

The kernel enforces important security boundaries.

Examples include:

- Process isolation.
- User and group permissions.
- Access-control checks.
- Privilege separation.
- Security mechanisms such as Linux Security Modules.

## 4. Kernel Space and User Space

Linux separates execution into two important areas:

### Kernel space

Kernel space is the privileged environment where the operating-system kernel executes.

It has access to critical system resources and hardware-management functionality.

### User space

User space is where ordinary applications and many system programs execute.

Examples include:

- Web browsers.
- Text editors.
- Terminal applications.
- Shells.
- File managers.
- Command-line utilities.

### Comparison

| Kernel Space | User Space |
|---|---|
| Runs the kernel | Runs applications and ordinary system programs |
| Highly privileged | Restricted privileges |
| Manages hardware and resources | Requests services from the kernel |
| Contains core kernel functionality | Contains shells, utilities and applications |

**Security importance:** Separating kernel space and user space helps prevent ordinary applications from directly interfering with critical operating-system operations.

## 5. What Is a Shell?

A shell is a program that provides an interface between the user and the operating system.

It accepts commands, interprets them, and coordinates their execution.

For example:

```bash
pwd
```

The shell receives the command and executes the appropriate program or built-in operation.

A shell can provide:

- Command execution.
- Variables.
- Functions.
- Loops.
- Conditional statements.
- Input and output redirection.
- Pipes.
- Scripting and automation.

Shells are especially useful for system administration and cybersecurity because they allow users to automate repetitive tasks.

## 6. Types of Linux Shells

Linux supports different shells. Each has its own features and syntax.

### 1. Bash (Bourne Again Shell)

Bash is one of the most widely used Linux shells.

It supports:

- Command execution.
- Shell scripting.
- Variables.
- Functions.
- Loops.
- Conditional statements.
- Command history.

Example:

```bash
echo "Hello Linux"
```

### 2. Sh (Bourne Shell)

The Bourne shell is an older Unix shell.

It influenced many modern shells and remains important for understanding portable shell scripting.

### 3. Zsh (Z Shell)

Zsh is an advanced shell with features such as:

- Improved command completion.
- Customisation.
- Themes and plugins.
- Advanced interactive features.

### 4. Fish (Friendly Interactive Shell)

Fish is designed to provide a user-friendly interactive command-line experience.

It includes:

- Syntax highlighting.
- Autosuggestions.
- Helpful command completion.

Its scripting syntax differs from Bash.

### Shell comparison

| Shell | Main Characteristics |
|---|---|
| Bash | Widely used and suitable for scripting |
| Sh | Traditional Unix shell and scripting interface |
| Zsh | Advanced interactive features and customisation |
| Fish | User-friendly interactive features |

## 7. Difference Between Kernel, Shell, and Terminal

These three terms are related, but they do not mean the same thing.

### Kernel

The kernel manages hardware and system resources.

### Shell

The shell interprets commands and coordinates their execution.

### Terminal

A terminal is an application that provides a text-based interface for interacting with a shell.

For example, in Kali Linux:

1. You open the terminal application.
2. The terminal starts or connects to a shell.
3. You type a command.
4. The shell interprets the command.
5. The requested program executes.
6. The result appears in the terminal.

### Simple comparison

| Component | Function |
|---|---|
| Kernel | Manages hardware and system resources |
| Shell | Interprets commands |
| Terminal | Provides the interface for entering and viewing text |

**Remember:** A terminal is not the kernel, and a shell is not the terminal.

## 8. How a Linux Command Is Executed

Let's understand what happens when you enter:

```bash
ls /home
```

The execution process can be explained in these steps:

**Step 1 – User input**

You enter the command in the terminal.

**Step 2 – Shell interpretation**

The shell reads the command and identifies the program and its arguments.

**Step 3 – Program execution**

The shell starts the `ls` program.

**Step 4 – System call**

The program requests services from the kernel when it needs to access the filesystem.

**Step 5 – Kernel processing**

The kernel checks the relevant permissions and handles the requested filesystem operations.

**Step 6 – Output**

The program displays the directory contents in the terminal.

### Execution flow

```text
User
  |
  v
Terminal
  |
  v
Shell
  |
  v
Command / Application
  |
  v
System Calls
  |
  v
Linux Kernel
  |
  v
Hardware and System Resources
```

The result is returned through the application and shell to the terminal.

## 9. Understanding System Calls

A system call is a controlled mechanism through which a user-space program requests a service from the kernel.

Applications use system calls to perform operations that require kernel services.

Examples include:

| System Call | Purpose |
|---|---|
| `openat()` | Opens or accesses a file relative to a directory |
| `read()` | Reads data |
| `write()` | Writes data |
| `close()` | Closes a file descriptor |
| `fork()` | Creates a new process |
| `execve()` | Executes a program |
| `socket()` | Creates a communication endpoint |

For example, a program that reads a file may use `openat()` and `read()` to request access to file contents.

You do not normally need to call these system calls manually when using ordinary shell commands. Applications use them internally.

**Cybersecurity connection:** Understanding system calls helps security researchers study program behaviour, process execution, filesystem access, and suspicious activity.

## 10. Understanding the Default Shell

When you open a Linux terminal, the system normally starts your configured login or interactive shell.

To identify your current shell, run:

```bash
echo "$SHELL"
```

This usually displays the configured login shell.

To identify the shell process currently running, use:

```bash
ps -p $$ -o comm=
```

To see available installed shells, run:

```bash
cat /etc/shells
```

To check the Bash version:

```bash
bash --version
```

To check the current user's account information:

```bash
getent passwd "$USER"
```

The final field in a typical account entry identifies the user's configured login shell.

## 11. Basic Shell Commands

Before learning shell scripting, understand these common commands.

### Display text

```bash
echo "Welcome to Linux"
```

### Display the current directory

```bash
pwd
```

### List files

```bash
ls -la
```

### Display the current user

```bash
whoami
```

### Display system information

```bash
uname -a
```

### Display command history

```bash
history
```

### Display the current date

```bash
date
```

### Display the current shell

```bash
echo "$SHELL"
```

These commands are useful for everyday Linux administration and system inspection.

## 12. Introduction to Bash Variables

A variable stores information that can be reused in a shell session or script.

Example:

```bash
name="Abbas"
```

Display the variable:

```bash
echo "$name"
```

Output:

```text
Abbas
```

Notice that there must be no spaces around the equals sign when assigning a Bash variable.

### Another example

```bash
course="Linux for Ethical Hackers"
echo "I am studying $course"
```

Output:

```text
I am studying Linux for Ethical Hackers
```

### Environment variables

Environment variables store information that can be made available to programs launched from a shell.

Examples:

```bash
echo "$USER"
echo "$HOME"
echo "$PATH"
```

- `$USER` commonly contains the current username.
- `$HOME` identifies the user's home directory.
- `$PATH` contains directories searched when the shell looks for executable commands.

To create an environment variable:

```bash
export COURSE="Linux for Ethical Hackers"
```

To display it:

```bash
echo "$COURSE"
```

## 13. Introduction to Shell Scripting

A shell script is a file containing commands that a shell can execute.

Shell scripts help automate repetitive administrative and security tasks.

### Example: First Bash script

Create a file:

```bash
nano hello.sh
```

Enter the following content:

```bash
#!/usr/bin/env bash

echo "Welcome to Linux for Ethical Hackers"
echo "Current user: $(whoami)"
echo "Current directory: $(pwd)"
echo "Current date: $(date)"
```

Save the file.

Make it executable:

```bash
chmod +x hello.sh
```

Run it:

```bash
./hello.sh
```

### Understanding the script

- `#!/usr/bin/env bash` identifies Bash as the interpreter.
- `echo` displays text.
- `$(whoami)` executes a command and inserts its output.
- `$(pwd)` displays the current directory.
- `$(date)` displays the current date.
- `chmod +x` adds executable permission.

Shell scripting will be covered in greater detail in a later lesson.

## 14. The Kernel and Shell in Cybersecurity

Understanding the kernel and shell is important for cybersecurity professionals.

### Kernel security

Cybersecurity researchers study the kernel to understand:

- Privilege boundaries.
- Process isolation.
- Memory protection.
- Kernel vulnerabilities.
- Device and driver security.
- System-level monitoring.

A kernel vulnerability may allow an attacker to affect the operating system at a highly privileged level.

### Shell security

The shell is important because it is frequently used for system administration and automation.

Security professionals use shells to:

- Inspect system configuration.
- Monitor processes.
- Review logs.
- Analyse network settings.
- Automate security checks.
- Investigate suspicious activity.

### Example: Inspect running processes

```bash
ps aux
```

This displays running processes.

### Example: Inspect listening network sockets

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

### Example: Inspect kernel messages

```bash
dmesg | tail
```

This displays recent kernel messages, subject to the system's access restrictions.

**Important:** A shell is powerful, so commands should be understood before execution. Never run unknown scripts with root privileges.

## 15. Practical Lab: Exploring the Kernel and Shell

**Lab objective:** Identify your Linux kernel, determine which shell you are using, and practise basic shell operations.

Use your Kali Linux virtual machine.

### Part A – Kernel inspection

**Step 1: Display kernel information**

```bash
uname -a
```

**Step 2: Display the kernel version**

```bash
uname -r
```

**Step 3: Display the machine architecture**

```bash
uname -m
```

**Step 4: Display loaded kernel modules**

```bash
lsmod | head
```

**Step 5: Display kernel messages**

```bash
dmesg | tail
```

Some systems restrict access to kernel messages. If access is denied, continue with the remaining tasks.

### Part B – Shell inspection

**Step 1: Identify the configured shell**

```bash
echo "$SHELL"
```

**Step 2: Identify the current shell process**

```bash
ps -p $$ -o comm=
```

**Step 3: Check Bash version**

```bash
bash --version
```

**Step 4: View available shells**

```bash
cat /etc/shells
```

### Part C – Bash variables

Run the following commands:

```bash
name="Linux Student"
```

```bash
echo "$name"
```

Create an environment variable:

```bash
export COURSE="Linux for Ethical Hackers"
```

Display it:

```bash
echo "$COURSE"
```

### Part D – Create a simple shell script

Create a file:

```bash
nano system-report.sh
```

Enter:

```bash
#!/usr/bin/env bash

echo "===== LINUX SYSTEM REPORT ====="
echo "User: $(whoami)"
echo "Kernel: $(uname -r)"
echo "Architecture: $(uname -m)"
echo "Shell: $SHELL"
echo "Date: $(date)"
echo "================================"
```

Save the file.

Make it executable:

```bash
chmod +x system-report.sh
```

Execute it:

```bash
./system-report.sh
```

Observe the information displayed.

## 16. Important Terms

| Term | Meaning |
|---|---|
| Kernel | Core component of an operating system |
| Shell | Program that interprets commands |
| Terminal | Application used to interact with a shell |
| Kernel Space | Privileged environment where the kernel executes |
| User Space | Environment where ordinary applications execute |
| System Call | Mechanism for requesting kernel services |
| Bash | Bourne Again Shell |
| Zsh | Z Shell |
| Fish | Friendly Interactive Shell |
| Variable | Named storage for a value |
| Environment Variable | Variable made available to child processes |
| Shell Script | File containing commands for a shell to execute |
| Kernel Module | Code that can be loaded into or removed from the kernel |

## 17. Knowledge Check

Answer these questions without looking at the lesson.

1. What is the Linux kernel?
2. State five responsibilities of the kernel.
3. What is kernel space?
4. What is user space?
5. Define a shell.
6. Name four Linux shells.
7. What is the difference between a shell and a terminal?
8. What is a system call?
9. What does the `uname -r` command display?
10. What is Bash scripting used for?
11. What does the `$PATH` environment variable contain?
12. Why is understanding the kernel important in cybersecurity?

## 18. Practical Assignment

Complete the following tasks:

1. Open your Kali Linux terminal.
2. Identify your kernel version using `uname -r`.
3. Identify your machine architecture using `uname -m`.
4. Identify your current shell.
5. Display the available shells on your system.
6. Create a Bash variable named `student`.
7. Store your preferred study label in the variable.
8. Display the variable using `echo`.
9. Create a Bash script that displays your username, kernel version, shell, and current date.
10. Run the script and record its output.

Save your assignment as:

`Linux-Kernel-and-Shell-Assignment.md`

## 19. Exam-Ready Summary

The Linux kernel is the core component of a Linux-based operating system. It manages processes, memory, devices, filesystems, networking, and security. It operates in kernel space, while ordinary applications execute in user space. A shell is a program that provides an interface for entering commands and executing programs. Common Linux shells include Bash, Sh, Zsh, and Fish. A terminal provides the interface through which users interact with a shell. Applications request kernel services through system calls. Shell scripting allows users to automate tasks using commands, variables, functions, and control structures.

## 20. Lesson Conclusion

The kernel and shell are two fundamental components of Linux.

The kernel manages the computer's resources and provides essential operating-system services. The shell allows users to interact with the system through commands and scripts.

As you continue learning Linux for ethical hacking, you will use the shell to inspect systems, manage files, automate tasks, monitor processes, and perform security operations.

**Next Lesson:** Lesson 08 – Open Source and GNU.