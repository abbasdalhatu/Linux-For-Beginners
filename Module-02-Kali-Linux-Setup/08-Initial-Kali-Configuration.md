# LESSON 08: INITIAL KALI CONFIGURATION

**Module:** 02 – Kali Linux Setup  
**File:** `08-Initial-Kali-Configuration.md`

## 1. Introduction

After installing an operating system, we need to perform some initial configuration.

Initial configuration means preparing the system for normal use by checking its identity, network connection, time settings, storage, and essential system settings.

In this lesson, we will configure our Kali Linux virtual machine.

## 2. Open the Terminal

The terminal is a command-line interface used to communicate with Linux.

In Kali XFCE:

1. Log in to Kali.
2. Open the Applications menu.
3. Search for Terminal.
4. Launch the terminal.

You can also use the keyboard shortcut:

`Ctrl + Alt + T`

## 3. Verify Your System Identity

Run the following commands.

### Check the hostname

```bash
hostname
```

Expected result:

```text
kali
```

### Check your username

```bash
whoami
```

This displays the account currently logged in.

### Check the operating system

```bash
cat /etc/os-release
```

This displays Kali Linux distribution information.

### Check the kernel version

```bash
uname -r
```

This displays the running Linux kernel version.

## 4. Configure the Hostname

A hostname is the name assigned to a computer on a network.

Our hostname should be:

`kali`

Check it:

```bash
hostnamectl
```

If your hostname is not `kali`, you can change it using:

```bash
sudo hostnamectl set-hostname kali
```

Enter your password if requested.

Verify again:

```bash
hostname
```

## 5. Check the Network Connection

Networking is essential for downloading updates and installing packages.

### Step 1: Display network interfaces

```bash
ip addr
```

This displays your network interfaces and assigned IP addresses.

Look for an interface such as:

`eth0`

or another name beginning with `ens`.

### Step 2: Check your routing table

```bash
ip route
```

Look for a default route.

A default route tells Kali where to send traffic destined for other networks.

### Step 3: Test internet connectivity

```bash
ping -c 4 1.1.1.1
```

This sends four packets to Cloudflare's public DNS address.

Then test DNS resolution:

```bash
ping -c 4 kali.org
```

If the first test works but the second fails, there may be a DNS problem.

If both fail, check the VMware NAT network adapter and Windows internet connection.

## 6. Check Date and Time

Correct system time is important for logs, certificates, network security, and troubleshooting.

Run:

```bash
timedatectl
```

Look for the time zone.

For Nigeria, the expected time zone is:

`Africa/Lagos`

If it is incorrect, run:

```bash
sudo timedatectl set-timezone Africa/Lagos
```

Verify:

```bash
timedatectl
```

## 7. Check Disk Storage

Run:

```bash
df -h
```

This displays filesystem storage usage in a human-readable format.

Also run:

```bash
lsblk
```

This displays disks, partitions, and mount points.

Remember that your Kali virtual disk is separate from your Windows filesystem at the operating-system level.

## 8. Check Memory and CPU

### Memory

```bash
free -h
```

This displays total, used, and available memory.

### CPU

```bash
lscpu
```

This displays CPU architecture, processor information, and available cores.

Because we configured two virtual CPU cores and 4 GB RAM, Kali should see approximately those allocated resources, although some displayed memory may be reserved.

## 9. Create a Course Workspace

We will create a dedicated directory for our future ethical hacking labs.

Run:

```bash
mkdir -p ~/Linux-Ethical-Hacking/{Labs,Notes,Tools,Reports}
```

Explanation:

- `mkdir` creates directories.
- `-p` creates parent directories when necessary.
- `~` represents your home directory.
- `{}` allows us to create multiple directories in one command.

Check the result:

```bash
ls -R ~/Linux-Ethical-Hacking
```

You should see:

```text
Linux-Ethical-Hacking/
├── Labs
├── Notes
├── Reports
└── Tools
```

## 10. Understand Important Kali Directories

| Directory | Purpose |
|---|---|
| `/home` | Normal users' personal directories |
| `/root` | Root user's home directory |
| `/etc` | System configuration files |
| `/var` | Logs and variable data |
| `/tmp` | Temporary files |
| `/usr` | Applications and system resources |
| `/bin` | Essential command binaries (often linked into `/usr/bin`) |
| `/boot` | Boot-related files |

We will study the Linux filesystem in detail in Module 03.

## 11. Practical Assignment

Complete these tasks:

- [ ] Open the Kali terminal.
- [ ] Verify your hostname.
- [ ] Verify your username.
- [ ] Check the operating system.
- [ ] Check the kernel version.
- [ ] Check your network interfaces.
- [ ] Check the routing table.
- [ ] Test internet connectivity.
- [ ] Verify the Africa/Lagos time zone.
- [ ] Check disk usage.
- [ ] Check memory and CPU.
- [ ] Create the Linux-Ethical-Hacking workspace.
- [ ] Display the workspace directory structure.

## 12. Knowledge Check

1. What is a hostname?
2. What does `ip addr` display?
3. What is the purpose of `ip route`?
4. What does `timedatectl` do?
5. What is the difference between `df -h` and `lsblk`?
6. What does `mkdir -p` do?
7. Why is correct system time important in cybersecurity?

## 13. Lesson Summary

You have learned how to configure and inspect a fresh Kali Linux installation.

You can now verify your system identity, network connection, time zone, storage, memory, and CPU.

You also created a dedicated workspace for future practical cybersecurity exercises.

**Next Lesson:** Lesson 09 – Updating and Upgrading Kali Linux.