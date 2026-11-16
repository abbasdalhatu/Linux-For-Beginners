
# MODULE 17 – ADVANCED LINUX
## LESSON 11: ADVANCED LINUX SECURITY

---

## 1. Introduction to Advanced Linux Security

Linux security involves protecting the operating system, users, files, processes, applications, and network services against unauthorized access and malicious activity.

Advanced Linux security combines multiple protective mechanisms rather than relying on a single security tool.

These mechanisms include:

- User and group security.
- File permissions and access control.
- Authentication security.
- SSH hardening.
- Firewall protection.
- Mandatory access control.
- Security auditing.
- System updates.
- Log monitoring.
- Intrusion detection.
- Secure service configuration.

### Learning objectives

By the end of this lesson, you should be able to:

1. Explain advanced Linux security principles.
2. Inspect users, groups, and privileges.
3. Understand Linux access-control mechanisms.
4. Explain PAM authentication.
5. Understand SSH security.
6. Explain SELinux and AppArmor.
7. Inspect Linux security configurations.
8. Understand Linux auditing.
9. Identify common Linux security weaknesses.
10. Develop a basic Linux security assessment report.

---

## 2. Understanding the Linux Security Model

Linux security is built around several important principles.

### A. Authentication

Authentication verifies the identity of a user or system.

Examples:

- Password authentication.
- SSH key authentication.
- Multi-factor authentication.

### B. Authorization

Authorization determines what an authenticated user is permitted to do.

Examples:

- Reading files.
- Modifying directories.
- Executing programs.
- Managing system services.

### C. Accounting and Auditing

Accounting and auditing record activities so administrators can investigate what happened.

Examples:

- Login records.
- Authentication failures.
- Privileged command execution.
- System service events.

### D. Least Privilege

The principle of least privilege means users and applications should receive only the permissions required to perform their tasks.

For example, a normal user should not need unrestricted administrative privileges to read a document.

### E. Defense in Depth

Defense in depth uses multiple security layers.

For example:

1. Strong authentication.
2. Appropriate permissions.
3. Firewall protection.
4. Security updates.
5. Application isolation.
6. Log monitoring.

If one protection fails, other layers may still reduce the damage.

---

## 3. User and Group Security

Linux uses users and groups to control access to resources.

### Display the current user

```bash
whoami
```

### Display user identity and group membership

```bash
id
```

### Display logged-in users

```bash
who
```

### Display recent login activity

```bash
last
```

### Inspect user accounts

```bash
cat /etc/passwd
```

### Inspect group accounts

```bash
cat /etc/group
```

### Understanding `/etc/passwd`

Example:

```text
student:x:1000:1000:Student:/home/student:/bin/bash
```

The fields represent:

| Field | Meaning |
|---|---|
| student | Username |
| x | Password information stored elsewhere |
| 1000 | User ID |
| 1000 | Primary group ID |
| Student | User description |
| /home/student | Home directory |
| /bin/bash | Login shell |

The `x` does not represent the password itself. Password hashes are generally stored in `/etc/shadow`.

### Inspect password-account information

```bash
sudo getent shadow "$USER"
```

This command may display sensitive password-hash information. Do not share or publish its output.

### Inspect sudo privileges

```bash
sudo -l
```

This displays the commands the current account is permitted to execute through sudo.

---

## 4. Advanced Linux File Permissions

Linux permissions determine who can access files and directories.

There are three traditional permission categories:

- User (owner).
- Group.
- Others.

There are also three basic permission types:

| Permission | Symbol | Meaning |
|---|---|---|
| Read | r | Read file contents or list directory entries |
| Write | w | Modify file contents or directory entries |
| Execute | x | Execute a file or traverse a directory |

### Inspect permissions

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

Interpretation:

- Owner: read, write, execute.
- Group: read, execute.
- Others: read only.

### Change permissions

```bash
chmod 640 example.txt
```

This gives:

- Owner: read and write.
- Group: read.
- Others: no permissions.

### Change file ownership

```bash
sudo chown username:groupname example.txt
```

Only perform ownership changes on files you administer.

### Special permission bits

Linux supports three important special permission bits:

| Permission | Meaning |
|---|---|
| SUID | Executable runs with the file owner's effective identity |
| SGID | Executable can run with the file group's effective identity; on directories, it supports group inheritance |
| Sticky bit | Restricts deletion or renaming in shared directories according to ownership and privileges |

### Inspect SUID files

```bash
find /usr/bin -perm -4000 -type f 2>/dev/null
```

### Inspect SGID files

```bash
find /usr/bin -perm -2000 -type f 2>/dev/null
```

These commands inspect executable files in `/usr/bin`.

Finding an SUID or SGID file does not automatically mean it is vulnerable. Such permissions are sometimes necessary for normal system operation.

---

## 5. Access Control Lists (ACLs)

Traditional Linux permissions provide owner, group, and others access categories.

Access Control Lists allow more specific permissions for individual users and groups.

### Check ACL support

```bash
command -v getfacl
```

### Create a practice file

```bash
mkdir -p ~/module17-lab/lesson11
cd ~/module17-lab/lesson11
touch acl-test.txt
```

### Inspect permissions

```bash
ls -l acl-test.txt
```

### Inspect ACL information

```bash
getfacl acl-test.txt
```

### Assign an ACL entry

For a user account that exists on your system:

```bash
setfacl -m u:YOUR_USERNAME:r-- acl-test.txt
```

Replace `YOUR_USERNAME` with your actual Linux username.

### Inspect the result

```bash
getfacl acl-test.txt
```

### Remove the practice ACL entry

```bash
setfacl -x u:YOUR_USERNAME acl-test.txt
```

### Why ACLs matter

ACLs help administrators implement more precise access policies without changing the file's traditional owner and group.

---

## 6. Linux Authentication and PAM

PAM stands for Pluggable Authentication Modules.

It provides a framework that Linux applications can use to authenticate users.

Applications such as login services, sudo, and SSH may use PAM.

### PAM configuration location

```text
/etc/pam.d/
```

### List PAM configuration files

```bash
ls /etc/pam.d/
```

### Inspect a PAM configuration file

```bash
cat /etc/pam.d/common-auth
```

The exact configuration differs between Linux distributions.

### Main PAM management categories

| Category | Purpose |
|---|---|
| auth | Authentication |
| account | Account restrictions and validity |
| password | Password management |
| session | Session setup and cleanup |

### Why PAM is important

PAM can support:

- Password authentication.
- Authentication policies.
- Account restrictions.
- Session controls.
- Multi-factor authentication integrations.

**Security warning:** Do not edit PAM configuration files during this lesson. Incorrect PAM changes can prevent users, including administrators, from logging in.

---

## 7. SSH Security and Hardening

SSH stands for Secure Shell.

It provides encrypted remote access to Linux systems.

SSH is commonly used by system administrators to manage remote servers.

### Check SSH service status

```bash
systemctl status ssh
```

If the service is unavailable, it may not be installed or enabled.

### Inspect SSH configuration

```bash
sudo sshd -T
```

This displays the effective SSH server configuration when the server binary is installed and configuration is valid.

### Main SSH configuration file

```text
/etc/ssh/sshd_config
```

### Important SSH security settings

| Setting | Purpose |
|---|---|
| `PermitRootLogin` | Controls direct root login |
| `PasswordAuthentication` | Controls password-based authentication |
| `PubkeyAuthentication` | Controls public-key authentication |
| `MaxAuthTries` | Limits authentication attempts per connection |
| `AllowUsers` | Restricts permitted login accounts |

### SSH key authentication

SSH keys use a public/private key pair.

- Public key: installed on the destination system.
- Private key: kept securely by the user.

Generate a practice key pair:

```bash
ssh-keygen -t ed25519 -C "linux-security-lab"
```

Use a passphrase when appropriate.

The private key must remain confidential.

### Inspect generated keys

```bash
ls -l ~/.ssh/
```

### Important security practices

1. Protect private keys.
2. Use strong passphrases.
3. Prefer key-based authentication where practical.
4. Limit permitted accounts.
5. Monitor authentication failures.
6. Keep SSH software updated.
7. Test configuration changes before applying them.

Do not change your SSH server configuration on your main system as part of this exercise.

---

## 8. Mandatory Access Control (MAC)

Mandatory Access Control adds security restrictions beyond traditional file permissions.

Two major Linux MAC systems are SELinux and AppArmor.

### A. SELinux

SELinux stands for Security-Enhanced Linux.

It provides mandatory access control through security policies and labels.

Check whether SELinux tools are available:

```bash
command -v getenforce
```

If available:

```bash
getenforce
```

Possible results include:

- Enforcing.
- Permissive.
- Disabled.

### B. AppArmor

AppArmor is a Linux security module that applies application-specific security profiles.

Check AppArmor status:

```bash
sudo aa-status
```

If the command is unavailable, AppArmor tools may not be installed or active.

### SELinux vs AppArmor

| Feature | SELinux | AppArmor |
|---|---|---|
| Main approach | Security labels and policies | Application profiles |
| Policy model | Label-based | Path-based profiles |
| Main purpose | Mandatory access control | Mandatory access control |
| Common use | Enterprise Linux environments | Ubuntu and other Linux distributions |

Both technologies can restrict what applications are allowed to do.

**Important:** Do not disable, unload, or change the enforcement state of these security systems during this lesson.

---

## 9. Linux Capabilities

Linux capabilities divide traditional root privileges into smaller units.

Instead of granting an application every administrative privilege, a system can grant only specific capabilities.

Examples include:

| Capability | Purpose |
|---|---|
| CAP_NET_ADMIN | Network administration |
| CAP_NET_RAW | Raw network socket access |
| CAP_SYS_ADMIN | Broad system administration operations |
| CAP_CHOWN | Change file ownership |
| CAP_DAC_OVERRIDE | Bypass certain discretionary access checks |

### Inspect capabilities

Check availability:

```bash
command -v getcap
```

### Inspect capabilities on a file

```bash
getcap /usr/bin/ping
```

The result depends on your distribution and version.

### Inspect process capabilities

```bash
grep '^Cap' /proc/self/status
```

### Why capabilities matter

They support the principle of least privilege by allowing administrators to grant specific permissions instead of unrestricted root access.

---

## 10. Linux Security Auditing

Security auditing involves collecting and examining system activity to detect suspicious behaviour.

Important audit sources include:

- Authentication logs.
- System logs.
- Service logs.
- Kernel messages.
- Audit events.
- Login records.

### A. Inspect authentication logs

On many Debian-based systems:

```bash
sudo tail -n 50 /var/log/auth.log
```

The file may not exist on systems that use a different logging configuration.

### B. Inspect system logs

```bash
journalctl -b
```

### C. Inspect warning-level events

```bash
journalctl -p warning -b
```

### D. Inspect failed services

```bash
systemctl --failed
```

### E. Inspect recent login records

```bash
last -n 10
```

### F. Inspect failed login records

```bash
sudo lastb -n 10
```

This command may require the relevant login-failure database and permissions.

### Understanding Linux log locations

| Location | Purpose |
|---|---|
| `/var/log/auth.log` | Authentication events on systems using this log |
| `/var/log/syslog` | General system events on systems using this log |
| `/var/log/kern.log` | Kernel-related events on systems using this log |
| `/var/log/audit/` | Audit logs when auditd is configured |
| `journalctl` | systemd journal inspection |

---

## 11. Linux Auditing with auditd

`auditd` is the Linux Audit daemon.

It records configured security-related events.

### Check whether auditd exists

```bash
command -v auditctl
```

### Check service status

```bash
systemctl status auditd
```

### Inspect current audit rules

```bash
sudo auditctl -l
```

### Display audit status

```bash
sudo auditctl -s
```

### Search audit records

```bash
sudo ausearch -m USER_LOGIN
```

Available event types depend on the audit configuration and recorded events.

### Inspect audit reports

```bash
sudo aureport
```

### Important distinction

Auditd records events according to its configured rules. Installing it does not automatically guarantee that every security-relevant action is being monitored.

Do not modify audit rules or stop the audit daemon during this lesson.

---

## 12. Linux Security Updates

Security updates fix known vulnerabilities and improve system reliability.

### Refresh package information

```bash
sudo apt update
```

### Inspect available upgrades

```bash
apt list --upgradable
```

### Inspect installed package information

```bash
apt policy openssh-server
```

### Search for security-related packages

```bash
apt search security
```

### Why updates matter

Outdated software can contain publicly documented vulnerabilities.

Good maintenance practices include:

1. Review available updates.
2. Understand important changes.
3. Back up important information.
4. Apply updates through trusted repositories.
5. Verify service operation afterward.

For this lesson, inspect available updates. Do not start a large upgrade unless you have sufficient time, storage, and a suitable backup.

---

## 13. Common Linux Security Weaknesses

| Weakness | Possible consequence |
|---|---|
| Weak passwords | Unauthorized account access |
| Excessive privileges | Greater impact after compromise |
| Outdated software | Exposure to known vulnerabilities |
| Insecure SSH configuration | Increased remote-access risk |
| World-writable sensitive files | Unauthorized modification |
| Unnecessary services | Larger attack surface |
| Poor log monitoring | Delayed incident detection |
| Exposed private keys | Unauthorized authentication |
| Incorrect ACLs | Unintended access |
| Missing security updates | Unpatched vulnerabilities |

### Security inspection commands

Inspect world-writable directories:

```bash
find /tmp -maxdepth 1 -type d -perm -0002 -print
```

Inspect listening services:

```bash
ss -tulpn
```

Inspect failed services:

```bash
systemctl --failed
```

Inspect current privileges:

```bash
sudo -l
```

These commands provide information for assessment. Their results must be interpreted in context.

---

## 14. Practical Lab: Linux Security Inspection

This practical exercise focuses on read-only security inspection and a temporary ACL test.

### Step 1: Create your lab directory

```bash
mkdir -p ~/module17-lab/lesson11
cd ~/module17-lab/lesson11
```

### Step 2: Inspect your user identity

```bash
whoami
id
```

### Step 3: Inspect logged-in users

```bash
who
```

### Step 4: Inspect account information

```bash
getent passwd "$USER"
```

### Step 5: Inspect sudo permissions

```bash
sudo -l
```

### Step 6: Inspect file permissions

```bash
ls -la ~
```

### Step 7: Inspect SUID files

```bash
find /usr/bin -perm -4000 -type f 2>/dev/null
```

### Step 8: Inspect SSH configuration

```bash
sudo sshd -T
```

If the SSH server is not installed, record that fact rather than installing it just for this inspection.

### Step 9: Inspect MAC status

```bash
command -v getenforce
```

```bash
sudo aa-status
```

Record which security framework is available and active.

### Step 10: Inspect system logs

```bash
journalctl -p warning -b
```

### Step 11: Inspect failed services

```bash
systemctl --failed
```

### Step 12: Inspect listening services

```bash
ss -tulpn
```

### Step 13: Inspect audit status

```bash
command -v auditctl
```

If available:

```bash
sudo auditctl -s
```

### Step 14: Perform a temporary ACL exercise

```bash
cd ~/module17-lab/lesson11
touch security-test.txt
chmod 640 security-test.txt
getfacl security-test.txt
```

If ACL utilities are available:

```bash
setfacl -m u:YOUR_USERNAME:r-- security-test.txt
getfacl security-test.txt
```

Replace `YOUR_USERNAME` with your Linux username.

Remove the temporary ACL entry:

```bash
setfacl -x u:YOUR_USERNAME security-test.txt
```

### Step 15: Verify cleanup

```bash
getfacl security-test.txt
```

---

## 15. Practical Project: Build a Linux Security Audit Report

Create a Bash script that collects basic security information without modifying system configuration.

### Step 1: Create the script

```bash
nano security-audit.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson11/security-audit-report.txt"

{
    echo "======================================"
    echo "       LINUX SECURITY AUDIT"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "CURRENT USER:"
    whoami

    echo
    echo "USER IDENTITY:"
    id

    echo
    echo "LOGGED-IN USERS:"
    who

    echo
    echo "SUDO PRIVILEGES:"
    sudo -l

    echo
    echo "LISTENING SERVICES:"
    ss -tulpn

    echo
    echo "FAILED SERVICES:"
    systemctl --failed

    echo
    echo "RECENT WARNING EVENTS:"
    journalctl -p warning -b -n 30 --no-pager

    echo
    echo "SSH CONFIGURATION:"
    if command -v sshd >/dev/null 2>&1; then
        sudo sshd -T
    else
        echo "SSH server binary not found."
    fi

    echo
    echo "SELINUX STATUS:"
    if command -v getenforce >/dev/null 2>&1; then
        getenforce
    else
        echo "SELinux tools not found."
    fi

    echo
    echo "APPARMOR STATUS:"
    if command -v aa-status >/dev/null 2>&1; then
        sudo aa-status
    else
        echo "AppArmor tools not found."
    fi

    echo
    echo "AUDITD STATUS:"
    if command -v auditctl >/dev/null 2>&1; then
        sudo auditctl -s
    else
        echo "Audit tools not found."
    fi

    echo
    echo "SUID FILES IN /USR/BIN:"
    find /usr/bin -perm -4000 -type f 2>/dev/null

    echo
    echo "AVAILABLE PACKAGE UPDATES:"
    apt list --upgradable 2>/dev/null

    echo
    echo "======================================"
    echo "       AUDIT COMPLETE"
    echo "======================================"

} > "$REPORT" 2>&1

echo "Security report saved to: $REPORT"
```

### Step 2: Make the script executable

```bash
chmod +x security-audit.sh
```

### Step 3: Run the report

```bash
./security-audit.sh
```

The script may request your sudo password because it collects some privileged information.

### Step 4: Read the report

```bash
less ~/module17-lab/lesson11/security-audit-report.txt
```

Press `q` to exit.

### Step 5: Verify the report

```bash
ls -lh ~/module17-lab/lesson11/
```

**Important:** The report can contain usernames, network ports, security configurations, and other sensitive information. Keep it private.

---

## 16. Security Assessment Methodology

A professional Linux security assessment should follow a structured process.

### Phase 1: Preparation

- Identify the system owner.
- Obtain authorization.
- Define assessment scope.
- Understand operational restrictions.

### Phase 2: Information Collection

- Inspect operating system details.
- Identify users and groups.
- Review permissions.
- Identify running services.
- Inspect security configurations.

### Phase 3: Security Analysis

- Identify unnecessary privileges.
- Review authentication settings.
- Inspect exposed services.
- Examine security logs.
- Review update status.

### Phase 4: Risk Evaluation

Assess findings according to:

- Likelihood.
- Impact.
- Exploitability.
- Existing protections.
- Business importance.

### Phase 5: Reporting

Document:

1. Finding title.
2. Affected component.
3. Evidence.
4. Security impact.
5. Risk rating.
6. Recommended remediation.
7. Verification method.

### Phase 6: Remediation and Verification

- Obtain approval for changes.
- Apply appropriate fixes.
- Test the affected services.
- Verify that the vulnerability is addressed.
- Record the outcome.

---

## 17. Knowledge Check

Answer these questions in your notebook.

1. What is advanced Linux security?
2. Explain authentication and authorization.
3. What is the principle of least privilege?
4. What is defense in depth?
5. What is the purpose of `/etc/passwd`?
6. Explain SUID, SGID, and the sticky bit.
7. What are Access Control Lists?
8. What is PAM?
9. What is SSH hardening?
10. Differentiate SELinux and AppArmor.
11. What are Linux capabilities?
12. What is the purpose of auditd?
13. Why are system security updates important?
14. Identify five common Linux security weaknesses.
15. Explain the phases of a Linux security assessment.

---

## 18. Important Commands Summary

| Command | Purpose |
|---|---|
| `whoami` | Display current user |
| `id` | Display user and group identity |
| `who` | Display logged-in users |
| `last` | Display login history |
| `sudo -l` | Inspect sudo privileges |
| `ls -l` | Inspect file permissions |
| `getfacl` | Inspect ACLs |
| `setfacl` | Modify ACLs |
| `find /usr/bin -perm -4000` | Inspect SUID files |
| `sshd -T` | Display effective SSH configuration |
| `getenforce` | Inspect SELinux mode |
| `aa-status` | Inspect AppArmor status |
| `getcap` | Inspect file capabilities |
| `journalctl` | Inspect system journal |
| `auditctl -s` | Inspect audit subsystem status |
| `ausearch` | Search audit records |
| `systemctl --failed` | Inspect failed services |

---

## 19. Lesson Summary

In this lesson, you studied advanced Linux security.

You learned:

- Linux authentication and authorization.
- User and group security.
- Advanced file permissions.
- Access Control Lists.
- PAM authentication.
- SSH hardening.
- SELinux and AppArmor.
- Linux capabilities.
- Security auditing.
- Auditd.
- Security updates.
- Common Linux vulnerabilities.
- Security assessment methodology.

You also created a Linux security audit report.

### Completion checklist

- [ ] Understand the Linux security model.
- [ ] Inspect users and groups.
- [ ] Understand advanced file permissions.
- [ ] Understand ACLs.
- [ ] Understand PAM.
- [ ] Understand SSH hardening.
- [ ] Explain SELinux and AppArmor.
- [ ] Understand Linux capabilities.
- [ ] Inspect Linux security logs.
- [ ] Understand auditd.
- [ ] Complete the security inspection lab.
- [ ] Build the Linux security audit script.

**END OF LESSON 11**
