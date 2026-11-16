# Module 07 – Users and Permissions

# Lesson 12: Changing Permissions with chmod – Symbolic Mode

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of `chmod`.
2. Understand symbolic permission notation.
3. Identify permission categories in symbolic mode.
4. Add permissions using `chmod`.
5. Remove permissions using `chmod`.
6. Set permissions using symbolic mode.
7. Apply permissions to files and directories.
8. Understand the security implications of permission changes.

---

## 2. Introduction to chmod

In Linux, permissions determine what users can do with files and directories.

Sometimes, the existing permissions are too restrictive or too permissive.

For example:

- A script cannot execute because it lacks execute permission.
- A confidential file is readable by unauthorised users.
- A directory needs to allow a particular group to access its contents.

Linux provides the `chmod` command to change file and directory permissions.

`chmod` means **change mode**.

Basic syntax:

```bash
chmod [options] permissions filename
```

Example:

```bash
chmod u+x script.sh
```

This adds execute permission for the file owner.

---

## 3. Understanding Symbolic Mode

Symbolic mode uses letters and symbols to modify permissions.

It is easier to understand than numeric mode when you want to make specific permission changes.

Symbolic mode contains three main parts:

1. Who receives the permission change.
2. What operation should be performed.
3. Which permission should be changed.

Example:

```bash
chmod u+x script.sh
```

Breakdown:

| Symbol | Meaning |
|---|---|
| `u` | Owner (user) |
| `+` | Add permission |
| `x` | Execute permission |

The command adds execute permission for the owner.

---

## 4. Permission Categories in Symbolic Mode

Linux uses four main symbolic categories.

| Symbol | Meaning |
|---|---|
| `u` | Owner (user) |
| `g` | Group |
| `o` | Others |
| `a` | All categories |

### Examples

```bash
chmod u+x file.txt
```

Add execute permission to the owner.

```bash
chmod g+r file.txt
```

Add read permission to the group.

```bash
chmod o-r file.txt
```

Remove read permission from others.

```bash
chmod a+r file.txt
```

Add read permission to everyone.

---

## 5. Understanding the Three Permission Operators

Symbolic mode uses three operators.

### A. Plus (+)

Adds a permission without intentionally removing other permissions.

Example:

```bash
chmod u+w report.txt
```

Adds write permission to the owner.

### B. Minus (-)

Removes a specified permission.

Example:

```bash
chmod o-r report.txt
```

Removes read permission from others.

### C. Equals (=)

Sets the specified permissions exactly for the selected category.

Example:

```bash
chmod u=rw report.txt
```

Sets the owner's permissions to read and write.

Any owner permissions not included in the expression are removed.

---

## 6. Understanding Multiple Permission Changes

You can combine several symbolic changes in one command.

Example:

```bash
chmod u+r,g+r,o-r file.txt
```

This performs three operations:

- Adds read permission to the owner.
- Adds read permission to the group.
- Removes read permission from others.

You can also use commas to separate different permission assignments.

Example:

```bash
chmod u=rw,g=r,o= file.txt
```

The resulting permissions are:

| Category | Permission |
|---|---|
| Owner | Read and write |
| Group | Read only |
| Others | No permissions |

---

## 7. Practical Examples

### Example 1: Make a Script Executable for Its Owner

```bash
chmod u+x script.sh
```

### Example 2: Remove Write Permission from Others

```bash
chmod o-w report.txt
```

### Example 3: Allow the Group to Read and Write

```bash
chmod g+rw report.txt
```

### Example 4: Remove All Permissions from Others

```bash
chmod o-rwx confidential.txt
```

### Example 5: Give Everyone Read Permission

```bash
chmod a+r public.txt
```

### Example 6: Set Exact Permissions for All Categories

```bash
chmod u=rw,g=r,o= file.txt
```

---

## 8. Changing Directory Permissions

The same `chmod` command works on directories.

Example:

```bash
chmod u+x documents
```

Adds directory traversal permission for the owner.

Example:

```bash
chmod g+r documents
```

Allows group members to list directory entries, assuming they also have the necessary traversal permission.

Example:

```bash
chmod o-rwx documents
```

Removes read, write, and execute permissions from others.

Remember:

- Directory read permission allows listing entries.
- Directory write permission allows creating, deleting, and renaming entries.
- Directory execute permission allows traversal.

---

## 9. Understanding Recursive Permission Changes

The `-R` option applies a permission change recursively to a directory and its contents.

Example:

```bash
chmod -R g+r directoryname
```

This adds group read permission throughout the directory tree.

### Important Security Warning

Recursive permission changes can affect many files and directories.

A careless command can expose confidential data or break applications.

Never practise recursive permission changes on:

- `/`
- `/etc`
- `/usr`
- `/var`
- Your entire home directory

Use a disposable practice directory instead.

We will use a safe example in the laboratory.

---

## 10. Verifying Permission Changes

After changing permissions, inspect the result.

Use:

```bash
ls -l filename
```

For detailed information:

```bash
stat filename
```

Example:

```bash
chmod u+x script.sh
ls -l script.sh
```

If the owner previously had `rw-`, the permissions should now show `rwx` for the owner.

You can also inspect numeric permissions:

```bash
stat -c '%A %a %n' script.sh
```

This displays both symbolic and numeric representations.

---

## 11. Practical Laboratory

### Lab Objective

Practise adding, removing, and setting permissions using symbolic mode.

All exercises will be performed inside:

```text
~/module07-lab/permissions
```

### Step 1: Enter the Practice Directory

```bash
cd ~/module07-lab/permissions
```

### Step 2: Inspect Existing Files

```bash
ls -l
```

You should see files created in Lesson 11, including:

- `report.txt`
- `documents/`
- `test.sh`

### Step 3: Add Execute Permission to the Script Owner

```bash
chmod u+x test.sh
```

Verify:

```bash
ls -l test.sh
```

### Step 4: Execute the Script

```bash
./test.sh
```

Expected output:

```text
Linux permissions laboratory
```

### Step 5: Add Group Read Permission to report.txt

```bash
chmod g+r report.txt
```

Verify:

```bash
ls -l report.txt
```

### Step 6: Remove Others' Read Permission

```bash
chmod o-r report.txt
```

Verify:

```bash
ls -l report.txt
```

### Step 7: Set Owner Permissions Exactly

```bash
chmod u=rw report.txt
```

Verify:

```bash
ls -l report.txt
```

The owner should have read and write permissions, without execute permission.

### Step 8: Create a Confidential File

```bash
touch confidential.txt
```

### Step 9: Remove All Permissions from Others

```bash
chmod o-rwx confidential.txt
```

Verify:

```bash
ls -l confidential.txt
```

### Step 10: Practise Multiple Changes

```bash
chmod u=rw,g=r,o= confidential.txt
```

Verify:

```bash
ls -l confidential.txt
```

Expected permission pattern:

```text
-rw-r-----
```

### Step 11: Create a Directory for Recursive Practice

```bash
mkdir -p recursive-lab/subfolder
```

Create two files:

```bash
touch recursive-lab/file1.txt
touch recursive-lab/subfolder/file2.txt
```

### Step 12: Apply a Recursive Permission Change

```bash
chmod -R g+r recursive-lab
```

Inspect:

```bash
ls -lR recursive-lab
```

Notice that group read permission has been added to the directory tree.

This is a controlled demonstration. Do not use recursive changes on important system directories.

---

## 12. Common chmod Mistakes

### Mistake 1: Confusing u and g

Incorrect understanding:

- `u` means group.
- `g` means owner.

Correct meaning:

- `u` = owner.
- `g` = group.

### Mistake 2: Using = When You Mean +

```bash
chmod u=x file.txt
```

This sets the owner's permissions to execute only. It removes their previous read and write permissions.

If you only want to add execute permission, use:

```bash
chmod u+x file.txt
```

### Mistake 3: Forgetting Directory Traversal

A user may have permission to read a file but still be unable to reach it because a parent directory lacks execute permission.

### Mistake 4: Applying Recursive Changes Carelessly

Using `chmod -R` on important system directories can break system functionality or weaken security.

Always verify the target path before executing recursive commands.

---

## 13. Knowledge Check

Answer the following questions:

1. What does `chmod` mean?
2. What is symbolic permission mode?
3. What does `u` represent?
4. What does `g` represent?
5. What does `o` represent?
6. What does `a` represent?
7. What is the difference between `+`, `-`, and `=`?
8. What does `chmod u+x script.sh` do?
9. What does `chmod g+r file.txt` do?
10. What does `chmod o-rwx file.txt` do?
11. What is the purpose of the `-R` option?
12. Why can recursive permission changes be dangerous?
13. Which command can verify permission changes?
14. What is the difference between adding permissions and setting permissions?
15. Why should administrators follow the principle of least privilege?

---

## 14. Lesson Summary

In this lesson, we learned:

- `chmod` changes Linux file and directory permissions.
- Symbolic mode uses `u`, `g`, `o`, and `a`.
- `+` adds permissions.
- `-` removes permissions.
- `=` sets permissions exactly for the selected category.
- Multiple changes can be combined using commas.
- `-R` applies changes recursively.
- `ls -l` and `stat` help verify permission changes.

**Security Principle:** Give users only the permissions they need. Avoid unnecessary access, especially on confidential files and important directories.

---

**End of Lesson 12**

**Next Lesson:** Changing Permissions with chmod – Numeric Mode.