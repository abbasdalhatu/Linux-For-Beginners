---
---


# MODULE 17 – ADVANCED LINUX
## LESSON 08: LINUX KERNEL AND MODULES

---

## 1. Introduction to the Linux Kernel

The Linux kernel is the core component of the Linux operating system.

It acts as an intermediary between computer hardware and software applications.

The kernel manages:

- CPU scheduling.
- Memory management.
- Process management.
- Device communication.
- Filesystem operations.
- Network communication.
- Hardware drivers.
- System security mechanisms.

### Understanding the architecture

```text
+----------------------------+
|      User Applications     |
+----------------------------+
|      Shell and Utilities   |
+----------------------------+
|       System Libraries     |
+----------------------------+
|       Linux Kernel         |
|                            |
| Process Management         |
| Memory Management          |
| Device Drivers             |
| Networking                 |
| Filesystem Management      |
+----------------------------+
|          Hardware          |
+----------------------------+
```

### Learning objectives

By the end of this lesson, you should understand:

1. Linux kernel architecture.
2. Kernel versions.
3. Kernel configuration.
4. Kernel modules.
5. Module loading and unloading.
6. Device drivers.
7. Kernel logs.
8. Kernel parameters.
9. Kernel security.
10. Basic kernel troubleshooting.

---

## 2. Understanding Kernel Space and User Space

Linux separates execution into two major environments.

### A. Kernel Space

Kernel space is where the kernel executes with privileged access to system resources.

It manages hardware and essential operating-system operations.

### B. User Space

User space is where ordinary applications and user processes execute with restricted privileges.

Examples include:

- Web browsers.
- Text editors.
- Terminal applications.
- Programming tools.
- User scripts.

### Comparison

| Kernel Space | User Space |
|---|---|
| Privileged execution | Restricted execution |
| Manages hardware | Runs applications |
| Controls memory management | Requests memory |
| Handles system calls | Uses system calls |
| Kernel components execute here | Ordinary applications execute here |

### Why separation matters

If an ordinary application crashes, it normally does not crash the entire operating system.

However, a serious kernel failure can affect the whole system.

This separation is an important operating-system security principle.

---

## 3. Understanding System Calls

A system call is a controlled mechanism through which an application requests a service from the kernel.

Applications cannot directly perform every privileged operation.

Instead, they request kernel services.

### Examples of system calls

| System call | Purpose |
|---|---|
| `open()` | Open a file |
| `read()` | Read data |
| `write()` | Write data |
| `close()` | Close a file descriptor |
| `fork()` | Create a process |
| `execve()` | Execute a program |
| `mmap()` | Map memory |
| `socket()` | Create a communication endpoint |

### Example

When a program reads a file:

1. The application requests the file.
2. It makes a system call.
3. The kernel checks permissions.
4. The kernel accesses the filesystem.
5. Data is returned to the application.

### Inspect system calls

Linux provides `strace` for tracing system calls.

Check whether it is installed:

```bash
command -v strace
```

If unavailable:

```bash
sudo apt update
sudo apt install strace
```

Run a harmless command under tracing:

```bash
strace ls
```

Display a limited number of system calls:

```bash
strace -e trace=openat,read,write ls
```

System-call tracing is useful for debugging application behaviour.

---

## 4. Understanding Linux Kernel Versions

Linux kernels receive updates that may include:

- Security fixes.
- Hardware support.
- Performance improvements.
- Bug fixes.
- New features.
- Driver updates.

### Display the running kernel version

```bash
uname -r
```

Display detailed kernel information:

```bash
uname -a
```

Display the kernel name:

```bash
uname -s
```

Display the machine architecture:

```bash
uname -m
```

### Understanding a kernel version

An example version:

```text
6.12.10-amd64
```

The exact format depends on the distribution and kernel build.

It can identify the kernel release and architecture-specific build information.

### Why kernel updates matter

Outdated kernels may contain known vulnerabilities.

However, kernel upgrades should be performed carefully because they can affect:

- Hardware compatibility.
- Virtual machine drivers.
- Boot behaviour.
- Third-party kernel modules.

---

## 5. Understanding Linux Kernel Modules

A kernel module is code that can be loaded into the running kernel to provide additional functionality.

Modules are commonly used for:

- Hardware drivers.
- Filesystem support.
- Network functionality.
- Security mechanisms.
- Virtualization support.

Many Linux drivers are implemented as kernel modules.

### Advantages of kernel modules

- Additional functionality without rebuilding the entire kernel.
- Easier driver management.
- Modular system architecture.
- Support for different hardware configurations.

### Display loaded modules

```bash
lsmod
```

Display module information:

```bash
modinfo MODULE_NAME
```

Example:

```bash
modinfo loop
```

This displays information about the loop device module if available.

### Search for a module

```bash
lsmod | grep MODULE_NAME
```

Replace `MODULE_NAME` with the module you want to inspect.

---

## 6. Loading and Unloading Kernel Modules

Linux provides commands for managing kernel modules.

### A. Load a module

```bash
sudo modprobe MODULE_NAME
```

`modprobe` attempts to load a module and resolve its dependencies.

### B. Remove a module

```bash
sudo modprobe -r MODULE_NAME
```

This attempts to remove a module if it is not in use and removal is permitted.

### C. Display module information

```bash
modinfo MODULE_NAME
```

### D. Display currently loaded modules

```bash
lsmod
```

### Important difference

| Command | Purpose |
|---|---|
| `lsmod` | Lists loaded modules |
| `modprobe` | Loads or removes modules |
| `modinfo` | Displays module information |
| `insmod` | Inserts a module file directly |
| `rmmod` | Attempts to remove a loaded module |

### Security warning

Do not experiment by removing essential modules.

Removing a storage, network, filesystem, or device-driver module can interrupt your system.

This lesson focuses on inspection and safe module identification.

---

## 7. Understanding Device Drivers

A device driver allows the operating system to communicate with hardware or virtual devices.

Examples include:

- Network adapters.
- Graphics devices.
- USB controllers.
- Storage controllers.
- Audio devices.
- Virtual machine devices.

### Inspect PCI devices

```bash
lspci
```

Display additional details:

```bash
lspci -nnk
```

The `-k` option displays the kernel driver associated with supported devices.

### Inspect USB devices

```bash
lsusb
```

### Inspect hardware information

```bash
sudo lshw -short
```

Install `lshw` if required:

```bash
sudo apt install lshw
```

### Why device-driver inspection matters

It helps administrators investigate:

- Missing drivers.
- Hardware detection problems.
- Network adapter failures.
- Storage controller issues.
- Virtual machine compatibility.

---

## 8. Understanding Kernel Logs

The kernel records important system events.

These events can include:

- Hardware detection.
- Driver initialization.
- Device errors.
- Memory problems.
- Network interface events.
- Kernel warnings.

### Display kernel messages

```bash
dmesg
```

Display recent messages:

```bash
dmesg | tail -n 30
```

Search for errors:

```bash
sudo dmesg | grep -i error
```

Search for hardware-related events:

```bash
sudo dmesg | grep -iE 'usb|pci|nvme|network'
```

### Use journalctl for kernel messages

```bash
journalctl -k
```

Display current boot kernel logs:

```bash
journalctl -k -b
```

Display recent kernel messages:

```bash
journalctl -k -n 30
```

### Why kernel logs matter

Kernel logs are valuable when investigating:

- Driver failures.
- Hardware detection issues.
- Filesystem errors.
- Kernel warnings.
- Unexpected device disconnections.

---

## 9. Understanding Kernel Parameters

Kernel parameters influence how the Linux kernel behaves.

They can be supplied during boot or exposed through runtime interfaces.

### Display kernel command-line parameters

```bash
cat /proc/cmdline
```

### Inspect kernel information

```bash
cat /proc/version
```

### Display kernel settings

```bash
sysctl -a
```

This may produce a large amount of output.

Search for a particular setting:

```bash
sysctl vm.swappiness
```

Display a network-related setting:

```bash
sysctl net.ipv4.ip_forward
```

### Understanding sysctl

`sysctl` allows administrators to inspect and configure kernel parameters.

Examples of parameter categories:

| Category | Purpose |
|---|---|
| `vm` | Virtual memory |
| `net` | Networking |
| `kernel` | General kernel behaviour |
| `fs` | Filesystem behaviour |

### Important warning

Do not change kernel parameters randomly.

Incorrect settings can affect networking, memory management, security, and system stability.

---

## 10. Understanding the /proc and /sys Filesystems

Linux exposes important kernel and hardware information through virtual filesystems.

### A. /proc

`/proc` exposes process and kernel information.

Examples:

```bash
cat /proc/cpuinfo
```

```bash
cat /proc/meminfo
```

```bash
cat /proc/uptime
```

```bash
cat /proc/version
```

### B. /sys

`/sys` exposes device, driver, and kernel-object information.

Inspect its top-level directories:

```bash
ls /sys
```

Inspect available devices:

```bash
ls /sys/devices
```

### Comparison

| `/proc` | `/sys` |
|---|---|
| Process and kernel information | Device and kernel-object information |
| Includes memory statistics | Includes device information |
| Includes CPU information | Includes driver information |
| Includes process directories | Includes hardware hierarchy |

Both are virtual filesystems, not ordinary storage directories.

---

## 11. Kernel Security Fundamentals

The kernel is a critical security boundary.

A vulnerability in the kernel may allow an attacker to compromise the entire operating system.

Important security principles include:

### A. Keep the kernel updated

Security patches help address known vulnerabilities.

### B. Use trusted kernel modules

Only load modules from trusted sources.

### C. Apply least privilege

Ordinary users should not have unnecessary administrative privileges.

### D. Monitor kernel logs

Investigate unusual warnings and hardware events.

### E. Secure module loading

Linux supports mechanisms such as module signing and signature verification.

### F. Protect kernel interfaces

Avoid unnecessary exposure of privileged kernel interfaces.

### G. Use kernel security features

Examples include:

- Linux Security Modules (LSM).
- AppArmor.
- SELinux.
- Seccomp.
- Kernel Address Space Layout Randomization (KASLR).

These mechanisms provide different forms of access control, isolation, and attack-surface reduction.

---

## 12. Practical Lab: Kernel Inspection

This exercise uses read-only commands.

### Step 1: Create a laboratory directory

```bash
mkdir -p ~/module17-lab/lesson08
cd ~/module17-lab/lesson08
```

### Step 2: Identify the running kernel

```bash
uname -r
```

### Step 3: Display complete kernel information

```bash
uname -a
```

### Step 4: Inspect the kernel version file

```bash
cat /proc/version
```

### Step 5: Inspect kernel command-line parameters

```bash
cat /proc/cmdline
```

### Step 6: List loaded kernel modules

```bash
lsmod
```

### Step 7: Inspect a module

```bash
modinfo loop
```

If the module is not available, inspect another module shown by `lsmod`.

### Step 8: Inspect PCI devices

```bash
lspci -nnk
```

### Step 9: Inspect USB devices

```bash
lsusb
```

### Step 10: Inspect recent kernel messages

```bash
sudo dmesg | tail -n 30
```

### Step 11: Inspect memory information

```bash
cat /proc/meminfo
```

### Step 12: Inspect CPU information

```bash
cat /proc/cpuinfo
```

Record your observations.

---

## 13. Practical Project: Build a Kernel Information Report

Create a script that collects information about the running Linux kernel.

### Step 1: Create the script

```bash
nano kernel-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson08/kernel-report.txt"

{
    echo "===================================="
    echo "       LINUX KERNEL REPORT"
    echo "===================================="

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "KERNEL VERSION:"
    uname -r

    echo
    echo "KERNEL INFORMATION:"
    uname -a

    echo
    echo "KERNEL BUILD INFORMATION:"
    cat /proc/version

    echo
    echo "LOADED MODULES:"
    lsmod

    echo
    echo "CPU INFORMATION:"
    lscpu

    echo
    echo "MEMORY INFORMATION:"
    free -h

    echo
    echo "RECENT KERNEL MESSAGES:"
    sudo dmesg | tail -n 20

    echo
    echo "===================================="
    echo "         REPORT COMPLETE"
    echo "===================================="

} > "$REPORT"

echo "Kernel report saved to: $REPORT"
```

### Step 2: Make the script executable

```bash
chmod +x kernel-report.sh
```

### Step 3: Run the script

```bash
./kernel-report.sh
```

If the kernel log command requests elevated permissions, enter your password when prompted.

### Step 4: Read the report

```bash
cat ~/module17-lab/lesson08/kernel-report.txt
```

### Step 5: Verify the file

```bash
ls -lh ~/module17-lab/lesson08/
```

---

## 14. Kernel Troubleshooting Methodology

When investigating kernel-related problems, follow a structured approach.

### Step 1: Identify the problem

Determine what is failing.

Examples:

- Network adapter unavailable.
- USB device not detected.
- Storage device missing.
- Driver failure.

### Step 2: Identify the kernel version

```bash
uname -r
```

### Step 3: Inspect hardware

```bash
lspci -nnk
```

### Step 4: Inspect loaded modules

```bash
lsmod
```

### Step 5: Inspect kernel logs

```bash
sudo dmesg | tail -n 50
```

### Step 6: Inspect system logs

```bash
journalctl -k -b
```

### Step 7: Research the identified error

Use trusted sources such as official distribution documentation and kernel documentation.

### Step 8: Test changes carefully

Back up important information and avoid unnecessary kernel modifications.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is the Linux kernel?
2. Explain the difference between kernel space and user space.
3. What is a system call?
4. Mention five common Linux system calls.
5. What is a kernel module?
6. What is the purpose of `lsmod`?
7. Explain the difference between `modprobe` and `modinfo`.
8. What is a device driver?
9. What does `dmesg` display?
10. What is the purpose of `sysctl`?
11. Explain the difference between `/proc` and `/sys`.
12. What are three kernel security mechanisms?
13. Why should administrators avoid removing essential kernel modules?
14. Why are kernel updates important?

---

## 16. Important Commands Summary

| Command | Purpose |
|---|---|
| `uname -r` | Display kernel release |
| `uname -a` | Display kernel information |
| `lsmod` | List loaded modules |
| `modinfo MODULE` | Display module information |
| `modprobe MODULE` | Load a module |
| `modprobe -r MODULE` | Attempt to remove a module |
| `lspci -nnk` | Inspect PCI devices and drivers |
| `lsusb` | List USB devices |
| `dmesg` | Display kernel messages |
| `journalctl -k` | Display kernel journal logs |
| `sysctl -a` | Display kernel parameters |
| `cat /proc/cmdline` | Display boot parameters |
| `cat /proc/version` | Display kernel build information |
| `cat /proc/meminfo` | Display memory information |
| `strace` | Trace system calls |
| `lscpu` | Display CPU architecture information |

---

## 17. Lesson Summary

In this lesson, you studied the Linux kernel and its modules.

You learned:

- Kernel architecture.
- Kernel space and user space.
- System calls.
- Kernel versions.
- Kernel modules.
- Module management.
- Device drivers.
- Kernel logs.
- Kernel parameters.
- `/proc` and `/sys`.
- Kernel security.
- Kernel troubleshooting.

You also built a kernel information reporting script.

### Completion checklist

- [ ] Understand Linux kernel architecture.
- [ ] Understand kernel space and user space.
- [ ] Understand system calls.
- [ ] Inspect the kernel version.
- [ ] List loaded kernel modules.
- [ ] Inspect module information.
- [ ] Inspect hardware drivers.
- [ ] Read kernel logs.
- [ ] Understand kernel parameters.
- [ ] Explore `/proc` and `/sys`.
- [ ] Build the kernel report.
- [ ] Understand kernel security principles.

**END OF LESSON 08**
