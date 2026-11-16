
# Module 13 – Lesson 05: Sudo and Privilege Management

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux privilege management.
2. Understand root privileges.
3. Explain the purpose of `sudo`.
4. Differentiate between `sudo` and `su`.
5. Understand the sudoers configuration.
6. Inspect administrative privileges.
7. Understand privilege escalation.
8. Apply the Principle of Least Privilege.
9. Identify common sudo security risks.
10. Perform a basic sudo security audit.

---

## 2. Introduction to Linux Privilege Management

Privilege management refers to controlling what users and processes are allowed to do on a Linux system.

Not every user should have permission to perform administrative operations.

For example:

- A regular user can create files in their home directory.
- An administrator can install system packages.
- Root can modify almost any part of the operating system.

Linux uses privileges to separate ordinary activities from administrative activities.

### Simple Analogy

Imagine a university.

A student can:

- Attend classes.
- Submit assignments.
- Access learning materials.

However, a student cannot automatically:

- Change university examination records.
- Modify the university database.
- Create administrator accounts.

Those activities require special authority.

Linux works similarly.

---

## 3. Understanding the Root User

The root account is the most privileged account in a traditional Linux system.

Its numerical user ID is:

```text
0
```

Root can perform operations such as:

- Installing and removing software.
- Creating and deleting accounts.
- Modifying protected files.
- Managing system services.
- Configuring network settings.
- Changing file ownership.
- Managing system security.

### Inspect the Root Account

```bash
getent passwd root
```

Example:

```text
root:x:0:0:root:/root:/bin/bash
```

The UID value `0` identifies the root account.

### Inspect Your Current Identity

```bash
whoami
```

```bash
id
```

These commands show which account is executing your commands.

### Why Root Access Is Dangerous

A command executed with root privileges can affect the entire operating system.

For example:

```bash
sudo rm -rf /some/path
```

Depending on the target and system configuration, a destructive command can delete important data.

Always inspect commands and their targets before using administrative privileges.

---

## 4. What Is sudo?

`sudo` stands for **superuser do**.

It allows authorised users to execute commands with elevated privileges according to the system's sudo policy.

### Example

Updating package information:

```bash
sudo apt update
```

The command runs with the privileges granted by the sudo configuration.

### Another Example

Inspecting a protected system file:

```bash
sudo less /etc/shadow
```

Only use privileged access when there is a legitimate reason.

### How sudo Works

A simplified process:

1. A user requests permission to run a command.
2. Sudo checks the system's authorisation rules.
3. The user may be asked to authenticate.
4. If authorised, the command executes with the permitted privileges.
5. The activity may be recorded in system logs.

The exact behaviour depends on the system configuration.

---

## 5. Understanding sudo Privileges

Not every Linux user can automatically use sudo.

A system administrator controls which users or groups may use it.

On Debian-based systems, membership in the `sudo` group commonly grants sudo privileges through the default sudoers configuration.

### Inspect Group Membership

```bash
groups
```

### Inspect the Sudo Group

```bash
getent group sudo
```

### Inspect Your Sudo Permissions

```bash
sudo -l
```

This displays the commands your current account is authorised to execute through sudo.

### Validate Your Sudo Authentication

```bash
sudo -v
```

This checks or refreshes your sudo authentication credentials.

It does not execute an administrative command.

---

## 6. Difference Between sudo and su

Both commands can be used to obtain elevated privileges, but they operate differently.

### sudo

Runs a permitted command with elevated privileges.

Example:

```bash
sudo apt update
```

### su

The `su` command switches to another user account.

Example:

```bash
su username
```

To request a root login shell:

```bash
su -
```

This normally requires the target account's password.

### Comparison

| Feature | sudo | su |
|---|---|---|
| Main purpose | Execute authorised commands with elevated privileges | Switch user identity |
| Authentication | Usually the invoking user's password | Usually the target user's password |
| Command execution | Can execute a specific command | Can start a shell or switch identity |
| Access control | Sudoers policy | Target account authentication |
| Security advantage | Supports granular privilege control | Provides a different user session |

**Security recommendation:** Prefer narrowly authorised sudo commands over unnecessary root shells.

---

## 7. The sudoers Configuration File

Linux uses the sudoers configuration to define who can execute privileged commands.

The main configuration file is:

```text
/etc/sudoers
```

Additional configuration may be stored in:

```text
/etc/sudoers.d/
```

### Why sudoers Is Important

It controls:

- Which users can use sudo.
- Which groups can use sudo.
- Which commands are permitted.
- Whether authentication is required.
- Other privilege-related settings.

### Inspect the Configuration

Use:

```bash
sudo visudo -c
```

This checks the syntax of the sudoers configuration.

To inspect the main file:

```bash
sudo less /etc/sudoers
```

Do not edit the file directly using an ordinary text editor.

If an administrator needs to modify sudoers, `visudo` should be used because it performs syntax validation.

### Example Sudoers Rule

A simplified example is:

```text
%sudo ALL=(ALL:ALL) ALL
```

Explanation:

| Component | Meaning |
|---|---|
| `%sudo` | Members of the sudo group |
| `ALL` | Applies to all hosts in the rule's scope |
| `(ALL:ALL)` | Permitted run-as users and groups |
| `ALL` | All commands |

The actual configuration may contain additional rules and restrictions.

---

## 8. Privilege Escalation

### Definition

Privilege escalation occurs when a user or process gains permissions beyond those originally assigned.

There are two main categories.

### 8.1 Vertical Privilege Escalation

A lower-privileged user obtains higher privileges.

Example:

A normal user obtains unauthorised root access.

### 8.2 Horizontal Privilege Escalation

A user gains access to another account or resources at a similar privilege level.

Example:

Student A gains unauthorised access to Student B's private files.

### Common Causes

Privilege escalation can result from:

- Misconfigured sudo permissions.
- Vulnerable software.
- Incorrect file permissions.
- Insecure SUID programs.
- Exposed credentials.
- Weak access controls.

### Prevention

Administrators should:

1. Restrict sudo access.
2. Apply security updates.
3. Review file permissions.
4. Audit SUID and SGID files.
5. Protect authentication credentials.
6. Monitor administrative activity.
7. Follow the Principle of Least Privilege.

---

## 9. Understanding SUID and Privileged Programs

SUID is a special file permission that can allow an executable to run with the effective user identity of its owner.

Some legitimate system programs use this mechanism.

However, incorrectly configured SUID programs can create security risks.

### Inspect SUID Files

A read-only inspection command is:

```bash
find /usr/bin -perm -4000 -type f -ls 2>/dev/null
```

This lists files under `/usr/bin` that have the SUID permission bit.

### Inspect SGID Files

```bash
find /usr/bin -perm -2000 -type f -ls 2>/dev/null
```

Not every SUID or SGID file is dangerous.

An administrator should investigate:

- File ownership.
- File location.
- Whether the file belongs to a legitimate package.
- Whether the permission is required.
- Whether the package is up to date.

Do not remove special permissions indiscriminately.

---

## 10. Common sudo Security Risks

### Risk 1: Excessive Administrative Access

Giving too many users sudo privileges increases the potential impact of compromised accounts.

### Risk 2: Unrestricted Command Permissions

Allowing users to execute unrestricted administrative commands can effectively give them complete system control.

### Risk 3: Shared Administrator Accounts

Shared accounts make it difficult to determine who performed an action.

### Risk 4: Poor Credential Protection

If an attacker obtains an administrator's password, they may gain access to privileged operations.

### Risk 5: Incorrect sudoers Configuration

Incorrect rules can accidentally grant excessive privileges or prevent administrators from accessing necessary commands.

### Security Recommendations

- Use individual accounts.
- Restrict administrative group membership.
- Use strong authentication.
- Review sudoers rules.
- Monitor privileged activity.
- Avoid unnecessary root shells.
- Keep software updated.

---

# 11. Practical Lab: Sudo and Privilege Security Audit

## Lab Objective

Inspect your Kali Linux administrative privileges and identify important privilege-related security configurations.

You will perform inspection only.

Do not modify sudoers, add users to administrative groups, or change account privileges.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson05
cd ~/module13-lab/lesson05
```

### Step 2: Identify Your Current User

```bash
whoami
```

### Step 3: Inspect User and Group Information

```bash
id
```

### Step 4: Inspect Sudo Group Membership

```bash
getent group sudo
```

### Step 5: Inspect Your Permitted Sudo Commands

```bash
sudo -l
```

Study the output.

Identify whether your account has unrestricted or restricted administrative permissions.

### Step 6: Validate Sudo Authentication

```bash
sudo -v
```

### Step 7: Validate the Sudoers Configuration

```bash
sudo visudo -c
```

A successful syntax check should report that the configuration parsed successfully.

### Step 8: Inspect SUID Files

```bash
find /usr/bin -perm -4000 -type f -ls 2>/dev/null
```

Record a few examples.

Do not remove or change their permissions.

### Step 9: Inspect Recent Sudo-Related Logs

Try:

```bash
sudo journalctl -b --no-pager | grep -i sudo | tail -n 20
```

Log availability depends on your system configuration.

### Step 10: Generate Your Security Audit Report

Run:

```bash
{
    echo "LINUX SUDO AND PRIVILEGE SECURITY AUDIT"
    echo "======================================="
    echo
    echo "CURRENT USER"
    whoami
    echo
    echo "USER IDENTIFICATION"
    id
    echo
    echo "SUDO GROUP"
    getent group sudo
    echo
    echo "AUTHORISED SUDO COMMANDS"
    sudo -l
    echo
    echo "SUDOERS CONFIGURATION VALIDATION"
    sudo visudo -c
    echo
    echo "SUID EXECUTABLES UNDER /usr/bin"
    find /usr/bin -perm -4000 -type f -ls 2>/dev/null
    echo
    echo "RECENT SUDO LOGS"
    sudo journalctl -b --no-pager | grep -i sudo | tail -n 20
} > privilege-security-report.txt 2>&1
```

Inspect your report:

```bash
cat privilege-security-report.txt
```

### Expected Files

```text
lesson05/
└── privilege-security-report.txt
```

---

## 12. Real-World Cybersecurity Application

Imagine you are conducting a security audit for a university Linux server.

You discover that several student accounts belong to the sudo group.

This may be a security concern.

Your investigation should establish:

1. Which accounts have administrative access?
2. Why were those privileges granted?
3. Are the privileges still necessary?
4. Are the users authorised?
5. Are administrative actions being logged?
6. Are any sudo rules unnecessarily broad?

You should document your findings and recommend appropriate changes.

Do not remove privileges without confirming the operational requirements and obtaining authorisation.

---

## 13. Knowledge Check

Answer these questions in your notebook.

1. What is privilege management?
2. What is the root account?
3. What does sudo mean?
4. How does sudo work?
5. What is the difference between sudo and su?
6. What is the purpose of `/etc/sudoers`?
7. Why should administrators use `visudo`?
8. What is privilege escalation?
9. Differentiate between vertical and horizontal privilege escalation.
10. What is SUID?
11. Why can excessive sudo permissions be dangerous?
12. Why are individual administrator accounts important?
13. What does `sudo -l` display?
14. What is the purpose of `visudo -c`?
15. Explain how the Principle of Least Privilege applies to sudo access.

---

## 14. Practical Completion Checklist

- [ ] I understand Linux privilege management.
- [ ] I understand root privileges.
- [ ] I understand sudo.
- [ ] I understand the difference between sudo and su.
- [ ] I understand the sudoers configuration.
- [ ] I can inspect my sudo privileges.
- [ ] I understand privilege escalation.
- [ ] I understand SUID and SGID risks.
- [ ] I understand common sudo security weaknesses.
- [ ] I completed the privilege security audit.
- [ ] I generated my privilege security report.

**Lesson 05 Complete: Sudo and Privilege Management.**
