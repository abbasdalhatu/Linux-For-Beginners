# Module 01 – Lesson 05: Linux Architecture

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Linux Fundamentals  
**Lesson:** 05 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

Linux architecture describes how the different components of a Linux operating system are organised and how they interact with one another.

A Linux system contains several layers, including hardware, the kernel, system libraries, system utilities, shells, and applications.

Each layer has a specific responsibility, but all the layers work together to make the operating system function.

Understanding Linux architecture is important for ethical hackers because it helps explain how applications communicate with the kernel, how hardware resources are accessed, and where security controls operate.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Define Linux architecture.
2. Identify the major layers of Linux architecture.
3. Explain the role of hardware.
4. Understand the responsibilities of the Linux kernel.
5. Explain system calls and system libraries.
6. Understand the role of shells and system utilities.
7. Describe how applications interact with the kernel.
8. Explain why Linux architecture matters in cybersecurity.

## 3. What Is Linux Architecture?

Linux architecture is the organisation of the components that make up a Linux-based operating system and the way those components interact.

A simplified Linux architecture can be represented as follows:

<AsyncImage query="Linux operating system architecture diagram hardware kernel system libraries shell applications layers" aspectRatio="16:9" width="100%" />

The major components include:

1. Hardware.
2. Linux kernel.
3. System call interface.
4. System libraries.
5. System utilities and services.
6. Shell and user interfaces.
7. Application software.

These components communicate to provide a functional operating environment.

## 4. Layer 1: Hardware

Hardware refers to the physical components of a computer.

Examples include:

- CPU.
- RAM.
- Hard disk drives.
- Solid-state drives.
- Network adapters.
- Keyboard.
- Mouse.
- Display.

The hardware provides the physical resources that the operating system manages.

### 4.1 Role of Hardware in Linux

The hardware performs the actual computing operations.

For example:

- The CPU executes instructions.
- RAM temporarily stores data used by running programs.
- Storage devices retain files.
- Network adapters transmit and receive network data.

Linux uses kernel components and device drivers to communicate with hardware.

## 5. Layer 2: Linux Kernel

The Linux kernel is the core component of the operating system.

It manages communication between software and hardware and controls important system resources.

### 5.1 Main Responsibilities of the Kernel

**1. Process Management**

Creates, schedules, and terminates processes.

**2. Memory Management**

Allocates memory and manages virtual memory.

**3. Device Management**

Communicates with hardware through device drivers.

**4. Filesystem Management**

Provides filesystem interfaces and manages access to stored data.

**5. Networking**

Implements important networking functionality, including network protocols and socket interfaces.

**6. Security and Access Control**

Enforces important operating-system security mechanisms, including permissions and privilege boundaries.

### 5.2 Kernel Space

Kernel space is the protected memory area in which the kernel executes.

It has privileged access to important system resources.

### 5.3 User Space

User space is where ordinary applications and many system programs execute.

Applications generally cannot directly access protected kernel memory.

This separation helps improve system stability and security.

## 6. Layer 3: System Call Interface

System calls provide a controlled way for user-space programs to request services from the kernel.

For example, an application may need to:

- Open a file.
- Read data.
- Write data.
- Create a process.
- Communicate over a network.
- Allocate memory.

It requests these services through system calls.

### 6.1 Example of File Access

Imagine opening a text file using a text editor.

The simplified process is:

1. You select the file in the application.
2. The application requests access to the file.
3. The request reaches the kernel through a system call.
4. The kernel checks permissions and handles the filesystem operation.
5. The application receives the requested data if access is permitted.

The application does not normally access the physical storage device directly.

## 7. Layer 4: System Libraries

System libraries provide reusable functions that applications can use to access operating-system services.

They help programmers avoid implementing low-level operations from scratch.

One important example is the GNU C Library (glibc).

### 7.1 What Do System Libraries Do?

They provide functions for tasks such as:

- File operations.
- Memory management.
- String handling.
- Process interaction.
- System-call interfaces.
- Networking operations.

### 7.2 Relationship Between Libraries and System Calls

A library function may internally invoke one or more system calls.

For example, a program can use a high-level file operation provided by a library, which eventually requests the kernel to open or read a file.

Not every library function requires a system call.

## 8. Layer 5: System Utilities and Services

System utilities are programs that help users and administrators manage the operating system.

Examples include:

| Utility | Purpose |
|---|---|
| `ls` | Lists directory contents |
| `cp` | Copies files |
| `mv` | Moves or renames files |
| `ps` | Displays process information |
| `ip` | Inspects network configuration |
| `systemctl` | Manages systemd services |
| `grep` | Searches text patterns |

System services are background programs that provide operating-system functionality.

Examples include networking services, logging services, and scheduled tasks.

## 9. Layer 6: Shell and User Interface

The shell provides a command-line environment through which users interact with the operating system.

Examples include:

- Bash.
- Zsh.
- Fish.

A shell reads commands, interprets them, and starts programs or requests operating-system services.

Example:

```bash
ls -lah
```

The shell interprets this command and runs the `ls` utility with the supplied options.

A graphical user interface can provide another way to interact with the operating system.

The shell is not the kernel. It is a user-space program.

## 10. Layer 7: Application Software

Applications are programs designed to perform tasks for users.

Examples include:

- Web browsers.
- Text editors.
- Programming tools.
- Database applications.
- Media players.
- Security tools.

Cybersecurity applications such as Nmap and Wireshark run in user space and rely on operating-system services to perform many of their functions.

## 11. How Linux Components Work Together

Consider the process of running a command:

```bash
cat /etc/hostname
```

A simplified sequence is:

1. You enter the command in the terminal.
2. The shell interprets the command.
3. The shell locates and executes the `cat` utility.
4. The utility requests access to the specified file.
5. The kernel handles the filesystem operation and checks access permissions.
6. The file data is returned to the utility.
7. The utility displays the content in your terminal.

This demonstrates how user-space programs interact with kernel services.

## 12. Important Linux Architecture Concepts

### 12.1 Multitasking

Linux can manage multiple processes that make progress concurrently.

The kernel schedules CPU time among runnable processes.

### 12.2 Multiuser Support

Linux supports multiple user accounts with different permissions and access rights.

### 12.3 Device Drivers

Device drivers allow the kernel to communicate with hardware devices.

Some drivers are built into the kernel, while others can be loaded as kernel modules.

### 12.4 Kernel Modules

Kernel modules are components that can be loaded into or removed from a running kernel to provide additional functionality.

For example, a kernel module may provide support for a particular hardware device or filesystem.

### 12.5 Virtual Memory

Virtual memory gives processes their own virtual address spaces and allows the operating system to manage physical memory efficiently.

### 12.6 Filesystem Abstraction

Linux provides a common filesystem interface that allows different filesystem implementations to be accessed through a consistent directory structure.

## 13. Cybersecurity Application

Understanding Linux architecture helps security professionals identify how attacks and defensive controls interact with the operating system.

### 13.1 Privilege Escalation

An attacker may attempt to gain privileges beyond those assigned to their account.

Understanding kernel and user-space separation helps explain why privilege boundaries matter.

### 13.2 Kernel Vulnerabilities

A vulnerability in the kernel may affect critical system components because the kernel operates with high privileges.

### 13.3 Malicious Applications

Malicious applications generally execute in user space but may attempt to exploit vulnerabilities or misuse permissions to access restricted resources.

### 13.4 System Monitoring

Security analysts inspect processes, services, kernel information, and system logs to understand system behaviour.

### 13.5 System Hardening

Administrators can improve security by updating the kernel, controlling loaded modules, restricting privileges, and disabling unnecessary services.

Only conduct security testing on systems you own or have explicit permission to assess.

## 14. Practical Lab: Exploring Linux Architecture

Perform these exercises inside your Kali Linux virtual machine.

### Exercise 1: Identify the Kernel

```bash
uname -a
```

Record the kernel name, version, and architecture.

### Exercise 2: Inspect CPU Information

```bash
lscpu
```

Identify the CPU architecture and number of available CPUs.

### Exercise 3: Inspect Memory

```bash
free -h
```

Observe total, used, and available memory.

### Exercise 4: Inspect Running Processes

```bash
ps aux | head
```

Identify the processes displayed.

### Exercise 5: Identify Your Shell

```bash
echo "$SHELL"
```

Record the configured login shell.

### Exercise 6: Inspect Kernel Modules

```bash
lsmod | head
```

This displays a sample of currently loaded kernel modules.

### Exercise 7: Inspect System Calls

Check whether `strace` is available:

```bash
strace --version
```

If it is installed, run:

```bash
strace -e trace=openat cat /etc/hostname
```

Observe the system calls made while the program accesses the file.

If `strace` is unavailable, do not install anything yet. Record that result and continue.

### Exercise 8: Inspect Device Information

```bash
lsblk
```

Identify the storage devices visible to your Kali virtual machine.

## 15. Important Terms

| Term | Meaning |
|---|---|
| Architecture | Organisation of operating-system components |
| Kernel | Core component managing system resources |
| Kernel Space | Protected memory where kernel code executes |
| User Space | Environment where ordinary applications execute |
| System Call | Controlled request for a kernel service |
| System Library | Collection of reusable programming functions |
| Device Driver | Software that enables communication with hardware |
| Kernel Module | Component that can extend kernel functionality |
| Process | Running instance of a program |
| Shell | Program that interprets user commands |

## 16. Knowledge Check

Answer these questions in your notebook.

1. What is Linux architecture?
2. Mention seven major layers or components of Linux architecture.
3. What is the role of hardware?
4. Explain five responsibilities of the Linux kernel.
5. Differentiate between kernel space and user space.
6. What is a system call?
7. What are system libraries?
8. Explain the role of a device driver.
9. What is a kernel module?
10. How does a shell interact with the operating system?
11. Explain how Linux handles the command `cat /etc/hostname`.
12. Why is understanding Linux architecture important in cybersecurity?

## 17. Practical Assignment

Write a short report explaining how Linux components work together when a user runs a command.

Use this example:

```bash
cat /etc/hostname
```

Your report must explain:

1. The role of the terminal.
2. The role of the shell.
3. The role of the application.
4. The role of system libraries.
5. The role of system calls.
6. The role of the kernel.
7. How the result reaches the user.

## 18. Exam-Ready Summary

Linux architecture describes how hardware, the kernel, system-call interfaces, libraries, system utilities, shells, and applications work together. The kernel manages hardware and system resources, while user-space applications request kernel services through controlled interfaces such as system calls. System libraries provide reusable functions, device drivers enable hardware communication, and shells allow users to execute commands. Understanding this architecture is essential for Linux administration and cybersecurity.

## 19. Lesson Conclusion

You have learned how Linux is organised and how its major components interact.

You now understand the relationship between hardware, the kernel, system calls, libraries, shells, and applications.

This knowledge provides the foundation for understanding Linux security, process management, system administration, and ethical hacking.

**Next Lesson:** Lesson 06 – Linux Kernel and Shell.