# Module 07 – Users and Permissions

# Lesson 03: Linux User and Group Configuration Files

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Identify important Linux user and group configuration files.
2. Understand the purpose of `/etc/passwd`.
3. Understand the purpose of `/etc/shadow`.
4. Understand the purpose of `/etc/group`.
5. Understand the purpose of `/etc/gshadow`.
6. Inspect account configuration safely.
7. Explain why these files are important for cybersecurity.

---

## 2. Introduction

Linux stores user and group information in several important configuration files.

These files help the operating system manage:

- User identities.
- Password authentication.
- Group memberships.
- Account security.
- Administrative access.

Most of these files are located inside:

```bash
/etc
```

The `/etc` directory contains system-wide configuration files.

For example:

```text
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow
```

These four files are especially important when studying Linux account management.

---

## 3. The /etc/passwd File

The `/etc/passwd` file contains basic information about Linux user accounts.

It normally contains one entry per account.

View it using:

```bash
cat /etc/passwd
```

A typical entry looks like this:

```text
kali:x:1000:1000:Kali User:/home/kali:/bin/bash
```

### Understanding the Seven Fields

| Field | Example | Meaning |
|---|---|---|
| 1 | kali | Username |
| 2 | x | Password information placeholder |
| 3 | 1000 | User ID |
| 4 | 1000 | Primary Group ID |
| 5 | Kali User | User description |
| 6 | /home/kali | Home directory |
| 7 | /bin/bash | Login shell |

### Important Information

The `x` in the second field means password authentication information is stored separately, normally in `/etc/shadow`.

The `/etc/passwd` file is generally readable by ordinary users because many system programs need account information.

It should not contain users' actual password hashes.

---

## 4. The /etc/shadow File

The `/etc/shadow` file stores password authentication information and password-aging details.

View it with administrative privileges:

```bash
sudo cat /etc/shadow
```

A typical entry has this structure:

```text
username:password_hash:last_change:min:max:warn:inactive:expire:reserved
```

### Understanding Its Fields

| Field | Meaning |
|---|---|
| 1 | Username |
| 2 | Password hash or password-status marker |
| 3 | Date of last password change |
| 4 | Minimum password age |
| 5 | Maximum password age |
| 6 | Warning period before expiration |
| 7 | Inactivity period after password expiration |
| 8 | Account expiration date |
| 9 | Reserved field |

### Why Is /etc/shadow Important?

It protects password authentication information from ordinary users.

Its access is restricted because exposing password hashes can allow attackers to attempt offline password cracking.

**Security rule:** Never publish or share the contents of your `/etc/shadow` file.

For this lesson, inspect its structure without copying or sharing password hashes.

---

## 5. The /etc/group File

The `/etc/group` file stores Linux group information.

View it using:

```bash
cat /etc/group
```

Example:

```text
cybersecurity:x:1500:abbas,ahmad,fatima
```

### Understanding the Four Fields

| Field | Example | Meaning |
|---|---|---|
| 1 | cybersecurity | Group name |
| 2 | x | Group password placeholder |
| 3 | 1500 | Group ID |
| 4 | abbas,ahmad,fatima | Supplementary group members |

### Why Is It Important?

Linux uses groups to organise users and simplify access management.

For example, an administrator can allow all members of a cybersecurity group to access a shared laboratory directory.

Remember that a user's primary group may not appear in the fourth field.

---

## 6. The /etc/gshadow File

The `/etc/gshadow` file stores protected group authentication and administration information.

It contains information such as:

- Group names.
- Group password hashes or markers.
- Group administrators.
- Group members.

Inspect it with:

```bash
sudo cat /etc/gshadow
```

Its structure is generally:

```text
group_name:password:administrators:members
```

### Why Is It Important?

It provides additional protection for group-related authentication and administration information.

Like `/etc/shadow`, it should not be exposed unnecessarily.

---

## 7. Comparing the Four Important Files

| File | Main Purpose | Sensitive? |
|---|---|---|
| `/etc/passwd` | Basic user account information | Generally not secret |
| `/etc/shadow` | Password hashes and aging information | Highly sensitive |
| `/etc/group` | Group information | Generally not secret |
| `/etc/gshadow` | Protected group information | Sensitive |

### Easy Way to Remember

- `passwd` = User account details.
- `shadow` = Protected password information.
- `group` = Group details.
- `gshadow` = Protected group information.

---

## 8. Practical Commands for Account Inspection

### Command 1: View user accounts

```bash
cat /etc/passwd
```

### Command 2: View group records

```bash
cat /etc/group
```

### Command 3: Find your own account

```bash
grep "^$(whoami):" /etc/passwd
```

### Command 4: Find a particular group

```bash
getent group sudo
```

This displays information about the `sudo` group if it exists in your system's configured account database.

### Command 5: Inspect your password-aging information

```bash
sudo chage -l "$(whoami)"
```

This displays password-aging information for your account.

It does not reveal your password.

### Command 6: Check permissions of the important files

```bash
ls -l /etc/passwd /etc/shadow /etc/group /etc/gshadow
```

Observe the ownership and permission differences.

Do not change these permissions during this exercise.

---

## 9. Understanding NSS and getent

Linux may obtain account information from sources other than local files.

For example, an organisation may use a central directory service.

The Name Service Switch (NSS) determines how the system looks up account and group information.

The `getent` command retrieves entries through these configured lookup services.

Examples:

```bash
getent passwd
```

```bash
getent group
```

```bash
getent passwd "$(whoami)"
```

This is useful because it checks the system's configured account database rather than only reading one local file.

---

## 10. Cybersecurity Applications

These files are important during Linux security auditing.

### A. User Account Auditing

Security administrators examine `/etc/passwd` to identify accounts that should not exist.

### B. Password Security

The `/etc/shadow` file contains password authentication information that must be protected.

### C. Group Privilege Investigation

Administrators inspect group memberships to identify accounts with unnecessary privileges.

### D. Permission Verification

The file permissions of sensitive account databases help protect system security.

### E. Incident Response

During an investigation, analysts may examine account records to understand how the system was configured.

**Important:** Finding an unexpected account or group is a reason to investigate, not automatic proof of compromise. Always establish whether the entry is legitimate.

---

## 11. Practical Laboratory

### Lab Objective

Inspect the important Linux account configuration files without modifying them.

### Step 1: Inspect /etc/passwd

```bash
cat /etc/passwd
```

Identify:

- Username.
- UID.
- GID.
- Home directory.
- Login shell.

### Step 2: Inspect your own account

```bash
grep "^$(whoami):" /etc/passwd
```

### Step 3: Inspect /etc/group

```bash
cat /etc/group
```

Identify three group names and their GIDs.

### Step 4: Inspect the sudo group

```bash
getent group sudo
```

Observe the returned group information.

### Step 5: Inspect file permissions

```bash
ls -l /etc/passwd /etc/shadow /etc/group /etc/gshadow
```

Compare their permissions.

### Step 6: Inspect password-aging information

```bash
sudo chage -l "$(whoami)"
```

### Step 7: Inspect the protected files

If you have administrative privileges, you can inspect the file structure with:

```bash
sudo awk -F: '{print $1, "fields:", NF}' /etc/shadow
```

This displays usernames and the number of fields without printing password hashes.

Repeat for the group security file:

```bash
sudo awk -F: '{print $1, "fields:", NF}' /etc/gshadow
```

Do not copy or share the protected file contents.

---

## 12. Knowledge Check

Answer the following questions.

1. What is the purpose of the `/etc` directory?
2. What information is stored in `/etc/passwd`?
3. How many fields are normally found in an `/etc/passwd` entry?
4. What does the `x` represent in the second field of `/etc/passwd`?
5. What is the purpose of `/etc/shadow`?
6. Why should `/etc/shadow` be protected?
7. What information is stored in `/etc/group`?
8. What is the purpose of `/etc/gshadow`?
9. What is the difference between `/etc/passwd` and `/etc/shadow`?
10. What does the `getent` command do?

### Practical Questions

11. Write a command to display user account records.
12. Write a command to display group records.
13. Write a command to find your account in `/etc/passwd`.
14. Write a command to inspect the sudo group.
15. Write a command to display permissions for the four account configuration files.

---

## 13. Lesson Summary

In this lesson, we learned:

- Linux stores system configuration information in `/etc`.
- `/etc/passwd` contains basic user account information.
- `/etc/shadow` contains protected password authentication information.
- `/etc/group` contains group information.
- `/etc/gshadow` contains protected group information.
- `getent` retrieves account information through configured lookup services.
- Sensitive account files must be protected from unauthorised access.

---

**End of Lesson 03**

**Next Lesson:** Creating Users with useradd and adduser.