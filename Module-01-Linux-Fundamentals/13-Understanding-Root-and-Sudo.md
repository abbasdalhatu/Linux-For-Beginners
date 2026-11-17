---
---

# Module 01: Linux Fundamentals

# Lesson 13 – Understanding Root and Sudo

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 13 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

Linux is a multiuser operating system. This means multiple users can have accounts and access the same computer while having different permissions.

Not every user should have permission to perform every operation.

For example:

- An ordinary user may read personal files.
- A system administrator may install software.
- A privileged user may modify system configuration.
- The root account can perform almost any operation on the system.

Linux uses a permission and privilege system to control access to files, directories, processes, and administrative operations.

Two important concepts in Linux administration are:

- **Root:** The superuser account with User ID 0.
- **Sudo:** A command that allows authorised users to execute commands with elevated privileges.

Understanding root and sudo is essential for Linux system administration and ethical hacking.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux users and privileges.
2. Define the root account.
3. Understand User ID (UID) and Group ID (GID).
4. Explain the difference between root and ordinary users.
5. Understand the purpose of sudo.
6. Explain the difference between `sudo` and `su`.
7. Inspect user and group information.
8. Understand Linux file permissions.
9. Identify the risks of excessive privileges.
10. Apply safe privilege-management practices.

## 3. Understanding Linux Users

A Linux user account identifies a person or service that interacts with the operating system.

Each account has an identity and associated permissions.

Linux users can be divided into three broad categories.

### 3.1 Root User

The root user is the superuser account.

It has User ID 0 and possesses extensive privileges over the operating system.

The root account can perform administrative operations such as:

- Installing and removing software.
- Managing user accounts.
- Modifying system configuration.
- Changing file ownership.
- Managing system services.
- Changing permissions.
- Accessing many protected files.

### 3.2 Ordinary Users

Ordinary users have limited permissions.

They can normally:

- Access their personal files.
- Run permitted applications.
- Create and edit files they own.
- Use network applications.
- Execute commands allowed by their permissions.

They cannot automatically modify protected system files.

### 3.3 System Users

System accounts are commonly used by services and background processes.

For example, a service may run under a dedicated account rather than the root account.

This helps limit the damage that could occur if the service is compromised.

## 4. Understanding UID and GID

Linux identifies users and groups using numerical identifiers.

### UID – User ID

A UID is a numerical identifier assigned to a user account.

The root account always has UID 0.

### GID – Group ID

A GID is a numerical identifier assigned to a group.

Groups allow administrators to manage permissions for multiple users together.

### Example

Run:

```bash
id
```

Example output:

```text
uid=1000(kali) gid=1000(kali) groups=1000(kali),27(sudo)
```

Your actual output may differ.

Explanation:

| Field | Meaning |
|---|---|
| `uid=1000` | Numerical user identity |
| `gid=1000` | Primary group identity |
| `groups` | Groups associated with the user |
| `sudo` | A group that may grant administrative privileges when configured |

**Important:** Membership in a group named `sudo` does not independently guarantee access. The sudo policy determines which users and groups may execute privileged commands.

## 5. Understanding the Root Account

The root account is the most privileged standard account in Linux.

Its UID is always 0.

### Check the current user

```bash
whoami
```

If the result is:

```text
kali
```

You are operating as the `kali` user.

If the result is:

```text
root
```

You are operating as root.

### Check the UID

```bash
id -u
```

If the output is:

```text
0
```

The current process has UID 0.

### Check root account information

```bash
getent passwd root
```

Example output:

```text
root:x:0:0:root:/root:/bin/bash
```

Explanation:

| Field | Meaning |
|---|---|
| `root` | Username |
| `x` | Password information stored separately |
| `0` | UID |
| `0` | Primary GID |
| `root` | Account description |
| `/root` | Home directory |
| `/bin/bash` | Login shell |

The `x` does not represent the actual password. Password information is generally stored in protected files such as `/etc/shadow`.

## 6. Why Does Linux Have a Root Account?

Linux needs an administrative account capable of managing the entire system.

Without administrative privileges, ordinary users could not perform certain important tasks.

Examples include:

1. Installing system-wide packages.
2. Managing protected configuration files.
3. Creating and managing system users.
4. Starting and stopping privileged services.
5. Changing system-wide security settings.
6. Managing ownership and permissions.

However, root privileges must be controlled carefully because mistakes can affect the entire system.

## 7. Understanding Sudo

`sudo` stands for **Superuser Do**.

It allows an authorised user to execute a command with elevated privileges.

Rather than remaining root for every operation, users can request elevated privileges only when needed.

### Example

Display system information:

```bash
whoami
```

Install a package:

```bash
sudo apt install tree
```

The command requests administrative permission to install the package.

### How sudo works

1. A user enters a command beginning with `sudo`.
2. The system checks the sudo security policy.
3. If authorised, authentication may be required.
4. The command runs with the permitted elevated privileges.
5. The command finishes.
6. The user's ordinary shell remains active.

### Important distinction

Using `sudo` does not permanently turn your account into root.

It elevates the privileges of the specific command or command session.

## 8. Common Sudo Commands

### 8.1 Execute a Command as Root

```bash
sudo whoami
```

If authorised, the output should be:

```text
root
```

### 8.2 Update Package Information

```bash
sudo apt update
```

This refreshes the package lists from configured repositories.

### 8.3 Install Software

```bash
sudo apt install tree
```

### 8.4 Edit a Protected Configuration File

```bash
sudo nano /etc/hosts
```

This opens the hosts file with elevated privileges.

Do not make unnecessary changes to system configuration.

### 8.5 Display Sudo Privileges

```bash
sudo -l
```

This displays the commands your account is permitted to execute through sudo.

### 8.6 Display Sudo Version

```bash
sudo --version
```

## 9. Understanding the `su` Command

`su` means **Substitute User**.

It allows a user to switch to another account.

If no username is provided, it commonly attempts to switch to root.

Example:

```bash
su -
```

This requests a root login shell.

On many systems, you must provide the root account's password.

### Difference Between `su` and `su -`

The command:

```bash
su
```

Switches to another user while generally retaining much of the current environment.

The command:

```bash
su -
```

Starts a login shell and loads the target user's login environment.

### Switch to a named user

```bash
su - username
```

Replace `username` with an actual account name.

## 10. Difference Between Root, Sudo, and Su

| Feature | Root | Sudo | Su |
|---|---|---|---|
| Meaning | Superuser account | Superuser Do | Substitute User |
| Main purpose | Full system administration | Execute authorised privileged commands | Switch user accounts |
| Authentication | Root authentication, depending on configuration | Usually authenticates the invoking user | Usually authenticates the target user |
| Privileges | Extensive UID 0 privileges | Depends on sudo policy | Depends on the account switched to |
| Typical use | System administration | Individual administrative commands | Switching accounts |

### Example

Ordinary user:

```bash
whoami
```

Output:

```text
kali
```

Elevated command:

```bash
sudo whoami
```

Output:

```text
root
```

Return to the normal shell automatically when the command finishes.

## 11. Understanding Linux File Permissions

Linux uses permissions to control who can read, modify, and execute files.

There are three main permission categories:

1. Owner.
2. Group.
3. Others.

There are also three basic permissions:

| Permission | Symbol | Meaning |
|---|---|---|
| Read | `r` | Read file contents or list directory entries |
| Write | `w` | Modify a file or directory entries |
| Execute | `x` | Execute a file or access a directory |

### Example

Run:

```bash
ls -l
```

Example output:

```text
-rwxr-xr-- 1 kali kali 1200 Nov 14 script.sh
```

The permission section is:

```text
-rwxr-xr--
```

Breakdown:

| Part | Meaning |
|---|---|
| `-` | Regular file |
| `rwx` | Owner permissions |
| `r-x` | Group permissions |
| `r--` | Others permissions |

The owner can read, write, and execute the file.

Group members can read and execute it.

Other users can only read it.

## 12. Understanding Permission Numbers

Linux permissions can also be represented using numbers.

| Permission | Numeric Value |
|---|---:|
| Read (`r`) | 4 |
| Write (`w`) | 2 |
| Execute (`x`) | 1 |

The values are added together.

Examples:

- Read only: 4
- Write only: 2
- Execute only: 1
- Read and write: 6
- Read and execute: 5
- Read, write, and execute: 7

### Example: 755

```bash
chmod 755 script.sh
```

The permission values are:

| Category | Value | Permission |
|---|---:|---|
| Owner | 7 | Read, write, execute |
| Group | 5 | Read, execute |
| Others | 5 | Read, execute |

### Example: 644

```bash
chmod 644 notes.txt
```

| Category | Value | Permission |
|---|---:|---|
| Owner | 6 | Read, write |
| Group | 4 | Read |
| Others | 4 | Read |

Avoid granting permissions more broadly than necessary.

## 13. Managing File Ownership

Linux associates files and directories with owners and groups.

Use `ls -l` to inspect ownership.

Example:

```text
-rw-r--r-- 1 kali kali 100 notes.txt
```

The first `kali` is the owner.

The second `kali` is the group.

### Change file ownership

The `chown` command changes ownership.

Example:

```bash
sudo chown username:groupname notes.txt
```

Replace the placeholders with valid user and group names.

### Change group ownership

```bash
sudo chgrp groupname notes.txt
```

These commands require appropriate permissions.

Only change ownership when you understand why it is necessary.

## 14. The Principle of Least Privilege

The Principle of Least Privilege is an important cybersecurity concept.

It means that users, applications, and services should receive only the permissions required to perform their tasks.

### Example

Suppose an ordinary user needs to view a file.

They should not need full root access just to read it.

Similarly, a web server should not automatically run as root when a less privileged service account is sufficient.

### Benefits

- Reduces accidental system damage.
- Limits the impact of compromised accounts.
- Protects sensitive information.
- Reduces opportunities for privilege abuse.
- Improves system security.

## 15. Risks of Running Commands as Root

Root access is powerful, but careless use can damage the system.

### Risk 1: Accidental File Deletion

A command executed as root may remove important system files.

### Risk 2: Incorrect Permissions

Changing permissions on system directories can break applications or weaken security.

### Risk 3: Malware Impact

Malicious software running with root privileges may gain extensive control over the system.

### Risk 4: Configuration Damage

Incorrect modifications to protected configuration files may prevent services or the operating system from functioning correctly.

### Safe practices

1. Use an ordinary account for daily activities.
2. Use sudo only when required.
3. Read commands before executing them.
4. Avoid copying unknown commands into a root shell.
5. Verify file paths before deletion.
6. Keep the system updated.
7. Review sudo permissions regularly.

## 16. Understanding Privilege Escalation

Privilege escalation is a security concept in which a user or process obtains permissions beyond those originally assigned.

There are two broad categories.

### 16.1 Vertical Privilege Escalation

This occurs when an account or process obtains a higher level of privilege.

Example:

An ordinary user obtains root-level access.

### 16.2 Horizontal Privilege Escalation

This occurs when a user accesses another user's resources or account at a similar privilege level without authorisation.

Example:

A normal user gains unauthorised access to another ordinary user's private records.

### Why security professionals study privilege escalation

Security professionals assess privilege boundaries to identify weaknesses such as:

- Incorrect file permissions.
- Misconfigured administrative policies.
- Unnecessary privileged services.
- Exposed credentials.
- Vulnerable software.

These weaknesses should be investigated only on systems that are owned or explicitly authorised for testing.

## 17. Practical Lab: Root and Sudo in Kali Linux

**Lab objective:** Understand user identities, inspect sudo privileges, practise safe administrative commands, and examine file permissions.

Perform these exercises inside your Kali Linux virtual machine.

### Part A – Identify your current user

Run:

```bash
whoami
```

Then:

```bash
id
```

Then:

```bash
id -u
```

Record the results.

### Part B – Inspect the root account

Run:

```bash
getent passwd root
```

Then:

```bash
id root
```

Observe the root UID.

### Part C – Check sudo availability

Run:

```bash
sudo --version
```

Then:

```bash
sudo -l
```

Observe the permissions available to your account.

### Part D – Execute a command with sudo

Run:

```bash
sudo whoami
```

If authorised, the output should be:

```text
root
```

Now run:

```bash
whoami
```

Notice that your normal shell remains associated with your ordinary user.

### Part E – Inspect user groups

Run:

```bash
groups
```

Then:

```bash
getent group sudo
```

Observe the membership information.

### Part F – Practise file permissions

Create a practice directory:

```bash
mkdir -p ~/Privilege-Lab
```

Enter it:

```bash
cd ~/Privilege-Lab
```

Create a file:

```bash
touch permissions.txt
```

Inspect its permissions:

```bash
ls -l permissions.txt
```

Set permissions to 644:

```bash
chmod 644 permissions.txt
```

Inspect again:

```bash
ls -l permissions.txt
```

Set permissions to 600:

```bash
chmod 600 permissions.txt
```

Inspect again:

```bash
ls -l permissions.txt
```

Compare the permission results.

### Part G – Practise ownership inspection

Run:

```bash
ls -ln permissions.txt
```

This displays numerical UID and GID values instead of account names.

Then run:

```bash
stat permissions.txt
```

Observe the file's metadata, ownership, and permissions.

### Part H – Understand the sudo policy

Run:

```bash
sudo visudo -c
```

This checks the sudoers configuration for syntax errors.

Do not edit the sudoers configuration during this exercise.

### Part I – Clean up

Return to your home directory:

```bash
cd ~
```

Remove the practice directory:

```bash
rm -r Privilege-Lab
```

Verify that it has been removed:

```bash
ls -d ~/Privilege-Lab
```

The final command should report that the directory does not exist.

## 18. Important Terms

| Term | Meaning |
|---|---|
| Root | Linux superuser account |
| UID | User ID |
| GID | Group ID |
| Sudo | Command for executing authorised privileged operations |
| Su | Command for switching user accounts |
| Privilege | Permission or authority to perform an operation |
| Permission | Rule controlling access to a resource |
| Owner | User associated with a file |
| Group | Collection of users sharing permissions |
| Chmod | Command for changing file permissions |
| Chown | Command for changing ownership |
| Chgrp | Command for changing group ownership |
| Least Privilege | Giving users only the access they require |
| Privilege Escalation | Obtaining higher or otherwise unauthorised privileges |

## 19. Knowledge Check

Answer these questions without looking at the lesson.

1. What is the root account?
2. What is the UID of root?
3. What is the difference between ordinary users and root?
4. What does UID mean?
5. What does GID mean?
6. What is the purpose of sudo?
7. What does the `su` command do?
8. Explain the difference between `sudo` and `su`.
9. What does `sudo -l` display?
10. What are the three main Linux permission categories?
11. Explain read, write, and execute permissions.
12. What does permission number 755 represent?
13. What is the purpose of `chmod`?
14. Explain the Principle of Least Privilege.
15. What is privilege escalation?
16. Explain the difference between vertical and horizontal privilege escalation.

## 20. Practical Assignment

Complete the following tasks:

1. Define the root account.
2. Explain the difference between root and an ordinary user.
3. Explain UID and GID with examples.
4. Define sudo and explain how it works.
5. Compare `sudo`, `su`, and `su -`.
6. Explain the purpose of the `id` command.
7. Explain Linux read, write, and execute permissions.
8. Convert permission number 755 into symbolic permissions.
9. Explain the Principle of Least Privilege.
10. Define privilege escalation and explain its two main categories.
11. Explain four risks associated with careless root access.
12. Describe five safe practices for using administrative privileges.

Save your assignment as:

`Root-and-Sudo-Assignment.md`

## 21. Exam-Ready Summary

Linux uses user accounts and permissions to control access to system resources. The root account is the superuser and has UID 0. Ordinary users operate with limited permissions, while system accounts are commonly used by background services. The `sudo` command allows authorised users to execute commands with elevated privileges, while `su` switches to another user account. Linux file permissions include read, write, and execute permissions for owners, groups, and others. Commands such as `chmod`, `chown`, and `chgrp` help manage permissions and ownership. The Principle of Least Privilege requires users and services to receive only the permissions they need. Privilege escalation occurs when an account or process obtains additional privileges beyond those originally assigned.

## 22. Lesson Conclusion

Understanding root and sudo is essential for anyone working with Linux.

You have learned how Linux identifies users, controls permissions, manages administrative privileges, and protects system resources.

You have also learned why excessive privileges can create security risks and why the Principle of Least Privilege is important.

As an ethical hacker, you must understand both how permissions protect a system and how misconfigurations can weaken those protections.

**Next Lesson:** Lesson 14 – Linux Environment Overview.