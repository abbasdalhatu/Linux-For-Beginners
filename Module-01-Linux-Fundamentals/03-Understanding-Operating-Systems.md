# Module 01 – Lesson 03: Understanding Operating Systems

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Linux Fundamentals  
**Lesson:** 03 of 15  
**Level:** Beginner

---

## 1. Introduction

Every computer requires an operating system to manage its hardware and software resources.

Without an operating system, applications would have no standard environment through which to communicate with computer hardware.

Operating systems control important activities such as memory allocation, process execution, file management, networking, and security.

For an ethical hacker, understanding operating systems is essential because security testing involves examining how systems operate, how users interact with them, and how resources are protected.

In this lesson, we will study operating-system concepts, architectures, types, components, and security responsibilities.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Define an operating system.
2. Explain the major functions of an operating system.
3. Identify the main components of an operating system.
4. Understand different operating-system types.
5. Explain kernel mode and user mode.
6. Differentiate between command-line and graphical interfaces.
7. Compare Linux, Windows, and macOS.
8. Explain why operating systems matter in cybersecurity.

## 3. What Is an Operating System?

An operating system (OS) is system software that manages computer hardware and software resources and provides common services for application programs.

It acts as an intermediary between users, applications, and hardware.

Examples include:

- Microsoft Windows.
- Linux distributions.
- macOS.
- Android.
- iOS.

### 3.1 Simple Explanation

Imagine a computer as an organisation.

- Hardware represents the physical equipment.
- Applications represent the workers performing tasks.
- The operating system acts as the manager coordinating resources and activities.

The operating system makes sure that programs can use the computer's resources in an organised way.

## 4. Main Functions of an Operating System

An operating system performs several important functions.

### 4.1 Process Management

A process is a running instance of a program.

The operating system manages processes by:

- Creating processes.
- Scheduling CPU time.
- Pausing and resuming processes.
- Terminating processes.
- Managing communication between processes.

Example:

When you open a browser, the operating system creates and manages the processes needed to run it.

### 4.2 Memory Management

Memory management controls how RAM is allocated and used.

The operating system:

- Allocates memory to processes.
- Tracks memory usage.
- Reclaims memory when processes finish.
- Provides memory protection.
- Manages virtual memory.

Example:

When you open several applications simultaneously, the operating system manages their memory requirements.

### 4.3 File Management

File management controls how information is stored and accessed.

It handles:

- Creating files.
- Deleting files.
- Organising directories.
- Reading and writing data.
- Managing permissions.
- Controlling file access.

Linux uses a hierarchical filesystem beginning at the root directory `/`.

### 4.4 Device Management

The operating system manages hardware devices through device drivers and other system interfaces.

Examples include:

- Keyboard.
- Mouse.
- Display.
- Storage devices.
- Network adapters.
- Printers.

### 4.5 Security and Access Control

Operating systems provide mechanisms that help protect resources.

These include:

- User authentication.
- File permissions.
- Process isolation.
- Access control.
- Security policies.
- Auditing and logging.

### 4.6 Networking

Operating systems provide networking capabilities that allow computers and applications to communicate.

Examples include:

- TCP/IP networking.
- Network interface management.
- Routing.
- Firewall integration.
- Network socket communication.

### 4.7 User Interface Management

The operating system provides ways for users to interact with the computer.

The two major interface types are:

- Command-Line Interface (CLI).
- Graphical User Interface (GUI).

## 5. Main Components of an Operating System

An operating system contains several components that work together.

### 5.1 Kernel

The kernel is the central component responsible for managing hardware and system resources.

It handles processes, memory, devices, filesystems, and networking.

### 5.2 Shell

A shell is a program that interprets commands and provides an interface between the user and operating-system services.

Examples include:

- Bash.
- Zsh.
- Fish.
- PowerShell.

### 5.3 File System

The filesystem organises data into files and directories and controls how stored information is accessed.

Examples of Linux-supported filesystems include:

- ext4.
- XFS.
- Btrfs.
- FAT32.
- exFAT.

### 5.4 Device Drivers

Device drivers allow the operating system to communicate with hardware devices.

For example, a network driver enables the operating system to interact with a network adapter.

### 5.5 System Libraries

System libraries provide reusable functions that applications can use to access operating-system services.

For example, many Linux applications use the GNU C Library (glibc).

### 5.6 System Utilities

System utilities are programs used to manage and inspect the operating system.

Linux examples include:

- `ls`
- `cp`
- `mv`
- `ps`
- `ip`
- `systemctl`

## 6. Types of Operating Systems

Operating systems can be classified according to their purpose and design.

### 6.1 Single-User Operating Systems

Designed primarily for one user at a time.

### 6.2 Multiuser Operating Systems

Allow multiple users to access system resources, sometimes simultaneously.

Linux and Unix-like systems are important examples.

### 6.3 Multitasking Operating Systems

Allow multiple processes to make progress concurrently through CPU scheduling and other mechanisms.

Modern Windows, Linux, and macOS systems support multitasking.

### 6.4 Multiprocessing Operating Systems

Support systems with multiple processing units or CPU cores.

They can schedule work across multiple processors or cores.

### 6.5 Real-Time Operating Systems (RTOS)

Designed to respond to events within defined timing requirements.

They are used in applications such as:

- Industrial control systems.
- Medical devices.
- Robotics.
- Automotive systems.

### 6.6 Distributed Operating-System Environments

Distributed computing environments coordinate resources across multiple networked computers.

Not every distributed system uses a single distributed operating-system kernel; many are built using networked operating systems and distributed software.

## 7. Kernel Mode and User Mode

Modern operating systems use privilege levels to protect critical system resources.

Two important execution modes are:

### 7.1 Kernel Mode

Kernel mode is a highly privileged execution mode.

Code running in kernel mode can access protected hardware and system resources.

The operating-system kernel and certain low-level components execute with these privileges.

### 7.2 User Mode

User mode is a restricted execution mode used by ordinary applications.

Applications cannot directly access all protected hardware and kernel memory.

When an application needs a privileged service, it requests the operating system through controlled mechanisms such as system calls.

### 7.3 Comparison

| Kernel Mode | User Mode |
|---|---|
| Highly privileged | Restricted privileges |
| Used by kernel code | Used by ordinary applications |
| Can access protected system resources | Accesses resources through controlled interfaces |
| Errors can affect the entire system | Application errors are generally more isolated |

**Security importance:** Privilege separation helps prevent ordinary applications from directly controlling critical operating-system resources.

## 8. Command-Line Interface and Graphical User Interface

Users interact with operating systems through different interfaces.

### 8.1 Command-Line Interface (CLI)

A CLI allows users to interact with a computer by entering text commands.

Linux example:

```bash
pwd
```

This displays the current working directory.

Advantages:

- Efficient for repetitive tasks.
- Supports automation.
- Provides detailed system control.
- Useful for remote administration.
- Widely used in cybersecurity.

### 8.2 Graphical User Interface (GUI)

A GUI allows users to interact with a computer through visual elements.

Examples include:

- Windows desktop.
- GNOME desktop.
- KDE Plasma desktop.
- macOS desktop.

Advantages:

- Easy for beginners.
- Uses windows, icons, menus, and pointers.
- Suitable for visual applications.
- Reduces the need to memorise commands.

### 8.3 CLI vs GUI

| CLI | GUI |
|---|---|
| Text-based | Visual |
| Commands are entered manually or scripted | Uses windows, menus, and icons |
| Highly useful for automation | Often easier for beginners |
| Common in server administration | Common in desktop computing |

Both interfaces are useful. Professional Linux users often combine them.

## 9. Comparing Linux, Windows, and macOS

| Feature | Linux | Windows | macOS |
|---|---|---|---|
| Developer | Open-source community and organisations | Microsoft | Apple |
| Kernel | Linux kernel | Windows NT kernel | XNU kernel |
| Source code | Open source | Mostly proprietary | Mostly proprietary |
| Common interface | CLI and GUI | GUI and PowerShell/CMD | GUI and Terminal |
| Common use | Servers, cloud, development, security | Desktop, business, gaming | Apple computers, development, creative work |
| Customisation | Extensive | More restricted | More restricted |

Each operating system has its own strengths and security model.

A cybersecurity professional should understand more than one operating system because real-world environments frequently combine Windows, Linux, macOS, and network devices.

## 10. Virtualisation and Operating Systems

Virtualisation allows multiple virtual computers to run on one physical computer.

A virtual machine behaves like a separate computer with virtual hardware and its own operating system.

For example, you can run Kali Linux inside VMware on a Windows 11 computer.

Important concepts:

- **Host OS:** The operating system installed on the physical computer.
- **Guest OS:** The operating system running inside a virtual machine.
- **Hypervisor:** Software that creates and manages virtual machines.

Virtualisation is useful for cybersecurity training because it allows isolated environments for authorised experiments.

However, virtual machines are not perfect security boundaries. Network settings and shared resources must be configured carefully.

## 11. Practical Lab: Understanding Your Operating Systems

Perform these exercises in your Kali Linux virtual machine.

### Exercise 1: Identify Your Operating System

```bash
cat /etc/os-release
```

Record the distribution name and version information.

### Exercise 2: Identify the Kernel

```bash
uname -s
uname -r
uname -m
```

These commands display:

- Kernel name.
- Kernel release.
- System architecture.

### Exercise 3: Inspect Running Processes

```bash
ps aux | head
```

This displays a sample of running processes.

### Exercise 4: Inspect Memory

```bash
free -h
```

This displays memory and swap usage.

### Exercise 5: Inspect CPU Information

```bash
lscpu
```

This displays CPU architecture and processor information.

### Exercise 6: Identify Your Shell

```bash
echo "$SHELL"
```

This displays the configured login shell for your user.

### Exercise 7: Inspect System Uptime

```bash
uptime
```

This displays how long the system has been running, along with load averages.

## 12. Cybersecurity Application

Understanding operating systems is essential in ethical hacking because security weaknesses often involve operating-system configurations and behaviours.

Examples include:

**1. Privilege Escalation**

An attacker may attempt to gain higher privileges by exploiting a vulnerability or misconfiguration.

**2. Process Investigation**

Security analysts inspect running processes to identify unusual or suspicious activity.

**3. File Permission Auditing**

Incorrect permissions can expose sensitive files or allow unauthorised modifications.

**4. Network Security**

Operating systems manage network interfaces, sockets, routing, and firewall integration.

**5. System Hardening**

Administrators configure operating systems to reduce unnecessary services, limit privileges, and improve security.

All security testing must be conducted on systems you own or have explicit permission to assess.

## 13. Knowledge Check

Answer these questions in your notebook.

1. Define an operating system.
2. Mention six major functions of an operating system.
3. What is process management?
4. Explain memory management.
5. What is the role of a device driver?
6. Differentiate between a kernel and a shell.
7. What is the difference between kernel mode and user mode?
8. Explain the difference between CLI and GUI.
9. What is a real-time operating system?
10. What is virtualisation?
11. Differentiate between a host OS and a guest OS.
12. Why should cybersecurity professionals understand operating systems?

## 14. Practical Assignment

Create a short comparison of your two environments:

- Windows 11 installed on your physical laptop.
- Kali Linux running inside VMware.

Include:

1. Operating-system name.
2. Kernel information.
3. Main interface.
4. How applications are installed.
5. How users are managed.
6. One security feature available in each system.

Use the commands and system information available to you. Do not assume the two systems use the same management tools.

## 15. Exam-Ready Summary

An operating system is system software that manages computer hardware and software resources and provides services to applications. Its major functions include process management, memory management, file management, device management, networking, security, and user-interface management. Important operating-system components include the kernel, shell, filesystems, device drivers, libraries, and system utilities. Operating systems use privilege separation, including kernel mode and user mode, to protect critical resources. Understanding these concepts is essential for system administration and cybersecurity.

## 16. Lesson Conclusion

You have learned the purpose of operating systems, their major functions, components, and different types.

You also studied kernel mode, user mode, CLI, GUI, and virtualisation.

These concepts will help you understand how Linux operates internally and how security professionals examine computer systems.

**Next Lesson:** Lesson 04 – Linux Distributions.