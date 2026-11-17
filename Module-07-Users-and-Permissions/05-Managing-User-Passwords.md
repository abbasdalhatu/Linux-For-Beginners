---
---

# Module 07 – Users and Permissions

# Lesson 05: Managing User Passwords

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand how Linux manages user passwords.
2. Change your own password.
3. Change another user's password as an administrator.
4. Lock and unlock user passwords.
5. Check password status.
6. Understand password expiration.
7. Use the `passwd` and `chage` commands.
8. Apply password security principles.

---

## 2. Introduction to Linux Password Management

Passwords help protect Linux user accounts from unauthorised access.

When a user attempts to log in, Linux verifies the supplied password using its authentication mechanisms.

Linux does not normally store passwords as readable plain text.

Instead, it stores password hashes in a protected account database, commonly `/etc/shadow`.

### What Is Password Hashing?

Hashing is a process that transforms data into a fixed-format value using a mathematical algorithm.

For example:

```text
Original password: ExamplePassword123
                  |
                  v
              Hashing
                  |
                  v
        Stored password hash
```

A password hash is not simply an encrypted password that can be decrypted to reveal the original.

Modern password storage uses dedicated password-hashing schemes with salts and computational work factors.

**Important:** Never share your real password or password hash in your notes or screenshots.

---

## 3. Understanding the passwd Command

The `passwd` command manages Linux account passwords.

It can be used to:

- Change a password.
- Assign a password to an account.
- Lock a password.
- Unlock a password.
- Display password status.

Basic syntax:

```bash
passwd
```

To change your own password, simply run:

```bash
passwd
```

Linux normally requests:

1. Your current password.
2. Your new password.
3. Confirmation of your new password.

The password characters will not appear as you type.

This is normal.

---

## 4. Changing Another User's Password

An administrator can change another user's password using `sudo`.

Example:

```bash
sudo passwd labanalyst
```

The administrator enters a new password for the account.

This operation does not require knowing the user's previous password.

### Important

Changing another user's password should only be performed by an authorised administrator.

---

## 5. Checking Password Status

You can inspect an account's password status using:

```bash
sudo passwd -S labanalyst
```

The output may resemble:

```text
labanalyst P 20680 0 99999 7 -1
```

The exact values depend on your system.

Common status indicators include:

| Indicator | Meaning |
|---|---|
| P | Password is set |
| L | Password is locked |
| NP | No password is set |

Other fields provide password-aging information.

---

## 6. Locking a User Password

Locking a password prevents the account from authenticating using that password.

Command:

```bash
sudo passwd -l labanalyst
```

Check the result:

```bash
sudo passwd -S labanalyst
```

The status should indicate that the password is locked.

### Why Lock an Account?

An administrator may lock an account when:

- An employee leaves an organisation.
- An account is suspected of compromise.
- A user should temporarily lose password-based access.
- An investigation requires restricting an account.

**Important security distinction:** Locking a password does not necessarily disable every possible way of accessing the account. SSH keys, existing sessions, or other authentication mechanisms may still work depending on the system configuration.

---

## 7. Unlocking a User Password

To unlock a password that was previously locked with `passwd -l`, use:

```bash
sudo passwd -u labanalyst
```

Verify:

```bash
sudo passwd -S labanalyst
```

The password should no longer be marked as locked.

Only unlock accounts when authorised and appropriate.

---

## 8. Understanding Password Expiration

Organisations sometimes require users to change their passwords periodically.

Linux provides the `chage` command for password-aging management.

`chage` means change age.

It manages password expiration and related account-aging settings.

### Command 1: View Password Aging

```bash
sudo chage -l labanalyst
```

This displays information such as:

- Last password change.
- Password expiration.
- Minimum password age.
- Maximum password age.
- Warning period.
- Account inactivity period.

### Command 2: Set Maximum Password Age

Example:

```bash
sudo chage -M 90 labanalyst
```

This sets the maximum password age to 90 days.

### Command 3: Set Minimum Password Age

```bash
sudo chage -m 1 labanalyst
```

This sets a minimum interval of one day before the password can normally be changed again.

### Command 4: Set a Password Expiration Warning

```bash
sudo chage -W 7 labanalyst
```

This sets a seven-day warning period before password expiration.

### Command 5: Require a Password Change at Next Login

```bash
sudo chage -d 0 labanalyst
```

This sets the last password-change date to zero, causing the user to be prompted to change the password at the next applicable login.

Do not apply these settings to your main account during this lesson.

---

## 9. Password Security Best Practices

A secure Linux environment requires good password practices.

### A. Use Long Passwords

Longer passwords or passphrases are generally harder to guess.

### B. Avoid Predictable Passwords

Avoid passwords based on:

- Your name.
- Your birthday.
- Simple number sequences.
- Common dictionary words.
- Publicly available personal information.

### C. Use Unique Passwords

Do not reuse important passwords across different accounts.

### D. Protect Password Hashes

Restrict access to `/etc/shadow`.

### E. Use Multi-Factor Authentication Where Appropriate

Multi-factor authentication adds another verification factor beyond a password.

### F. Avoid Unnecessary Password Exposure

Never put passwords directly into command arguments, scripts, or shared documents.

---

## 10. Practical Laboratory

### Lab Objective

Practise password management using the temporary account `labanalyst`.

### Step 1: Check the account

```bash
id labanalyst
```

If the account does not exist, return to Lesson 04 and create it before continuing.

### Step 2: Check its password status

```bash
sudo passwd -S labanalyst
```

### Step 3: Change its password

```bash
sudo passwd labanalyst
```

Enter a new practice password.

Do not use your main account password.

### Step 4: Verify the password status

```bash
sudo passwd -S labanalyst
```

### Step 5: Inspect password-aging information

```bash
sudo chage -l labanalyst
```

### Step 6: Set a password warning period

```bash
sudo chage -W 7 labanalyst
```

### Step 7: Verify the warning period

```bash
sudo chage -l labanalyst
```

### Step 8: Lock the practice account password

```bash
sudo passwd -l labanalyst
```

### Step 9: Confirm the account is locked

```bash
sudo passwd -S labanalyst
```

### Step 10: Unlock the practice account password

```bash
sudo passwd -u labanalyst
```

### Step 11: Verify the final status

```bash
sudo passwd -S labanalyst
```

Your practice account should now have an unlocked password.

---

## 11. Understanding Common Errors

### Error 1: Authentication Failure

Possible cause:

- Incorrect password.
- Insufficient administrative privileges.
- Authentication configuration problems.

Check that you are using the correct account and have the required privileges.

### Error 2: User Does Not Exist

Example:

```text
passwd: user 'analyst' does not exist
```

Solution:

```bash
getent passwd analyst
```

Verify the username.

### Error 3: Password Is Locked

A locked password may prevent normal password authentication.

An authorised administrator can inspect its status using:

```bash
sudo passwd -S labanalyst
```

---

## 12. Cybersecurity Applications

Password management is a major part of account security.

### A. Account Protection

Strong password practices reduce the risk of unauthorised access.

### B. Incident Response

Administrators can lock password authentication for an account suspected of compromise.

### C. Security Auditing

Security teams inspect password-aging settings and account status.

### D. Authentication Security

Password hashes must be protected against unauthorised access and offline guessing attempts.

### E. Access Revocation

Administrators can restrict password-based authentication when a user no longer requires access.

---

## 13. Knowledge Check

Answer the following questions.

1. What is the purpose of the `passwd` command?
2. Where are Linux password hashes normally stored?
3. What is password hashing?
4. How do you change your own password?
5. How do you change another user's password as an administrator?
6. What does `passwd -l` do?
7. What does `passwd -u` do?
8. What does `chage` manage?
9. What does `chage -M 90` mean?
10. What does `chage -d 0` do?

### Practical Questions

11. Write a command to check the password status of `labanalyst`.
12. Write a command to lock its password.
13. Write a command to unlock its password.
14. Write a command to display its password-aging information.
15. Write a command to set a seven-day password warning period.

---

## 14. Lesson Summary

In this lesson, we learned:

- Linux uses password hashes rather than storing normal passwords as plain text.
- `passwd` manages user passwords.
- `passwd -S` displays password status.
- `passwd -l` locks password authentication.
- `passwd -u` unlocks a previously locked password.
- `chage` manages password-aging settings.
- `/etc/shadow` contains protected password authentication information.
- Strong passwords and appropriate access controls improve account security.

---

**End of Lesson 05**

**Next Lesson:** Modifying User Accounts with usermod.