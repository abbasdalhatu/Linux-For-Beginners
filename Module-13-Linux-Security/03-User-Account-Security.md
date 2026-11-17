---
---


# Module 13 – Lesson 03: User Account Security

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux user account security.
2. Understand Linux user and group accounts.
3. Identify different types of Linux accounts.
4. Understand UID and GID.
5. Inspect user account information.
6. Understand account passwords and authentication.
7. Identify inactive and unnecessary accounts.
8. Explain account lockout and expiration.
9. Understand administrative account protection.
10. Perform a basic user account security audit.

---

## 2. Introduction to User Account Security

User account security involves protecting Linux accounts against unauthorised access and misuse.

Every user account represents an identity that can interact with the operating system.

Linux uses accounts to determine:

- Who is accessing the system.
- Which files a user can access.
- Which commands a user can execute.
- Which groups a user belongs to.
- Whether administrative privileges are available.

### Simple Example

Imagine a university computer laboratory.

There are:

- Students.
- Lecturers.
- Laboratory technicians.
- IT administrators.

Each person needs an account with permissions appropriate to their responsibilities.

Students should not automatically receive the same permissions as IT administrators.

Linux uses user accounts, groups, permissions, and privileges to enforce these restrictions.

---

## 3. Types of Linux User Accounts

Linux commonly has several categories of accounts.

### 3.1 Root Account

The root account is the superuser account.

It has UID 0 and can perform almost any administrative operation on the system.

Examples of administrative tasks include:

- Installing software.
- Managing system services.
- Changing system configurations.
- Managing user accounts.
- Modifying protected system files.

Check the root account:

```bash
getent passwd root
```

Example output:

```text
root:x:0:0:root:/root:/bin/bash
```

The important fields include:

- Username: `root`
- UID: `0`
- Primary GID: `0`
- Home directory: `/root`
- Login shell: `/bin/bash`

**Security warning:** Root access should be protected carefully. Unnecessary root access increases the potential impact of mistakes and compromised accounts.

### 3.2 Regular User Accounts

Regular users perform ordinary activities.

Examples:

- Running applications.
- Creating files.
- Managing personal directories.
- Performing assigned tasks.

Inspect your current user:

```bash
whoami
```

Display your user information:

```bash
id
```

### 3.3 System Accounts

System accounts are commonly used by operating-system services and applications.

Examples may include:

- `www-data`
- `messagebus`
- `nobody`

These accounts allow services to operate without needing full root privileges.

Many system accounts are configured with non-interactive login shells.

This helps prevent direct interactive login, although it is not a complete security boundary by itself.

---

## 4. Understanding UID and GID

Linux identifies users and groups using numerical identifiers.

### UID – User Identifier

UID stands for User Identifier.

It is a number assigned to a user account.

### GID – Group Identifier

GID stands for Group Identifier.

It identifies a group associated with user permissions.

Example:

```bash
id
```

Possible output:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

Explanation:

| Field | Meaning |
|---|---|
| UID 1000 | User identifier |
| GID 1000 | Primary group identifier |
| Group kali | Primary group |
| Group sudo | Additional group membership |

UID 0 is reserved for root privileges.

UID ranges for regular and system accounts can vary between Linux distributions and configurations.

---

## 5. Important Linux Account Files

Linux stores account information in several important files.

### 5.1 /etc/passwd

This file stores basic user account information.

Inspect it:

```bash
cat /etc/passwd
```

A typical entry looks like:

```text
kali:x:1000:1000:Kali User:/home/kali:/bin/bash
```

The fields are:

| Position | Meaning |
|---|---|
| 1 | Username |
| 2 | Password placeholder |
| 3 | UID |
| 4 | Primary GID |
| 5 | User description |
| 6 | Home directory |
| 7 | Login shell |

The `x` indicates that password hash information is stored separately rather than directly in this field.

### 5.2 /etc/shadow

This file contains password-related information, including password hashes and account aging information.

Inspect its permissions:

```bash
ls -l /etc/shadow
```

Read its contents only when authorised and necessary.

```bash
sudo less /etc/shadow
```

**Important:** Never copy, publish, or share password hashes. They are sensitive authentication data.

### 5.3 /etc/group

This file stores group information.

Inspect it:

```bash
cat /etc/group
```

Example:

```text
sudo:x:27:kali
```

This indicates that `kali` is listed as a supplementary member of the `sudo` group.

### 5.4 /etc/gshadow

This file stores protected group authentication and membership information.

Inspect its permissions:

```bash
ls -l /etc/gshadow
```

Do not modify it manually during this lesson.

---

## 6. Inspecting Linux User Accounts

Linux provides commands for inspecting accounts.

### Display the Current User

```bash
whoami
```

### Display User and Group Identifiers

```bash
id
```

### Display Group Membership

```bash
groups
```

### Display All User Accounts

```bash
getent passwd
```

### Display a Specific User

```bash
getent passwd kali
```

Replace `kali` with the username you want to inspect.

### Display Logged-in Users

```bash
who
```

### Display Login History

```bash
last
```

These commands help administrators understand who has accounts and who has accessed a system.

---

## 7. User Account Creation and Management

Linux administrators sometimes need to create, modify, or remove accounts.

These operations require care because they affect access to the system.

### 7.1 Creating a User

The general command is:

```bash
sudo adduser username
```

Example:

```bash
sudo adduser labstudent
```

This normally creates an account and guides you through setting its password and other details.

### 7.2 Modifying a User

The `usermod` command changes account properties.

For example:

```bash
sudo usermod -c "Laboratory Student" labstudent
```

This changes the account's descriptive comment.

### 7.3 Deleting a User

The general command is:

```bash
sudo deluser username
```

Deleting accounts can affect ownership and access to files.

Always verify that the account is no longer required and that important data has been preserved.

Do not run account deletion commands on your main or system accounts during this lesson.

---

## 8. Password Security

Passwords are an important part of account protection.

Weak passwords make unauthorised access easier.

### Characteristics of Strong Passwords

A strong password should:

1. Be sufficiently long.
2. Be unique to the account.
3. Avoid predictable information.
4. Not be reused across important services.
5. Be stored securely in a password manager.

Passphrases can be useful because they are easier to remember while still being long and difficult to guess.

### Inspect Password Status

Use:

```bash
sudo passwd -S "$USER"
```

This displays password status information for your current user.

The exact output depends on the distribution.

### Change Your Password

The command is:

```bash
passwd
```

This changes the password for the current account.

Do not share your password with anyone.

---

## 9. Account Locking and Unlocking

Linux provides mechanisms for locking and unlocking password authentication for accounts.

### Lock an Account

General syntax:

```bash
sudo passwd -l username
```

### Unlock an Account

General syntax:

```bash
sudo passwd -u username
```

These commands affect password authentication.

They do not necessarily prevent every possible form of access, such as access through an already established session or another authentication mechanism.

**Safety:** Do not experiment with locking your own active account. You could lose access to your laboratory.

Understand these commands without executing them.

---

## 10. Account Expiration

Administrators may need to configure accounts to expire after a particular date.

This is useful for:

- Temporary employees.
- Contractors.
- Temporary laboratory accounts.
- Short-term training accounts.

The `chage` command manages password aging and account expiration settings.

Inspect account aging information:

```bash
sudo chage -l "$USER"
```

Example information may include:

- Last password change.
- Password expiration.
- Password inactivity period.
- Account expiration date.

Account expiration and password expiration are different concepts.

An expired password may require a password change, while an expired account may prevent normal account use.

---

## 11. Linux Group Security

Groups allow administrators to assign permissions to multiple users.

For example, instead of granting file access separately to 20 students, an administrator can create a group and assign permissions to that group.

### Display Groups

```bash
getent group
```

### Display a User's Groups

```bash
groups "$USER"
```

### Inspect the Sudo Group

```bash
getent group sudo
```

Members of the sudo group are commonly permitted to use sudo, subject to the system's sudo policy.

Membership should be restricted to users who genuinely need administrative access.

---

## 12. Administrative Account Security

Administrative accounts require special protection because they can make system-wide changes.

Recommended practices include:

1. Avoid unnecessary root login.
2. Use individual user accounts.
3. Grant sudo privileges only when required.
4. Use strong, unique passwords.
5. Protect SSH authentication.
6. Review group membership.
7. Remove or disable accounts that are no longer needed.
8. Monitor authentication activity.
9. Keep account information protected.
10. Avoid sharing administrator credentials.

### Inspect Sudo Privileges

Display your current user's groups:

```bash
groups
```

Check whether sudo is available:

```bash
sudo -v
```

This validates your sudo credentials if required.

Inspect permitted sudo commands:

```bash
sudo -l
```

This displays the commands your current account is permitted to execute through sudo.

Do not change sudo configuration files during this lesson.

---

# 13. Practical Lab: Linux User Account Security Audit

## Objective

Perform a basic inspection of user accounts, group membership, account configuration, and login activity.

You will not create, delete, lock, or modify accounts.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson03
cd ~/module13-lab/lesson03
```

### Step 2: Identify Your Current Account

```bash
whoami
id
groups
```

Record your username, UID, GID, and group memberships.

### Step 3: Inspect the Root Account

```bash
getent passwd root
```

Identify the root UID and home directory.

### Step 4: Inspect User Account Entries

```bash
getent passwd
```

Look at several accounts.

Identify which appear to be regular users and which appear to be service accounts.

Do not assume every account with a particular UID range has the same purpose across all distributions.

### Step 5: Inspect Sensitive Account File Permissions

```bash
ls -l /etc/passwd /etc/shadow /etc/group /etc/gshadow
```

Observe the differences in access permissions.

Do not change these permissions.

### Step 6: Inspect Group Membership

```bash
getent group sudo
```

Inspect your own groups:

```bash
groups "$USER"
```

### Step 7: Inspect Login Activity

```bash
who
```

Then:

```bash
last
```

The amount of historical information available depends on the system's login accounting configuration.

### Step 8: Inspect Password Aging

```bash
sudo chage -l "$USER"
```

Review the information without modifying it.

### Step 9: Create Your Account Security Report

Run:

```bash
{
    echo "LINUX USER ACCOUNT SECURITY AUDIT"
    echo "================================="
    echo
    echo "CURRENT USER"
    whoami
    echo
    echo "USER IDENTIFICATION"
    id
    echo
    echo "GROUP MEMBERSHIP"
    groups
    echo
    echo "ROOT ACCOUNT"
    getent passwd root
    echo
    echo "ACCOUNT FILE PERMISSIONS"
    ls -l /etc/passwd /etc/shadow /etc/group /etc/gshadow
    echo
    echo "SUDO GROUP INFORMATION"
    getent group sudo
    echo
    echo "CURRENT LOGIN SESSIONS"
    who
    echo
    echo "RECENT LOGIN HISTORY"
    last
    echo
    echo "PASSWORD AGING INFORMATION"
    sudo chage -l "$USER"
} > user-account-security-report.txt 2>&1
```

Inspect your report:

```bash
cat user-account-security-report.txt
```

### Expected Files

```text
lesson03/
└── user-account-security-report.txt
```

---

## 14. Real-World Cybersecurity Application

Imagine that you are auditing a university Linux server.

You discover:

- Several regular user accounts.
- Multiple service accounts.
- A sudo group.
- An account belonging to a former employee.
- A service account with unnecessary interactive login capability.

Your job is to investigate and document these observations.

Do not immediately delete accounts.

First establish:

1. Whether the account is still required.
2. Who owns the account.
3. Whether any running services depend on it.
4. Which files it owns.
5. Whether it has administrative privileges.
6. What access restrictions are appropriate.

This demonstrates an important security principle:

**Investigate before changing access controls.**

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is Linux user account security?
2. What is the root account?
3. What is the difference between a regular user and a system account?
4. What does UID mean?
5. What does GID mean?
6. What information is stored in `/etc/passwd`?
7. What is the purpose of `/etc/shadow`?
8. What information is stored in `/etc/group`?
9. Why should sudo privileges be restricted?
10. What is the purpose of the `passwd` command?
11. What is the difference between locking an account and expiring an account?
12. What is the purpose of the `chage` command?
13. Why are individual user accounts preferable to shared administrator accounts?
14. Why should administrators inspect inactive accounts?
15. Explain how user account security supports the Principle of Least Privilege.

---

## 16. Practical Completion Checklist

- [ ] I understand Linux user account security.
- [ ] I understand root, regular, and system accounts.
- [ ] I understand UID and GID.
- [ ] I understand `/etc/passwd`.
- [ ] I understand `/etc/shadow`.
- [ ] I understand `/etc/group` and `/etc/gshadow`.
- [ ] I can inspect users and group membership.
- [ ] I understand password security.
- [ ] I understand account locking and expiration.
- [ ] I understand sudo privileges.
- [ ] I completed the user account security audit.
- [ ] I generated my account security report.

**Lesson 03 Complete: User Account Security.**
