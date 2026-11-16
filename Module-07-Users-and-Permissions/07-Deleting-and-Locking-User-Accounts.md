# Module 07 – Users and Permissions

# Lesson 07: Deleting and Locking User Accounts

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain why Linux accounts may need to be removed.
2. Delete a user account using `userdel`.
3. Understand the difference between `userdel` and `userdel -r`.
4. Lock and unlock user accounts.
5. Identify inactive accounts.
6. Understand the security risks of unused accounts.
7. Verify account deletion and account status.

---

## 2. Introduction to Account Deletion

Linux administrators sometimes need to remove user accounts.

For example:

- An employee leaves an organisation.
- A temporary laboratory account is no longer required.
- A test account has completed its purpose.
- An account was created by mistake.
- An organisation needs to remove an unauthorised account.

Removing unnecessary accounts helps reduce the number of possible entry points into a system.

However, account deletion must be handled carefully because important files may belong to the account.

---

## 3. Understanding the userdel Command

`userdel` is a Linux command used to delete user accounts.

### Basic Syntax

```bash
sudo userdel username
```

Example:

```bash
sudo userdel labanalyst
```

This removes the account entry.

However, it does not normally remove the user's home directory or all files owned by that user.

Those files may remain on the system.

### Important

Deleting an account does not automatically delete every file belonging to that user.

Administrators should inspect ownership and decide how to handle the user's files before deletion.

---

## 4. Deleting a User and Its Home Directory

The `-r` option removes the user's home directory and associated mail spool when supported.

Example:

```bash
sudo userdel -r labanalyst
```

This attempts to:

- Delete the user account.
- Remove the user's home directory.
- Remove the user's mail spool.

### Difference Between userdel and userdel -r

| Command | Action |
|---|---|
| `userdel username` | Deletes the account without automatically removing the home directory |
| `userdel -r username` | Deletes the account and removes its home directory and mail spool |

**Security warning:** The `-r` option is destructive. Always verify the username and inspect the home directory before using it.

Never use it on your main Kali account.

---

## 5. Understanding Account Locking

Sometimes an administrator does not want to delete an account permanently.

Instead, the administrator may temporarily restrict authentication.

This is called account locking.

For example:

```bash
sudo passwd -l labanalyst
```

This locks the password.

Another method is:

```bash
sudo usermod -L labanalyst
```

Both commands lock password authentication.

### Why Lock an Account?

- Suspected account compromise.
- Temporary suspension.
- Employee leave.
- Security investigation.
- Temporary access restriction.

Remember that locking a password does not necessarily disable SSH-key authentication, terminate active sessions, or block every other authentication method.

---

## 6. Unlocking an Account

An administrator can unlock a password that was previously locked.

Using `passwd`:

```bash
sudo passwd -u labanalyst
```

Using `usermod`:

```bash
sudo usermod -U labanalyst
```

Verify the result:

```bash
sudo passwd -S labanalyst
```

Only unlock accounts when authorised.

---

## 7. Understanding Account Expiration

Instead of deleting an account, an administrator can set an expiration date.

Example:

```bash
sudo usermod -e 2026-12-31 labanalyst
```

This sets the account expiration date.

Inspect the account's aging information:

```bash
sudo chage -l labanalyst
```

To remove the configured expiration date:

```bash
sudo usermod -e "" labanalyst
```

This is useful for temporary accounts.

For example, a university might create an account for a student participating in a short-term research project.

The account can be configured to expire after the project ends.

---

## 8. Checking Whether a User Exists

Before deleting or modifying an account, verify that it exists.

### Method 1: getent

```bash
getent passwd labanalyst
```

If the account exists, its entry should be displayed.

### Method 2: id

```bash
id labanalyst
```

This displays the account's identity and group memberships.

### Method 3: Check the Home Directory

```bash
ls -ld /home/labanalyst
```

The directory may exist even if the account has already been deleted.

This is one reason account and file checks should be performed separately.

---

## 9. Understanding Orphaned Files

An orphaned file is a file that remains on a system after its original user account has been removed.

The file can still retain the original numeric UID and GID as ownership information.

For example:

```text
-rw-r--r-- 1 1500 1500 2048 Nov 14 report.txt
```

This indicates numeric ownership.

The original username may no longer be available for that UID.

An administrator can investigate using:

```bash
ls -ln
```

Do not automatically delete files simply because their owner account no longer exists.

They may contain important evidence or business information.

---

## 10. Practical Laboratory

### Lab Objective

Practise account locking, unlocking, and safe account deletion.

We will use the temporary account:

`labanalyst`

### Step 1: Confirm the Account Exists

```bash
getent passwd labanalyst
```

If the account was renamed, use its current username.

### Step 2: Check the Current Status

```bash
sudo passwd -S labanalyst
```

### Step 3: Lock the Password

```bash
sudo passwd -l labanalyst
```

### Step 4: Verify the Lock

```bash
sudo passwd -S labanalyst
```

### Step 5: Unlock the Password

```bash
sudo passwd -u labanalyst
```

### Step 6: Verify the Unlock

```bash
sudo passwd -S labanalyst
```

### Step 7: Inspect the Home Directory

```bash
ls -ld /home/labanalyst
```

### Step 8: Inspect Account Information

```bash
id labanalyst
```

### Step 9: Delete the Practice Account

Only after confirming that you no longer need the practice account:

```bash
sudo userdel labanalyst
```

This removes the account without automatically deleting its home directory.

### Step 10: Verify Account Deletion

```bash
getent passwd labanalyst
```

No result should be returned if the account has been removed.

### Step 11: Inspect the Remaining Home Directory

```bash
ls -ld /home/labanalyst
```

The home directory may still exist.

Do not remove it automatically. Inspect it and confirm that no required data remains.

If you deliberately want to remove the remaining practice directory, first verify the exact path and its contents.

---

## 11. Understanding Common Errors

### Error 1: User Does Not Exist

Example:

```text
userdel: user 'labanalyst' does not exist
```

Solution:

```bash
getent passwd labanalyst
```

Verify the correct username.

### Error 2: User Is Currently Logged In

Linux may refuse to delete an account while it has active processes or sessions.

Inspect the user's processes:

```bash
ps -u labanalyst
```

An administrator should investigate and safely end relevant sessions before retrying deletion.

Do not terminate processes blindly.

### Error 3: Home Directory Still Exists

This is normal when the account is deleted without `-r`.

Inspect the directory:

```bash
ls -la /home/labanalyst
```

Determine whether the data should be retained, archived, or removed according to the organisation's policy.

---

## 12. Cybersecurity Applications

Account removal and locking are important in cybersecurity.

### A. Incident Response

Administrators may restrict password authentication for accounts suspected of compromise.

### B. Employee Offboarding

When an employee leaves an organisation, their access must be reviewed and revoked.

### C. Attack Surface Reduction

Removing unnecessary accounts reduces opportunities for unauthorised access.

### D. Digital Forensics

Files belonging to deleted accounts may contain useful investigation evidence.

### E. Temporary Account Management

Expiration settings help prevent temporary accounts from remaining active indefinitely.

**Important security principle:** Account removal should be part of a wider access-revocation process. Check active sessions, SSH keys, scheduled jobs, service dependencies, and other authentication methods.

---

## 13. Knowledge Check

Answer these questions.

1. What is the purpose of `userdel`?
2. What is the difference between `userdel` and `userdel -r`?
3. What happens to a user's home directory when the account is deleted without `-r`?
4. What does account locking mean?
5. Which command locks a user's password?
6. Which command unlocks a user's password?
7. Why might an administrator lock an account instead of deleting it?
8. What is an orphaned file?
9. How can you check whether a user account exists?
10. Why should account deletion be performed carefully?

### Practical Questions

11. Write a command to delete a user without automatically deleting the home directory.
12. Write a command to delete a user and their home directory.
13. Write a command to lock a user's password.
14. Write a command to unlock a user's password.
15. Write a command to display a user's account information.

---

## 14. Lesson Summary

In this lesson, we learned:

- `userdel` removes Linux user accounts.
- `userdel -r` also removes the home directory and mail spool.
- `passwd -l` locks password authentication.
- `passwd -u` unlocks password authentication.
- `usermod -L` and `usermod -U` provide alternative locking operations.
- Account expiration can be used for temporary accounts.
- Files may remain after their owner's account is deleted.
- Account deletion should follow careful security and data-retention procedures.

---

**End of Lesson 07**

**Next Lesson:** Managing Groups in Linux.