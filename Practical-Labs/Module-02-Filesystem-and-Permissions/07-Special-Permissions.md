
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 07: SPECIAL PERMISSIONS

### 1. Introduction

Linux has three important special permission mechanisms:

1. SUID – Set User ID
2. SGID – Set Group ID
3. Sticky Bit

These permissions provide additional control over how files and directories are accessed.

They are especially important in Linux system administration and cybersecurity because incorrect configurations can create serious security risks.

### 2. Learning Objectives

By the end of this lab, you should be able to:

- Explain SUID, SGID, and sticky bit.
- Identify special permissions using `ls -l`.
- Apply and remove special permissions using `chmod`.
- Understand the security implications of special permissions.
- Identify potentially risky SUID and SGID files.
- Investigate special permissions safely.

### 3. Prepare Your Lab Workspace

Create your working directory:

```bash
mkdir -p ~/practical-labs/module02/lab07
cd ~/practical-labs/module02/lab07
```

Create sample files and directories:

```bash
touch normal-file.txt
mkdir shared-directory
mkdir temporary-directory
```

Inspect:

```bash
ls -l
```

### 4. Understanding SUID

SUID means Set User ID.

When SUID is applied to an executable file, the program runs with the effective user ID of the file's owner rather than simply the identity of the user who launched it.

For example, a root-owned executable with SUID may execute with root privileges.

This can be dangerous if the program has vulnerabilities or performs unsafe operations.

**Important:** SUID is primarily meaningful on executable files. It does not normally provide useful SUID behavior on ordinary scripts on Linux.

### 5. Apply SUID

Set a normal permission first:

```bash
chmod 755 normal-file.txt
```

Apply SUID:

```bash
chmod u+s normal-file.txt
```

Inspect:

```bash
ls -l normal-file.txt
```

You may see:

```text
-rwsr-xr-x
```

The `s` in the owner's execute position represents SUID when the execute bit is also set.

For this exercise, the file is not executable, so you may see a capital `S`:

```text
-rwSr-xr-x
```

This indicates that SUID is set but the owner's execute permission is missing.

Remove SUID:

```bash
chmod u-s normal-file.txt
```

Verify:

```bash
ls -l normal-file.txt
```

### 6. Numeric SUID Notation

Special permissions use an additional leading octal digit.

| Special permission | Numeric value |
|---|---:|
| SUID | 4 |
| SGID | 2 |
| Sticky bit | 1 |

Examples:

```bash
chmod 4755 normal-file.txt
chmod 0755 normal-file.txt
```

`4755` means:
- SUID: enabled.
- Owner: `rwx`.
- Group: `r-x`.
- Others: `r-x`.

`0755` means ordinary permissions without special bits.

For your practice file:

```bash
chmod 4755 normal-file.txt
ls -l normal-file.txt
```

Remove the special permission:

```bash
chmod 0755 normal-file.txt
```

### 7. Understanding SGID

SGID means Set Group ID.

Its behavior depends on the object.

**On executable files:**

The program runs with the effective group ID of the file's group.

**On directories:**

New files and directories created inside inherit the directory's group ownership.

This is useful for shared project directories.

### 8. Apply SGID to a Directory

Apply SGID:

```bash
chmod g+s shared-directory
```

Inspect:

```bash
ls -ld shared-directory
```

Expected permission indicator:

```text
drwxr-sr-x
```

The `s` in the group's execute position represents SGID.

### 9. SGID Numeric Notation

Use:

```bash
chmod 2775 shared-directory
```

This means:
- SGID enabled.
- Owner has full permissions.
- Group has full permissions.
- Others can read and execute.

Create a test file:

```bash
touch shared-directory/report.txt
```

Inspect group ownership:

```bash
ls -l shared-directory
```

The new file should inherit the directory's group, subject to filesystem and application behavior.

### 10. Understanding the Sticky Bit

The sticky bit is commonly used on shared directories.

It restricts who can delete or rename entries inside the directory.

When a directory has the sticky bit, a user generally can delete or rename an entry only if they:

- Own the file.
- Own the directory.
- Have the relevant privileged permissions, such as root.

The sticky bit does not prevent users from reading files or writing to files when other permissions allow it.

A common example is `/tmp`.

Inspect:

```bash
ls -ld /tmp
```

You will commonly see:

```text
drwxrwxrwt
```

The final `t` represents the sticky bit.

### 11. Apply Sticky Bit

Apply it to your temporary directory:

```bash
chmod +t temporary-directory
```

Inspect:

```bash
ls -ld temporary-directory
```

You should see a `t` at the end of the permission string.

Remove it:

```bash
chmod -t temporary-directory
```

Verify:

```bash
ls -ld temporary-directory
```

### 12. Numeric Sticky Bit Notation

The sticky bit has numeric value `1`.

Apply:

```bash
chmod 1777 temporary-directory
```

This represents:
- Sticky bit enabled.
- Owner: full permissions.
- Group: full permissions.
- Others: full permissions.

The sticky bit controls deletion and renaming behavior; it does not make the directory private.

### 13. Combining Special Permissions

You can combine SUID, SGID, and sticky bit in one permission value.

Example:

```bash
chmod 7755 normal-file.txt
```

The first digit represents the special permissions:

- `4` = SUID.
- `2` = SGID.
- `1` = Sticky bit.

However, do not apply all special permissions indiscriminately. Each has a specific purpose, and unnecessary special permissions can increase security risk.

For a combined numeric example:

```bash
chmod 6755 normal-file.txt
ls -l normal-file.txt
```

This sets SUID and SGID.

Remove them:

```bash
chmod 0755 normal-file.txt
```

### 14. Identify Special Permissions

Use `find` to locate SUID files:

```bash
find . -type f -perm -4000 -ls
```

Locate SGID files:

```bash
find . -type f -perm -2000 -ls
```

Locate directories with the sticky bit:

```bash
find . -type d -perm -1000 -ls
```

Search the entire local filesystem for SUID files:

```bash
sudo find / -type f -perm -4000 -ls 2>/dev/null
```

Search for SGID files:

```bash
sudo find / -type f -perm -2000 -ls 2>/dev/null
```

These commands inspect metadata. They do not exploit the files.

### 15. Security Investigation

Special permissions can be legitimate, but unexpected permissions deserve investigation.

When examining a special-permission file, ask:

1. What is the file's purpose?
2. Who owns it?
3. Which group owns it?
4. Is it part of an installed package?
5. Is its special permission required?
6. Has its ownership or permission changed unexpectedly?

Inspect an example:

```bash
stat /usr/bin/passwd
```

Also inspect:

```bash
ls -l /usr/bin/passwd
```

The `passwd` utility commonly uses SUID because it needs to perform privileged password-management operations.

Do not modify permissions on system executables during this exercise.

### 16. Create a Special-Permissions Audit Report

Run:

```bash
cd ~/practical-labs/module02/lab07
```

Generate your report:

```bash
{
    echo "SPECIAL PERMISSIONS AUDIT"
    echo "========================="
    echo "Date: $(date)"
    echo "User: $(whoami)"
    echo
    echo "SUID FILES IN LAB:"
    find . -type f -perm -4000 -ls
    echo
    echo "SGID FILES IN LAB:"
    find . -type f -perm -2000 -ls
    echo
    echo "STICKY DIRECTORIES IN LAB:"
    find . -type d -perm -1000 -ls
    echo
    echo "DIRECTORY PERMISSIONS:"
    ls -ld .
    ls -ld shared-directory temporary-directory
} > special-permissions-report.txt
```

Read:

```bash
cat special-permissions-report.txt
```

### 17. Lab Cleanup

Remove the special permissions from your practice objects:

```bash
chmod u-s normal-file.txt
chmod g-s shared-directory
chmod -t temporary-directory
```

Verify:

```bash
ls -l
ls -ld shared-directory temporary-directory
```

Your practice objects should no longer have the special permissions you applied.

### 18. Knowledge Check

1. What does SUID stand for?
2. What happens when SUID is applied to an executable?
3. What does SGID do on a directory?
4. What is the purpose of the sticky bit?
5. Which special permission has numeric value `4`?
6. Which special permission has numeric value `2`?
7. Which special permission has numeric value `1`?
8. What does the `t` in `drwxrwxrwt` represent?
9. Why can unnecessary SUID permissions be dangerous?
10. Why should system special-permission files be investigated before changing them?

### 19. Lab Completion Checklist

- [ ] Created the Lab 07 workspace.
- [ ] Applied and removed SUID.
- [ ] Practised SUID numeric notation.
- [ ] Applied SGID to a directory.
- [ ] Practised SGID numeric notation.
- [ ] Applied and removed the sticky bit.
- [ ] Practised numeric special permissions.
- [ ] Identified SUID files.
- [ ] Identified SGID files.
- [ ] Identified sticky-bit directories.
- [ ] Inspected a legitimate system SUID executable.
- [ ] Generated `special-permissions-report.txt`.
- [ ] Cleaned up the practice permissions.

### 20. Expected Workspace

```text
~/practical-labs/module02/lab07/
├── normal-file.txt
├── shared-directory/
│   └── report.txt
├── temporary-directory/
└── special-permissions-report.txt
```

**Lab 07 Status:** Complete when the practical exercises and checklist are finished.

**Next Lab:** Lab 08 – Links and Mount Points.
