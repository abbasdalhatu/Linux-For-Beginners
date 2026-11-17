---
---

# Lesson 02: Linux Kernel Architecture

**Course:** Linux for Ethical Hackers  
**Module:** 04 – Linux Kernel and Shell  
**Lesson:** 02 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

In the previous lesson, we learned that the Linux kernel is the central component responsible for managing hardware and providing services to applications.

Now, we will examine how the kernel is organized internally.

This organization is known as **kernel architecture**.

Understanding kernel architecture helps us understand how Linux handles processes, memory, devices, filesystems, networking and security.

For ethical hackers, this knowledge is useful when investigating system vulnerabilities, understanding privilege boundaries and examining how the operating system operates.

## 2. What Is Kernel Architecture?

Kernel architecture describes how the internal components of a kernel are organized and how they communicate with one another.

Think of a university:

- The administration manages the institution.
- The finance department handles money.
- The academic department manages education.
- The ICT department manages technology.

Each department has a responsibility, but they work together.

Similarly, the Linux kernel contains different subsystems responsible for different operating-system functions.

## 3. Major Types of Kernel Architecture

There are several approaches to designing an operating-system kernel.

### 3.1 Monolithic Kernel

A monolithic kernel places many core operating-system services inside kernel space.

These services commonly include:

- Process management.
- Memory management.
- Filesystem management.
- Device drivers.
- Networking.

Because these components operate within kernel space, they can communicate efficiently.

**Advantages:**
- Efficient communication between kernel components.
- Generally good performance.
- Direct access to core kernel services.

**Disadvantages:**
- A serious bug in a privileged component can affect the entire system.
- The kernel can become complex.
- Debugging certain kernel problems can be difficult.

Examples commonly associated with monolithic kernel designs include Linux and traditional UNIX kernels.

### 3.2 Microkernel

A microkernel keeps the core kernel as small as possible.

It typically handles essential functions such as:

- Basic scheduling.
- Basic memory management.
- Inter-process communication.

Other services, such as some device drivers and filesystem services, can run outside the kernel in user space.

**Advantages:**
- Smaller privileged core.
- Better isolation between some services.
- Certain failures may be contained more easily.

**Disadvantages:**
- Communication between services can introduce overhead.
- System design can be more complicated.

Examples include MINIX 3 and QNX.

### 3.3 Hybrid Kernel

A hybrid kernel combines design ideas from monolithic and microkernel architectures.

It aims to balance performance, modularity and separation of responsibilities.

Examples include the Windows NT kernel architecture and Apple's XNU kernel.

### 3.4 Modular Kernel

A modular kernel supports adding or removing certain functionality through modules.

Linux uses a monolithic kernel design with modular capabilities.

For example, Linux can load a device driver as a kernel module without requiring the entire kernel to be rebuilt.

Important distinction: modularity is a design capability, not a completely separate category that describes every aspect of kernel architecture.

## 4. Comparing Kernel Architectures

| Feature | Monolithic | Microkernel | Hybrid |
|---|---|---|---|
| Core services | Mostly kernel space | Minimal kernel space | Combination |
| Performance | Often efficient | May involve IPC overhead | Designed to balance both |
| Isolation | Depends on implementation | Often stronger service separation | Depends on implementation |
| Complexity | Can become large | More distributed | Can be complex |
| Example | Linux | MINIX 3 | Windows NT |

## 5. Understanding Linux Kernel Components

The Linux kernel contains several important subsystems.

### 5.1 Process Scheduler

The scheduler determines which process or thread receives CPU time.

A computer can run many programs even though each CPU core can execute only a limited number of instructions at a time.

The scheduler coordinates execution.

Its responsibilities include:

- Selecting tasks for execution.
- Managing scheduling priorities.
- Sharing CPU time.
- Supporting multitasking.

Example:

When Firefox, a terminal and a music player are running simultaneously, the scheduler coordinates their access to CPU resources.

Useful command:

```bash
ps -eo pid,comm,ni,stat
```

Displays process IDs, command names, nice values and process states.

### 5.2 Memory Management Subsystem

This subsystem manages physical and virtual memory.

Its responsibilities include:

- Allocating memory.
- Releasing memory.
- Managing virtual memory.
- Handling memory protection.
- Managing paging and memory mappings.

Useful commands:

```bash
free -h
```

```bash
cat /proc/meminfo
```

### 5.3 Virtual Filesystem Switch (VFS)

The Virtual Filesystem Switch provides a common interface for interacting with different filesystem types.

For example, Linux can support filesystems such as:

- ext4
- XFS
- Btrfs
- FAT
- NTFS

Applications can use familiar file operations without needing to understand every filesystem's internal implementation.

This abstraction makes filesystem support more flexible.

### 5.4 Device Drivers

Device drivers allow the kernel to communicate with hardware.

Examples include drivers for:

- Network cards.
- USB controllers.
- Storage devices.
- Graphics hardware.
- Audio devices.

Many Linux drivers can be provided as loadable kernel modules.

### 5.5 Networking Subsystem

The networking subsystem implements network functionality.

It supports:

- TCP/IP communication.
- Network interfaces.
- Routing.
- Network packet processing.
- Socket communication.
- Packet filtering interfaces.

This subsystem is especially important in cybersecurity because network traffic passes through kernel-managed networking facilities.

Useful commands:

```bash
ip addr
```

```bash
ip route
```

```bash
ss -tulnp
```

### 5.6 Inter-Process Communication (IPC)

Inter-Process Communication allows processes to exchange information and coordinate activities.

Linux supports several IPC mechanisms, including:

- Pipes.
- Signals.
- Shared memory.
- Message queues.
- Unix domain sockets.

For example, one program can send data to another through a pipe.

```bash
ls | grep ".txt"
```

Here, the output of `ls` is passed to `grep`.

### 5.7 Security Subsystem

The kernel provides mechanisms for enforcing system security.

Examples include:

- User and group permission checks.
- Process isolation.
- Capabilities.
- Linux Security Modules (LSM).
- Mandatory Access Control integrations.

Security frameworks such as SELinux and AppArmor use kernel security mechanisms to enforce additional restrictions.

## 6. A Simplified Linux Kernel Architecture Diagram

```text
+--------------------------------------------------+
|                  USER SPACE                      |
|                                                  |
|  Applications | Shell | Services | Utilities     |
+--------------------------------------------------+
                       |
                       v
+--------------------------------------------------+
|              SYSTEM CALL INTERFACE               |
+--------------------------------------------------+
                       |
                       v
+--------------------------------------------------+
|                  KERNEL SPACE                    |
|                                                  |
|  +-------------------+  +---------------------+  |
|  | Process Scheduler|  | Memory Management   |  |
|  +-------------------+  +---------------------+  |
|                                                  |
|  +-------------------+  +---------------------+  |
|  | Virtual Filesystem|  | Networking          |  |
|  +-------------------+  +---------------------+  |
|                                                  |
|  +-------------------+  +---------------------+  |
|  | Device Drivers    |  | Security Mechanisms |  |
|  +-------------------+  +---------------------+  |
|                                                  |
|       Other Kernel Subsystems and Modules        |
+--------------------------------------------------+
                       |
                       v
+--------------------------------------------------+
|                    HARDWARE                      |
|                                                  |
|       CPU | RAM | Storage | Network Devices      |
+--------------------------------------------------+
```

This diagram is a simplified conceptual view. Real Linux kernel components interact in more complex ways.

## 7. How Kernel Components Work Together

Consider what happens when you download a file using a web browser.

1. The browser requests network communication.
2. The application uses networking interfaces provided by the operating system.
3. The kernel networking subsystem processes network traffic.
4. The network driver communicates with the network hardware.
5. Received data is made available to the application.
6. The browser requests that the file be saved.
7. The filesystem subsystem handles the file operation.
8. The storage driver communicates with the storage device.

Several kernel subsystems cooperate to complete one ordinary activity.

## 8. Linux Kernel Modules

Linux supports loadable kernel modules.

A module extends kernel functionality without requiring the entire kernel to be rebuilt.

Common examples include:

- Hardware drivers.
- Filesystem support.
- Networking functionality.

Useful commands:

**List loaded modules:**

```bash
lsmod
```

**Inspect module information:**

```bash
modinfo MODULE_NAME
```

**Display module dependencies:**

```bash
modprobe --show-depends MODULE_NAME
```

For this lesson, use these commands only to inspect your system. Do not load or remove unfamiliar modules.

## 9. Kernel Architecture and Cybersecurity

Understanding kernel architecture is important because the kernel operates with high privileges.

### 9.1 Attack Surface

The attack surface refers to the different points through which an attacker might attempt to interact with or compromise a system.

Kernel-related attack surfaces may include:

- System calls.
- Device drivers.
- Kernel modules.
- Networking interfaces.
- Filesystem implementations.

Reducing unnecessary components and keeping software updated can help reduce risk.

### 9.2 Kernel Privileges

Kernel code operates with greater privileges than ordinary applications.

A vulnerability affecting kernel-space code can therefore have serious consequences.

Depending on the weakness, an attacker might attempt to:

- Gain elevated privileges.
- Crash the system.
- Access protected resources.
- Interfere with system operations.

### 9.3 Driver Security

Device drivers are important because they interact closely with hardware and often operate with kernel privileges.

Security professionals should ensure that drivers come from trusted sources and are appropriately maintained.

### 9.4 Security Modules

Linux Security Modules provide a framework for additional access-control checks.

Examples:

- AppArmor.
- SELinux.

These mechanisms can restrict what applications and services are allowed to do.

## 10. Practical Lab: Exploring Linux Kernel Architecture

**Environment:** Kali Linux virtual machine  
**Objective:** Identify kernel information and observe different kernel subsystems using safe inspection commands.

### Task 1: Identify the system architecture

```bash
uname -m
```

Possible outputs include:

- `x86_64` – 64-bit x86 architecture.
- `aarch64` – 64-bit ARM architecture.
- `i686` – 32-bit x86 architecture.

### Task 2: Inspect CPU information

```bash
lscpu
```

Observe:

- CPU architecture.
- Number of CPUs.
- CPU model.
- CPU cores.
- Virtualization information, if available.

### Task 3: Inspect memory management

```bash
free -h
```

```bash
head -n 15 /proc/meminfo
```

### Task 4: Inspect running processes

```bash
ps aux | head -n 15
```

Observe the process IDs and resource information.

### Task 5: Inspect filesystems

```bash
findmnt
```

This displays mounted filesystems and their mount points.

### Task 6: Inspect network interfaces

```bash
ip -br addr
```

```bash
ip route
```

### Task 7: Inspect kernel modules

```bash
lsmod | head -n 15
```

Observe the module names.

### Task 8: Inspect kernel security information

```bash
cat /sys/kernel/security/lsm
```

If the file is available, it displays the Linux Security Modules enabled on the running kernel.

Some systems may not expose this file or may require different access conditions.

## 11. Important Terms

| Term | Meaning |
|---|---|
| Kernel Architecture | Organization of kernel components |
| Monolithic Kernel | Kernel design where many core services operate in kernel space |
| Microkernel | Kernel design that keeps the privileged core relatively small |
| Hybrid Kernel | Architecture combining design approaches |
| Modular Kernel | Kernel supporting additional functionality through modules |
| Scheduler | Component responsible for scheduling processes and threads |
| VFS | Common interface for different filesystem implementations |
| Device Driver | Software that enables communication with hardware |
| IPC | Communication between processes |
| Attack Surface | Points through which a system may be exposed to attacks |
| LSM | Linux Security Modules framework |

## 12. Knowledge Check

Answer the following questions:

1. What is kernel architecture?
2. Explain the difference between monolithic and microkernel architecture.
3. What type of kernel architecture does Linux use?
4. What is a loadable kernel module?
5. Explain the responsibilities of the process scheduler.
6. What is the purpose of the Virtual Filesystem Switch?
7. What role do device drivers play?
8. What is Inter-Process Communication?
9. Why are kernel vulnerabilities potentially dangerous?
10. Name two security frameworks that use Linux kernel security mechanisms.

## 13. Practical Assignment

Create a report titled:

**Linux Kernel Architecture Investigation**

Include:

1. Your system architecture from `uname -m`.
2. CPU information from `lscpu`.
3. Memory information from `free -h`.
4. Five running processes.
5. Three mounted filesystems.
6. Your network interface names.
7. Five loaded kernel modules.
8. A short explanation of how three kernel subsystems work together when downloading a file.

## 14. Exam-Ready Summary

Kernel architecture describes how the internal components of an operating-system kernel are organized.

Major kernel architecture approaches include monolithic, microkernel and hybrid designs.

Linux is a monolithic kernel with support for loadable modules.

Important Linux kernel subsystems include:

- Process scheduling.
- Memory management.
- Virtual Filesystem Switch.
- Device drivers.
- Networking.
- Inter-Process Communication.
- Security mechanisms.

These subsystems cooperate to manage hardware, execute applications and enforce system security.

Understanding kernel architecture helps ethical hackers understand privileged system operations, identify potential attack surfaces and investigate security weaknesses.

## 15. Conclusion

You have learned how the Linux kernel is structured and how its major subsystems cooperate.

You should now understand why Linux is described as a monolithic kernel with modular capabilities.

In the next lesson, we will explore an important security concept: the separation between kernel space and user space.

**Next Lesson:** Lesson 03 – Kernel Space and User Space.