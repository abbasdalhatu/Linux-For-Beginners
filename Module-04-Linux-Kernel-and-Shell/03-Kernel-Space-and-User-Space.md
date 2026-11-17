---
---

# Lesson 03: Kernel Space and User Space

**Course:** Linux for Ethical Hackers  
**Module:** 04 – Linux Kernel and Shell  
**Lesson:** 03 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

One of the most important concepts in Linux is the separation between **kernel space** and **user space**.

Linux separates system operations into two major execution environments:

1. Kernel Space
2. User Space

This separation helps protect the operating system from applications that might crash, behave incorrectly or attempt unauthorized operations.

For cybersecurity professionals, understanding this separation is essential because many security vulnerabilities involve interactions between applications and privileged kernel operations.

## 2. What Is Kernel Space?

Kernel space is the privileged memory and execution environment where the Linux kernel operates.

The kernel has access to important system resources, including:

- CPU management facilities.
- Physical and virtual memory management.
- Hardware devices.
- Filesystem operations.
- Network operations.
- Security enforcement mechanisms.

Code executing in kernel space generally has extensive privileges.

For example, the kernel can communicate with hardware through device drivers and manage memory used by processes.

**Important:** Kernel space is not simply a folder or directory. It is an execution and memory-protection concept enforced by the processor and operating system.

## 3. What Is User Space?

User space is the environment where ordinary applications and user-level programs execute.

Examples include:

- Firefox.
- Google Chrome.
- Bash.
- Text editors.
- Python programs.
- Network utilities.
- Office applications.

Applications running in user space do not normally have unrestricted access to hardware or protected kernel memory.

Instead, they request services from the kernel through controlled interfaces.

For example, when a text editor opens a document, it requests the kernel to access the file.

## 4. Understanding the Difference

| Feature | Kernel Space | User Space |
|---|---|---|
| Main purpose | Run kernel code and manage system resources | Run ordinary applications |
| Privilege | Highly privileged | Restricted |
| Hardware access | Managed through kernel mechanisms | Normally indirect |
| Memory access | Access to kernel-managed memory | Restricted to permitted memory |
| Examples | Kernel scheduler, drivers | Bash, Firefox, Python |
| Failure impact | Can affect the entire operating system | Usually limited to the affected application |

## 5. Understanding CPU Privilege Levels

Modern processors provide privilege mechanisms that help separate operating-system code from ordinary applications.

On x86 and x86-64 processors, these privilege levels are commonly called **CPU rings**.

The most important levels are:

- Ring 0: Most privileged execution level, commonly used by the operating-system kernel.
- Ring 3: Least privileged execution level, commonly used by ordinary applications.

Linux normally executes kernel code at Ring 0 and user applications at Ring 3 on x86 systems.

This is a hardware-supported security boundary.

Other processor architectures use different privilege mechanisms, but the general idea is similar.

## 6. How User Applications Communicate With the Kernel

Applications need kernel services to perform many operations.

Linux provides an interface known as the **system call interface**.

A system call is a controlled request made by a user-space program to obtain a service from the kernel.

Examples include requests to:

- Open files.
- Read data.
- Write data.
- Create processes.
- Communicate through sockets.
- Allocate memory mappings.

Consider the following example:

When you open a file using a text editor:

1. The text editor runs in user space.
2. It requests access to the file.
3. A system call transfers execution into the kernel.
4. The kernel checks permissions and processes the request.
5. The kernel returns a result.
6. The application continues running in user space.

We will study system calls in detail in Lesson 04.

## 7. Visual Representation

```text
+-----------------------------------------+
|               USER SPACE                |
|                                         |
| Firefox | Bash | Python | Text Editor   |
|                                         |
|       Restricted Application Access    |
+-----------------------------------------+
                    |
                    |
             SYSTEM CALLS
                    |
                    v
+-----------------------------------------+
|              KERNEL SPACE               |
|                                         |
| Process Management                      |
| Memory Management                       |
| Filesystem Management                   |
| Networking                              |
| Device Drivers                          |
| Security Mechanisms                     |
|                                         |
|       Privileged Kernel Operations      |
+-----------------------------------------+
                    |
                    v
+-----------------------------------------+
|                 HARDWARE                |
|                                         |
| CPU | RAM | Storage | Network Devices   |
+-----------------------------------------+
```

## 8. Why Does Linux Separate Kernel Space and User Space?

The separation provides several security and stability benefits.

### 8.1 Memory Protection

Ordinary applications cannot freely access protected kernel memory.

This helps prevent one application from accidentally or deliberately modifying important kernel data.

### 8.2 System Stability

If an ordinary application crashes, the operating system can often continue operating.

For example, if Firefox crashes, your terminal may continue working.

A serious kernel failure, however, can affect the entire operating system.

### 8.3 Access Control

Applications must request privileged operations through controlled mechanisms.

The kernel can check whether an operation is permitted before carrying it out.

### 8.4 Process Isolation

Each process normally receives its own virtual address space.

This helps prevent one process from directly accessing another process's private memory.

### 8.5 Reduced Security Risk

Applications are restricted from performing many dangerous operations directly.

This limits the damage that an ordinary compromised application can cause, although it does not eliminate security risks.

## 9. What Happens When an Application Needs a Privileged Operation?

Consider an ordinary user trying to read a protected system file.

For example:

```bash
cat /etc/shadow
```

On a typical Linux installation, this operation is denied to ordinary users because the file contains sensitive password-hash information.

The kernel enforces filesystem permissions.

An administrator may use:

```bash
sudo cat /etc/shadow
```

This works only if the user is authorized to use `sudo` and the requested operation is permitted.

Notice that `sudo` does not remove the kernel's security mechanisms. The operation still passes through kernel-controlled interfaces and permission checks.

## 10. Understanding Privilege Escalation

Privilege escalation occurs when a user or process gains permissions beyond those originally assigned to it.

There are two major categories:

### 10.1 Vertical Privilege Escalation

A user or process gains a higher privilege level.

Example:

An ordinary user obtains administrator-level privileges.

### 10.2 Horizontal Privilege Escalation

A user gains access to another user's resources or account without necessarily obtaining administrator privileges.

Example:

One ordinary user accesses another user's private files through an authorization weakness.

### Why Kernel Space Matters

Some kernel vulnerabilities can allow attackers to cross privilege boundaries.

For example, a kernel vulnerability might allow an ordinary user to execute operations with elevated privileges.

This is one reason operating-system security updates are important.

## 11. Understanding User-Space and Kernel-Space Failures

| Failure | Possible Result |
|---|---|
| Browser crashes | Browser may close while Linux continues operating |
| Terminal crashes | Terminal session may end |
| User application runs out of memory | Application may be terminated |
| Kernel encounters a serious fault | System may crash or become unstable |
| Faulty kernel driver | Hardware or system functionality may fail |

Linux uses mechanisms such as the Out-Of-Memory Killer to handle certain memory-exhaustion situations.

## 12. Practical Commands for Observing Processes

You can use Linux commands to investigate processes and their execution environments.

### Command 1: Display your current user

```bash
whoami
```

### Command 2: Display your user ID and groups

```bash
id
```

### Command 3: Display running processes

```bash
ps aux
```

### Command 4: Display process information

```bash
ps -p $$ -o pid,ppid,user,comm
```

The `$$` variable represents the process ID of the current shell.

### Command 5: Display process status

```bash
cat /proc/self/status
```

This displays information about the process accessing the file.

### Command 6: Inspect memory information

```bash
cat /proc/meminfo
```

### Command 7: Inspect kernel information

```bash
uname -a
```

These commands help you observe the system, although they do not directly display every CPU privilege transition or memory-protection boundary.

## 13. Kernel Space and User Space in Cybersecurity

This concept is especially important in ethical hacking.

### 13.1 Exploit Development

Security researchers study how vulnerabilities affect application and kernel execution.

### 13.2 Privilege Escalation Testing

Ethical hackers examine whether weaknesses allow ordinary users to gain unauthorized privileges.

### 13.3 Malware Analysis

Some malware operates in user space, while more advanced threats may attempt to interact with kernel components.

### 13.4 Rootkits

A rootkit is a collection of techniques or tools designed to hide malicious activity or maintain unauthorized access.

Kernel-level rootkits are particularly dangerous because they may manipulate kernel behaviour.

### 13.5 System Hardening

Administrators use security controls to reduce the possibility of unauthorized access to privileged operations.

Examples include:

- Applying security updates.
- Restricting administrative privileges.
- Using trusted kernel modules.
- Enforcing application security policies.
- Monitoring system activity.

## 14. Practical Lab: Exploring User and Kernel Boundaries

**Environment:** Kali Linux virtual machine  
**Objective:** Understand the distinction between ordinary user operations and privileged system operations.

### Task 1: Identify your current user

```bash
whoami
```

Record the output.

### Task 2: Examine your permissions

```bash
id
```

Identify your user ID, primary group and supplementary groups.

### Task 3: Examine the current shell process

```bash
ps -p $$ -o pid,ppid,user,comm
```

Record the process ID and process name.

### Task 4: Examine process information

```bash
cat /proc/self/status
```

Look for:

- Name
- State
- PID
- PPID
- Uid
- Gid

### Task 5: Compare ordinary and privileged access

First, run:

```bash
cat /etc/shadow
```

Observe the result.

If your current user does not have permission, you should receive a permission-denied message.

If you are using an administrator account, the result may differ.

Now run:

```bash
sudo cat /etc/shadow
```

Only do this on your own Kali VM. Do not copy or share the displayed password hashes.

Observe how administrative authorization changes whether the operation is permitted.

### Task 6: Examine process information again

```bash
ps aux | head -n 15
```

Identify several processes running under different users.

### Task 7: Examine system memory

```bash
free -h
```

Record total and available memory.

### Lab Safety

This lesson is observational.

Do not attempt kernel exploitation, privilege-escalation exploits, rootkit installation or modification of kernel memory.

## 15. Important Terms

| Term | Meaning |
|---|---|
| Kernel Space | Privileged execution environment for kernel code |
| User Space | Execution environment for ordinary applications |
| Ring 0 | Highly privileged x86 CPU execution level |
| Ring 3 | Restricted x86 CPU execution level |
| System Call | Controlled request for a kernel service |
| Memory Protection | Mechanisms that restrict memory access |
| Process Isolation | Separation of process execution and memory |
| Privilege Escalation | Obtaining higher permissions |
| Rootkit | Techniques or tools designed to hide malicious activity or maintain unauthorized access |
| Kernel Vulnerability | Security weakness affecting kernel functionality |

## 16. Knowledge Check

Answer these questions in your own words.

1. What is kernel space?
2. What is user space?
3. Explain three differences between kernel space and user space.
4. What is Ring 0?
5. What is Ring 3?
6. Why do applications need system calls?
7. What happens when a normal application crashes?
8. Why is kernel memory protected?
9. Explain vertical and horizontal privilege escalation.
10. Why are kernel-level rootkits dangerous?

## 17. Practical Assignment

Create a report titled:

**Kernel Space and User Space Investigation**

Include:

1. Your current Linux username.
2. Your user ID and group information.
3. Your current shell process ID.
4. Five details obtained from `/proc/self/status`.
5. The result of attempting to read `/etc/shadow` without administrative privileges, if applicable.
6. The difference between ordinary and authorized administrative access.
7. A short explanation of why kernel space must be protected from ordinary applications.

## 18. Exam-Ready Summary

Linux separates system execution into kernel space and user space.

Kernel space is the privileged environment where the Linux kernel executes and manages hardware and system resources.

User space is where ordinary applications execute with restricted privileges.

On x86 systems, kernel code commonly operates at Ring 0, while ordinary applications operate at Ring 3.

Applications request kernel services through system calls.

This separation improves memory protection, process isolation, system stability and security.

Understanding these concepts is important when studying privilege escalation, kernel vulnerabilities, malware and operating-system hardening.

## 19. Conclusion

You have learned the difference between kernel space and user space, how applications communicate with the kernel, and why Linux separates privileged operations from ordinary application execution.

This knowledge provides an important foundation for understanding system calls.

**Next Lesson:** Lesson 04 – Understanding System Calls.