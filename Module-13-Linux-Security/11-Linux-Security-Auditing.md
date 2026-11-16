
# Module 13 – Lesson 11: Linux Security Auditing

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define Linux security auditing.
2. Explain why security audits are important.
3. Identify common Linux security weaknesses.
4. Inspect users, groups, and privileges.
5. Examine file permissions and ownership.
6. Inspect running services and network ports.
7. Review authentication and system logs.
8. Understand Linux auditing tools.
9. Generate a basic security audit report.
10. Explain how security auditing supports penetration testing and system administration.

---

## 2. What Is Security Auditing?

Security auditing is the systematic examination of a computer system to determine whether its security controls are properly configured and operating as intended.

In Linux, a security audit examines areas such as:

- User accounts.
- Password policies.
- File permissions.
- Administrative privileges.
- Running services.
- Network connections.
- Firewall configurations.
- System logs.
- Installed software.
- Security updates.

The purpose is to identify weaknesses before attackers exploit them.

### Simple Analogy

Imagine inspecting a university building.

A security auditor checks:

- Who has access to the building?
- Are important rooms locked?
- Are security cameras working?
- Are visitors monitored?
- Are emergency exits protected?

A Linux security audit asks similar questions about a computer system.

---

## 3. Why Security Auditing Is Important

Security auditing helps organisations:

1. Identify misconfigurations.
2. Discover unnecessary privileges.
3. Detect suspicious activities.
4. Identify outdated software.
5. Protect sensitive information.
6. Improve system security.
7. Support regulatory and organisational requirements.
8. Establish security baselines.
9. Investigate security incidents.
10. Reduce cybersecurity risks.

### Real-World Applications

Security auditing is used by:

- System administrators.
- Security analysts.
- Penetration testers.
- Security engineers.
- Digital forensic investigators.
- IT auditors.

---

## 4. Types of Security Audits

### 4.1 Configuration Audit

Examines whether system settings follow approved security requirements.

Examples:

- SSH configuration.
- Firewall configuration.
- Password policies.
- File permissions.

### 4.2 User and Privilege Audit

Examines user accounts and their permissions.

Examples:

- Administrator accounts.
- Sudo privileges.
- Unused accounts.
- Group memberships.

### 4.3 Network Security Audit

Examines network exposure.

Examples:

- Listening ports.
- Running network services.
- Firewall rules.
- Unnecessary network protocols.

### 4.4 Software and Patch Audit

Examines installed software and update status.

Examples:

- Outdated packages.
- Unsupported software.
- Known vulnerabilities.
- Package repository configuration.

### 4.5 Log Audit

Examines system records for suspicious or unusual activity.

Examples:

- Failed login attempts.
- Successful authentication.
- Service failures.
- Administrative commands.

---

## 5. Understanding the Security Audit Process

A security audit follows a structured process.

### Phase 1: Define Scope

Identify the system being audited.

Examples:

- Personal Linux computer.
- University server.
- Web server.
- Internal network machine.

Only audit systems you own or have explicit permission to examine.

### Phase 2: Gather Information

Collect information about:

- Operating system.
- Kernel version.
- Users.
- Groups.
- Installed software.
- Services.
- Network interfaces.

### Phase 3: Examine Security Controls

Inspect:

- Permissions.
- Authentication.
- Firewall.
- SSH.
- Logging.
- Security policies.

### Phase 4: Identify Findings

Record weaknesses and unusual configurations.

### Phase 5: Assess Risk

Determine the potential impact and likelihood of each finding.

### Phase 6: Recommend Improvements

Suggest appropriate corrective actions.

### Phase 7: Prepare the Report

Document findings, evidence, risks, and recommendations.

### Audit Workflow

```text
Define Scope
     |
     v
Gather Information
     |
     v
Inspect Security Controls
     |
     v
Identify Findings
     |
     v
Assess Risk
     |
     v
Recommend Improvements
     |
     v
Prepare Audit Report
```

---

## 6. Auditing Linux User Accounts

User accounts are important audit targets.

An attacker who compromises an account may gain access to sensitive information or system resources.

### Display User Accounts

```bash
getent passwd
```

### Display Groups

```bash
getent group
```

### Inspect Sudo Group Membership

```bash
getent group sudo
```

### Identify Accounts with Login Shells

```bash
awk -F: '$7 !~ /(nologin|false)$/ {print $1, $7}' /etc/passwd
```

### What Should an Auditor Look For?

- Unexpected user accounts.
- Unnecessary administrator privileges.
- Accounts belonging to former users.
- Shared accounts.
- Accounts with inappropriate login access.

Remember that an account with a login shell is not automatically suspicious. Investigate its purpose.

---

## 7. Auditing File Permissions

File permissions determine who can read, modify, or execute files.

Poor permissions can expose sensitive information.

### Inspect Password-Related Files

```bash
ls -l /etc/passwd /etc/shadow /etc/group
```

### Inspect SSH Configuration

```bash
ls -l /etc/ssh/sshd_config
```

### Find World-Writable Files in Your Own Home Directory

```bash
find "$HOME" -type f -perm -0002 -print
```

This searches your home directory, not the entire system.

### What Should an Auditor Look For?

- Sensitive files accessible to unnecessary users.
- Unexpected world-writable files.
- Incorrect file ownership.
- Unusual SUID and SGID programs.
- Exposed SSH private keys.

A permission that appears unusual must be investigated before being classified as a vulnerability.

---

## 8. Auditing Running Services

Services operate in the background and provide different system functions.

Unnecessary services can increase the attack surface.

### Display Running Services

```bash
systemctl list-units --type=service --state=running --no-pager
```

### Display Failed Services

```bash
systemctl --failed
```

### Inspect a Specific Service

```bash
systemctl status ssh --no-pager
```

### Auditor's Checklist

- Is the service required?
- Is it correctly configured?
- Does it need network access?
- Is it running with excessive privileges?
- Are its logs being monitored?
- Is the software supported and updated?

---

## 9. Auditing Network Exposure

Network auditing helps identify services that accept incoming connections.

### Inspect Listening TCP and UDP Ports

```bash
sudo ss -tulpen
```

### Inspect Network Interfaces

```bash
ip -br addr
```

### Inspect Routing Information

```bash
ip route
```

### Understand the Output

The `ss` command displays socket information.

Important options:

| Option | Meaning |
|---|---|
| `-t` | TCP sockets |
| `-u` | UDP sockets |
| `-l` | Listening sockets |
| `-p` | Process information |
| `-e` | Extended information |
| `-n` | Numeric addresses and ports |

An open listening port is not automatically a vulnerability. Determine which application owns it and whether it should be exposed.

---

## 10. Auditing Firewall Configuration

Firewalls control network traffic according to security rules.

An auditor checks whether firewall policies provide appropriate protection.

### Inspect UFW

```bash
sudo ufw status verbose
```

### Inspect nftables Rules

```bash
sudo nft list ruleset
```

### Inspect iptables Rules

```bash
sudo iptables -S
```

Not all firewall tools are necessarily installed or active.

### Audit Questions

1. Is a firewall present?
2. Is it active?
3. Are unnecessary inbound connections restricted?
4. Are required services allowed?
5. Is IPv6 traffic considered?
6. Are firewall rules documented?

Do not change firewall settings during this practical.

---

## 11. Auditing System Logs

Logs provide evidence about activities performed on a Linux system.

They are important for detecting and investigating security incidents.

### View Recent System Logs

```bash
sudo journalctl -n 50 --no-pager
```

### View Authentication-Related Logs

```bash
sudo journalctl _COMM=sshd --no-pager -n 30
```

This command may return no results if the SSH daemon is not running or the relevant records are unavailable.

### View Failed Systemd Units

```bash
systemctl --failed
```

### Common Log Locations

| Location | Purpose |
|---|---|
| `/var/log/auth.log` | Authentication events on some Debian-based systems |
| `/var/log/syslog` | General system messages on some systems |
| `/var/log/kern.log` | Kernel messages on some systems |
| `/var/log/secure` | Authentication events on some Red Hat-based systems |
| `journalctl` | Queries the systemd journal |

Log locations depend on the distribution and logging configuration.

### Suspicious Events to Investigate

- Repeated failed logins.
- Unexpected administrator activity.
- Unknown services starting.
- Unusual system errors.
- Unexpected changes to sensitive files.

A failed login alone does not prove an attack occurred.

---

## 12. Auditing Installed Software

Outdated or unnecessary software may introduce security risks.

### List Installed Packages

```bash
dpkg -l
```

### Search for Upgradable Packages

```bash
apt list --upgradable
```

### Simulate an Upgrade

```bash
sudo apt-get -s upgrade
```

The simulation displays what an upgrade would do without installing the updates.

### Audit Questions

- Is the software necessary?
- Is it supported?
- Is it outdated?
- Does it have known vulnerabilities?
- Does it come from a trusted repository?

---

## 13. Linux Security Auditing Tools

Several tools help administrators perform security audits.

### 13.1 Lynis

Lynis is a security auditing tool for Unix-like systems.

It examines system configurations and provides security recommendations.

Typical areas include:

- Authentication.
- File permissions.
- Kernel settings.
- Logging.
- Firewall configuration.
- Installed software.

### 13.2 Linux Audit Framework (auditd)

The Linux Audit Framework records selected security-related system events.

It can help administrators investigate:

- File access.
- System calls.
- Authentication events.
- Changes to monitored files.
- Administrative activity.

### 13.3 AIDE

AIDE stands for Advanced Intrusion Detection Environment.

It checks file integrity by comparing current files with a previously established baseline.

It can help detect unexpected changes to monitored files.

### 13.4 OpenSCAP

OpenSCAP is a framework for assessing systems against defined security policies and compliance requirements.

### 13.5 Fail2ban

Fail2ban monitors selected log files and can apply protective actions when repeated suspicious behaviour is detected.

It is commonly used to help protect services against repeated authentication attempts.

### Tool Comparison

| Tool | Main Purpose |
|---|---|
| Lynis | Security auditing |
| auditd | Security event auditing |
| AIDE | File integrity monitoring |
| OpenSCAP | Security configuration assessment |
| Fail2ban | Automated response to selected suspicious activity |

These tools have different purposes and are not interchangeable.

---

## 14. Risk Assessment During an Audit

Not every finding has the same level of risk.

An auditor must prioritise findings.

### Basic Risk Formula

Risk can be estimated using:

Risk = Likelihood × Impact

For a simple classroom model, assign each value from 1 to 5.

### Example

| Finding | Likelihood | Impact | Score |
|---|---:|---:|---:|
| Weak administrator password | 5 | 5 | 25 |
| Unnecessary exposed service | 3 | 4 | 12 |
| Missing security documentation | 2 | 2 | 4 |

### Example Risk Categories

| Score | Risk |
|---|---|
| 1–5 | Low |
| 6–10 | Moderate |
| 11–15 | High |
| 16–25 | Critical |

This is a simplified educational scoring model, not a formal CVSS calculation.

### Important Principle

Prioritise findings according to their actual context, exploitability, exposure, and potential impact.

---

## 15. Writing a Security Audit Report

A professional security audit report should contain:

1. Audit title.
2. Audit date.
3. System information.
4. Audit scope.
5. Tools used.
6. Evidence collected.
7. Security findings.
8. Risk ratings.
9. Recommendations.
10. Limitations.
11. Conclusion.

### Example Finding

**Finding:** Unnecessary network service is listening.

**Evidence:** A listening socket was identified during the network inspection.

**Risk:** The service may expose functionality that is not required.

**Recommendation:** Identify the application, confirm business requirements, and restrict or remove unnecessary exposure through an approved change process.

A finding must be supported by evidence. Do not label a service vulnerable merely because it is listening.

---

# 16. Practical Lab: Linux Security Audit

## Lab Objective

Perform a basic security audit of your Kali Linux VM.

You will inspect the system and create an audit report.

This practical is inspection-only.

Do not modify system settings or test against machines you do not own.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module13-lab/lesson11
cd ~/module13-lab/lesson11
```

### Step 2: Collect System Information

```bash
{
    echo "SYSTEM INFORMATION"
    date
    hostname
    uname -a
    cat /etc/os-release
} > security-audit-report.txt
```

### Step 3: Audit User Accounts

```bash
{
    echo
    echo "USER ACCOUNTS"
    getent passwd
    echo
    echo "GROUPS"
    getent group
    echo
    echo "SUDO GROUP"
    getent group sudo
} >> security-audit-report.txt
```

### Step 4: Audit File Permissions

```bash
{
    echo
    echo "IMPORTANT FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow /etc/group
    echo
    echo "SSH CONFIGURATION"
    ls -l /etc/ssh/sshd_config
    echo
    echo "WORLD-WRITABLE FILES IN HOME DIRECTORY"
    find "$HOME" -type f -perm -0002 -print
} >> security-audit-report.txt 2>&1
```

### Step 5: Audit Running Services

```bash
{
    echo
    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running --no-pager
    echo
    echo "FAILED SERVICES"
    systemctl --failed
} >> security-audit-report.txt 2>&1
```

### Step 6: Audit Network Exposure

```bash
{
    echo
    echo "LISTENING NETWORK SOCKETS"
    sudo ss -tulpen
    echo
    echo "NETWORK INTERFACES"
    ip -br addr
    echo
    echo "ROUTES"
    ip route
} >> security-audit-report.txt 2>&1
```

### Step 7: Audit Firewall Configuration

```bash
{
    echo
    echo "UFW STATUS"
    sudo ufw status verbose
    echo
    echo "NFTABLES RULES"
    sudo nft list ruleset
} >> security-audit-report.txt 2>&1
```

### Step 8: Audit System Logs

```bash
{
    echo
    echo "RECENT SYSTEM LOGS"
    sudo journalctl -n 50 --no-pager
    echo
    echo "FAILED SERVICES"
    systemctl --failed
} >> security-audit-report.txt 2>&1
```

### Step 9: Audit Software Updates

```bash
{
    echo
    echo "UPGRADABLE PACKAGES"
    apt list --upgradable
    echo
    echo "SIMULATED PACKAGE UPGRADE"
    sudo apt-get -s upgrade
} >> security-audit-report.txt 2>&1
```

### Step 10: Inspect Your Report

```bash
less security-audit-report.txt
```

Press `q` to exit `less`.

Check the file:

```bash
ls -lh security-audit-report.txt
```

### Expected Output

```text
lesson11/
└── security-audit-report.txt
```

---

## 17. Practical Analysis Questions

After generating your report, answer these questions:

1. What Linux distribution are you auditing?
2. How many user accounts are listed?
3. Which accounts have administrative privileges?
4. What important file permissions did you inspect?
5. Which services are running?
6. Which network ports are listening?
7. Is UFW available and active?
8. Are there failed systemd services?
9. Are there packages awaiting updates?
10. What three areas would you investigate further?

Do not assume a system is secure simply because the audit commands returned no errors.

---

## 18. Knowledge Check

1. Define security auditing.
2. Explain five reasons security auditing is important.
3. What is a configuration audit?
4. What is a user and privilege audit?
5. Describe the seven phases of a security audit.
6. How can an administrator inspect Linux user accounts?
7. Why are file permissions important during an audit?
8. How can an administrator inspect listening ports?
9. What is the purpose of system logs?
10. What is Lynis?
11. Explain the purpose of auditd.
12. What is AIDE?
13. What is OpenSCAP?
14. What is Fail2ban?
15. Explain the relationship between likelihood and impact.
16. Why should audit findings contain evidence?
17. What information belongs in a security audit report?
18. Why is an open port not automatically a vulnerability?

---

## 19. Practical Completion Checklist

- [ ] I understand security auditing.
- [ ] I understand the different audit types.
- [ ] I understand the security audit process.
- [ ] I inspected user accounts and groups.
- [ ] I inspected important file permissions.
- [ ] I inspected running services.
- [ ] I inspected listening network ports.
- [ ] I inspected firewall configuration.
- [ ] I reviewed system logs.
- [ ] I inspected installed software and updates.
- [ ] I understand Lynis, auditd, AIDE, OpenSCAP, and Fail2ban.
- [ ] I understand basic risk assessment.
- [ ] I generated my security audit report.

**Lesson 11 Complete: Linux Security Auditing.**
