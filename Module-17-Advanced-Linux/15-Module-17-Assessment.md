---
---


# MODULE 17 – ADVANCED LINUX
## LESSON 15: COMPREHENSIVE MODULE ASSESSMENT

---

## 1. Introduction

Welcome to the final assessment of Module 17: Advanced Linux.

This assessment evaluates your understanding of advanced Linux administration, automation, scripting, networking, performance monitoring, security, virtualization, logging, and practical projects.

### Assessment objectives

By completing this assessment, you should demonstrate the ability to:

1. Explain advanced Linux concepts.
2. Administer Linux systems.
3. Write and execute Bash scripts.
4. Monitor system performance.
5. Inspect Linux networking.
6. Understand Linux security mechanisms.
7. Investigate system logs.
8. Automate administrative tasks.
9. Build practical Linux tools.
10. Document technical work professionally.

---

# PART A: THEORETICAL ASSESSMENT

## Section 1: Advanced Linux Fundamentals

**Question 1:** Explain the difference between user space and kernel space.

**Question 2:** What is a system call?

**Question 3:** Explain the Linux boot process from firmware initialization to the login prompt.

**Question 4:** What is the purpose of the `/proc` and `/sys` filesystems?

**Question 5:** Explain the relationship between the Linux kernel, shell, and user applications.

## Section 2: Advanced Shell Operations

**Question 6:** Explain the difference between:

- Standard input.
- Standard output.
- Standard error.

**Question 7:** What is the purpose of a pipeline in Linux?

**Question 8:** Explain the difference between `>` and `>>`.

**Question 9:** What is command substitution?

**Question 10:** What does an exit status of `0` generally indicate?

## Section 3: Bash Scripting and Automation

**Question 11:** What is a Bash script?

**Question 12:** Explain the purpose of the following:

- Variables.
- Functions.
- Conditional statements.
- Loops.
- Command-line arguments.

**Question 13:** What is the purpose of `chmod +x`?

**Question 14:** Explain the difference between cron and systemd timers.

**Question 15:** What is error handling, and why is it important in Bash scripting?

## Section 4: Process and Service Management

**Question 16:** What is a Linux process?

**Question 17:** Explain the difference between a process and a service.

**Question 18:** What is a zombie process?

**Question 19:** What is the purpose of `systemctl`?

**Question 20:** Explain the difference between starting and enabling a systemd service.

## Section 5: Storage and Kernel Management

**Question 21:** What is a filesystem?

**Question 22:** Explain the difference between a partition and a filesystem.

**Question 23:** What is LVM?

**Question 24:** What is swap memory?

**Question 25:** What is a kernel module?

## Section 6: Advanced Networking

**Question 26:** What is the purpose of a routing table?

**Question 27:** Explain the difference between TCP and UDP.

**Question 28:** What is DNS?

**Question 29:** What is the purpose of the `ss` command?

**Question 30:** Explain the difference between an IP address and a MAC address.

## Section 7: Performance Monitoring

**Question 31:** What is system load average?

**Question 32:** Explain the difference between RAM and swap.

**Question 33:** What does `df -h` display?

**Question 34:** What is the purpose of `top`?

**Question 35:** Why should administrators monitor disk I/O?

## Section 8: Advanced Linux Security

**Question 36:** What is the principle of least privilege?

**Question 37:** What is Access Control List (ACL)?

**Question 38:** Explain the purpose of PAM.

**Question 39:** What is the difference between SELinux and AppArmor?

**Question 40:** What are Linux capabilities?

## Section 9: Containers and Virtualization

**Question 41:** What is virtualization?

**Question 42:** Explain the difference between a Type 1 and Type 2 hypervisor.

**Question 43:** What is a container?

**Question 44:** Explain the difference between a virtual machine and a container.

**Question 45:** What is Docker used for?

## Section 10: Logging and Auditing

**Question 46:** Why are system logs important?

**Question 47:** What is the purpose of `journalctl`?

**Question 48:** What is auditd?

**Question 49:** What is log rotation?

**Question 50:** Why is time synchronization important during security investigations?

---

# PART B: COMMAND-LINE PRACTICAL ASSESSMENT

Complete these exercises in your Kali Linux virtual machine.

## Task 1: System Information

Display:

1. Linux distribution.
2. Kernel version.
3. Hostname.
4. Current username.
5. System uptime.
6. CPU information.
7. Memory information.

Useful commands:

```bash
cat /etc/os-release
uname -a
hostname
whoami
uptime
lscpu
free -h
```

## Task 2: Filesystem Inspection

Display:

1. Mounted filesystems.
2. Available disk space.
3. Directory sizes.
4. Block devices.
5. Filesystem types.

Useful commands:

```bash
df -hT
du -sh ~
lsblk -f
findmnt
```

## Task 3: Process Management

Perform the following:

1. Display running processes.
2. Identify the top CPU-consuming processes.
3. Identify the top memory-consuming processes.
4. Create a practice process.
5. Inspect its PID.
6. Terminate only the process you created.

Useful commands:

```bash
ps aux
top
sleep 300 &
pgrep -a sleep
kill PID
```

Replace `PID` with the actual PID of your practice process.

## Task 4: Networking

Display:

1. Network interfaces.
2. Assigned IP addresses.
3. Routing table.
4. Listening ports.
5. Socket statistics.

Useful commands:

```bash
ip -brief address
ip route
ss -tulpn
ss -s
```

## Task 5: Service Management

Perform these inspection tasks:

```bash
systemctl --failed
systemctl list-units --type=service
systemctl status
```

Choose one existing service and inspect its status. Do not stop or disable essential services.

## Task 6: Log Investigation

Inspect:

```bash
journalctl -b -n 30
journalctl -p err -b
journalctl -p warning -b
dmesg --level=err,warn
```

Identify one event and explain what it means.

## Task 7: Security Inspection

Inspect:

```bash
id
groups
sudo -l
ls -ld ~
find /usr/bin -perm -4000 -type f 2>/dev/null
```

Explain what each command reveals about system security.

## Task 8: Bash Scripting

Create a script named:

`final-assessment.sh`

The script must display:

- Current date.
- Hostname.
- Username.
- Kernel version.
- CPU core count.
- Memory usage.
- Disk usage.
- Network addresses.
- System uptime.

Make it executable and run it.

---

# PART C: SCENARIO-BASED QUESTIONS

These scenarios test your ability to think like a Linux administrator and cybersecurity analyst.

### Scenario 1: High CPU Usage

A Linux server suddenly becomes slow. Its CPU usage remains very high.

Answer:

1. Which commands would you use?
2. How would you identify the responsible process?
3. How would you determine whether the process is legitimate?
4. What precautions should you take before terminating anything?

### Scenario 2: Low Disk Space

A Linux server reports that its root filesystem is almost full.

Answer:

1. Which command would you use to confirm the problem?
2. How would you identify large directories?
3. How would you investigate log growth?
4. Why should you avoid deleting unknown system files?

### Scenario 3: Suspicious Login Activity

An administrator notices unexpected authentication events.

Answer:

1. Which logs would you inspect?
2. How would you identify the time of the events?
3. What information should you preserve?
4. Why should you avoid changing evidence during an investigation?

### Scenario 4: Network Connectivity Failure

A Linux machine cannot communicate with another authorized lab machine.

Answer:

1. How would you check its network interface?
2. How would you inspect its IP configuration?
3. How would you inspect its routing table?
4. How would you investigate DNS problems?
5. How would you distinguish a local problem from a remote problem?

### Scenario 5: Unauthorized Access Risk

An ordinary account has unnecessary administrative privileges.

Answer:

1. Why is this dangerous?
2. How does least privilege help?
3. What evidence should an administrator collect?
4. Why should privilege changes follow an approved administrative process?

### Scenario 6: Failed Service

A systemd service repeatedly fails to start.

Answer:

1. Which command would you use to inspect its status?
2. How would you inspect its journal entries?
3. What configuration issues could cause the failure?
4. Why should you investigate before repeatedly restarting it?

---

# PART D: PRACTICAL PROJECT SUBMISSION

Select your completed projects from Lesson 14.

Your portfolio should contain:

| Project | Required deliverable |
|---|---|
| System Dashboard | Bash script and sample output |
| Health Monitor | Script and health report |
| Permission Auditor | Script and audit report |
| Backup Utility | Script and verified archive |
| Log Analyzer | Script and log report |
| Network Inspector | Script and network report |
| Security Assessment | Script and assessment report |
| Integrated Toolkit | Working menu and README |

## Project folder structure

Your final project folder should resemble:

```text
module17-lab/
└── lesson14/
    ├── project1/
    │   └── system-dashboard.sh
    ├── project2/
    │   ├── health-monitor.sh
    │   └── health-report.txt
    ├── project3/
    │   ├── permission-auditor.sh
    │   └── permission-report.txt
    ├── project4/
    │   ├── backup.sh
    │   ├── sample-data/
    │   └── backups/
    ├── project5/
    │   ├── log-analyzer.sh
    │   └── log-report.txt
    ├── project6/
    │   ├── network-inspector.sh
    │   └── network-report.txt
    ├── project7/
    │   ├── security-assessment.sh
    │   └── security-assessment.txt
    └── project8/
        ├── linux-toolkit.sh
        └── README.md
```

Your actual directory may contain additional files.

---

# PART E: MARKING SCHEME

The following is a suggested 100-mark assessment structure.

| Assessment area | Marks |
|---|---:|
| Theoretical assessment | 30 |
| Command-line practical tasks | 25 |
| Scenario-based questions | 20 |
| Practical projects | 20 |
| Documentation and organization | 5 |
| **Total** | **100** |

### Performance guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Needs improvement |

These are suggested course assessment bands, not an official university grading scale.

---

# PART F: FINAL REFLECTION

Answer these questions honestly.

1. What Linux topic did you find most challenging?
2. Which project are you most proud of?
3. Can you now write a Bash script independently?
4. Can you inspect Linux processes and services?
5. Can you investigate basic Linux networking problems?
6. Can you identify common Linux security risks?
7. Can you inspect and interpret system logs?
8. Which area do you want to specialize in next?

---

# FINAL COURSE COMPLETION CHECKLIST

- [ ] Module 01 – Linux Fundamentals
- [ ] Module 02 – Kali Linux Setup
- [ ] Module 03 – Linux Filesystem
- [ ] Module 04 – Linux Kernel and Shell
- [ ] Module 05 – Linux Commands
- [ ] Module 06 – File and Directory Management
- [ ] Module 07 – Users and Permissions
- [ ] Module 08 – Process Management
- [ ] Module 09 – Package Management
- [ ] Module 10 – Linux Networking
- [ ] Module 11 – Bash Scripting
- [ ] Module 12 – Linux System Administration
- [ ] Module 13 – Linux Security
- [ ] Module 14 – Firewalls and IDS/IPS
- [ ] Module 15 – Network Security
- [ ] Module 16 – Penetration Testing
- [ ] Module 17 – Advanced Linux

---

# FINAL MESSAGE

Congratulations on reaching the end of the 17-module Linux course.

You have studied Linux fundamentals, system administration, networking, Bash scripting, cybersecurity, penetration-testing methodology, virtualization, performance monitoring, and advanced Linux security.

The most important lesson is that Linux expertise is developed through consistent practical work.

Do not measure your progress only by the number of commands you know. Measure it by your ability to understand a problem, investigate it, choose the right solution, verify the result, and document your work.

**END OF MODULE 17**

**END OF THE 17-MODULE LINUX COURSE**
