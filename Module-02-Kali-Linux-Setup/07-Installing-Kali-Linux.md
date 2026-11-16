# LESSON 07: INSTALLING KALI LINUX

**Module:** 02 – Kali Linux Setup  
**File:** `07-Installing-Kali-Linux.md`

## 1. Introduction

In this lesson, we will install Kali Linux inside the virtual machine we created in VMware.

We will use the graphical installer, which provides a step-by-step interface for configuring the operating system.

At the end, Kali Linux should boot into its desktop environment.

## 2. Attach the Kali ISO

Before installation, we must connect the downloaded ISO file to our virtual machine.

### Steps

1. Open VMware Workstation.
2. Select your `Kali-Linux` virtual machine.
3. Click **Edit virtual machine settings**.
4. Select **CD/DVD (SATA)**.
5. Select **Use ISO image file**.
6. Click Browse.
7. Locate the Kali Installer ISO you downloaded.
8. Select the ISO file.
9. Enable **Connect at power on**, if available.
10. Click OK.

The ISO now acts like a virtual installation DVD.

## 3. Start the Virtual Machine

1. Select `Kali-Linux`.
2. Click **Power on this virtual machine**.
3. Wait for the Kali boot menu.

You should see installation options.

Select:

**Graphical install**

Press Enter.

## 4. Configure the Installation

Follow the installer prompts.

### Step 1: Language

Choose:

English

### Step 2: Location

Choose:

Nigeria

### Step 3: Keyboard

Choose:

American English, or the keyboard layout that matches your physical keyboard.

### Step 4: Hostname

Enter:

`kali`

The hostname identifies your computer on a network.

### Step 5: Domain Name

Leave the domain name blank unless you have a specific domain configuration.

### Step 6: Create a User Account

Create your normal user account.

For example:

- Full name: Your preferred display name
- Username: `kaliuser`
- Password: Create a strong password and keep it private.

Do not share your password in this conversation.

Modern Kali installations use a regular user account by default. Administrative tasks can be performed using `sudo`.

## 5. Configure the Clock

Select your appropriate time zone.

For Nigeria, select:

`Africa/Lagos`

## 6. Partition the Disk

The installer will ask how to partition the disk.

Select:

**Guided – use entire disk**

You should see the VMware virtual disk that you created.

Important:

The installer is operating on the virtual disk, not your physical Windows SSD partition.

Select the available virtual disk and continue.

For the partitioning scheme, choose:

**All files in one partition (recommended for new users)**

Select Finish partitioning and write changes to disk.

When asked whether to write changes to the disk, select:

**Yes**

Double-check that the target is the VMware virtual disk before confirming.

## 7. Install the Base System

The installer will copy and configure the required system files.

This may take several minutes.

Wait patiently.

Do not shut down the VM during installation.

## 8. Select Software

When asked which software to install, keep the standard Kali desktop environment and recommended default tools selected.

For the desktop environment, XFCE is a good choice for our laptop because it is relatively lightweight.

Avoid selecting every available tool collection. We can install additional tools when needed.

## 9. Install the GRUB Bootloader

When asked whether to install the GRUB bootloader, choose:

**Yes**

Select the virtual disk as the installation destination, usually something like:

`/dev/sda`

This is the virtual disk, not your Windows boot drive.

## 10. Complete Installation

When installation finishes:

1. Select Continue or Finish installation.
2. Allow the VM to reboot.
3. If the installer starts again, disconnect the ISO from the virtual CD/DVD device or remove it from the boot sequence.
4. Allow Kali to boot from the installed virtual disk.

You should eventually see the Kali login screen.

Log in using the username and password you created.

## 11. Verify Your Installation

Open the Kali terminal.

Run these commands individually:

### Check the operating system

```bash
cat /etc/os-release
```

This displays information about your Linux distribution.

### Check the kernel

```bash
uname -r
```

This displays the installed Linux kernel version.

### Check the hostname

```bash
hostname
```

Expected result:

```text
kali
```

### Check the current user

```bash
whoami
```

Expected result:

```text
kaliuser
```

The result will match the username you created.

### Check memory

```bash
free -h
```

This displays the available and used memory.

### Check storage

```bash
lsblk
```

This displays the available block devices and partitions.

## 12. Practical Assignment

- [ ] Attach the Kali ISO to VMware.
- [ ] Start the virtual machine.
- [ ] Select Graphical install.
- [ ] Configure language and keyboard.
- [ ] Set hostname to kali.
- [ ] Create your normal user account.
- [ ] Configure the Nigeria time zone.
- [ ] Partition the VMware virtual disk.
- [ ] Install the base system.
- [ ] Install the GRUB bootloader on the virtual disk.
- [ ] Complete installation.
- [ ] Log in to Kali.
- [ ] Run all six verification commands.

## 13. Knowledge Check

1. What is an ISO file used for during installation?
2. What is the purpose of a hostname?
3. Why do we create a normal user account instead of working as root all the time?
4. What is GRUB?
5. What does `uname -r` display?
6. What does `lsblk` display?

## 14. Lesson Summary

In this lesson, we installed Kali Linux inside VMware.

We learned how to configure the operating system, create a user account, partition a virtual disk, install GRUB, and verify the installation using terminal commands.

**Next Lesson:** Lesson 08 – Initial Kali Configuration.