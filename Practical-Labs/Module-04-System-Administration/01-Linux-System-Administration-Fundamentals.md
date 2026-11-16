# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 01: LINUX SYSTEM ADMINISTRATION FUNDAMENTALS

### 1. Lab Overview

**Objective:** Understand the responsibilities of a Linux system administrator and learn how to inspect, manage, and maintain a Linux system.

Linux system administration involves managing the resources and services that allow a Linux computer to operate reliably.

A Linux system administrator is responsible for:

- Managing users and groups.
- Monitoring system resources.
- Managing running processes.
- Installing and updating software.
- Managing system services.
- Inspecting logs.
- Monitoring storage.
- Maintaining system security.
- Troubleshooting system problems.

These skills are important in cybersecurity, cloud computing, DevOps, server administration, and software engineering.

### 2. Prepare Your Lab Environment

Create your working directory:

```bash
mkdir -p ~/practical-labs/module04/lab01
cd ~/practical-labs/module04/lab01
```

Confirm your location:

```bash
pwd
```

Create a directory for reports:

```bash
mkdir reports
```

All practical exercises should be performed inside this dedicated lab environment unless a task specifically requires read-only inspection of system information.

### 3. Understanding the Linux System

A Linux system can be understood through several major components.

| Component | Responsibility |
|---|---|
| Kernel | Manages hardware and system resources |
| Shell | Provides a command interface |
| Filesystem | Organizes files and directories |
| Processes | Represent running programs |
| Users | Identify accounts accessing the system |
| Services | Provide background functionality |
| Package manager | Installs and manages software |

The kernel is the central component of the operating system.

Inspect your kernel:

```bash
uname -r
```

Display detailed system information:

```bash
uname -a
```

Identify your distribution:

```bash
cat /etc/os-release
```

Display your hostname:

```bash
hostname
```

### 4. Understanding Administrative Privileges

Linux distinguishes between ordinary users and privileged administrative accounts.

Check your current identity:

```bash
whoami
```

Display your user ID and group memberships:

```bash
id
```

Check whether sudo is available:

```bash
command -v sudo
```

Display your sudo permissions:

```bash
sudo -l
```

This last command may request your password and should be used only to inspect your own authorized account permissions.

**Important:** Do not use `sudo` automatically. Most administrative learning exercises can begin with ordinary user privileges.

### 5. Inspecting System Resources

System administrators regularly monitor CPU, memory, storage, and uptime.

**Check uptime:**

```bash
uptime
```

Display a readable uptime value:

```bash
uptime -p
```

**Check memory:**

```bash
free -h
```

**Check disk capacity:**

```bash
df -h
```

**Check directory size:**

```bash
du -sh ~/practical-labs
```

**Inspect CPU information:**

```bash
lscpu
```

These commands help administrators understand the current state of a machine.

### 6. Understanding Running Processes

A process is an instance of a running program.

Display running processes:

```bash
ps aux
```

Display processes belonging to your current user:

```bash
ps -u "$(whoami)"
```

Search for a particular process:

```bash
ps aux | grep bash
```

Display a live process monitor:

```bash
top
```

Exit `top` by pressing:

```text
q
```

Understanding:

- PID: Process ID.
- USER: Account running the process.
- %CPU: CPU usage.
- %MEM: Memory usage.
- COMMAND: Program associated with the process.

Do not terminate unfamiliar system processes during this lab.

### 7. Understanding System Services

Linux services are background programs that provide functionality such as networking, scheduling, logging, and remote access.

Many Linux distributions use `systemd` to manage services.

Check whether systemd is running:

```bash
ps -p 1 -o comm=
```

If the result is `systemd`, inspect system state:

```bash
systemctl is-system-running
```

List active services:

```bash
systemctl list-units --type=service --state=running
```

Inspect the status of a service:

```bash
systemctl status cron
```

Depending on your distribution, the service may instead be named `crond`, or the service may not be installed.

For this lab, use service commands for inspection only. Do not stop or disable services.

### 8. Understanding Package Management

Package managers help install, update, and remove software.

Kali Linux and Debian-based distributions commonly use APT.

Display APT information:

```bash
apt --version
```

Inspect configured repositories:

```bash
cat /etc/apt/sources.list
```

Additional repository configuration may exist in:

```bash
ls /etc/apt/sources.list.d/
```

Search for an available package:

```bash
apt search htop
```

Display package information:

```bash
apt show htop
```

**Important:** Searching for packages does not install them. During this introductory lab, do not install or remove packages.

### 9. Understanding System Logs

System logs record events and activities generated by the operating system and applications.

If systemd is available, inspect recent journal entries:

```bash
journalctl -n 20
```

Display recent messages:

```bash
journalctl -p warning -n 20
```

Display messages from the current boot:

```bash
journalctl -b -n 30
```

Some distributions also maintain traditional log files inside:

```bash
/var/log/
```

Inspect available log filenames:

```bash
ls -lah /var/log/
```

Do not modify or delete system logs.

### 10. Creating a System Information Report

Now combine your administrative knowledge into a simple report.

Create:

```bash
nano system-information.sh
```

Enter:

```bash
#!/bin/bash

set -u

report="reports/system-information.txt"

{
    echo "========== SYSTEM INFORMATION =========="

    echo ""
    echo "Date:"
    date

    echo ""
    echo "Hostname:"
    hostname

    echo ""
    echo "Current User:"
    whoami

    echo ""
    echo "Kernel:"
    uname -r

    echo ""
    echo "Distribution:"
    cat /etc/os-release

    echo ""
    echo "Uptime:"
    uptime

    echo ""
    echo "Memory:"
    free -h

    echo ""
    echo "Disk Usage:"
    df -h

    echo ""
    echo "========================================"
} > "$report"

echo "System report created: $report"
```

Validate the script:

```bash
bash -n system-information.sh
```

Execute:

```bash
bash system-information.sh
```

Inspect the report:

```bash
cat reports/system-information.txt
```

### 11. Practical Exercise 1 – System Identification

Create:

`system-identification.sh`

Requirements:

1. Display the Linux distribution.
2. Display the kernel version.
3. Display the hostname.
4. Display the current username.
5. Display the architecture.
6. Save the results in `reports/system-identification.txt`.

Useful commands:

```bash
cat /etc/os-release
uname -r
hostname
whoami
uname -m
```

### 12. Practical Exercise 2 – Resource Monitoring

Create:

`resource-monitor.sh`

Requirements:

Display:

- CPU information.
- Memory usage.
- Disk usage.
- System uptime.
- Size of your practical-labs directory.

Save the output to:

`reports/resource-monitor.txt`

### 13. Practical Exercise 3 – Process Inspection

Create:

`process-inspector.sh`

Requirements:

1. Display processes belonging to your current user.
2. Display their PIDs.
3. Display the command names.
4. Save the results in `reports/process-report.txt`.

Use:

```bash
ps
```

and appropriate output formatting.

### 14. Practical Exercise 4 – Service Inspection

Create:

`service-inspector.sh`

Requirements:

1. Determine whether systemd is running.
2. If available, display the status of the cron service.
3. If unavailable, display an appropriate message.
4. Do not start, stop, or disable any service.

### 15. Practical Exercise 5 – Administrative Dashboard

Create:

`admin-dashboard.sh`

Requirements:

Build a script that displays:

1. System identity.
2. Kernel information.
3. Current user.
4. CPU information.
5. Memory usage.
6. Disk usage.
7. Uptime.
8. Process information.

Save the complete output inside:

`reports/admin-dashboard.txt`

Add basic error handling and make the script readable.

### 16. Script Validation

Validate all scripts:

```bash
for file in *.sh
do
    echo "Checking $file"
    bash -n "$file"
done
```

Inspect the lab directory:

```bash
find . -maxdepth 3 -type f
```

Inspect generated reports:

```bash
ls -lh reports/
```

### 17. Knowledge Check

1. What is Linux system administration?
2. What is the role of the Linux kernel?
3. What is the difference between a user and a privileged administrator?
4. What does `id` display?
5. What does `free -h` display?
6. What is a Linux process?
7. What is a PID?
8. What is the purpose of `systemctl`?
9. What is a package manager?
10. What information can system logs provide?
11. What is the purpose of `journalctl`?
12. Why should administrators avoid terminating unfamiliar processes?

### 18. Lab Completion Checklist

- [ ] Created the Lab 01 directory.
- [ ] Identified the Linux distribution.
- [ ] Inspected kernel information.
- [ ] Inspected administrative privileges.
- [ ] Checked CPU and memory.
- [ ] Checked disk usage.
- [ ] Inspected running processes.
- [ ] Inspected system services.
- [ ] Explored package management.
- [ ] Inspected system logs.
- [ ] Generated a system information report.
- [ ] Completed all five practical exercises.
- [ ] Validated Bash scripts.

### 19. Expected Outcome

After completing this lab, you should understand:

- The responsibilities of a Linux system administrator.
- Basic Linux architecture.
- User privileges.
- System resource monitoring.
- Process inspection.
- Service management fundamentals.
- Package management.
- Linux logging.
- Basic administrative automation.

**Next Lab:** User and Group Administration.