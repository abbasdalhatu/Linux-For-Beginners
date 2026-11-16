
# LINUX SYSTEM ADMINISTRATION — PRACTICAL EXERCISES

Welcome to the Linux System Administration exercises.

These activities help learners understand how to manage, monitor, and maintain Linux systems.

**Difficulty:** Beginner to Advanced

IMPORTANT: Perform administrative activities inside your own Linux environment or an authorized virtual machine.

## Exercise 1: System Information

Run the following commands:

```bash
uname -a
hostname
hostnamectl
uptime
cat /etc/os-release
```

Tasks:

1. Identify your Linux distribution.
2. Find your kernel version.
3. Display your hostname.
4. Check system uptime.
5. Explain why system information is important.

## Exercise 2: User Management

Practise these commands in a test Linux virtual machine:

```bash
whoami
id
who
groups
```

Tasks:

1. Identify the current user.
2. Display the user's ID.
3. Identify the user's groups.
4. Explain the difference between a normal user and root.

Challenge:

Research how administrators create, modify, and remove user accounts safely.

## Exercise 3: Group Management

Answer the following:

1. What is a Linux group?
2. Why are groups useful?
3. What is the purpose of the `/etc/group` file?
4. What is the difference between a primary and supplementary group?

Practise:

```bash
getent group
groups
id
```

## Exercise 4: File Permissions

Create a test file:

```bash
touch admin-notes.txt
ls -l admin-notes.txt
```

Change its permissions:

```bash
chmod 644 admin-notes.txt
ls -l admin-notes.txt
```

Tasks:

1. Explain read, write, and execute permissions.
2. Explain what permission 644 means.
3. Explain what permission 755 means.
4. Identify the difference between file permissions and ownership.

## Exercise 5: Process Management

Run:

```bash
ps aux
top
pgrep -a bash
```

Tasks:

1. Display running processes.
2. Identify a process ID.
3. Explain what a PID is.
4. Explain why process monitoring matters.
5. Describe when an administrator might need to stop a process.

## Exercise 6: Service Management

Use a Linux virtual machine with systemd.

Run:

```bash
systemctl list-units --type=service
systemctl --failed
```

Tasks:

1. Display available service information.
2. Identify failed services, if any.
3. Explain what systemd does.
4. Explain why services are important.

Challenge:

Research how to inspect the status of a service without changing its configuration.

## Exercise 7: Storage Management

Run:

```bash
df -h
du -sh /home
lsblk
```

Tasks:

1. Display available disk space.
2. Identify storage devices.
3. Check the size of a directory.
4. Explain the difference between a disk and a partition.
5. Explain why administrators monitor disk usage.

## Exercise 8: System Logs

Run:

```bash
journalctl -n 20
journalctl -b
journalctl -p err
```

Tasks:

1. Display recent system logs.
2. Display logs from the current boot.
3. Identify error messages, if present.
4. Explain why logs are important for troubleshooting.

## Exercise 9: Network Administration

Run:

```bash
ip addr
ip route
ss -tuln
```

Tasks:

1. Identify active network interfaces.
2. Display routing information.
3. Identify listening ports.
4. Explain why administrators monitor network connections.

## Exercise 10: System Monitoring Challenge

Create a basic system monitoring report.

Your report should include:

- Hostname.
- Current username.
- Kernel version.
- System uptime.
- Memory usage.
- Disk usage.
- Network interfaces.

Use suitable Linux commands and save the output in a text file.

## Exercise 11: Troubleshooting Scenarios

### Scenario A: High Disk Usage

A Linux computer reports that its root filesystem is almost full.

Questions:

1. Which command checks available disk space?
2. Which command helps identify large directories?
3. What could cause excessive disk usage?
4. What precautions should be taken before deleting files?

### Scenario B: Failed Service

A system service is not running.

Questions:

1. Which command checks its status?
2. Where can you inspect service logs?
3. What information should you collect before attempting a fix?

### Scenario C: High CPU Usage

A computer becomes unusually slow.

Questions:

1. Which commands can identify resource-intensive processes?
2. What information should you examine?
3. Why should unknown processes not be terminated immediately?

## Exercise 12: Final Administration Challenge

Prepare a basic Linux administration report for your own test virtual machine.

Include:

1. System information.
2. User and group information.
3. File permission examples.
4. Running process information.
5. Service status.
6. Storage information.
7. System log observations.
8. Network configuration.

Write a short conclusion explaining the overall system condition and any issues discovered.

Do not expose private network details, passwords, or sensitive system information in public reports.

## Knowledge Assessment

1. What is Linux system administration?
2. What is the root account?
3. What is a Linux group?
4. What is a process ID?
5. What is systemd?
6. What is a system service?
7. What is the purpose of journalctl?
8. What does df -h display?
9. Why are file permissions important?
10. Why is regular system monitoring necessary?

## Completion Checklist

- [ ] Understand Linux system information.
- [ ] Understand user management.
- [ ] Understand groups.
- [ ] Manage basic file permissions.
- [ ] Inspect running processes.
- [ ] Understand system services.
- [ ] Monitor storage.
- [ ] Inspect system logs.
- [ ] Understand network administration.
- [ ] Complete troubleshooting scenarios.
- [ ] Complete the final administration report.

**Learning objective:** Develop practical Linux system administration and troubleshooting skills.
