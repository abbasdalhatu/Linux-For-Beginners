---
---

# Module 07 – Users and Permissions

# Lesson 04: Creating Users with useradd and adduser

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain how Linux user accounts are created.
2. Understand the difference between `useradd` and `adduser`.
3. Create a new Linux user.
4. Set a password for a user.
5. Create a user's home directory.
6. Verify that a user account exists.
7. Understand important security considerations when creating accounts.

---

## 2. Introduction to User Creation

Linux allows administrators to create multiple user accounts on a single computer.

For example, a cybersecurity laboratory might contain these accounts:

| Username | Purpose |
|---|---|
| kali | Main laboratory account |
| analyst | Security analysis |
| developer | Programming practice |
| tester | Testing applications |

Each account has its own identity and can have its own home directory, groups, and permissions.

Creating accounts is an important responsibility of a Linux system administrator.

**Important:** User creation normally requires administrative privileges.

---

## 3. Understanding useradd

`useradd` is a Linux command used to create user accounts.

It is a low-level account management utility.

### Basic Syntax

```bash
sudo useradd username
```

Example:

```bash
sudo useradd analyst
```

This creates an account named `analyst`.

However, depending on the distribution and configuration, this command may not automatically create a home directory.

### Creating a User with a Home Directory

Use:

```bash
sudo useradd -m analyst
```

The `-m` option creates the user's home directory if it does not exist.

Expected directory:

```text
/home/analyst
```

### Creating a User with a Specific Shell

```bash
sudo useradd -m -s /bin/bash analyst
```

Explanation:

- `sudo`: Runs the command with administrative privileges.
- `useradd`: Creates a user.
- `-m`: Creates the home directory.
- `-s`: Specifies the login shell.
- `/bin/bash`: Selects Bash as the shell.
- `analyst`: Username.

### Creating a User with a Description

```bash
sudo useradd -m -c "Security Analyst" analyst
```

The `-c` option sets the account description.

---

## 4. Understanding adduser

`adduser` is a higher-level utility available on Debian-based distributions such as Kali Linux.

It provides a more interactive account creation experience.

Example:

```bash
sudo adduser analyst
```

It typically asks for:

1. A password.
2. Password confirmation.
3. Full name or account description.
4. Additional optional information.

It normally creates the home directory and sets up the account more conveniently.

### Important Difference

| useradd | adduser |
|---|---|
| Lower-level utility | Higher-level utility |
| Often used with explicit options | Often interactive |
| Home directory may require `-m` | Normally creates home directory |
| Common across Linux distributions | Behaviour depends on distribution |

On Kali Linux, `adduser` is generally convenient for manually creating ordinary user accounts.

---

## 5. Setting a User Password

After creating an account with `useradd`, assign its password using:

```bash
sudo passwd analyst
```

The system asks you to enter and confirm a new password.

Linux normally does not display the password characters while you type.

This is expected behaviour.

### Password Security

A good password should:

- Be sufficiently long.
- Be difficult to guess.
- Avoid personal information.
- Not be reused across unrelated accounts.

Never write passwords directly into shared scripts or public notes.

---

## 6. Understanding User Home Directories

A home directory is the personal working directory associated with a user account.

For example:

```text
/home/analyst
```

It is normally used to store:

- Personal documents.
- Downloads.
- Configuration files.
- Scripts.
- Project folders.

The `-m` option creates this directory when using `useradd`.

To inspect a user's home directory:

```bash
ls -ld /home/analyst
```

To inspect its ownership:

```bash
stat /home/analyst
```

---

## 7. Creating a User with a Specific UID

Linux assigns numerical UIDs to accounts.

You can specify a UID during account creation using `-u`.

Example:

```bash
sudo useradd -m -u 1500 analyst
```

This requests UID 1500 for the new account.

Before choosing a specific UID, verify that it is not already in use:

```bash
getent passwd 1500
```

If the command returns an existing account, choose another available UID.

**Security note:** Do not assign UID 0 to ordinary users. UID 0 represents the root identity and gives extensive administrative privileges.

---

## 8. Creating a User with a Specific Primary Group

The `-g` option allows you to specify a primary group.

Example:

```bash
sudo useradd -m -g developers developer
```

The `developers` group must already exist.

To check whether it exists:

```bash
getent group developers
```

You can also specify a group ID when the group already exists:

```bash
sudo useradd -m -u 1501 -g 1500 analyst
```

Use actual available IDs from your system rather than assuming these example values are free.

---

## 9. Verifying a Newly Created Account

After creating an account, verify that Linux recognises it.

### Method 1: Using getent

```bash
getent passwd analyst
```

### Method 2: Using id

```bash
id analyst
```

### Method 3: Checking the home directory

```bash
ls -ld /home/analyst
```

### Method 4: Checking account information

```bash
sudo passwd -S analyst
```

This displays the account's password status without revealing the password.

---

## 10. Practical Laboratory

### Lab Objective

Create and inspect a temporary Linux user account safely.

We will create an account named:

`labanalyst`

This is a practice account, not your main Kali account.

### Step 1: Check whether the username already exists

```bash
getent passwd labanalyst
```

If an account already exists, do not create a duplicate. Inspect the existing account or choose another practice username.

### Step 2: Create the user

```bash
sudo adduser labanalyst
```

Follow the prompts and choose a practice password.

Do not use your personal or important account password.

### Step 3: Verify the account

```bash
id labanalyst
```

### Step 4: Inspect the account record

```bash
getent passwd labanalyst
```

### Step 5: Check the home directory

```bash
ls -ld /home/labanalyst
```

### Step 6: Check the password status

```bash
sudo passwd -S labanalyst
```

### Step 7: Inspect the user's identity

```bash
sudo -iu labanalyst whoami
```

Expected output:

```text
labanalyst
```

The `sudo -iu` command starts a login-style shell as the specified user and runs the command.

### Step 8: Inspect the user's home directory

```bash
sudo -iu labanalyst pwd
```

Expected output:

```text
/home/labanalyst
```

### Step 9: Return to your normal account

```bash
exit
```

You should now be back in your original terminal session.

---

## 11. Understanding Common Errors

### Error 1: User Already Exists

Example:

```text
useradd: user 'analyst' already exists
```

Solution:

Check the account:

```bash
getent passwd analyst
```

Choose a different username if necessary.

### Error 2: Permission Denied

Example:

```text
Permission denied
```

Possible cause:

You attempted to create an account without sufficient administrative privileges.

Solution:

```bash
sudo useradd -m analyst
```

### Error 3: Group Does Not Exist

Example:

```text
useradd: group 'developers' does not exist
```

Solution:

Check the group:

```bash
getent group developers
```

Create the group before assigning it as the primary group.

We will study group creation in Lesson 08.

---

## 12. Cybersecurity Applications

User creation is important in system security.

### A. Account Provisioning

Administrators create accounts for employees, developers, students, and other authorised users.

### B. Least Privilege

New accounts should receive only the permissions required for their responsibilities.

### C. Account Auditing

Security teams review accounts to identify unnecessary or unauthorised users.

### D. Service Isolation

Services can run under dedicated accounts instead of using root privileges unnecessarily.

### E. Laboratory Management

Different accounts can be used to simulate different users during authorised security testing.

---

## 13. Knowledge Check

Answer these questions.

1. What is the purpose of `useradd`?
2. What is the purpose of `adduser`?
3. What is one major difference between `useradd` and `adduser`?
4. What does the `-m` option do?
5. What does the `-s` option specify?
6. Which command assigns a password to an account?
7. What is a Linux home directory?
8. What does the `-u` option specify?
9. Why should ordinary users never be assigned UID 0?
10. Why should a newly created account be verified?

### Practical Questions

11. Write a command to create a user named `researcher` with a home directory.
12. Write a command to create a user with Bash as the login shell.
13. Write a command to set the user's password.
14. Write a command to display the user's UID and groups.
15. Write a command to inspect the user's account record.

---

## 14. Lesson Summary

In this lesson, we learned:

- Linux supports the creation of multiple user accounts.
- `useradd` creates user accounts.
- `adduser` provides an interactive account creation process on Kali Linux.
- `-m` creates a home directory.
- `-s` specifies the login shell.
- `-u` specifies a UID.
- `-g` specifies a primary group.
- `passwd` assigns or changes account passwords.
- `getent` and `id` help verify user accounts.

---

**End of Lesson 04**

**Next Lesson:** Managing User Passwords.