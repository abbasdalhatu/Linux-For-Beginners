---
---


# Module 13 – Lesson 04: File Permissions and Security

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux file permissions.
2. Understand file ownership.
3. Identify read, write, and execute permissions.
4. Understand symbolic and numeric permission notation.
5. Use `chmod` to modify permissions.
6. Understand `chown` and `chgrp`.
7. Explain special Linux permissions.
8. Understand Access Control Lists (ACLs).
9. Identify insecure file permission configurations.
10. Perform a basic file permission security audit.

---

## 2. Introduction to Linux File Permissions

Linux is a multi-user operating system.

This means several users can access the same computer while having different levels of access.

Not every user should be allowed to read, modify, or execute every file.

Linux uses file permissions to control access to files and directories.

### Simple Example

Imagine a university department with three employees:

- Lecturer.
- Accountant.
- IT administrator.

The lecturer should be able to access lecture materials but should not automatically modify financial records.

The accountant should access financial records but should not automatically modify system configuration files.

Linux file permissions help enforce these restrictions.

---

## 3. Understanding Linux File Ownership

Every Linux file and directory has an owner and an associated group.

For example:

```bash
ls -l
```

Possible output:

```text
-rw-r--r-- 1 kali kali 1200 Nov 15 10:00 notes.txt
```

Let's examine the output:

```text
-rw-r--r-- 1 kali kali 1200 Nov 15 10:00 notes.txt
```

| Field | Meaning |
|---|---|
| `-` | Regular file |
| `rw-r--r--` | Permissions |
| `1` | Link count |
| `kali` | Owner |
| `kali` | Group |
| `1200` | File size |
| `Nov 15 10:00` | Modification date |
| `notes.txt` | Filename |

The two important ownership fields are:

- Owner: `kali`
- Group: `kali`

---

## 4. Three Main Linux Permission Types

Linux has three basic permission types.

### 4.1 Read Permission (r)

Read permission allows a user to read a file.

For directories, it allows a user to list directory entries, subject to the required directory search permission.

Symbol:

```text
r
```

Numeric value:

```text
4
```

### 4.2 Write Permission (w)

Write permission allows a user to modify a file.

For directories, write permission allows directory entries to be created, deleted, or renamed, subject to the required search permission.

Symbol:

```text
w
```

Numeric value:

```text
2
```

### 4.3 Execute Permission (x)

Execute permission allows a user to execute a file when it is an executable program or script.

For directories, execute permission means search or traversal permission.

Symbol:

```text
x
```

Numeric value:

```text
1
```

### Permission Summary

| Permission | Symbol | Numeric Value |
|---|---|---:|
| Read | r | 4 |
| Write | w | 2 |
| Execute | x | 1 |

---

## 5. The Three Permission Categories

Linux permissions apply to three main categories.

### 5.1 User (u)

The user category refers to the file owner.

### 5.2 Group (g)

The group category refers to members of the file's associated group.

### 5.3 Others (o)

Others refers to users who are neither the file owner nor members of its group.

The categories are commonly represented as:

```text
u = User
g = Group
o = Others
a = All
```

---

## 6. Understanding Permission Notation

Consider this permission string:

```text
-rwxr-xr--
```

Break it into sections:

```text
- | rwx | r-x | r--
    USER  GROUP OTHERS
```

Explanation:

| Category | Permissions | Meaning |
|---|---|---|
| User | `rwx` | Read, write, execute |
| Group | `r-x` | Read and execute |
| Others | `r--` | Read only |

The first character describes the file type.

Common file types include:

| Symbol | Meaning |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |

---

## 7. Numeric Permission System

Linux allows permissions to be represented using numbers.

Remember:

- Read = 4
- Write = 2
- Execute = 1

Add the values to calculate the required permission.

### Example 1: Read Only

```text
r-- = 4
```

### Example 2: Read and Write

```text
rw- = 4 + 2 = 6
```

### Example 3: Read and Execute

```text
r-x = 4 + 1 = 5
```

### Example 4: Full Permissions

```text
rwx = 4 + 2 + 1 = 7
```

### Common Permission Values

| Number | Permission |
|---:|---|
| 0 | No permissions |
| 1 | Execute |
| 2 | Write |
| 3 | Write and execute |
| 4 | Read |
| 5 | Read and execute |
| 6 | Read and write |
| 7 | Read, write and execute |

### Example: 755

```text
755
```

Break it down:

- Owner = 7 = rwx
- Group = 5 = r-x
- Others = 5 = r-x

Therefore:

```text
rwxr-xr-x
```

### Example: 644

```text
644
```

Break it down:

- Owner = 6 = rw-
- Group = 4 = r--
- Others = 4 = r--

Therefore:

```text
rw-r--r--
```

### Example: 600

```text
600
```

Break it down:

- Owner = 6 = rw-
- Group = 0 = ---
- Others = 0 = ---

Therefore:

```text
rw-------
```

This is commonly suitable for files containing private information, provided ownership is correct.

---

## 8. Managing Permissions with chmod

The `chmod` command changes file and directory permissions.

Its name means Change Mode.

General syntax:

```bash
chmod permissions filename
```

### Example 1: Set Permissions to 755

```bash
chmod 755 script.sh
```

This gives:

- Owner: read, write, execute.
- Group: read, execute.
- Others: read, execute.

### Example 2: Set Permissions to 644

```bash
chmod 644 notes.txt
```

This gives:

- Owner: read and write.
- Group: read.
- Others: read.

### Example 3: Set Permissions to 600

```bash
chmod 600 private.txt
```

This restricts file access to its owner.

### Example 4: Remove Execute Permission from Others

```bash
chmod o-x script.sh
```

### Example 5: Add Execute Permission to the Owner

```bash
chmod u+x script.sh
```

### Example 6: Remove Write Permission from the Group

```bash
chmod g-w notes.txt
```

### Important Security Warning

Avoid blindly using commands such as:

```bash
chmod -R 777 /
```

This is extremely dangerous and can destroy system security.

Never grant unrestricted permissions without a clear requirement.

---

## 9. Managing Ownership with chown

The `chown` command changes file ownership.

General syntax:

```bash
chown username filename
```

Example:

```bash
sudo chown labuser notes.txt
```

This changes the file owner to `labuser`.

To change both owner and group:

```bash
sudo chown labuser:students notes.txt
```

Inspect ownership:

```bash
ls -l notes.txt
```

### Recursive Ownership Changes

The `-R` option applies changes recursively.

Example:

```bash
sudo chown -R labuser:students project/
```

This changes ownership throughout the project directory.

Use recursive ownership changes only when you have carefully confirmed the target directory and intended ownership.

---

## 10. Managing Groups with chgrp

The `chgrp` command changes a file's group ownership.

General syntax:

```bash
chgrp groupname filename
```

Example:

```bash
sudo chgrp students lecture-notes.txt
```

Inspect the result:

```bash
ls -l lecture-notes.txt
```

---

## 11. Directory Permissions

Directory permissions behave differently from file permissions.

### Read (r)

Allows users to list directory entries.

### Write (w)

Allows users to create, delete, or rename entries in the directory, subject to search permission.

### Execute (x)

Allows users to traverse or search the directory.

### Example

A directory with permission:

```text
rwxr-x---
```

Means:

- Owner: full access.
- Group: read and traverse.
- Others: no access.

This can be useful for a shared departmental directory.

Remember that deleting a file is generally controlled by permissions on its containing directory, not just by the file's own write permission.

---

## 12. Special Linux Permissions

Linux provides three important special permission mechanisms.

### 12.1 SUID – Set User ID

SUID is represented numerically by 4000.

When applied to an executable file, it allows the program to run with the effective user identity of the file owner.

Example:

```text
-rwsr-xr-x
```

The `s` in the owner's execute position indicates SUID.

SUID programs require careful security review because they can run with elevated privileges.

### 12.2 SGID – Set Group ID

SGID is represented numerically by 2000.

For executable files, it affects the effective group identity.

For directories, newly created files and subdirectories generally inherit the directory's group ownership.

Example directory permission:

```text
drwxrws---
```

The `s` in the group execute position indicates SGID.

### 12.3 Sticky Bit

The sticky bit is represented numerically by 1000.

It is commonly used on shared directories.

For example, `/tmp` often uses sticky-bit permissions.

Inspect:

```bash
ls -ld /tmp
```

Example:

```text
drwxrwxrwt
```

The `t` indicates the sticky bit.

In a sticky directory, users generally cannot delete or rename other users' entries unless they own the entry or have the relevant administrative privileges.

### Special Permission Summary

| Permission | Numeric Value | Purpose |
|---|---:|---|
| SUID | 4000 | Effective user identity |
| SGID | 2000 | Effective group identity or directory group inheritance |
| Sticky bit | 1000 | Restricts deletion and renaming in shared directories |

---

## 13. Understanding Access Control Lists (ACLs)

Traditional Linux permissions provide three main access categories:

- Owner.
- Group.
- Others.

Sometimes these categories are not flexible enough.

Access Control Lists provide more detailed permissions for specific users and groups.

### Example

Imagine a project directory owned by `student1`.

You want:

- The owner to have full access.
- The group to have read access.
- `student2` to have read and write access.
- Other users to have no access.

ACLs can provide this additional flexibility.

### Inspect ACLs

```bash
getfacl filename
```

### Modify ACLs

General example:

```bash
setfacl -m u:student2:rw filename
```

This grants `student2` read and write access.

### Remove an ACL Entry

```bash
setfacl -x u:student2 filename
```

### Important

ACLs can affect the effective permissions beyond what a basic `ls -l` display suggests.

When reviewing permissions, use `getfacl` when ACLs may be present.

---

# 14. Practical Lab: File Permission Security

## Lab Objective

Create test files and directories, apply different permissions, and inspect their security configurations.

All operations will take place inside your own laboratory directory.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson04
cd ~/module13-lab/lesson04
```

### Step 2: Create Test Files

```bash
echo "Public information" > public.txt
echo "Private information" > private.txt
echo "Administrative information" > admin.txt
```

### Step 3: Inspect Initial Permissions

```bash
ls -l
```

Observe the default permissions assigned by your system's umask.

### Step 4: Apply Different Permissions

Public file:

```bash
chmod 644 public.txt
```

Private file:

```bash
chmod 600 private.txt
```

Administrative test file:

```bash
chmod 640 admin.txt
```

### Step 5: Verify Permissions

```bash
ls -l
```

Your results should include:

```text
-rw-r--r-- public.txt
-rw------- private.txt
-rw-r----- admin.txt
```

The owner and group names will depend on your account.

### Step 6: Create an Executable Script

```bash
nano test-script.sh
```

Add:

```bash
#!/bin/bash

echo "Linux file permission test"
echo "The script executed successfully."
```

Save and exit.

Give the owner execute permission:

```bash
chmod u+x test-script.sh
```

Run it:

```bash
./test-script.sh
```

### Step 7: Create a Protected Directory

```bash
mkdir secure-directory
chmod 700 secure-directory
```

Inspect it:

```bash
ls -ld secure-directory
```

Expected permissions:

```text
drwx------
```

### Step 8: Inspect Ownership

```bash
ls -l
```

Inspect a specific file:

```bash
stat private.txt
```

This displays detailed information about the file, including ownership and permissions.

### Step 9: Inspect ACL Availability

Check whether the commands are available:

```bash
command -v getfacl
command -v setfacl
```

If `getfacl` is installed, inspect your private file:

```bash
getfacl private.txt
```

Do not install additional software unless necessary for your environment.

### Step 10: Create a Security Report

Run:

```bash
{
    echo "LINUX FILE PERMISSION SECURITY REPORT"
    echo "====================================="
    echo
    echo "CURRENT USER"
    whoami
    echo
    echo "FILE PERMISSIONS"
    ls -l
    echo
    echo "DIRECTORY PERMISSIONS"
    ls -ld secure-directory
    echo
    echo "DETAILED PRIVATE FILE INFORMATION"
    stat private.txt
    echo
    echo "ACL INFORMATION"
    if command -v getfacl >/dev/null 2>&1; then
        getfacl private.txt
    else
        echo "getfacl is not installed."
    fi
} > file-permission-security-report.txt 2>&1
```

Inspect your report:

```bash
cat file-permission-security-report.txt
```

### Expected Files

```text
lesson04/
├── public.txt
├── private.txt
├── admin.txt
├── test-script.sh
├── secure-directory/
└── file-permission-security-report.txt
```

---

## 15. Real-World Cybersecurity Application

Imagine you are auditing a Linux server belonging to a university.

You discover that a file containing sensitive student information has these permissions:

```text
-rw-r--r-- student1 students student-records.txt
```

This means other local users may be able to read the file.

If the information is confidential, this configuration may be inappropriate.

A possible correction, after confirming ownership and access requirements, could be:

```bash
chmod 600 student-records.txt
```

This gives read and write access only to the owner.

However, changing permissions without checking the application's requirements can break legitimate access.

A professional administrator should:

1. Identify the file owner.
2. Identify legitimate users.
3. Determine the required access.
4. Inspect existing ACLs.
5. Apply the least-privilege configuration.
6. Verify the result.

---

## 16. Knowledge Check

Answer these questions in your notebook.

1. What are Linux file permissions?
2. What is file ownership?
3. Explain read, write, and execute permissions.
4. What do user, group, and others mean?
5. What does permission 755 mean?
6. What does permission 644 mean?
7. What does permission 600 mean?
8. What is the purpose of `chmod`?
9. What is the difference between `chown` and `chgrp`?
10. Why are directory permissions different from file permissions?
11. What is SUID?
12. What is SGID?
13. What is the sticky bit?
14. What are Access Control Lists?
15. Why should administrators avoid unnecessary permission 777 configurations?

---

## 17. Practical Completion Checklist

- [ ] I understand Linux file ownership.
- [ ] I understand read, write, and execute permissions.
- [ ] I understand user, group, and others.
- [ ] I can interpret symbolic permissions.
- [ ] I can calculate numeric permissions.
- [ ] I understand `chmod`.
- [ ] I understand `chown` and `chgrp`.
- [ ] I understand directory permissions.
- [ ] I understand SUID, SGID, and the sticky bit.
- [ ] I understand Access Control Lists.
- [ ] I completed the file permission security lab.
- [ ] I generated my security report.

**Lesson 04 Complete: File Permissions and Security.**
