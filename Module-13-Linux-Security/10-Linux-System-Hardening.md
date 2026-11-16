
# Module 13 – Lesson 10: Linux System Hardening

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define Linux system hardening.
2. Understand the attack surface.
3. Identify unnecessary services.
4. Understand secure system configurations.
5. Inspect Linux boot and kernel security settings.
6. Understand account and permission hardening.
7. Explain application security controls.
8. Understand security frameworks such as AppArmor.
9. Identify common system-hardening mistakes.
10. Perform a basic Linux system-hardening audit.

---

## 2. What Is Linux System Hardening?

Linux system hardening is the process of improving a Linux system's security by reducing vulnerabilities, restricting unnecessary access, and applying secure configurations.

The goal is to make the system more resistant to attacks.

### Simple Analogy

Imagine a house.

You improve its security by:

- Locking unnecessary doors.
- Installing strong locks.
- Protecting windows.
- Restricting access to sensitive rooms.
- Installing alarms.
- Monitoring suspicious activity.

Linux hardening follows the same principle.

### Why System Hardening Matters

It helps:

- Reduce the attack surface.
- Prevent unauthorised access.
- Protect sensitive information.
- Restrict administrative privileges.
- Reduce the impact of malware.
- Improve system reliability.
- Strengthen overall cybersecurity.

---

## 3. Understanding the Attack Surface

The attack surface is the collection of points through which an attacker could potentially interact with or compromise a system.

Examples include:

- Network services.
- Open ports.
- User accounts.
- Applications.
- Exposed APIs.
- Weak passwords.
- Misconfigured permissions.
- Outdated software.

### Example

Imagine a Linux server running:

- SSH.
- Apache.
- MySQL.
- FTP.
- Telnet.
- Several unnecessary background services.

Every unnecessary service may increase the system's exposure.

Reducing unnecessary services helps reduce the attack surface.

### Attack Surface Diagram

```text
             LINUX SYSTEM
                   |
       +-----------+-----------+
       |           |           |
    Network     Accounts   Applications
    Services      and       and Software
                Permissions
       |           |           |
       +-----------+-----------+
                   |
              ATTACK SURFACE
```

---

## 4. Principle of Least Functionality

A secure system should provide only the functionality required for its intended purpose.

For example:

A Linux web server may need:

- Web server software.
- SSH administration.
- Required application dependencies.

It may not need:

- An FTP server.
- A Telnet service.
- Unused database software.
- Unnecessary remote-access applications.

Removing unnecessary components can reduce risk.

**Important:** Never remove a service simply because you do not recognise its name. Identify its purpose and dependencies first.

---

## 5. Service Hardening

Linux systems run services that perform background tasks.

Some services listen for network connections, while others operate internally.

Unnecessary services can create security risks.

### Inspect Running Services

```bash
systemctl list-units --type=service --state=running
```

### List Installed Service Unit Files

```bash
systemctl list-unit-files --type=service
```

### Inspect a Specific Service

Example:

```bash
systemctl status ssh --no-pager
```

### Inspect Listening Network Services

```bash
sudo ss -tulpen
```

### Security Recommendation

Administrators should:

1. Identify running services.
2. Determine their purpose.
3. Identify which services are required.
4. Review their network exposure.
5. Remove or disable unnecessary services only after impact analysis and authorisation.

Do not disable system services blindly.

---

## 6. User Account Hardening

User accounts are an important part of Linux security.

A compromised account can provide an attacker with access to files, applications, and potentially administrative functions.

### Recommended Practices

- Use individual accounts.
- Remove or disable obsolete accounts through an approved process.
- Protect administrator accounts.
- Use strong authentication.
- Restrict sudo access.
- Monitor account activity.
- Avoid sharing credentials.

### Inspect User Accounts

```bash
getent passwd
```

### Inspect Accounts with Login Shells

```bash
awk -F: '$7 !~ /(nologin|false)$/ {print $1, $7}' /etc/passwd
```

This is an initial inspection only. Some legitimate system accounts may have login-capable shells.

### Inspect Group Membership

```bash
getent group sudo
```

---

## 7. File System Hardening

File permissions determine who can access files and directories.

Incorrect permissions may expose sensitive data.

### Inspect Important System File Permissions

```bash
ls -l /etc/passwd /etc/shadow /etc/group
```

### Inspect SSH Configuration Permissions

```bash
ls -l /etc/ssh/sshd_config
```

### Inspect Home Directory Permissions

```bash
ls -ld /home/*
```

### Security Recommendations

- Protect password-related files.
- Restrict access to sensitive directories.
- Avoid world-writable files unless specifically required.
- Use appropriate ownership.
- Review SUID and SGID programs.
- Apply ACLs where appropriate.

Never use broad recursive permission changes to "secure" the whole system.

---

## 8. Kernel Hardening

The Linux kernel controls essential operating-system operations.

Kernel hardening involves reducing opportunities for attackers to exploit kernel features or weaknesses.

### Important Areas

1. Kernel security updates.
2. Restricted kernel module loading.
3. Secure memory handling.
4. Network security settings.
5. Protection against privilege escalation.
6. Appropriate kernel boot configuration.

### Inspect Running Kernel

```bash
uname -r
```

### Inspect Kernel Parameters

Linux provides a system interface for kernel settings through `/proc/sys/`.

Example:

```bash
sysctl -a 2>/dev/null | head -n 30
```

### Inspect a Specific Security Setting

```bash
sysctl kernel.randomize_va_space
```

A common value is:

```text
kernel.randomize_va_space = 2
```

This setting controls Address Space Layout Randomization (ASLR).

ASLR randomises important memory locations to make some exploitation techniques harder.

Do not modify kernel parameters during this inspection lesson.

---

## 9. Understanding AppArmor

AppArmor is a Linux security module that provides Mandatory Access Control (MAC).

It restricts what applications can access according to defined security profiles.

### What Is Mandatory Access Control?

Mandatory Access Control means access restrictions are enforced by system security policy rather than relying only on ordinary file ownership and permissions.

### Example

Imagine a web server application.

Even if the application is compromised, an AppArmor profile may restrict which files and system resources it can access.

This can reduce the damage caused by a compromised application.

### Check AppArmor Status

```bash
command -v aa-status
```

If available:

```bash
sudo aa-status
```

### Important AppArmor Modes

| Mode | Meaning |
|---|---|
| Enforce | Enforces the profile's restrictions |
| Complain | Records policy violations without enforcing them |
| Unconfined | Process is not confined by an AppArmor profile |

AppArmor availability and configuration depend on the Linux distribution and kernel setup.

---

## 10. Understanding SELinux

SELinux stands for Security-Enhanced Linux.

It is another Mandatory Access Control security system.

SELinux provides policy-based restrictions on processes, files, and other system resources.

### Important SELinux Concepts

- Security contexts.
- Policies.
- Domains.
- Types.
- Enforcement modes.

### Common SELinux Modes

| Mode | Meaning |
|---|---|
| Enforcing | Applies security policies |
| Permissive | Logs policy violations without enforcing them |
| Disabled | SELinux policy enforcement is disabled |

SELinux is commonly associated with distributions such as Red Hat Enterprise Linux and Fedora.

AppArmor is commonly used in Ubuntu and is also available in other Linux distributions.

Do not assume Kali has SELinux enabled.

---

## 11. Network Hardening

Network hardening reduces unnecessary network exposure.

### Important Practices

- Restrict unnecessary listening services.
- Use a properly configured firewall.
- Secure SSH.
- Protect network interfaces.
- Monitor network connections.
- Use secure protocols.
- Protect DNS and network configurations.

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

### Inspect Firewall Status

```bash
sudo ufw status verbose
```

The UFW command may be unavailable if UFW is not installed.

---

## 12. Boot and Physical Security

Linux security begins before the operating system finishes booting.

Physical access to a computer can create security risks.

### Important Protection Measures

1. Protect physical access to computers.
2. Use appropriate firmware passwords where supported.
3. Protect bootloader configuration.
4. Use full-disk encryption where appropriate.
5. Keep Secure Boot enabled where supported and compatible.
6. Protect recovery environments.
7. Restrict unauthorised external boot access.

### Why Full-Disk Encryption Matters

Full-disk encryption helps protect stored information if a device is lost or stolen.

It is especially useful for laptops containing sensitive information.

Encryption does not replace strong login authentication or secure backups.

---

## 13. Security Baselines

A security baseline is a documented collection of minimum security requirements for a system.

An organisation may define a Linux baseline containing:

- Approved software.
- Account security requirements.
- Password policies.
- SSH configuration.
- Firewall policies.
- Logging requirements.
- File permission requirements.
- Update schedules.
- Backup procedures.

Security baselines make it easier to maintain consistent configurations across multiple machines.

### Example

A university may require every Linux server to:

1. Use supported software versions.
2. Restrict administrative access.
3. Enable appropriate firewall rules.
4. Monitor authentication events.
5. Maintain regular security updates.
6. Protect sensitive files.

---

## 14. Common System-Hardening Mistakes

### Mistake 1: Disabling Unknown Services

A service may be necessary for system operation.

### Mistake 2: Applying Random Security Commands

Unverified configuration changes may break the operating system.

### Mistake 3: Ignoring IPv6

Protecting IPv4 alone may leave other network paths insufficiently restricted.

### Mistake 4: Giving Everyone Administrative Privileges

This increases the potential impact of compromised accounts.

### Mistake 5: Ignoring Logs

Suspicious activities may remain undetected.

### Mistake 6: Disabling Security Features

Turning off security mechanisms may increase exposure.

### Mistake 7: Failing to Maintain Backups

A security incident or configuration error may cause data loss.

### Best Practice

Use a documented, tested, and reversible hardening process.

---

# 15. Practical Lab: Linux System-Hardening Audit

## Lab Objective

Inspect your Kali Linux system's security configuration and identify areas that may require further investigation.

This is an inspection-only exercise.

Do not disable services, modify kernel parameters, change account permissions, or alter boot settings.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson10
cd ~/module13-lab/lesson10
```

### Step 2: Inspect Your Operating System

```bash
cat /etc/os-release
```

### Step 3: Inspect Your Running Kernel

```bash
uname -r
```

### Step 4: Inspect Running Services

```bash
systemctl list-units --type=service --state=running --no-pager
```

### Step 5: Inspect Listening Network Services

```bash
sudo ss -tulpen
```

### Step 6: Inspect User Accounts

```bash
getent passwd
```

### Step 7: Inspect Sudo Group Membership

```bash
getent group sudo
```

### Step 8: Inspect Important File Permissions

```bash
ls -l /etc/passwd /etc/shadow /etc/group /etc/ssh/sshd_config
```

Some paths may not exist on every system.

### Step 9: Inspect Kernel ASLR

```bash
sysctl kernel.randomize_va_space
```

### Step 10: Inspect AppArmor Availability

```bash
command -v aa-status
```

If available:

```bash
sudo aa-status
```

### Step 11: Inspect Firewall Status

```bash
sudo ufw status verbose
```

### Step 12: Inspect Network Configuration

```bash
ip -br addr
```

```bash
ip route
```

### Step 13: Generate Your System-Hardening Report

Run:

```bash
{
    echo "LINUX SYSTEM HARDENING AUDIT"
    echo "============================="
    echo
    echo "OPERATING SYSTEM"
    cat /etc/os-release
    echo
    echo "RUNNING KERNEL"
    uname -r
    echo
    echo "RUNNING SERVICES"
    systemctl list-units --type=service --state=running --no-pager
    echo
    echo "LISTENING NETWORK SERVICES"
    sudo ss -tulpen
    echo
    echo "USER ACCOUNTS"
    getent passwd
    echo
    echo "SUDO GROUP MEMBERSHIP"
    getent group sudo
    echo
    echo "IMPORTANT FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow /etc/group /etc/ssh/sshd_config
    echo
    echo "KERNEL ASLR SETTING"
    sysctl kernel.randomize_va_space
    echo
    echo "APPARMOR AVAILABILITY"
    command -v aa-status
    echo
    echo "APPARMOR STATUS"
    if command -v aa-status >/dev/null 2>&1; then
        sudo aa-status
    else
        echo "AppArmor status command unavailable."
    fi
    echo
    echo "FIREWALL STATUS"
    sudo ufw status verbose
    echo
    echo "NETWORK INTERFACES"
    ip -br addr
    echo
    echo "ROUTING INFORMATION"
    ip route
} > system-hardening-report.txt 2>&1
```

Inspect your report:

```bash
cat system-hardening-report.txt
```

### Expected Files

```text
lesson10/
└── system-hardening-report.txt
```

---

## 16. Real-World Cybersecurity Application

Imagine you are a cybersecurity administrator responsible for a university Linux server.

Your audit discovers:

- Several unnecessary services.
- Excessive administrative access.
- Weak file permissions.
- Incomplete firewall restrictions.
- Missing security updates.
- Inadequate system monitoring.

Your hardening recommendations should include:

1. Reduce unnecessary services.
2. Apply least privilege.
3. Correct insecure file permissions.
4. Review firewall rules.
5. Apply approved security patches.
6. Strengthen SSH configuration.
7. Enable appropriate security monitoring.
8. Document the final security baseline.

Always assess the operational impact before making changes.

---

## 17. Knowledge Check

Answer these questions in your notebook.

1. What is Linux system hardening?
2. What is an attack surface?
3. Why should unnecessary services be restricted?
4. What is the Principle of Least Functionality?
5. How can administrators inspect running services?
6. What is user account hardening?
7. Why are file permissions important?
8. What is kernel hardening?
9. What does ASLR stand for?
10. What is AppArmor?
11. What is Mandatory Access Control?
12. What is SELinux?
13. Differentiate between AppArmor and SELinux.
14. Why is network hardening important?
15. What is full-disk encryption?
16. What is a security baseline?
17. Identify five common system-hardening mistakes.
18. Why should hardening changes be tested before production deployment?

---

## 18. Practical Completion Checklist

- [ ] I understand Linux system hardening.
- [ ] I understand the attack surface.
- [ ] I can inspect running services.
- [ ] I understand account hardening.
- [ ] I understand file-system hardening.
- [ ] I understand kernel hardening.
- [ ] I understand ASLR.
- [ ] I understand AppArmor.
- [ ] I understand SELinux.
- [ ] I understand network hardening.
- [ ] I understand boot and physical security.
- [ ] I understand security baselines.
- [ ] I completed the system-hardening audit.
- [ ] I generated my system-hardening report.

**Lesson 10 Complete: Linux System Hardening.**
