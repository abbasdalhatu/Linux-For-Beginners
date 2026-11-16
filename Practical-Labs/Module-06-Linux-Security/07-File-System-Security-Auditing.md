
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 07: FILESYSTEM SECURITY AUDITING

### 1. Introduction

Filesystem security auditing is the process of examining files, directories, permissions, ownership, and access controls to identify security weaknesses.

Linux uses permissions and ownership to control who can access files and directories.

A poorly configured filesystem may expose:

- Confidential information.
- Application configuration files.
- User data.
- Backup files.
- Authentication-related information.
- Executable scripts.
- Sensitive system resources.

The goal of this lab is to learn how to identify risky permissions without modifying protected system files.

### 2. Understanding Linux File Permissions

Linux permissions are divided into three categories:

| Category | Meaning |
|---|---|
| User (u) | Owner of the file |
| Group (g) | Members of the file's group |
| Others (o) | Everyone else |

There are three basic permissions:

| Permission | Symbol | Meaning |
|---|---|---|
| Read | r | View file contents |
| Write | w | Modify file contents |
| Execute | x | Execute a file or access a directory |

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
-     rwx     r-x     r--
|      |       |       |
Type  Owner   Group   Others
```

The owner can read, write, and execute.

The group can read and execute.

Others can only read.

### 3. Understand Numeric Permissions

Linux permissions can also be represented numerically.

| Permission | Value |
|---|---:|
| Read | 4 |
| Write | 2 |
| Execute | 1 |

Examples:

| Numeric Mode | Meaning |
|---|---|
| 600 | Owner read/write |
| 640 | Owner read/write, group read |
| 644 | Owner read/write, others read |
| 700 | Owner full access |
| 750 | Owner full, group read/execute |
| 755 | Owner full, others read/execute |
| 777 | Everyone full access |

Permissions such as 777 should be avoided unless there is a clearly justified and controlled reason.

### 4. Create Your Lab Directory

Run:

```bash
mkdir -p ~/practical-labs/module06/lab07
cd ~/practical-labs/module06/lab07
```

### 5. Inspect Your Current Identity

Run:

```bash
whoami
```

Then:

```bash
id
```

This identifies your current user and group memberships.

### 6. Inspect File Permissions

Run:

```bash
ls -l
```

For a detailed listing of your home directory:

```bash
ls -la ~
```

Observe:

- File permissions.
- File owners.
- File groups.
- File sizes.
- Modification dates.

The `-a` option includes hidden files.

### 7. Inspect Directory Permissions

Run:

```bash
stat ~
```

Then:

```bash
stat ~/practical-labs
```

The `stat` command provides detailed information about files and directories.

Pay attention to:

- Access permissions.
- Owner.
- Group.
- File type.
- Modification time.

### 8. Understand Directory Permissions

Directory permissions behave differently from file permissions.

**Read permission:**

Allows a user to list directory entries.

**Write permission:**

Allows a user to create, delete, or rename entries, subject to other access controls.

**Execute permission:**

Allows a user to traverse or access entries within the directory when other permissions permit it.

A directory that is writable by everyone may create serious security risks.

### 9. Create Test Files

Create three files:

```bash
touch file-private.txt
touch file-shared.txt
touch file-public.txt
```

Create a test directory:

```bash
mkdir test-directory
```

Assign different permissions:

```bash
chmod 600 file-private.txt
chmod 640 file-shared.txt
chmod 644 file-public.txt
chmod 700 test-directory
```

Inspect:

```bash
ls -l
```

Record the differences.

These are test files inside your dedicated lab directory.

### 10. Inspect World-Writable Files

A world-writable file allows other local users to modify its contents.

Search your lab directory:

```bash
find ~/practical-labs/module06/lab07 -type f -perm -0002 -ls
```

Search for world-writable directories:

```bash
find ~/practical-labs/module06/lab07 -type d -perm -0002 -ls
```

A world-writable directory can be particularly dangerous because users may be able to create, delete, or rename entries.

Some shared directories legitimately use world-writable permissions with the sticky bit.

Do not automatically classify every world-writable object as a vulnerability. Investigate its purpose and surrounding controls.

### 11. Understand the Sticky Bit

The sticky bit is commonly used on shared directories.

Example:

```text
drwxrwxrwt
```

The final `t` represents the sticky bit.

It helps prevent ordinary users from deleting or renaming files belonging to other users in a shared directory, subject to ownership and privilege rules.

Inspect `/tmp`:

```bash
ls -ld /tmp
```

A common secure configuration is:

```text
drwxrwxrwt
```

### 12. Inspect SUID and SGID Permissions

Linux supports special permissions.

**SUID:** Set User ID.

An executable with SUID may run with the effective privileges of its owner.

**SGID:** Set Group ID.

An executable with SGID may run with the effective group privileges of its group. On directories, SGID helps new entries inherit the directory's group.

Inspect SUID files:

```bash
find ~/practical-labs/module06/lab07 -type f -perm -4000 -ls
```

Inspect SGID files:

```bash
find ~/practical-labs/module06/lab07 -type f -perm -2000 -ls
```

Your test directory should not normally contain SUID or SGID executables.

### 13. Inspect File Ownership

Run:

```bash
ls -ln ~/practical-labs/module06/lab07
```

The `-n` option displays numeric user and group IDs.

Inspect a specific file:

```bash
stat file-private.txt
```

Ownership determines which user and group are associated with a file.

Unexpected ownership may indicate a configuration problem.

### 14. Understand Access Control Lists

Access Control Lists (ACLs) allow more specific permissions than traditional owner-group-other permissions.

Inspect ACL information:

```bash
getfacl file-private.txt
```

If `getfacl` is unavailable, record that the ACL utility is not installed.

Inspect the ACLs of your test directory:

```bash
getfacl test-directory
```

ACLs may provide additional access to users or groups.

A security audit should consider ACL entries, not just the basic permissions shown by `ls -l`.

### 15. Inspect Sensitive System File Metadata

Linux contains files that require strict access controls.

Examples:

```text
/etc/passwd
/etc/shadow
/etc/sudoers
/etc/ssh/sshd_config
```

Inspect their permissions without displaying sensitive contents:

```bash
stat /etc/passwd
stat /etc/shadow
stat /etc/sudoers
```

If SSH configuration exists:

```bash
stat /etc/ssh/sshd_config
```

Do not display or copy password hashes, private keys, or confidential configuration values into your report.

### 16. Understand Umask

Umask controls the default permissions assigned to newly created files and directories.

Inspect your current umask:

```bash
umask
```

Display it symbolically:

```bash
umask -S
```

Example:

```text
0022
```

A common result is:

- New regular files: 644.
- New directories: 755.

The actual result depends on the application's requested creation mode and the umask.

### 17. Inspect Unexpected Executable Files

Find executable files inside your lab directory:

```bash
find ~/practical-labs/module06/lab07 -type f -perm /111 -ls
```

Investigate:

- Who owns the file?
- Why is it executable?
- Is it an expected script?
- Does it need executable permissions?

Executable permission should be granted only when needed.

### 18. Build a Filesystem Security Audit Script

Create:

```bash
nano filesystem-audit.sh
```

Enter:

```bash
#!/bin/bash

LAB="$HOME/practical-labs/module06/lab07"
REPORT="$LAB/filesystem-security-report.txt"

{
    echo "FILESYSTEM SECURITY AUDIT"
    echo "========================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo "User: $(whoami)"
    echo

    echo "CURRENT UMASK"
    umask
    echo

    echo "LAB FILE PERMISSIONS"
    ls -la "$LAB"
    echo

    echo "WORLD-WRITABLE FILES"
    find "$LAB" -type f -perm -0002 -ls
    echo

    echo "WORLD-WRITABLE DIRECTORIES"
    find "$LAB" -type d -perm -0002 -ls
    echo

    echo "SUID FILES"
    find "$LAB" -type f -perm -4000 -ls
    echo

    echo "SGID FILES"
    find "$LAB" -type f -perm -2000 -ls
    echo

    echo "EXECUTABLE FILES"
    find "$LAB" -type f -perm /111 -ls
    echo

    echo "SENSITIVE SYSTEM FILE METADATA"
    for FILE in /etc/passwd /etc/shadow /etc/sudoers; do
        if [ -e "$FILE" ]; then
            stat -c '%A %a %U:%G %n' "$FILE"
        fi
    done
    echo

    echo "TEMP DIRECTORY PERMISSIONS"
    stat -c '%A %a %U:%G %n' /tmp

} > "$REPORT" 2>&1

echo "Filesystem security report generated:"
echo "$REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x filesystem-audit.sh
```

Run:

```bash
./filesystem-audit.sh
```

Inspect the report:

```bash
less filesystem-security-report.txt
```

Press `q` to exit.

### 19. Analyze Your Findings

Answer these questions in your notes:

1. Did you find any world-writable files in your lab directory?
2. Did you find any world-writable directories?
3. Did you find SUID or SGID files?
4. What permissions were assigned to `file-private.txt`?
5. What permissions were assigned to `file-shared.txt`?
6. What permissions were assigned to `file-public.txt`?
7. What is the purpose of the sticky bit?
8. What is the difference between SUID and SGID?
9. Why should `/etc/shadow` have restricted permissions?
10. What is the purpose of umask?
11. How do ACLs extend traditional Linux permissions?
12. Why should executable permissions be reviewed?

### 20. Cleanup

After recording your findings, remove only the test files and directory created in this lab:

```bash
rm -f file-private.txt file-shared.txt file-public.txt
rm -rf test-directory
```

Keep the following files:

```text
filesystem-audit.sh
filesystem-security-report.txt
```

Do not use broad deletion commands or remove files outside your lab directory.

### 21. Lab Documentation

Create:

```bash
nano lab07-report.txt
```

Record:

```text
MODULE 06 – LAB 07
FILESYSTEM SECURITY AUDITING

1. Current Linux user:
2. Current umask:
3. Private test file permissions:
4. Shared test file permissions:
5. Public test file permissions:
6. World-writable files found:
7. World-writable directories found:
8. SUID files found:
9. SGID files found:
10. /tmp permissions:
11. /etc/shadow permissions:
12. ACL utility available: YES / NO
13. Purpose of the sticky bit:
14. Difference between SUID and SGID:
15. Three filesystem security recommendations:

AUDIT REPORT:
Generated successfully: YES / NO

TEST FILES:
Cleaned up: YES / NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 22. Completion Checklist

- [ ] Understood Linux filesystem permissions.
- [ ] Reviewed numeric permission values.
- [ ] Inspected ownership and directory permissions.
- [ ] Created test files with different permissions.
- [ ] Identified world-writable objects.
- [ ] Inspected sticky-bit permissions.
- [ ] Inspected SUID and SGID permissions.
- [ ] Reviewed file ownership.
- [ ] Inspected ACL information.
- [ ] Checked sensitive system file metadata.
- [ ] Inspected umask.
- [ ] Identified executable files.
- [ ] Generated the filesystem audit report.
- [ ] Completed lab documentation.
- [ ] Cleaned up test files.

**LAB 07 STATUS: READY FOR PRACTICAL COMPLETION**
