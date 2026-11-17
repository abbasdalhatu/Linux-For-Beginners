---
---

# Module 01 – Lesson 01: Introduction to Linux

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Linux Fundamentals  
**Lesson:** 01 of 15  
**Level:** Beginner

---

## 1. Introduction

Linux is one of the most important operating systems in the world of computing and cybersecurity.

It is widely used in personal computers, servers, cloud infrastructure, supercomputers, embedded devices, and cybersecurity laboratories.

Linux is particularly important to ethical hackers because many cybersecurity tools and penetration-testing environments operate on Linux-based systems.

In this lesson, we will understand what Linux is, how it works, why it is important, and how it is used in the real world.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Define Linux.
2. Explain what an operating system is.
3. Understand the relationship between Linux and the Linux kernel.
4. Identify common uses of Linux.
5. Explain why cybersecurity professionals use Linux.
6. Understand the difference between Linux and a Linux distribution.

## 3. What Is Linux?

Linux is an open-source, Unix-like operating system kernel.

In everyday conversation, the word Linux is also commonly used to describe complete operating systems built around the Linux kernel.

A complete Linux-based operating system normally includes:

- The Linux kernel.
- System libraries.
- Command-line utilities.
- A package manager.
- Applications.
- A desktop environment, where applicable.

These components work together to provide a usable operating system.

**Simple definition:**

Linux is an open-source, Unix-like operating system family built around the Linux kernel.

### 3.1 Understanding Open Source

Open-source software is software whose source code is made available under a licence that allows people to use, study, modify, and redistribute it according to that licence.

Linux is open source, which means developers and organisations can inspect its source code, contribute improvements, and create their own distributions.

### 3.2 What Does Unix-Like Mean?

Unix-like systems follow many design principles associated with the original Unix operating system.

Examples include:

- Using a hierarchical filesystem.
- Supporting multiple users.
- Providing command-line interfaces.
- Using file permissions.
- Supporting processes and multitasking.

Linux is not Unix itself, but it follows many Unix design principles.

## 4. What Is an Operating System?

An operating system (OS) is system software that manages computer hardware and software resources and provides services for applications.

Examples of operating systems include:

- Microsoft Windows.
- Linux distributions.
- macOS.
- Android.
- iOS.

An operating system acts as an intermediary between applications, users, and computer hardware.

### 4.1 Main Functions of an Operating System

**1. Process Management**

Controls running programs and allocates CPU time.

**2. Memory Management**

Manages RAM and allocates memory to running applications.

**3. File Management**

Organises files and directories and controls access to stored data.

**4. Device Management**

Communicates with hardware devices such as keyboards, storage devices, and network adapters.

**5. Security and Access Control**

Manages user accounts, authentication, permissions, and other security mechanisms.

**6. User Interface**

Provides ways for users to interact with the computer, such as graphical interfaces and command-line terminals.

## 5. Understanding the Linux Kernel

The kernel is the central component of an operating system.

It manages communication between software and hardware.

The Linux kernel handles:

- Process scheduling.
- Memory management.
- Device drivers.
- Filesystem support.
- Networking.
- System calls.
- Hardware resource management.

When an application needs access to a hardware resource, it normally requests the service through the operating system's interfaces, including system calls.

**Example:**

When you open a file in Linux, the application requests the operating system to access the file. The kernel manages the underlying filesystem and storage operations.

We will study the Linux kernel in greater detail in a later lesson.

## 6. Where Is Linux Used?

Linux is used in many areas of technology.

### 6.1 Servers

Many web servers, database servers, and application servers run Linux.

### 6.2 Cloud Computing

Cloud platforms use Linux extensively to host applications, virtual machines, containers, and infrastructure services.

### 6.3 Cybersecurity

Linux is widely used for:

- Penetration testing.
- Network analysis.
- Digital forensics.
- Security monitoring.
- Vulnerability assessment.
- Incident response.

### 6.4 Supercomputers

Linux is widely used in high-performance computing because it can be customised for demanding workloads.

### 6.5 Embedded Systems

Linux can power routers, smart devices, industrial systems, and other specialised equipment.

### 6.6 Mobile Devices

Android uses the Linux kernel, although Android is a distinct operating system platform with its own architecture and software environment.

## 7. Why Is Linux Important in Cybersecurity?

Linux provides powerful tools and features that help security professionals understand and examine computer systems.

Some important reasons include:

**1. Command-Line Control**

Linux provides commands that allow professionals to inspect files, processes, users, permissions, and network activity.

**2. Security Tools**

Many security tools are available for Linux distributions.

Examples include:

- Nmap – Network discovery and security auditing.
- Wireshark – Network protocol analysis.
- Metasploit Framework – Security testing framework.
- Burp Suite – Web application security testing.
- tcpdump – Command-line packet analysis.

These tools must only be used against systems you own or have explicit permission to test.

**3. Customisation**

Linux allows users to configure the operating system and install tools according to their requirements.

**4. System Administration**

Linux provides strong command-line and scripting capabilities for managing users, services, processes, and networks.

**5. Open-Source Ecosystem**

The availability of source code and community-developed tools supports learning, research, and security auditing.

## 8. Introduction to Linux Distributions

A Linux distribution, commonly called a distro, is a complete operating system built around the Linux kernel.

A distribution combines the kernel with system utilities, libraries, applications, package-management tools, and configuration choices.

Examples include:

| Distribution | Common Use |
|---|---|
| Ubuntu | Desktop and server computing |
| Debian | Stable general-purpose systems |
| Kali Linux | Penetration testing and security auditing |
| Fedora | Desktop and development |
| Arch Linux | Customisable Linux environment |
| Linux Mint | Beginner-friendly desktop computing |

### 8.1 Why Do Different Distributions Exist?

Different distributions are designed for different users and purposes.

For example:

- Ubuntu focuses on accessibility and general-purpose computing.
- Debian is known for its stability and broad software ecosystem.
- Kali Linux provides a collection of tools for authorised security testing.
- Arch Linux gives users extensive control over system configuration.

We will study distributions and Kali Linux in more detail in upcoming lessons.

## 9. Practical Lab: Your First Linux Inspection

Perform these exercises inside your Kali Linux virtual machine.

### Exercise 1: Open the Terminal

Open the Kali terminal.

The terminal allows you to interact with Linux using commands.

### Exercise 2: Identify Your User

Run:

```bash
whoami
```

This displays the username of the current user.

### Exercise 3: Identify Your System

Run:

```bash
uname -a
```

This displays information about the kernel and system architecture.

### Exercise 4: Check the Linux Distribution

Run:

```bash
cat /etc/os-release
```

This displays information about your installed Linux distribution.

### Exercise 5: Check the Kernel Version

Run:

```bash
uname -r
```

This displays the Linux kernel release version.

### Exercise 6: Display the Current Directory

Run:

```bash
pwd
```

This displays your current working directory.

### Exercise 7: Display the Current Date

Run:

```bash
date
```

This displays the system date and time.

## 10. Important Linux Terms

| Term | Meaning |
|---|---|
| Linux | Operating system family built around the Linux kernel |
| Kernel | Core component that manages hardware and system resources |
| Distribution | Complete operating system built around the Linux kernel |
| Open Source | Software distributed under licences allowing access to source code and specified freedoms |
| Terminal | Interface used to interact with the system |
| Shell | Program that interprets commands |
| Command | Instruction given to the operating system through a shell or utility |
| Root | The superuser account, normally with UID 0 |
| Filesystem | Method used to organise and manage stored data |
| Process | A running instance of a program |

## 11. Cybersecurity Application

Imagine you are a junior cybersecurity analyst investigating a Linux computer.

You may need to:

1. Identify the operating system.
2. Determine the kernel version.
3. Identify the current user.
4. Inspect running processes.
5. Examine filesystem permissions.
6. Investigate network connections.

Linux provides commands and system interfaces that help you perform these investigations.

This is why understanding Linux fundamentals is essential before learning penetration testing.

## 12. Knowledge Check

Answer these questions in your notebook.

1. What is Linux?
2. What is an operating system?
3. What is the Linux kernel?
4. Explain the meaning of open-source software.
5. Mention five functions of an operating system.
6. What is a Linux distribution?
7. Give five examples of Linux distributions.
8. Mention five areas where Linux is used.
9. Why is Linux important in cybersecurity?
10. What is the difference between a kernel and a complete Linux distribution?

## 13. Practical Assignment

Complete the following tasks:

1. Open your Kali Linux terminal.
2. Run `whoami`.
3. Run `uname -a`.
4. Run `cat /etc/os-release`.
5. Run `uname -r`.
6. Run `pwd`.
7. Record the output of each command.
8. Write a short explanation of what you learned.

Do not modify system settings during this introductory exercise.

## 14. Exam-Ready Summary

Linux is an open-source, Unix-like operating system family built around the Linux kernel. The kernel manages computer hardware and system resources, while a complete Linux distribution combines the kernel with system utilities, libraries, and applications. Linux is widely used in servers, cloud computing, embedded systems, and cybersecurity because of its flexibility, customisation, command-line capabilities, and extensive software ecosystem.

## 15. Lesson Conclusion

In this lesson, you learned what Linux is, what an operating system does, how the Linux kernel works at a basic level, and why Linux is important in modern computing and cybersecurity.

You also performed your first Linux system inspection using basic terminal commands.

**Next Lesson:** Lesson 02 – History and Evolution of Linux.