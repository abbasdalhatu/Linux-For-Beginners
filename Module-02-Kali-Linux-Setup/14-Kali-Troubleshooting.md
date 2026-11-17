---
---

# LESSON 14: KALI LINUX TROUBLESHOOTING

**Module:** 02 – Kali Linux Setup  
**File:** `14-Kali-Troubleshooting.md`

## 1. Introduction

Troubleshooting is the process of identifying, diagnosing, and resolving problems in a computer system.

As a Linux administrator or cybersecurity professional, you must know how to investigate system errors rather than randomly executing commands.

In this lesson, we will learn how to diagnose common Kali Linux and VMware problems.

## 2. Understanding the Troubleshooting Process

A good troubleshooting process follows these steps:

1. Identify the problem.
2. Gather information.
3. Investigate possible causes.
4. Test a solution.
5. Verify whether the problem is resolved.
6. Document the result.

Never make several unrelated changes at once. Otherwise, it becomes difficult to identify which change fixed or caused the problem.

## 3. Problem: Kali Linux Has No Internet Connection

One common problem is losing internet connectivity inside the virtual machine.

### Step 1: Check the network interface

```bash
ip addr
```

Look for your Ethernet interface.

If it has no IPv4 address, it may not have received network configuration.

### Step 2: Check the routing table

```bash
ip route
```

Look for a default route.

### Step 3: Test the gateway

Use the gateway address displayed by `ip route`.

Example:

```bash
ping -c 4 192.168.100.2
```

Replace the example address with your actual gateway.

### Step 4: Test external connectivity

```bash
ping -c 4 1.1.1.1
```

### Step 5: Test DNS

```bash
getent hosts kali.org
```

### Step 6: Inspect VMware

Check that:

- The VM network adapter is enabled.
- The network mode is NAT.
- Connect at power on is enabled.
- The Windows host has internet access.

Do not randomly change network adapters or disable Windows security protections.

## 4. Problem: APT Update Fails

Sometimes Kali cannot retrieve package information.

Possible causes include:

- No internet connection.
- DNS problems.
- Incorrect repository configuration.
- Temporary repository availability problems.
- Incorrect system date and time.

### Diagnostic commands

Check connectivity:

```bash
ping -c 4 1.1.1.1
```

Check DNS:

```bash
getent hosts http.kali.org
```

Inspect repository configuration:

```bash
cat /etc/apt/sources.list
```

Check system time:

```bash
timedatectl
```

After identifying and correcting the underlying problem, retry:

```bash
sudo apt update
```

Do not disable repository signature verification or add untrusted repositories to bypass errors.

## 5. Problem: Disk Space Is Full

Linux needs available storage to install packages, create files, and run applications.

Check filesystem usage:

```bash
df -h
```

Check which directories occupy space:

```bash
du -h --max-depth=1 ~
```

Check package cache usage:

```bash
du -sh /var/cache/apt/archives
```

You can safely remove downloaded package cache files with:

```bash
sudo apt clean
```

Never randomly delete system directories to free space.

Also remember that VMware snapshots and virtual disk files can consume additional Windows storage.

## 6. Problem: Kali Is Running Slowly

A virtual machine may become slow when it does not have sufficient resources.

Possible causes:

- Insufficient RAM.
- Too many running applications.
- Heavy background processes.
- Limited host storage.
- High CPU usage.
- Several virtual machines running simultaneously.

### Check memory

```bash
free -h
```

### Check CPU and running processes

```bash
top
```

Press `q` to exit.

### Check storage

```bash
df -h
```

You can also inspect VMware's virtual machine hardware settings.

For our 16 GB RAM laptop, avoid allocating most of the host's memory to Kali.

## 7. Problem: A Command Is Not Found

Linux may display:

```text
command not found
```

Possible reasons:

1. The command is not installed.
2. The command name was typed incorrectly.
3. Its executable directory is not included in `PATH`.
4. The command is unavailable in your current environment.

Check whether a command exists:

```bash
command -v tree
```

Search for a package:

```bash
apt search tree
```

Install it if necessary:

```bash
sudo apt install tree
```

Do not install random software simply because a command is missing. First identify what the command is supposed to do.

## 8. Problem: Permission Denied

Linux uses file permissions to control access.

Example:

```text
Permission denied
```

This means the current account does not have sufficient permission for the requested operation.

Inspect file permissions:

```bash
ls -l filename
```

Inspect your identity:

```bash
id
```

For a legitimate administrative operation, you may need `sudo`.

Example:

```bash
sudo apt update
```

Do not automatically use `sudo` to bypass every permission error. Understand why access was denied first.

## 9. Problem: Kali Does Not Boot

If Kali fails to start, investigate the following:

- Check whether the virtual disk is connected.
- Check whether the VM is booting from the correct device.
- Check whether the Kali ISO is still attached and selected first in the boot order.
- Check the VM's hardware configuration.
- Read any error message displayed by VMware.

If the installer appears again after installation, the ISO may still be connected.

Disconnect the ISO from the virtual CD/DVD device after installation and restart the VM.

Do not format the virtual disk or reinstall Kali until you understand the problem.

## 10. Understanding Linux Logs

Linux records system events in log files.

Logs help administrators investigate errors and system behavior.

### View recent system messages

```bash
sudo journalctl -n 50
```

### View errors from the current boot

```bash
sudo journalctl -b -p err
```

### View service status

```bash
systemctl status NetworkManager
```

This checks the NetworkManager service where it is installed and managed by systemd.

### Follow system logs

```bash
sudo journalctl -f
```

Press `Ctrl + C` to stop following the logs.

## 11. Useful Troubleshooting Commands

| Command | Purpose |
|---|---|
| `ip addr` | Checks network interfaces and addresses |
| `ip route` | Checks routing configuration |
| `ping` | Tests network reachability |
| `df -h` | Checks filesystem storage |
| `du -h` | Measures directory storage usage |
| `free -h` | Checks memory usage |
| `top` | Monitors running processes |
| `journalctl` | Examines system logs |
| `systemctl status` | Checks service status |
| `lsblk` | Displays storage devices |
| `uname -a` | Displays kernel and system information |

## 12. Practical Assignment

Complete these diagnostic exercises:

- [ ] Display your network interfaces.
- [ ] Inspect your routing table.
- [ ] Test network connectivity.
- [ ] Check DNS resolution.
- [ ] Check filesystem storage.
- [ ] Inspect home-directory storage usage.
- [ ] Check memory usage.
- [ ] Open `top` and inspect running processes.
- [ ] Display the last 50 system log entries.
- [ ] Display system errors from the current boot.
- [ ] Check NetworkManager service status.
- [ ] Identify where your Kali VM files are stored in Windows.

For this exercise, observe and diagnose. Do not deliberately break your system to create errors.

## 13. Knowledge Check

1. What is troubleshooting?
2. What are the major steps in troubleshooting?
3. What command displays Linux IP addresses?
4. What command checks available filesystem storage?
5. What does `free -h` display?
6. What is the purpose of `journalctl`?
7. What can cause an APT update failure?
8. What does permission denied mean?
9. Why should we avoid randomly deleting system files?
10. Why should we diagnose an error before reinstalling Kali?

## 14. Lesson Summary

In this lesson, we learned how to identify and diagnose common Kali Linux problems.

We covered network failures, APT errors, disk space issues, slow performance, missing commands, permission errors, boot failures, and system logs.

Troubleshooting is an essential skill for Linux administrators and cybersecurity professionals.

**Next Lesson:** Lesson 15 – Module 02 Assessment.