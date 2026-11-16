
# PRACTICAL LABS
# MODULE 04 – LINUX SYSTEM ADMINISTRATION

## LAB 02: USER AND GROUP ADMINISTRATION

### 1. Lab Overview

Linux is a multi-user operating system. This means multiple users can access the same system while having different permissions and levels of access.

As a Linux system administrator, you must understand how to:

- Create and manage user accounts.
- Create and manage groups.
- Assign users to groups.
- Inspect user identities and permissions.
- Understand account configuration files.
- Apply the principle of least privilege.

**Lab environment:** Your Kali Linux virtual machine.

**Safety:** Perform all exercises inside your Kali VM. Do not modify important system accounts or remove existing users.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain the difference between users and groups.
2. Inspect existing user accounts.
3. Create and manage a dedicated lab user.
4. Create and manage groups.
5. Add users to supplementary groups.
6. Understand `/etc/passwd`, `/etc/shadow`, and `/etc/group`.
7. Inspect account permissions and group membership.

### 3. Understanding Linux Users

A Linux user account represents an identity on the operating system.

Each user has a unique User ID (UID).

Common user categories include:

| User category | Description |
|---|---|
| Root | Superuser with administrative privileges |
| System users | Accounts used by operating system services |
| Regular users | Accounts used by people |

The root account normally has UID `0`.

Regular user accounts commonly have higher UID values.

Inspect your current identity:

```bash
whoami
```

Display your user ID and group information:

```bash
id
```

Display your UID:

```bash
id -u
```

Display your primary group:

```bash
id -gn
```

Display all groups you belong to:

```bash
groups
```

### 4. Understanding Linux Groups

A group is a collection of user accounts.

Groups make it easier to manage permissions for multiple users.

For example, an organization might create:

- `developers` – for software developers.
- `security` – for security personnel.
- `administrators` – for authorized administrators.

Instead of assigning permissions to individual users repeatedly, administrators can assign permissions to groups.

Inspect your groups:

```bash
groups
```

Display detailed group information:

```bash
id
```

### 5. Understanding Important Account Files

Linux stores user and group information in several configuration files.

#### A. /etc/passwd

This file contains account information.

Inspect it:

```bash
cat /etc/passwd
```

View only the first few entries:

```bash
head /etc/passwd
```

A typical entry looks like:

```text
username:x:1000:1000:User Name:/home/username:/bin/bash
```

The fields represent:

1. Username
2. Password placeholder
3. User ID (UID)
4. Primary Group ID (GID)
5. User description
6. Home directory
7. Login shell

#### B. /etc/shadow

This file stores password-related account information.

Inspect its permissions:

```bash
ls -l /etc/shadow
```

Do not display or share its password hashes.

The file is restricted because it contains sensitive authentication information.

#### C. /etc/group

This file contains group information.

Inspect it:

```bash
cat /etc/group
```

A typical entry looks like:

```text
developers:x:1002:student1,student2
```

The fields represent:

1. Group name
2. Group password placeholder
3. Group ID (GID)
4. Group members

### 6. Create a Dedicated Lab User

We will create a separate account for practice.

First, create your lab directory:

```bash
mkdir -p ~/practical-labs/module04/lab02
```

Create a user named `labuser`:

```bash
sudo adduser labuser
```

Follow the prompts to configure the account.

You may enter a test password for this isolated VM. Do not reuse your personal password.

Verify that the account exists:

```bash
id labuser
```

Inspect its account entry:

```bash
grep '^labuser:' /etc/passwd
```

Check its home directory:

```bash
ls -ld /home/labuser
```

### 7. Create a Group

Create a group called `securitylab`:

```bash
sudo groupadd securitylab
```

Verify the group:

```bash
getent group securitylab
```

Inspect its group ID:

```bash
getent group securitylab
```

### 8. Add a User to a Group

Add `labuser` to the `securitylab` group:

```bash
sudo usermod -aG securitylab labuser
```

Important:

- `usermod` modifies an existing account.
- `-a` means append.
- `-G` specifies supplementary groups.

The `-a` option is important because using `-G` without it can replace the user's existing supplementary group memberships.

Verify membership:

```bash
id labuser
```

You should see `securitylab` among the user's groups.

### 9. Create a Group-Based Workspace

Create a workspace for the security group:

```bash
sudo mkdir -p /opt/securitylab
```

Assign group ownership:

```bash
sudo chown root:securitylab /opt/securitylab
```

Give the owner and group full access, while others receive no access:

```bash
sudo chmod 770 /opt/securitylab
```

Inspect the result:

```bash
ls -ld /opt/securitylab
```

Understand the permission:

```text
drwxrwx--- root securitylab /opt/securitylab
```

- Owner: read, write, execute.
- Group: read, write, execute.
- Others: no permissions.

This is an example of group-based access control.

### 10. Useful User Administration Commands

| Command | Purpose |
|---|---|
| `whoami` | Display current username |
| `id` | Display UID, GID and groups |
| `groups` | Display group membership |
| `adduser` | Create a user interactively |
| `useradd` | Create a user account |
| `passwd` | Set or change a password |
| `usermod` | Modify a user account |
| `userdel` | Delete a user account |
| `groupadd` | Create a group |
| `groupmod` | Modify a group |
| `groupdel` | Delete a group |
| `getent passwd` | Query user account information |
| `getent group` | Query group information |

### 11. Security Best Practices

As a security-conscious administrator:

1. Never use the root account for ordinary activities.
2. Give users only the permissions they need.
3. Avoid unnecessary administrator privileges.
4. Use groups to manage shared access.
5. Protect `/etc/shadow`.
6. Remove or disable accounts that are no longer needed through an approved administrative process.
7. Never share passwords or password hashes.
8. Review group membership regularly.

### 12. Practical Exercises

Complete the following exercises in your Kali VM.

**Exercise 1 – User Identification**

Run:

```bash
whoami
id
groups
```

Record your current username, UID, primary group and supplementary groups.

**Exercise 2 – Account Inspection**

Run:

```bash
head /etc/passwd
ls -l /etc/shadow
getent group
```

Explain what each command reveals.

**Exercise 3 – User Creation**

Create `labuser` and verify the account:

```bash
sudo adduser labuser
id labuser
```

**Exercise 4 – Group Administration**

Create `securitylab` and add `labuser` to it:

```bash
sudo groupadd securitylab
sudo usermod -aG securitylab labuser
id labuser
```

**Exercise 5 – Group-Based Permissions**

Create `/opt/securitylab`, assign its group ownership to `securitylab`, and apply permission `770`.

Verify:

```bash
ls -ld /opt/securitylab
```

### 13. Lab Documentation

Inside your lab directory, create a report:

```bash
nano ~/practical-labs/module04/lab02/lab02-report.txt
```

Record:

```text
MODULE 04 – LAB 02
USER AND GROUP ADMINISTRATION

1. Current username:
2. Current UID:
3. Primary group:
4. Lab user created:
5. Group created:
6. Group membership verified:
7. Workspace created:
8. Workspace permissions:
9. What I learned:
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 14. Cleanup (Optional)

Only perform this after completing and documenting the exercises.

Remove the test workspace:

```bash
sudo rm -r /opt/securitylab
```

Remove the test user:

```bash
sudo deluser --remove-home labuser
```

Remove the test group:

```bash
sudo groupdel securitylab
```

These commands remove only the dedicated lab resources created in this exercise. Do not use them on your normal accounts or system groups.

### 15. Knowledge Check

1. What is the difference between a user and a group?
2. What does UID mean?
3. Which UID normally belongs to root?
4. What information is stored in `/etc/passwd`?
5. Why is `/etc/shadow` protected?
6. What does `usermod -aG` do?
7. What does permission `770` mean?
8. Why is group-based access control useful?
9. What is the principle of least privilege?
10. Why should administrators avoid using root for everyday tasks?

### 16. Lab Completion Checklist

- [ ] Inspected current user identity.
- [ ] Inspected user and group configuration files.
- [ ] Created the `labuser` account.
- [ ] Created the `securitylab` group.
- [ ] Added `labuser` to the group.
- [ ] Created a group-owned workspace.
- [ ] Verified permissions.
- [ ] Created the lab report.
- [ ] Completed cleanup if desired.

**LAB 02 STATUS: READY FOR PRACTICAL COMPLETION**
