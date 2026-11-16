
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 04: PASSWORD POLICIES AND AUTHENTICATION

### 1. Introduction

Passwords are one of the most common methods used to authenticate users on Linux systems.

A password policy is a set of rules that determines how passwords should be created, protected, changed, and managed.

A weak password policy can expose a Linux system to:

- Brute-force attacks.
- Password guessing.
- Credential stuffing.
- Unauthorized account access.
- Password reuse attacks.
- Account compromise.

In this lab, you will investigate Linux password management, authentication files, password aging, and password security policies.

### 2. What Is Authentication?

Authentication is the process of verifying the identity of a user or system.

For example, when you log into Kali Linux, the operating system verifies your credentials before granting access.

Common authentication methods include:

1. Password authentication.
2. SSH key authentication.
3. Multi-factor authentication.
4. Biometric authentication.
5. Certificate-based authentication.

### 3. What Is a Password Policy?

A password policy defines the requirements users must follow when creating and maintaining passwords.

Common password policy elements include:

- Minimum password length.
- Password complexity.
- Password history.
- Password expiration.
- Failed login restrictions.
- Account lockout.
- Password reset requirements.

A good password policy should balance security and usability.

### 4. Characteristics of Strong Passwords

A strong password should:

- Be sufficiently long.
- Be unique to each account.
- Avoid predictable patterns.
- Avoid personal information.
- Not be reused across important services.
- Be stored securely.

Example of a weak password:

```text
password123
```

Example of a stronger passphrase:

```text
River!Laptop7Cloud#Garden
```

These are examples only. Do not use them as your real passwords.

A password manager can help generate and store unique credentials.

### 5. Understanding Linux Password Storage

Linux uses several files and components to manage authentication.

| File or component | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Password hashes and password aging data |
| `/etc/login.defs` | Login and account defaults |
| PAM | Pluggable Authentication Modules |
| `pam_pwquality` | Password quality enforcement when configured |
| `pam_faillock` | Authentication failure tracking and lockout when configured |

### 6. Password Hashing

Linux does not normally store user passwords as readable plaintext in `/etc/shadow`.

Instead, password authentication commonly uses password hashes.

A hash function transforms input data into a fixed-length representation.

Password hashing systems use additional protections such as salts and computationally expensive algorithms to make offline password guessing harder.

Common password hash formats may include:

- SHA-512 crypt.
- Yescrypt.
- Other supported password hashing schemes.

The actual configuration depends on the distribution.

**Important:** A password hash is sensitive authentication information. Never publish, email, or include password hashes in your reports.

### 7. Inspect Password File Permissions

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module06/lab04
cd ~/practical-labs/module06/lab04
```

Inspect the account database:

```bash
ls -l /etc/passwd
```

Inspect the shadow file permissions:

```bash
ls -l /etc/shadow
```

Inspect login configuration:

```bash
ls -l /etc/login.defs
```

Explain why `/etc/shadow` requires restricted access.

Do not display the contents of `/etc/shadow`.

### 8. Inspect Password Policy Defaults

Run:

```bash
grep -E '^(PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE|PASS_MIN_LEN)' /etc/login.defs
```

These settings may define default password aging behavior for newly created accounts.

Understand the options:

| Setting | Purpose |
|---|---|
| PASS_MAX_DAYS | Maximum password age |
| PASS_MIN_DAYS | Minimum time between password changes |
| PASS_WARN_AGE | Warning period before expiration |
| PASS_MIN_LEN | Legacy minimum-length setting; not necessarily the effective password-quality control |

Modern password-quality enforcement may be handled through PAM modules instead.

### 9. Inspect Your Current Account's Password Aging

Run:

```bash
sudo chage -l "$(whoami)"
```

This displays password aging information for your account.

Look for:

- Last password change.
- Password expiration.
- Minimum password age.
- Maximum password age.
- Warning period.
- Account expiration.

Do not change your normal account's password aging settings during this lab.

### 10. Understand PAM Authentication

PAM means Pluggable Authentication Modules.

PAM provides a flexible framework for Linux authentication.

Applications can use PAM to authenticate users without implementing every authentication mechanism themselves.

PAM may manage:

- Password verification.
- Password quality checks.
- Authentication failure limits.
- Account restrictions.
- Session configuration.

Inspect PAM configuration files:

```bash
ls -l /etc/pam.d/
```

Inspect the common authentication configuration:

```bash
cat /etc/pam.d/common-auth
```

Inspect password configuration:

```bash
cat /etc/pam.d/common-password
```

Kali and Debian-based systems may use different PAM configurations depending on installed packages and system setup.

Read these files only. Do not edit them in this introductory lab.

### 11. Understand Password Quality Enforcement

The `pam_pwquality` module can enforce password quality requirements.

It may check:

- Minimum password length.
- Character variety.
- Similarity to previous passwords.
- Dictionary-based weaknesses.
- Password reuse restrictions, depending on configuration.

Check whether the package is installed:

```bash
dpkg -s libpam-pwquality
```

If it is not installed, record that fact.

Installation is not required for this lab.

If available, inspect its configuration:

```bash
cat /etc/security/pwquality.conf
```

Do not change the configuration yet.

### 12. Understand Authentication Failure Controls

Repeated failed login attempts can indicate password guessing or brute-force activity.

Linux can use PAM modules to track failed authentication attempts.

One example is `pam_faillock`.

Check whether its configuration is present:

```bash
grep -R "pam_faillock" /etc/pam.d/ 2>/dev/null
```

If the module is configured, inspect the relevant configuration carefully.

Do not deliberately generate repeated failed login attempts on your normal account.

An account lockout policy can accidentally prevent legitimate access if configured incorrectly.

### 13. Inspect Authentication Logs

Authentication events may be recorded in system logs.

First, inspect the journal:

```bash
sudo journalctl -b --no-pager | grep -Ei 'authentication|login|sudo|failed'
```

On systems using traditional authentication logs, inspect their existence:

```bash
ls -l /var/log/auth.log
```

If the file exists:

```bash
sudo tail -30 /var/log/auth.log
```

Some Kali installations may rely more heavily on journald.

Record relevant observations without copying passwords, tokens, or other secrets.

### 14. Practical Exercise – Inspect Password Security Settings

Create a file:

```bash
nano password-policy-check.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module06/lab04/password-policy-report.txt"

{
    echo "LINUX PASSWORD POLICY REPORT"
    echo "============================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "CURRENT USER"
    whoami
    echo

    echo "LOGIN DEFAULTS"
    grep -E '^(PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE|PASS_MIN_LEN)' \
        /etc/login.defs
    echo

    echo "PASSWORD FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow
    echo

    echo "PASSWORD AGING"
    sudo chage -l "$(whoami)"
    echo

    echo "PAM CONFIGURATION FILES"
    ls -l /etc/pam.d/
    echo

    echo "PASSWORD QUALITY PACKAGE STATUS"
    dpkg -s libpam-pwquality 2>/dev/null | grep -E '^(Package|Status|Version)'
    echo

    echo "PASSWORD QUALITY CONFIGURATION"
    if [ -f /etc/security/pwquality.conf ]; then
        grep -vE '^[[:space:]]*(#|$)' /etc/security/pwquality.conf
    else
        echo "Configuration file not found."
    fi
    echo

    echo "AUTHENTICATION FAILURE MODULE REFERENCES"
    grep -R "pam_faillock" /etc/pam.d/ 2>/dev/null
    echo

    echo "RECENT AUTHENTICATION-RELATED JOURNAL EVENTS"
    sudo journalctl -b -n 100 --no-pager |
        grep -Ei 'authentication|login|sudo|failed' |
        tail -30

} > "$REPORT" 2>&1

echo "Password policy report generated:"
echo "$REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x password-policy-check.sh
```

Run:

```bash
./password-policy-check.sh
```

Inspect:

```bash
less password-policy-report.txt
```

Press `q` to exit.

### 15. Password Security Best Practices

A Linux administrator should:

1. Encourage unique passwords.
2. Use sufficiently long passphrases.
3. Protect authentication files.
4. Avoid shared accounts.
5. Monitor failed login attempts.
6. Apply appropriate password policies.
7. Use SSH keys for suitable remote administration scenarios.
8. Enable multi-factor authentication where appropriate.
9. Avoid storing passwords in scripts.
10. Review authentication configuration changes before applying them.

### 16. Real-World Security Scenario

**Situation:**

A university server administrator discovers repeated failed login attempts against several user accounts.

The administrator suspects password guessing.

A responsible investigation should:

1. Identify the affected accounts.
2. Establish when the failures occurred.
3. Determine whether the attempts originated from one or multiple sources.
4. Check whether any successful login followed the failures.
5. Review authentication configuration.
6. Determine whether account restrictions are necessary.
7. Preserve relevant evidence.
8. Document findings and recommended actions.

Do not assume that every failed login is malicious. Mistyped passwords and expired credentials can also produce failures.

### 17. Knowledge Check

1. What is authentication?
2. What is a password policy?
3. Identify five characteristics of a strong password.
4. What is password hashing?
5. What is a salt?
6. What is the purpose of `/etc/shadow`?
7. What is PAM?
8. What is `pam_pwquality`?
9. What is `pam_faillock`?
10. What does `chage -l` display?
11. Why should authentication logs be monitored?
12. What is a brute-force attack?
13. Why should passwords not be reused?
14. Why should authentication configuration files be changed carefully?
15. How can a password manager improve security?

### 18. Lab Documentation

Create:

```bash
nano lab04-report.txt
```

Record:

```text
MODULE 06 – LAB 04
PASSWORD POLICIES AND AUTHENTICATION

1. Current username:
2. /etc/passwd permissions:
3. /etc/shadow permissions:
4. Password aging defaults:
5. Current account password aging:
6. PAM configuration inspected:
7. Is pam_pwquality installed?
8. Is pwquality configuration present?
9. Are pam_faillock references configured?
10. Where are authentication events recorded?
11. What is password hashing?
12. What is PAM?
13. Five password security best practices:
14. What did I learn?

PASSWORD POLICY REPORT:
Generated successfully: YES / NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 19. Completion Checklist

- [ ] Understood authentication.
- [ ] Studied password policies.
- [ ] Learned password hashing.
- [ ] Inspected `/etc/passwd` permissions.
- [ ] Inspected `/etc/shadow` permissions.
- [ ] Reviewed `/etc/login.defs`.
- [ ] Inspected password aging.
- [ ] Studied PAM.
- [ ] Inspected password quality configuration.
- [ ] Investigated authentication failure controls.
- [ ] Reviewed authentication logs.
- [ ] Generated the password policy report.
- [ ] Completed lab documentation.

**LAB 04 STATUS: READY FOR PRACTICAL COMPLETION**
