---
---

# Lesson 05: Kernel Modules and Device Drivers

**Course:** Linux for Ethical Hackers  
**Module:** 04 – Linux Kernel and Shell  
**Lesson:** 05 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

The Linux kernel is responsible for managing hardware and providing essential services to applications.

However, Linux supports many different types of hardware devices. It would be inefficient to build every possible device driver directly into one fixed kernel image.

Linux addresses this challenge through **kernel modules**.

Kernel modules allow additional functionality to be added to a running kernel without rebuilding the entire kernel.

Many device drivers are implemented as kernel modules.

Understanding modules and drivers is important for Linux administrators and ethical hackers because they operate close to the operating system's most privileged components.

## 2. What Is a Kernel Module?

A kernel module is a piece of code that can be loaded into or removed from the running Linux kernel.

Modules extend the kernel's functionality.

They can provide support for:

- Hardware devices.
- Filesystems.
- Networking features.
- Other kernel functionality.

For example, a Linux system may use a kernel module to support a particular network adapter.

Instead of rebuilding the entire kernel, the appropriate module can be loaded when needed.

## 3. What Is a Device Driver?

A device driver is software that allows the operating system to communicate with a hardware device.

Examples include:

- Keyboard drivers.
- Mouse drivers.
- Network card drivers.
- USB drivers.
- Graphics drivers.
- Storage drivers.

A driver translates requests from the operating system into operations that a particular device understands.

For example, when you connect a USB device, Linux needs appropriate driver support to communicate with it.

**Important distinction:**

- A kernel module is a mechanism for extending kernel functionality.
- A device driver is software that controls or communicates with a device.
- Many Linux device drivers are distributed as kernel modules, but not every kernel module is a device driver.

## 4. How Kernel Modules Work

A simplified process looks like this:

1. Linux starts and initializes the kernel.
2. The kernel identifies available hardware and required functionality.
3. Appropriate built-in drivers or modules provide support.
4. The kernel communicates with the hardware through the driver.
5. Applications use operating-system interfaces to access the device.

### Simplified Diagram

```text
+--------------------------------------+
|             APPLICATION              |
|                                      |
|       Network Browser / Terminal     |
+--------------------------------------+
                  |
                  v
+--------------------------------------+
|             LINUX KERNEL             |
|                                      |
|       Networking Subsystem           |
|                                      |
|       Network Device Driver          |
+--------------------------------------+
                  |
                  v
+--------------------------------------+
|              HARDWARE                |
|                                      |
|             Network Card             |
+--------------------------------------+
```

This diagram illustrates a simplified relationship. Actual device communication involves additional kernel subsystems and hardware interfaces.

## 5. Built-In Drivers vs Loadable Modules

Linux supports two major ways of providing kernel functionality.

### 5.1 Built-In Kernel Drivers

A built-in driver is compiled directly into the kernel.

It is available as part of the kernel itself.

Advantages:

- Available early during boot.
- Does not require a separate module-loading operation.
- Useful for functionality needed to start the system.

Disadvantages:

- Cannot be independently unloaded like a loadable module.
- Changes generally require building and installing a different kernel.

### 5.2 Loadable Kernel Modules

A loadable kernel module can be added to or removed from a running kernel, subject to system requirements and permissions.

Advantages:

- Flexible.
- Supports different hardware configurations.
- Can reduce the need to include every optional feature in the kernel image.

Disadvantages:

- A faulty module can cause serious system problems.
- A malicious module can compromise kernel integrity.
- Module compatibility must match the running kernel.

### Comparison

| Feature | Built-In Driver | Loadable Module |
|---|---|---|
| Included in kernel image | Yes | No |
| Loaded separately | No | Yes |
| Independently removable | No | Sometimes |
| Flexible deployment | Less flexible | More flexible |
| Example | Essential boot-time driver | Optional hardware driver |

## 6. Understanding Kernel Module Files

Linux kernel modules are commonly stored in:

```text
/lib/modules/
```

Each kernel version has its own module directory.

You can inspect the directory for your running kernel:

```bash
ls /lib/modules/$(uname -r)
```

Kernel module files commonly use extensions such as:

- `.ko`
- `.ko.xz`
- `.ko.zst`

The `.ko` extension means kernel object.

Compressed module files may use compression formats such as XZ or Zstandard.

## 7. Important Kernel Module Commands

Linux provides several commands for inspecting and managing modules.

For this lesson, we will focus on safe inspection.

### 7.1 lsmod

Displays currently loaded kernel modules.

```bash
lsmod
```

Example output:

```text
Module                  Size  Used by
nf_conntrack           20480  1
x_tables               40960  2
```

Your output will be different.

The columns generally represent:

- Module: Name of the module.
- Size: Module size in memory.
- Used by: Reference count and information about dependent users.

### 7.2 modinfo

Displays information about a kernel module.

```bash
modinfo MODULE_NAME
```

Replace `MODULE_NAME` with a module actually installed on your system.

It may display:

- Description.
- Author.
- License.
- Module filename.
- Dependencies.
- Supported hardware identifiers.

### 7.3 modprobe

`modprobe` is used to load or remove kernel modules and manage their dependencies.

Load a module:

```bash
sudo modprobe MODULE_NAME
```

Remove a module:

```bash
sudo modprobe -r MODULE_NAME
```

**Warning:** Do not execute these commands on unfamiliar modules during this lesson. Removing an active device driver or loading an untrusted module can disrupt or compromise your system.

### 7.4 rmmod

Removes a module from the running kernel when possible.

```bash
sudo rmmod MODULE_NAME
```

This command should only be used when you understand the module's purpose, dependencies and impact.

### 7.5 depmod

Generates dependency information for kernel modules.

```bash
sudo depmod -a
```

This is an administrative maintenance operation. You do not need to run it during this introductory lesson.

### 7.6 modprobe --show-depends

Displays dependencies that would be considered when loading a module.

```bash
modprobe --show-depends MODULE_NAME
```

This is useful for understanding module relationships without intentionally loading the module.

## 8. Understanding Module Dependencies

Some kernel modules depend on other modules.

For example, a particular driver may require another kernel component to provide supporting functionality.

A module manager such as `modprobe` can use dependency information to load required components.

The dependency information is maintained in the kernel's module directory.

Useful command:

```bash
ls /lib/modules/$(uname -r)
```

You may also inspect:

```bash
ls /lib/modules/$(uname -r)/kernel
```

## 9. Device Drivers in Linux

Linux supports many hardware categories through device drivers.

### 9.1 Network Drivers

Network drivers support communication with network adapters.

They help the kernel communicate with:

- Ethernet adapters.
- Wireless adapters.
- Virtual network adapters.

Useful commands:

```bash
ip link
```

Displays network interfaces.

```bash
lspci -k
```

Displays PCI devices and associated kernel drivers where available.

### 9.2 USB Drivers

USB drivers support communication with USB devices.

Useful command:

```bash
lsusb
```

Displays connected USB devices.

### 9.3 Storage Drivers

Storage drivers enable communication with disks and other storage devices.

Useful commands:

```bash
lsblk
```

Displays block devices.

```bash
sudo fdisk -l
```

Displays partition information.

The second command may require administrative privileges.

### 9.4 Graphics Drivers

Graphics drivers allow Linux to communicate with graphics hardware.

Useful command:

```bash
lspci -k
```

Look for graphics-related devices and their associated drivers.

## 10. Device Files and the /dev Directory

Linux represents many devices through special files inside:

```text
/dev
```

These are known as device files.

There are two major categories:

### 10.1 Character Devices

Character devices generally handle data as a stream of characters or bytes.

Examples may include:

- Terminal devices.
- Serial devices.

### 10.2 Block Devices

Block devices handle data in blocks and are commonly used for storage.

Examples include:

- Hard drives.
- SSDs.
- Virtual disks.

Inspect device files:

```bash
ls -l /dev
```

Inspect block devices:

```bash
lsblk
```

A device file is an interface to a device or kernel functionality; it is not the physical hardware itself.

## 11. Kernel Modules and Cybersecurity

Kernel modules are security-sensitive because they execute with kernel-level privileges.

A compromised module can affect the entire operating system.

### 11.1 Malicious Kernel Modules

An attacker with sufficient privileges may attempt to install a malicious kernel module.

Such a module could potentially:

- Interfere with kernel operations.
- Hide malicious activity.
- Access sensitive system resources.
- Manipulate device behaviour.

### 11.2 Kernel Rootkits

Some rootkits attempt to operate at the kernel level.

They may attempt to manipulate system behaviour to conceal processes, files or network activity.

Kernel rootkits are particularly dangerous because they operate within a highly privileged environment.

### 11.3 Trusted Module Sources

Administrators should obtain kernel modules from trusted sources.

Installing unknown kernel modules can introduce serious security risks.

### 11.4 Module Signing

Linux supports module-signing mechanisms.

Depending on kernel configuration and platform security settings, signature verification can help restrict which modules are accepted.

Module signing is especially relevant to systems using Secure Boot.

### 11.5 Monitoring Loaded Modules

Administrators can inspect loaded modules to identify unexpected components.

Useful command:

```bash
lsmod
```

For more information about a particular module:

```bash
modinfo MODULE_NAME
```

Unexpected modules should be investigated rather than automatically assumed to be malicious.

## 12. Practical Lab: Investigating Kernel Modules and Drivers

**Environment:** Kali Linux virtual machine

**Objective:** Identify loaded modules, inspect drivers and understand how Linux manages hardware.

### Task 1: List loaded modules

```bash
lsmod
```

Identify five module names.

### Task 2: Count loaded modules

```bash
lsmod | wc -l
```

Record the result.

The output includes a header row, so the number of modules is generally one less than the line count.

### Task 3: Inspect module information

Choose a module from your `lsmod` output.

```bash
modinfo MODULE_NAME
```

Replace `MODULE_NAME` with the actual name.

Record:

- Module name.
- Description.
- License.
- Filename.
- Dependencies, if listed.

### Task 4: Inspect installed module directories

```bash
ls /lib/modules/$(uname -r)
```

### Task 5: Inspect module files

```bash
find /lib/modules/$(uname -r) -type f -name '*.ko*' | head -n 20
```

This displays up to 20 matching module files.

### Task 6: Inspect hardware drivers

```bash
lspci -k
```

Observe the hardware devices and associated kernel drivers.

### Task 7: Inspect USB devices

```bash
lsusb
```

Record any USB devices reported by your VM.

### Task 8: Inspect storage devices

```bash
lsblk
```

Identify your virtual disk and its partitions.

### Task 9: Inspect device files

```bash
ls -l /dev | head -n 30
```

Observe the different device-file entries.

### Task 10: Examine a module's dependencies

Choose a suitable module and run:

```bash
modprobe --show-depends MODULE_NAME
```

This displays dependency information without intentionally loading the module.

## 13. Important Terms

| Term | Meaning |
|---|---|
| Kernel Module | Code that extends kernel functionality |
| Device Driver | Software that enables communication with hardware |
| Loadable Module | Module that can be loaded into a running kernel |
| Built-In Driver | Driver compiled directly into the kernel |
| `lsmod` | Displays loaded modules |
| `modinfo` | Displays module information |
| `modprobe` | Loads or removes modules and handles dependencies |
| `rmmod` | Removes a kernel module |
| `depmod` | Generates module dependency information |
| Device File | Special file representing a device interface |
| Character Device | Device interface that handles character or byte streams |
| Block Device | Device interface that handles data in blocks |
| Kernel Rootkit | Rootkit that operates at the kernel level |

## 14. Knowledge Check

Answer these questions in your own words.

1. What is a kernel module?
2. What is a device driver?
3. Explain the difference between a kernel module and a device driver.
4. What is the difference between a built-in driver and a loadable module?
5. What is the purpose of `lsmod`?
6. What information does `modinfo` provide?
7. What does `modprobe` do?
8. What is the purpose of the `/lib/modules/` directory?
9. Explain the difference between character devices and block devices.
10. Why are malicious kernel modules dangerous?
11. What is a kernel rootkit?
12. Why should administrators use trusted kernel modules?

## 15. Practical Assignment

Create a report titled:

**Linux Kernel Modules and Device Drivers Investigation**

Include:

1. Your running kernel version.
2. Five loaded kernel modules.
3. Information about two modules using `modinfo`.
4. The number of loaded modules.
5. Five hardware devices observed using `lspci -k`, where available.
6. Your storage devices from `lsblk`.
7. An explanation of the difference between a device driver and a kernel module.
8. Three security risks associated with untrusted kernel modules.

Save your report in this Markdown file.

## 16. Exam-Ready Summary

A kernel module is a piece of code that extends the functionality of the Linux kernel.

A device driver is software that enables the operating system to communicate with hardware.

Many Linux device drivers are implemented as loadable kernel modules.

Important module-management commands include:

```bash
lsmod
modinfo
modprobe
rmmod
depmod
```

The `/lib/modules/` directory stores modules associated with installed kernel versions.

Linux also represents many hardware devices through special files in `/dev`.

Kernel modules are security-sensitive because they operate with high privileges. Administrators should inspect modules, use trusted sources and maintain secure systems.

## 17. Conclusion

You have learned what kernel modules and device drivers are, how Linux uses them, and how to inspect installed modules and hardware devices.

You also learned why kernel modules are important from a cybersecurity perspective.

In the next lesson, we will explore Linux kernel versions, updates and management.

**Next Lesson:** Lesson 06 – Kernel Versions and Management.