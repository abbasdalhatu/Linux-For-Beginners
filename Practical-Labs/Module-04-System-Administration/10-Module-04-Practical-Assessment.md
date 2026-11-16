
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 10: MODULE 04 PRACTICAL ASSESSMENT

### 1. Assessment Overview

This assessment evaluates your practical understanding of Linux system administration.

You will demonstrate your ability to:

1. Inspect Linux system information.
2. Manage users and groups.
3. Monitor and control processes.
4. Investigate systemd services.
5. Inspect package management.
6. Analyze storage and disk usage.
7. Monitor system performance.
8. Investigate system logs.
9. Create and restore backups.
10. Generate an administrative system report.

**Assessment environment:** Kali Linux VM.

**Important:** Perform all practical activities inside your own Kali VM. Do not modify production systems or scan unauthorized devices.

### 2. Assessment Preparation

Create the assessment directory:

```bash
mkdir -p ~/practical-labs/module04/final-assessment
```

Move into it:

```bash
cd ~/practical-labs/module04/final-assessment
```

Create these directories:

```bash
mkdir -p reports
mkdir -p backup-source
mkdir -p backup-restored
```

Confirm your location:

```bash
pwd
```

### 3. Task 1 – System Information Investigation

Inspect your Linux system.

Run:

```bash
uname -a
```

```bash
cat /etc/os-release
```

```bash
hostnamectl
```

```bash
whoami
```

```bash
id
```

```bash
uptime
```

**Your objective:**

Identify:

- Linux distribution.
- Kernel version.
- Hostname.
- Current username.
- User ID.
- System uptime.

Save the findings:

```bash
uname -a > reports/system-information.txt
cat /etc/os-release >> reports/system-information.txt
hostnamectl >> reports/system-information.txt
whoami >> reports/system-information.txt
id >> reports/system-information.txt
uptime >> reports/system-information.txt
```

### 4. Task 2 – User and Group Administration

Inspect existing users:

```bash
cut -d: -f1 /etc/passwd
```

Inspect groups:

```bash
cat /etc/group
```

Inspect your current identity:

```bash
id
```

Create a test user:

```bash
sudo useradd -m assessmentuser
```

Create a group:

```bash
sudo groupadd assessmentgroup
```

Add the user to the group:

```bash
sudo usermod -aG assessmentgroup assessmentuser
```

Verify:

```bash
id assessmentuser
```

Create a group workspace:

```bash
sudo mkdir -p /opt/assessment-workspace
```

Set ownership:

```bash
sudo chown root:assessmentgroup /opt/assessment-workspace
```

Set permissions:

```bash
sudo chmod 770 /opt/assessment-workspace
```

Inspect:

```bash
ls -ld /opt/assessment-workspace
```

**Your objective:** Demonstrate that you understand users, groups, ownership, and permissions.

### 5. Task 3 – Process Management

Inspect running processes:

```bash
ps aux
```

Inspect the process tree:

```bash
pstree
```

Inspect CPU and memory usage:

```bash
top
```

Start a harmless test process:

```bash
sleep 300 &
```

Identify it:

```bash
pgrep -a sleep
```

Inspect its process information:

```bash
ps -C sleep -o pid,ppid,stat,cmd
```

Terminate only your test process:

```bash
pkill -TERM -x sleep
```

Verify:

```bash
pgrep -a sleep
```

**Your objective:** Demonstrate process inspection, identification, and safe termination.

### 6. Task 4 – Systemd and Service Management

Inspect systemd:

```bash
ps -p 1 -o pid,comm,args
```

List running services:

```bash
systemctl list-units --type=service --state=running
```

Inspect failed services:

```bash
systemctl --failed
```

Inspect cron:

```bash
systemctl status cron --no-pager
```

Inspect its logs:

```bash
journalctl -u cron -n 20 --no-pager
```

Inspect listening sockets:

```bash
ss -tulpen
```

**Your objective:** Identify the role of systemd and explain how service and socket inspection helps administrators.

Do not stop, disable, or restart system services during this assessment.

### 7. Task 5 – Package Management

Inspect APT:

```bash
apt --version
```

Search for a package:

```bash
apt search curl
```

Inspect package information:

```bash
apt show curl
```

Inspect installed packages:

```bash
apt list --installed | head -n 20
```

Inspect package policy:

```bash
apt-cache policy curl
```

Simulate an installation:

```bash
sudo apt-get -s install curl
```

The `-s` option simulates the operation without actually installing the package.

**Your objective:** Demonstrate package discovery, inspection, and safe simulation.

### 8. Task 6 – Storage and Disk Management

Inspect block devices:

```bash
lsblk
```

Inspect disk usage:

```bash
df -h
```

Inspect inode usage:

```bash
df -i
```

Inspect mounted filesystems:

```bash
findmnt
```

Inspect the home directory size:

```bash
du -sh ~
```

Inspect partition information:

```bash
sudo fdisk -l
```

**Your objective:** Identify available storage, mounted filesystems, and disk usage.

Do not partition or format any disk.

### 9. Task 7 – System Monitoring and Performance

Inspect CPU information:

```bash
lscpu
```

Check CPU count:

```bash
nproc
```

Inspect load averages:

```bash
uptime
```

Inspect memory:

```bash
free -h
```

Inspect memory details:

```bash
head -n 15 /proc/meminfo
```

Inspect the top memory-consuming processes:

```bash
ps aux --sort=-%mem | head
```

Inspect the top CPU-consuming processes:

```bash
ps aux --sort=-%cpu | head
```

Inspect network interface statistics:

```bash
ip -s link
```

**Your objective:** Identify the main resources that affect Linux system performance.

### 10. Task 8 – System Log Investigation

Inspect recent journal entries:

```bash
journalctl -n 30 --no-pager
```

Inspect errors:

```bash
journalctl -p err -b --no-pager
```

Inspect warnings:

```bash
journalctl -p warning -b -n 30 --no-pager
```

Inspect authentication-related events:

```bash
journalctl -b --no-pager | grep -iE 'sudo|authentication|failed'
```

Inspect kernel messages:

```bash
sudo dmesg -T | tail -n 30
```

**Your objective:** Identify relevant system events and explain how logs support troubleshooting and incident response.

### 11. Task 9 – Backup and Recovery

Create sample files:

```bash
echo "Linux Administration Assessment" > backup-source/notes.txt
```

```bash
echo "System backup verification" > backup-source/verification.txt
```

Create a compressed backup:

```bash
tar -czvf reports/assessment-backup.tar.gz backup-source
```

Inspect the archive:

```bash
tar -tzvf reports/assessment-backup.tar.gz
```

Restore the archive:

```bash
tar -xzvf reports/assessment-backup.tar.gz -C backup-restored
```

Compare the original and restored directories:

```bash
diff -r backup-source backup-restored/backup-source
```

Generate a checksum:

```bash
sha256sum reports/assessment-backup.tar.gz
```

**Your objective:** Demonstrate backup creation, restoration, and integrity verification.

### 12. Task 10 – Generate Your Final Administrative Report

Create the report script:

```bash
nano system-admin-report.sh
```

Enter:

```bash
#!/bin/bash

BASE="$HOME/practical-labs/module04/final-assessment"
REPORT="$BASE/reports/final-system-administration-report.txt"

{
    echo "MODULE 04 FINAL SYSTEM ADMINISTRATION REPORT"
    echo "============================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "SYSTEM INFORMATION"
    uname -a
    cat /etc/os-release
    echo

    echo "CURRENT USER"
    whoami
    id
    echo

    echo "SYSTEM UPTIME"
    uptime
    echo

    echo "MEMORY"
    free -h
    echo

    echo "STORAGE"
    df -h
    echo

    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running --no-pager
    echo

    echo "FAILED SERVICES"
    systemctl --failed --no-pager
    echo

    echo "RECENT SYSTEM ERRORS"
    journalctl -p err -b -n 15 --no-pager
    echo

    echo "BACKUP ARCHIVE"
    ls -lh "$BASE/reports/assessment-backup.tar.gz"
    echo

    echo "BACKUP CHECKSUM"
    sha256sum "$BASE/reports/assessment-backup.tar.gz"

} > "$REPORT" 2>&1

echo "Final report generated: $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x system-admin-report.sh
```

Run it:

```bash
./system-admin-report.sh
```

Inspect the report:

```bash
cat reports/final-system-administration-report.txt
```

### 13. Final Knowledge Assessment

Answer these questions in your notebook or assessment report.

1. What is Linux system administration?
2. What is the difference between a user and a group?
3. What is the purpose of file permissions?
4. What is a Linux process?
5. What is the role of systemd?
6. What is the difference between `apt update` and `apt upgrade`?
7. What is the purpose of `lsblk`?
8. What is the difference between RAM usage and disk usage?
9. What is the purpose of `journalctl`?
10. Why are authentication logs important?
11. What is the difference between a full and incremental backup?
12. Explain the 3-2-1 backup rule.
13. Why should backup restoration be tested?
14. What is the importance of monitoring system resources?
15. How can Linux logs support cybersecurity investigations?

### 14. Assessment Completion Checklist

- [ ] Inspected Linux system information.
- [ ] Created and inspected a test user and group.
- [ ] Configured a group workspace.
- [ ] Inspected and managed a harmless test process.
- [ ] Investigated systemd services.
- [ ] Inspected APT package information.
- [ ] Investigated storage and disk usage.
- [ ] Monitored system performance.
- [ ] Investigated system logs.
- [ ] Created and restored a backup.
- [ ] Verified backup integrity.
- [ ] Generated the final administrative report.
- [ ] Completed the knowledge assessment.

### 15. Assessment Cleanup

Remove only the test user and group created in this assessment.

First, remove the test user's group membership by deleting the test user:

```bash
sudo userdel -r assessmentuser
```

Remove the test group:

```bash
sudo groupdel assessmentgroup
```

Remove the test workspace:

```bash
sudo rm -r /opt/assessment-workspace
```

These commands target only the assessment resources created above. Verify each resource before removing it.

Keep your assessment reports and backup files for future reference.

### 16. Module 04 Completion

Congratulations!

You have now worked through the core responsibilities of a Linux system administrator.

You have practiced:

- Linux system inspection.
- User and group administration.
- Process management.
- Service management.
- Package management.
- Storage inspection.
- Performance monitoring.
- Log analysis.
- Backup and recovery.

These skills form an important foundation for Linux security engineering, system administration, and cybersecurity operations.

**MODULE 04 STATUS: FINAL ASSESSMENT READY**
