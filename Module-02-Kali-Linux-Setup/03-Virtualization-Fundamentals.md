---
---

# Lesson 03: Virtualization Fundamentals

## 1. Introduction to Virtualization

Virtualization is a technology that allows one physical computer to create and run multiple virtual computers.

These virtual computers are called Virtual Machines (VMs).

Each virtual machine can have its own:
- Operating system.
- Virtual CPU.
- Virtual memory.
- Virtual storage.
- Virtual network adapter.
- Applications and software.

For example, you can run Kali Linux inside a virtual machine while your main computer continues running Windows 11.

This allows you to practice Linux and cybersecurity without replacing your main operating system.

## 2. What Is a Virtual Machine?

A Virtual Machine (VM) is a software-based computer that behaves like a physical computer.

It uses resources from the physical computer but operates in a virtualized environment.

Example:

Your physical computer:
- Windows 11.
- Intel Core i7.
- 16 GB RAM.
- 256 GB SSD.

Your virtual computer:
- Kali Linux.
- Virtual CPU cores.
- Allocated virtual RAM.
- Virtual hard disk.
- Virtual network adapter.

The virtual machine uses part of your physical computer's resources.

## 3. Understanding Host and Guest Operating Systems

Virtualization introduces two important terms.

### A. Host Operating System

The host operating system is the main operating system installed directly on the physical computer.

Example:

Windows 11 Pro.

### B. Guest Operating System

The guest operating system runs inside a virtual machine.

Example:

Kali Linux.

### Example Architecture

```text
+----------------------------------+
|       PHYSICAL COMPUTER          |
|                                  |
|       Windows 11 Pro             |
|       HOST OPERATING SYSTEM      |
|                                  |
|  +----------------------------+  |
|  | VMware Workstation          |  |
|  |                            |  |
|  |  +----------------------+  |  |
|  |  | Kali Linux           |  |  |
|  |  | GUEST OPERATING SYS. |  |  |
|  |  +----------------------+  |  |
|  |                            |  |
|  +----------------------------+  |
+----------------------------------+
```

## 4. What Is a Hypervisor?

A hypervisor is software or firmware that creates and manages virtual machines.

It allocates physical resources such as CPU, memory, storage, and networking to virtual machines.

There are two major types.

### Type 1: Bare-Metal Hypervisor

A Type 1 hypervisor runs directly on physical hardware.

Examples:
- VMware ESXi.
- Microsoft Hyper-V Server.
- Xen.

It is commonly used in enterprise servers and data centers.

### Type 2: Hosted Hypervisor

A Type 2 hypervisor runs as an application on an existing operating system.

Examples:
- VMware Workstation.
- Oracle VirtualBox.
- Parallels Desktop.

For this course, we will use VMware Workstation on Windows 11.

## 5. Understanding VMware Workstation

VMware Workstation is desktop virtualization software that allows users to create and operate virtual machines.

It supports running multiple operating systems on one physical computer.

Examples of operating systems that can run inside VMware include:
- Kali Linux.
- Ubuntu.
- Debian.
- Windows.
- Metasploitable.

### Important VMware Features

**A. Virtual Machine Creation**

Allows users to create virtual computers.

**B. Virtual Hardware Configuration**

Allows users to allocate:
- CPU cores.
- RAM.
- Virtual storage.
- Network adapters.

**C. Snapshots**

Allows users to save a virtual machine's state so they can return to it later.

**D. Network Configuration**

Allows virtual machines to communicate using different network modes.

**E. Isolation**

Allows virtual machines to operate separately from the host operating system, although this is not an absolute security boundary.

## 6. Advantages of Virtualization

1. **Isolation:** Virtual machines can be separated from the main operating environment.
2. **Cost Reduction:** Multiple operating systems can run on one physical computer.
3. **Testing:** Software can be tested without installing it directly on the host.
4. **Snapshots:** Previous virtual machine states can be restored.
5. **Learning:** Students can practice different operating systems.
6. **Flexibility:** Virtual hardware can be configured for different workloads.
7. **Lab Creation:** Multiple virtual machines can be connected to create cybersecurity laboratories.

## 7. Disadvantages of Virtualization

1. **Resource Consumption:** Virtual machines consume CPU, RAM, and storage.
2. **Performance Overhead:** Virtual machines may run slower than systems installed directly on hardware.
3. **Storage Requirements:** Each virtual machine requires virtual disk space.
4. **Configuration Complexity:** Network and hardware settings may require technical knowledge.
5. **Security Risks:** Incorrect configuration can expose the host or other systems.

## 8. Understanding Virtual Machine Resources

When creating a virtual machine, you must allocate resources.

### A. CPU

CPU allocation determines how many virtual processor cores the VM can use.

### B. RAM

RAM allocation determines how much memory is available to the guest operating system.

### C. Virtual Disk

The virtual disk stores the guest operating system, applications, and files.

### D. Network Adapter

The virtual network adapter allows the guest operating system to communicate with networks.

### E. Graphics Memory

Graphics resources help display the virtual machine's graphical interface.

## 9. Recommended Kali Virtual Machine Configuration

For a standard Kali desktop virtual machine on your 16 GB RAM Windows laptop, begin with these settings:

| Resource | Suggested Allocation |
|---|---|
| Operating System | Kali Linux 64-bit |
| CPU | 2 virtual cores |
| RAM | 4 GB |
| Virtual Disk | 50 GB |
| Network Adapter | NAT |
| Firmware | UEFI, where supported |
| Desktop | XFCE |

These are starting recommendations, not mandatory requirements. You can adjust them depending on available storage and performance.

**Important:** Your laptop has a 256 GB SSD. Keep enough free storage for Windows, applications, virtual machine files, and future coursework.

## 10. Understanding Virtual Machine Network Modes

Virtualization software provides different networking modes.

### A. NAT (Network Address Translation)

NAT allows a virtual machine to access external networks through the host's network connection.

The guest normally uses a private virtual network and shares the host's external connection.

Uses:
- Downloading updates.
- Accessing websites.
- Installing packages.

### B. Bridged Networking

Bridged mode connects the virtual machine to the same physical network as the host.

The VM may receive an IP address from the physical network's DHCP server.

Uses:
- Authorized network testing.
- Testing communication between devices on a controlled network.

### C. Host-Only Networking

Host-only networking creates a private network between the host and connected virtual machines.

It normally does not provide direct external network access.

Uses:
- Isolated cybersecurity laboratories.
- Testing communication between Kali and vulnerable virtual machines.

### D. Custom Virtual Networking

Custom networking allows administrators to configure specific virtual networks and network connections.

**Security note:** NAT and host-only modes are useful for lab isolation, but they do not guarantee complete security. Avoid attaching vulnerable machines to untrusted networks.

## 11. Understanding Virtual Machine Snapshots

A snapshot records the state of a virtual machine at a particular point in time.

It can help you return to a previous state after testing or configuration changes.

Example:

1. Install Kali Linux.
2. Update the system.
3. Create a snapshot called `Clean-Kali-Installation`.
4. Perform experiments.
5. Revert to the snapshot if necessary.

Snapshots are not substitutes for independent backups.

They can consume considerable disk space, especially when the VM changes frequently.

## 12. Virtualization in Cybersecurity

Virtualization is important in ethical hacking because it allows security students to create controlled testing environments.

Example laboratory:

```text
+------------------------------------+
|       Windows 11 HOST               |
|                                    |
|          VMware Workstation         |
|                                    |
|  +--------------+  +--------------+ |
|  | Kali Linux   |  | Metasploitable|
|  | Attacker VM  |  | Target VM     |
|  +--------------+  +--------------+ |
|           HOST-ONLY NETWORK         |
+------------------------------------+
```

In this laboratory:

- Kali Linux acts as the testing machine.
- Metasploitable acts as the intentionally vulnerable target.
- Host-only networking connects the machines privately.

This setup is suitable for learning penetration testing without targeting public systems.

## 13. Practical Lab: Understanding Your Computer's Virtualization Support

Before creating a virtual machine, inspect your Windows computer.

Open Windows CMD or PowerShell.

### Task 1: Identify your Windows version

```cmd
winver
```

### Task 2: Inspect CPU information

```cmd
systeminfo
```

Look for the Hyper-V Requirements section, if displayed.

### Task 3: Check virtualization support

Open Task Manager:

1. Press `Ctrl + Shift + Esc`.
2. Select Performance.
3. Select CPU.
4. Find the Virtualization field.

It should indicate whether virtualization is enabled.

### Task 4: Check available memory

Open CMD:

```cmd
systeminfo | findstr /C:"Total Physical Memory" /C:"Available Physical Memory"
```

### Task 5: Check available storage

```cmd
wmic logicaldisk get caption,freespace,size
```

If WMIC is unavailable on your Windows installation, use PowerShell:

```powershell
Get-Volume
```

### Task 6: Understand your virtual machine requirements

Write down:
- Available RAM.
- Available SSD space.
- CPU virtualization status.
- Recommended Kali VM configuration.

Do not change BIOS or Windows virtualization settings yet. We will inspect the environment before configuring VMware.

## 14. Exam-Ready Questions and Answers

**Q1. What is virtualization?**

Virtualization is a technology that allows one physical computer to create and run multiple virtual computing environments.

**Q2. What is a virtual machine?**

A virtual machine is a software-based computer that uses virtualized hardware resources to run an operating system and applications.

**Q3. What is a hypervisor?**

A hypervisor is software or firmware responsible for creating and managing virtual machines.

**Q4. Mention the two types of hypervisors.**

Type 1 (bare-metal) and Type 2 (hosted).

**Q5. What is the difference between a host OS and a guest OS?**

The host OS runs directly on the physical computer, while the guest OS runs inside a virtual machine.

**Q6. What is NAT networking?**

NAT networking allows a virtual machine to access external networks through the host's network connection.

**Q7. What is host-only networking?**

Host-only networking provides a private virtual network connecting the host and virtual machines without normally providing direct external network access.

**Q8. What is a snapshot?**

A snapshot records a virtual machine's state at a particular point in time and allows the user to return to that state later.

**Q9. Mention four advantages of virtualization.**

Isolation, testing, resource flexibility, and snapshot recovery.

**Q10. Why is virtualization useful in ethical hacking?**

It allows security professionals and students to create controlled environments for testing vulnerabilities and practicing penetration testing.

## 15. Knowledge Check

Answer without looking at your notes.

1. Define virtualization.
2. What is a virtual machine?
3. What is a hypervisor?
4. Differentiate between Type 1 and Type 2 hypervisors.
5. What is the difference between a host OS and guest OS?
6. Mention four resources allocated to a VM.
7. What is NAT networking?
8. What is bridged networking?
9. What is host-only networking?
10. Why are snapshots important?
11. What is the purpose of VMware Workstation?
12. Why is virtualization important in cybersecurity?

## 16. Lesson Summary

In this lesson, we studied:

- Virtualization.
- Virtual machines.
- Host and guest operating systems.
- Hypervisors.
- VMware Workstation.
- Advantages and disadvantages of virtualization.
- Virtual machine resource allocation.
- NAT, bridged, and host-only networking.
- Snapshots and backups.
- Virtualization in cybersecurity.

**Key takeaway:** Virtualization allows you to create controlled computing environments on one physical computer. It is one of the most useful technologies for building safe cybersecurity laboratories.