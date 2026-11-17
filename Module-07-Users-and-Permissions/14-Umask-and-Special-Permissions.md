---
---

# Module 07 – Users and Permissions

# Lesson 14: Umask and Special Permissions

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what umask means.
2. Understand how umask affects newly created files and directories.
3. Display and change the current umask.
4. Calculate default permissions using umask.
5. Explain Setuid, Setgid, and Sticky Bit.
6. Identify special permissions in Linux.
7. Inspect special permission settings.
8. Understand the security risks associated with incorrect special permissions.

---

## 2. Introduction to Umask

Whenever you create a new file or directory in Linux, the operating system assigns initial permissions.

For example:

```bash
touch report.txt
```

Linux does not automatically give every new file full permissions.

Instead, the permissions are influenced by a setting called **umask**.

Umask means **user file creation mode mask**.

It determines which permissions should be removed from the maximum permissions requested when a new file or directory is created.

In simple terms:

**Umask helps control the default permissions of newly created files and directories.**

---

## 3. Understanding Default Creation Permissions

Linux commonly uses these starting permission modes:

| Object | Maximum Starting Permissions |
|---|---|
| Regular file | 666 |
| Directory | 777 |

Why are they different?

A regular file is not normally created with execute permission by default.

A directory needs execute permission for traversal.

These are starting modes, not necessarily the final permissions.

The umask removes permissions from them.

---

## 4. Checking the Current Umask

Use:

```bash
umask
```

Example output:

```text
0022
```

This means the current umask is `022`.

You may also see:

```bash
umask -S
```

This displays the umask symbolically.

Example:

```text
u=rwx,g=rx,o=rx
```

This represents the permissions that are allowed by the mask in symbolic form.

The output can vary depending on the system configuration.

---

## 5. Understanding Umask Calculation

Let's assume the umask is:

```text
022
```

For ordinary permission calculations:

### A. Regular Files

Maximum starting permissions:

```text
666
```

Umask:

```text
022
```

Result:

```text
644
```

Symbolic permissions:

```text
rw-r--r--
```

### B. Directories

Maximum starting permissions:

```text
777
```

Umask:

```text
022
```

Result:

```text
755
```

Symbolic permissions:

```text
rwxr-xr-x
```

Therefore, a common umask of `022` usually produces:

- Files: `644`
- Directories: `755`

**Important:** This subtraction explanation is a convenient shortcut for common masks. Technically, umask removes permission bits using a bitwise mask, rather than ordinary arithmetic subtraction in every possible case.

---

## 6. Understanding Other Common Umask Values

| Umask | Typical File Permissions | Typical Directory Permissions |
|---|---|---|
| 000 | 666 | 777 |
| 002 | 664 | 775 |
| 022 | 644 | 755 |
| 027 | 640 | 750 |
| 077 | 600 | 700 |

### Example: Umask 077

Regular file:

```text
666 masked by 077 = 600
```

Directory:

```text
777 masked by 077 = 700
```

This provides more restrictive default permissions.

It is useful in environments where users need private files by default.

---

## 7. Changing the Umask

You can temporarily change your current shell's umask using:

```bash
umask 077
```

Check it:

```bash
umask
```

Expected output:

```text
0077
```

Now create a file:

```bash
touch private.txt
```

Inspect its permissions:

```bash
ls -l private.txt
```

You should normally see:

```text
-rw-------
```

Create a directory:

```bash
mkdir private-folder
```

Inspect it:

```bash
ls -ld private-folder
```

You should normally see:

```text
drwx------
```

### Restoring the Previous Setting

For this lesson, if your previous umask was `022`, restore it using:

```bash
umask 022
```

A shell-level umask change normally affects that shell and its child processes. It does not automatically change every user's system-wide settings.

---

## 8. Introduction to Special Permissions

Linux has three important special permission bits:

1. Setuid
2. Setgid
3. Sticky Bit

They provide additional behaviour beyond ordinary read, write, and execute permissions.

These permissions are particularly important in Linux administration and cybersecurity.

---

## 9. Setuid (Set User ID)

Setuid is a special permission bit used primarily with executable files.

When an executable with Setuid is run, it normally runs with the effective user ID of the file's owner rather than simply the ID of the user who launched it.

Example:

```text
-rwsr-xr-x
```

Notice the `s` in the owner's execute position.

It indicates that Setuid is enabled and owner execute permission is also enabled.

### Numeric Value

```text
4
```

### Example

The traditional Linux `passwd` program is commonly configured with Setuid because changing a password involves updating protected account information.

Inspect it with:

```bash
ls -l /usr/bin/passwd
```

The exact permissions may vary by distribution.

### Security Importance

An incorrectly configured Setuid executable can create a privilege-escalation vulnerability.

Administrators must carefully inspect Setuid programs.

---

## 10. Setgid (Set Group ID)

Setgid is another special permission bit.

Its behaviour depends on whether it is applied to a file or directory.

### A. Setgid on an Executable File

When an executable with Setgid is run, its process normally receives the effective group ID associated with the file.

### B. Setgid on a Directory

When Setgid is enabled on a directory, newly created files and subdirectories normally inherit the directory's group ownership.

This is especially useful for shared team directories.

Example:

```bash
chmod 2775 team-directory
```

The leading `2` enables Setgid.

Inspect it:

```bash
ls -ld team-directory
```

A typical result is:

```text
drwxrwsr-x
```

Notice the `s` in the group's execute position.

### Numeric Value

```text
2
```

### Security Importance

Setgid helps maintain consistent group ownership in shared directories.

However, administrators must ensure that only authorised users belong to the relevant group.

---

## 11. Sticky Bit

The Sticky Bit is commonly used on shared directories.

It restricts who can delete or rename entries inside a directory.

When enabled, users generally can delete or rename an entry only if they:

- Own the file or directory entry.
- Own the containing directory.
- Have the relevant administrative privileges.

Example:

```bash
chmod 1777 shared-directory
```

The leading `1` enables the Sticky Bit.

Inspect it:

```bash
ls -ld shared-directory
```

Typical output:

```text
drwxrwxrwt
```

Notice the `t` at the end.

### Common Real-World Example

The `/tmp` directory commonly uses the Sticky Bit.

Inspect it:

```bash
ls -ld /tmp
```

It commonly appears similar to:

```text
drwxrwxrwt
```

This allows users to create temporary files while helping prevent them from deleting other users' files.

### Numeric Value

```text
1
```

---

## 12. Understanding Special Permission Values

| Special Permission | Numeric Value | Symbol |
|---|---:|---|
| Setuid | 4 | s in owner execute position |
| Setgid | 2 | s in group execute position |
| Sticky Bit | 1 | t in others execute position |

### Four-Digit Permission Structure

A four-digit permission number has this structure:

```text
SPECIAL OWNER GROUP OTHERS
```

Example:

```text
4755
```

Breakdown:

- `4` = Setuid.
- `7` = Owner has read, write, execute.
- `5` = Group has read and execute.
- `5` = Others have read and execute.

Another example:

```text
2775
```

- `2` = Setgid.
- `7` = Owner permissions.
- `7` = Group permissions.
- `5` = Others permissions.

Another example:

```text
1777
```

- `1` = Sticky Bit.
- `7` = Owner permissions.
- `7` = Group permissions.
- `7` = Others permissions.

---

## 13. Symbolic Methods for Special Permissions

You can also configure special permissions using symbolic mode.

### Setuid

```bash
chmod u+s filename
```

### Remove Setuid

```bash
chmod u-s filename
```

### Setgid

```bash
chmod g+s directoryname
```

### Remove Setgid

```bash
chmod g-s directoryname
```

### Sticky Bit

```bash
chmod +t directoryname
```

### Remove Sticky Bit

```bash
chmod -t directoryname
```

Always verify the result using:

```bash
ls -l
```

---

## 14. Understanding Security Risks

Special permissions are powerful and must be handled carefully.

### A. Setuid Misconfiguration

An unsafe Setuid executable may allow a normal user to perform operations with the effective privileges of another account.

This can lead to privilege escalation.

### B. Setgid Misconfiguration

Incorrect group ownership or permissions can expose shared files to unauthorised users.

### C. Sticky Bit Misunderstanding

The Sticky Bit does not make a directory private.

It only restricts deletion and renaming of entries under its rules.

### D. Excessive Permissions

Combining special permission bits with overly broad access permissions can create serious security problems.

### Security Rule

Never enable Setuid or Setgid on an executable simply to fix a permission error.

Only use special permissions when their behaviour is understood and required.

---

## 15. Practical Laboratory

### Lab Objective

Practise umask, Setgid, and Sticky Bit using disposable files and directories.

All work will take place inside:

```text
~/module07-lab/permissions
```

### Step 1: Enter the Practice Directory

```bash
cd ~/module07-lab/permissions
```

### Step 2: Check Your Current Umask

```bash
umask
```

Record the result.

### Step 3: Temporarily Change Umask

```bash
umask 077
```

Verify:

```bash
umask
```

### Step 4: Create a File

```bash
touch umask-file.txt
```

Inspect:

```bash
ls -l umask-file.txt
```

Expected ordinary permission:

```text
-rw-------
```

### Step 5: Create a Directory

```bash
mkdir umask-directory
```

Inspect:

```bash
ls -ld umask-directory
```

Expected ordinary permission:

```text
drwx------
```

### Step 6: Restore the Original Common Setting

If your recorded starting value was `0022`, restore it:

```bash
umask 022
```

If your starting value was different, restore that value instead.

Verify:

```bash
umask
```

### Step 7: Create a Setgid Practice Directory

```bash
mkdir setgid-lab
```

Enable Setgid:

```bash
chmod 2775 setgid-lab
```

Verify:

```bash
ls -ld setgid-lab
```

Look for `s` in the group execute position.

### Step 8: Create a File Inside the Setgid Directory

```bash
touch setgid-lab/shared-file.txt
```

Inspect ownership:

```bash
ls -l setgid-lab
```

The new file should inherit the directory's group ownership.

The actual group name depends on the ownership of your practice directory.

### Step 9: Create a Sticky Bit Directory

```bash
mkdir sticky-lab
```

Enable Sticky Bit:

```bash
chmod 1777 sticky-lab
```

Verify:

```bash
ls -ld sticky-lab
```

Look for `t` at the end of the permission string.

### Step 10: Inspect a Real Setuid Program

```bash
ls -l /usr/bin/passwd
```

Look for `s` in the owner's execute position if Setuid is enabled.

Do not modify this system executable.

### Step 11: Inspect the System Temporary Directory

```bash
ls -ld /tmp
```

Observe its permissions.

It commonly has the Sticky Bit enabled.

Do not change `/tmp` permissions.

### Step 12: Final Inspection

```bash
ls -la ~/module07-lab/permissions
```

Inspect your practice directories and files.

---

## 16. Knowledge Check

Answer the following questions:

1. What does umask mean?
2. What is the maximum starting permission mode for regular files?
3. What is the maximum starting permission mode for directories?
4. What does umask 022 commonly produce for files?
5. What does umask 077 commonly produce for directories?
6. Which command displays the current umask?
7. Which command changes the current shell's umask?
8. What is Setuid?
9. What is the numeric value of Setuid?
10. What is Setgid?
11. What happens when Setgid is enabled on a directory?
12. What is the Sticky Bit?
13. What is the numeric value of the Sticky Bit?
14. Why does `/tmp` commonly have the Sticky Bit?
15. What security risks can result from incorrectly configured special permissions?

---

## 17. Lesson Summary

In this lesson, we learned:

- Umask controls which permission bits are removed from newly created files and directories.
- Common starting modes are 666 for files and 777 for directories.
- Umask 022 commonly produces file permissions of 644 and directory permissions of 755.
- Setuid allows an executable to run with the effective user ID of its owner.
- Setgid can provide group inheritance on directories.
- Sticky Bit restricts deletion and renaming of directory entries.
- Special permissions have numeric values of 4, 2, and 1.
- Incorrect special permissions can create privilege-escalation and access-control vulnerabilities.

**Key Security Principle:** Special permissions should be enabled only when there is a clear administrative requirement and their security implications are understood.

---

**End of Lesson 14**

**Next Lesson:** Module 07 Assessment.