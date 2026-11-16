
# Module 13 – Lesson 01: Introduction to Linux Security

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define Linux security.
2. Explain why Linux systems need protection.
3. Identify common Linux security threats.
4. Understand the CIA security triad.
5. Explain the role of system administrators in security.
6. Identify important Linux security tools.
7. Understand the relationship between Linux security and cybersecurity.

---

## 2. What Is Linux Security?

Linux security refers to the methods, technologies, configurations, and practices used to protect Linux operating systems against unauthorised access, attacks, misuse, and data loss.

Linux security helps protect:

- User accounts.
- Files and directories.
- System processes.
- Network connections.
- Running services.
- Applications.
- Sensitive information.
- System configurations.

### Simple Explanation

Imagine that your Linux computer is a house.

The house has:

- Doors.
- Locks.
- Security cameras.
- Windows.
- Keys.
- An alarm system.

Linux security works similarly.

| House Security | Linux Security |
|---|---|
| Door lock | Authentication |
| House key | Password or SSH key |
| Room access | File permissions |
| Security camera | Monitoring |
| Alarm system | Intrusion detection |
| Security guard | Administrator |
| Restricted area | Privilege control |

The purpose is to ensure that only authorised users can access resources and perform permitted activities.

---

## 3. Why Is Linux Security Important?

Linux is used in many important environments.

### A. Servers

Websites, databases, applications, and enterprise systems frequently run on Linux.

A compromised server could expose sensitive information or interrupt services.

### B. Cloud Computing

Linux powers many cloud servers and container environments.

Weak security configurations can expose applications and infrastructure.

### C. Cybersecurity Infrastructure

Linux is used in:

- Security monitoring systems.
- Firewalls.
- Intrusion detection systems.
- Digital forensics.
- Security testing environments.

### D. Personal Computers

Linux computers also need protection against:

- Malicious software.
- Unauthorised access.
- Data theft.
- Account compromise.

### E. Critical Infrastructure

Linux systems can support important services such as telecommunications, industrial systems, and research infrastructure.

Security failures in these environments can have serious consequences.

---

## 4. The CIA Triad

One of the most important concepts in information security is the CIA Triad.

CIA stands for:

1. Confidentiality.
2. Integrity.
3. Availability.

These three principles help organisations protect information and systems.

### 4.1 Confidentiality

Confidentiality means ensuring that information is accessible only to authorised individuals.

Example:

A university student should not automatically have access to another student's private academic records.

Linux security mechanisms supporting confidentiality include:

- User authentication.
- File permissions.
- Encryption.
- Access control.

Example command:

```bash
chmod 600 private.txt
```

This gives the file owner read and write access while removing access for group members and others.

### 4.2 Integrity

Integrity means ensuring that information remains accurate and is not modified without authorisation.

Example:

An attacker should not be able to secretly modify a university's examination results.

Linux security mechanisms supporting integrity include:

- File permissions.
- Checksums.
- Digital signatures.
- Audit logs.
- File integrity monitoring.

Example:

```bash
sha256sum important-file.txt
```

This calculates a SHA-256 hash of the file.

If the file changes, its hash will normally change too.

### 4.3 Availability

Availability means ensuring that authorised users can access systems and information when required.

Example:

A university portal should remain accessible to students during registration.

Linux security mechanisms supporting availability include:

- System monitoring.
- Backups.
- Resource management.
- Service monitoring.
- Protection against denial-of-service attacks.

Useful commands:

```bash
uptime
free -h
df -h
systemctl --failed
```

### CIA Triad Summary

| Principle | Meaning | Example |
|---|---|---|
| Confidentiality | Prevent unauthorised access | File permissions |
| Integrity | Prevent unauthorised modification | SHA-256 verification |
| Availability | Keep systems accessible | Backups and monitoring |

Remember:

- Confidentiality protects secrecy.
- Integrity protects correctness.
- Availability protects accessibility.

---

## 5. Common Linux Security Threats

A security threat is a potential danger that could exploit a weakness and harm a system.

### 5.1 Weak Passwords

Weak passwords make it easier for attackers to compromise user accounts.

Examples of weak passwords include:

```text
123456
password
admin123
qwerty
```

Security recommendations:

- Use long, unique passwords.
- Avoid predictable information.
- Never reuse important passwords.
- Use multi-factor authentication where supported.

### 5.2 Brute-Force Attacks

A brute-force attack involves repeatedly trying different passwords or authentication combinations until access is obtained.

For example, an attacker may attempt thousands of password combinations against an exposed SSH service.

Defences include:

- Strong authentication.
- SSH key authentication.
- Rate limiting.
- Multi-factor authentication.
- Monitoring failed login attempts.

### 5.3 Malware

Malware means malicious software designed to harm, spy on, disrupt, or gain unauthorised access to a system.

Examples include:

| Malware | Description |
|---|---|
| Virus | Malicious code that attaches to other files or programs |
| Worm | Malware that can spread between systems |
| Trojan | Malicious software disguised as legitimate software |
| Ransomware | Malware that encrypts or otherwise holds data hostage |
| Rootkit | Tools designed to conceal malicious activity and maintain privileged access |
| Spyware | Software that secretly collects information |

### 5.4 Privilege Escalation

Privilege escalation occurs when an attacker obtains permissions beyond those originally assigned.

Example:

A normal user obtains unauthorised root-level access.

Linux security practices that reduce this risk include:

- Proper sudo configuration.
- Least privilege.
- Secure file permissions.
- Regular updates.
- Auditing privileged operations.

### 5.5 Misconfigured Services

A Linux server may expose unnecessary services.

For example, a service might listen on a network port even though it is not required.

Inspect listening sockets:

```bash
ss -tuln
```

Inspect active services:

```bash
systemctl list-units --type=service --state=running
```

### 5.6 Unpatched Vulnerabilities

Software vulnerabilities are weaknesses that attackers may exploit.

Outdated packages can contain known security problems.

On Kali Linux, package information can be refreshed with:

```bash
sudo apt update
```

This refreshes package indexes; it does not itself install upgrades.

Always review proposed changes before upgrading packages.

### 5.7 Social Engineering

Social engineering involves manipulating people into revealing information or performing unsafe actions.

Examples:

- Fake login pages.
- Phishing emails.
- Fraudulent technical support messages.
- Malicious attachments.

Technical security is important, but users also need security awareness.

---

## 6. Linux Security Layers

Linux security should not depend on only one protection mechanism.

A secure system uses multiple layers.

### Layer 1: Physical Security

Protect the physical computer and its storage devices.

### Layer 2: Account Security

Protect user accounts using authentication and appropriate account privileges.

### Layer 3: File Security

Control who can read, write, and execute files.

### Layer 4: Application Security

Install software from trusted sources and keep applications updated.

### Layer 5: Network Security

Control network access and reduce unnecessary exposure.

### Layer 6: Monitoring and Auditing

Inspect logs, processes, services, and security events.

### Layer 7: Backup and Recovery

Maintain backups so information can be recovered after accidental deletion, hardware failure, or an attack.

This approach is called **defence in depth**.

---

## 7. Important Linux Security Tools

Linux provides several tools that help administrators inspect and protect systems.

| Tool | Purpose |
|---|---|
| `sudo` | Controlled administrative privileges |
| `chmod` | Modify file permissions |
| `chown` | Change file ownership |
| `passwd` | Manage passwords |
| `ss` | Inspect network sockets |
| `journalctl` | Inspect system logs |
| `systemctl` | Manage and inspect services |
| `ufw` | Manage firewall rules |
| `nftables` | Linux packet filtering framework |
| `fail2ban` | Help respond to repeated authentication failures |
| `auditd` | Record selected system security events |
| `AIDE` | File integrity monitoring |

You will study these tools and concepts in more detail throughout Module 13 and Module 14.

---

## 8. Practical Lab: Basic Linux Security Inspection

### Lab Objective

Perform a basic security inspection of your Kali Linux virtual machine.

You will inspect:

- Current user.
- User privileges.
- Operating system.
- File permissions.
- Running processes.
- Network listening sockets.
- Failed system services.

All commands in this lab are for inspection. You will not modify system security settings.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module13-lab/lesson01
cd ~/module13-lab/lesson01
```

### Step 2: Identify your current user

```bash
whoami
```

This displays the username of the currently logged-in account.

### Step 3: Inspect user identity and groups

```bash
id
```

This displays:

- User ID.
- Primary group ID.
- Supplementary groups.

### Step 4: Inspect operating system information

```bash
cat /etc/os-release
```

This displays information about your Linux distribution.

### Step 5: Inspect file permissions

Create a test file:

```bash
echo "Linux Security Laboratory" > security-test.txt
```

Inspect its permissions:

```bash
ls -l security-test.txt
```

Example output:

```text
-rw-r--r-- 1 kali kali 26 Nov 15 10:00 security-test.txt
```

The permissions indicate:

- Owner: read and write.
- Group: read.
- Others: read.

### Step 6: Inspect running processes

```bash
ps aux
```

This displays running processes.

Look for unfamiliar processes, but do not assume that an unfamiliar process is malicious.

### Step 7: Inspect listening network sockets

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

### Step 8: Inspect failed services

```bash
systemctl --failed --no-pager
```

This displays services or units that systemd considers failed.

### Step 9: Create a security inspection report

Run:

```bash
{
    echo "LINUX SECURITY INSPECTION REPORT"
    echo "================================"
    echo
    echo "CURRENT USER"
    whoami
    echo
    echo "USER IDENTIFICATION"
    id
    echo
    echo "OPERATING SYSTEM"
    cat /etc/os-release
    echo
    echo "TEST FILE PERMISSIONS"
    ls -l security-test.txt
    echo
    echo "LISTENING NETWORK SOCKETS"
    ss -tuln
    echo
    echo "FAILED SYSTEM SERVICES"
    systemctl --failed --no-pager
} > security-inspection-report.txt 2>&1
```

Read your report:

```bash
cat security-inspection-report.txt
```

### Expected Files

Your lab directory should contain:

```text
lesson01/
├── security-test.txt
└── security-inspection-report.txt
```

---

## 9. Real-World Cybersecurity Application

Imagine you are working as a junior Linux security administrator for a university.

Your responsibilities include:

1. Protecting student information.
2. Monitoring server activity.
3. Reviewing user privileges.
4. Identifying unnecessary network exposure.
5. Checking system logs.
6. Maintaining secure backups.
7. Reporting suspicious observations.

The CIA Triad helps you understand what you are protecting.

For example:

| Situation | Security Principle |
|---|---|
| Preventing unauthorised access to student records | Confidentiality |
| Preventing unauthorised changes to examination results | Integrity |
| Keeping the university portal online | Availability |

Linux security is therefore not just about running security commands. It is about protecting the confidentiality, integrity, and availability of information.

---

## 10. Knowledge Check

Answer these questions in your notebook.

1. What is Linux security?
2. Why is Linux security important?
3. What does CIA stand for?
4. Explain confidentiality with a practical example.
5. Explain integrity with a practical example.
6. Explain availability with a practical example.
7. What is a brute-force attack?
8. What is malware?
9. Explain privilege escalation.
10. What is defence in depth?
11. Name five Linux security tools and explain their purposes.
12. Why should administrators inspect listening network ports?
13. What is the purpose of security auditing?
14. How do backups support system availability?
15. Why is security awareness important?

---

## 11. Practical Completion Checklist

- [ ] I understand the meaning of Linux security.
- [ ] I understand the importance of Linux security.
- [ ] I can explain the CIA Triad.
- [ ] I understand common Linux security threats.
- [ ] I understand defence in depth.
- [ ] I can identify important Linux security tools.
- [ ] I completed the basic security inspection.
- [ ] I generated my security inspection report.
- [ ] I understand how Linux security applies to real-world systems.

**Lesson 01 Complete: Introduction to Linux Security.**
