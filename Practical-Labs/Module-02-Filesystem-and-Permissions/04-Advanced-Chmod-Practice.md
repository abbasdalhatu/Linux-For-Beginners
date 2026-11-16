
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 04: ADVANCED CHMOD PRACTICE

### 1. Introduction

In the previous lab, you learned the fundamentals of Linux file permissions.

Now we will go deeper into `chmod`, the command used to modify permissions.

Advanced permission management helps Linux administrators:

- Protect sensitive files.
- Control access to directories.
- Prevent unauthorized modifications.
- Configure shared workspaces.
- Reduce security risks caused by incorrect permissions.

This practical focuses on applying permissions accurately and verifying the results.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Use symbolic `chmod` operations confidently.
2. Apply numeric permissions.
3. Set exact permissions with `=`.
4. Modify permissions recursively.
5. Understand the difference between recursive file and directory permissions.
6. Use `umask` to control default permissions.
7. Audit permissions within a directory.
8. Correct unsafe permissions in a controlled lab.

### 3. Create Your Workspace

Run:

```bash
mkdir -p ~/practical-labs/module02/lab04
cd ~/practical-labs/module02/lab04
```

Create a practice structure:

```bash
mkdir documents scripts shared backup
touch documents/report.txt
touch documents/notes.txt
touch scripts/backup.sh
touch shared/team-file.txt
touch backup/archive.txt
```

Add sample content:

```bash
echo "Confidential practice report" > documents/report.txt
echo "General practice notes" > documents/notes.txt
echo '#!/bin/bash' > scripts/backup.sh
echo 'echo "Backup practice"' >> scripts/backup.sh
echo "Shared practice information" > shared/team-file.txt
```

Inspect the structure:

```bash
find . -maxdepth 2 -print
```

### 4. Advanced Symbolic Permission Operations

The general format is:

```bash
chmod [who][operator][permission] file
```

Permission categories:

- `u` = owner
- `g` = group
- `o` = others
- `a` = all

Operators:

- `+` = add
- `-` = remove
- `=` = assign exact permissions

#### Exercise A: Add permissions

Give the owner execute permission:

```bash
chmod u+x scripts/backup.sh
```

Give the group read permission:

```bash
chmod g+r documents/report.txt
```

Give others read permission:

```bash
chmod o+r documents/notes.txt
```

Inspect:

```bash
ls -l documents scripts
```

#### Exercise B: Remove permissions

Remove write permission from others:

```bash
chmod o-w documents/notes.txt
```

Remove execute permission from the group:

```bash
chmod g-x scripts/backup.sh
```

Inspect:

```bash
ls -l documents scripts
```

#### Exercise C: Set exact permissions

Use `=` when you want to replace the existing permissions for a category.

Example:

```bash
chmod u=rw,g=r,o= documents/report.txt
```

Expected result:

```text
-rw-r-----
```

Another example:

```bash
chmod u=rwx,g=rx,o= scripts/backup.sh
```

Expected result:

```text
-rwxr-x---
```

### 5. Combining Symbolic Operations

You can modify multiple categories in one command.

Example:

```bash
chmod u+rwx,g+rx,o-rwx scripts/backup.sh
```

This:

- Adds read, write, and execute for the owner.
- Adds read and execute for the group.
- Removes all permissions from others.

Another example:

```bash
chmod a-r documents/notes.txt
```

This removes read permission from all three categories.

Restore a useful permission:

```bash
chmod 644 documents/notes.txt
```

### 6. Numeric Permission Practice

Remember:

| Permission | Value |
|---|---:|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

Common combinations:

| Value | Permission |
|---|---|
| 0 | `---` |
| 1 | `--x` |
| 2 | `-w-` |
| 3 | `-wx` |
| 4 | `r--` |
| 5 | `r-x` |
| 6 | `rw-` |
| 7 | `rwx` |

Apply the following:

```bash
chmod 640 documents/report.txt
chmod 644 documents/notes.txt
chmod 750 scripts
chmod 700 backup
chmod 770 shared
```

Verify:

```bash
ls -ld documents scripts shared backup
ls -l documents
```

Understand the results:

- `640`: owner reads and writes; group reads; others have no access.
- `644`: owner reads and writes; group and others read.
- `750`: owner has full access; group reads and traverses; others have no access.
- `700`: owner has full access; nobody else has access.
- `770`: owner and group have full access; others have no access.

### 7. Recursive Permission Changes

The `-R` option applies permission changes recursively.

For example:

```bash
chmod -R 750 shared
```

This applies `750` to every object under `shared`, including regular files.

That is often not appropriate because regular files generally do not need execute permission.

A better method is to treat files and directories separately.

Set directory permissions:

```bash
find shared -type d -exec chmod 750 {} \;
```

Set regular file permissions:

```bash
find shared -type f -exec chmod 640 {} \;
```

Verify:

```bash
find shared -printf '%M %p\n'
```

Explanation:

- `find shared`: searches the shared directory.
- `-type d`: selects directories.
- `-type f`: selects regular files.
- `-exec`: runs a command on each result.
- `chmod`: changes permissions.
- `{}`: represents the current result.
- `\;`: ends the command executed for each result.

### 8. Understanding Umask

`umask` controls which permissions are removed from the default permissions of newly created files and directories.

Inspect your current setting:

```bash
umask
```

Display it symbolically:

```bash
umask -S
```

For example, a common umask is:

```text
0022
```

Typical base permissions are:

- Regular files: `666`
- Directories: `777`

The umask removes permissions from these defaults.

For umask `022`:

Regular files:

```text
666 - masked permissions = 644
```

Directories:

```text
777 - masked permissions = 755
```

This is a bitmask operation rather than ordinary decimal subtraction.

### 9. Practical Umask Exercise

Create a temporary test directory:

```bash
mkdir umask-test
cd umask-test
```

Check the current umask:

```bash
umask
```

Set a temporary umask:

```bash
umask 027
```

Create a file and directory:

```bash
touch restricted-file.txt
mkdir restricted-directory
```

Inspect:

```bash
ls -ld restricted-directory
ls -l restricted-file.txt
```

Expected permissions:

| Object | Expected Permissions |
|---|---|
| Regular file | `640` |
| Directory | `750` |

Return to the lab directory:

```bash
cd ..
```

Restore your original umask if you changed it for the exercise. You can inspect the original value from your earlier output.

Note: The `umask` command changes the setting for the current shell and its child processes. It does not permanently change system-wide settings.

### 10. Permission Auditing

A Linux administrator should regularly inspect permissions on important files and directories.

List files with their permission strings:

```bash
find . -printf '%M %u %g %p\n'
```

Find regular files that are executable by others:

```bash
find . -type f -perm -0001 -print
```

Find files with write permission for others:

```bash
find . -type f -perm -0002 -print
```

Find directories writable by others:

```bash
find . -type d -perm -0002 -print
```

These commands identify permission conditions that may deserve review. They do not automatically prove a security problem.

### 11. Practical Security Challenge

You have been asked to secure a small project directory.

The requirements are:

| Object | Required Permissions |
|---|---|
| `documents/` | Owner full, group read/traverse, others none |
| `report.txt` | Owner read/write, group read, others none |
| `notes.txt` | Owner read/write, group read, others read |
| `scripts/` | Owner full, group read/traverse, others none |
| `backup.sh` | Owner full, group read/execute, others none |
| `backup/` | Owner only |
| `shared/` | Owner and group full, others none |

Apply the required permissions:

```bash
chmod 750 documents
chmod 640 documents/report.txt
chmod 644 documents/notes.txt
chmod 750 scripts
chmod 750 scripts/backup.sh
chmod 700 backup
chmod 770 shared
```

Verify:

```bash
ls -ld documents scripts backup shared
ls -l documents scripts
```

Important: These permissions describe the intended owner/group/other access. Actual access also depends on ownership, directory traversal permissions, ACLs, and other security controls.

### 12. Generate an Advanced Permission Report

Run:

```bash
cd ~/practical-labs/module02/lab04

{
    echo "ADVANCED CHMOD AUDIT REPORT"
    echo "==========================="
    echo "Date: $(date)"
    echo "User: $(whoami)"
    echo "Umask: $(umask)"
    echo
    echo "DIRECTORY STRUCTURE:"
    find . -maxdepth 3 -print
    echo
    echo "PERMISSIONS AND OWNERSHIP:"
    find . -printf '%M %u %g %p\n'
    echo
    echo "FILES WRITABLE BY OTHERS:"
    find . -type f -perm -0002 -print
    echo
    echo "DIRECTORIES WRITABLE BY OTHERS:"
    find . -type d -perm -0002 -print
} > advanced-permission-report.txt
```

Read the report:

```bash
cat advanced-permission-report.txt
```

### 13. Knowledge Check

1. What is the difference between `+`, `-`, and `=` in symbolic chmod?
2. What does `chmod 640 file.txt` mean?
3. What does `chmod -R` do?
4. Why should you avoid blindly applying `chmod -R 777`?
5. Why is it better to set directory and file permissions separately?
6. What is umask?
7. What permissions are commonly created for a file with umask `022`?
8. What permissions are commonly created for a directory with umask `027`?
9. Which command finds files writable by others?
10. Why should administrators perform permission audits?

### 14. Lab Completion Checklist

- [ ] Created the Lab 04 workspace.
- [ ] Practised symbolic permission changes.
- [ ] Used exact permission assignment.
- [ ] Applied numeric permissions.
- [ ] Practised recursive permission management.
- [ ] Changed permissions on files and directories separately.
- [ ] Inspected and tested umask.
- [ ] Audited files writable by others.
- [ ] Completed the security challenge.
- [ ] Generated `advanced-permission-report.txt`.

### 15. Expected Workspace

```text
~/practical-labs/module02/lab04/
├── documents/
│   ├── report.txt
│   └── notes.txt
├── scripts/
│   └── backup.sh
├── shared/
│   └── team-file.txt
├── backup/
│   └── archive.txt
├── umask-test/
│   ├── restricted-file.txt
│   └── restricted-directory/
└── advanced-permission-report.txt
```

**Lab 04 Status:** Complete when the practical exercises and checklist are finished.

**Next Lab:** Lab 05 – Ownership and Groups.
