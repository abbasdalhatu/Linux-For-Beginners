---
---

# Lesson 07: The Linux Boot Process

## 1. Introduction

Every time you turn on a computer running Linux, a sequence of events takes place before you can use the operating system.

This sequence is called the **Linux boot process**.

During booting, the computer:
1. Powers on and initializes its hardware.
2. Performs hardware checks.
3. Finds and loads the bootloader.
4. Loads the Linux kernel into memory.
5. Initializes essential hardware and system components.
6. Starts the initial userspace environment.
7. Starts system services.
8. Presents the login screen or terminal.

Understanding this process is important for Linux administrators, system engineers, and ethical hackers because it helps with troubleshooting, system recovery, and understanding how Linux starts.

## 2. What Is Booting?

Booting is the process of starting a computer and loading its operating system into memory.

When you press the power button, the computer does not immediately display the Linux desktop.

Several components work together to start the operating system.

### Main components involved

| Component | Responsibility |
|---|---|
| BIOS/UEFI | Initializes hardware and begins the boot process |
| Bootloader | Loads the Linux kernel |
| Kernel | Initializes and manages system resources |
| initramfs | Provides an initial temporary userspace environment |
| Init system | Starts and manages system services |
| Login manager | Provides a graphical login screen when configured |

## 3. The Linux Boot Process: Overview

The following diagram illustrates a typical modern Linux boot sequence.

```text
        COMPUTER POWER ON
                |
                v
          BIOS / UEFI
                |
                v
        HARDWARE INITIALIZATION
                |
                v
          BOOTLOADER (GRUB)
                |
                v
          LINUX KERNEL
                |
                v
             INITRAMFS
                |
                v
          ROOT FILESYSTEM
                |
                v
       INIT SYSTEM (SYSTEMD)
                |
                v
         SYSTEM SERVICES
                |
                v
       LOGIN SCREEN / TERMINAL
```

Each stage depends on the previous stage completing successfully.

## 4. Stage One: Power-On and BIOS/UEFI

When you press the power button, the computer begins its startup sequence.

The firmware responsible for this stage is usually BIOS or UEFI.

### What is BIOS?

BIOS stands for Basic Input/Output System.

It is traditional motherboard firmware responsible for initializing hardware and beginning the boot process.

### What is UEFI?

UEFI stands for Unified Extensible Firmware Interface.

It is the modern firmware interface used by most contemporary computers.

UEFI supports features such as:
- GPT partitioning.
- Secure Boot.
- Firmware boot managers.
- More flexible boot configurations.

### BIOS vs UEFI

| BIOS | UEFI |
|---|---|
| Older firmware standard | Modern firmware standard |
| Traditionally associated with MBR booting | Commonly used with GPT |
| More limited boot environment | More advanced boot environment |
| Traditionally uses text-based configuration | Can provide graphical configuration |

### What happens during this stage?

The firmware:
1. Initializes essential hardware.
2. Performs hardware checks.
3. Detects available boot devices.
4. Identifies a bootable device.
5. Transfers control to the next boot stage.

Depending on the system, hardware checks may include memory, processor, storage, and other connected components.

## 5. Stage Two: The Bootloader

After firmware initialization, the system transfers control to a bootloader.

A bootloader is a program responsible for loading an operating system kernel.

One commonly used Linux bootloader is GRUB.

GRUB stands for GRand Unified Bootloader.

### What does GRUB do?

GRUB can:
- Locate available Linux kernels.
- Display a boot menu.
- Load the selected kernel.
- Pass boot parameters to the kernel.
- Provide access to recovery or alternate boot entries when configured.

### Example GRUB boot menu

A Linux computer with multiple boot entries might display:

```text
GNU GRUB

Kali GNU/Linux
Advanced options for Kali GNU/Linux
Memory test
```

The available options depend on the system configuration.

### Why is the bootloader important?

Without a functioning bootloader or another valid boot mechanism, the computer may be unable to start the installed operating system normally.

## 6. Stage Three: Loading the Linux Kernel

After the bootloader selects an operating system entry, it loads the Linux kernel into memory.

The kernel is the central component of the Linux operating system.

Its responsibilities include:
- Managing CPU operations.
- Managing system memory.
- Detecting and communicating with hardware.
- Managing processes.
- Providing system calls.
- Managing filesystems and devices.

### What happens when the kernel loads?

The kernel begins initializing the system.

It establishes essential internal structures, initializes available drivers, and prepares the environment needed to continue booting.

The kernel also receives boot parameters from the bootloader.

These parameters can influence how the kernel starts.

## 7. Stage Four: initramfs

After the kernel begins initialization, it uses an initial temporary userspace environment called initramfs.

initramfs means Initial RAM Filesystem.

It provides essential tools and drivers needed to continue the boot process.

### Why is initramfs needed?

Imagine that your Linux root filesystem is stored on a device that requires a special storage driver.

The kernel may need that driver before it can access the root filesystem.

The initial RAM filesystem provides the necessary environment to load such components.

### Responsibilities of initramfs

- Loads essential drivers.
- Helps discover storage devices.
- Supports encrypted or complex storage configurations.
- Locates the real root filesystem.
- Helps prepare the system to switch to the real root filesystem.

### Important distinction

The initramfs is temporary.

It is used during early startup before the system transitions to its actual root filesystem.

## 8. Stage Five: Mounting the Root Filesystem

The Linux root filesystem is represented by `/`.

It contains the main directory structure required by the operating system.

Examples include:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── root
├── usr
└── var
```

During booting, the system locates and makes the real root filesystem available.

After the necessary early initialization, the system transitions from the temporary initial environment to the real root filesystem.

This transition is commonly called switching root.

### Why is the root filesystem important?

Without access to the required root filesystem, Linux cannot complete normal startup.

Incorrect filesystem configuration, missing storage devices, or filesystem corruption can prevent successful booting.

## 9. Stage Six: The Init System

Once the real root filesystem is available, the init system takes responsibility for bringing up userspace services.

On modern Kali Linux installations, the init system is normally systemd.

The init system is one of the first major userspace processes started during boot.

It typically runs as PID 1.

### What is PID 1?

PID means Process ID.

Every running process has a process identifier.

The init system normally receives process ID 1.

Inspect it using:

```bash
ps -p 1 -o pid,comm,args
```

You should see information about the process running with PID 1.

### Responsibilities of systemd

systemd:
- Starts system services.
- Manages dependencies between services.
- Handles system targets.
- Coordinates startup tasks.
- Manages certain system resources.
- Helps control shutdown and reboot operations.

We will study systemd in greater detail in Lesson 08.

## 10. Stage Seven: Starting System Services

After the init system starts, it activates the services required by the system's configuration.

A service is a background program that provides a particular function.

Examples include:
- Networking services.
- Logging services.
- Time synchronization.
- SSH services, when installed and enabled.
- Display management services.

Not every Linux installation runs the same services.

### What is a daemon?

A daemon is a background process that provides a service without requiring direct interaction from a user.

For example, an SSH daemon can accept remote SSH connections when the service is installed, configured, and running.

Inspect services with:

```bash
systemctl --type=service --state=running
```

This displays currently running systemd services.

## 11. Stage Eight: Login Screen or Terminal

When the required startup tasks are complete, Linux presents a user interface.

Depending on the installation, this may be:

### A. Graphical login

A graphical login manager displays a screen where users enter their credentials.

After successful authentication, the desktop environment starts.

Examples of Linux desktop environments include:
- GNOME.
- KDE Plasma.
- Xfce.

### B. Command-line login

A command-line installation may display a text-based login prompt.

After authentication, the user receives a shell.

Both graphical and command-line environments operate on top of the Linux kernel.

## 12. Understanding the Complete Boot Sequence

Let's summarize the entire process.

| Stage | Component | Main responsibility |
|---|---|---|
| 1 | Power-on | Starts the computer |
| 2 | BIOS/UEFI | Initializes hardware |
| 3 | Bootloader | Selects and loads the kernel |
| 4 | Kernel | Initializes core system components |
| 5 | initramfs | Provides temporary early userspace |
| 6 | Root filesystem | Makes the real operating system filesystem available |
| 7 | Init system | Starts and coordinates userspace services |
| 8 | Services | Provides system functionality |
| 9 | Login | Allows the user to access Linux |

Remember that some implementation details vary depending on firmware, bootloader, distribution, storage configuration, and init system.

## 13. Inspecting the Boot Process in Kali Linux

Linux provides several commands that help you investigate how the computer started.

### 13.1 Check system uptime

```bash
uptime
```

Displays how long the system has been running.

### 13.2 Check the system boot time

```bash
who -b
```

Displays the most recent system boot time.

### 13.3 Inspect the kernel command line

```bash
cat /proc/cmdline
```

Displays the boot parameters passed to the running kernel.

These parameters can provide useful information about how the kernel was started.

### 13.4 Inspect PID 1

```bash
ps -p 1 -o pid,comm,args
```

Displays information about the first userspace process.

### 13.5 Inspect the system boot journal

```bash
journalctl -b
```

Displays logs from the current boot.

### 13.6 Display kernel boot messages

```bash
dmesg | less
```

Displays kernel messages in a scrollable interface.

Press `q` to exit.

### 13.7 Check system startup duration

```bash
systemd-analyze
```

Displays timing information about the system startup process when supported.

For a more detailed breakdown:

```bash
systemd-analyze blame
```

This displays services ordered by their reported startup time.

A service taking longer does not automatically mean it is malfunctioning.

## 14. Common Linux Boot Problems

Understanding the boot sequence makes troubleshooting easier.

### Problem 1: GRUB does not appear

Possible causes:
- Incorrect firmware boot order.
- Bootloader installation problems.
- Missing or damaged boot files.

### Problem 2: Kernel panic

A kernel panic occurs when the kernel encounters a serious problem and cannot safely continue.

Possible causes include:
- Kernel bugs.
- Serious driver problems.
- Hardware faults.
- Filesystem or storage problems.

### Problem 3: Unable to mount root filesystem

Possible causes:
- Incorrect root filesystem configuration.
- Missing storage drivers.
- Damaged filesystems.
- Unavailable storage devices.

### Problem 4: System stuck during startup

Possible causes:
- A failing service.
- Hardware initialization problems.
- Driver problems.
- Filesystem errors.

### Problem 5: Graphical login does not appear

Possible causes:
- Display manager failure.
- Graphics driver problems.
- Desktop environment issues.

**Important:** Do not immediately reinstall Linux when a boot problem occurs. First identify which stage is failing.

## 15. Linux Boot Process and Cybersecurity

The boot process is important in cybersecurity because attackers may attempt to interfere with system startup.

### 15.1 Bootloader attacks

If an attacker gains unauthorized control over the bootloader, they may attempt to change boot parameters or interfere with the operating system startup process.

Physical access and bootloader protections are therefore important security considerations.

### 15.2 Bootkits

A bootkit is malicious software designed to compromise the boot process or components involved in starting an operating system.

Bootkits can be difficult to detect because they may operate before normal operating system security tools start.

### 15.3 Secure Boot

Secure Boot is a UEFI security feature designed to verify trusted boot components before allowing them to execute.

It helps reduce the risk of unauthorized boot components being loaded.

Secure Boot is not a complete defense against every boot-level threat.

### 15.4 Defensive security practices

1. Protect physical access to your computer.
2. Use firmware security features appropriately.
3. Keep the operating system and boot components updated.
4. Use trusted bootloader and kernel packages.
5. Protect important firmware and boot settings.
6. Monitor unexpected changes to boot configuration.
7. Maintain backups and recovery options.

## 16. Practical Lab: Investigating the Linux Boot Process

### Lab objective

Use Kali Linux commands to investigate the system's startup information.

This is an inspection-only exercise. You will not change firmware settings, bootloader configuration, or startup services.

### Step 1: Check uptime

```bash
uptime
```

Record how long your Kali VM has been running.

### Step 2: Check the last boot time

```bash
who -b
```

Observe the displayed boot time.

### Step 3: Inspect PID 1

```bash
ps -p 1 -o pid,comm,args
```

Identify the process running as PID 1.

### Step 4: Inspect kernel boot parameters

```bash
cat /proc/cmdline
```

Observe the parameters passed to the kernel.

### Step 5: Inspect current boot logs

```bash
journalctl -b -n 50
```

Review the latest 50 log entries from the current boot.

### Step 6: Inspect kernel startup messages

```bash
dmesg | tail -n 30
```

Look for hardware detection and initialization messages.

### Step 7: Check startup duration

```bash
systemd-analyze
```

Record the reported startup timing if available.

### Step 8: Inspect service startup timing

```bash
systemd-analyze blame
```

Observe which services took the longest to start.

### Step 9: Inspect running services

```bash
systemctl --type=service --state=running
```

Observe some of the services active on your Kali VM.

### Lab safety

Do not:
- Modify GRUB.
- Change BIOS or UEFI settings.
- Disable Secure Boot.
- Remove startup services.
- Change boot parameters.
- Attempt to install a bootkit or modify boot files.

## 17. Knowledge Check

Answer these questions in your notebook.

1. What is the Linux boot process?
2. What is the difference between BIOS and UEFI?
3. What is a bootloader?
4. What is GRUB?
5. What happens when the Linux kernel loads?
6. What is initramfs?
7. Why does Linux need a root filesystem?
8. What is an init system?
9. What is PID 1?
10. What is a daemon?
11. What happens after systemd starts?
12. What is Secure Boot?
13. What is a bootkit?
14. Which command displays the current kernel boot parameters?
15. Which command displays information about PID 1?

## 18. Practical Assignment

Write a short report titled:

**Investigating the Linux Boot Process in Kali Linux**

Your report should contain:

1. An explanation of the Linux boot process.
2. The difference between BIOS and UEFI.
3. The role of GRUB.
4. The role of the Linux kernel during startup.
5. The purpose of initramfs.
6. The role of systemd.
7. Your Kali VM's PID 1 process.
8. Your system's boot time.
9. One interesting message from the kernel logs.
10. Three security risks associated with the Linux boot process.

Include command outputs where appropriate.

## 19. Key Terms

| Term | Meaning |
|---|---|
| Booting | Starting a computer and loading its operating system |
| BIOS | Basic Input/Output System |
| UEFI | Unified Extensible Firmware Interface |
| Bootloader | Program responsible for loading an operating system kernel |
| GRUB | GRand Unified Bootloader |
| Kernel | Core component of the Linux operating system |
| initramfs | Temporary initial userspace environment |
| Root filesystem | Main filesystem mounted as `/` |
| Init system | Component responsible for starting userspace services |
| PID 1 | Process identifier normally assigned to the init system |
| Daemon | Background process that provides a service |
| Secure Boot | UEFI feature that verifies trusted boot components |
| Bootkit | Malware that compromises the boot process |

## 20. Exam Summary

Remember these important points:

- Booting is the process of starting a computer and loading its operating system.
- BIOS and UEFI initialize hardware and begin the boot process.
- GRUB is a commonly used Linux bootloader.
- The bootloader loads the Linux kernel.
- The kernel initializes core system components.
- initramfs provides a temporary early userspace environment.
- Linux transitions to its real root filesystem.
- systemd commonly acts as the init system on modern Kali installations.
- The init system normally runs as PID 1.
- System services start before the user accesses the system.
- Secure Boot helps verify trusted boot components.
- Bootkits are a type of malware that targets the boot process.

## 21. Conclusion

The Linux boot process is a sequence of important stages that work together to start the operating system.

You have learned how firmware, the bootloader, the kernel, initramfs, the root filesystem, and the init system contribute to startup.

You have also learned how to inspect boot information using Linux commands.

Understanding these stages will help you diagnose startup failures and recognize security threats that target the boot process.

**Next Lesson: Lesson 08 — Init Systems and systemd.**