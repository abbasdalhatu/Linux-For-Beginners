
# Module 13 – Lesson 14: Practical Linux Security Projects

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Apply Linux security concepts to practical projects.
2. Perform a basic security assessment.
3. Build a file-integrity monitoring demonstration.
4. Create a security inspection script.
5. Understand centralised security monitoring.
6. Design a basic incident-response procedure.
7. Document security findings professionally.
8. Understand how Linux security projects relate to cybersecurity careers.

---

# PROJECT 1: Linux Security Assessment Toolkit

## 2. Project Overview

You will create a small Linux security assessment toolkit that collects information about a Linux system.

The toolkit will inspect:

- Operating system information.
- Kernel version.
- User accounts.
- Sudo group membership.
- Running services.
- Listening network ports.
- Important file permissions.
- Firewall status.
- Available security updates.

### Project Objective

Build a simple tool that helps a system administrator understand the security condition of a Linux computer.

### Step 1: Create the Project Directory

```bash
mkdir -p ~/module13-lab/lesson14/project1
cd ~/module13-lab/lesson14/project1
```

### Step 2: Create the Script

```bash
nano security-assessment.sh
```

Enter:

```bash
#!/bin/bash

# Linux Security Assessment Toolkit
# Educational inspection tool

REPORT="security-assessment-report.txt"

{
    echo "======================================"
    echo "       LINUX SECURITY ASSESSMENT"
    echo "======================================"

    echo
    echo "DATE"
    date

    echo
    echo "HOSTNAME"
    hostname

    echo
    echo "OPERATING SYSTEM"
    cat /etc/os-release

    echo
    echo "KERNEL VERSION"
    uname -r

    echo
    echo "USER ACCOUNTS"
    getent passwd

    echo
    echo "SUDO GROUP"
    getent group sudo

    echo
    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running --no-pager

    echo
    echo "LISTENING PORTS"
    ss -tuln

    echo
    echo "IMPORTANT FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow /etc/group

    echo
    echo "FIREWALL STATUS"

    if command -v ufw >/dev/null 2>&1; then
        sudo ufw status verbose
    else
        echo "UFW is not installed."
    fi

    echo
    echo "UPGRADABLE PACKAGES"
    apt list --upgradable

    echo
    echo "ASSESSMENT COMPLETED"

} > "$REPORT" 2>&1

echo "Security assessment completed."
echo "Report saved to: $REPORT"
```

Save and exit Nano:

- `Ctrl + O`
- Press `Enter`
- `Ctrl + X`

### Step 3: Make the Script Executable

```bash
chmod +x security-assessment.sh
```

### Step 4: Execute the Script

```bash
./security-assessment.sh
```

Some sections may require sudo privileges. The script uses sudo only for the firewall inspection.

### Step 5: Inspect the Report

```bash
less security-assessment-report.txt
```

Press `q` to exit.

### Expected Project Structure

```text
project1/
├── security-assessment.sh
└── security-assessment-report.txt
```

### What You Have Built

You have created a basic Linux security assessment tool.

This is an example of how Bash scripting can automate repetitive security administration tasks.

---

# PROJECT 2: File Integrity Monitoring System

## 3. Project Overview

File Integrity Monitoring (FIM) helps identify unexpected modifications to important files.

In this project, you will create a simple integrity-monitoring demonstration using SHA-256 hashes.

### Project Objective

Build a tool that:

1. Creates test files.
2. Generates SHA-256 hashes.
3. Stores a baseline.
4. Checks files against the baseline.
5. Reports detected changes.

This is a classroom demonstration, not a tamper-resistant production FIM system.

### Step 1: Create the Project Directory

```bash
mkdir -p ~/module13-lab/lesson14/project2
cd ~/module13-lab/lesson14/project2
```

### Step 2: Create Test Files

```bash
mkdir monitored-files
```

```bash
echo "University security policy" > monitored-files/policy.txt
```

```bash
echo "Approved server configuration" > monitored-files/server.txt
```

### Step 3: Generate a Baseline

```bash
sha256sum monitored-files/* > baseline.txt
```

Inspect it:

```bash
cat baseline.txt
```

### Step 4: Verify File Integrity

```bash
sha256sum -c baseline.txt
```

Expected output:

```text
monitored-files/policy.txt: OK
monitored-files/server.txt: OK
```

### Step 5: Simulate a File Modification

Modify one of your test files:

```bash
echo "Unauthorised test modification" >> monitored-files/policy.txt
```

### Step 6: Check Integrity Again

```bash
sha256sum -c baseline.txt
```

The modified file should produce a failure result.

### Step 7: Understand the Result

The hash has changed because the file contents changed.

This is the basic principle behind file-integrity monitoring.

### Important Security Limitation

If an attacker can modify both monitored files and the baseline, they may be able to conceal changes.

Production FIM systems should protect their baselines using appropriate access controls and trusted storage.

### Expected Project Structure

```text
project2/
├── baseline.txt
└── monitored-files/
    ├── policy.txt
    └── server.txt
```

---

# PROJECT 3: Linux Security Log Monitor

## 4. Project Overview

System logs contain information about events occurring on a Linux computer.

A security administrator can examine these logs to identify suspicious activity.

In this project, you will create a basic log-monitoring script.

### Project Objective

Create a script that:

- Collects recent system logs.
- Searches for selected security-related keywords.
- Saves matching entries to a report.

### Step 1: Create the Project Directory

```bash
mkdir -p ~/module13-lab/lesson14/project3
cd ~/module13-lab/lesson14/project3
```

### Step 2: Create the Script

```bash
nano log-monitor.sh
```

Enter:

```bash
#!/bin/bash

REPORT="log-monitor-report.txt"

{
    echo "================================"
    echo "       LINUX LOG MONITOR"
    echo "================================"

    echo
    echo "DATE"
    date

    echo
    echo "RECENT SYSTEM LOGS"
    sudo journalctl -n 100 --no-pager

    echo
    echo "SELECTED SECURITY-RELATED EVENTS"

    sudo journalctl -n 200 --no-pager |
        grep -Ei "failed|failure|denied|authentication|unauthorized" |
        tail -n 50

    echo
    echo "MONITORING COMPLETED"

} > "$REPORT" 2>&1

echo "Log monitoring completed."
echo "Report saved to: $REPORT"
```

### Step 3: Make It Executable

```bash
chmod +x log-monitor.sh
```

### Step 4: Execute It

```bash
./log-monitor.sh
```

### Step 5: Inspect the Report

```bash
less log-monitor-report.txt
```

### Important Limitation

Searching for keywords is a simple demonstration.

It does not understand the full context of events and may produce false positives or miss suspicious activity.

A professional monitoring platform uses additional analysis, correlation, and alerting.

---

# PROJECT 4: Linux User and Permission Audit

## 5. Project Overview

This project focuses on inspecting user accounts and important file permissions.

### Project Objective

Create a report showing:

- User accounts.
- Group membership.
- Sudo group membership.
- Important system file permissions.
- Accounts with login-capable shells.

### Step 1: Create the Project Directory

```bash
mkdir -p ~/module13-lab/lesson14/project4
cd ~/module13-lab/lesson14/project4
```

### Step 2: Create the Script

```bash
nano user-audit.sh
```

Enter:

```bash
#!/bin/bash

REPORT="user-audit-report.txt"

{
    echo "================================"
    echo "      LINUX USER AUDIT"
    echo "================================"

    echo
    echo "USER ACCOUNTS"
    getent passwd

    echo
    echo "GROUP INFORMATION"
    getent group

    echo
    echo "SUDO GROUP MEMBERS"
    getent group sudo

    echo
    echo "ACCOUNTS WITH LOGIN-CAPABLE SHELLS"
    awk -F: '$7 !~ /(nologin|false)$/ {print $1, $7}' /etc/passwd

    echo
    echo "IMPORTANT FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow /etc/group

    echo
    echo "AUDIT COMPLETED"

} > "$REPORT" 2>&1

echo "User audit completed."
echo "Report saved to: $REPORT"
```

### Step 3: Make It Executable

```bash
chmod +x user-audit.sh
```

### Step 4: Execute It

```bash
./user-audit.sh
```

### Step 5: Inspect the Report

```bash
less user-audit-report.txt
```

### Important Security Note

The script inspects account information but does not determine whether an account is malicious.

An administrator must investigate unfamiliar accounts before making decisions.

---

# PROJECT 5: Basic Linux Incident Response Plan

## 6. Project Overview

Incident response is the organised process of handling cybersecurity incidents.

Examples include:

- Malware infection.
- Unauthorised access.
- Data theft.
- Ransomware.
- Compromised user accounts.
- Suspicious network activity.

### Project Objective

Create a written incident-response procedure for a Linux server.

### The Six Main Incident-Response Stages

#### 1. Preparation

Before incidents occur:

- Establish security policies.
- Prepare monitoring tools.
- Maintain backups.
- Define responsibilities.
- Train administrators.

#### 2. Detection and Analysis

Identify and investigate suspicious activity.

Examples:

- Repeated failed logins.
- Unknown processes.
- Unusual network connections.
- Unexpected file modifications.

#### 3. Containment

Limit the incident's impact.

Examples:

- Isolate an affected system according to the response plan.
- Restrict compromised credentials.
- Protect unaffected systems.

#### 4. Eradication

Remove the cause of the incident.

Examples:

- Remove malicious components.
- Close exploited vulnerabilities.
- Correct insecure configurations.

#### 5. Recovery

Restore normal operations.

Examples:

- Restore trusted backups.
- Rebuild compromised systems when necessary.
- Apply security updates.
- Validate system operation.

#### 6. Lessons Learned

Review the incident.

Document:

- What happened.
- How it happened.
- What was affected.
- How it was handled.
- What should improve.

### Incident Response Workflow

```text
PREPARATION
     |
     v
DETECTION AND ANALYSIS
     |
     v
CONTAINMENT
     |
     v
ERADICATION
     |
     v
RECOVERY
     |
     v
LESSONS LEARNED
```

### Create Your Incident Response Document

```bash
mkdir -p ~/module13-lab/lesson14/project5
cd ~/module13-lab/lesson14/project5
nano incident-response-plan.txt
```

Enter:

```text
LINUX INCIDENT RESPONSE PLAN

1. PREPARATION
Maintain security tools, backups, policies and trained personnel.

2. DETECTION AND ANALYSIS
Identify suspicious activities and collect relevant evidence.

3. CONTAINMENT
Limit the spread and impact of the incident.

4. ERADICATION
Remove the confirmed cause of the compromise.

5. RECOVERY
Restore normal operations using trusted systems and backups.

6. LESSONS LEARNED
Document findings and improve security controls.

INCIDENT RESPONSE OWNER:
To be assigned.

INCIDENT DATE:
To be completed when an incident occurs.

INCIDENT STATUS:
Not applicable - educational exercise.
```

Save and exit.

---

# 7. Understanding Professional Security Documentation

Cybersecurity professionals must communicate their findings clearly.

A useful technical report contains:

1. Project title.
2. Objective.
3. Scope.
4. Tools used.
5. Methodology.
6. Evidence collected.
7. Findings.
8. Risk assessment.
9. Recommendations.
10. Conclusion.

### Example Finding

**Finding:** An unexpected service is listening on a network port.

**Evidence:** The service was identified using `ss`.

**Potential Risk:** The service may expose unnecessary functionality.

**Recommendation:** Identify its purpose and confirm whether it is authorised. Restrict unnecessary exposure through an approved change process.

**Status:** Requires investigation.

---

# 8. Professional Applications of These Projects

| Project | Practical Application |
|---|---|
| Security Assessment Toolkit | Linux security administration |
| File Integrity Monitoring | Detecting unauthorised file changes |
| Log Monitor | Security event investigation |
| User and Permission Audit | Access-control reviews |
| Incident Response Plan | Cybersecurity incident handling |

These projects demonstrate foundational skills relevant to:

- Linux System Administration.
- Security Operations Centre (SOC) Analysis.
- Cybersecurity Engineering.
- Security Auditing.
- Junior Penetration Testing.
- IT Security Support.

---

# 9. Knowledge Check

Answer these questions in your notebook.

1. What is the purpose of a Linux security assessment toolkit?
2. What is SHA-256?
3. How does file integrity monitoring work?
4. What happens when a monitored file changes?
5. What is the purpose of Linux system logs?
6. Why is keyword-based log monitoring limited?
7. Why should user accounts be audited?
8. What is incident response?
9. Explain the six incident-response stages.
10. Why is evidence preservation important?
11. What should a professional security report contain?
12. Identify five cybersecurity careers that use Linux security skills.

---

# 10. Practical Completion Checklist

- [ ] I created the Linux Security Assessment Toolkit.
- [ ] I generated a security assessment report.
- [ ] I created the File Integrity Monitoring demonstration.
- [ ] I detected a controlled file modification.
- [ ] I created the Linux Security Log Monitor.
- [ ] I created the User and Permission Audit.
- [ ] I prepared an Incident Response Plan.
- [ ] I understand the six incident-response stages.
- [ ] I understand professional security documentation.
- [ ] I understand the real-world applications of these projects.

**Lesson 14 Complete: Practical Linux Security Projects.**
