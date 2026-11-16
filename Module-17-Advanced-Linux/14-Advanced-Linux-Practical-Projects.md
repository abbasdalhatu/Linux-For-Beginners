
# MODULE 17 – ADVANCED LINUX
## LESSON 14: ADVANCED LINUX PRACTICAL PROJECTS

---

## 1. Introduction

This lesson brings together the major concepts covered throughout Module 17.

Instead of studying individual commands, you will build practical Linux administration and security projects.

These projects simulate tasks performed by Linux system administrators, cybersecurity analysts, and infrastructure engineers.

### Learning objectives

By the end of this lesson, you should be able to:

1. Build a Linux system information dashboard.
2. Automate system health monitoring.
3. Build a user and permission auditing tool.
4. Create a Linux backup utility.
5. Develop a log analysis tool.
6. Build a network inspection utility.
7. Create a Linux security assessment tool.
8. Combine multiple administrative tasks into one toolkit.
9. Document your projects professionally.

---

## 2. Project 1: Linux System Information Dashboard

### Objective

Create a Bash script that displays important information about your Linux system.

### Skills applied

- Linux commands.
- Bash scripting.
- CPU inspection.
- Memory monitoring.
- Storage inspection.
- Operating system identification.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project1
cd ~/module17-lab/lesson14/project1
```

### Step 2: Create the script

```bash
nano system-dashboard.sh
```

Enter:

```bash
#!/bin/bash

clear

echo "======================================"
echo "       LINUX SYSTEM DASHBOARD"
echo "======================================"

echo
echo "[ SYSTEM INFORMATION ]"
echo "Hostname: $(hostname)"
echo "Kernel: $(uname -r)"
echo "Operating System: $(. /etc/os-release && echo "$PRETTY_NAME")"
echo "Date: $(date)"

echo
echo "[ CPU INFORMATION ]"
echo "CPU Cores: $(nproc)"

echo
echo "[ MEMORY INFORMATION ]"
free -h

echo
echo "[ STORAGE INFORMATION ]"
df -h

echo
echo "[ SYSTEM LOAD ]"
uptime

echo
echo "[ NETWORK INFORMATION ]"
ip -brief address

echo
echo "[ RUNNING SERVICES ]"
systemctl --type=service --state=running --no-pager |
head -n 10

echo
echo "======================================"
echo "       DASHBOARD COMPLETE"
echo "======================================"
```

### Step 3: Make it executable

```bash
chmod +x system-dashboard.sh
```

### Step 4: Run the dashboard

```bash
./system-dashboard.sh
```

### Expected result

The dashboard should display:

- Hostname.
- Kernel version.
- Operating system.
- CPU cores.
- Memory usage.
- Disk usage.
- System load.
- Network addresses.
- Running services.

### Challenge

Modify the dashboard to display the current username and uptime in a more readable format.

---

## 3. Project 2: Automated System Health Monitor

### Objective

Create a script that checks important system health indicators and produces a report.

### Skills applied

- Performance monitoring.
- Bash scripting.
- Conditional statements.
- Resource inspection.
- Report generation.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project2
cd ~/module17-lab/lesson14/project2
```

### Step 2: Create the script

```bash
nano health-monitor.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson14/project2/health-report.txt"

{
    echo "======================================"
    echo "       LINUX HEALTH MONITOR"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "SYSTEM UPTIME:"
    uptime

    echo
    echo "CPU COUNT:"
    nproc

    echo
    echo "MEMORY STATUS:"
    free -h

    echo
    echo "DISK STATUS:"
    df -h

    echo
    echo "FAILED SERVICES:"
    systemctl --failed

    echo
    echo "NETWORK INTERFACES:"
    ip -brief link

    echo
    echo "NETWORK ADDRESSES:"
    ip -brief address

    echo
    echo "HEALTH CHECK COMPLETE"

} > "$REPORT" 2>&1

echo "Health report saved to: $REPORT"
```

### Step 3: Make it executable

```bash
chmod +x health-monitor.sh
```

### Step 4: Execute the monitor

```bash
./health-monitor.sh
```

### Step 5: Read the report

```bash
cat health-report.txt
```

### Challenge

Add a check that identifies filesystems with less than 15% available space.

Remember that a filesystem's usage percentage is not always the same as its percentage of available space.

---

## 4. Project 3: Linux User and Permission Auditor

### Objective

Build a tool that examines local user accounts, groups, and file permissions.

### Skills applied

- User management.
- Group inspection.
- File permissions.
- Access control.
- Security auditing.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project3
cd ~/module17-lab/lesson14/project3
```

### Step 2: Create the script

```bash
nano permission-auditor.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson14/project3/permission-report.txt"

{
    echo "======================================"
    echo "     LINUX PERMISSION AUDITOR"
    echo "======================================"

    echo
    echo "CURRENT USER:"
    whoami

    echo
    echo "USER IDENTITY:"
    id

    echo
    echo "LOCAL USER ACCOUNTS:"
    cut -d: -f1 /etc/passwd

    echo
    echo "LOCAL GROUPS:"
    cut -d: -f1 /etc/group

    echo
    echo "CURRENT USER GROUPS:"
    groups

    echo
    echo "HOME DIRECTORY PERMISSIONS:"
    ls -ld "$HOME"

    echo
    echo "SUDO PRIVILEGES:"
    sudo -l

    echo
    echo "SUID FILES IN /USR/BIN:"
    find /usr/bin -perm -4000 -type f 2>/dev/null

    echo
    echo "PERMISSION AUDIT COMPLETE"

} > "$REPORT" 2>&1

echo "Permission report saved to: $REPORT"
```

### Step 3: Make it executable

```bash
chmod +x permission-auditor.sh
```

### Step 4: Run the auditor

```bash
./permission-auditor.sh
```

### Step 5: Read the report

```bash
less permission-report.txt
```

### Challenge

Explain why a user account with unrestricted sudo privileges presents a greater security risk than an ordinary user account.

---

## 5. Project 4: Linux Backup Utility

### Objective

Build a backup script that creates a timestamped archive of a directory you own.

### Skills applied

- File management.
- Bash scripting.
- Compression.
- Backup automation.
- Data protection.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project4
cd ~/module17-lab/lesson14/project4
```

### Step 2: Create sample data

```bash
mkdir -p sample-data
echo "Linux backup practice" > sample-data/example.txt
echo "Module 17 project" > sample-data/project.txt
```

### Step 3: Create the backup script

```bash
nano backup.sh
```

Enter:

```bash
#!/bin/bash

SOURCE="$HOME/module17-lab/lesson14/project4/sample-data"
DEST="$HOME/module17-lab/lesson14/project4/backups"

mkdir -p "$DEST"

TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")

ARCHIVE="$DEST/backup_$TIMESTAMP.tar.gz"

if [ ! -d "$SOURCE" ]; then
    echo "Source directory does not exist."
    exit 1
fi

if tar -czf "$ARCHIVE" -C "$(dirname "$SOURCE")" "$(basename "$SOURCE")"; then
    echo "Backup completed successfully."
    echo "Archive: $ARCHIVE"
else
    echo "Backup failed."
    exit 1
fi
```

### Step 4: Make it executable

```bash
chmod +x backup.sh
```

### Step 5: Run the backup

```bash
./backup.sh
```

### Step 6: Inspect backup files

```bash
ls -lh backups/
```

### Step 7: Verify archive contents

```bash
tar -tzf backups/backup_*.tar.gz
```

If multiple archives exist, specify the exact archive you want to inspect.

### Challenge

Create a second backup after modifying one of your sample files.

Compare the two archive names and explain why timestamps are useful.

---

## 6. Project 5: Linux Log Analysis Tool

### Objective

Create a script that searches the system journal for errors and warnings.

### Skills applied

- Journal inspection.
- Log filtering.
- Bash scripting.
- Security investigation.
- Evidence collection.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project5
cd ~/module17-lab/lesson14/project5
```

### Step 2: Create the script

```bash
nano log-analyzer.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson14/project5/log-report.txt"

{
    echo "======================================"
    echo "        LINUX LOG ANALYZER"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "RECENT SYSTEM ERRORS:"
    journalctl -p err -b -n 30 --no-pager

    echo
    echo "RECENT SYSTEM WARNINGS:"
    journalctl -p warning -b -n 30 --no-pager

    echo
    echo "RECENT AUTHENTICATION EVENTS:"
    journalctl -b -n 100 --no-pager |
    grep -iE 'authentication|sudo|ssh' |
    tail -n 30

    echo
    echo "FAILED SERVICES:"
    systemctl --failed

    echo
    echo "LOG ANALYSIS COMPLETE"

} > "$REPORT" 2>&1

echo "Log report saved to: $REPORT"
```

### Step 3: Make it executable

```bash
chmod +x log-analyzer.sh
```

### Step 4: Run the analyzer

```bash
./log-analyzer.sh
```

### Step 5: Read the report

```bash
less log-report.txt
```

### Challenge

Identify one warning or error in your report and explain whether it represents a genuine problem, an expected event, or something requiring further investigation.

---

## 7. Project 6: Linux Network Inspection Tool

### Objective

Build a network inspection script that gathers information about network interfaces, IP addresses, routes, and listening services.

### Skills applied

- Linux networking.
- Network diagnostics.
- Socket inspection.
- Routing.
- Bash automation.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project6
cd ~/module17-lab/lesson14/project6
```

### Step 2: Create the script

```bash
nano network-inspector.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson14/project6/network-report.txt"

{
    echo "======================================"
    echo "       NETWORK INSPECTION TOOL"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "NETWORK INTERFACES:"
    ip -brief link

    echo
    echo "IP ADDRESSES:"
    ip -brief address

    echo
    echo "ROUTING TABLE:"
    ip route

    echo
    echo "LISTENING SERVICES:"
    ss -tulpn

    echo
    echo "SOCKET SUMMARY:"
    ss -s

    echo
    echo "NETWORK STATISTICS:"
    ip -s link

    echo
    echo "NETWORK INSPECTION COMPLETE"

} > "$REPORT" 2>&1

echo "Network report saved to: $REPORT"
```

### Step 3: Make it executable

```bash
chmod +x network-inspector.sh
```

### Step 4: Run the tool

```bash
./network-inspector.sh
```

### Step 5: Read the report

```bash
less network-report.txt
```

### Challenge

Identify:

1. Your active network interface.
2. Your assigned IP address.
3. Your default route.
4. Whether any services are listening on network ports.

Only inspect systems and network services you own or are authorized to administer.

---

## 8. Project 7: Linux Security Assessment Tool

### Objective

Create a basic security assessment script that combines information from earlier lessons.

### Skills applied

- User security.
- File permissions.
- Service inspection.
- Authentication records.
- System auditing.
- Security reporting.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project7
cd ~/module17-lab/lesson14/project7
```

### Step 2: Create the script

```bash
nano security-assessment.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson14/project7/security-assessment.txt"

{
    echo "======================================"
    echo "      LINUX SECURITY ASSESSMENT"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "OPERATING SYSTEM:"
    cat /etc/os-release

    echo
    echo "CURRENT USER:"
    whoami

    echo
    echo "USER PRIVILEGES:"
    id

    echo
    echo "SUDO PRIVILEGES:"
    sudo -l

    echo
    echo "LISTENING NETWORK SERVICES:"
    ss -tulpn

    echo
    echo "FAILED SERVICES:"
    systemctl --failed

    echo
    echo "RECENT SYSTEM ERRORS:"
    journalctl -p err -b -n 20 --no-pager

    echo
    echo "SUID EXECUTABLES:"
    find /usr/bin -perm -4000 -type f 2>/dev/null

    echo
    echo "SSH CONFIGURATION:"
    if command -v sshd >/dev/null 2>&1; then
        sudo sshd -T
    else
        echo "SSH server is not installed."
    fi

    echo
    echo "SECURITY ASSESSMENT COMPLETE"

} > "$REPORT" 2>&1

echo "Security assessment saved to: $REPORT"
```

### Step 3: Make it executable

```bash
chmod +x security-assessment.sh
```

### Step 4: Run the assessment

```bash
./security-assessment.sh
```

### Step 5: Review the report

```bash
less security-assessment.txt
```

### Important

This is an introductory inspection tool, not a complete vulnerability scanner or professional penetration-testing platform.

A finding must be validated before it is classified as a vulnerability.

---

## 9. Project 8: Build an Integrated Linux Administration Toolkit

### Objective

Combine several of your projects into one menu-driven toolkit.

The toolkit will allow a user to choose between:

1. System dashboard.
2. Health monitoring.
3. Permission auditing.
4. Backup creation.
5. Log analysis.
6. Network inspection.
7. Security assessment.

### Step 1: Create the project directory

```bash
mkdir -p ~/module17-lab/lesson14/project8
cd ~/module17-lab/lesson14/project8
```

### Step 2: Create the toolkit

```bash
nano linux-toolkit.sh
```

Enter:

```bash
#!/bin/bash

BASE="$HOME/module17-lab/lesson14"

while true
do

    clear

    echo "======================================"
    echo "       ADVANCED LINUX TOOLKIT"
    echo "======================================"

    echo
    echo "1. System Dashboard"
    echo "2. Health Monitor"
    echo "3. Permission Auditor"
    echo "4. Backup Utility"
    echo "5. Log Analyzer"
    echo "6. Network Inspector"
    echo "7. Security Assessment"
    echo "8. Exit"

    echo
    read -rp "Select an option: " choice

    case "$choice" in

        1)
            bash "$BASE/project1/system-dashboard.sh"
            ;;

        2)
            bash "$BASE/project2/health-monitor.sh"
            ;;

        3)
            bash "$BASE/project3/permission-auditor.sh"
            ;;

        4)
            bash "$BASE/project4/backup.sh"
            ;;

        5)
            bash "$BASE/project5/log-analyzer.sh"
            ;;

        6)
            bash "$BASE/project6/network-inspector.sh"
            ;;

        7)
            bash "$BASE/project7/security-assessment.sh"
            ;;

        8)
            echo "Exiting Linux Toolkit."
            break
            ;;

        *)
            echo "Invalid selection."
            ;;

    esac

    echo
    read -rp "Press Enter to return to the menu..."

done
```

### Step 3: Make it executable

```bash
chmod +x linux-toolkit.sh
```

### Step 4: Run the toolkit

```bash
./linux-toolkit.sh
```

### Step 5: Test every menu option

Verify that each option runs its corresponding script.

The toolkit expects the seven previous projects to exist at the specified paths.

---

## 10. Project Documentation

Professional developers and system administrators document their work.

Create a `README.md` file inside your toolkit directory.

```bash
nano README.md
```

Use this template:

```markdown
# Advanced Linux Administration Toolkit

## Description

A Bash-based toolkit for Linux administration,
system monitoring, security inspection, and
basic backup operations.

## Features

1. System Dashboard
2. Health Monitor
3. Permission Auditor
4. Backup Utility
5. Log Analyzer
6. Network Inspector
7. Security Assessment

## Requirements

- Linux operating system
- Bash shell
- Standard Linux utilities
- Appropriate permissions for privileged inspections

## Installation

Clone or copy the project into your Linux environment.

Make the scripts executable:

chmod +x *.sh

## Usage

Run:

./linux-toolkit.sh

Select the desired operation from the menu.

## Security

The toolkit is designed for authorized administration
and read-only system inspection, except for its
backup operation, which creates archives in the
project directory.

## Author

Your Name

## Version

1.0
```

Replace `Your Name` with your preferred author name.

---

## 11. Project Verification Checklist

Before considering your toolkit complete, verify each project.

| Project | Expected output |
|---|---|
| System Dashboard | System information displayed |
| Health Monitor | Health report generated |
| Permission Auditor | Permission report generated |
| Backup Utility | Timestamped archive created |
| Log Analyzer | Log report generated |
| Network Inspector | Network report generated |
| Security Assessment | Security report generated |
| Integrated Toolkit | All menu options work |

### Verify project files

```bash
find ~/module17-lab/lesson14 -maxdepth 3 -type f
```

### Inspect disk usage

```bash
du -sh ~/module17-lab/lesson14
```

### Inspect permissions

```bash
find ~/module17-lab/lesson14 -type f -name "*.sh" -exec ls -l {} \;
```

---

## 12. Professional Development Challenges

After completing the eight projects, attempt these additional challenges.

### Challenge A: Improve the dashboard

Add:

- Current username.
- Number of running processes.
- System uptime in readable format.
- Available memory.
- Disk usage warnings.

### Challenge B: Improve the backup utility

Add:

- Backup verification.
- A restore option.
- Clear error handling.
- A backup history file.

Perform restoration only into a separate practice directory.

### Challenge C: Improve the security assessment

Add:

- Failed login summaries.
- Available package update information.
- AppArmor or SELinux status where available.
- Auditd availability.
- A clear statement that findings require manual validation.

### Challenge D: Improve the toolkit

Add:

- Input validation.
- Better error messages.
- A help section.
- Version information.
- A log file for toolkit operations.

### Challenge E: Add automation

Explore how to schedule your health-monitoring script using a user-level systemd timer.

Use the same safe user-level approach covered in Lesson 04.

---

## 13. Knowledge Check

Answer these questions in your notebook.

1. What is system administration automation?
2. Why do administrators create monitoring scripts?
3. What is the purpose of a system information dashboard?
4. Why is backup verification important?
5. What is the difference between a backup and a restore?
6. How can Bash scripts improve security monitoring?
7. Why should security findings be validated?
8. What is a menu-driven Bash application?
9. Why is documentation important in software projects?
10. Explain how your integrated toolkit combines different Module 17 topics.

---

## 14. Lesson Summary

In this lesson, you applied the concepts from Module 17 to practical projects.

You built:

1. Linux System Information Dashboard.
2. Automated System Health Monitor.
3. User and Permission Auditor.
4. Linux Backup Utility.
5. Linux Log Analysis Tool.
6. Network Inspection Tool.
7. Linux Security Assessment Tool.
8. Integrated Linux Administration Toolkit.

You also created project documentation and explored professional development challenges.

### Completion checklist

- [ ] Complete Project 1.
- [ ] Complete Project 2.
- [ ] Complete Project 3.
- [ ] Complete Project 4.
- [ ] Complete Project 5.
- [ ] Complete Project 6.
- [ ] Complete Project 7.
- [ ] Complete Project 8.
- [ ] Create the README.md file.
- [ ] Verify all project scripts.
- [ ] Attempt at least two development challenges.

**END OF LESSON 14**
