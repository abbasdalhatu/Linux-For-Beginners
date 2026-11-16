# Module 07 – Users and Permissions

# Lesson 10: Understanding File Ownership

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain file ownership in Linux.
2. Identify file owners and groups.
3. Understand the relationship between UID and GID.
4. Inspect ownership using `ls -l` and `stat`.
5. Change file ownership using `chown`.
6. Change group ownership using `chgrp`.
7. Apply ownership management to cybersecurity.

---

## 2. What Is File Ownership?

Every Linux file and directory has an owner and an associated group.

Linux uses ownership to determine who can access, modify, or execute files.

For example, imagine a shared university server containing:

```text
/home/abbas/assignment.txt
```

The file may belong to:

- Owner: abbas
- Group: students

This means Linux can apply different permissions to the owner, members of the group, and other users.

### Why Is File Ownership Important?

Ownership helps administrators:

- Protect confidential files.
- Control access to directories.
- Manage shared resources.
- Prevent unauthorised modifications.
- Enforce security policies.

---

## 3. Understanding User Ownership

The owner of a file is the user account associated with that file.

When a user creates a file, Linux normally assigns that user as its owner.

Example:

```bash
touch report.txt
```

Inspect the file:

```bash
ls -l report.txt
```

Possible output:

```text
-rw-r--r-- 1 abbas abbas 0 Nov 14 10:30 report.txt
```

The first `abbas` represents the owner.

The second `abbas` represents the group.

---

## 4. Understanding Group Ownership

Every file also has an associated group.

Group ownership allows several users to receive access based on shared membership.

Example:

```text
-rw-r----- 1 abbas securitylab 2048 Nov 14 10:30 report.txt
```

Here:

- Owner: abbas
- Group: securitylab
- File size: 2048 bytes

If the permissions allow group reading, authorised members of `securitylab` can read the file.

---

## 5. Understanding UID and GID

Linux internally identifies users and groups using numeric identifiers.

### UID

UID means User ID.

It identifies a user account.

Check a user's UID:

```bash
id abbas
```

### GID

GID means Group ID.

It identifies a group.

Check group information:

```bash
getent group securitylab
```

### Why Numeric IDs Matter

Linux stores ownership using numeric IDs rather than depending only on usernames and group names.

This explains why files can still exist after an account has been deleted.

The operating system may display a numeric UID when it cannot resolve the original username.

---

## 6. Inspecting Ownership with ls -l

The `ls -l` command displays detailed file information.

Example:

```bash
ls -l
```

Possible output:

```text
-rw-r--r-- 1 abbas students 1024 Nov 14 11:00 notes.txt
drwxr-xr-x 2 abbas students 4096 Nov 14 11:05 documents
```

Important fields:

| Field | Meaning |
|---|---|
| `-rw-r--r--` | Permissions |
| `1` or `2` | Link count |
| `abbas` | Owner |
| `students` | Group |
| `1024` or `4096` | Size |
| Date and time | Modification time |
| Filename | File or directory name |

The first character indicates the file type:

- `-` = regular file
- `d` = directory
- `l` = symbolic link

---

## 7. Inspecting Ownership with stat

The `stat` command displays detailed file metadata.

Example:

```bash
stat report.txt
```

It can show:

- File type.
- File size.
- Inode number.
- Permissions.
- UID.
- GID.
- Access time.
- Modification time.
- Change time.

To display only selected ownership information:

```bash
stat -c '%U %G %u %g' report.txt
```

The output represents:

1. Owner username.
2. Group name.
3. Numeric UID.
4. Numeric GID.

---

## 8. Changing File Ownership with chown

The `chown` command changes file ownership.

### Basic Syntax

```bash
sudo chown username filename
```

Example:

```bash
sudo chown ahmad report.txt
```

This changes the owner of `report.txt` to `ahmad`.

The existing group ownership is normally preserved.

### Change Both Owner and Group

```bash
sudo chown ahmad:securitylab report.txt
```

This changes:

- Owner to `ahmad`.
- Group to `securitylab`.

### Change Only the Group Using chown

```bash
sudo chown :securitylab report.txt
```

This changes the group while leaving the owner unchanged.

---

## 9. Changing Group Ownership with chgrp

The `chgrp` command changes the group associated with a file or directory.

### Syntax

```bash
sudo chgrp groupname filename
```

Example:

```bash
sudo chgrp securitylab report.txt
```

Verify:

```bash
ls -l report.txt
```

The group column should display `securitylab`.

---

## 10. Recursive Ownership Changes

Sometimes administrators need to change ownership for an entire directory and its contents.

The `-R` option performs recursive changes.

Example:

```bash
sudo chown -R abbas:securitylab /home/abbas/project
```

This changes ownership throughout the specified directory tree.

**Security warning:** Recursive ownership changes can affect many files. Always inspect the exact path first.

Avoid running broad commands such as:

```bash
sudo chown -R abbas:securitylab /
```

Changing ownership of the entire system can break Linux and create serious security problems.

---

## 11. Understanding Ownership and Permissions Together

Ownership determines which permission category applies to a user.

Linux generally evaluates access using:

1. Owner permissions.
2. Group permissions.
3. Other-user permissions.

Example:

```text
-rw-r----- 1 abbas securitylab 2048 report.txt
```

Interpretation:

| Category | Permissions |
|---|---|
| Owner | Read and write |
| Group | Read |
| Others | No permissions |

An authorised group member can read the file but cannot modify it.

A user who is neither the owner nor a member of the relevant group falls under the others category, unless another access-control mechanism applies.

---

## 12. Practical Laboratory

### Lab Objective

Create practice files, inspect their ownership, and safely change their owners and groups.

We will work inside:

```text
~/module07-lab
```

### Step 1: Create a Practice Directory

```bash
mkdir -p ~/module07-lab/ownership
```

### Step 2: Enter the Directory

```bash
cd ~/module07-lab/ownership
```

### Step 3: Create a Practice File

```bash
touch report.txt
```

### Step 4: Inspect Ownership

```bash
ls -l report.txt
```

### Step 5: Inspect Detailed Metadata

```bash
stat report.txt
```

### Step 6: Check Your User and Group IDs

```bash
id
```

### Step 7: Change the File's Group

If the `securitylab` group exists:

```bash
sudo chgrp securitylab report.txt
```

If it does not exist, create it first:

```bash
sudo groupadd securitylab
```

### Step 8: Verify the New Group

```bash
ls -l report.txt
```

### Step 9: Inspect Numeric Ownership

```bash
ls -ln report.txt
```

This displays numeric UID and GID values.

### Step 10: Create Another Practice File

```bash
touch confidential.txt
```

### Step 11: Inspect Its Ownership

```bash
stat confidential.txt
```

### Step 12: Change Ownership to Your Current User

Replace `YOUR_USERNAME` with the username returned by `whoami`:

```bash
sudo chown YOUR_USERNAME:securitylab confidential.txt
```

### Step 13: Verify the Change

```bash
ls -l confidential.txt
```

### Step 14: Inspect the Entire Practice Directory

```bash
ls -l ~/module07-lab/ownership
```

You should now understand how ownership and group membership are represented.

---

## 13. Cybersecurity Applications

### A. Protecting Confidential Information

Administrators can assign ownership to authorised accounts and restrict access to sensitive documents.

### B. Shared Development Environments

Developers can share files through a common group.

### C. Incident Response

Unexpected file ownership can indicate misconfiguration or suspicious activity.

### D. Linux Server Hardening

System administrators should review ownership of sensitive files and directories.

### E. Privilege Escalation Prevention

Incorrect ownership of executable files or configuration files can create security vulnerabilities.

Useful inspection commands:

```bash
ls -l
ls -ln
stat filename
```

---

## 14. Knowledge Check

Answer these questions:

1. What is file ownership in Linux?
2. Why does every file have an owner?
3. What is the difference between UID and GID?
4. Which command displays detailed ownership information?
5. Which command changes file ownership?
6. Which command changes group ownership?
7. What does `chown username:groupname file` do?
8. What does `chown -R` mean?
9. Why is recursive ownership modification dangerous?
10. What is the purpose of `stat`?
11. What does `ls -ln` display?
12. What happens to file ownership when a user account is deleted?
13. How does group ownership support collaboration?
14. Why is correct ownership important for system security?
15. Explain the relationship between ownership and file permissions.

---

## 15. Lesson Summary

In this lesson, we learned:

- Every Linux file has an owner and an associated group.
- UID identifies users.
- GID identifies groups.
- `ls -l` displays ownership information.
- `stat` displays detailed metadata.
- `chown` changes file ownership.
- `chgrp` changes group ownership.
- `chown -R` performs recursive ownership changes.
- Correct ownership helps protect files and directories.

**Key Security Principle:** Every file and directory should have appropriate ownership so that only authorised users and groups can access or modify it.

---

**End of Lesson 10**

**Next Lesson:** Understanding Linux File Permissions.