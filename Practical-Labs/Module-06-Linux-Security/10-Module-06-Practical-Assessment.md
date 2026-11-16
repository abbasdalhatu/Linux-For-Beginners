
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 10: MODULE 06 FINAL PRACTICAL ASSESSMENT

### 1. Assessment Overview

This is the final practical assessment for Module 06 – Linux Security.

You will demonstrate your understanding of Linux security fundamentals, account protection, authentication, privilege management, SSH security, filesystem auditing, service security, and system hardening.

**Assessment environment:** Your own Kali Linux virtual machine.

**Assessment type:** Practical security assessment.

**Testing approach:** Local inspection and controlled lab exercises.

**Total marks:** 100.

### 2. Assessment Objectives

By completing this assessment, you should demonstrate the ability to:

1. Inspect Linux security configurations.
2. Identify user and group information.
3. Review authentication policies.
4. Examine administrative privileges.
5. Inspect SSH security.
6. Audit filesystem permissions.
7. Inspect system services.
8. Identify network listening sockets.
9. Review security-related logs.
10. Produce a professional Linux security assessment report.

### 3. Create the Assessment Directory

Run:

```bash
mkdir -p ~/practical-labs/module06/lab10
cd ~/practical-labs/module06/lab10
```

Confirm your location:

```bash
pwd
```

### 4. Task 1 – System Security Inventory

**Marks: 10**

Inspect your Linux environment.

Run:

```bash
cat /etc/os-release
```

```bash
uname -a
```

```bash
hostname
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

```bash
free -h
```

```bash
df -h
```

**Expected outcome:**

You should understand the operating system, kernel, current user, groups, uptime, memory, and storage.

Document your findings.

### 5. Task 2 – User and Account Security

**Marks: 10**

Inspect user accounts:

```bash
getent passwd | cut -d: -f1,3,4,6,7
```

Inspect administrative groups:

```bash
getent group sudo
```

Inspect your current account:

```bash
id
```

Inspect password aging:

```bash
chage -l "$(whoami)"
```

Inspect the permissions of the shadow database:

```bash
stat /etc/shadow
```

**Expected outcome:**

You should identify your account, group memberships, password aging information, and shadow-file access permissions without exposing password hashes.

### 6. Task 3 – Sudo and Privilege Management

**Marks: 10**

Inspect your administrative privileges:

```bash
sudo -l
```

Validate sudo configuration:

```bash
sudo visudo -c
```

Inspect the sudoers directory:

```bash
sudo ls -la /etc/sudoers.d/
```

Answer:

- What is the purpose of sudo?
- What is the principle of least privilege?
- Why should sudoers files be edited with visudo?

**Expected outcome:**

You should understand how Linux controls administrative privileges.

Do not change your sudo configuration.

### 7. Task 4 – SSH Security Assessment

**Marks: 10**

Check the SSH client:

```bash
ssh -V
```

Check whether the server package is installed:

```bash
dpkg -s openssh-server 2>/dev/null | grep -E '^(Package|Status|Version)'
```

Check service status:

```bash
systemctl status ssh --no-pager
```

If available, inspect effective server settings:

```bash
sudo sshd -T 2>/dev/null | grep -E 'permitrootlogin|passwordauthentication|pubkeyauthentication|maxauthtries|port'
```

Inspect SSH listening sockets:

```bash
sudo ss -ltnp
```

Answer:

- What is the default SSH port?
- What is the difference between public and private keys?
- Why should private keys be protected?
- Why should administrators monitor SSH authentication events?

**Expected outcome:**

You should demonstrate the ability to inspect SSH configuration and understand its security requirements.

### 8. Task 5 – Filesystem Security Assessment

**Marks: 10**

Inspect important system file metadata:

```bash
stat /etc/passwd
```

```bash
stat /etc/shadow
```

```bash
stat /etc/sudoers
```

Inspect your home directory:

```bash
stat ~
```

Inspect your current umask:

```bash
umask
```

Inspect the temporary directory:

```bash
ls -ld /tmp
```

Search your assessment directory for world-writable files:

```bash
find . -type f -perm -0002 -ls
```

Search for SUID and SGID files:

```bash
find . -type f \( -perm -4000 -o -perm -2000 \) -ls
```

**Expected outcome:**

You should understand file permissions, ownership, special permissions, and filesystem security.

### 9. Task 6 – Linux Service Security

**Marks: 10**

Inspect running services:

```bash
systemctl list-units --type=service --state=running --no-pager
```

Inspect failed services:

```bash
systemctl --failed --no-pager
```

Inspect enabled services:

```bash
systemctl list-unit-files --type=service --state=enabled --no-pager
```

Inspect systemd security properties where supported:

```bash
systemd-analyze security systemd-journald.service
```

Answer:

- What is systemd?
- What is the difference between running and enabled?
- Why should unnecessary services be reviewed?
- Why should service dependencies be examined before making changes?

**Expected outcome:**

You should understand service management and the importance of reducing unnecessary attack surfaces.

Do not disable services.

### 10. Task 7 – Network Exposure Assessment

**Marks: 10**

Inspect network interfaces:

```bash
ip -br addr
```

Inspect routes:

```bash
ip route
```

Inspect TCP listening sockets:

```bash
sudo ss -ltnp
```

Inspect UDP listening sockets:

```bash
sudo ss -lunp
```

Answer:

- What is a listening socket?
- What does `127.0.0.1` mean?
- What does `0.0.0.0` mean?
- Why does a listening port require investigation?

**Expected outcome:**

You should identify local network configuration and understand which services may accept connections.

Do not scan external networks.

### 11. Task 8 – Security Log Analysis

**Marks: 10**

Inspect recent system events:

```bash
sudo journalctl -b -n 50 --no-pager
```

Inspect authentication-related events:

```bash
sudo journalctl -b --no-pager | grep -Ei 'authentication|sudo|ssh|failed|denied' | tail -40
```

Inspect system errors:

```bash
sudo journalctl -p err -b --no-pager
```

Answer:

- Why are security logs important?
- What could repeated authentication failures indicate?
- Why should administrators investigate unusual service failures?

**Expected outcome:**

You should be able to review relevant system events and identify observations requiring further investigation.

### 12. Task 9 – Software Security and Updates

**Marks: 5**

Refresh package metadata:

```bash
sudo apt update
```

Inspect available updates:

```bash
apt list --upgradable
```

Inspect package history:

```bash
sudo tail -30 /var/log/apt/history.log
```

Answer:

- What is the purpose of `apt update`?
- Why are security patches important?
- Why should software come from trusted repositories?

**Expected outcome:**

You should understand the importance of software maintenance and security updates.

Do not perform a full distribution upgrade during this assessment.

### 13. Task 10 – Create a Security Assessment Script

**Marks: 5**

Create:

```bash
nano final-security-assessment.sh
```

Enter:

```bash
#!/bin/bash

LAB="$HOME/practical-labs/module06/lab10"
REPORT="$LAB/final-security-assessment-report.txt"

mkdir -p "$LAB"

{
    echo "======================================"
    echo " LINUX FINAL SECURITY ASSESSMENT"
    echo "======================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo "User: $(whoami)"
    echo

    echo "SYSTEM INFORMATION"
    cat /etc/os-release
    uname -r
    uname -m
    uptime
    free -h
    df -h
    echo

    echo "USER AND GROUP INFORMATION"
    id
    getent group sudo
    echo

    echo "PASSWORD AGING"
    chage -l "$(whoami)"
    echo

    echo "SUDO CONFIGURATION"
    sudo visudo -c
    sudo -l
    echo

    echo "SSH STATUS"
    systemctl status ssh --no-pager 2>&1 | head -15
    if command -v sshd >/dev/null 2>&1; then
        sudo sshd -T 2>/dev/null |
            grep -E 'permitrootlogin|passwordauthentication|pubkeyauthentication|maxauthtries|port'
    else
        echo "SSH server binary unavailable."
    fi
    echo

    echo "FILESYSTEM SECURITY"
    stat /etc/passwd /etc/shadow /etc/sudoers
    umask
    ls -ld /tmp
    echo

    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running --no-pager
    echo

    echo "FAILED SERVICES"
    systemctl --failed --no-pager
    echo

    echo "NETWORK LISTENERS"
    sudo ss -tulpn
    echo

    echo "PACKAGE UPDATES"
    apt list --upgradable 2>/dev/null
    echo

    echo "RECENT SECURITY EVENTS"
    sudo journalctl -b --no-pager |
        grep -Ei 'authentication|sudo|ssh|failed|denied' |
        tail -40

    echo
    echo "======================================"
    echo "ASSESSMENT COMPLETE"
    echo "======================================"

} > "$REPORT" 2>&1

echo "Assessment report generated:"
echo "$REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x final-security-assessment.sh
```

Run:

```bash
./final-security-assessment.sh
```

Inspect the generated report:

```bash
less final-security-assessment-report.txt
```

Press `q` to exit.

### 14. Task 11 – Professional Security Report

**Marks: 10**

Create:

```bash
nano module06-final-report.txt
```

Use the following structure:

```text
=========================================
MODULE 06 – LINUX SECURITY
FINAL PRACTICAL ASSESSMENT REPORT
=========================================

STUDENT INFORMATION

Student Name:
Course:
Module:
Assessment Date:

1. SYSTEM INFORMATION

Operating System:
Kernel:
Architecture:
Hostname:

2. ACCOUNT SECURITY

Current User:
Administrative Groups:
Password Aging Reviewed:
Shadow File Permissions Reviewed:

3. PRIVILEGE SECURITY

Sudo Configuration Valid:
Sudo Privileges Reviewed:
Least Privilege Explained:

4. SSH SECURITY

SSH Client Available:
SSH Server Installed:
SSH Service Status:
SSH Configuration Reviewed:

5. FILESYSTEM SECURITY

Sensitive File Permissions Reviewed:
Umask:
Sticky Bit:
SUID/SGID Inspection:

6. SERVICE SECURITY

Running Services Reviewed:
Failed Services:
Enabled Services Reviewed:

7. NETWORK SECURITY

Network Interfaces Reviewed:
Listening TCP Sockets:
Listening UDP Sockets:

8. LOG ANALYSIS

System Logs Reviewed:
Authentication Events Reviewed:
Important Observations:

9. SOFTWARE SECURITY

Package Metadata Updated:
Available Updates Reviewed:

10. SECURITY FINDINGS

Finding 1:
Risk:
Recommendation:

Finding 2:
Risk:
Recommendation:

Finding 3:
Risk:
Recommendation:

11. SECURITY RECOMMENDATIONS

1.
2.
3.
4.
5.

12. CONCLUSION

Summarize the security condition of the assessed
Linux virtual machine.

=========================================
ASSESSMENT STATUS:
COMPLETED / NOT COMPLETED
=========================================
```

Do not invent findings. If you identify no confirmed weakness in a particular area, record that the relevant control was inspected and no issue was confirmed.

### 15. Assessment Marking Scheme

| Assessment Task | Marks |
|---|---:|
| System security inventory | 10 |
| User and account security | 10 |
| Sudo and privilege management | 10 |
| SSH security assessment | 10 |
| Filesystem security assessment | 10 |
| Linux service security | 10 |
| Network exposure assessment | 10 |
| Security log analysis | 10 |
| Software security and updates | 5 |
| Security assessment script | 5 |
| Professional security report | 10 |
| **Total** | **100** |

### 16. Final Submission Checklist

- [ ] Completed all 11 assessment tasks.
- [ ] Generated the final security assessment script.
- [ ] Generated the final security assessment report.
- [ ] Created the professional report.
- [ ] Documented security findings.
- [ ] Provided security recommendations.
- [ ] Confirmed no unnecessary system configuration changes were made.

**MODULE 06 STATUS: READY FOR FINAL ASSESSMENT**

Congratulations on reaching the final assessment of Linux Security.
