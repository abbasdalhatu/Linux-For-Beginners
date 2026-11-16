
# MODULE 15 – CAPSTONE PROJECT

# LESSON 05: SYSTEM HARDENING AND REMEDIATION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain system hardening.
2. Understand the principle of least privilege.
3. Identify unnecessary services.
4. Review Linux users and permissions.
5. Understand SSH security.
6. Configure basic firewall protection.
7. Apply security updates safely.
8. Document remediation activities.

---

## 2. What Is System Hardening?

System hardening is the process of reducing security weaknesses by improving system configurations and removing unnecessary exposure.

A system may work correctly but still have poor security configurations.

For example:
- Unnecessary services are running.
- Default accounts remain enabled.
- Weak passwords are permitted.
- Excessive permissions are assigned.
- Firewall protection is missing.
- Security updates are not installed.

Hardening reduces the opportunities available to attackers.

## 3. Principle of Least Privilege

The principle of least privilege means giving users and applications only the permissions they need to perform their duties.

Example:

A standard user who only needs to read a document should not receive administrator privileges.

Benefits:
- Reduces unauthorized access.
- Limits damage from compromised accounts.
- Improves accountability.
- Reduces unnecessary privileges.

## 4. System Hardening Areas

| Area | Security Improvement |
|---|---|
| User accounts | Remove unnecessary accounts |
| Passwords | Enforce strong authentication |
| Services | Disable unnecessary services |
| Permissions | Restrict access |
| SSH | Use secure remote administration |
| Firewall | Restrict unwanted traffic |
| Updates | Patch known weaknesses |
| Logging | Monitor security events |

## 5. Practical Lab A: Inspect System Information

On Kali Linux, run:

```bash
hostnamectl
```

Check the operating system:

```bash
cat /etc/os-release
```

Check the current user:

```bash
whoami
```

Check privileges:

```bash
id
```

Check uptime:

```bash
uptime
```

Record the results in your lab notes.

## 6. Practical Lab B: Review User Accounts

List local accounts:

```bash
cut -d: -f1 /etc/passwd
```

Review accounts with administrative privileges:

```bash
getent group sudo
```

Review the current user's groups:

```bash
groups
```

Understand:
- Which accounts are interactive users?
- Which accounts are service accounts?
- Which users have administrative privileges?

Do not delete accounts simply because you do not recognize their names. Linux services often require dedicated accounts.

## 7. Practical Lab C: Inspect File Permissions

Create a practice directory:

```bash
mkdir -p ~/practical-labs/module15/hardening
cd ~/practical-labs/module15/hardening
```

Create a sample file:

```bash
echo "Confidential laboratory information" > sample.txt
```

Inspect permissions:

```bash
ls -l sample.txt
```

Set owner-only read and write permissions:

```bash
chmod 600 sample.txt
```

Verify:

```bash
ls -l sample.txt
```

Expected permission pattern:

```text
-rw-------
```

This demonstrates restricting access to the file owner.

## 8. Practical Lab D: Inspect Running Services

List active services:

```bash
systemctl list-units --type=service --state=running
```

Check a specific service:

```bash
systemctl status ssh
```

List enabled services:

```bash
systemctl list-unit-files --type=service --state=enabled
```

### Important

Do not disable services without understanding their purpose.

For this exercise, inspect and document services only. Do not make unnecessary changes to your Kali VM.

## 9. Practical Lab E: Review SSH Security

SSH provides encrypted remote administration.

Inspect the SSH configuration:

```bash
sudo nano /etc/ssh/sshd_config
```

Review these settings if present:

```text
PermitRootLogin
PasswordAuthentication
PubkeyAuthentication
```

Security considerations:

- Avoid direct root login.
- Prefer key-based authentication where practical.
- Use strong authentication.
- Restrict access to authorized users.
- Keep SSH software updated.

Do not change authentication settings on a machine you depend on without first ensuring you have a working recovery method.

Exit without making changes.

## 10. Practical Lab F: Inspect Firewall Status

Check UFW:

```bash
sudo ufw status verbose
```

If UFW is installed, inspect its configuration:

```bash
sudo ufw show added
```

Check whether nftables is available:

```bash
sudo nft list ruleset
```

A firewall helps control network traffic according to defined rules.

For this lesson, inspect the existing firewall configuration. Do not enable or modify firewall rules blindly, especially while connected remotely.

## 11. Practical Lab G: Review Security Updates

Refresh package information:

```bash
sudo apt update
```

List packages that can be upgraded:

```bash
apt list --upgradable
```

Review the available updates.

If you decide to install updates, use the normal package manager:

```bash
sudo apt upgrade
```

Read the package manager's proposed changes before confirming.

Do not use unfamiliar upgrade scripts or commands from untrusted websites.

## 12. Practical Lab H: Create a Hardening Checklist

Create a directory:

```bash
mkdir -p ~/practical-labs/module15/hardening
```

Create the checklist:

```bash
cat > ~/practical-labs/module15/hardening/hardening-checklist.md <<'EOF'
# SYSTEM HARDENING CHECKLIST

## System
- [ ] Operating system identified
- [ ] System updates reviewed
- [ ] Unnecessary software identified

## Accounts
- [ ] User accounts reviewed
- [ ] Administrative privileges reviewed
- [ ] Unnecessary accounts investigated

## Permissions
- [ ] Sensitive file permissions reviewed
- [ ] Excessive permissions identified

## Services
- [ ] Running services reviewed
- [ ] Enabled services reviewed
- [ ] Unnecessary services identified

## SSH
- [ ] SSH configuration reviewed
- [ ] Root login policy reviewed
- [ ] Authentication methods reviewed

## Firewall
- [ ] Firewall status checked
- [ ] Existing rules reviewed
- [ ] Network exposure documented

## Monitoring
- [ ] Security logs identified
- [ ] Monitoring requirements documented

## Conclusion
[Write your observations here.]
EOF
```

Edit the checklist and record your observations.

## 13. Practical Lab I: Create a Remediation Plan

A remediation plan describes how identified security weaknesses should be corrected.

Create:

```bash
cat > ~/practical-labs/module15/hardening/remediation-plan.md <<'EOF'
# SYSTEM REMEDIATION PLAN

## Project
Isolated Cybersecurity Assessment and Defense Lab

## Objective
Document improvements required to reduce system risk.

## Remediation 1: Unnecessary Services
Action:
Identify services that are not required.

Validation:
Review service configuration and operational requirements.

## Remediation 2: Weak Permissions
Action:
Apply appropriate file and directory permissions.

Validation:
Inspect permissions using ls -l and stat.

## Remediation 3: Outdated Software
Action:
Review and install applicable security updates.

Validation:
Check installed package versions and vendor advisories.

## Remediation 4: Remote Access
Action:
Review SSH authentication and access restrictions.

Validation:
Inspect SSH configuration.

## Remediation 5: Network Protection
Action:
Review firewall policies and unnecessary exposure.

Validation:
Inspect firewall rules and network connectivity.

## Change Management
Record:
- Original configuration
- Proposed change
- Reason for change
- Approval
- Implementation date
- Validation result
- Rollback plan

## Final Status
[Pending / Completed / Requires Further Review]
EOF
```

## 14. Understanding Change Management

Security changes should be planned and documented.

A professional remediation process includes:

1. Identify the weakness.
2. Understand the affected system.
3. Plan the correction.
4. Assess possible operational impact.
5. Obtain required authorization.
6. Apply the change.
7. Verify that it works.
8. Record the result.
9. Maintain a rollback plan.

A poorly planned security change can interrupt legitimate services.

## 15. Knowledge Check

1. What is system hardening?
2. Explain the principle of least privilege.
3. Why should unnecessary services be removed?
4. What does `chmod 600` do?
5. Why should administrator privileges be restricted?
6. What is SSH used for?
7. What is the purpose of a firewall?
8. Why are software updates important?
9. What is a remediation plan?
10. Why is rollback planning necessary?

## 16. Practical Completion Checklist

- [ ] I inspected system information.
- [ ] I reviewed local user accounts.
- [ ] I inspected administrative privileges.
- [ ] I practiced file permission hardening.
- [ ] I inspected running services.
- [ ] I reviewed SSH configuration.
- [ ] I checked firewall status.
- [ ] I reviewed available updates.
- [ ] I created a hardening checklist.
- [ ] I created a remediation plan.

## 17. Key Takeaway

System hardening reduces the attack surface of a computer by removing unnecessary exposure, strengthening configurations, controlling privileges, and applying security updates.

**Remember: Security is not only about detecting weaknesses. It is also about correcting them safely and verifying that the corrections work.**
