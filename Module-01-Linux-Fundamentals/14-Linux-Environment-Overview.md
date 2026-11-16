# Module 01: Linux Fundamentals

# Lesson 14 – Linux Environment Overview

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 14 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

A Linux environment is the complete working environment provided by a Linux operating system.

It includes the components, tools, directories, processes, services, and configurations that allow users and applications to interact with the computer.

When you open Kali Linux, you are interacting with a Linux environment.

This environment may include:

- A graphical desktop.
- A terminal and shell.
- A Linux filesystem.
- Running processes.
- Background services.
- Network interfaces.
- User accounts.
- System configuration files.
- Installed applications and security tools.

Understanding the Linux environment is important because ethical hackers must know where system information is stored, how applications operate, and how different system components communicate.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a Linux environment is.
2. Identify the main components of a Linux environment.
3. Understand graphical and command-line environments.
4. Identify important Linux directories.
5. Understand environment variables.
6. Explain processes and services.
7. Inspect system resources.
8. Identify network interfaces and IP addresses.
9. Understand basic system configuration.
10. Apply Linux environment inspection commands.

## 3. What Is a Linux Environment?

A Linux environment is the collection of software and system components that provide the facilities needed to use and manage a Linux computer.

Think of a Linux environment as a complete workspace.

For example, when you start your Kali Linux virtual machine, several components work together:

1. The kernel manages hardware and system resources.
2. The desktop environment provides a graphical interface.
3. The shell interprets commands.
4. The filesystem stores files and directories.
5. Processes execute programs.
6. Services perform background tasks.
7. Network components enable communication.

All these components form the Linux environment.

## 4. Main Components of a Linux Environment

### 4.1 Linux Kernel

The kernel is the core component of the operating system.

It manages:

- CPU scheduling.
- Memory management.
- Hardware communication.
- Device drivers.
- Processes.
- Filesystems.
- Network operations.

The kernel acts as an intermediary between applications and computer hardware.

Check your kernel version:

```bash
uname -r
```

Display detailed kernel information:

```bash
uname -a
```

### 4.2 Shell

A shell is a command interpreter that allows users to interact with the operating system through commands.

Common Linux shells include:

| Shell | Description |
|---|---|
| Bash | Bourne Again Shell; widely used |
| Zsh | Z Shell; offers extensive customisation |
| Sh | Traditional Unix shell interface |
| Fish | Friendly Interactive Shell |

Check your current shell:

```bash
echo "$SHELL"
```

Check the shell process currently running:

```bash
ps -p $$ -o comm=
```

### 4.3 Terminal

A terminal provides an interface for entering commands and viewing their output.

Examples include:

- Kali Terminal.
- GNOME Terminal.
- Xfce Terminal.
- Konsole.

Remember:

- Terminal: Provides the command-line interface.
- Shell: Interprets and executes commands.
- Kernel: Manages system resources and communicates with hardware.

### 4.4 Desktop Environment

A desktop environment provides the graphical interface of a Linux system.

It commonly includes:

- Desktop panels.
- Application menus.
- File managers.
- Window managers.
- Desktop settings.
- Graphical applications.

Kali Linux commonly uses XFCE by default, although GNOME and KDE Plasma are also available.

<text>Example of a Linux desktop environment:</text>

![Linux desktop environment](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5a/Xfce_4.14_desktop.png/1280px-Xfce_4.14_desktop.png)

The graphical desktop is optional. Linux can operate without a graphical desktop environment.

## 5. Understanding the Linux Filesystem

Linux uses a hierarchical filesystem structure.

Unlike Windows, where drives are commonly represented using letters such as `C:` and `D:`, Linux organises its filesystem under one main directory called the root directory.

The root directory is represented by:

```text
/
```

All directories and files are organised beneath it.

### 5.1 Important Linux Directories

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/home` | Ordinary users' home directories |
| `/root` | Root user's home directory |
| `/etc` | System-wide configuration files |
| `/bin` | Essential user command binaries |
| `/sbin` | Essential system administration binaries; may be merged into `/usr/sbin` |
| `/usr` | Many user applications, libraries, and shared resources |
| `/var` | Frequently changing data, logs, caches, and application data |
| `/tmp` | Temporary files |
| `/opt` | Optional third-party application software |
| `/dev` | Device files |
| `/proc` | Virtual filesystem exposing process and kernel information |
| `/sys` | Virtual filesystem exposing device and kernel information |
| `/boot` | Bootloader and kernel-related files |
| `/lib` | Essential shared libraries; may be merged into `/usr/lib` |
| `/mnt` | Common temporary mount location |
| `/media` | Common mount location for removable media |

### 5.2 Understanding Important Directories

**`/home`**

This directory contains the personal directories of ordinary users.

Example:

```bash
/home/kali
```

**`/etc`**

Contains system configuration files.

Examples:

```text
/etc/hostname
/etc/hosts
/etc/passwd
/etc/fstab
```

**`/var`**

Contains data that changes during normal system operation.

An important example is:

```text
/var/log
```

This directory commonly contains system and application logs.

**`/proc`**

A virtual filesystem that provides information about running processes and the kernel.

For example:

```bash
cat /proc/cpuinfo
```

**`/dev`**

Contains device files representing hardware and certain virtual devices.

Examples include:

```text
/dev/null
/dev/zero
/dev/sda
```

The exact devices depend on the system.

### 5.3 Explore the Filesystem

Display the current directory:

```bash
pwd
```

List the root directory:

```bash
ls /
```

Display directory information:

```bash
ls -la /etc
```

Inspect the home directory:

```bash
ls -la ~
```

Display filesystem structure:

```bash
ls -la /
```

You can also use `tree` if it is installed:

```bash
tree -L 2 /
```

This displays a limited number of directory levels. Some directories may produce permission warnings.

## 6. Understanding Environment Variables

Environment variables are named values that provide information to programs and processes.

They can influence how applications behave.

For example, a program may use an environment variable to determine:

- The current username.
- The user's home directory.
- The default shell.
- Where executable programs are located.
- Language and display preferences.

### 6.1 Common Environment Variables

| Variable | Purpose |
|---|---|
| `$USER` | Current username in the environment |
| `$HOME` | User's home directory |
| `$SHELL` | User's configured login shell |
| `$PATH` | Directories searched for executable commands |
| `$PWD` | Current working directory |
| `$LANG` | Language and locale setting |
| `$TERM` | Terminal type |

### 6.2 Display Environment Variables

Display the username:

```bash
echo "$USER"
```

Display the home directory:

```bash
echo "$HOME"
```

Display the shell:

```bash
echo "$SHELL"
```

Display the PATH variable:

```bash
echo "$PATH"
```

Display the current directory:

```bash
echo "$PWD"
```

Display all exported environment variables:

```bash
printenv
```

### 6.3 Create a Temporary Environment Variable

You can create your own shell variable:

```bash
COURSE="Linux for Ethical Hackers"
```

Display it:

```bash
echo "$COURSE"
```

To make it available to child processes:

```bash
export COURSE
```

This variable normally exists only for the current shell session unless you configure it to load automatically.

## 7. Understanding Linux Processes

A process is a program that is currently executing.

When you open a terminal, browser, file manager, or text editor, the operating system manages the running program as a process.

Each process has a Process ID called a PID.

### 7.1 Display Running Processes

Run:

```bash
ps
```

Display more detailed process information:

```bash
ps aux
```

Display processes in a live updating interface:

```bash
top
```

If installed, you can also use:

```bash
htop
```

### 7.2 Important Process Information

| Term | Meaning |
|---|---|
| PID | Process ID |
| PPID | Parent Process ID |
| CPU | Processor usage |
| Memory | RAM usage |
| USER | Account associated with the process |
| COMMAND | Program or command being executed |

### 7.3 Find a Specific Process

For example, search for processes containing the word `ssh`:

```bash
ps aux | grep ssh
```

The output may also include the `grep` command itself.

A more convenient option is:

```bash
pgrep -a ssh
```

This displays matching process IDs and command names when matches exist.

### 7.4 Understanding Process Termination

The `kill` command sends a signal to a process.

For example:

```bash
kill PID
```

Replace `PID` with the actual process ID.

By default, `kill` sends SIGTERM, which requests that the process terminate gracefully.

Do not terminate system processes unless you understand their purpose.

## 8. Understanding Linux Services

A service is a background program that provides functionality to the system or other applications.

Examples include:

- SSH services.
- Networking services.
- Web servers.
- Database services.
- Logging services.
- Time synchronisation services.

Many Linux distributions, including standard Kali installations, use systemd to manage services.

### 8.1 Check Whether Systemd Is Running

```bash
ps -p 1 -o comm=
```

On a typical systemd installation, the output is:

```text
systemd
```

Some Linux environments use different init systems.

### 8.2 Check Service Status

For example:

```bash
systemctl status ssh
```

The SSH service may not be installed or running on your system.

### 8.3 List Running Services

```bash
systemctl list-units --type=service --state=running
```

### 8.4 View Failed Services

```bash
systemctl --failed
```

### 8.5 Difference Between Processes and Services

| Processes | Services |
|---|---|
| Running instances of programs | Background programs providing system or application functions |
| May be started by users | Often managed by the service manager |
| Can be interactive or non-interactive | Commonly run in the background |
| Identified using PIDs | Often identified by service units |

A service is generally implemented using one or more processes, but not every process is a service.

## 9. Understanding System Resources

Linux needs system resources to operate.

The major resources include:

1. CPU.
2. RAM.
3. Storage.
4. Network interfaces.

### 9.1 CPU Information

The CPU executes program instructions.

Display CPU information:

```bash
lscpu
```

### 9.2 Memory Information

RAM temporarily stores information used by running programs.

Display memory usage:

```bash
free -h
```

The `-h` option displays values in human-readable units.

### 9.3 Storage Information

Check filesystem space:

```bash
df -h
```

Display storage devices:

```bash
lsblk
```

### 9.4 System Uptime

Display how long the system has been running:

```bash
uptime
```

### 9.5 Resource Monitoring

Run:

```bash
top
```

This allows you to monitor processes and system resource usage.

## 10. Understanding Linux Networking

Networking allows Linux systems to communicate with other devices.

A Linux environment may contain several network interfaces.

Examples include:

- Ethernet interfaces.
- Wireless interfaces.
- Loopback interfaces.
- Virtual machine network interfaces.

### 10.1 Display Network Interfaces

Run:

```bash
ip addr
```

Or:

```bash
ip a
```

### 10.2 Display IP Routing Information

```bash
ip route
```

This shows the system's routing table.

### 10.3 Display Network Interfaces

```bash
ip link
```

This displays network interface information and link states.

### 10.4 Display Listening Network Ports

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

Remember that this command shows listening sockets, not every active network connection.

### 10.5 Test Network Connectivity

Test communication with your local loopback interface:

```bash
ping -c 4 127.0.0.1
```

The loopback address refers to the local machine.

You can also inspect your VM's default route:

```bash
ip route
```

Your Kali VM may use a NAT or host-only network depending on your VMware configuration.

## 11. Understanding System Configuration

Linux stores configuration information in several locations.

The `/etc` directory is especially important.

Examples:

| File | Purpose |
|---|---|
| `/etc/hostname` | System hostname |
| `/etc/hosts` | Local hostname-to-address mappings |
| `/etc/passwd` | User account information |
| `/etc/group` | Group account information |
| `/etc/fstab` | Filesystem mount configuration |
| `/etc/os-release` | Operating system identification |

### 11.1 Check the Hostname

```bash
hostname
```

### 11.2 Read the Hostname Configuration

```bash
cat /etc/hostname
```

### 11.3 Identify the Linux Distribution

```bash
cat /etc/os-release
```

### 11.4 Display Current Date and Time

```bash
date
```

### 11.5 Display System Information

```bash
hostnamectl
```

This command displays system identification and related information when systemd is available.

## 12. Understanding Logs

Logs are records of system and application events.

They are important for troubleshooting, monitoring, and security investigations.

Common log locations include:

```text
/var/log
```

Examples of log files may include:

- Authentication-related logs.
- System logs.
- Package management logs.
- Application logs.
- Web server logs.

The exact files depend on the distribution and logging configuration.

### 12.1 List Log Files

```bash
ls -lah /var/log
```

### 12.2 Read a Log File

For example, if a file exists:

```bash
less /var/log/dpkg.log
```

Press `q` to exit `less`.

### 12.3 View Recent System Journal Entries

```bash
journalctl -n 20
```

This displays recent journal entries when the system uses systemd's journal.

### 12.4 View Authentication-Related Logs

Depending on the distribution, authentication events may be stored in files such as:

```text
/var/log/auth.log
```

Check whether the file exists:

```bash
ls -l /var/log/auth.log
```

You may need elevated privileges to read protected logs.

For security professionals, logs help investigate failed login attempts, suspicious activity, and service failures.

## 13. Linux Environment and Cybersecurity

Understanding the Linux environment is essential in cybersecurity because security professionals need to inspect how systems operate.

### 13.1 System Enumeration

System enumeration means collecting information about a system.

Examples include:

- Operating system version.
- Kernel version.
- User accounts.
- Running processes.
- Installed services.
- Network interfaces.
- Open listening ports.

### 13.2 System Monitoring

Security professionals monitor system behaviour to identify unusual activity.

Examples:

- Unexpected processes.
- Unusual resource consumption.
- Unknown listening services.
- Suspicious login events.
- Unexpected configuration changes.

### 13.3 Log Analysis

Logs help security professionals investigate events and identify possible incidents.

### 13.4 Network Inspection

Network commands help identify interfaces, addresses, routes, and listening services.

### 13.5 System Hardening

System hardening involves reducing unnecessary functionality and improving security configurations.

Examples include:

- Removing unnecessary services.
- Applying security updates.
- Restricting user privileges.
- Configuring firewalls.
- Monitoring logs.
- Applying appropriate file permissions.

## 14. Practical Lab: Exploring Your Kali Linux Environment

**Lab objective:** Explore the important components of your Kali Linux environment and learn how to collect basic system information.

Perform the following exercises inside your Kali Linux VM.

### Part A – Identify the System

Run:

```bash
uname -a
```

Then:

```bash
cat /etc/os-release
```

Then:

```bash
hostname
```

Record your operating system, kernel version, and hostname.

### Part B – Explore the Filesystem

Run:

```bash
pwd
```

Then:

```bash
ls /
```

Inspect these directories:

```bash
ls -la /home
```

```bash
ls -la /etc
```

```bash
ls -la /var
```

```bash
ls -la /tmp
```

Identify the purpose of each directory.

### Part C – Inspect Your Environment Variables

Run:

```bash
echo "$USER"
```

```bash
echo "$HOME"
```

```bash
echo "$SHELL"
```

```bash
echo "$PATH"
```

```bash
echo "$PWD"
```

Then:

```bash
printenv
```

Observe the information displayed.

### Part D – Inspect Running Processes

Run:

```bash
ps aux | head
```

Then:

```bash
top
```

Press `q` to exit `top`.

If installed, try:

```bash
htop
```

### Part E – Inspect Services

Run:

```bash
ps -p 1 -o comm=
```

Then:

```bash
systemctl --failed
```

Finally:

```bash
systemctl list-units --type=service --state=running
```

Observe the services running on your VM.

### Part F – Inspect System Resources

Run:

```bash
lscpu
```

Then:

```bash
free -h
```

Then:

```bash
df -h
```

Then:

```bash
lsblk
```

Finally:

```bash
uptime
```

Record the CPU, memory, storage, and uptime information.

### Part G – Inspect Networking

Run:

```bash
ip addr
```

Then:

```bash
ip route
```

Then:

```bash
ip link
```

Finally:

```bash
ss -tuln
```

Identify your network interfaces and any listening sockets.

### Part H – Inspect System Logs

Run:

```bash
ls -lah /var/log
```

Then:

```bash
journalctl -n 20
```

If access is restricted, try:

```bash
sudo journalctl -n 20
```

Observe the recent system events.

### Part I – Inspect System Configuration

Run:

```bash
cat /etc/hostname
```

Then:

```bash
cat /etc/os-release
```

Finally:

```bash
hostnamectl
```

Compare the information returned by these commands.

## 15. Important Terms

| Term | Meaning |
|---|---|
| Linux Environment | Complete working environment provided by Linux |
| Kernel | Core of the operating system |
| Shell | Command interpreter |
| Terminal | Interface for entering commands |
| Desktop Environment | Graphical user interface |
| Filesystem | Organised structure for storing data |
| Root Directory | Top-level directory represented by `/` |
| Environment Variable | Named value used by processes and applications |
| Process | Running instance of a program |
| PID | Process ID |
| Service | Background program providing a system or application function |
| CPU | Central Processing Unit |
| RAM | Random Access Memory |
| Storage | Persistent data storage |
| Network Interface | Component used for network communication |
| Logs | Records of system and application events |
| Enumeration | Collecting information about a system |
| System Hardening | Improving security by reducing weaknesses |

## 16. Knowledge Check

Answer these questions without looking at the lesson.

1. What is a Linux environment?
2. Identify six components of a Linux environment.
3. What is the purpose of the Linux kernel?
4. Explain the difference between a terminal and a shell.
5. What is a desktop environment?
6. What is the root directory in Linux?
7. State the purpose of `/home`, `/etc`, `/var`, and `/tmp`.
8. What is an environment variable?
9. What is the purpose of `$PATH`?
10. What is a process?
11. What does PID mean?
12. Explain the difference between a process and a service.
13. What is the purpose of `systemctl`?
14. Which command displays memory usage?
15. Which command displays storage usage?
16. Which command displays network interfaces?
17. What is the purpose of `/var/log`?
18. Explain system enumeration.
19. Why are logs important in cybersecurity?
20. What is system hardening?

## 17. Practical Assignment

Create a Markdown file named:

`Linux-Environment-Assignment.md`

Answer the following:

1. Define a Linux environment.
2. Explain the functions of the kernel, shell, terminal, and desktop environment.
3. Describe ten important Linux directories.
4. Explain environment variables and provide five examples.
5. Define a process and explain the purpose of a PID.
6. Explain the difference between processes and services.
7. Describe four commands used to inspect system resources.
8. Explain how to inspect Linux network interfaces.
9. Describe the importance of Linux logs in cybersecurity.
10. Explain system enumeration and system hardening.

Include actual command outputs from your Kali VM for at least five practical inspection commands. Remove any sensitive information before sharing your assignment.

## 18. Exam-Ready Summary

A Linux environment consists of the kernel, shell, terminal, desktop environment, filesystem, processes, services, and system configurations. The kernel manages hardware and system resources, while the shell interprets commands entered through a terminal. Linux organises files and directories under the root directory `/`. Important directories include `/home`, `/etc`, `/var`, `/tmp`, `/proc`, and `/dev`. Environment variables provide information to applications and processes. Processes are running programs identified by Process IDs, while services provide background functionality. Commands such as `ps`, `top`, `systemctl`, `free`, `df`, `ip`, and `journalctl` help users inspect the system. Understanding these components is important for system administration, troubleshooting, enumeration, monitoring, and cybersecurity.

## 19. Lesson Conclusion

You have now explored the major components of a Linux environment.

You understand how the filesystem is organised, how environment variables work, how processes and services operate, and how to inspect system resources and networking.

These skills form the foundation for more advanced Linux administration and ethical hacking.

As an ethical hacker, you must first understand the environment you are assessing before attempting to identify weaknesses.

**Next Lesson:** Lesson 15 – Module 01 Assessment.