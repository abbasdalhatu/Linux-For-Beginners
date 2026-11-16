# Module 01: Linux Fundamentals

# Lesson 09 – Linux Advantages and Disadvantages

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 09 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

Linux is one of the most important operating systems in modern computing.

It is used in personal computers, servers, cloud infrastructure, supercomputers, embedded devices, networking equipment, and cybersecurity environments.

Linux has many advantages, including flexibility, customisation, stability, and powerful command-line tools. However, it also has limitations involving software compatibility, hardware support, and the learning curve for new users.

Understanding both its strengths and weaknesses helps you decide when Linux is the right operating system for a particular task.

As a future software engineer and ethical hacker, you should understand not only why Linux is popular but also when another operating system may be more suitable.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Identify the major advantages of Linux.
2. Explain the disadvantages and limitations of Linux.
3. Understand Linux performance and resource management.
4. Explain Linux security strengths and weaknesses.
5. Understand software and hardware compatibility.
6. Identify industries and environments where Linux is used.
7. Compare Linux with other operating systems.
8. Explain why Linux is valuable in cybersecurity.
9. Inspect basic system resources using Linux commands.

## 3. Major Advantages of Linux

### 3.1 Open-Source Ecosystem

Linux is built around an open-source kernel and a large ecosystem of open-source software.

Users and developers can inspect, modify, and redistribute software according to the applicable licences.

Benefits include:

- Learning how software works.
- Customising operating-system components.
- Contributing to projects.
- Developing specialised tools.
- Inspecting code for potential security issues.

**Example:** A developer can study the Linux kernel source code to understand how processes, memory, and hardware communication work.

### 3.2 Cost and Licensing Flexibility

Many Linux distributions can be downloaded and used without purchasing a conventional desktop operating-system licence.

Examples include:

- Ubuntu.
- Debian.
- Fedora.
- Linux Mint.
- Kali Linux.

However, some commercial Linux distributions, support services, and enterprise subscriptions have associated costs.

**Important:** Linux is not always completely free of cost. Software licences and commercial support arrangements differ.

### 3.3 Stability and Reliability

Linux is widely used in environments that require reliable operation.

Examples include:

- Web servers.
- Database servers.
- Cloud infrastructure.
- Network services.
- Embedded systems.

Its stability depends on proper administration, software quality, updates, and hardware configuration.

### 3.4 Strong Networking Capabilities

Linux provides powerful networking tools and services.

Examples include:

- `ip`
- `ss`
- `ping`
- `traceroute`
- `tcpdump`
- `curl`
- `dig`

These tools help administrators and security professionals inspect network configuration, test connectivity, and analyse traffic.

### 3.5 Customisation

Linux offers extensive customisation options.

Users can configure:

- Desktop environments.
- Window managers.
- Shells.
- Themes.
- System services.
- Kernel configurations.
- Software packages.

For example, Kali Linux commonly uses XFCE as its default desktop environment, but other desktop environments can also be used.

### 3.6 Powerful Command-Line Interface

Linux provides shells such as Bash and Zsh.

The command line makes it possible to:

- Automate administrative tasks.
- Manage files.
- Monitor processes.
- Configure services.
- Analyse logs.
- Perform security assessments.

Example:

```bash
ps aux | grep ssh
```

This searches the displayed process list for entries containing `ssh`.

### 3.7 Efficient Resource Management

Linux can run on systems with different hardware capabilities.

Depending on the distribution and configuration, it can operate effectively on:

- Low-resource computers.
- Personal computers.
- Powerful servers.
- Embedded devices.
- High-performance computing systems.

A lightweight Linux installation can use fewer resources than some desktop environments, although actual resource usage depends on the workload and configuration.

### 3.8 Strong Multiuser Support

Linux supports multiple user accounts and groups.

This allows administrators to:

- Assign different permissions.
- Separate user environments.
- Restrict access to files.
- Control administrative privileges.

Example:

```bash
whoami
```

Displays the current username.

### 3.9 Excellent Development Environment

Linux is widely used for software development.

It supports programming languages and development tools such as:

- Python.
- C and C++.
- Java.
- JavaScript and Node.js.
- Go.
- Rust.
- Git.
- Docker.

Many development tools are available through distribution repositories or official installation methods.

### 3.10 Excellent Server and Cloud Support

Linux is widely used in server infrastructure and cloud computing.

It supports technologies such as:

- Apache.
- Nginx.
- Docker.
- Kubernetes.
- OpenSSH.
- PostgreSQL.
- MySQL.

Linux is therefore an important platform for system administrators, DevOps engineers, and cloud engineers.

## 4. Major Disadvantages of Linux

Linux has many strengths, but it also has limitations.

### 4.1 Learning Curve

Beginners may find Linux difficult because it often requires learning:

- Terminal commands.
- File permissions.
- Directory structures.
- Package management.
- User administration.
- System configuration.

For example, a beginner who has only used graphical interfaces may need time to understand commands such as:

```bash
chmod
```

```bash
chown
```

```bash
grep
```

```bash
find
```

These commands become easier with regular practice.

### 4.2 Software Compatibility

Some commercial applications are designed primarily for Windows or macOS.

Examples include certain professional engineering applications, specialised business software, and desktop publishing tools.

Some applications may work through alternatives, compatibility layers, or virtual machines, but the experience is not always identical.

### 4.3 Hardware Compatibility

Most modern hardware is supported by Linux, but some devices may require additional drivers or configuration.

Possible challenges include:

- Certain Wi-Fi adapters.
- Specialised printers.
- Fingerprint readers.
- Proprietary graphics drivers.
- Some professional hardware.

Always check hardware compatibility before choosing a Linux distribution for a specific computer.

### 4.4 Gaming Compatibility

Linux gaming has improved significantly, especially through Steam and Proton.

However, some games may not work correctly because of:

- Anti-cheat restrictions.
- Unsupported launchers.
- Driver limitations.
- Compatibility problems.

Windows still has broader compatibility with many commercial PC games.

### 4.5 Distribution Differences

Linux has many distributions.

Although this provides flexibility, it can also create confusion.

Different distributions may use different:

- Package managers.
- Release models.
- Desktop environments.
- Configuration tools.
- Default software.

For example:

| Distribution | Package Manager |
|---|---|
| Kali Linux | APT |
| Ubuntu | APT |
| Fedora | DNF |
| Arch Linux | Pacman |
| Alpine Linux | APK |

Learning the differences takes time.

### 4.6 Troubleshooting Can Require Technical Knowledge

Some problems require users to understand logs, services, drivers, permissions, and configuration files.

For example, diagnosing a network service may require commands such as:

```bash
systemctl status ssh
```

And:

```bash
journalctl -u ssh
```

These tools are powerful, but beginners may need guidance to interpret their output.

### 4.7 Some Commercial Support Is Limited

Not every software vendor provides official Linux support.

Some commercial products have Linux versions, while others provide only Windows or macOS applications.

This can be a limitation in organisations that depend on specific proprietary software.

## 5. Linux Security: Strengths and Limitations

Linux is often considered a strong platform for security, but it is not automatically secure.

Its security depends on configuration, updates, software quality, and administration.

### Security strengths

**1. User and group permissions**

Linux supports ownership and permissions that help control access to files and directories.

**2. Privilege separation**

Ordinary users generally operate with fewer privileges than the administrator account.

**3. Software repositories**

Package managers can obtain software from configured repositories, which helps centralise software installation and updates.

**4. Security frameworks**

Linux supports security mechanisms such as:

- SELinux.
- AppArmor.
- Linux Security Modules.

**5. Firewall support**

Linux provides firewall functionality through tools and frameworks such as:

- nftables.
- UFW.
- iptables compatibility tools.

### Security limitations

Linux systems can still suffer from:

- Vulnerable applications.
- Weak passwords.
- Incorrect file permissions.
- Misconfigured services.
- Unpatched kernel vulnerabilities.
- Malware.
- Privilege-escalation vulnerabilities.

### Important security principle

No operating system is perfectly secure.

Security requires continuous maintenance, monitoring, correct configuration, and responsible user behaviour.

## 6. Linux Performance and Resource Management

Linux provides tools that allow administrators to monitor system performance.

Important system resources include:

### 6.1 CPU

The Central Processing Unit executes instructions and performs calculations.

Linux allows users to inspect CPU information using:

```bash
lscpu
```

### 6.2 RAM

Random Access Memory stores data and instructions currently used by running programs.

Inspect memory usage with:

```bash
free -h
```

### 6.3 Storage

Storage devices hold files, applications, and operating-system data.

Inspect filesystem usage:

```bash
df -h
```

Inspect storage devices:

```bash
lsblk
```

### 6.4 Running Processes

A process is a running instance of a program.

Display running processes:

```bash
ps aux
```

Display an interactive process monitor:

```bash
top
```

### 6.5 System Uptime

Uptime shows how long the system has been running.

```bash
uptime
```

These tools help administrators identify resource problems and understand system behaviour.

## 7. Where Linux Is Used

Linux is used in many areas of technology.

### 7.1 Servers

Linux powers many web servers, database systems, and network services.

### 7.2 Cloud Computing

Linux is widely used in cloud infrastructure and virtual machines.

### 7.3 Cybersecurity

Security professionals use Linux for:

- Network analysis.
- Vulnerability assessment.
- Digital forensics.
- Security monitoring.
- Penetration testing in authorised environments.

### 7.4 Supercomputers

Linux is widely used in high-performance computing environments because of its flexibility and suitability for specialised workloads.

### 7.5 Embedded Systems

Linux is used in some routers, smart devices, industrial systems, and other embedded environments.

### 7.6 Software Development

Developers use Linux to build, test, and deploy applications.

### 7.7 Android

Android uses a modified Linux kernel as part of its operating-system architecture.

Android is not simply a conventional Linux desktop distribution; it has its own framework, runtime, and application environment.

## 8. Linux vs Windows: Choosing the Right Operating System

Linux and Windows serve different needs, and the best choice depends on the task.

| Task | Commonly Suitable Choice |
|---|---|
| Learning Linux administration | Linux |
| Practising with Kali security tools | Kali Linux |
| General desktop productivity | Windows or Linux |
| Many commercial Windows applications | Windows |
| Web server administration | Linux is widely used |
| PowerShell administration | Windows |
| Bash scripting | Linux |
| Cloud infrastructure | Linux is widely used |
| Gaming compatibility | Windows generally has broader support |
| Windows enterprise administration | Windows |

This table describes common use cases, not absolute rules. Both operating systems support many development and administrative tasks.

## 9. Practical Lab: Inspecting Linux Strengths and Limitations

**Lab objective:** Explore your Kali Linux system and collect information about its hardware, performance, storage, and running processes.

Use your Kali Linux virtual machine.

### Part A – Identify system information

Run:

```bash
uname -a
```

This displays kernel and system information.

### Part B – Inspect CPU information

Run:

```bash
lscpu
```

Identify:

- CPU architecture.
- Number of CPUs visible to the virtual machine.
- CPU model information, if exposed by the virtual hardware.

### Part C – Inspect memory usage

Run:

```bash
free -h
```

Identify:

- Total memory.
- Used memory.
- Available memory.
- Swap information.

### Part D – Inspect storage

Run:

```bash
df -h
```

Identify the mounted filesystems and their available space.

Then run:

```bash
lsblk
```

Observe the storage devices and their partitions.

### Part E – Inspect running processes

Run:

```bash
ps aux | head
```

This displays a sample of running processes.

Then run:

```bash
top
```

Observe CPU and memory usage.

Press `q` to exit.

### Part F – Inspect system uptime

Run:

```bash
uptime
```

Observe how long the system has been running and the load averages.

### Part G – Inspect network information

Run:

```bash
ip addr
```

This displays network interfaces and assigned addresses.

**Safety note:** These commands inspect your own system. They do not require disabling security controls, changing system configuration, or attacking another device.

## 10. Important Terms

| Term | Meaning |
|---|---|
| Open Source | Software distributed under an open-source licence |
| Stability | Ability of a system to operate reliably |
| Compatibility | Ability of software or hardware to work with a system |
| Distribution | A complete operating-system package built around the Linux kernel |
| CPU | Central Processing Unit |
| RAM | Random Access Memory |
| Swap | Storage space used as an extension of virtual memory |
| Process | A running instance of a program |
| Filesystem | Method of organising and storing files |
| Cloud Computing | Delivery of computing resources over a network |
| Embedded System | Computer system built into a larger device |
| Privilege Escalation | Gaining higher permissions than originally authorised |

## 11. Knowledge Check

Answer these questions without looking at the lesson.

1. State five advantages of Linux.
2. State five disadvantages of Linux.
3. Why is Linux popular for server administration?
4. What does open-source mean?
5. Why can Linux have a learning curve for beginners?
6. What is hardware compatibility?
7. Name three Linux security mechanisms.
8. Is Linux completely immune to malware? Explain.
9. What is the purpose of the `lscpu` command?
10. What does `free -h` display?
11. What is the difference between RAM and storage?
12. Why is Linux widely used in cloud computing?
13. Why should ethical hackers understand Linux limitations?
14. When might Windows be a better choice than Linux?

## 12. Practical Assignment

Complete the following tasks:

1. Identify five advantages of Linux.
2. Identify five disadvantages of Linux.
3. Explain why Linux is popular in cybersecurity.
4. Compare Linux and Windows for software development.
5. Run `lscpu` and record the CPU information visible to your Kali virtual machine.
6. Run `free -h` and record memory usage.
7. Run `df -h` and record available storage.
8. Run `ps aux` and identify three running processes.
9. Explain why Linux is not automatically secure.
10. Write a short conclusion explaining when Linux is a suitable operating-system choice.

Save your work as:

`Linux-Advantages-and-Disadvantages-Assignment.md`

## 13. Exam-Ready Summary

Linux offers several advantages, including an open-source ecosystem, customisation, strong networking tools, multiuser support, scripting capabilities, and widespread use in servers and cloud computing. It is also popular in cybersecurity because it supports many security tools and provides a flexible command-line environment. However, Linux has limitations, including a learning curve, possible hardware and software compatibility issues, distribution differences, and limited support for some commercial applications and games. Linux is not automatically secure; proper configuration, updates, permissions, and monitoring are necessary. Its suitability depends on the user's requirements and the intended workload.

## 14. Lesson Conclusion

Linux is a powerful and flexible operating system with important advantages in software development, system administration, networking, cloud computing, and cybersecurity.

However, no operating system is perfect. Linux has limitations that users should understand before choosing it for a particular task.

The goal is not to claim that Linux is always better than Windows. The goal is to understand Linux well enough to use it effectively and recognise situations where another operating system may be more appropriate.

As an ethical hacker, understanding the strengths and weaknesses of Linux will help you make better technical decisions.

**Next Lesson:** Lesson 10 – Linux in Cybersecurity.