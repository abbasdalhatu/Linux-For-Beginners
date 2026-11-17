---
---

# Module 07 – Users and Permissions

# Lesson 06: Modifying User Accounts with usermod

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain the purpose of `usermod`.
2. Change a user's login name.
3. Change a user's home directory.
4. Change a user's login shell.
5. Modify a user's primary group.
6. Add a user to supplementary groups.
7. Lock and unlock accounts using `usermod`.
8. Verify account modifications.

---

## 2. Introduction to usermod

Linux administrators sometimes need to change existing user accounts.

For example:

- An employee changes departments.
- A user needs access to another group.
- A username must be corrected.
- A home directory needs to be moved.
- An account requires a different login shell.

Linux provides the `usermod` command for these operations.

`usermod` means **user modify**.

It modifies an existing Linux user account.

### Basic Syntax

```bash
sudo usermod [options] username
```

Example:

```bash
sudo usermod -c "Security Analyst" labanalyst
```

This changes the account description.

---

## 3. Understanding Important usermod Options

| Option | Purpose |
|---|---|
| `-l` | Change the login name |
| `-d` | Change the home directory |
| `-m` | Move existing home directory contents |
| `-s` | Change the login shell |
| `-g` | Change the primary group |
| `-G` | Set supplementary groups |
| `-aG` | Append supplementary groups |
| `-L` | Lock the account password |
| `-U` | Unlock the account password |
| `-c` | Change the account description |
| `-e` | Set an account expiration date |

Let's examine these options.

---

## 4. Changing a Username

The `-l` option changes a user's login name.

Example:

```bash
sudo usermod -l securityanalyst labanalyst
```

This changes:

```text
Old username: labanalyst
New username: securityanalyst
```

However, changing the username does not automatically rename the user's home directory.

The home directory may still be:

```text
/home/labanalyst
```

You can inspect the account using:

```bash
getent passwd securityanalyst
```

**Important:** Avoid changing the username of your active account. Perform username modification on a separate practice account.

---

## 5. Changing a Home Directory

The `-d` option changes the home directory recorded for an account.

Example:

```bash
sudo usermod -d /home/securityanalyst securityanalyst
```

This updates the account's home-directory field.

### Moving Existing Home Directory Contents

Use `-m` together with `-d`:

```bash
sudo usermod -d /home/securityanalyst -m securityanalyst
```

The `-m` option moves the existing home directory contents to the new location.

**Important:** Make sure the destination is appropriate and does not contain unrelated data. Do not experiment with home-directory moves on your main account.

---

## 6. Changing a Login Shell

A login shell is the command interpreter started for an interactive login.

Common shells include:

- Bash: `/bin/bash`
- Zsh: `/usr/bin/zsh`
- Dash: `/usr/bin/dash`

The `-s` option changes the login shell.

Example:

```bash
sudo usermod -s /bin/bash securityanalyst
```

Check the result:

```bash
getent passwd securityanalyst
```

The final field should show the configured login shell.

You can inspect available login shells using:

```bash
cat /etc/shells
```

Only assign a shell that exists and is appropriate for the account.

---

## 7. Changing a Primary Group

The `-g` option changes a user's primary group.

Example:

```bash
sudo usermod -g developers securityanalyst
```

The `developers` group must already exist.

Check the group:

```bash
getent group developers
```

Verify the user's new primary group:

```bash
id securityanalyst
```

---

## 8. Adding a User to Supplementary Groups

The `-aG` options are particularly important.

- `-a` means append.
- `-G` specifies supplementary groups.

Example:

```bash
sudo usermod -aG developers securityanalyst
```

This adds the user to the `developers` group without removing existing supplementary group memberships.

### Why Is -aG Important?

Consider this command:

```bash
sudo usermod -G developers securityanalyst
```

It replaces the user's existing supplementary group list with the specified list.

This can unintentionally remove existing memberships.

For ordinary group additions, prefer:

```bash
sudo usermod -aG groupname username
```

Always verify the result:

```bash
id securityanalyst
```

---

## 9. Locking and Unlocking Accounts

You can lock an account's password using:

```bash
sudo usermod -L securityanalyst
```

To unlock it:

```bash
sudo usermod -U securityanalyst
```

Check its status:

```bash
sudo passwd -S securityanalyst
```

**Security note:** Like `passwd -l`, locking the password does not necessarily terminate active sessions or disable every authentication mechanism.

---

## 10. Changing Account Descriptions

The `-c` option changes the account's descriptive information.

Example:

```bash
sudo usermod -c "Senior Security Analyst" securityanalyst
```

Inspect the result:

```bash
getent passwd securityanalyst
```

The description appears in the fifth field of the account entry.

---

## 11. Setting an Account Expiration Date

The `-e` option sets an account expiration date.

Example:

```bash
sudo usermod -e 2026-12-31 securityanalyst
```

This sets the account expiration date to December 31, 2026.

The date format is:

```text
YYYY-MM-DD
```

To remove a previously configured expiration date:

```bash
sudo usermod -e "" securityanalyst
```

Verify account-aging information using:

```bash
sudo chage -l securityanalyst
```

---

## 12. Practical Laboratory

### Lab Objective

Modify a temporary Linux user account and verify each change.

We will use:

```text
labanalyst
```

First, confirm that the account exists:

```bash
getent passwd labanalyst
```

If you previously renamed it, use its current username.

### Step 1: Inspect the Current Account

```bash
id labanalyst
```

```bash
getent passwd labanalyst
```

### Step 2: Change the Account Description

```bash
sudo usermod -c "Linux Security Practice Account" labanalyst
```

Verify:

```bash
getent passwd labanalyst
```

### Step 3: Inspect the Available Shells

```bash
cat /etc/shells
```

### Step 4: Set the Login Shell

```bash
sudo usermod -s /bin/bash labanalyst
```

Verify:

```bash
getent passwd labanalyst
```

### Step 5: Inspect Existing Groups

```bash
id labanalyst
```

### Step 6: Create a Practice Group

If the group does not already exist:

```bash
getent group securitylab
```

If no group is returned, create it:

```bash
sudo groupadd securitylab
```

### Step 7: Add the User to the Group

```bash
sudo usermod -aG securitylab labanalyst
```

### Step 8: Verify Group Membership

```bash
id labanalyst
```

You should see `securitylab` among the supplementary groups.

### Step 9: Inspect the Home Directory

```bash
getent passwd labanalyst
```

Observe the home-directory field.

Do not move or rename your main account's home directory during this exercise.

### Step 10: Inspect the Account Status

```bash
sudo passwd -S labanalyst
```

The account should have an unlocked password when you finish.

---

## 13. Common Mistakes and Troubleshooting

### Mistake 1: Forgetting -a with -G

Incorrect for adding one supplementary group while preserving others:

```bash
sudo usermod -G securitylab labanalyst
```

Better:

```bash
sudo usermod -aG securitylab labanalyst
```

### Mistake 2: Changing an Active Account

Changing the username or home directory of the account currently being used can create problems.

Use a separate practice account.

### Mistake 3: Specifying a Nonexistent Group

Example:

```text
usermod: group 'securitylab' does not exist
```

Check first:

```bash
getent group securitylab
```

Create the group if appropriate.

### Mistake 4: Using a Nonexistent Shell

Check available shells:

```bash
cat /etc/shells
```

Use a valid shell path.

---

## 14. Cybersecurity Applications

The `usermod` command is important in Linux security administration.

### A. Access Control

Administrators can add or remove group memberships to manage access.

### B. Account Maintenance

Usernames, descriptions, and home directories can be updated when organisational requirements change.

### C. Incident Response

Administrators can lock password authentication for accounts suspected of compromise.

### D. Privilege Management

Supplementary group membership can affect access to sensitive resources.

### E. Account Lifecycle Management

Account expiration settings help manage temporary accounts.

---

## 15. Knowledge Check

Answer the following questions.

1. What is the purpose of `usermod`?
2. What does the `-l` option do?
3. What does the `-d` option do?
4. What is the purpose of `-m`?
5. What does the `-s` option change?
6. Explain the difference between `-g` and `-G`.
7. Why is `-aG` safer when adding supplementary groups?
8. What does `usermod -L` do?
9. What does `usermod -U` do?
10. What is the purpose of the `-e` option?

### Practical Questions

11. Write a command to change `labanalyst` into `securityanalyst`.
12. Write a command to change a user's login shell to Bash.
13. Write a command to add a user to `securitylab` without removing existing supplementary groups.
14. Write a command to change the user's account description.
15. Write a command to display the user's account information.

---

## 16. Lesson Summary

In this lesson, we learned:

- `usermod` modifies existing Linux user accounts.
- `-l` changes the login name.
- `-d` changes the home directory.
- `-m` moves existing home-directory contents.
- `-s` changes the login shell.
- `-g` changes the primary group.
- `-aG` adds supplementary groups while preserving existing memberships.
- `-L` and `-U` lock and unlock password authentication.
- `-e` sets an account expiration date.
- Account changes should always be verified.

---

**End of Lesson 06**

**Next Lesson:** Deleting and Locking User Accounts.