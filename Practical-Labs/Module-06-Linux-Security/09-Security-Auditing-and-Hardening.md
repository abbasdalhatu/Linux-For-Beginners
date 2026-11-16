
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 09: SECURITY AUDITING AND HARDENING

### 1. Introduction

Linux security auditing is the systematic examination of a Linux computer to identify security weaknesses, misconfigurations, unnecessary privileges, outdated software, and suspicious activities.

Linux hardening is the process of improving system security by reducing unnecessary exposure and strengthening configurations.

In this lab, you will combine concepts from:

- Linux security fundamentals.
- Patch management.
- User account security.
- Password policies.
- Sudo and privilege management.
- SSH security.
- Filesystem security.
- Service security.

You will build a consolidated security audit tool and produce a professional security report.

### 2. Understand the Security Auditing Process

A basic security audit follows these stages:

1. Define the audit scope.
2. Gather system information.
3. Inspect users and privileges.
4. Review authentication controls.
5. Inspect SSH configuration.
6. Examine filesystem permissions.
7. Review running services.
8. Inspect listening network sockets.
9. Review software update status.
10. Document findings.
11. Recommend improvements.
12. Verify the results.

The purpose is to identify and document security risks without unnecessarily disrupting the system.

### 3. Create Your Lab Directory

Run:

```bash
mkdir -p ~/practical-labs/module06/lab09
cd ~/practical-labs/module06/lab09
```

### 4. Define the Audit Scope

For this exercise, your audit scope is your own Kali Linux virtual machine.

Your audit must not include:

- Other people's computers.
- Public IP addresses.
- University networks.
- Systems without authorization.
- External servers.

Record:

```text
Audit Target: Personal Kali Linux Virtual Machine
Audit Type: Local Security Assessment
Testing Method: Read-only inspection
Authorization: System owner
```

### 5. Perform a System Inventory

Inspect the operating system:

```bash
cat /etc/os-release
```

Inspect the kernel:

```bash
uname -r
```

Inspect the hostname:

```bash
hostname
```

Inspect uptime:

```bash
uptime
```

Inspect system architecture:

```bash
uname -m
```

Inspect memory:

```bash
free -h
```

Inspect disk space:

```bash
df -h
```

Record the operating system, kernel, architecture, memory, and available storage.

### 6. Audit User Accounts

Inspect account information without exposing password hashes:

```bash
getent passwd | cut -d: -f1,3,4,6,7
```

Inspect administrative group membership:

```bash
getent group sudo
```

Inspect your current identity:

```bash
whoami
id
```

Inspect your sudo permissions:

```bash
sudo -l
```

Review:

- Unexpected accounts.
- Accounts with unusual shells.
- Unnecessary administrative privileges.
- Accounts that should no longer exist.

Do not delete or modify accounts during this audit.

### 7. Audit Password and Authentication Policies

Inspect password aging information for your own account:

```bash
chage -l "$(whoami)"
```

Inspect password policy defaults:

```bash
grep -E '^(PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE)' /etc/login.defs
```

Inspect relevant PAM configuration filenames:

```bash
ls -l /etc/pam.d/common-auth /etc/pam.d/common-password 2>/dev/null
```

Review the existence and permissions of the shadow database:

```bash
stat /etc/shadow
```

Never copy password hashes into your report.

### 8. Audit Sudo Configuration

Validate sudo configuration syntax:

```bash
sudo visudo -c
```

Inspect the sudo configuration directory:

```bash
sudo ls -la /etc/sudoers.d/
```

Inspect your authorized privileges:

```bash
sudo -l
```

Review whether administrative access follows the principle of least privilege.

Do not modify sudoers files during this exercise.

### 9. Audit SSH Security

Check whether the SSH server is installed:

```bash
dpkg -s openssh-server 2>/dev/null | grep -E '^(Package|Status|Version)'
```

Check the SSH service:

```bash
systemctl status ssh --no-pager
```

Inspect listening sockets:

```bash
sudo ss -ltnp
```

If the SSH server binary exists, inspect selected effective settings:

```bash
sudo sshd -T 2>/dev/null | grep -E 'permitrootlogin|passwordauthentication|pubkeyauthentication|maxauthtries|port'
```

Record unavailable components as not installed or not available.

Do not expose SSH to external networks.

### 10. Audit Filesystem Security

Inspect the permissions of important system files:

```bash
stat /etc/passwd
stat /etc/shadow
stat /etc/sudoers
```

Inspect your home directory:

```bash
stat ~
```

Search your lab directory for world-writable files:

```bash
find ~/practical-labs/module06/lab09 -type f -perm -0002 -ls
```

Search for SUID files in the lab directory:

```bash
find ~/practical-labs/module06/lab09 -type f -perm -4000 -ls
```

Search for SGID files in the lab directory:

```bash
find ~/practical-labs/module06/lab09 -type f -perm -2000 -ls
```

Record the findings.

### 11. Audit Running Services

List running services:

```bash
systemctl list-units --type=service --state=running --no-pager
```

List enabled service files:

```bash
systemctl list-unit-files --type=service --state=enabled --no-pager
```

Inspect failed services:

```bash
systemctl --failed --no-pager
```

Review:

- Services that are necessary.
- Unexpected failed services.
- Unnecessary services.
- Services requiring further investigation.

Do not disable any service based only on its name.

### 12. Audit Network Exposure

Inspect TCP listening sockets:

```bash
sudo ss -ltnp
```

Inspect UDP listening sockets:

```bash
sudo ss -lunp
```

Inspect network interfaces:

```bash
ip -br addr
```

Inspect routing:

```bash
ip route
```

Look for:

- Unexpected listening ports.
- Services bound to all interfaces.
- Unnecessary network listeners.
- Services that should only be accessible locally.

A listening socket is not automatically an externally accessible service. Firewall rules and network topology also matter.

### 13. Audit Software Updates

Refresh package metadata:

```bash
sudo apt update
```

List available package updates:

```bash
apt list --upgradable
```

Inspect APT history:

```bash
sudo tail -30 /var/log/apt/history.log
```

Record:

- Whether updates are available.
- Whether security-related packages require attention.
- Whether the system has been updated recently.

Do not perform a full distribution upgrade as part of this lab.

### 14. Audit Security Logs

Inspect recent system events:

```bash
sudo journalctl -b -n 50 --no-pager
```

Inspect authentication-related events:

```bash
sudo journalctl -b --no-pager | grep -Ei 'authentication|sudo|ssh|failed|denied' | tail -40
```

Inspect failed services:

```bash
sudo journalctl -p err -b --no-pager
```

Look for unusual events, repeated failures, unexpected service restarts, and permission errors.

Do not assume every warning represents an attack.

### 15. Build a Consolidated Security Audit Script

Create:

```bash
nano linux-security-audit.sh
```

Enter:

```bash
#!/bin/bash

LAB="$HOME/practical-labs/module06/lab09"
REPORT="$LAB/linux-security-audit-report.txt"

mkdir -p "$LAB"

{
    echo "======================================"
    echo "       LINUX SECURITY AUDIT"
    echo "======================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo "Audited User: $(whoami)"
    echo

    echo "1. SYSTEM INFORMATION"
    echo "----------------------"
    cat /etc/os-release
    uname -r
    uname -m
    uptime
    free -h
    df -h
    echo

    echo "2. USER AND GROUP SECURITY"
    echo "--------------------------"
    id
    getent group sudo
    echo

    echo "3. PASSWORD POLICY"
    echo "------------------"
    grep -E '^(PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE)' /etc/login.defs
    chage -l "$(whoami)"
    echo

    echo "4. SUDO SECURITY"
    echo "----------------"
    sudo visudo -c
    sudo -l
    echo

    echo "5. SSH SECURITY"
    echo "---------------"
    dpkg -s openssh-server 2>/dev/null |
        grep -E '^(Package|Status|Version)'
    systemctl status ssh --no-pager 2>&1 | head -15

    if command -v sshd >/dev/null 2>&1; then
        sudo sshd -T 2>/dev/null |
            grep -E 'permitrootlogin|passwordauthentication|pubkeyauthentication|maxauthtries|port'
    else
        echo "SSH server binary not installed."
    fi
    echo

    echo "6. SENSITIVE FILE PERMISSIONS"
    echo "-----------------------------"
    for FILE in /etc/passwd /etc/shadow /etc/sudoers; do
        if [ -e "$FILE" ]; then
            stat -c '%A %a %U:%G %n' "$FILE"
        fi
    done
    echo

    echo "7. LAB DIRECTORY PERMISSION AUDIT"
    echo "---------------------------------"
    find "$LAB" -type f -perm -0002 -ls
    echo "SUID files:"
    find "$LAB" -type f -perm -4000 -ls
    echo "SGID files:"
    find "$LAB" -type f -perm -2000 -ls
    echo

    echo "8. RUNNING SERVICES"
    echo "-------------------"
    systemctl list-units --type=service --state=running --no-pager
    echo

    echo "9. FAILED SERVICES"
    echo "------------------"
    systemctl --failed --no-pager
    echo

    echo "10. NETWORK LISTENERS"
    echo "---------------------"
    sudo ss -tulpn
    echo

    echo "11. AVAILABLE PACKAGE UPDATES"
    echo "-----------------------------"
    apt list --upgradable 2>/dev/null
    echo

    echo "12. RECENT SECURITY-RELEVANT EVENTS"
    echo "-----------------------------------"
    sudo journalctl -b --no-pager |
        grep -Ei 'authentication|sudo|ssh|failed|denied' |
        tail -40

    echo
    echo "======================================"
    echo "           AUDIT COMPLETE"
    echo "======================================"

} > "$REPORT" 2>&1

echo "Security audit completed."
echo "Report saved to: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x linux-security-audit.sh
```

Run:

```bash
./linux-security-audit.sh
```

Inspect:

```bash
less linux-security-audit-report.txt
```

Press `q` to exit.

### 16. Understand Security Risk Ratings

Professional security assessments commonly classify findings according to risk.

| Risk | Meaning |
|---|---|
| Critical | Severe weakness requiring immediate attention |
| High | Significant security weakness |
| Medium | Meaningful weakness requiring remediation |
| Low | Limited security impact |
| Informational | Observation without an identified direct security weakness |

Risk depends on:

- Likelihood of exploitation.
- Potential impact.
- Exposure.
- Existing security controls.
- Business importance of the affected system.

Do not assign a high risk rating simply because a configuration looks unfamiliar.

### 17. Create a Security Findings Register

Create:

```bash
nano security-findings.txt
```

Use this template:

```text
LINUX SECURITY FINDINGS REGISTER

Finding ID:
Finding Title:
Affected Component:
Description:
Evidence:
Risk Rating:
Potential Impact:
Recommended Remediation:
Verification Method:
Status:
```

Record at least three observations from your audit.

If you do not identify a weakness, document the security control you inspected and why it appears appropriate.

Do not invent vulnerabilities.

### 18. Develop a Linux Hardening Checklist

Review these recommendations:

**Account Security**
- Remove unnecessary accounts through an approved administrative process.
- Restrict administrative privileges.
- Use strong authentication.
- Review password aging policies.

**SSH Security**
- Use key-based authentication where appropriate.
- Protect private keys.
- Restrict root access.
- Monitor authentication events.

**Filesystem Security**
- Apply least-privilege permissions.
- Protect sensitive configuration files.
- Review SUID and SGID executables.
- Inspect ACLs.

**Service Security**
- Identify unnecessary services.
- Review listening network sockets.
- Monitor service failures.
- Apply appropriate systemd restrictions.

**Software Security**
- Install security updates.
- Use trusted package repositories.
- Review installed software.
- Remove unnecessary software through a controlled process.

**Monitoring**
- Review authentication logs.
- Monitor service failures.
- Investigate unusual activity.
- Preserve useful audit evidence.

### 19. Create Your Final Audit Summary

Create:

```bash
nano lab09-report.txt
```

Record:

```text
MODULE 06 – LAB 09
SECURITY AUDITING AND HARDENING

AUDIT INFORMATION
Target:
Operating System:
Kernel Version:
Audit Date:
Audit Method:

SYSTEM SECURITY
1. Current user:
2. Administrative group:
3. Sudo configuration valid:
4. Password policy inspected:
5. SSH server status:
6. Sensitive file permissions inspected:

FILESYSTEM SECURITY
7. World-writable files found:
8. SUID/SGID observations:
9. ACL inspection completed:

SERVICE SECURITY
10. Running services inspected:
11. Failed services observed:
12. Listening sockets inspected:

NETWORK SECURITY
13. Network interfaces inspected:
14. Routing information inspected:
15. Unexpected listeners requiring investigation:

SOFTWARE SECURITY
16. Package metadata refreshed:
17. Updates available:

LOG ANALYSIS
18. Authentication events reviewed:
19. Service errors reviewed:

SECURITY FINDINGS
20. Finding 1:
21. Finding 2:
22. Finding 3:

RECOMMENDATIONS
23. Recommendation 1:
24. Recommendation 2:
25. Recommendation 3:

AUDIT REPORT:
Generated successfully: YES / NO

SYSTEM CONFIGURATION MODIFIED:
NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 20. Completion Checklist

- [ ] Defined the audit scope.
- [ ] Inspected operating system information.
- [ ] Audited user accounts.
- [ ] Reviewed authentication policies.
- [ ] Validated sudo configuration.
- [ ] Inspected SSH security.
- [ ] Reviewed sensitive file permissions.
- [ ] Inspected filesystem permissions.
- [ ] Audited running services.
- [ ] Checked failed services.
- [ ] Inspected network listeners.
- [ ] Reviewed software updates.
- [ ] Examined security-related logs.
- [ ] Created a consolidated audit script.
- [ ] Generated the security audit report.
- [ ] Documented at least three observations.
- [ ] Prepared a hardening checklist.
- [ ] Completed the final audit summary.

**LAB 09 STATUS: READY FOR PRACTICAL COMPLETION**
