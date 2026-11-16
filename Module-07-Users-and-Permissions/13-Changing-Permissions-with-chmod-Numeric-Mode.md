# Module 07 – Users and Permissions

# Lesson 13: Changing Permissions with chmod – Numeric Mode

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain numeric permission mode.
2. Understand the values 4, 2, and 1.
3. Calculate Linux permission values.
4. Convert symbolic permissions into numeric permissions.
5. Change permissions using numeric mode.
6. Interpret common permission values such as 644, 755, and 700.
7. Apply numeric permissions safely.
8. Understand the security implications of permission configurations.

---

## 2. Introduction to Numeric Mode

In Lesson 12, we learned how to change permissions using symbolic mode.

For example:

```bash
chmod u+x script.sh
```

Linux also allows permissions to be changed using numbers.

This method is called **numeric mode** or **octal mode**.

Example:

```bash
chmod 755 script.sh
```

This sets the permissions of the file to:

- Owner: Read, write, execute.
- Group: Read, execute.
- Others: Read, execute.

Numeric mode is widely used by Linux administrators, developers, and cybersecurity professionals.

---

## 3. Understanding the Three Permission Values

Linux assigns a numerical value to each basic permission.

| Permission | Symbol | Numeric Value |
|---|---|---:|
| Read | r | 4 |
| Write | w | 2 |
| Execute | x | 1 |
| No permission | - | 0 |

These values are added together to produce a permission number.

### Example 1: Read Only

```text
r--
```

Calculation:

```text
4 + 0 + 0 = 4
```

Permission value: `4`

### Example 2: Read and Write

```text
rw-
```

Calculation:

```text
4 + 2 + 0 = 6
```

Permission value: `6`

### Example 3: Read, Write and Execute

```text
rwx
```

Calculation:

```text
4 + 2 + 1 = 7
```

Permission value: `7`

### Example 4: No Permission

```text
---
```

Calculation:

```text
0 + 0 + 0 = 0
```

Permission value: `0`

---

## 4. Understanding the Three Numeric Positions

A standard three-digit permission number represents:

1. Owner.
2. Group.
3. Others.

Example:

```text
755
```

Breakdown:

| Position | Category | Value |
|---|---|---:|
| First digit | Owner | 7 |
| Second digit | Group | 5 |
| Third digit | Others | 5 |

Now convert each digit:

- 7 = `rwx`
- 5 = `r-x`
- 5 = `r-x`

Therefore:

```text
755 = rwxr-xr-x
```

This means:

- Owner can read, write, and execute.
- Group can read and execute.
- Others can read and execute.

---

## 5. How to Calculate Permission Numbers

Let's practise calculating numeric permissions.

### Example 1: 644

First digit:

```text
4 + 2 = 6
```

Second digit:

```text
4 = 4
```

Third digit:

```text
4 = 4
```

Result:

```text
644
```

Symbolic representation:

```text
rw-r--r--
```

### Example 2: 700

Owner:

```text
4 + 2 + 1 = 7
```

Group:

```text
0 = 0
```

Others:

```text
0 = 0
```

Result:

```text
700 = rwx------
```

### Example 3: 750

Owner:

```text
4 + 2 + 1 = 7
```

Group:

```text
4 + 1 = 5
```

Others:

```text
0 = 0
```

Result:

```text
750 = rwxr-x---
```

### Example 4: 600

Owner:

```text
4 + 2 = 6
```

Group:

```text
0
```

Others:

```text
0
```

Result:

```text
600 = rw-------
```

---

## 6. Common Linux Permission Values

These permission configurations are frequently encountered.

| Numeric Mode | Symbolic Mode | Meaning |
|---|---|---|
| 777 | rwxrwxrwx | Everyone has full permissions |
| 755 | rwxr-xr-x | Owner has full access; others can read and execute |
| 750 | rwxr-x--- | Owner has full access; group can read and execute |
| 700 | rwx------ | Only owner has access |
| 644 | rw-r--r-- | Owner can read and write; others can read |
| 640 | rw-r----- | Owner can read and write; group can read |
| 600 | rw------- | Only owner can read and write |
| 444 | r--r--r-- | Everyone can read |
| 000 | --------- | No basic permissions |

**Important:** The suitability of a permission value depends on the file, its purpose, ownership, and the application's requirements.

Avoid using `777` as a general solution to permission problems. It gives every user full basic permissions.

---

## 7. Changing Permissions with Numeric Mode

The basic syntax is:

```bash
chmod NUMBER filename
```

### Example 1: Set a File to 644

```bash
chmod 644 report.txt
```

Result:

```text
-rw-r--r--
```

### Example 2: Set a Script to 755

```bash
chmod 755 script.sh
```

Result:

```text
-rwxr-xr-x
```

### Example 3: Set a Confidential File to 600

```bash
chmod 600 confidential.txt
```

Result:

```text
-rw-------
```

### Example 4: Set a Directory to 700

```bash
chmod 700 private
```

Result:

```text
drwx------
```

---

## 8. Understanding Numeric Mode for Files and Directories

Numeric permissions apply to both regular files and directories.

However, the practical meaning of permissions differs.

For a regular file:

- `4` allows reading contents.
- `2` allows modifying contents.
- `1` allows execution.

For a directory:

- `4` allows listing entries.
- `2` allows creating, deleting, and renaming entries, subject to other restrictions.
- `1` allows traversal.

Example:

```bash
chmod 750 documents
```

This gives:

- Owner: Full directory access.
- Group: Listing and traversal.
- Others: No access.

---

## 9. Understanding Four-Digit Permission Modes

Linux also supports a fourth leading digit.

This represents special permission bits:

| Special Permission | Numeric Value |
|---|---:|
| Setuid | 4 |
| Setgid | 2 |
| Sticky bit | 1 |

Example:

```bash
chmod 4755 filename
```

The leading `4` enables the setuid bit.

Another example:

```bash
chmod 2775 directoryname
```

The leading `2` enables the setgid bit.

We will study these special permissions in detail in Lesson 14.

For now, focus on ordinary three-digit permissions.

---

## 10. Verifying Numeric Permissions

Use:

```bash
ls -l filename
```

To display numeric permissions:

```bash
stat -c '%a %n' filename
```

To display both symbolic and numeric permissions:

```bash
stat -c '%A %a %n' filename
```

Example output:

```text
-rw-r--r-- 644 report.txt
```

This confirms that the file has numeric permissions of `644`.

---

## 11. Practical Laboratory

### Lab Objective

Practise calculating and applying numeric permissions to files and directories.

All exercises will take place inside:

```text
~/module07-lab/permissions
```

### Step 1: Enter the Practice Directory

```bash
cd ~/module07-lab/permissions
```

### Step 2: Create Three Practice Files

```bash
touch public.txt
touch private.txt
touch script.sh
```

### Step 3: Set Public File Permissions

```bash
chmod 644 public.txt
```

Verify:

```bash
stat -c '%A %a %n' public.txt
```

Expected permission:

```text
-rw-r--r-- 644 public.txt
```

### Step 4: Set Private File Permissions

```bash
chmod 600 private.txt
```

Verify:

```bash
stat -c '%A %a %n' private.txt
```

Expected permission:

```text
-rw------- 600 private.txt
```

### Step 5: Make the Script Executable

Add script content:

```bash
echo '#!/bin/bash' > script.sh
echo 'echo "Numeric permissions practice"' >> script.sh
```

Set permissions:

```bash
chmod 755 script.sh
```

Verify:

```bash
ls -l script.sh
```

Execute:

```bash
./script.sh
```

Expected output:

```text
Numeric permissions practice
```

### Step 6: Create a Private Directory

```bash
mkdir private-directory
```

Set permissions:

```bash
chmod 700 private-directory
```

Verify:

```bash
ls -ld private-directory
```

### Step 7: Practise Group Access

Create a directory:

```bash
mkdir team-directory
```

Set permissions:

```bash
chmod 750 team-directory
```

Verify:

```bash
stat -c '%A %a %n' team-directory
```

### Step 8: Compare All Permissions

Run:

```bash
ls -l
```

Then:

```bash
stat -c '%a %n' *.txt *.sh
```

Observe the differences between the permission configurations.

### Step 9: Practise Changing Permissions Again

Change `public.txt` from `644` to `600`:

```bash
chmod 600 public.txt
```

Verify:

```bash
stat -c '%A %a %n' public.txt
```

Now change it back:

```bash
chmod 644 public.txt
```

Verify again.

---

## 12. Common Numeric Permission Mistakes

### Mistake 1: Incorrect Calculation

Forgetting that:

- Read = 4.
- Write = 2.
- Execute = 1.

Remember that permission values are added together.

### Mistake 2: Confusing the Three Positions

For a three-digit permission:

- First digit = Owner.
- Second digit = Group.
- Third digit = Others.

### Mistake 3: Giving Every User Full Access

Example:

```bash
chmod 777 confidential.txt
```

This gives everyone read, write, and execute permissions.

It is generally unsafe for confidential files.

### Mistake 4: Applying File Permissions to Directories Without Understanding Them

For example, directory execute permission controls traversal.

A directory with read permission but no execute permission does not provide normal access to its entries.

### Mistake 5: Ignoring Ownership

Changing permissions does not change file ownership.

The `chmod` command changes permissions, while `chown` and `chgrp` change ownership information.

---

## 13. Knowledge Check

Answer the following questions:

1. What is numeric permission mode?
2. What numeric value represents read permission?
3. What numeric value represents write permission?
4. What numeric value represents execute permission?
5. What is the numeric value of `rwx`?
6. What does permission `644` mean?
7. What does permission `755` mean?
8. What does permission `600` mean?
9. What does permission `700` mean?
10. Convert `rw-r-----` into numeric mode.
11. Convert `rwxr-xr-x` into numeric mode.
12. What is the purpose of the first digit in a three-digit permission?
13. What does `chmod 640 report.txt` do?
14. Why is `chmod 777` generally discouraged?
15. What is the difference between `chmod` and `chown`?

---

## 14. Lesson Summary

In this lesson, we learned:

- Numeric mode uses numbers to represent permissions.
- Read = 4.
- Write = 2.
- Execute = 1.
- Permission values are added together.
- The first digit represents the owner.
- The second digit represents the group.
- The third digit represents others.
- `chmod 644` is a common file permission configuration.
- `chmod 755` is a common executable-file permission configuration.
- `chmod 600` restricts basic file access to the owner.
- `stat` helps verify numeric permissions.

**Security Principle:** Choose permission values based on the access users genuinely require, not simply because a command makes an error disappear.

---

**End of Lesson 13**

**Next Lesson:** Umask and Special Permissions.