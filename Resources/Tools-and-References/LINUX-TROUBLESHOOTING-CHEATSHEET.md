
# LINUX TROUBLESHOOTING CHEATSHEET

## 1. Introduction

Linux troubleshooting is the process of identifying and resolving problems affecting a Linux computer.

This reference contains useful commands for diagnosing common system issues.

## 2. System Information

| Command | Purpose |
|---|---|
| uname -a | Display system and kernel information |
| hostname | Display computer name |
| uptime | Show how long the system has been running |
| whoami | Display current username |
| date | Display current date and time |
| cat /etc/os-release | Display Linux distribution information |

## 3. Disk Troubleshooting

Check available disk space:

```bash
df -h
```

Check directory size:

```bash
du -sh /home
```

Display storage devices:

```bash
lsblk
```

Check mounted filesystems:

```bash
mount
```

## 4. Memory Troubleshooting

Display RAM and swap usage:

```bash
free -h
```

Monitor system resources:

```bash
top
```

## 5. Process Troubleshooting

Display running processes:

```bash
ps aux
```

Search for a process:

```bash
pgrep -a process_name
```

Display process activity interactively:

```bash
top
```

Terminate a process only after identifying it and confirming that it is safe to stop:

```bash
kill PID
```

Replace PID with the actual process ID.

## 6. Network Troubleshooting

Display network interfaces:

```bash
ip addr
```

Display routing information:

```bash
ip route
```

Check DNS resolution:

```bash
nslookup example.com
```

Test network connectivity:

```bash
ping -c 4 example.com
```

Display listening ports:

```bash
ss -tuln
```

## 7. Service Troubleshooting

Check a service:

```bash
systemctl status service_name
```

Start a service:

```bash
sudo systemctl start service_name
```

Stop a service:

```bash
sudo systemctl stop service_name
```

Restart a service:

```bash
sudo systemctl restart service_name
```

Use the correct service name for your Linux distribution.

## 8. Viewing System Logs

Display recent system logs:

```bash
journalctl -n 50
```

Display errors from the current boot:

```bash
journalctl -b -p err
```

Follow logs in real time:

```bash
journalctl -f
```

## 9. File Permission Troubleshooting

Display file permissions:

```bash
ls -l filename
```

Change file permissions:

```bash
chmod 644 filename
```

Change file ownership:

```bash
sudo chown username:groupname filename
```

Only change ownership or permissions when you understand the consequences.

## 10. Package Management

### Debian and Ubuntu

Update package information:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt upgrade
```

Find a package:

```bash
apt search package_name
```

### Fedora

Update packages:

```bash
sudo dnf upgrade
```

## 11. Basic Troubleshooting Procedure

1. Identify the problem.
2. Observe the error message.
3. Check relevant system information.
4. Inspect logs where appropriate.
5. Identify the likely cause.
6. Apply a controlled fix.
7. Test whether the problem is resolved.
8. Document the solution.

## 12. Important Safety Notes

- Read commands before executing them.
- Avoid deleting unknown system files.
- Do not copy commands from untrusted sources without understanding them.
- Back up important data before major changes.
- Use administrator privileges only when necessary.
- Record changes made during troubleshooting.

## 13. Summary

Linux troubleshooting depends on understanding system information, storage, memory, processes, networking, services, logs, and permissions.

The commands in this reference provide a starting point for diagnosing common Linux problems.
