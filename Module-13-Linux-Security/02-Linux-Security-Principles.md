---
---


# Module 13 – Lesson 02: Linux Security Principles

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the fundamental principles of computer security.
2. Understand the Principle of Least Privilege.
3. Explain defence in depth.
4. Understand Zero Trust security.
5. Explain separation of duties.
6. Understand attack surfaces and security boundaries.
7. Apply security principles to Linux systems.
8. Perform a basic Linux security principles inspection.

---

## 2. Introduction to Linux Security Principles

Security principles are fundamental rules and ideas that guide how computer systems should be protected.

They help administrators make decisions about:

- Who can access a system.
- What users can do.
- Which services should run.
- How information should be protected.
- How security incidents should be detected.
- How systems should recover from failures.

Think of security principles as the foundation of a building.

Without a strong foundation, even expensive security tools may fail to protect a system properly.

---

## 3. Principle of Least Privilege (PoLP)

### Definition

The Principle of Least Privilege means that every user, application, or process should receive only the permissions necessary to perform its assigned tasks.

A user should not receive unnecessary administrative privileges.

### Simple Example

Imagine a university with three employees:

| Employee | Responsibility | Required Access |
|---|---|---|
| Lecturer | Upload lecture materials | Teaching materials |
| Accountant | Manage financial records | Financial records |
| IT Administrator | Maintain the server | Approved administrative access |

The lecturer should not automatically have access to financial records.

The accountant should not automatically receive permission to modify system configurations.

Each person receives only the access needed for their work.

### Linux Example

Inspect your current identity:

```bash
whoami
id
```

Inspect file permissions:

```bash
ls -l
```

Example:

```text
-rw-r----- 1 abbas students 1200 notes.txt
```

Interpretation:

- Owner: read and write.
- Group: read.
- Others: no access.

This limits access according to ownership and group membership.

### Why Least Privilege Matters

It helps reduce:

1. Unauthorised access.
2. Accidental file modification.
3. Damage caused by compromised accounts.
4. Privilege escalation opportunities.
5. Unnecessary administrative activity.

**Important:** Avoid using root privileges for ordinary tasks. Use `sudo` only when administrative privileges are required.

---

## 4. Defence in Depth

### Definition

Defence in depth is a security strategy that uses multiple layers of protection rather than depending on one security mechanism.

If one protection fails, other layers may still prevent or limit an attack.

### Example: Securing a Linux Server

A Linux server can use several security layers:

| Layer | Protection |
|---|---|
| 1 | Physical security |
| 2 | Strong user authentication |
| 3 | File permissions |
| 4 | Application security |
| 5 | Firewall rules |
| 6 | Security monitoring |
| 7 | Backup and recovery |

### Real-World Example

Imagine an attacker obtains a user's password.

That does not necessarily mean the attacker can control the entire system.

Additional protections may include:

- Limited user privileges.
- Multi-factor authentication.
- Restricted network access.
- Monitoring and alerts.
- Regular backups.

Each layer reduces the impact of a possible security failure.

### Linux Commands for Inspection

Inspect user identity:

```bash
id
```

Inspect file permissions:

```bash
ls -l
```

Inspect listening ports:

```bash
ss -tuln
```

Inspect failed services:

```bash
systemctl --failed
```

These commands help inspect different parts of the system's security posture.

---

## 5. Zero Trust Security

### Definition

Zero Trust is a security approach based on the principle:

**Never automatically trust; always verify.**

In traditional network security, users inside an organisation's network may sometimes be treated as more trustworthy.

Zero Trust challenges this assumption.

Every access request should be evaluated using appropriate identity, device, resource, and policy information.

### Important Zero Trust Concepts

1. Verify identity.
2. Check access permissions.
3. Limit privileges.
4. Monitor activity.
5. Re-evaluate access when circumstances change.

### Example

A university lecturer tries to access an examination database.

Even though the lecturer is connected to the university network, the system should still verify:

- Who the lecturer is.
- Whether the account is authenticated.
- Whether the lecturer is authorised to access the database.
- Whether the requested operation is permitted.

Being connected to a trusted network does not automatically grant access to every resource.

### Linux Security Application

Linux systems can support Zero Trust ideas through:

- Individual user accounts.
- SSH key authentication.
- Sudo access controls.
- File permissions.
- Firewall restrictions.
- Logging and auditing.

Zero Trust is not one Linux command or a single software package. It is an overall security architecture.

---

## 6. Separation of Duties

### Definition

Separation of Duties means dividing important responsibilities among different people or roles so that one person does not have unnecessary control over an entire sensitive process.

### Example

Consider a university payment system.

| Role | Responsibility |
|---|---|
| Finance officer | Records payments |
| Finance supervisor | Reviews payment records |
| IT administrator | Maintains the technical system |

Separating responsibilities helps reduce fraud, mistakes, and abuse of privileges.

### Linux Application

A Linux environment can use:

- Separate user accounts.
- Different groups.
- Restricted sudo permissions.
- Administrative logging.
- Controlled access to sensitive files.

For example, a user responsible for reading application logs does not necessarily need permission to modify system configuration files.

---

## 7. Attack Surface

### Definition

An attack surface is the collection of points where an attacker could potentially attempt to interact with or compromise a system.

Examples include:

- Open network ports.
- Running services.
- Web applications.
- User accounts.
- Remote login interfaces.
- Installed software.
- Exposed APIs.

### Simple Example

Imagine a Linux server running:

- SSH.
- A web server.
- A database server.
- An unnecessary remote administration service.

Every exposed service may introduce additional security risks.

Reducing unnecessary exposure helps reduce the attack surface.

### Inspect Your Attack Surface

Inspect listening sockets:

```bash
ss -tuln
```

Inspect running services:

```bash
systemctl list-units --type=service --state=running
```

Inspect network interfaces:

```bash
ip addr
```

Inspect active processes:

```bash
ps aux
```

**Important:** A listening port is not automatically dangerous. Investigate which application owns it, why it is exposed, and whether that exposure is necessary.

---

## 8. Security Boundaries

### Definition

A security boundary separates resources or environments that have different access requirements.

It helps control how users, applications, and systems interact.

### Examples

- Normal user versus root account.
- Guest account versus administrator account.
- Host operating system versus virtual machine.
- Public network versus private network.
- Application process versus operating system.

### Your VMware Laboratory

Your Kali Linux virtual machine provides a separate guest operating system from your Windows host.

This creates a useful boundary for learning and experimentation.

However, a virtual machine is not an absolute security guarantee.

Security depends on:

- Correct virtual machine configuration.
- Network mode.
- Shared folders.
- Clipboard integration.
- Shared devices.
- Host and guest security updates.

Continue using controlled lab environments and avoid exposing test services unnecessarily.

---

## 9. Secure by Default

### Definition

Secure by Default means that systems should begin with safe configurations rather than requiring users to discover and fix every insecure setting themselves.

Examples:

- Unnecessary services are not enabled.
- Sensitive files have restrictive permissions.
- Administrative access is controlled.
- Unneeded network exposure is avoided.
- Default credentials are changed.

### Linux Example

Inspect file permissions:

```bash
ls -l /etc/shadow
```

This file contains password hash information and should be protected from ordinary users.

Do not change its permissions manually.

Inspect privileged account information:

```bash
getent passwd root
```

A secure configuration should avoid unnecessary access to sensitive system resources.

---

## 10. Accountability and Auditing

### Definition

Accountability means that actions performed on a system can be associated with identifiable users or processes.

Auditing means recording and reviewing activities to determine what happened.

### Why It Matters

Suppose an important configuration file is modified unexpectedly.

An administrator may need to investigate:

- When did the modification occur?
- Which account performed it?
- What process was involved?
- Was the change authorised?
- Did it cause other problems?

### Linux Tools

Inspect recent login history:

```bash
last
```

Inspect recent authentication-related messages where available:

```bash
sudo journalctl -b --no-pager | grep -iE 'authentication|login|sudo'
```

Inspect system logs:

```bash
journalctl -b -n 50 --no-pager
```

Not every Linux installation records the same events in the same way. Log availability depends on system configuration and permissions.

---

## 11. Risk Management

### Definition

Risk management involves identifying potential threats, evaluating their likelihood and impact, and choosing appropriate measures to reduce risk.

A simple risk model is:

Risk = Likelihood × Impact

For example:

| Threat | Likelihood | Impact | Priority |
|---|---|---|---|
| Weak password | High | High | High |
| Unnecessary exposed service | Medium | High | High |
| Accidental file deletion | Medium | Medium | Medium |
| Temporary CPU spike | Medium | Low | Lower |

This is a simplified prioritisation model, not a precise mathematical measurement.

### Risk Treatment

Common approaches include:

1. Avoid the risk.
2. Reduce the risk.
3. Transfer the risk.
4. Accept the risk.

Example:

If a remote service is unnecessary, disabling its exposure may reduce the risk.

If a service is essential, strengthen its authentication and monitoring instead.

---

## 12. Practical Lab: Linux Security Principles Inspection

### Lab Objective

Inspect your Kali VM and identify how different security principles apply.

### Step 1: Create your workspace

```bash
mkdir -p ~/module13-lab/lesson02
cd ~/module13-lab/lesson02
```

### Step 2: Inspect user privileges

```bash
whoami
id
groups
```

Identify your current user and group memberships.

### Step 3: Inspect sensitive file permissions

```bash
ls -l /etc/passwd /etc/shadow
```

Observe the difference between the permissions of these files.

Do not modify them.

### Step 4: Inspect network exposure

```bash
ss -tuln
```

Record any listening ports.

Do not assume that every port is a vulnerability.

### Step 5: Inspect running services

```bash
systemctl list-units --type=service --state=running --no-pager
```

Identify several running services.

### Step 6: Inspect recent system activity

```bash
journalctl -b -n 30 --no-pager
```

Look for useful administrative events.

### Step 7: Create a security principles report

Run:

```bash
{
    echo "LINUX SECURITY PRINCIPLES REPORT"
    echo "================================"
    echo
    echo "CURRENT USER"
    whoami
    echo
    echo "USER GROUPS"
    groups
    echo
    echo "SENSITIVE FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow
    echo
    echo "LISTENING NETWORK SOCKETS"
    ss -tuln
    echo
    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running --no-pager
    echo
    echo "RECENT SYSTEM LOGS"
    journalctl -b -n 30 --no-pager
} > security-principles-report.txt 2>&1
```

Inspect your report:

```bash
cat security-principles-report.txt
```

### Expected Files

```text
lesson02/
└── security-principles-report.txt
```

---

## 13. Applying Security Principles in Real Life

Imagine that you are responsible for securing a university Linux server.

Your security decisions should follow these principles:

| Principle | Administrative Decision |
|---|---|
| Least Privilege | Give users only necessary permissions |
| Defence in Depth | Use multiple protection layers |
| Zero Trust | Verify each access request |
| Separation of Duties | Divide sensitive responsibilities |
| Attack Surface Reduction | Remove unnecessary exposure |
| Secure by Default | Use safe initial configurations |
| Accountability | Maintain useful audit records |
| Risk Management | Prioritise important threats |

These principles guide the selection and configuration of actual Linux security tools.

---

## 14. Knowledge Check

Answer these questions in your notebook.

1. What is the Principle of Least Privilege?
2. Why should ordinary users avoid unnecessary root access?
3. Explain defence in depth.
4. What does Zero Trust mean?
5. Why is network location alone insufficient to establish trust?
6. Explain separation of duties with a university example.
7. What is an attack surface?
8. Give five examples of Linux attack surfaces.
9. What is a security boundary?
10. Why should systems be secure by default?
11. What is accountability?
12. What is security auditing?
13. Explain the basic risk management model.
14. How can unnecessary listening services increase risk?
15. Explain how your Kali VMware environment provides a useful security boundary.

---

## 15. Practical Completion Checklist

- [ ] I understand the Principle of Least Privilege.
- [ ] I understand defence in depth.
- [ ] I understand Zero Trust.
- [ ] I understand separation of duties.
- [ ] I can identify attack surfaces.
- [ ] I understand security boundaries.
- [ ] I understand secure-by-default configurations.
- [ ] I understand accountability and auditing.
- [ ] I understand basic risk management.
- [ ] I completed the Linux security inspection.
- [ ] I generated my security principles report.

**Lesson 02 Complete: Linux Security Principles.**
