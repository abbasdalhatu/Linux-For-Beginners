---
---


# Module 12 – Lesson 14: Practical System Administration Projects

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Apply Linux administration skills to practical scenarios.
2. Create system information reports.
3. Monitor system resources.
4. Inspect Linux services.
5. Analyse system logs.
6. Create and verify backups.
7. Inspect user accounts and permissions.
8. Investigate network configurations.
9. Automate basic administrative tasks.
10. Document administrative findings professionally.

---

## 2. Introduction to Practical System Administration

In previous lessons, you studied individual Linux administration concepts.

You learned how to:

- Manage services.
- Monitor system resources.
- Inspect logs.
- Schedule tasks.
- Manage storage.
- Work with filesystems.
- Manage swap.
- Inspect processes.
- Maintain Linux systems.
- Create backups.
- Monitor performance.
- Use SSH.

Now you will combine these skills into practical projects.

A real Linux administrator rarely performs just one task.

For example, when investigating a slow server, an administrator may need to inspect:

1. CPU usage.
2. Memory usage.
3. Running processes.
4. Disk capacity.
5. System logs.
6. Network connections.
7. Failed services.

This lesson teaches you how to approach these tasks systematically.

---

# PROJECT 1: Linux System Information Dashboard

## 3. Project Overview

### Objective

Create a Bash script that collects important Linux system information and displays it in one report.

### Skills Used

- Bash scripting.
- System inspection.
- CPU monitoring.
- Memory monitoring.
- Storage monitoring.
- Network inspection.
- Report generation.

### Step 1: Create the project directory

```bash
mkdir -p ~/module12-lab/lesson14/project1
cd ~/module12-lab/lesson14/project1
```

### Step 2: Create the script

```bash
nano system-dashboard.sh
```

Add:

```bash
#!/bin/bash

echo "===================================="
echo "       LINUX SYSTEM DASHBOARD"
echo "===================================="

echo
echo "DATE AND TIME"
date

echo
echo "HOSTNAME"
hostname

echo
echo "OPERATING SYSTEM"
cat /etc/os-release | grep -E '^(PRETTY_NAME|VERSION)='

echo
echo "CPU INFORMATION"
lscpu | grep -E 'Model name|Architecture|CPU\(s\)'

echo
echo "MEMORY INFORMATION"
free -h

echo
echo "STORAGE INFORMATION"
df -h

echo
echo "SYSTEM UPTIME"
uptime

echo
echo "TOP CPU PROCESSES"
ps aux --sort=-%cpu | head -n 6

echo
echo "TOP MEMORY PROCESSES"
ps aux --sort=-%mem | head -n 6

echo
echo "NETWORK INFORMATION"
ip addr

echo
echo "LISTENING PORTS"
ss -tuln

echo
echo "FAILED SYSTEM SERVICES"
systemctl --failed --no-pager

echo
echo "===================================="
echo "       DASHBOARD COMPLETED"
echo "===================================="
```

Save and exit Nano.

### Step 3: Make the script executable

```bash
chmod +x system-dashboard.sh
```

### Step 4: Run the dashboard

```bash
./system-dashboard.sh
```

### Step 5: Save the output

```bash
./system-dashboard.sh > system-dashboard-report.txt
```

Display the report:

```bash
cat system-dashboard-report.txt
```

### Expected Result

You should have:

```text
system-dashboard.sh
system-dashboard-report.txt
```

The report should contain system information, CPU and memory information, storage usage, network information, and service status.

---

# PROJECT 2: Linux Service Monitoring

## 4. Project Overview

### Objective

Create a script that checks the status of important Linux services.

This is useful when an administrator needs to investigate whether required services are operating correctly.

### Step 1: Create the project directory

```bash
mkdir -p ~/module12-lab/lesson14/project2
cd ~/module12-lab/lesson14/project2
```

### Step 2: Create the script

```bash
nano service-monitor.sh
```

Add:

```bash
#!/bin/bash

echo "LINUX SERVICE MONITOR"
echo "====================="

SERVICES=("ssh" "cron" "NetworkManager")

for SERVICE in "${SERVICES[@]}"
do
    echo
    echo "Checking service: $SERVICE"

    if systemctl is-active --quiet "$SERVICE"; then
        echo "$SERVICE is ACTIVE"
    else
        echo "$SERVICE is NOT ACTIVE"
    fi
done

echo
echo "SERVICE MONITORING COMPLETED"
```

Save and exit.

### Step 3: Make the script executable

```bash
chmod +x service-monitor.sh
```

### Step 4: Run the script

```bash
./service-monitor.sh
```

### Understanding the result

You may see:

```text
ssh is NOT ACTIVE
cron is ACTIVE
NetworkManager is ACTIVE
```

This is only an example.

Your actual results depend on the services installed and configured in your Kali VM.

A service reported as inactive is not automatically a problem. Some services are intentionally stopped.

### Step 5: Save the results

```bash
./service-monitor.sh > service-monitor-report.txt
```

---

# PROJECT 3: Automated Backup System

## 5. Project Overview

### Objective

Create a Bash script that automatically creates a timestamped backup of a project directory.

### Skills Used

- Bash scripting.
- Directory management.
- File archiving.
- Compression.
- Timestamps.
- Backup verification.

### Step 1: Create the project directory

```bash
mkdir -p ~/module12-lab/lesson14/project3/source
mkdir -p ~/module12-lab/lesson14/project3/backups
cd ~/module12-lab/lesson14/project3
```

### Step 2: Create sample files

```bash
echo "Linux Administration Project" > source/notes.txt
```

```bash
echo "Backup automation exercise" > source/instructions.txt
```

### Step 3: Create the backup script

```bash
nano backup-system.sh
```

Add:

```bash
#!/bin/bash

SOURCE="$HOME/module12-lab/lesson14/project3/source"
DESTINATION="$HOME/module12-lab/lesson14/project3/backups"

TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")

BACKUP_FILE="$DESTINATION/backup-$TIMESTAMP.tar.gz"

mkdir -p "$DESTINATION"

if [ ! -d "$SOURCE" ]; then
    echo "ERROR: Source directory does not exist."
    exit 1
fi

echo "Starting backup..."

if tar -czf "$BACKUP_FILE" -C "$(dirname "$SOURCE")" "$(basename "$SOURCE")"; then
    echo "Backup completed successfully."
    echo "Backup location: $BACKUP_FILE"
else
    echo "ERROR: Backup failed."
    exit 1
fi

echo
echo "Backup verification:"

if tar -tzf "$BACKUP_FILE" > /dev/null; then
    echo "Backup archive is readable."
else
    echo "ERROR: Backup verification failed."
    exit 1
fi

sha256sum "$BACKUP_FILE"
```

Save and exit.

### Step 4: Make the script executable

```bash
chmod +x backup-system.sh
```

### Step 5: Execute the backup

```bash
./backup-system.sh
```

### Step 6: Inspect the backup directory

```bash
ls -lh backups/
```

### Step 7: Inspect the backup contents

Use the actual archive filename displayed by the script:

```bash
tar -tzf backups/backup-FILENAME.tar.gz
```

Replace `backup-FILENAME.tar.gz` with your real filename.

**Important:** The script creates a new backup each time it runs. It does not delete previous backups.

---

# PROJECT 4: Linux User and Permission Audit

## 6. Project Overview

### Objective

Create a basic report showing user accounts, administrative group membership, and permissions in a dedicated lab directory.

This project demonstrates how administrators inspect access control.

### Step 1: Create the project directory

```bash
mkdir -p ~/module12-lab/lesson14/project4
cd ~/module12-lab/lesson14/project4
```

### Step 2: Create the audit script

```bash
nano user-audit.sh
```

Add:

```bash
#!/bin/bash

echo "LINUX USER AND PERMISSION AUDIT"
echo "==============================="

echo
echo "CURRENT USER"
whoami

echo
echo "CURRENT USER ID"
id

echo
echo "ADMINISTRATIVE GROUP"
getent group sudo

echo
echo "LOCAL USER ACCOUNTS"
cut -d: -f1,3 /etc/passwd

echo
echo "LAB DIRECTORY PERMISSIONS"
ls -ld "$HOME/module12-lab"

echo
echo "AUDIT COMPLETED"
```

Save and exit.

### Step 3: Make the script executable

```bash
chmod +x user-audit.sh
```

### Step 4: Execute the audit

```bash
./user-audit.sh
```

### Step 5: Save the audit report

```bash
./user-audit.sh > user-audit-report.txt
```

Inspect the report:

```bash
cat user-audit-report.txt
```

### Security lesson

A user account should receive only the permissions needed to perform its assigned tasks.

This principle is called the **Principle of Least Privilege**.

---

# PROJECT 5: Linux Log Investigation

## 7. Project Overview

### Objective

Create a script that collects recent system log information for administrative investigation.

### Step 1: Create the project directory

```bash
mkdir -p ~/module12-lab/lesson14/project5
cd ~/module12-lab/lesson14/project5
```

### Step 2: Create the script

```bash
nano log-investigator.sh
```

Add:

```bash
#!/bin/bash

echo "LINUX LOG INVESTIGATION"
echo "======================="

echo
echo "RECENT SYSTEM LOGS"

journalctl -b -n 20 --no-pager

echo
echo "ERROR PRIORITY LOGS"

journalctl -p err -b -n 20 --no-pager

echo
echo "FAILED SYSTEM SERVICES"

systemctl --failed --no-pager

echo
echo "LOG INVESTIGATION COMPLETED"
```

Save and exit.

### Step 3: Make the script executable

```bash
chmod +x log-investigator.sh
```

### Step 4: Execute the investigation

```bash
./log-investigator.sh
```

### Step 5: Save the investigation report

```bash
./log-investigator.sh > log-investigation-report.txt 2>&1
```

### Understanding the output

You may encounter:

- Informational messages.
- Warnings.
- Errors.
- Service startup events.
- Authentication-related events.

Not every error represents a security incident.

Investigate unusual events using their timestamps, sources, and surrounding context.

---

# PROJECT 6: Linux Network Administration Inspection

## 8. Project Overview

### Objective

Create a report that gathers network configuration and connection information.

### Step 1: Create the project directory

```bash
mkdir -p ~/module12-lab/lesson14/project6
cd ~/module12-lab/lesson14/project6
```

### Step 2: Create the script

```bash
nano network-audit.sh
```

Add:

```bash
#!/bin/bash

echo "LINUX NETWORK ADMINISTRATION REPORT"
echo "==================================="

echo
echo "NETWORK INTERFACES"
ip addr

echo
echo "ROUTING TABLE"
ip route

echo
echo "LISTENING TCP AND UDP PORTS"
ss -tuln

echo
echo "NETWORK INTERFACE STATISTICS"
ip -s link

echo
echo "DNS CONFIGURATION"
cat /etc/resolv.conf

echo
echo "NETWORK INSPECTION COMPLETED"
```

Save and exit.

### Step 3: Make the script executable

```bash
chmod +x network-audit.sh
```

### Step 4: Execute the script

```bash
./network-audit.sh
```

### Step 5: Save the report

```bash
./network-audit.sh > network-audit-report.txt
```

### Important security lesson

Network administration requires understanding which services are exposed and which network interfaces are active.

Only expose services that are required and authorised.

---

# 9. Combining Your Projects

You have now created six practical administration projects.

| Project | Main Skill |
|---|---|
| 1. System Dashboard | System monitoring |
| 2. Service Monitor | Service administration |
| 3. Backup System | Backup automation |
| 4. User Audit | Access control |
| 5. Log Investigator | Log analysis |
| 6. Network Audit | Network administration |

These projects represent common tasks performed by Linux administrators.

They can also serve as the foundation of a small technical portfolio.

---

# 10. Final Practical Challenge

## Scenario: You Are the Linux System Administrator

Imagine that you have been assigned responsibility for a Linux server used by a university department.

Your supervisor wants a basic administration inspection.

You must prepare a report covering:

1. System identity.
2. CPU and memory.
3. Disk usage.
4. Running processes.
5. Failed services.
6. Network configuration.
7. Listening ports.
8. User and administrative group information.
9. Recent system errors.
10. Backup verification.

### Your task

Create a directory:

```bash
mkdir -p ~/module12-lab/lesson14/final-challenge
```

Collect the information using the commands learned in previous lessons.

Save the final report as:

```text
final-administration-report.txt
```

Your report must contain clear headings and actual command output.

Do not change system settings or attempt to repair anything during this inspection.

The objective is to assess the system and document your findings.

---

# 11. Professional Administration Reporting

A professional administrator should document:

### A. Finding

What did you discover?

Example:

The root filesystem has 75% usage.

### B. Evidence

Which command produced the information?

Example:

```bash
df -h
```

### C. Assessment

Why does the finding matter?

Example:

Storage usage should be monitored to prevent the filesystem from becoming full.

### D. Recommendation

What should be done?

Example:

Continue monitoring storage and investigate unnecessary large files before capacity becomes critical.

### E. Verification

How will you confirm that the recommended action worked?

Example:

Run `df -h` again and compare the available storage.

A report should distinguish confirmed problems from observations that require further investigation.

---

# 12. Lesson Summary

You have applied Linux administration skills to six projects:

1. System information dashboard.
2. Service monitoring.
3. Automated backup.
4. User and permission auditing.
5. System log investigation.
6. Network administration inspection.

You also learned how to combine these activities into a professional administrative report.

---

# 13. Knowledge Check

1. What is the purpose of a system information dashboard?
2. Why should administrators monitor services?
3. What is the purpose of automated backups?
4. What does the Principle of Least Privilege mean?
5. Why are system logs important?
6. What is the purpose of a network audit?
7. Why should administrators verify backup archives?
8. Why is documentation important?
9. What is the difference between a finding and a recommendation?
10. Why should administrators inspect systems before making changes?

---

# 14. Practical Completion Checklist

- [ ] I created the system dashboard.
- [ ] I created the service-monitoring script.
- [ ] I created the automated backup system.
- [ ] I created the user and permission audit.
- [ ] I created the log investigation script.
- [ ] I created the network administration report.
- [ ] I completed the final administration challenge.
- [ ] I understand how to document findings professionally.
- [ ] I understand how these projects apply to real Linux administration.

**Lesson 14 is complete when you have completed the six projects and produced your final administration report.**
