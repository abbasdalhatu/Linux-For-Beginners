
# PRACTICAL LABS
# MODULE 02 – FILESYSTEM AND PERMISSION LABS

## LAB 05: OWNERSHIP AND GROUPS

### 1. Introduction

Linux uses users and groups to control access to files and directories.

Every file and directory has an owner and an associated group.

Ownership works together with permissions to determine who can access or modify an object.

For example, a file may belong to:

```text
Owner: kali
Group: kali
File: report.txt
```

The owner and group help Linux determine which permission category applies to a user.

This is especially important when managing multi-user servers, shared project directories, and sensitive system files.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Understand Linux users and groups.
2. Identify file ownership.
3. Inspect user and group IDs.
4. Use `chown` to change ownership.
5. Use `chgrp` to change group ownership.
6. Understand primary and supplementary groups.
7. Inspect group membership.
8. Audit ownership in a directory.

### 3. Understanding Linux Users

Every Linux user has a user account.

Each account is associated with a numerical identifier called a UID (User ID).

Inspect your current account:

```bash
whoami
id
```

Example output:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali)
```

Your actual values may differ.

The output identifies:

- `uid`: Your user ID.
- `gid`: Your primary group ID.
- `groups`: Groups associated with your account.

Display the current username:

```bash
whoami
```

Display detailed account information:

```bash
id
```

Inspect the account database:

```bash
getent passwd
```

Display your account entry:

```bash
getent passwd "$(whoami)"
```

### 4. Understanding Linux Groups

A group is a collection of user accounts.

Groups make it easier to manage access for multiple users.

For example:

A university server may have groups such as:

```text
students
lecturers
administrators
developers
```

Instead of assigning permissions to every individual user, an administrator can assign permissions to a group.

Inspect your groups:

```bash
groups
```

Display group information:

```bash
getent group
```

Display your primary group:

```bash
id -gn
```

Display all group names associated with your account:

```bash
id -nG
```

### 5. Understanding Primary and Supplementary Groups

Linux users can belong to multiple groups.

**Primary group**

The default group associated with the user's account.

New files normally inherit this group, subject to directory setgid behavior and other filesystem rules.

Inspect it:

```bash
id -gn
```

**Supplementary groups**

Additional groups that provide extra access.

Inspect them:

```bash
id -nG
```

A user can belong to several groups simultaneously.

### 6. Understanding File Ownership

Run:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 kali kali 120 report.txt
```

The important fields are:

```text
-rw-r--r-- 1 kali kali 120 report.txt
                 |    |
                 |    └── Group owner
                 └─────── User owner
```

Linux stores ownership information as numerical IDs, while commands commonly display account names.

Inspect a file:

```bash
stat report.txt
```

Look for:

```text
Uid:
Gid:
```

### 7. Create Your Practical Workspace

Run:

```bash
mkdir -p ~/practical-labs/module02/lab05
cd ~/practical-labs/module02/lab05
```

Create sample files:

```bash
touch project.txt
touch confidential.txt
mkdir team-project
```

Add sample information:

```bash
echo "Linux ownership practice" > project.txt
echo "Confidential practice information" > confidential.txt
```

Inspect ownership:

```bash
ls -l
stat project.txt
stat confidential.txt
```

### 8. Understanding the `chown` Command

`chown` means **change owner**.

It changes the user owner of a file or directory and can also change its group.

General syntax:

```bash
chown [OPTIONS] USER[:GROUP] FILE
```

Example:

```bash
sudo chown student project.txt
```

This changes the owner to `student`, if that account exists.

You can also specify a group:

```bash
sudo chown student:developers project.txt
```

This sets:

- Owner: `student`
- Group: `developers`

These are examples. Do not execute them unless the accounts and groups exist and you are authorized to change the object.

### 9. Understanding the `chgrp` Command

`chgrp` means **change group**.

It changes the group ownership of a file or directory.

General syntax:

```bash
chgrp GROUP FILE
```

Example:

```bash
sudo chgrp developers project.txt
```

This changes the group owner to `developers`, provided the group exists and you have the required privileges.

### 10. Inspect Available Users and Groups

Before changing ownership, inspect available accounts.

List users:

```bash
getent passwd
```

List groups:

```bash
getent group
```

Search for a particular group:

```bash
getent group sudo
```

Check whether a particular user exists:

```bash
getent passwd kali
```

Replace `kali` with an actual username if necessary.

### 11. Practical Exercise: Create a Test Group

For this exercise, use a group dedicated to your laboratory.

First check whether the group already exists:

```bash
getent group labteam
```

If the command returns no result, create it:

```bash
sudo groupadd labteam
```

Verify:

```bash
getent group labteam
```

If the group already exists, do not create it again.

### 12. Practical Exercise: Create a Test User

Check whether the user already exists:

```bash
getent passwd labuser
```

If no result appears, create the user:

```bash
sudo useradd -m -s /bin/bash labuser
```

Set a password interactively:

```bash
sudo passwd labuser
```

Use a unique practice password. Do not reuse an important personal password.

Verify the account:

```bash
id labuser
```

Inspect its home directory:

```bash
ls -ld /home/labuser
```

### 13. Add a User to a Group

Add the test account to your laboratory group:

```bash
sudo usermod -aG labteam labuser
```

Important:

- `usermod` modifies a user account.
- `-a` means append.
- `-G` specifies supplementary groups.

The `-a` option matters. Using `-G` without `-a` can replace the user's existing supplementary group list.

Verify:

```bash
id labuser
groups labuser
```

### 14. Change Ownership of Your Practice Files

Assign ownership of `project.txt` to the test user:

```bash
sudo chown labuser project.txt
```

Assign both owner and group:

```bash
sudo chown labuser:labteam confidential.txt
```

Assign group ownership of the directory:

```bash
sudo chgrp labteam team-project
```

Verify:

```bash
ls -l
ls -ld team-project
```

Inspect metadata:

```bash
stat project.txt
stat confidential.txt
```

### 15. Understanding Recursive Ownership Changes

The `-R` option applies ownership changes recursively.

Example:

```bash
sudo chown -R labuser:labteam team-project
```

This changes the ownership of the directory and its contents.

Inspect:

```bash
ls -la team-project
```

Use recursive ownership changes carefully. An incorrect path can affect many files.

For this lab, only use the dedicated `team-project` directory.

### 16. Understanding Shared Group Directories

A shared directory allows members of a group to collaborate.

For example:

```text
team-project/
```

Set group ownership:

```bash
sudo chown root:labteam team-project
```

Set appropriate directory permissions:

```bash
sudo chmod 2770 team-project
```

The leading `2` enables the setgid permission on the directory.

This helps new entries inherit the directory's group ownership.

We will study special permissions in more detail in a later lab.

Inspect:

```bash
ls -ld team-project
```

### 17. Security Connection: Why Ownership Matters

Incorrect ownership can create security problems.

Examples:

- A sensitive file owned by an unintended user may expose information.
- An application directory writable by an unauthorized group may allow tampering.
- An incorrectly configured shared directory may expose other users' files.
- Unexpected ownership changes may indicate a configuration error or suspicious activity.

During a security audit, inspect:

```bash
ls -lah
```

For a directory tree:

```bash
find ~/practical-labs/module02/lab05 -printf '%u %g %m %p\n'
```

This displays:

- User owner
- Group owner
- Permission mode
- Path

Do not assume that unusual ownership automatically means malicious activity. Investigate the context.

### 18. Practical Ownership Investigation

Inspect important account files:

```bash
ls -l /etc/passwd
ls -l /etc/group
ls -l /etc/shadow
```

You should observe that `/etc/shadow` has more restrictive access than ordinary account files.

Do not attempt to read or modify protected credentials.

Inspect ownership of important system directories:

```bash
ls -ld /etc /var/log /root /home
```

### 19. Generate an Ownership Audit Report

Run:

```bash
cd ~/practical-labs/module02/lab05

{
    echo "LINUX OWNERSHIP AND GROUP AUDIT"
    echo "==============================="
    echo "Date: $(date)"
    echo "Current User: $(whoami)"
    echo
    echo "CURRENT USER IDENTIFIERS:"
    id
    echo
    echo "CURRENT GROUPS:"
    groups
    echo
    echo "LAB FILE OWNERSHIP:"
    ls -lah
    echo
    echo "DETAILED OWNERSHIP:"
    find . -printf '%u %g %m %p\n'
    echo
    echo "TEST USER:"
    id labuser 2>&1
    echo
    echo "TEST GROUP:"
    getent group labteam
} > ownership-audit-report.txt
```

Read the report:

```bash
cat ownership-audit-report.txt
```

### 20. Lab Cleanup

After collecting your report, you can remove the temporary test account and group.

First inspect whether the account and group are used by other practice work. Only remove the dedicated objects created for this lab.

Remove the test user and its home directory:

```bash
sudo userdel -r labuser
```

Remove the test group:

```bash
sudo groupdel labteam
```

If a group is still used by another account or process, resolve that dependency before removal.

Your report should remain in the lab directory.

### 21. Knowledge Check

1. What is a UID?
2. What is a GID?
3. What is the difference between a user and a group?
4. What does `chown` do?
5. What does `chgrp` do?
6. What is a primary group?
7. What are supplementary groups?
8. Why is `usermod -aG` useful?
9. What does `chown -R` do?
10. Why is file ownership important in cybersecurity?

### 22. Lab Completion Checklist

- [ ] Inspected the current user using `whoami`.
- [ ] Inspected user IDs using `id`.
- [ ] Inspected groups using `groups`.
- [ ] Created the Lab 05 workspace.
- [ ] Created sample files and directories.
- [ ] Inspected file ownership.
- [ ] Created a dedicated test group if needed.
- [ ] Created a dedicated test user if needed.
- [ ] Added the test user to the test group.
- [ ] Practised `chown`.
- [ ] Practised `chgrp`.
- [ ] Practised recursive ownership changes.
- [ ] Configured a shared group directory.
- [ ] Generated `ownership-audit-report.txt`.
- [ ] Cleaned up temporary accounts and groups safely.

### 23. Expected Workspace

```text
~/practical-labs/module02/lab05/
├── project.txt
├── confidential.txt
├── team-project/
└── ownership-audit-report.txt
```

**Lab 05 Status:** Complete when the practical exercises and checklist are finished.

**Next Lab:** Lab 06 – ACLs and Advanced Permissions.
