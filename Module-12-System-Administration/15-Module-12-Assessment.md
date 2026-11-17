---
---


# Module 12 – Lesson 15: Linux System Administration Assessment

## 1. Assessment Overview

This assessment evaluates your understanding of Linux System Administration.

It covers all 14 lessons in Module 12.

### Assessment Objectives

You should demonstrate the ability to:

1. Explain Linux system administration.
2. Inspect and manage Linux services.
3. Monitor system resources.
4. Investigate system logs.
5. Understand scheduled tasks.
6. Inspect disks and filesystems.
7. Explain swap memory.
8. Manage and inspect processes.
9. Perform basic system maintenance.
10. Create and verify backups.
11. Analyse system performance.
12. Understand SSH administration.
13. Perform practical administrative tasks.
14. Produce professional administration reports.

### Assessment Structure

| Section | Assessment | Marks |
|---|---|---:|
| A | Multiple-choice questions | 20 |
| B | Short-answer questions | 20 |
| C | Practical Linux commands | 30 |
| D | Troubleshooting scenarios | 20 |
| E | Practical administration report | 10 |
| **Total** | | **100** |

---

# SECTION A: MULTIPLE-CHOICE QUESTIONS

**Instructions:** Select the correct answer.

### 1. What is Linux system administration?

A. Designing websites  
B. Managing and maintaining Linux systems  
C. Creating animations  
D. Writing HTML only

### 2. Which command displays system uptime?

A. `whoami`  
B. `uptime`  
C. `pwd`  
D. `touch`

### 3. Which command manages systemd services?

A. `systemctl`  
B. `mkdir`  
C. `grep`  
D. `nano`

### 4. Which command displays memory usage?

A. `df -h`  
B. `free -h`  
C. `lsblk`  
D. `hostname`

### 5. Which command displays system logs?

A. `journalctl`  
B. `chmod`  
C. `cp`  
D. `rmdir`

### 6. Which command edits the current user's cron jobs?

A. `crontab -e`  
B. `systemctl edit`  
C. `nano /etc/passwd`  
D. `ip route`

### 7. Which command displays block devices?

A. `lsblk`  
B. `ps aux`  
C. `who`  
D. `date`

### 8. What is swap memory?

A. A network protocol  
B. Disk space used as virtual memory  
C. A Linux user account  
D. A system log

### 9. What does PID mean?

A. Process Identification  
B. Program Installation Directory  
C. Primary Internet Device  
D. Package Information Database

### 10. Which command displays running processes?

A. `ps aux`  
B. `df -h`  
C. `ip addr`  
D. `tar`

### 11. Which package manager is commonly used in Kali Linux?

A. APT  
B. Chocolatey  
C. Homebrew  
D. Pacman

### 12. Which command displays filesystem disk usage?

A. `df -h`  
B. `free -h`  
C. `uname -r`  
D. `whoami`

### 13. Which command creates a compressed tar archive?

A. `tar -czf`  
B. `chmod -R`  
C. `ip link`  
D. `killall`

### 14. What does SSH stand for?

A. Secure Shell  
B. System Security Hardware  
C. Secure Storage Host  
D. Server Shell Handler

### 15. Which TCP port is the default SSH port?

A. 21  
B. 22  
C. 53  
D. 443

### 16. Which SSH key type is a modern elliptic-curve option?

A. Ed25519  
B. MD5  
C. FTP  
D. HTTP

### 17. What is the purpose of a backup?

A. To delete unnecessary files  
B. To preserve data for recovery  
C. To disable services  
D. To increase CPU speed

### 18. Which command displays listening TCP and UDP sockets?

A. `ss -tuln`  
B. `free -h`  
C. `ls -la`  
D. `pwd`

### 19. What is the Principle of Least Privilege?

A. Give every user root access  
B. Give users only the permissions they need  
C. Disable all user accounts  
D. Allow unrestricted access

### 20. Why should administrators monitor system performance?

A. To identify resource bottlenecks  
B. To remove all processes  
C. To disable networking  
D. To prevent users from logging in

---

# SECTION B: SHORT-ANSWER QUESTIONS

Answer each question in your own words.

1. Explain five responsibilities of a Linux system administrator.
2. What is the difference between a process and a service?
3. Explain the difference between `systemctl start` and `systemctl enable`.
4. What are system logs, and why are they important?
5. Explain how cron scheduling works.
6. Differentiate between a disk partition and a filesystem.
7. Explain the difference between RAM and swap memory.
8. Describe three Linux process signals.
9. What is the purpose of system maintenance?
10. Explain the 3-2-1 backup strategy.
11. What is a performance bottleneck?
12. Explain how SSH public-key authentication works.
13. Differentiate between SCP and SFTP.
14. What is a checksum, and how does it help verify backups?
15. Explain why Linux administrators should follow the Principle of Least Privilege.

---

# SECTION C: PRACTICAL LINUX COMMANDS

**Instructions:** Perform these exercises inside your Kali Linux virtual machine.

Use your existing workspace:

```bash
mkdir -p ~/module12-lab/assessment
cd ~/module12-lab/assessment
```

Do not perform destructive operations on system disks, partitions, services, or user accounts.

## Task 1: System Information

Display:

- Your username.
- Your hostname.
- Linux kernel version.
- System uptime.

Commands to investigate:

```bash
whoami
hostname
uname -r
uptime
```

## Task 2: Resource Monitoring

Display:

- CPU information.
- RAM usage.
- Filesystem usage.
- Running processes.

Useful commands:

```bash
lscpu
free -h
df -h
ps aux
```

## Task 3: Service Inspection

Display:

- The status of the cron service.
- Failed system services.
- Available service units.

Useful commands:

```bash
systemctl status cron --no-pager
systemctl --failed --no-pager
systemctl list-units --type=service --no-pager
```

## Task 4: Log Investigation

Display the last 20 system log entries.

```bash
journalctl -n 20 --no-pager
```

Display error-priority entries from the current boot:

```bash
journalctl -p err -b --no-pager
```

## Task 5: Storage Inspection

Display:

```bash
lsblk
df -h
df -Th
findmnt
```

Identify the filesystem types and available storage.

## Task 6: Process Inspection

Display the top CPU-consuming processes:

```bash
ps aux --sort=-%cpu | head -n 10
```

Display the top memory-consuming processes:

```bash
ps aux --sort=-%mem | head -n 10
```

## Task 7: Backup Verification

Create a sample file:

```bash
echo "Module 12 assessment backup" > assessment-notes.txt
```

Create an archive:

```bash
tar -czf assessment-backup.tar.gz assessment-notes.txt
```

Generate its SHA-256 checksum:

```bash
sha256sum assessment-backup.tar.gz
```

Verify the archive:

```bash
tar -tzf assessment-backup.tar.gz
```

## Task 8: Network Inspection

Run:

```bash
ip addr
ip route
ss -tuln
```

Identify your network interface, routing information, and listening sockets.

## Task 9: SSH Inspection

Check whether the SSH client is available:

```bash
ssh -V
```

Inspect your SSH configuration directory:

```bash
ls -la ~/.ssh
```

Do not expose or share private keys.

## Task 10: Create an Assessment Report

Save your command outputs in:

```text
~/module12-lab/assessment/final-assessment-report.txt
```

Your report should contain:

1. System information.
2. CPU and memory information.
3. Storage information.
4. Service status.
5. Log investigation results.
6. Process information.
7. Network information.
8. Backup verification results.
9. SSH inspection results.

---

# SECTION D: TROUBLESHOOTING SCENARIOS

Read each scenario and explain how you would investigate it.

### Scenario 1: High CPU Usage

A Linux server is responding very slowly. The administrator suspects that a process is consuming too much CPU.

Questions:

1. Which commands would you use?
2. How would you identify the process?
3. What steps would you take before terminating it?

### Scenario 2: Full Disk

A server reports that its root filesystem is 95% full.

Questions:

1. Which command displays filesystem usage?
2. How would you identify directories consuming large amounts of space?
3. Why should you investigate before deleting files?

### Scenario 3: Failed Service

A web service is not responding.

Questions:

1. Which command checks the service status?
2. Which command examines its logs?
3. What information would help identify the cause?

### Scenario 4: Failed Backup

An administrator discovers that yesterday's backup archive cannot be opened.

Questions:

1. How would you inspect the archive?
2. How would you verify its checksum?
3. Why should backup restoration be tested regularly?

### Scenario 5: Suspicious Network Port

You discover an unfamiliar listening port on a Linux server.

Questions:

1. Which command identifies listening sockets?
2. How would you identify the process associated with the port?
3. What should you investigate before making changes?

---

# SECTION E: PRACTICAL ADMINISTRATION REPORT

## Final Assignment

You have been assigned as the Linux administrator of a small university department.

Your supervisor requests a basic system health report.

Prepare a report containing:

### 1. System Identity

- Hostname.
- Operating system.
- Kernel version.
- Uptime.

### 2. Resource Status

- CPU information.
- Memory usage.
- Disk usage.
- Running processes.

### 3. Service Status

- Active services.
- Failed services.
- Important service observations.

### 4. Security Observations

- User account information.
- Administrative group membership.
- SSH client availability.
- Listening network ports.

### 5. Backup Status

- Backup archive filename.
- Archive contents.
- SHA-256 checksum.
- Verification result.

### 6. Recommendations

Write at least three recommendations based on your actual observations.

Do not invent problems. If everything appears normal, explain what should continue to be monitored.

---

# 2. Assessment Grading Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Requires further revision |

**Pass mark: 50/100.**

---

# 3. Final Module 12 Checklist

- [ ] Lesson 01 – Introduction to Linux System Administration
- [ ] Lesson 02 – Managing Linux Services with systemd
- [ ] Lesson 03 – System Monitoring and Resource Management
- [ ] Lesson 04 – Managing System Logs
- [ ] Lesson 05 – Task Scheduling with cron and at
- [ ] Lesson 06 – Managing Storage and Disks
- [ ] Lesson 07 – Filesystems and Mounting
- [ ] Lesson 08 – Swap Memory Management
- [ ] Lesson 09 – Managing System Processes and Services
- [ ] Lesson 10 – Linux System Maintenance
- [ ] Lesson 11 – Backup and Recovery Administration
- [ ] Lesson 12 – System Performance Monitoring
- [ ] Lesson 13 – Remote System Administration with SSH
- [ ] Lesson 14 – Practical System Administration Projects
- [ ] Lesson 15 – Module 12 Assessment

---

# 4. Final Reflection

Linux system administration is one of the foundations of professional Linux and cybersecurity work.

A capable administrator should understand how to monitor systems, manage services, investigate problems, protect data, maintain access controls, and document findings.

These skills are useful in:

- Linux server administration.
- Cloud computing.
- DevOps engineering.
- Cybersecurity operations.
- Security monitoring.
- Infrastructure engineering.
- Penetration testing lab management.

**Congratulations on reaching the end of Module 12!**
