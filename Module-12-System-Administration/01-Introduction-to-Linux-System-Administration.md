---
---


# Module 12 – Lesson 01: Introduction to Linux System Administration

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what Linux system administration means.
2. Understand the responsibilities of a Linux system administrator.
3. Identify important Linux administration tools.
4. Understand the difference between a normal user and the root user.
5. Inspect basic system information.
6. Perform basic administrative checks safely.

---

## 2. What Is Linux System Administration?

Linux system administration is the process of managing, maintaining, monitoring, and securing Linux computers and servers.

A person who performs these tasks is called a **Linux System Administrator (SysAdmin)**.

Think of a Linux computer as a building.

- The operating system is the building itself.
- Users are the people who enter the building.
- Permissions determine who can access different rooms.
- Services are the facilities running inside the building.
- Logs are records of activities.
- The system administrator manages everything to keep the building operational and secure.

### Real-world example

A university may use Linux servers to:

- Host its website.
- Store student records.
- Run its Learning Management System (LMS).
- Manage databases.
- Provide email services.
- Support network infrastructure.

A system administrator ensures these systems remain available, secure, and properly maintained.

---

## 3. Responsibilities of a Linux System Administrator

A Linux administrator performs several important tasks.

### A. User Management

Creating, modifying, and deleting user accounts.

Examples:

```bash
whoami
who
id
```

### B. Permission Management

Controlling who can read, write, or execute files.

Examples:

```bash
ls -l
chmod
chown
```

### C. Service Management

Starting, stopping, restarting, and checking system services.

Examples:

```bash
systemctl status ssh
systemctl list-units --type=service
```

### D. System Monitoring

Checking CPU, memory, disk usage, and running processes.

Examples:

```bash
top
free -h
df -h
uptime
```

### E. Package Management

Installing, updating, and removing software.

Examples:

```bash
apt update
apt list --installed
```

### F. Security Administration

Protecting Linux systems through:

- Strong authentication.
- Proper file permissions.
- Firewall configuration.
- Security updates.
- Log monitoring.
- SSH security.

### G. Backup and Recovery

Creating backups and restoring important files when necessary.

Example:

```bash
tar -czf backup.tar.gz important-folder/
```

---

## 4. Understanding Linux Users and Root Access

Linux uses different account privileges to control access to the system.

### Normal User

A normal user has limited permissions.

For example:

```bash
whoami
```

This displays the current username.

### Root User

The root user is the superuser in Linux.

Root has extensive administrative privileges and can modify almost every part of the operating system.

Check your current identity:

```bash
id
```

Check whether you can use sudo:

```bash
sudo -v
```

If authentication succeeds, your account can use sudo for permitted administrative operations.

### Understanding sudo

`sudo` means "superuser do."

It allows an authorised user to execute a command with elevated privileges.

Example:

```bash
sudo apt update
```

**Important security principle:** Do not use root privileges unnecessarily. A mistake made with administrative privileges can damage the operating system.

For this course, perform practical exercises in your own lab directory.

---

## 5. Important Linux Administration Commands

| Command | Purpose |
|---|---|
| `whoami` | Displays current username |
| `id` | Displays user and group information |
| `hostname` | Displays computer hostname |
| `uname -a` | Displays kernel and system information |
| `uptime` | Shows how long the system has been running |
| `date` | Displays current date and time |
| `free -h` | Displays memory usage |
| `df -h` | Displays filesystem disk usage |
| `lsblk` | Lists storage devices |
| `ps aux` | Lists running processes |
| `top` | Monitors processes and resources |
| `systemctl` | Manages systemd services |
| `journalctl` | Examines system logs |

These commands are useful for everyday administration and troubleshooting.

---

## 6. Inspecting Your Linux System

A system administrator should understand the computer before making changes.

Run these commands one at a time.

### Step 1: Identify your username

```bash
whoami
```

### Step 2: Identify your hostname

```bash
hostname
```

### Step 3: Display system information

```bash
uname -a
```

### Step 4: Check system uptime

```bash
uptime
```

### Step 5: Check memory

```bash
free -h
```

### Step 6: Check disk usage

```bash
df -h
```

### Step 7: List storage devices

```bash
lsblk
```

### Step 8: View running processes

```bash
ps aux
```

You have now performed a basic Linux system inspection.

---

## 7. Understanding the Linux System Administration Workflow

Professional administrators generally follow a structured process.

1. **Inspect:** Understand the current system condition.
2. **Identify:** Find the problem or administrative requirement.
3. **Plan:** Decide what action is appropriate.
4. **Execute:** Apply the change carefully.
5. **Verify:** Confirm that the action worked.
6. **Document:** Record what was changed and why.

### Example: Checking disk space

Suppose a Linux server is running out of storage.

First, inspect:

```bash
df -h
```

Then identify large directories:

```bash
du -sh ~/* 2>/dev/null
```

Investigate the results before deleting anything.

Never delete files simply because they appear large. Determine what they are used for first.

---

## 8. Practical Lab: Linux System Inspection Report

### Objective

Create a small system administration report using information from your Kali Linux virtual machine.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson01
cd ~/module12-lab/lesson01
```

### Step 2: Create a report file

```bash
touch system-report.txt
```

### Step 3: Record your system information

Run:

```bash
{
echo "LINUX SYSTEM ADMINISTRATION REPORT"
echo "================================="
echo ""
echo "Username:"
whoami
echo ""
echo "Hostname:"
hostname
echo ""
echo "Kernel Information:"
uname -a
echo ""
echo "System Uptime:"
uptime
echo ""
echo "Memory Information:"
free -h
echo ""
echo "Disk Usage:"
df -h
echo ""
echo "Storage Devices:"
lsblk
} > system-report.txt
```

### Step 4: Read your report

```bash
cat system-report.txt
```

### Step 5: Check the file

```bash
ls -lh system-report.txt
```

You should see your generated report.

### Step 6: Verify its contents

```bash
wc -l system-report.txt
```

This displays the number of lines in the report.

**Expected result:** A text file containing basic information about your Kali Linux system.

---

## 9. Cybersecurity Connection

System administration is an important foundation for cybersecurity.

A security professional needs to understand how a Linux system works before properly securing or investigating it.

For example:

- User administration helps identify unauthorised accounts.
- Permission management helps prevent unauthorised file access.
- Process monitoring helps identify suspicious processes.
- Log monitoring helps investigate unusual activity.
- Service management helps identify unnecessary network services.
- System updates help reduce exposure to known vulnerabilities.

**Remember:** You cannot effectively secure a system you do not understand.

---

## 10. Lesson Summary

In this lesson, you learned:

1. Linux system administration involves managing and maintaining Linux systems.
2. A system administrator manages users, permissions, services, storage, and security.
3. Root is the most privileged Linux account.
4. Sudo provides authorised administrative access.
5. Commands such as `free`, `df`, `uptime`, and `lsblk` help inspect a system.
6. A good administrator inspects, plans, executes, verifies, and documents changes.

---

## 11. Knowledge Check

Answer these questions in your own words.

1. What is Linux system administration?
2. Who is a Linux system administrator?
3. What is the difference between a normal user and the root user?
4. What does the `sudo` command do?
5. Which command displays memory usage?
6. Which command displays disk usage?
7. What is the purpose of `hostname`?
8. Why should administrators avoid unnecessary root privileges?
9. Name four responsibilities of a Linux system administrator.
10. Why is system administration important in cybersecurity?

---

## 12. Practical Completion Checklist

- [ ] I understand Linux system administration.
- [ ] I can explain the responsibilities of a system administrator.
- [ ] I understand normal users, root, and sudo.
- [ ] I executed the system inspection commands.
- [ ] I created `system-report.txt`.
- [ ] I viewed and verified my report.
- [ ] I understand the connection between system administration and cybersecurity.

**Lesson 01 is complete when you understand the concepts and successfully generate your system report.**
