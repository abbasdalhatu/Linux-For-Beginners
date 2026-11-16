
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 03: FILE PERMISSIONS FUNDAMENTALS

### 1. Introduction

Linux is a multi-user operating system. Multiple users can access the same computer, server, or filesystem.

To protect files and directories, Linux uses a permission system.

File permissions determine:

- Who can read a file.
- Who can modify a file.
- Who can execute a file.
- Who can access a directory.

Understanding permissions is essential for Linux administration and cybersecurity.

A poorly configured permission can expose sensitive information or allow unauthorized users to modify important files.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Understand Linux read, write, and execute permissions.
2. Identify the three permission categories.
3. Interpret permission strings.
4. Use `ls -l` to inspect permissions.
5. Change permissions using `chmod`.
6. Understand numeric and symbolic permission notation.
7. Explain how file and directory permissions differ.

### 3. The Three Permission Categories

Linux permissions are assigned to three categories.

| Category | Symbol | Meaning |
|---|---|---|
| User/Owner | `u` | Owner of the file |
| Group | `g` | Members of the file's group |
| Others | `o` | Everyone else |

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
- | rwx | r-x | r--
  |  u  |  g  |  o
```

- `-` means regular file.
- `rwx` means the owner can read, write, and execute.
- `r-x` means the group can read and execute.
- `r--` means others can only read.

### 4. Understanding the Three Basic Permissions

#### A. Read Permission (`r`)

Read permission allows a user to view file contents.

For a regular file:

```bash
cat filename.txt
```

If a user does not have the required read permission, access may be denied.

#### B. Write Permission (`w`)

Write permission allows a user to modify a file.

Example:

```bash
echo "Updated information" >> filename.txt
```

Without write permission, a user generally cannot modify the file's contents.

#### C. Execute Permission (`x`)

Execute permission allows a file to be executed as a program or script, subject to other system restrictions.

Example:

```bash
./script.sh
```

A script normally needs execute permission for direct execution.

### 5. Understanding Directory Permissions

Permissions behave differently for directories.

| Permission | Meaning for a Directory |
|---|---|
| Read (`r`) | List directory entries |
| Write (`w`) | Create, remove, or rename entries, subject to other restrictions |
| Execute (`x`) | Access/search directory entries when path permissions permit |

Important:

A directory's write permission does not automatically grant permission to modify every file inside it.

Similarly, being able to read a directory does not necessarily mean you can read every file inside it.

### 6. Create Your Practical Workspace

Run:

```bash
mkdir -p ~/practical-labs/module02/lab03
cd ~/practical-labs/module02/lab03
```

Create sample files:

```bash
touch public.txt private.txt script.sh
mkdir shared-folder
```

Add sample content:

```bash
echo "This is a public practice file." > public.txt
echo "This is a private practice file." > private.txt
echo '#!/bin/bash' > script.sh
echo 'echo "Linux permissions practical"' >> script.sh
```

### 7. Inspect File Permissions

Run:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 kali kali   31 public.txt
-rw-r--r-- 1 kali kali   32 private.txt
-rw-r--r-- 1 kali kali   49 script.sh
drwxr-xr-x 2 kali kali 4096 shared-folder
```

Your actual ownership, sizes, and timestamps may differ.

To inspect one file:

```bash
ls -l public.txt
```

To inspect metadata:

```bash
stat public.txt
```

### 8. Understanding Symbolic Permission Notation

The `chmod` command can use letters to change permissions.

General syntax:

```bash
chmod [who][operator][permission] filename
```

Permission categories:

- `u` = owner
- `g` = group
- `o` = others
- `a` = all users

Operators:

- `+` = add permission
- `-` = remove permission
- `=` = set exact permissions

Examples:

Give the owner execute permission:

```bash
chmod u+x script.sh
```

Remove write permission from others:

```bash
chmod o-w public.txt
```

Give the group read permission:

```bash
chmod g+r private.txt
```

Remove all permissions from others:

```bash
chmod o-rwx private.txt
```

Give everyone read permission:

```bash
chmod a+r public.txt
```

Inspect the results:

```bash
ls -l
```

### 9. Practical Exercise: Change Permissions

Start with:

```bash
chmod 644 public.txt
chmod 600 private.txt
chmod 755 script.sh
chmod 750 shared-folder
```

Inspect:

```bash
ls -l
```

Expected permission patterns:

| Object | Expected Permissions |
|---|---|
| `public.txt` | `-rw-r--r--` |
| `private.txt` | `-rw-------` |
| `script.sh` | `-rwxr-xr-x` |
| `shared-folder` | `drwxr-x---` |

These are permissions for your practice objects only.

### 10. Understanding Numeric Permissions

Linux also supports numeric permission notation.

Each basic permission has a numeric value:

| Permission | Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |

Add the values together to calculate each permission group.

Examples:

**7 = 4 + 2 + 1**

This represents:

```text
rwx
```

**6 = 4 + 2**

This represents:

```text
rw-
```

**5 = 4 + 1**

This represents:

```text
r-x
```

**4 = 4**

This represents:

```text
r--
```

**0 = 0**

This represents:

```text
---
```

### 11. Understanding Common Permission Numbers

| Number | Owner | Group | Others |
|---|---|---|---|
| 777 | rwx | rwx | rwx |
| 755 | rwx | r-x | r-x |
| 750 | rwx | r-x | --- |
| 700 | rwx | --- | --- |
| 644 | rw- | r-- | r-- |
| 640 | rw- | r-- | --- |
| 600 | rw- | --- | --- |
| 444 | r-- | r-- | r-- |

Example:

```bash
chmod 644 public.txt
```

Calculation:

- Owner: 6 = read + write.
- Group: 4 = read.
- Others: 4 = read.

Result:

```text
rw-r--r--
```

Another example:

```bash
chmod 700 private.txt
```

Calculation:

- Owner: 7 = read + write + execute.
- Group: 0 = no permissions.
- Others: 0 = no permissions.

Result:

```text
rwx------
```

### 12. Practical Numeric Permission Challenges

Perform these operations:

**Challenge A: Owner-only access**

```bash
chmod 700 private.txt
ls -l private.txt
```

**Challenge B: Public read access**

```bash
chmod 644 public.txt
ls -l public.txt
```

**Challenge C: Executable script**

```bash
chmod 755 script.sh
./script.sh
```

Expected output:

```text
Linux permissions practical
```

**Challenge D: Restricted directory**

```bash
chmod 750 shared-folder
ls -ld shared-folder
```

### 13. Important Security Lesson: Why 777 Is Dangerous

You may encounter this command online:

```bash
chmod 777 filename
```

It grants read, write, and execute permissions to everyone.

This can create serious security risks.

For example:

- Unauthorized users may modify files.
- Executable files may be replaced.
- Shared directories may become exposed.
- Applications may run files modified by untrusted users.

Avoid using `777` as a routine troubleshooting solution.

Always grant only the permissions required.

### 14. File Permissions vs Directory Permissions

Consider these two objects:

```text
report.txt
documents/
```

For `report.txt`:

- Read: view its contents.
- Write: modify its contents.
- Execute: run it as a program, if appropriate.

For `documents/`:

- Read: list directory entries.
- Write: create, delete, or rename entries.
- Execute: traverse/search the directory.

This distinction is especially important when investigating access-denied errors.

### 15. Generate a Permission Audit Report

Run:

```bash
cd ~/practical-labs/module02/lab03

{
    echo "LINUX FILE PERMISSION REPORT"
    echo "============================"
    echo "Date: $(date)"
    echo "User: $(whoami)"
    echo
    echo "CURRENT DIRECTORY:"
    pwd
    echo
    echo "FILE PERMISSIONS:"
    ls -lah
    echo
    echo "PUBLIC FILE:"
    stat public.txt
    echo
    echo "PRIVATE FILE:"
    stat private.txt
    echo
    echo "SCRIPT:"
    stat script.sh
    echo
    echo "DIRECTORY:"
    stat shared-folder
} > permission-report.txt
```

Read the report:

```bash
cat permission-report.txt
```

### 16. Knowledge Check

1. What are the three Linux permission categories?
2. What does read permission allow?
3. What does write permission allow?
4. What does execute permission allow?
5. What is the meaning of `chmod`?
6. What does the number 4 represent?
7. What does the number 7 represent?
8. What permissions does `chmod 644 file.txt` create?
9. Why can `chmod 777` be dangerous?
10. What is the difference between execute permission on a file and on a directory?

### 17. Lab Completion Checklist

- [ ] Created the Lab 03 workspace.
- [ ] Created sample files and a directory.
- [ ] Inspected permissions using `ls -l`.
- [ ] Inspected metadata using `stat`.
- [ ] Practised symbolic permission notation.
- [ ] Practised numeric permission notation.
- [ ] Executed a practice script.
- [ ] Applied restricted permissions to a practice file.
- [ ] Applied permissions to a practice directory.
- [ ] Generated `permission-report.txt`.

### 18. Expected Workspace

```text
~/practical-labs/module02/lab03/
├── public.txt
├── private.txt
├── script.sh
├── shared-folder/
└── permission-report.txt
```

**Lab 03 Status:** Complete when the practical exercises and checklist are finished.

**Next Lab:** Lab 04 – Advanced Chmod Practice.
