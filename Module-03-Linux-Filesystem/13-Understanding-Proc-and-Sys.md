# LESSON 13: UNDERSTANDING /PROC AND /SYS

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Linux provides special directories that expose information about the running operating system and its hardware.

Two important directories are:

```text
/proc
/sys
```

Although they look like ordinary directories, they contain special virtual filesystems.

They provide information about:

- Running processes
- CPU information
- Memory usage
- Kernel information
- Hardware devices
- System configuration
- Device drivers
- Kernel interfaces

These directories are important for Linux administration, system monitoring, troubleshooting, and cybersecurity investigations.

## 2. What Is `/proc`?

The `/proc` directory contains a virtual filesystem called procfs.

It provides information about:

- Running processes
- CPU information
- Memory information
- Kernel settings
- System uptime
- Hardware-related information

Unlike an ordinary filesystem, most information in `/proc` is generated dynamically by the kernel.

This means many entries do not represent ordinary files stored permanently on your disk.

Inspect it:

```bash
ls /proc
```

You will see several numbered directories and information files.

## 3. Understanding Process Directories

Inside `/proc`, you will find directories named with numbers.

Example:

```text
/proc/1
/proc/2
/proc/100
/proc/250
```

These numbers represent process IDs, also called PIDs.

Every running process normally has a process ID.

For example:

```bash
ps aux
```

This displays running processes.

To inspect a particular process:

```bash
ls /proc/1
```

Process 1 is normally the first userspace process started by the system.

On a typical Kali system using systemd, it is usually systemd.

You can inspect its command line:

```bash
cat /proc/1/cmdline
```

Note: Arguments are separated by null characters, so the output may appear joined together.

A more readable alternative is:

```bash
tr '\0' ' ' < /proc/1/cmdline
```

## 4. Important Files Inside `/proc`

Linux exposes several useful information files inside `/proc`.

### `/proc/cpuinfo`

Displays CPU information.

Command:

```bash
cat /proc/cpuinfo
```

Information may include:

- Processor model
- CPU architecture details
- Number of logical processors
- CPU features

### `/proc/meminfo`

Displays memory information.

Command:

```bash
cat /proc/meminfo
```

It includes information about:

- Total memory
- Available memory
- Free memory
- Cached memory
- Swap information

### `/proc/uptime`

Displays how long the system has been running.

Command:

```bash
cat /proc/uptime
```

The first number represents system uptime in seconds.

### `/proc/version`

Displays kernel version and build information.

Command:

```bash
cat /proc/version
```

### `/proc/mounts`

Displays information about mounted filesystems.

Command:

```bash
cat /proc/mounts
```

### `/proc/partitions`

Displays information about recognized block devices and partitions.

Command:

```bash
cat /proc/partitions
```

## 5. What Is `/sys`?

The `/sys` directory contains a virtual filesystem called sysfs.

It provides information about devices, drivers, and parts of the kernel's device model.

It helps Linux expose and organize information about:

- Hardware devices
- Device drivers
- Buses
- Storage devices
- Network interfaces
- Kernel subsystems

Inspect it:

```bash
ls /sys
```

You may see directories such as:

```text
block
bus
class
devices
dev
firmware
kernel
module
```

These directories represent different parts of the Linux device and kernel model.

## 6. Important Directories Inside `/sys`

### `/sys/block`

Contains information about block devices.

Command:

```bash
ls /sys/block
```

You may see entries representing storage devices.

### `/sys/class`

Organizes devices by class.

Examples include:

```text
/sys/class/net
/sys/class/block
/sys/class/tty
```

### `/sys/class/net`

Contains information about network interfaces.

Command:

```bash
ls /sys/class/net
```

You may see interfaces such as:

```text
eth0
lo
```

Your actual interface names may differ.

### `/sys/devices`

Contains the device hierarchy represented by the kernel.

Command:

```bash
ls /sys/devices
```

### `/sys/module`

Contains information about kernel modules.

Command:

```bash
ls /sys/module
```

Kernel modules provide functionality that can be loaded into or removed from the running kernel.

## 7. Understanding the Difference Between `/proc` and `/sys`

Both directories expose kernel-related information, but they serve different purposes.

| Feature | `/proc` | `/sys` |
|---|---|---|
| Virtual filesystem | procfs | sysfs |
| Main purpose | Process and kernel information | Devices, drivers, and kernel objects |
| Process information | Extensive | Not its main purpose |
| CPU information | `/proc/cpuinfo` | Device and subsystem information |
| Memory information | `/proc/meminfo` | Not its main purpose |
| Network interfaces | Some network information | `/sys/class/net` |
| Storage information | `/proc/partitions` | `/sys/block` |

Remember:

- `/proc` provides process and system information.
- `/sys` provides device and kernel object information.

## 8. Understanding `/proc/self`

The special path:

```text
/proc/self
```

refers to the process accessing it.

For example:

```bash
ls -l /proc/self
```

The actual process ID represented by this path depends on which process accesses it.

You can inspect its status:

```bash
cat /proc/self/status
```

This can show information about the current process, including its process ID and memory-related statistics.

## 9. Security Importance of `/proc` and `/sys`

These virtual filesystems are useful during cybersecurity investigations.

### Process Investigation

Security analysts can inspect process information to understand what is running.

Example:

```bash
ps aux
```

Process-specific information can also be inspected under:

```text
/proc/PID/
```

### Memory Investigation

The following file provides useful system memory information:

```text
/proc/meminfo
```

### Kernel Investigation

Inspect kernel information using:

```bash
uname -a
cat /proc/version
```

### Network Interface Investigation

Inspect network interfaces using:

```bash
ip addr
ls /sys/class/net
```

### Kernel Module Investigation

Inspect loaded kernel modules using:

```bash
lsmod
```

Kernel modules can also be inspected through:

```text
/sys/module
```

Unexpected processes, suspicious modules, and unusual network interfaces may be relevant during an investigation.

However, their presence alone does not prove that a system is compromised.

## 10. Important Security Warning

Some entries in `/proc` and `/sys` are read-only, while others expose interfaces that allow privileged users to change kernel or device settings.

Do not modify unknown entries.

For this lesson, use inspection commands only.

Avoid writing values into files under `/proc/sys` or `/sys`.

## 11. Practical Laboratory

Perform these exercises inside Kali Linux.

### Exercise 1: Explore `/proc`

```bash
ls /proc
```

Identify some numbered directories and information files.

### Exercise 2: Inspect CPU Information

```bash
cat /proc/cpuinfo
```

Identify the processor model and logical processor information.

### Exercise 3: Inspect Memory Information

```bash
cat /proc/meminfo
```

Find:

- MemTotal
- MemFree
- MemAvailable
- SwapTotal
- SwapFree

### Exercise 4: Inspect System Uptime

```bash
cat /proc/uptime
```

Record the first number.

### Exercise 5: Inspect Kernel Version

```bash
cat /proc/version
uname -a
```

Compare the information displayed.

### Exercise 6: Inspect Process 1

```bash
ls /proc/1
cat /proc/1/status
```

Identify:

- Process name
- Process ID
- Parent process ID

### Exercise 7: Explore `/sys`

```bash
ls /sys
```

Identify the major directories.

### Exercise 8: Inspect Storage Devices Through `/sys`

```bash
ls /sys/block
```

Compare the results with:

```bash
lsblk
```

### Exercise 9: Inspect Network Interfaces Through `/sys`

```bash
ls /sys/class/net
```

Compare the results with:

```bash
ip addr
```

### Exercise 10: Inspect Kernel Modules

```bash
lsmod
ls /sys/module
```

Observe the loaded kernel modules.

## 12. Knowledge Check

1. What is `/proc`?
2. What is `/sys`?
3. What does procfs mean?
4. What does sysfs mean?
5. What information is stored in `/proc/cpuinfo`?
6. What information is stored in `/proc/meminfo`?
7. What does `/proc/uptime` display?
8. What does `/proc/self` represent?
9. What is the purpose of `/sys/block`?
10. Where can you find information about network interfaces in `/sys`?
11. What is a kernel module?
12. Explain the difference between `/proc` and `/sys`.

## 13. Lesson Summary

Linux provides virtual filesystems that expose information about the running system.

We learned:

- `/proc` and procfs.
- `/sys` and sysfs.
- Process information.
- CPU and memory information.
- Kernel information.
- Hardware and device information.
- Network interfaces.
- Kernel modules.

Important commands:

```bash
ls /proc
cat /proc/cpuinfo
cat /proc/meminfo
cat /proc/uptime
cat /proc/version
ls /sys
ls /sys/block
ls /sys/class/net
lsmod
uname -a
```

These tools help system administrators and cybersecurity professionals understand and investigate Linux systems.

**Next Lesson:** Filesystem Inspection and Troubleshooting.