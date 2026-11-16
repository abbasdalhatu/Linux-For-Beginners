
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 03: USER ACCOUNT SECURITY

### 1. Introduction

User account security is the process of protecting Linux accounts against unauthorized access and misuse.

Linux is a multi-user operating system. Multiple users can have separate accounts, permissions, groups, and access privileges.

A poorly secured user account can become an entry point for an attacker.

For example:

Imagine a company has a Linux server containing confidential business information.

If every employee uses the same administrator account, it becomes difficult to determine who performed a particular action.

A better approach is to provide each employee with an individual account and assign only the permissions required for their responsibilities.

### 2. Understanding Linux User Accounts

Every Linux user account has important properties:

- Username.
- User ID (UID).
- Primary group.
- Supplementary groups.
- Home directory.
- Login shell.
- Authentication information.

Example:

```text
Username: student
UID: 1000
Primary Group: student
Home Directory: /home/student
Shell: /bin/bash
```

Your actual account details may be different.

### 3. Types of Linux Accounts

Linux accounts can be grouped into several categories.

#### A. Root Account

The root account is the superuser account.

It has extensive administrative privileges over the system.

Root UID:

```text
0
```

Because root has powerful privileges, it should be used carefully.

#### B. Regular User Accounts

These accounts are created for ordinary users.

They usually have limited permissions and cannot modify protected system resources without additional authorization.

#### C. System Accounts

System accounts are commonly used by services and applications.

Examples include accounts associated with web servers, databases, and other system components.

These accounts often have restricted login capabilities.

### 4. Understanding UID and GID

**UID – User Identifier**

A UID is a numerical identifier assigned to a Linux user.

**GID – Group Identifier**

A GID identifies a Linux group.

The root user has UID 0.

Regular users commonly have higher UIDs, but exact allocation ranges depend on system configuration.

### 5. Important User Account Files

Linux stores account and authentication information in important system files.

| File | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Password hashes and password aging information |
| `/etc/group` | Group information |
| `/etc/gshadow` | Secure group authentication information |
| `/etc/login.defs` | Login and account configuration defaults |

These files are important security targets.

Do not modify them manually during this introductory lab.

### 6. Inspect Your Current User Account

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module06/lab03
```

Move into it:

```bash
cd ~/practical-labs/module06/lab03
```

#### Task 1: Identify Your Username

```bash
whoami
```

#### Task 2: Inspect Your UID and GID

```bash
id
```

#### Task 3: Display Your Groups

```bash
groups
```

#### Task 4: Inspect Your Home Directory

```bash
echo "$HOME"
```

#### Task 5: Identify Your Login Shell

```bash
echo "$SHELL"
```

You can also inspect your account entry:

```bash
getent passwd "$(whoami)"
```

Understand the fields:

```text
username:password:UID:GID:comment:home_directory:login_shell
```

The password field usually contains `x`, indicating that the password hash is stored separately in `/etc/shadow`.

### 7. Inspect the User Account Database

#### Task 6: Inspect `/etc/passwd`

Run:

```bash
cat /etc/passwd
```

For a shorter view:

```bash
head -20 /etc/passwd
```

Identify:

- Usernames.
- UIDs.
- GIDs.
- Home directories.
- Login shells.

Remember that `/etc/passwd` is generally readable by ordinary users. It does not normally contain actual password hashes.

#### Task 7: Inspect `/etc/shadow` Permissions

Run:

```bash
ls -l /etc/shadow
```

You should see that access is restricted.

Do not display or copy password hashes into your report.

The file contains sensitive authentication information.

#### Task 8: Inspect Group Information

Run:

```bash
cat /etc/group
```

For a shorter view:

```bash
head -20 /etc/group
```

Understand that groups help administrators assign permissions to multiple users.

### 8. User Account Management Commands

Linux provides commands for creating, inspecting, modifying, and removing user accounts.

| Command | Purpose |
|---|---|
| `useradd` | Creates a user |
| `adduser` | Interactive user creation on systems that provide it |
| `usermod` | Modifies a user |
| `userdel` | Deletes a user |
| `passwd` | Changes account passwords |
| `chage` | Manages password aging |
| `id` | Displays user and group identifiers |
| `getent passwd` | Retrieves account information |

Use these commands carefully because account changes can affect access to the system.

### 9. Practical Exercise – Create a Temporary Test User

For this exercise, create a dedicated account inside your Kali VM.

Do not use your normal account as the test account.

#### Step 1: Create a Test User

Run:

```bash
sudo useradd -m securitylab
```

This creates:

- A user named `securitylab`.
- A home directory at `/home/securitylab`.

#### Step 2: Assign a Password

Run:

```bash
sudo passwd securitylab
```

Choose a temporary lab password.

Do not reuse your personal or important passwords.

#### Step 3: Inspect the New User

Run:

```bash
id securitylab
```

Then:

```bash
getent passwd securitylab
```

#### Step 4: Inspect the Home Directory

Run:

```bash
ls -ld /home/securitylab
```

#### Step 5: Inspect Group Membership

Run:

```bash
groups securitylab
```

Record the results.

### 10. Practical Exercise – Modify a Test User

We will now investigate how administrators modify account properties.

#### Step 1: Create a Test Group

Run:

```bash
sudo groupadd securityteam
```

#### Step 2: Add the Test User to the Group

Run:

```bash
sudo usermod -aG securityteam securitylab
```

The `-aG` options append the user to supplementary groups without replacing existing supplementary group memberships.

#### Step 3: Verify the Membership

Run:

```bash
id securitylab
```

Then:

```bash
groups securitylab
```

You should see `securityteam` in the supplementary groups.

### 11. Understanding Account Locking

Linux administrators may temporarily lock an account when access must be suspended.

For your test account:

```bash
sudo passwd -l securitylab
```

This locks password authentication for the account.

Check its status:

```bash
sudo passwd -S securitylab
```

Unlock it:

```bash
sudo passwd -u securitylab
```

Check again:

```bash
sudo passwd -S securitylab
```

**Important:** Password locking is not a universal guarantee that all access methods are blocked. SSH keys, existing sessions, and other authentication mechanisms may require separate investigation.

For this lab, do not lock your normal Kali account.

### 12. Understanding Password Aging

Password aging allows administrators to configure password expiration and account aging policies.

Inspect the test account:

```bash
sudo chage -l securitylab
```

This displays information such as:

- Last password change.
- Password expiration.
- Minimum password age.
- Maximum password age.
- Account expiration.

To understand the command options:

```bash
chage --help
```

Do not apply aggressive expiration policies to your normal account during this exercise.

### 13. Understanding Account Deletion

When an account is no longer required, an administrator may remove it.

Two important commands are:

**Delete the account only:**

```bash
sudo userdel securitylab
```

**Delete the account and its home directory:**

```bash
sudo userdel -r securitylab
```

For this lab, use the second command only after you have finished inspecting the test account.

First verify the account name carefully:

```bash
id securitylab
```

Then remove it:

```bash
sudo userdel -r securitylab
```

Remove the test group:

```bash
sudo groupdel securityteam
```

Verify the account is gone:

```bash
getent passwd securitylab
```

No output normally means the account was not found.

### 14. Security Risks Associated with User Accounts

#### A. Weak Passwords

Weak passwords can be guessed or discovered through credential attacks.

#### B. Shared Accounts

Multiple people using the same account makes accountability difficult.

#### C. Excessive Privileges

Giving ordinary users administrative permissions unnecessarily increases risk.

#### D. Dormant Accounts

Unused accounts may remain active even when nobody needs them.

#### E. Poor Group Management

Incorrect group membership may expose sensitive resources.

#### F. Unrestricted Login Shells

Some service accounts may not need interactive login access.

Administrators should review whether login access is appropriate for each account.

### 15. Practical Exercise – Audit Local User Accounts

Run:

```bash
getent passwd
```

Inspect accounts with interactive shells:

```bash
awk -F: '$7 ~ /(bash|sh|zsh)$/ {print $1, $3, $6, $7}' /etc/passwd
```

Inspect accounts with UID 0:

```bash
awk -F: '$3 == 0 {print $1, $3}' /etc/passwd
```

Normally, the root account is the only account with UID 0. Any additional UID 0 account should be investigated.

Inspect users with home directories:

```bash
awk -F: '$6 ~ /^\/home\// {print $1, $6}' /etc/passwd
```

These are inspection commands. Do not delete accounts based only on their names or shells.

### 16. Generate a User Account Security Report

Create:

```bash
nano user-security-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module06/lab03/user-security-report.txt"

{
    echo "LINUX USER ACCOUNT SECURITY REPORT"
    echo "=================================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "CURRENT USER"
    whoami
    echo

    echo "USER IDENTIFIERS"
    id
    echo

    echo "CURRENT GROUPS"
    groups
    echo

    echo "USER ACCOUNT DATABASE"
    getent passwd
    echo

    echo "ACCOUNTS WITH UID ZERO"
    awk -F: '$3 == 0 {print $1, $3}' /etc/passwd
    echo

    echo "INTERACTIVE SHELL ACCOUNTS"
    awk -F: '$7 ~ /(bash|sh|zsh)$/ {print $1, $3, $6, $7}' /etc/passwd
    echo

    echo "HOME DIRECTORIES"
    awk -F: '$6 ~ /^\/home\// {print $1, $6}' /etc/passwd
    echo

    echo "GROUP INFORMATION"
    getent group
    echo

    echo "SHADOW FILE PERMISSIONS"
    ls -l /etc/shadow

} > "$REPORT" 2>&1

echo "User security report generated:"
echo "$REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x user-security-report.sh
```

Run:

```bash
./user-security-report.sh
```

Inspect:

```bash
less user-security-report.txt
```

Do not include password hashes or other authentication secrets in the report.

### 17. Knowledge Check

1. What is a Linux user account?
2. What is a UID?
3. What is a GID?
4. What is the root account?
5. Differentiate between regular and system accounts.
6. What is the purpose of `/etc/passwd`?
7. What is the purpose of `/etc/shadow`?
8. What is the purpose of `/etc/group`?
9. What does `useradd` do?
10. What does `usermod` do?
11. What does `passwd -l` do?
12. What is password aging?
13. Why should unused accounts be reviewed?
14. What is the principle of least privilege?
15. Why is it dangerous to have unnecessary UID 0 accounts?

### 18. Lab Documentation

Create:

```bash
nano lab03-report.txt
```

Record:

```text
MODULE 06 – LAB 03
USER ACCOUNT SECURITY

1. Current username:
2. Current UID:
3. Current GID:
4. Primary group:
5. Home directory:
6. Login shell:
7. Purpose of /etc/passwd:
8. Purpose of /etc/shadow:
9. Purpose of /etc/group:
10. Test user created successfully:
11. Test group created successfully:
12. Supplementary group membership verified:
13. Account locking tested:
14. Account deletion completed:
15. Number of UID 0 accounts observed:
16. Five user account security risks:
17. What I learned:

USER SECURITY REPORT:
Generated successfully: YES / NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 19. Completion Checklist

- [ ] Understood Linux user accounts.
- [ ] Learned UID and GID.
- [ ] Understood root, regular, and system accounts.
- [ ] Inspected `/etc/passwd`.
- [ ] Inspected `/etc/shadow` permissions.
- [ ] Inspected `/etc/group`.
- [ ] Created a temporary test user.
- [ ] Created a test group.
- [ ] Modified group membership.
- [ ] Tested account locking and unlocking.
- [ ] Inspected password aging.
- [ ] Removed the temporary account and group.
- [ ] Audited local accounts.
- [ ] Generated the user security report.
- [ ] Completed the lab documentation.

**LAB 03 STATUS: READY FOR PRACTICAL COMPLETION**
