---
---


# MODULE 17 – ADVANCED LINUX
## LESSON 01: ADVANCED LINUX FUNDAMENTALS

---

## 1. Introduction to Advanced Linux

Linux is an open-source operating system built around the Linux kernel.

At the beginner level, we learn commands such as:

- `ls` – list files.
- `cd` – change directories.
- `mkdir` – create directories.
- `cp` – copy files.
- `mv` – move files.
- `rm` – remove files.

Advanced Linux goes beyond basic commands. It involves understanding how the operating system works internally and how to manage it efficiently, securely, and reliably.

Advanced Linux administration includes:

1. Kernel management.
2. Process and resource management.
3. Advanced shell operations.
4. Storage and filesystem administration.
5. Network administration.
6. System monitoring.
7. Security hardening.
8. Automation.
9. Virtualization and containers.

### Learning objectives

By the end of this lesson, you should be able to:

- Explain the architecture of a Linux operating system.
- Identify the major Linux system components.
- Understand user space and kernel space.
- Explain how system calls work.
- Identify important system directories.
- Inspect Linux system information.
- Understand the Linux boot process.

---

## 2. Linux Operating System Architecture

Linux architecture describes how different components work together to operate a computer.

The major components are:

1. Hardware.
2. Linux kernel.
3. System libraries.
4. Shell.
5. System utilities.
6. Applications.

### Linux architecture diagram

```text
+----------------------------------+
|          APPLICATIONS            |
| Browser, Editors, Security Tools  |
+----------------------------------+
|       SHELL AND UTILITIES         |
| Bash, ls, grep, systemctl         |
+----------------------------------+
|         SYSTEM LIBRARIES          |
|       glibc and other libraries   |
+----------------------------------+
|           LINUX KERNEL            |
| Process, Memory, Device, Network  |
+----------------------------------+
|             HARDWARE              |
| CPU, RAM, Disk, Network Devices   |
+----------------------------------+
```

### Explanation

**1. Hardware**

The physical components of a computer.

Examples:
- CPU.
- RAM.
- Hard disk.
- Keyboard.
- Network adapter.

**2. Linux Kernel**

The kernel is the core of the operating system.

It manages communication between software and hardware.

Its responsibilities include:
- Process scheduling.
- Memory management.
- Device management.
- Filesystem management.
- Network operations.
- Security enforcement.

**3. System Libraries**

Libraries provide reusable functions that applications can use to interact with operating-system services.

Example: GNU C Library (`glibc`).

**4. Shell**

A shell allows users to interact with Linux through commands.

Examples:
- Bash.
- Zsh.
- Fish.

**5. System Utilities**

These are programs that help users and administrators manage Linux.

Examples:
- `ip`
- `ps`
- `grep`
- `find`
- `systemctl`

**6. Applications**

Programs that perform specific tasks.

Examples:
- Web browsers.
- Text editors.
- Development tools.
- Security tools.

---

## 3. Understanding the Linux Kernel

The kernel is the central component of Linux.

It operates between applications and hardware, providing controlled access to system resources.

### Major kernel responsibilities

| Responsibility | Explanation |
|---|---|
| Process management | Controls running processes |
| Memory management | Allocates and manages RAM |
| Device management | Communicates with hardware |
| Filesystem management | Handles file access and storage |
| Networking | Manages network communication |
| Security | Enforces permissions and access controls |

### Inspecting kernel information

Run:

```bash
uname -r
```

Displays the running kernel version.

Run:

```bash
uname -a
```

Displays detailed system and kernel information.

Run:

```bash
cat /proc/version
```

Displays kernel build information.

Run:

```bash
hostnamectl
```

Displays system identity and operating-system information.

### Why this matters

Security professionals and Linux administrators need to understand the running kernel because kernel versions can affect:

- Hardware compatibility.
- System stability.
- Security vulnerabilities.
- Driver compatibility.
- Operating-system features.

---

## 4. User Space and Kernel Space

Linux separates execution into two major environments.

### A. User Space

User space is where ordinary applications and user programs operate.

Examples:
- Bash.
- Python.
- Firefox.
- Text editors.
- Most administration utilities.

Applications normally cannot directly access protected kernel memory or hardware resources.

### B. Kernel Space

Kernel space is the privileged environment where the Linux kernel operates.

It has access to protected memory and hardware-management facilities.

### Comparison

| User Space | Kernel Space |
|---|---|
| Runs ordinary applications | Runs kernel code |
| Restricted privileges | Highly privileged |
| Uses system calls | Provides system services |
| Application failures are usually isolated | Kernel failures can affect the entire system |

### Simple analogy

Think of a university:

- Students represent user-space applications.
- The administration office represents the kernel.
- Students submit requests.
- The administration office processes authorized requests.

Applications request operating-system services instead of directly controlling protected hardware resources.

---

## 5. Understanding System Calls

A system call is a controlled interface through which a user-space program requests a service from the kernel.

Examples include:

| System Call | Purpose |
|---|---|
| `open()` | Open a file |
| `read()` | Read data |
| `write()` | Write data |
| `close()` | Close a file |
| `fork()` | Create a process |
| `execve()` | Execute a program |
| `socket()` | Create a network socket |

### How a system call works

Consider opening a file:

1. A program requests access to a file.
2. The program invokes an appropriate system call.
3. The CPU transfers execution into kernel mode.
4. The kernel checks the request and permissions.
5. The kernel performs the requested operation.
6. The result is returned to the application.

### Important distinction

A shell command is not necessarily a system call.

For example:

- `cat` is a user-space program.
- `open()` and `read()` are system calls that programs can use.

---

## 6. Important Linux System Directories

Linux organizes system resources using a hierarchical filesystem.

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/bin` | Essential user commands |
| `/sbin` | Essential system administration commands |
| `/boot` | Bootloader and kernel-related files |
| `/dev` | Device files |
| `/etc` | System configuration |
| `/home` | User home directories |
| `/lib` | Essential shared libraries |
| `/media` | Removable media mount points |
| `/mnt` | Temporary mount points |
| `/opt` | Optional software |
| `/proc` | Virtual process and kernel information |
| `/root` | Root user's home directory |
| `/run` | Runtime system information |
| `/sys` | Kernel and device information |
| `/tmp` | Temporary files |
| `/usr` | User applications and shared resources |
| `/var` | Variable data, logs and caches |

### Special directories

**`/proc`**

Contains virtual information about running processes and the kernel.

Example:

```bash
cat /proc/cpuinfo
```

**`/sys`**

Provides information about devices, drivers, and kernel objects.

Example:

```bash
ls /sys/class
```

**`/dev`**

Contains device files representing hardware and virtual devices.

Example:

```bash
ls /dev
```

### Important warning

Do not randomly modify files inside `/proc`, `/sys`, `/dev`, or important system configuration directories.

Many entries are managed dynamically by the kernel or system services.

---

## 7. Understanding Linux Processes

A process is an instance of a running program.

Every process has a Process ID (PID).

To display running processes:

```bash
ps aux
```

To display processes interactively:

```bash
top
```

To display the current shell's PID:

```bash
echo $$
```

To display the parent process ID:

```bash
echo $PPID
```

To display the process tree:

```bash
pstree
```

If `pstree` is unavailable, you can install its package using:

```bash
sudo apt install psmisc
```

### Important process terms

- PID: Process ID.
- PPID: Parent Process ID.
- UID: User ID associated with a process.
- GID: Group ID associated with a process.
- CPU usage: Processor resources consumed.
- Memory usage: RAM consumed by a process.

---

## 8. Understanding the Linux Boot Process

The boot process describes how Linux starts after a computer is powered on.

### Main boot stages

**Stage 1: BIOS or UEFI**

The computer firmware initializes hardware and identifies a bootable device.

**Stage 2: Bootloader**

A bootloader such as GRUB loads the Linux kernel and may load an initial RAM filesystem.

**Stage 3: Kernel initialization**

The kernel initializes hardware, memory management, drivers, and other essential facilities.

**Stage 4: initramfs**

The initial RAM filesystem provides temporary tools and drivers needed to locate and mount the real root filesystem.

**Stage 5: systemd**

On many modern Linux distributions, systemd becomes the first userspace process and normally runs as PID 1.

**Stage 6: Services and login**

System services start, and the system provides a login interface.

### Boot process diagram

```text
Power On
   |
   v
BIOS / UEFI
   |
   v
Bootloader (GRUB)
   |
   v
Linux Kernel
   |
   v
initramfs
   |
   v
Root Filesystem
   |
   v
systemd (PID 1)
   |
   v
System Services
   |
   v
Login Screen / Terminal
```

### Inspecting boot information

```bash
systemctl status
```

Displays system manager status.

```bash
systemctl get-default
```

Displays the default systemd target.

```bash
journalctl -b
```

Displays logs from the current boot.

```bash
uptime
```

Displays how long the system has been running.

---

## 9. Practical Lab: Linux System Inspection

We will perform a read-only inspection of your Kali Linux virtual machine.

### Lab objectives

- Identify the Linux kernel version.
- Identify the operating system.
- Inspect CPU and memory information.
- Identify running processes.
- Inspect system uptime.
- Examine the boot process.

### Step 1: Create a laboratory directory

```bash
mkdir -p ~/module17-lab/lesson01
cd ~/module17-lab/lesson01
```

### Step 2: Identify the operating system

```bash
cat /etc/os-release
```

Record:
- Operating-system name.
- Version.
- Distribution identifier.

### Step 3: Inspect the kernel

```bash
uname -a
```

Then:

```bash
uname -r
```

Record the kernel version.

### Step 4: Inspect CPU information

```bash
lscpu
```

Identify:
- CPU architecture.
- Number of CPUs.
- CPU model.
- Number of cores.

### Step 5: Inspect memory

```bash
free -h
```

Record:
- Total memory.
- Used memory.
- Available memory.

### Step 6: Inspect running processes

```bash
ps aux
```

Then:

```bash
top
```

Press `q` to exit `top`.

### Step 7: Inspect system uptime

```bash
uptime
```

### Step 8: Inspect systemd

```bash
systemctl status
```

### Step 9: Inspect boot logs

```bash
journalctl -b -n 30
```

This displays the latest 30 log entries from the current boot.

### Step 10: Save your findings

Create a report:

```bash
nano system-inspection.txt
```

Record your findings using this template:

```text
MODULE 17 - LESSON 01
LINUX SYSTEM INSPECTION REPORT

1. Operating System:
2. OS Version:
3. Kernel Version:
4. CPU Architecture:
5. CPU Model:
6. Number of CPUs:
7. Total RAM:
8. Available RAM:
9. System Uptime:
10. Default Systemd Target:

OBSERVATIONS:
Write what you learned about your Linux system.
```

Save in Nano using:

- `CTRL + O`
- Press `ENTER`
- `CTRL + X`

---

## 10. Knowledge Check

Answer these questions in your notebook.

1. What is the Linux kernel?
2. Identify six major responsibilities of the Linux kernel.
3. Explain the difference between user space and kernel space.
4. What is a system call?
5. Give four examples of Linux system calls.
6. What information is stored inside `/proc`?
7. What is the purpose of `/etc`?
8. Explain the difference between a process and a program.
9. What is PID 1 responsible for on a typical systemd-based Linux distribution?
10. Explain the major stages of the Linux boot process.

---

## 11. Important Commands Summary

| Command | Purpose |
|---|---|
| `uname -a` | Display kernel and system information |
| `uname -r` | Display kernel version |
| `hostnamectl` | Display system identity |
| `cat /etc/os-release` | Identify Linux distribution |
| `lscpu` | Display CPU information |
| `free -h` | Display memory information |
| `ps aux` | List running processes |
| `top` | Monitor running processes |
| `pstree` | Display process hierarchy |
| `uptime` | Display system uptime |
| `systemctl status` | Display system manager status |
| `journalctl -b` | Display current boot logs |

---

## 12. Lesson Summary

In this lesson, we studied the foundations of advanced Linux administration.

You learned:

- Linux operating-system architecture.
- The responsibilities of the kernel.
- User space and kernel space.
- System calls.
- Important Linux directories.
- Process fundamentals.
- Linux boot stages.
- Basic system inspection.

These concepts are essential for understanding advanced Linux administration, cybersecurity, troubleshooting, and system engineering.

### Completion checklist

- [ ] Understand Linux architecture.
- [ ] Understand kernel responsibilities.
- [ ] Understand user space and kernel space.
- [ ] Understand system calls.
- [ ] Identify important Linux directories.
- [ ] Understand the Linux boot process.
- [ ] Complete the practical inspection.
- [ ] Save `system-inspection.txt`.
- [ ] Answer the knowledge-check questions.

**END OF LESSON 01**
