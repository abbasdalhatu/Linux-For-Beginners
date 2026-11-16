
# BASH SYSTEM ADMINISTRATION

## 1. Introduction

Bash can help Linux administrators automate common system management tasks.

Examples include:
- Checking system information.
- Monitoring memory and disk usage.
- Inspecting running processes.
- Checking system uptime.
- Monitoring network connectivity.

## 2. Checking System Information

Display the Linux kernel and system information:

```bash
uname -a
```

Display the Linux distribution information:

```bash
cat /etc/os-release
```

Display the hostname:

```bash
hostname
```

Display system uptime:

```bash
uptime
```

## 3. Monitoring Memory Usage

Display memory information:

```bash
free -h
```

The output includes:
- Total memory.
- Used memory.
- Available memory.
- Swap memory.

## 4. Monitoring Disk Usage

Display available disk space:

```bash
df -h
```

Display the size of a directory:

```bash
du -sh /home
```

## 5. Monitoring Running Processes

Display running processes:

```bash
ps aux
```

Display processes interactively:

```bash
top
```

Find a process by name:

```bash
pgrep -a bash
```

## 6. Checking Network Connectivity

Test connectivity to a server:

```bash
ping -c 4 example.com
```

Display network interfaces:

```bash
ip addr
```

Display routing information:

```bash
ip route
```

## 7. Checking Logged-in Users

Display currently logged-in users:

```bash
who
```

Display detailed login information:

```bash
w
```

Display the current username:

```bash
whoami
```

## 8. Checking System Services

List active services:

```bash
systemctl list-units --type=service --state=running
```

Check a specific service:

```bash
systemctl status ssh
```

Note: Service names and availability depend on the Linux distribution.

## 9. Creating a System Information Script

Create a script named `system-info.sh`.

```bash
#!/bin/bash

echo "===== SYSTEM INFORMATION ====="

echo "Hostname:"
hostname

echo
echo "Kernel:"
uname -r

echo
echo "Uptime:"
uptime

echo
echo "Memory Usage:"
free -h

echo
echo "Disk Usage:"
df -h

echo
echo "Current User:"
whoami

echo "===== REPORT COMPLETE ====="
```

Make it executable:

```bash
chmod +x system-info.sh
```

Run it:

```bash
./system-info.sh
```

## 10. Creating a Basic Disk Monitoring Script

```bash
#!/bin/bash

echo "===== DISK SPACE REPORT ====="

df -h

echo
echo "===== END OF REPORT ====="
```

This script displays disk usage information.

## 11. Administrative Safety

- Understand commands before running them.
- Avoid unnecessary root privileges.
- Do not terminate unknown system processes.
- Back up important configuration files.
- Review service changes before applying them.
- Test scripts in a safe environment.

## 12. Summary

Bash is useful for automating Linux system administration.

Important commands:

| Command | Purpose |
|---|---|
| uname | System and kernel information |
| hostname | Display hostname |
| uptime | System uptime |
| free | Memory usage |
| df | Disk space |
| du | Directory size |
| ps | Process information |
| top | Interactive process monitoring |
| ping | Connectivity testing |
| systemctl | Service management |

Bash scripts can combine these commands into reusable administrative tools.
