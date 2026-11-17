---
---

# Lesson 04: Understanding System Calls

**Course:** Linux for Ethical Hackers  
**Module:** 04 – Linux Kernel and Shell  
**Lesson:** 04 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

In the previous lesson, we learned that Linux separates execution into two environments:

- Kernel Space
- User Space

We also learned that ordinary applications cannot directly perform every privileged operation.

So, how does an application request services from the kernel?

The answer is **system calls**.

System calls provide a controlled interface through which user-space programs request services from the Linux kernel.

They are essential to almost everything a Linux computer does.

For ethical hackers, understanding system calls helps explain how applications interact with the operating system, how security controls are enforced and how attackers may attempt to abuse vulnerable interfaces.

## 2. What Is a System Call?

A **system call** is a controlled mechanism that allows a user-space program to request a service from the operating-system kernel.

Applications use system calls when they need services such as:

- Opening files.
- Reading data.
- Writing data.
- Creating processes.
- Communicating over networks.
- Managing memory.
- Changing process information.

Think of a system call as a service request.

Imagine visiting a bank:

1. You are a customer.
2. You request a service from the bank.
3. The bank checks your identity and permissions.
4. The bank processes your request.
5. You receive a result.

Similarly, an application requests a kernel service, the kernel performs the necessary checks, and the application receives a result.

## 3. Why Do System Calls Exist?

Applications need access to resources that are managed by the kernel.

Without system calls, applications would not have a standard controlled way to request many important operating-system services.

System calls provide:

1. Controlled access to hardware and system resources.
2. A boundary between user space and kernel space.
3. Standard interfaces for operating-system services.
4. A mechanism for enforcing permissions.
5. A way for applications to communicate with kernel functionality.

## 4. How a System Call Works

Consider a program that wants to read a file.

The simplified process is:

1. The application runs in user space.
2. It requests a file operation.
3. A library function or program interface prepares the request.
4. The system call mechanism transfers execution to the kernel.
5. The kernel checks the request and performs the operation if permitted.
6. The kernel returns a result.
7. The application continues executing in user space.

### Simplified Diagram

```text
+------------------------------------+
|            USER SPACE              |
|                                    |
|        Application Program         |
+------------------------------------+
                  |
                  v
+------------------------------------+
|        LIBRARY / API INTERFACE     |
+------------------------------------+
                  |
                  v
+------------------------------------+
|          SYSTEM CALL               |
+------------------------------------+
                  |
                  v
+------------------------------------+
|            KERNEL SPACE            |
|                                    |
|  Validate Request                  |
|  Check Permissions                 |
|  Perform Operation                 |
+------------------------------------+
                  |
                  v
+------------------------------------+
|          RESULT RETURNED           |
+------------------------------------+
                  |
                  v
+------------------------------------+
|         APPLICATION CONTINUES      |
+------------------------------------+
```

This is a simplified explanation. Real system-call execution involves architecture-specific instructions, registers and kernel entry mechanisms.

## 5. Categories of Linux System Calls

Linux provides many system calls. We can group them according to their purpose.

### 5.1 Process Control System Calls

These system calls manage processes.

Examples:

| System Call | Purpose |
|---|---|
| `fork()` | Creates a child process |
| `execve()` | Executes a program |
| `wait4()` | Waits for a child process and collects its status |
| `exit()` | Terminates a process |
| `getpid()` | Returns the calling process ID |

Example:

A shell creates a process when it launches many external commands.

### 5.2 File Management System Calls

These system calls handle file operations.

| System Call | Purpose |
|---|---|
| `openat()` | Opens a file relative to a directory file descriptor |
| `read()` | Reads data |
| `write()` | Writes data |
| `close()` | Closes a file descriptor |
| `unlinkat()` | Removes a directory entry |

Example:

When a text editor opens a document, it eventually uses kernel file-operation interfaces.

### 5.3 Memory Management System Calls

These system calls manage memory mappings.

| System Call | Purpose |
|---|---|
| `mmap()` | Creates a memory mapping |
| `munmap()` | Removes a memory mapping |
| `mprotect()` | Changes memory protection |
| `brk()` | Adjusts the process data segment boundary |

### 5.4 Network Communication System Calls

These system calls support network communication through sockets.

| System Call | Purpose |
|---|---|
| `socket()` | Creates a socket |
| `bind()` | Assigns a local address to a socket |
| `listen()` | Marks a socket as accepting connections |
| `accept()` | Accepts a connection |
| `connect()` | Initiates a connection |
| `sendto()` | Sends data through a socket |
| `recvfrom()` | Receives data through a socket |

These are especially relevant in cybersecurity because network applications depend on socket operations.

### 5.5 Security and Permission-Related System Calls

Some system calls interact with identity and permission information.

Examples include:

| System Call | Purpose |
|---|---|
| `getuid()` | Retrieves the real user ID |
| `geteuid()` | Retrieves the effective user ID |
| `setuid()` | Changes user identity, subject to permission rules |
| `chmod()` | Changes file permissions |
| `capget()` | Retrieves process capability information |

Important: System calls do not automatically grant permission. The kernel checks whether the requested operation is authorized.

## 6. System Calls vs Library Functions

System calls and library functions are related, but they are not the same thing.

A library function is a function provided by a software library.

A system call is an interface through which a program requests a kernel service.

For example, the C standard library provides `fopen()` for opening files.

Internally, the library may use lower-level system calls such as `openat()`.

| Feature | Library Function | System Call |
|---|---|---|
| Provided by | Software library | Kernel interface |
| Executes in | Usually user space | Enters kernel space |
| Main purpose | Provides reusable programming functionality | Requests kernel services |
| Example | `fopen()` | `openat()` |
| Overhead | Depends on the function | Involves a kernel transition |

A library function does not necessarily perform a system call. Some functions can complete entirely in user space.

## 7. Understanding File Descriptors

A file descriptor is a small non-negative integer used by a process to refer to an open file or another input/output resource.

Linux commonly uses these standard file descriptors:

| Number | Name | Purpose |
|---|---|---|
| 0 | stdin | Standard input |
| 1 | stdout | Standard output |
| 2 | stderr | Standard error |

For example, when a program opens a file, the kernel can return a file descriptor that the program uses for subsequent operations.

These descriptors are important when working with:

- Files.
- Pipes.
- Terminals.
- Sockets.

We will explore input/output redirection and pipes in Lesson 13.

## 8. The Linux System Call Table

Linux supports a large collection of system calls.

On many architectures, system calls are associated with numbers.

A system-call number identifies which operation the kernel should process.

The exact numbers and available system calls depend on the processor architecture and kernel version.

You can inspect system-call information through Linux documentation.

Useful command:

```bash
man 2 syscalls
```

The `2` indicates the system-call manual section.

You can also inspect individual system calls:

```bash
man 2 read
```

```bash
man 2 write
```

```bash
man 2 openat
```

## 9. Observing System Calls with strace

One of the most useful tools for learning system calls is `strace`.

**strace** is a diagnostic utility that traces system calls and signals made by a process.

It helps you understand what an application requests from the kernel.

### Installing strace

On Kali Linux:

```bash
sudo apt update
sudo apt install strace
```

Only install it if it is not already available.

### Example 1: Trace a Simple Command

```bash
strace ls
```

This displays the system calls made while executing `ls`.

The output may contain calls such as:

- `openat()`
- `read()`
- `write()`
- `close()`

### Example 2: Trace File Access

```bash
strace cat /etc/hostname
```

Observe the file-related system calls.

### Example 3: Trace Only File-Related Calls

```bash
strace -e trace=%file ls
```

This filters the trace to file-related system calls.

### Example 4: Trace Network-Related Calls

```bash
strace -e trace=%network COMMAND
```

Replace `COMMAND` with a program you own or are authorized to inspect.

### Example 5: Save Trace Output

```bash
strace -o system-calls.txt ls
```

This saves the trace output to `system-calls.txt`.

## 10. System Calls and Cybersecurity

System calls are important in security research because they provide insight into how programs interact with the operating system.

### 10.1 Malware Analysis

Analysts can examine system calls to understand what a suspicious program attempts to do.

For example, a program may:

- Open files.
- Create processes.
- Access network sockets.
- Change file permissions.

System-call traces can provide useful behavioural evidence, although they do not prove that a program is malicious.

### 10.2 Detecting Suspicious Activity

Unexpected file access, process creation or network communication may indicate suspicious behaviour.

Security monitoring systems can use operating-system events and related telemetry to investigate these activities.

### 10.3 Privilege Escalation

Some vulnerabilities involve incorrect handling of system calls or weaknesses in kernel components reached through them.

A successful exploit may attempt to bypass security checks or gain additional privileges.

### 10.4 Sandboxing

Applications can be restricted through sandboxing mechanisms.

Linux provides tools and mechanisms such as:

- Seccomp.
- Namespaces.
- Capabilities.
- AppArmor.
- SELinux.

Seccomp can restrict which system calls a process is allowed to make.

This reduces the number of kernel interfaces available to a restricted process.

### 10.5 System Call Monitoring

Tools such as `strace` can help administrators investigate program behaviour.

However, `strace` is primarily a diagnostic tool, not a complete security monitoring solution.

## 11. Practical Lab: Exploring Linux System Calls

**Environment:** Kali Linux virtual machine

**Objective:** Observe how ordinary Linux commands interact with the kernel.

### Task 1: Check whether strace is installed

```bash
strace --version
```

If it is missing:

```bash
sudo apt update
sudo apt install strace
```

### Task 2: Trace the ls command

```bash
strace ls
```

Observe the system-call names and their return values.

### Task 3: Trace file access

```bash
strace cat /etc/hostname
```

Identify calls related to opening, reading and closing files.

### Task 4: Filter file-related system calls

```bash
strace -e trace=%file cat /etc/hostname
```

Compare the output with the previous command.

### Task 5: Filter process-related system calls

```bash
strace -e trace=%process ls
```

Observe how the command is executed.

### Task 6: Save a system-call trace

```bash
strace -o lesson04-trace.txt cat /etc/hostname
```

Read the saved trace:

```bash
cat lesson04-trace.txt
```

### Task 7: Examine a system-call manual page

```bash
man 2 read
```

Identify:

- The purpose of `read()`.
- Its arguments.
- Its return value.
- Possible errors.

Press `q` to exit the manual page.

### Task 8: Count the system calls in your trace

```bash
wc -l lesson04-trace.txt
```

This counts the lines in the saved trace. Each line is not necessarily a separate successful system call, because traces can also include signals and unfinished or resumed calls.

## 12. Important Terms

| Term | Meaning |
|---|---|
| System Call | Controlled request for a kernel service |
| Kernel Interface | Mechanism through which software interacts with kernel functionality |
| `fork()` | Creates a child process |
| `execve()` | Executes a program |
| `read()` | Reads data |
| `write()` | Writes data |
| `openat()` | Opens a file relative to a directory file descriptor |
| Socket | Communication endpoint |
| File Descriptor | Integer identifying an open file or I/O resource |
| strace | Utility for tracing system calls and signals |
| Seccomp | Linux mechanism for restricting system calls |

## 13. Knowledge Check

Answer these questions in your own words.

1. What is a system call?
2. Why do applications need system calls?
3. Explain how a system call works.
4. What is the difference between a library function and a system call?
5. What is the purpose of `fork()`?
6. What does `execve()` do?
7. Explain the purpose of `read()` and `write()`.
8. What is a file descriptor?
9. What is the purpose of `strace`?
10. How can system-call monitoring assist malware analysis?
11. What is Seccomp?
12. Why should applications have restricted access to unnecessary system calls?

## 14. Practical Assignment

Create a report titled:

**Linux System Call Investigation**

Include:

1. The version of `strace` installed on your Kali VM.
2. A trace of the `ls` command.
3. A trace of `cat /etc/hostname`.
4. Five system calls you observed.
5. The purpose of each system call.
6. An explanation of the difference between `read()` and `write()`.
7. A short explanation of how system-call tracing can help cybersecurity analysts.

Save your trace file as:

`lesson04-trace.txt`

Save your report in this Markdown lesson file.

## 15. Exam-Ready Summary

A system call is a controlled interface through which a user-space program requests services from the Linux kernel.

System calls support process management, file operations, memory management, networking and other operating-system functions.

Examples include:

- `fork()`
- `execve()`
- `read()`
- `write()`
- `openat()`
- `close()`
- `socket()`
- `connect()`

System calls are different from library functions, although library functions may use system calls internally.

The `strace` utility allows users to observe system calls made by programs.

In cybersecurity, system-call knowledge is useful for malware analysis, behavioural investigation, sandboxing and understanding privilege boundaries.

## 16. Conclusion

You now understand how Linux applications communicate with the kernel through system calls.

You have also learned about common system-call categories, file descriptors and the `strace` utility.

This knowledge will help you understand how applications interact with privileged operating-system services.

**Next Lesson:** Lesson 05 – Kernel Modules and Device Drivers.