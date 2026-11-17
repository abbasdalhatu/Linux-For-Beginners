---
---

# LESSON 06: CREATING A KALI LINUX VIRTUAL MACHINE

**Module:** 02 – Kali Linux Setup  
**File:** `06-Creating-Kali-Virtual-Machine.md`

## 1. Introduction

Before installing Kali Linux, we need to create a virtual machine (VM).

A virtual machine is a software-based computer that runs inside your physical computer.

Our physical laptop runs Windows 11, while our virtual machine will run Kali Linux.

This means we can learn Linux and ethical hacking without replacing or modifying our Windows operating system.

## 2. Understanding Virtual Machine Resources

A virtual machine uses some of the physical computer's resources.

| Resource | Meaning |
|---|---|
| CPU | Processes instructions |
| RAM | Temporarily stores running programs and data |
| Virtual Disk | Stores the guest operating system and files |
| Network Adapter | Connects the VM to a network |

Our Dell Latitude 7320 has:

- Intel Core i7 processor
- 16 GB RAM
- 256 GB SSD
- Windows 11 Pro

We must allocate resources carefully because Windows also needs memory and storage.

## 3. Recommended Kali VM Configuration

For our initial setup, use:

| Setting | Recommended value |
|---|---|
| Virtual machine name | Kali-Linux |
| Guest OS | Linux |
| Version | Debian 12.x 64-bit, or closest available Debian 64-bit option |
| CPU cores | 2 |
| RAM | 4 GB |
| Virtual disk | 50 GB |
| Network | NAT |
| Firmware | UEFI |
| Display | Default VMware settings |

**Storage warning:** A 50 GB virtual disk is the maximum virtual-disk capacity, not necessarily 50 GB immediately consumed if you choose a growable disk. Check your available Windows storage before creating it. If space is limited, stop and review the available space first.

## 4. Create the Virtual Machine

### Step 1: Open VMware

1. Start VMware Workstation.
2. Select **Create a New Virtual Machine**.
3. Choose **Typical (recommended)**.
4. Click Next.

### Step 2: Select the Installation Source

Choose:

**I will install the operating system later.**

We will attach our Kali ISO during the next lesson.

Click Next.

### Step 3: Select the Guest Operating System

Choose:

- Guest operating system: Linux
- Version: Debian 12.x 64-bit, or the closest available Debian 64-bit option.

Click Next.

### Step 4: Name Your Virtual Machine

Enter:

`Kali-Linux`

Choose a storage location with sufficient free space.

Avoid placing the VM inside a system-protected Windows folder.

Click Next.

### Step 5: Configure the Virtual Disk

Set the maximum disk size to:

`50 GB`

Choose:

**Store virtual disk as a single file** (if convenient and supported by your storage setup).

Select Finish.

## 5. Configure Hardware

Before starting the VM, select **Edit virtual machine settings**.

### Memory

Set:

`4096 MB`

This gives Kali 4 GB of RAM.

### Processors

Set:

- Number of processors: 1
- Number of cores per processor: 2

### Network Adapter

Select:

**NAT**

NAT allows Kali to access the internet through Windows using VMware's network connection.

We will use a more isolated Host-only network later when building our ethical hacking lab.

### CD/DVD

Leave the virtual CD/DVD device available. We will attach the Kali ISO in Lesson 07.

Click OK to save the settings.

## 6. Understand the Network Modes

VMware commonly provides three network modes:

| Mode | Explanation |
|---|---|
| NAT | VM accesses external networks through the host |
| Bridged | VM appears as a separate device on the physical network |
| Host-only | Creates a private network between the host and VMs |

For now, use NAT.

Do not use Bridged mode for our vulnerable lab machines.

## 7. Practical Assignment

Complete these tasks:

- [ ] Open VMware Workstation.
- [ ] Create a new virtual machine.
- [ ] Select Typical configuration.
- [ ] Choose to install the OS later.
- [ ] Select Linux and a Debian 64-bit option.
- [ ] Name the VM Kali-Linux.
- [ ] Configure 4 GB RAM.
- [ ] Configure 2 CPU cores.
- [ ] Configure a 50 GB virtual disk, provided your storage allows it.
- [ ] Set the network adapter to NAT.
- [ ] Save the configuration.
- [ ] Confirm that Kali-Linux appears in the VMware library.

**Do not power on the virtual machine yet.**

## 8. Knowledge Check

1. What is a virtual machine?
2. What is the difference between host and guest operating systems?
3. Why are we allocating only 4 GB of RAM?
4. What does NAT mean?
5. Why should we avoid Bridged mode for our vulnerable lab?

## 9. Lesson Summary

You have learned how to create a virtual machine, allocate hardware resources, configure virtual storage, and select an appropriate network mode.

**Next Lesson:** Lesson 07 – Installing Kali Linux.