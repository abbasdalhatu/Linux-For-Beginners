---
---

# Module 07 – Users and Permissions

# Lesson 02: Understanding User Accounts, UID and GID

## 1. Learning Objectives

By the end of this lesson, you should understand:

1. What a Linux user account contains.
2. What UID means.
3. What GID means.
4. The difference between usernames and UIDs.
5. How Linux identifies users internally.
6. How to inspect user and group information.

---

## 2. What Is a Linux User Account?

A Linux user account is an identity registered in the operating system.

It contains information that helps Linux identify and manage a user.

A typical user account includes:

- Username.
- User ID (UID).
- Primary Group ID (GID).
- Home directory.
- Login shell.
- Password authentication information.

Example:

Imagine a Linux account:

```text
Username: abbas
UID: 1000
GID: 1000
Home: /home/abbas
Shell: /bin/bash
```

Linux uses this information to identify the account and control its access to system resources.

---

## 3. Understanding UID (User ID)

UID means **User Identifier**.

It is a numerical value assigned to every Linux user account.

Linux primarily uses UIDs internally to identify account ownership.

For example:

```text
abbas  →  1000
ahmad  →  1001
fatima →  1002
```

Even though humans prefer usernames, Linux uses numerical identifiers when managing ownership and permissions.

### Important UID Categories

Common conventions on many Linux distributions include:

| UID | Meaning |
|---|---|
| 0 | Root user |
| 1–999 | Commonly reserved for system or service accounts |
| 1000 and above | Commonly used for regular users |

**Important:** These ranges are conventions, not universal rules. Different distributions and configurations may use different UID ranges.

### Why Is UID Important?

UIDs help Linux:

- Identify file owners.
- Associate processes with users.
- Enforce access restrictions.
- Manage account privileges.

For example, if a file belongs to UID 1000, Linux checks whether the current user has that UID when evaluating ownership-based permissions.

---

## 4. Understanding GID (Group ID)

GID means **Group Identifier**.

It is a numerical value assigned to a Linux group.

Groups allow multiple users to share access to files and directories.

Example:

```text
Group: cybersecurity
GID: 1500
```

Suppose three users belong to this group:

| Username | UID | Primary Group |
|---|---:|---|
| Abbas | 1000 | cybersecurity |
| Ahmad | 1001 | cybersecurity |
| Fatima | 1002 | cybersecurity |

The operating system can use the group's GID when checking group-based permissions.

### Why Is GID Important?

GIDs help Linux:

- Manage shared file access.
- Organise users into groups.
- Control permissions.
- Support collaborative environments.

---

## 5. Difference Between UID and GID

| UID | GID |
|---|---|
| User Identifier | Group Identifier |
| Identifies a user account | Identifies a group |
| Associated with user ownership | Associated with group ownership |
| Example: 1000 | Example: 1500 |

Remember:

- UID identifies a user.
- GID identifies a group.

---

## 6. How Linux Uses UID and GID

Consider a file:

```text
-rw-r----- 1 abbas cybersecurity 2048 Nov 14 report.txt
```

This information indicates:

- Owner: `abbas`
- Group: `cybersecurity`
- File size: 2048 bytes
- Permissions: `rw-r-----`

Linux evaluates permissions using the file's ownership information and the identity of the user attempting to access it.

For this example:

- The owner can read and write.
- Members matching the file's group can read.
- Other users have no permissions.

We will study file permissions in detail later.

---

## 7. Practical Commands

Open your Kali Linux terminal.

### Command 1: Display your UID and GID

```bash
id
```

Example:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

### Command 2: Display only your UID

```bash
id -u
```

Example:

```text
1000
```

### Command 3: Display only your username

```bash
id -un
```

### Command 4: Display your primary GID

```bash
id -g
```

### Command 5: Display your primary group name

```bash
id -gn
```

### Command 6: Display all group IDs

```bash
id -G
```

### Command 7: Display all group names

```bash
id -Gn
```

---

## 8. Understanding the /etc/passwd File

Linux stores basic user account information in:

```text
/etc/passwd
```

You can inspect it with:

```bash
cat /etc/passwd
```

A typical entry looks like this:

```text
kali:x:1000:1000:Kali User:/home/kali:/bin/bash
```

Each entry contains seven fields separated by colons.

| Field | Example | Meaning |
|---|---|---|
| 1 | kali | Username |
| 2 | x | Password information stored separately |
| 3 | 1000 | UID |
| 4 | 1000 | Primary GID |
| 5 | Kali User | User description |
| 6 | /home/kali | Home directory |
| 7 | /bin/bash | Login shell |

**Security note:** The `x` does not represent the actual password. Password hashes are normally stored in `/etc/shadow`, which has restricted access.

---

## 9. Understanding the /etc/group File

Linux stores group information in:

```text
/etc/group
```

Inspect it using:

```bash
cat /etc/group
```

Example entry:

```text
cybersecurity:x:1500:abbas,ahmad,fatima
```

Its four fields are:

| Field | Example | Meaning |
|---|---|---|
| 1 | cybersecurity | Group name |
| 2 | x | Group password placeholder |
| 3 | 1500 | GID |
| 4 | abbas,ahmad,fatima | Supplementary group members |

**Important:** A user's primary group is recorded in `/etc/passwd`. The fourth field of `/etc/group` lists supplementary members and may not list every user whose primary group is that group.

---

## 10. Practical Laboratory

### Lab Objective

Investigate your own account's UID, GID, and group information.

### Step 1: Check your username

```bash
whoami
```

### Step 2: Check your UID

```bash
id -u
```

### Step 3: Check your primary GID

```bash
id -g
```

### Step 4: Check your primary group

```bash
id -gn
```

### Step 5: Check all group memberships

```bash
id -Gn
```

### Step 6: Inspect your account record

```bash
grep "^$(whoami):" /etc/passwd
```

This displays the account entry for your current user.

### Step 7: Inspect group information

```bash
cat /etc/group
```

Observe how groups are represented.

### Step 8: Display account information in a readable format

```bash
getent passwd "$(whoami)"
```

This retrieves the account entry using the system's configured account database sources.

---

## 11. Cybersecurity Applications

Understanding UID and GID is important during security investigations.

### A. File Ownership Investigation

An analyst can inspect a file's numeric ownership using:

```bash
ls -ln
```

The `-n` option displays numeric UID and GID values.

### B. Process Investigation

You can inspect process ownership using:

```bash
ps -eo pid,user,uid,group,gid,comm
```

This helps identify which accounts own running processes.

### C. Account Auditing

Security administrators can examine account information to identify unexpected users or suspicious account configurations.

### D. Privilege Analysis

Checking UIDs helps identify the root account and understand which processes operate with elevated privileges.

---

## 12. Knowledge Check

Answer these questions.

1. What does UID stand for?
2. What does GID stand for?
3. Which UID is assigned to the root account?
4. What is the common UID range for regular users on many Linux distributions?
5. Why does Linux use numerical identifiers?
6. What is the difference between UID and GID?
7. Which file stores basic user account information?
8. Which file stores group information?
9. What does the sixth field in `/etc/passwd` represent?
10. What command displays your numerical UID and GID?

### Practical Questions

11. Write a command to display only your UID.
12. Write a command to display your primary GID.
13. Write a command to display all your group names.
14. Write a command to inspect your account entry in `/etc/passwd`.
15. Write a command to display numerical ownership information for files.

---

## 13. Lesson Summary

In this lesson, we learned:

- A Linux user account contains identity and login information.
- UID identifies a user numerically.
- GID identifies a group numerically.
- UID 0 belongs to the root account.
- Regular user IDs commonly begin at 1000 on many distributions.
- `/etc/passwd` stores basic account information.
- `/etc/group` stores group information.
- `id` helps inspect user and group identifiers.
- Numeric ownership is useful in security investigations.

---

**End of Lesson 02**

**Next Lesson:** Linux User and Group Configuration Files.