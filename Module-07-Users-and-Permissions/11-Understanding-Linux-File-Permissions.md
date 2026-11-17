---
---

# Module 07 – Users and Permissions

# Lesson 11: Understanding Linux File Permissions

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux file permissions.
2. Identify the three permission categories.
3. Understand read, write, and execute permissions.
4. Interpret permission strings such as `rwxr-xr--`.
5. Understand file and directory permission differences.
6. Inspect permissions using `ls -l`.
7. Explain how Linux determines access.
8. Apply permission concepts to cybersecurity.

---

## 2. Introduction to Linux File Permissions

Linux is a multi-user operating system.

Multiple users can work on the same computer or server.

However, not every user should have access to every file.

For example:

- A student should not modify another student's private files.
- A system administrator should protect important configuration files.
- A web server should only access the directories it needs.
- Confidential documents should not be readable by unauthorised users.

Linux uses file permissions to control access to files and directories.

**File permissions determine what users are allowed to do with a particular file or directory.**

---

## 3. The Three Permission Categories

Linux divides users into three permission categories.

### A. Owner (User)

The owner is the user who owns the file.

The owner permissions determine what that user can do.

### B. Group

The group represents users who belong to the file's associated group.

Group permissions determine what members of that group can do.

### C. Others

Others represents users who are neither the file owner nor members of the relevant group.

Their access is controlled by the others permissions.

### Example

Consider this file:

```text
-rwxr-xr--
```

The permission categories are:

| Category | Permissions |
|---|---|
| Owner | `rwx` |
| Group | `r-x` |
| Others | `r--` |

The owner can read, write, and execute the file.

Group members can read and execute it.

Other users can only read it.

---

## 4. Understanding the Three Basic Permissions

Linux has three basic permission types.

### A. Read (r)

Read permission allows a user to view file contents.

For a regular file:

- Open and read the file.
- Display its contents.
- Copy its contents, provided the necessary access to the file and path exists.

Example:

```bash
cat report.txt
```

### B. Write (w)

Write permission allows a user to modify a file's contents.

For a regular file:

- Add information.
- Remove information.
- Change existing content.

Example:

```bash
nano report.txt
```

Write permission on a file does not automatically grant permission to delete or rename it. Those operations generally depend on permissions on its parent directory.

### C. Execute (x)

Execute permission allows a user to execute a file as a program or script, provided other execution requirements are satisfied.

Example:

```bash
./script.sh
```

A script usually needs execute permission to be run directly this way.

---

## 5. Understanding Directory Permissions

Permissions behave differently on directories.

### Read (r) on a Directory

Allows a user to list directory entries.

Example:

```bash
ls directoryname
```

### Write (w) on a Directory

Allows a user to create, delete, and rename entries inside the directory, subject to other restrictions.

### Execute (x) on a Directory

Allows a user to traverse the directory and access entries when the required permissions are available.

For example:

```bash
cd directoryname
```

Without execute permission, a user generally cannot traverse that directory.

### Important Difference

| Permission | Regular File | Directory |
|---|---|---|
| Read | Read contents | List entries |
| Write | Modify contents | Create, delete and rename entries |
| Execute | Run as a program | Traverse directory |

**Remember:** Directory permissions are especially important because they control access to files stored inside directories.

---

## 6. Understanding the Linux Permission Structure

Run:

```bash
ls -l
```

Example output:

```text
-rwxr-xr-- 1 abbas securitylab 1024 Nov 14 12:00 script.sh
```

Let's break down the permission section:

```text
-rwxr-xr--
```

The first character identifies the file type.

The remaining nine characters represent permissions.

```text
- | rwx | r-x | r--
    Owner Group Others
```

Explanation:

- `-` = Regular file.
- `rwx` = Owner permissions.
- `r-x` = Group permissions.
- `r--` = Others permissions.

Other common file type indicators include:

- `d` = Directory.
- `l` = Symbolic link.

---

## 7. Understanding Permission Combinations

Each category can have different combinations.

| Permission | Meaning |
|---|---|
| `---` | No permissions |
| `r--` | Read only |
| `-w-` | Write only |
| `--x` | Execute only |
| `rw-` | Read and write |
| `r-x` | Read and execute |
| `-wx` | Write and execute |
| `rwx` | Read, write and execute |

Example:

```text
-rw-r-----
```

Interpretation:

- Owner: Read and write.
- Group: Read only.
- Others: No access.

This is a common permission pattern for confidential files.

---

## 8. Understanding Permission Inheritance

Linux does not automatically copy a parent directory's permissions to every new file.

Instead, the permissions requested when creating a file or directory are affected by the process's umask.

For example:

- A newly created file commonly starts with maximum permissions of `666`.
- A newly created directory commonly starts with maximum permissions of `777`.

The umask removes permissions from those starting values.

We will study this in detail in Lesson 14.

---

## 9. Checking File Permissions

### Method 1: ls -l

```bash
ls -l filename
```

Displays the permission string alongside ownership information.

### Method 2: stat

```bash
stat filename
```

Displays detailed file metadata, including permissions.

### Method 3: Numeric Permission Information

```bash
stat -c '%a %n' filename
```

Example output:

```text
644 report.txt
```

This displays the numeric permission mode.

We will study numeric permission calculations in Lesson 13.

---

## 10. Understanding Why Permissions Matter in Cybersecurity

Incorrect permissions can expose a Linux system to security threats.

### A. Unauthorised File Access

If confidential files are readable by everyone, sensitive information may be exposed.

### B. Unauthorised Modification

If users can modify important configuration files, they may change system behaviour.

### C. Script Security

An executable script with inappropriate ownership or permissions may be abused.

### D. Directory Exposure

Incorrect directory permissions can allow users to access information they should not see.

### E. Privilege Escalation

Misconfigured permissions on sensitive files or executable programs may create opportunities for privilege escalation.

Administrators should regularly inspect important files and directories.

---

## 11. Practical Laboratory

### Lab Objective

Create files and directories, inspect their permissions, and understand how Linux represents access rights.

We will use:

```text
~/module07-lab/permissions
```

### Step 1: Create the Practice Directory

```bash
mkdir -p ~/module07-lab/permissions
```

### Step 2: Enter the Directory

```bash
cd ~/module07-lab/permissions
```

### Step 3: Create a Regular File

```bash
touch report.txt
```

### Step 4: Create a Directory

```bash
mkdir documents
```

### Step 5: Inspect Their Permissions

```bash
ls -l
```

Observe the differences between the regular file and directory.

### Step 6: Inspect File Metadata

```bash
stat report.txt
```

### Step 7: Inspect Directory Metadata

```bash
stat documents
```

### Step 8: Display Numeric Permissions

```bash
stat -c '%a %n' report.txt documents
```

### Step 9: Create a Script

```bash
echo '#!/bin/bash' > test.sh
echo 'echo "Linux permissions laboratory"' >> test.sh
```

### Step 10: Inspect Script Permissions

```bash
ls -l test.sh
```

Notice that the script may not yet have execute permission.

### Step 11: Inspect All Practice Files

```bash
ls -la ~/module07-lab/permissions
```

Do not change any permissions yet. We will practise that in the next two lessons.

---

## 12. Knowledge Check

Answer the following questions:

1. What are Linux file permissions?
2. Why does Linux use file permissions?
3. What are the three permission categories?
4. What does read permission mean?
5. What does write permission mean?
6. What does execute permission mean?
7. What is the difference between file read permission and directory read permission?
8. What does directory execute permission allow?
9. What does `rwxr-xr--` mean?
10. What does `---` represent?
11. Which command displays detailed file permissions?
12. What does the first character in a permission string represent?
13. Why are incorrect file permissions dangerous?
14. How can excessive write permissions affect system security?
15. What is the relationship between file ownership and file permissions?

---

## 13. Lesson Summary

In this lesson, we learned:

- Linux permissions control access to files and directories.
- There are three permission categories: owner, group, and others.
- Read (`r`) allows reading file contents or listing directory entries.
- Write (`w`) allows modifying files or changing directory entries.
- Execute (`x`) allows executing files or traversing directories.
- `ls -l` displays file permissions.
- `stat` provides detailed permission information.
- Incorrect permissions can create serious security vulnerabilities.

**Key Security Principle:** Grant only the permissions necessary for each user to perform their responsibilities.

---

**End of Lesson 11**

**Next Lesson:** Changing Permissions with chmod – Symbolic Mode.