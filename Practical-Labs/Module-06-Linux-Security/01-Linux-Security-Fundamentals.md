
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 01: LINUX SECURITY FUNDAMENTALS

### 1. Introduction to Linux Security

Linux security is the practice of protecting Linux systems, files, users, applications, services, and networks against unauthorized access, attacks, and misuse.

Linux is widely used in:

- Web servers.
- Cloud infrastructure.
- Enterprise networks.
- Cybersecurity laboratories.
- Embedded systems.
- Supercomputers.
- Software development environments.

Because Linux powers many important systems, securing it is an essential skill for system administrators and cybersecurity professionals.

### 2. What Is Linux Hardening?

Linux hardening is the process of reducing security weaknesses and improving the security configuration of a Linux system.

A newly installed Linux system may contain:

- Unnecessary services.
- Weak account configurations.
- Outdated packages.
- Excessive user permissions.
- Poorly configured network services.
- Insecure file permissions.
- Weak authentication settings.

Hardening helps reduce these risks.

**Example:**

Imagine a Linux server running five services, but only two are required.

Leaving all five services running increases the number of components that may expose the system to security problems.

A hardening process investigates unnecessary services and safely disables those that are not needed.

### 3. The CIA Triad

The CIA Triad is one of the fundamental concepts in information security.

It consists of three principles.

#### A. Confidentiality

Confidentiality ensures that information is accessible only to authorized people.

Example:

A student's academic records should only be accessible to authorized university personnel.

Linux security mechanisms:

- File permissions.
- User authentication.
- Encryption.
- Access control lists.

#### B. Integrity

Integrity ensures that information remains accurate and is not modified without authorization.

Example:

An attacker should not be able to modify a university's examination results without authorization.

Linux security mechanisms:

- File permissions.
- Cryptographic hashes.
- Audit logs.
- Digital signatures.

#### C. Availability

Availability ensures that systems and information remain accessible when required.

Example:

A university portal should remain accessible to students during registration.

Linux security mechanisms:

- System monitoring.
- Backups.
- Redundancy.
- Firewall configuration.
- Resource management.

### 4. Authentication vs Authorization

These two concepts are related but different.

**Authentication:** Verifies who you are.

**Authorization:** Determines what you are allowed to do.

Example:

When you log into Kali Linux:

1. The system checks your username and password.
2. It authenticates your identity.
3. It determines your permissions.
4. It allows or denies requested actions.

### 5. Linux Security Principles

#### Principle 1: Least Privilege

Users and applications should receive only the permissions necessary to perform their tasks.

Example:

A normal user should not automatically have unrestricted administrative privileges.

#### Principle 2: Defense in Depth

Use multiple security layers rather than depending on one protection mechanism.

Example:

- Strong passwords.
- Restricted permissions.
- Firewall protection.
- Security updates.
- Monitoring and logging.

#### Principle 3: Minimize the Attack Surface

The attack surface represents the areas through which an attacker might interact with a system.

Examples include:

- Open network ports.
- Running services.
- User accounts.
- Exposed applications.
- Weak authentication mechanisms.

Reducing unnecessary exposure makes systems harder to compromise.

#### Principle 4: Regular Updates

Software updates fix bugs and security vulnerabilities.

Outdated software may contain weaknesses that attackers already understand and know how to exploit.

#### Principle 5: Accountability

Security activities should be traceable.

Linux logging and auditing help administrators determine:

- Who accessed the system.
- When an event occurred.
- Which service generated an event.
- Whether suspicious activity took place.

### 6. Important Linux Security Components

| Component | Security Purpose |
|---|---|
| Users and groups | Control identities and access |
| File permissions | Restrict file access |
| sudo | Control administrative privileges |
| SSH | Secure remote administration |
| Firewall | Control network traffic |
| AppArmor | Restrict application capabilities |
| systemd | Manage system services |
| journald | Collect system logs |
| APT | Manage software packages |
| Audit tools | Monitor security-related events |

### 7. Understand Your Kali Security Environment

Before changing anything, inspect your current system.

Create a dedicated lab directory:

```bash
mkdir -p ~/practical-labs/module06/lab01
```

Move into it:

```bash
cd ~/practical-labs/module06/lab01
```

#### Task 1: Identify Your Linux Distribution

Run:

```bash
cat /etc/os-release
```

This displays information about your Linux distribution.

Look for:

- NAME
- VERSION
- ID
- PRETTY_NAME

#### Task 2: Identify the Kernel

Run:

```bash
uname -r
```

The kernel is the central component responsible for managing hardware and system resources.

Also run:

```bash
uname -a
```

#### Task 3: Identify the Current User

Run:

```bash
whoami
```

This displays your current username.

#### Task 4: Inspect User Identity and Groups

Run:

```bash
id
```

This displays:

- User ID (UID).
- Primary group ID (GID).
- Supplementary groups.

#### Task 5: Check Administrative Privileges

Run:

```bash
sudo -l
```

This displays the commands your current account is permitted to execute through sudo.

If authentication is requested, enter your own Kali account password.

Do not share your password.

#### Task 6: Inspect Logged-in Users

Run:

```bash
who
```

Then:

```bash
w
```

These commands provide information about currently logged-in users and sessions.

### 8. Inspect Security-Relevant System Information

#### Task 7: Inspect Running Processes

Run:

```bash
ps aux
```

This displays running processes.

For a shorter view:

```bash
ps aux | head -20
```

Look for processes you recognize.

Do not terminate unfamiliar processes without investigating them.

#### Task 8: Inspect Running Services

Run:

```bash
systemctl list-units --type=service --state=running
```

This lists currently running systemd services.

Not every running service is a security problem.

Your objective is to understand what is running.

#### Task 9: Inspect Listening Network Services

Run:

```bash
sudo ss -tulpn
```

This displays listening TCP and UDP sockets and, where permissions allow, associated processes.

Record any services that require further investigation.

#### Task 10: Inspect Recent Security-Relevant Logs

Run:

```bash
sudo journalctl -p warning -n 30 --no-pager
```

This displays recent warning-level and higher-priority journal messages.

Logs may contain routine system events, not necessarily attacks.

### 9. Understand Common Linux Security Threats

| Threat | Description |
|---|---|
| Brute-force attacks | Repeated attempts to guess credentials |
| Privilege escalation | Obtaining higher permissions than authorized |
| Malware | Software designed to perform harmful actions |
| Misconfiguration | Incorrect security settings |
| Unauthorized access | Access without permission |
| Denial of service | Making services unavailable |
| Unpatched vulnerabilities | Weaknesses in outdated software |
| Credential theft | Stealing usernames, passwords, or tokens |

### 10. Practical Exercise – Build a Security Inventory

Create a script:

```bash
nano security-inventory.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module06/lab01/security-inventory.txt"

{
    echo "LINUX SECURITY INVENTORY"
    echo "========================"
    echo "Date: $(date)"
    echo

    echo "OPERATING SYSTEM"
    cat /etc/os-release
    echo

    echo "KERNEL INFORMATION"
    uname -a
    echo

    echo "CURRENT USER"
    whoami
    echo

    echo "USER IDENTIFICATION"
    id
    echo

    echo "LOGGED-IN USERS"
    who
    echo

    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running
    echo

    echo "LISTENING NETWORK SERVICES"
    sudo ss -tulpn
    echo

    echo "RECENT WARNING LOGS"
    sudo journalctl -p warning -n 30 --no-pager

} > "$REPORT" 2>&1

echo "Security inventory generated:"
echo "$REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x security-inventory.sh
```

Run it:

```bash
sudo ./security-inventory.sh
```

Inspect the report:

```bash
less security-inventory.txt
```

Press `q` to exit.

### 11. Important Security Rules

During this module:

1. Never disable security protections without a clear reason.
2. Do not execute unknown scripts as root.
3. Do not change system permissions randomly.
4. Do not expose SSH to public networks for practice.
5. Do not scan networks you do not own or have permission to test.
6. Always inspect configurations before modifying them.
7. Record changes and understand how to reverse them.
8. Keep your Windows host separate from your Kali practical environment.

### 12. Knowledge Check

1. What is Linux security?
2. What is Linux hardening?
3. Explain the CIA Triad.
4. What is confidentiality?
5. What is integrity?
6. What is availability?
7. Differentiate between authentication and authorization.
8. Explain the principle of least privilege.
9. What is defense in depth?
10. What is an attack surface?
11. Why are software updates important?
12. What is the purpose of sudo?
13. What is AppArmor?
14. Why are system logs important?
15. Identify five common Linux security threats.

### 13. Lab Documentation

Create:

```bash
nano lab01-report.txt
```

Record:

```text
MODULE 06 – LAB 01
LINUX SECURITY FUNDAMENTALS

1. Linux distribution:
2. Kernel version:
3. Current username:
4. User ID:
5. Primary group:
6. Administrative privileges:
7. Number of running services:
8. Listening network services:
9. Security-relevant log observations:
10. Three security weaknesses worth investigating:
11. Five Linux security principles:
12. What I learned:

SECURITY INVENTORY:
Generated successfully: YES / NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 14. Completion Checklist

- [ ] Understood Linux security and hardening.
- [ ] Learned the CIA Triad.
- [ ] Understood authentication and authorization.
- [ ] Studied least privilege.
- [ ] Studied defense in depth.
- [ ] Understood attack surface reduction.
- [ ] Identified common Linux security threats.
- [ ] Inspected the operating system.
- [ ] Inspected users and privileges.
- [ ] Inspected running processes.
- [ ] Inspected running services.
- [ ] Inspected listening ports.
- [ ] Reviewed system logs.
- [ ] Generated the security inventory.
- [ ] Created the lab report.

**LAB 01 STATUS: READY FOR PRACTICAL COMPLETION**
