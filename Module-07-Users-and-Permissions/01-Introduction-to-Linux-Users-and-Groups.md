---
---

# Module 07 – Users and Permissions

# Lesson 01: Introduction to Linux Users and Groups

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a Linux user is.
2. Understand why Linux uses user accounts.
3. Explain the purpose of groups.
4. Understand the relationship between users, groups, and permissions.
5. Identify different categories of Linux users.
6. Use basic commands to identify the current user.

---

## 2. What Is a Linux User?

A Linux user is an account that allows a person or service to interact with the Linux operating system.

Every user has an identity that Linux uses to determine:

- Which files the user can access.
- Which directories the user can enter.
- Which programs the user can execute.
- Which system operations the user can perform.
- Whether the user has administrative privileges.

Think of a Linux computer as a university.

Different students have different accounts. A student can access their own academic records but cannot automatically access another student's private records.

Linux uses user accounts to enforce similar restrictions.

### Example

Imagine a Linux computer used by three people:

| User | Role |
|---|---|
| Abbas | Administrator |
| Ahmad | Standard user |
| Fatima | Standard user |

Each person has an individual account.

Linux can restrict Ahmad from accessing Abbas's private files while allowing Abbas to manage the system.

---

## 3. Why Does Linux Use User Accounts?

Linux is a multi-user operating system.

This means multiple users can have accounts on the same computer and use its resources.

User accounts provide several important benefits.

### A. Security

Users cannot automatically access everything on the computer.

### B. Privacy

Users can keep their personal files separate from other users.

### C. Accountability

Linux can associate processes and file ownership with specific user accounts.

### D. Resource Management

Administrators can control which users are allowed to use particular resources.

### E. Administrative Control

Only appropriately privileged users can perform certain administrative operations.

---

## 4. Types of Linux Users

Linux accounts can be broadly understood in three categories.

### 4.1 Root User

The root user is the superuser account in Linux.

It has UID 0 and possesses extensive administrative privileges.

The root account can perform operations such as:

- Installing and removing software.
- Creating and deleting user accounts.
- Changing system configurations.
- Managing system services.
- Changing file ownership and permissions.

The root account must be used carefully because mistakes can affect the entire operating system.

### 4.2 Standard Users

Standard users are ordinary accounts created for people who use the system.

They generally have restricted privileges.

Examples:

- Student accounts.
- Developer accounts.
- Researcher accounts.
- Regular desktop users.

### 4.3 System Users

System users are accounts created primarily to run services and background processes.

Examples include accounts used by web servers, databases, and other system services.

These accounts often have restricted login capabilities.

**Important:** Not every system account is intended for an interactive human login.

---

## 5. What Is a Linux Group?

A group is a collection of user accounts.

Linux uses groups to simplify permission management.

Instead of assigning access to several users individually, an administrator can create a group and grant permissions to that group.

### Example

Imagine a cybersecurity department with four students:

- Abbas
- Ahmad
- Fatima
- Musa

An administrator creates a group called:

`cybersecurity`

All four students are added to the group.

The administrator can then grant the group access to a shared project directory.

This is easier than managing access for each student separately.

---

## 6. Understanding the Relationship Between Users and Groups

Linux associates users with groups.

Two important concepts are:

### Primary Group

Every normal Linux user has a primary group.

Files created by that user are normally associated with the user's primary group.

### Supplementary Groups

A user can also belong to additional groups.

These provide extra access to shared resources.

### Example

Suppose a user named Abbas belongs to:

| Group | Purpose |
|---|---|
| abbas | Primary group |
| cybersecurity | Security laboratory access |
| developers | Software development resources |

The user has one primary group and can have multiple supplementary groups.

---

## 7. Identifying Your Current Linux User

Let's perform some practical exercises.

Open your Kali Linux terminal.

### Command 1: whoami

```bash
whoami
```

Purpose:

Displays the username of the account currently using the terminal.

Example output:

```text
kali
```

Your output may be different.

### Command 2: id

```bash
id
```

Purpose:

Displays your user ID, primary group ID, and group memberships.

Example output:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

Understanding the output:

- `uid=1000`: Your user ID.
- `kali`: Your username.
- `gid=1000`: Your primary group ID.
- `groups`: Groups associated with your account.

### Command 3: groups

```bash
groups
```

Purpose:

Displays the groups your current account belongs to.

### Command 4: id -un

```bash
id -un
```

Purpose:

Displays your username.

It is another way to identify the current user.

---

## 8. Practical Laboratory

### Lab Objective

Identify your Linux user account and examine its group memberships.

### Step 1: Identify your username

```bash
whoami
```

Write down the output.

### Step 2: Examine your user identity

```bash
id
```

Identify your:

- Username.
- UID.
- Primary group.
- GID.
- Supplementary groups.

### Step 3: List your groups

```bash
groups
```

Compare the output with the groups displayed by `id`.

### Step 4: Inspect the current user

```bash
id -un
```

Confirm that the username matches the output from `whoami`.

### Step 5: Create a practice directory

```bash
mkdir -p ~/module07-lab
```

Check that it exists:

```bash
ls -ld ~/module07-lab
```

This directory will be used for future practical exercises.

---

## 9. Cybersecurity Applications

Understanding Linux users and groups is essential in cybersecurity.

### A. Privilege Management

Security professionals examine which accounts have administrative privileges.

### B. Access Control

Administrators use groups and permissions to restrict access to sensitive information.

### C. Incident Investigation

Investigators examine user accounts to understand which account owns a file or process.

### D. Privilege Escalation Analysis

During authorised security assessments, professionals investigate whether a user can obtain privileges beyond those intended.

### E. System Hardening

Administrators restrict unnecessary accounts and reduce excessive privileges.

**Security principle: Least Privilege**

Every user and service should receive only the permissions necessary to perform its intended duties.

---

## 10. Knowledge Check

Answer these questions in your notebook.

1. What is a Linux user?
2. Why is Linux considered a multi-user operating system?
3. What is the root user?
4. What is a standard user?
5. What is a system user?
6. What is a Linux group?
7. Explain the difference between primary and supplementary groups.
8. What does the `whoami` command do?
9. What information does the `id` command display?
10. Explain the principle of least privilege.

---

## 11. Lesson Summary

In this lesson, we learned that:

- Linux supports multiple user accounts.
- Every account has a unique user identity.
- The root user has UID 0 and extensive administrative privileges.
- Standard users have more restricted privileges.
- System users support operating system services.
- Groups simplify access management.
- Users have primary and supplementary group memberships.
- Commands such as `whoami`, `id`, and `groups` help identify user information.
- The principle of least privilege is essential for Linux security.

---

**End of Lesson 01**

**Next Lesson:** Understanding User Accounts, UID and GID.