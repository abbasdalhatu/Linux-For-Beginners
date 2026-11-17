---
---


# Module 13 – Lesson 06: Authentication and Password Security

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define authentication and explain why it matters.
2. Differentiate authentication from authorisation.
3. Understand Linux password storage.
4. Explain password hashing and salting.
5. Inspect password and account policies.
6. Understand password expiration and account locking.
7. Explain multi-factor authentication (MFA).
8. Identify common authentication attacks.
9. Understand authentication logs.
10. Perform a basic Linux authentication security audit.

---

## 2. What Is Authentication?

Authentication is the process of verifying the identity of a person, device, or system attempting to access a resource.

In simple terms, authentication answers:

**Who are you?**

For example, when you log into Kali Linux, the system checks your username and password before allowing you to access your account.

### Common Authentication Methods

1. Password authentication.
2. PIN authentication.
3. SSH key authentication.
4. Biometric authentication.
5. Multi-factor authentication.
6. Security tokens.

### Real-World Example

Imagine accessing your university student portal.

You enter:

- Your student username.
- Your password.

The portal verifies your credentials before granting access.

That verification process is authentication.

---

## 3. Authentication vs Authorisation

These two concepts are closely related but different.

### Authentication

Verifies who you are.

### Authorisation

Determines what you are allowed to do.

| Authentication | Authorisation |
|---|---|
| Verifies identity | Determines permissions |
| Answers "Who are you?" | Answers "What can you access?" |
| Usually happens first | Usually follows authentication |
| Example: Login password | Example: Permission to view examination records |

### Example

A university administrator logs in successfully.

Authentication confirms the administrator's identity.

Authorisation determines whether that administrator can modify student records.

A successful login does not automatically grant access to everything.

---

## 4. Understanding Linux Password Storage

Linux does not normally store user passwords as readable plain text.

Instead, it uses password hashing.

Two important files are:

```text
/etc/passwd
/etc/shadow
```

### 4.1 The /etc/passwd File

This file stores general account information.

Inspect it using:

```bash
cat /etc/passwd
```

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
| Student | Account description |
| /home/student | Home directory |
| /bin/bash | Login shell |

### 4.2 The /etc/shadow File

This file stores password-related information, including password hashes and account ageing information.

Inspect its permissions:

```bash
ls -l /etc/shadow
```

Inspect its metadata:

```bash
sudo stat /etc/shadow
```

Only authorised administrators should access its contents.

Do not copy password hashes into reports or share them.

### Important Security Principle

The `/etc/shadow` file must be protected because stolen password hashes can potentially be attacked offline.

---

## 5. Password Hashing

A password hash is the output of a one-way cryptographic function applied to a password.

For example:

```text
Original password → Hashing algorithm → Password hash
```

When a user logs in, Linux verifies the supplied password against the stored password hash using the configured password authentication mechanism.

### Hashing vs Encryption

| Hashing | Encryption |
|---|---|
| Designed to be one-way | Designed to be reversible with the correct key |
| Used for password verification | Used to protect data confidentiality |
| Does not normally have a decryption operation | Can be decrypted with the appropriate key |
| Example: Argon2 | Example: AES |

### Common Password Hashing Algorithms

- yescrypt
- Argon2
- bcrypt
- scrypt
- PBKDF2

The algorithm available depends on the Linux distribution and authentication configuration.

Modern password storage should use a dedicated password-hashing scheme rather than a fast general-purpose hash such as plain SHA-256.

---

## 6. Understanding Password Salting

A salt is a random value added to a password before hashing.

### Why Is Salting Important?

Suppose two users choose the same password.

Without unique salts, their stored hashes could be identical when the same hashing algorithm and parameters are used.

With unique salts, the resulting password hashes are different.

### Simplified Example

```text
Password: MyPassword123
Salt A: random-value-A
Salt B: random-value-B
```

The password combined with Salt A produces a different hash from the password combined with Salt B.

### Benefits

- Makes precomputed rainbow-table attacks less useful.
- Prevents identical passwords from automatically producing identical stored hashes.
- Increases the cost of attacking many password hashes.

A salt is not a secret password. It is stored alongside the password hash.

---

## 7. Linux Password Management Commands

Linux provides several commands for inspecting and managing account password information.

### 7.1 passwd

Changes a user's password.

```bash
passwd
```

Changes the current user's password.

An administrator can change another account's password with:

```bash
sudo passwd username
```

Only perform account changes when authorised.

### 7.2 chage

The `chage` command manages password ageing information.

Inspect your account:

```bash
chage -l "$USER"
```

It may display:

- Last password change.
- Password expiration.
- Minimum password age.
- Maximum password age.
- Warning period.
- Account inactivity information.

### 7.3 passwd -S

Inspect password status:

```bash
passwd -S "$USER"
```

The output may show whether a password is set, locked, or in another status.

Do not experiment with locking your own account.

---

## 8. Password Expiration and Account Ageing

Password ageing policies control how long a password may remain valid and when users should be warned to change it.

Important settings include:

### Minimum Password Age

The minimum number of days before a user can change a password again.

### Maximum Password Age

The maximum number of days a password can remain valid under the configured policy.

### Warning Period

The number of days before expiration when a user receives a warning.

### Inactive Period

The period after password expiration before an account may become inactive.

### Account Expiration

A date after which an account is no longer permitted to log in.

### Inspect Password Ageing

```bash
chage -l "$USER"
```

Password expiration policies should reflect the organisation's risk model and authentication strategy.

Regular forced password changes are not automatically safer in every situation. Strong unique passwords, MFA, secure storage, and prompt changes after suspected compromise are also important.

---

## 9. Multi-Factor Authentication (MFA)

MFA requires users to provide two or more different types of authentication evidence.

### Three Common Authentication Factors

**1. Something you know**

- Password
- PIN
- Passphrase

**2. Something you have**

- Hardware security key
- Authenticator application
- Security token

**3. Something you are**

- Fingerprint
- Facial recognition
- Other biometric characteristics

### Example

A user logs into an online account using:

1. A password.
2. A one-time code from an authenticator application.

This is an example of two-factor authentication when the factors are independent.

### Why MFA Matters

If an attacker steals a password, MFA can provide an additional barrier to account compromise.

Phishing-resistant authentication methods, such as properly configured FIDO2 security keys and passkeys, offer stronger protection against many phishing attacks than ordinary one-time codes.

---

## 10. Common Authentication Attacks

### 10.1 Brute-Force Attack

An attacker repeatedly attempts different passwords until a valid password is discovered.

### 10.2 Dictionary Attack

An attacker tries passwords from a prepared list of common words and known passwords.

### 10.3 Credential Stuffing

An attacker uses username and password combinations stolen from other services.

This works when users reuse passwords.

### 10.4 Phishing

An attacker tricks a user into revealing authentication credentials.

### 10.5 Password Spraying

An attacker attempts a small number of common passwords against many accounts.

### 10.6 Offline Password Cracking

An attacker obtains password hashes and attempts to recover the original passwords without interacting with the login service.

### Defensive Measures

- Use long, unique passwords.
- Use a reputable password manager.
- Enable MFA.
- Protect password hashes.
- Monitor authentication failures.
- Apply sensible login restrictions.
- Avoid password reuse.
- Keep authentication software updated.

---

## 11. Linux Authentication Logs

Authentication logs help administrators investigate successful and unsuccessful login attempts.

On Debian-based systems, common locations include:

```text
/var/log/auth.log
```

Some systems primarily use the system journal.

### Inspect Authentication Logs

Try:

```bash
sudo journalctl -b --no-pager | grep -Ei 'authentication|failed password|accepted password|sudo' | tail -n 30
```

If the authentication log file exists:

```bash
sudo tail -n 30 /var/log/auth.log
```

The exact log location and available events depend on the distribution and logging configuration.

### Why Logs Matter

Logs can help identify:

- Failed login attempts.
- Successful authentication.
- Sudo activity.
- SSH authentication events.
- Repeated suspicious login attempts.

Do not assume every failed login is malicious. Investigate patterns and context.

---

## 12. Important Linux Authentication Files

| File or Directory | Purpose |
|---|---|
| `/etc/passwd` | General user account information |
| `/etc/shadow` | Password hashes and password ageing information |
| `/etc/group` | Group information |
| `/etc/gshadow` | Protected group information |
| `/etc/pam.d/` | PAM authentication configuration |
| `/etc/login.defs` | Login and account-related defaults |
| `/var/log/auth.log` | Authentication log on systems using this file |

### What Is PAM?

PAM stands for **Pluggable Authentication Modules**.

It provides a flexible framework for authentication services.

Applications can use PAM to authenticate users without implementing every authentication mechanism themselves.

For example, PAM can be involved in:

- Local login.
- Password authentication.
- Sudo authentication.
- SSH authentication, depending on configuration.

Inspect PAM configuration files:

```bash
ls -l /etc/pam.d/
```

Do not modify PAM configuration during this lesson.

Incorrect changes can prevent users from logging in.

---

# 13. Practical Lab: Authentication and Password Security Audit

## Lab Objective

Inspect your Kali Linux authentication configuration, account password status, password ageing information, and authentication logs.

This is an inspection-only exercise.

Do not change passwords, lock accounts, edit PAM files, or modify authentication policies.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson06
cd ~/module13-lab/lesson06
```

### Step 2: Identify Your Current User

```bash
whoami
```

### Step 3: Inspect Account Information

```bash
getent passwd "$USER"
```

### Step 4: Inspect Password File Permissions

```bash
ls -l /etc/passwd /etc/shadow
```

### Step 5: Inspect Password File Metadata

```bash
sudo stat /etc/shadow
```

Do not display or copy its password hashes.

### Step 6: Inspect Your Password Status

```bash
passwd -S "$USER"
```

### Step 7: Inspect Password Ageing Information

```bash
chage -l "$USER"
```

### Step 8: Inspect Login Defaults

```bash
grep -E '^(PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE|ENCRYPT_METHOD)' /etc/login.defs
```

Some settings may be absent or may not represent the complete effective authentication policy.

### Step 9: Inspect PAM Configuration

```bash
ls -l /etc/pam.d/
```

### Step 10: Inspect Authentication Events

```bash
sudo journalctl -b --no-pager | grep -Ei 'authentication|failed password|accepted password|sudo' | tail -n 30
```

If available, also inspect:

```bash
sudo tail -n 30 /var/log/auth.log
```

### Step 11: Generate Your Authentication Security Report

Run:

```bash
{
    echo "LINUX AUTHENTICATION AND PASSWORD SECURITY AUDIT"
    echo "================================================="
    echo
    echo "CURRENT USER"
    whoami
    echo
    echo "ACCOUNT INFORMATION"
    getent passwd "$USER"
    echo
    echo "PASSWORD FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow
    echo
    echo "SHADOW FILE METADATA"
    sudo stat /etc/shadow
    echo
    echo "PASSWORD STATUS"
    passwd -S "$USER"
    echo
    echo "PASSWORD AGEING INFORMATION"
    chage -l "$USER"
    echo
    echo "LOGIN DEFAULTS"
    grep -E '^(PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE|ENCRYPT_METHOD)' /etc/login.defs
    echo
    echo "PAM CONFIGURATION DIRECTORY"
    ls -l /etc/pam.d/
    echo
    echo "RECENT AUTHENTICATION EVENTS"
    sudo journalctl -b --no-pager | grep -Ei 'authentication|failed password|accepted password|sudo' | tail -n 30
} > authentication-security-report.txt 2>&1
```

Inspect your report:

```bash
cat authentication-security-report.txt
```

**Privacy warning:** Before sharing the report, review it and remove usernames, host details, IP addresses, and other information you do not want to disclose.

### Expected Files

```text
lesson06/
└── authentication-security-report.txt
```

---

## 14. Real-World Cybersecurity Application

Imagine you are auditing authentication security for a university server.

Your investigation discovers:

- Several failed login attempts.
- Users sharing passwords.
- Weak password policies.
- No MFA on important administrative accounts.
- Poor protection of password-related files.

Your security report should identify the risks and recommend:

1. Strong, unique passwords.
2. MFA for privileged accounts.
3. Proper protection of password hashes.
4. Appropriate password ageing policies.
5. Authentication log monitoring.
6. Secure account management.
7. Investigation of suspicious login activity.

This is the foundation of Linux authentication security auditing.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is authentication?
2. What is authorisation?
3. Explain the difference between authentication and authorisation.
4. What information is stored in `/etc/passwd`?
5. What is the purpose of `/etc/shadow`?
6. Why should password hashes be protected?
7. What is password hashing?
8. Differentiate between hashing and encryption.
9. What is a password salt?
10. What is password ageing?
11. Explain the purpose of the `chage` command.
12. What is MFA?
13. Identify the three main authentication factor categories.
14. Explain brute-force and dictionary attacks.
15. What is credential stuffing?
16. What is PAM?
17. Why are authentication logs important?
18. Identify three ways to improve Linux authentication security.

---

## 16. Practical Completion Checklist

- [ ] I understand authentication.
- [ ] I understand authorisation.
- [ ] I understand Linux password storage.
- [ ] I understand password hashing and salting.
- [ ] I can inspect password file permissions.
- [ ] I can inspect password status.
- [ ] I understand password ageing.
- [ ] I understand MFA.
- [ ] I can identify common authentication attacks.
- [ ] I understand Linux authentication logs.
- [ ] I understand PAM.
- [ ] I completed the authentication security audit.
- [ ] I generated my authentication security report.

**Lesson 06 Complete: Authentication and Password Security.**
