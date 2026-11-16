
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 05: SUDO AND PRIVILEGE MANAGEMENT

### 1. Introduction

Privilege management is the process of controlling what users are allowed to do on a Linux system.

Linux separates ordinary user privileges from administrative privileges.

This separation helps prevent accidental system damage and unauthorized changes.

For example, an ordinary user may be allowed to create files inside their home directory but not modify protected system configuration files.

An administrator can perform those operations using elevated privileges.

### 2. Understanding Root Privileges

The root account is the Linux superuser.

It has extensive control over the operating system.

The root account has:

- UID 0.
- Administrative access to system files.
- Permission to manage users and groups.
- Permission to install and remove software.
- Permission to manage system services.
- Permission to modify system configurations.

Because root has powerful permissions, mistakes made as root can seriously damage a system.

### 3. What Is sudo?

`sudo` means "superuser do."

It allows an authorized user to execute a command with elevated privileges.

Example:

```bash
sudo apt update
```

This executes APT with administrative privileges.

Another example:

```bash
sudo systemctl status ssh
```

This requests administrative execution for the service inspection command.

The user must be authorized by the sudo configuration.

### 4. sudo vs su

| Command | Purpose |
|---|---|
| `sudo COMMAND` | Executes one command with elevated privileges |
| `sudo -i` | Starts an interactive root login shell |
| `su USER` | Switches to another user |
| `su -` | Starts a root login shell when authorized |
| `whoami` | Displays the current username |
| `id` | Displays user and group identifiers |

Prefer using `sudo` for individual administrative commands when appropriate.

Avoid remaining in a root shell unnecessarily.

### 5. Understanding the sudoers File

Linux uses sudoers configuration to determine which users can execute administrative commands.

Important locations include:

- `/etc/sudoers`
- `/etc/sudoers.d/`

The sudoers file defines:

- Which users may use sudo.
- Which commands they may execute.
- Which users or groups may execute commands.
- Whether password authentication is required.

**Important:** Never edit `/etc/sudoers` using a normal text editor. Use `visudo`, which checks the syntax before saving.

### 6. Inspect Your Current Privileges

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module06/lab05
cd ~/practical-labs/module06/lab05
```

#### Task 1: Identify Your User

```bash
whoami
```

#### Task 2: Inspect User Identity

```bash
id
```

#### Task 3: Inspect Group Membership

```bash
groups
```

#### Task 4: Inspect sudo Permissions

```bash
sudo -l
```

This displays the commands your account is permitted to execute through sudo.

Review the output.

#### Task 5: Inspect the sudo Version

```bash
sudo --version
```

### 7. Inspect sudoers Configuration Safely

Run:

```bash
sudo visudo -c
```

This checks the syntax of the sudoers configuration.

Expected successful result:

```text
/etc/sudoers: parsed OK
```

There may also be additional configuration files checked.

Inspect the sudoers directory:

```bash
sudo ls -la /etc/sudoers.d/
```

Inspect the main file's permissions:

```bash
ls -l /etc/sudoers
```

Do not modify the configuration during this introductory exercise.

### 8. Understanding the Principle of Least Privilege

The principle of least privilege states that users should receive only the permissions required to perform their responsibilities.

Example:

A university student account may need permission to:

- Read course materials.
- Write assignments.
- Run programming tools.

It does not normally need permission to:

- Change system security policies.
- Delete other users.
- Modify protected system files.
- Change firewall configurations.

Least privilege reduces the impact of compromised accounts and mistakes.

### 9. Understanding Administrative Groups

Linux systems may use groups to grant administrative access.

Examples include:

- `sudo`
- `adm`
- `systemd-journal`

The meaning and permissions of groups depend on system configuration.

Inspect your group membership:

```bash
groups
```

Inspect the sudo group:

```bash
getent group sudo
```

Inspect the administrator group configuration:

```bash
grep -E '^(sudo|wheel):' /etc/group
```

Do not add your account to administrative groups just for practice.

### 10. Practical Exercise – Compare Normal and Elevated Privileges

#### Step 1: Check Your Current User

```bash
whoami
```

#### Step 2: Inspect a Protected File

```bash
ls -l /etc/shadow
```

#### Step 3: Attempt a Read Without Elevated Privileges

```bash
cat /etc/shadow
```

The system should deny access to an ordinary user.

If you are already operating as root, this restriction will not apply. Record that fact.

Do not copy the contents of this file into your report.

#### Step 4: Inspect Permissions Using sudo

```bash
sudo ls -l /etc/shadow
```

This demonstrates how elevated privileges allow an authorized administrator to inspect protected file metadata.

You do not need to display the sensitive contents.

### 11. Understanding sudo Logging

Administrative commands may be recorded in system logs.

Inspect sudo-related journal events:

```bash
sudo journalctl -b --no-pager | grep -i sudo | tail -30
```

On systems that use authentication logs:

```bash
sudo grep -i sudo /var/log/auth.log | tail -30
```

The second command may not work if the file does not exist.

Logs can help administrators determine:

- Which user executed a privileged command.
- When the command was executed.
- Whether authorization succeeded.
- Which command was requested.

### 12. Understanding sudo Security Risks

Common sudo security problems include:

**A. Excessive Privileges**

Giving every user unrestricted administrative permissions increases risk.

**B. Unrestricted Command Execution**

A user may be able to execute more commands than their role requires.

**C. Poor sudoers Configuration**

Incorrect rules can grant unintended access.

**D. Shared Administrator Accounts**

Shared credentials make accountability difficult.

**E. Unnecessary Root Sessions**

Remaining in an elevated shell increases the impact of accidental commands.

### 13. Practical Exercise – Create a Privilege Audit Report

Create:

```bash
nano privilege-audit.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module06/lab05/privilege-audit-report.txt"

{
    echo "LINUX PRIVILEGE MANAGEMENT AUDIT"
    echo "================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "CURRENT USER"
    whoami
    echo

    echo "USER IDENTIFIERS"
    id
    echo

    echo "GROUP MEMBERSHIP"
    groups
    echo

    echo "SUDO VERSION"
    sudo --version | head -5
    echo

    echo "SUDO PERMISSIONS"
    sudo -l
    echo

    echo "SUDOERS SYNTAX VALIDATION"
    sudo visudo -c
    echo

    echo "SUDOERS FILE PERMISSIONS"
    ls -l /etc/sudoers
    echo

    echo "SUDOERS DIRECTORY"
    sudo ls -la /etc/sudoers.d/
    echo

    echo "SUDO GROUP"
    getent group sudo
    echo

    echo "RECENT SUDO JOURNAL EVENTS"
    sudo journalctl -b --no-pager |
        grep -i sudo |
        tail -30

} > "$REPORT" 2>&1

echo "Privilege audit generated:"
echo "$REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x privilege-audit.sh
```

Run:

```bash
./privilege-audit.sh
```

Inspect:

```bash
less privilege-audit-report.txt
```

Press `q` to exit.

### 14. Real-World Security Scenario

**Situation:**

A company discovers that an ordinary employee account can execute administrative commands that are unrelated to the employee's job.

The security administrator must investigate.

A responsible investigation should:

1. Identify the affected user.
2. Inspect group membership.
3. Review `sudo -l`.
4. Validate sudoers configuration.
5. Identify unnecessary permissions.
6. Recommend a least-privilege configuration.
7. Obtain approval before changing access.
8. Verify the final permissions.
9. Document the changes.

Never remove administrative access without considering whether it is required for legitimate system operations.

### 15. Knowledge Check

1. What is privilege management?
2. What is the root account?
3. What is sudo?
4. What is the difference between sudo and su?
5. What is the purpose of `/etc/sudoers`?
6. Why should `visudo` be used?
7. What does `sudo -l` display?
8. What is the principle of least privilege?
9. Why is unrestricted administrative access dangerous?
10. What is the purpose of the sudo group?
11. Why are sudo logs important?
12. What is privilege escalation?
13. Why should root sessions be minimized?
14. What is the purpose of `visudo -c`?
15. How does privilege management improve Linux security?

### 16. Lab Documentation

Create:

```bash
nano lab05-report.txt
```

Record:

```text
MODULE 06 – LAB 05
SUDO AND PRIVILEGE MANAGEMENT

1. Current username:
2. UID:
3. Primary group:
4. Supplementary groups:
5. Sudo version:
6. Does the account have sudo access?
7. What commands are permitted by sudo -l?
8. Was sudoers syntax validation successful?
9. Main sudoers file permissions:
10. Is the sudo group present?
11. Where can sudo events be logged?
12. What is least privilege?
13. Five sudo security risks:
14. What did I learn?

PRIVILEGE AUDIT:
Generated successfully: YES / NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 17. Completion Checklist

- [ ] Understood root privileges.
- [ ] Learned how sudo works.
- [ ] Compared sudo and su.
- [ ] Inspected current user privileges.
- [ ] Inspected sudo permissions.
- [ ] Validated sudoers syntax.
- [ ] Inspected sudoers permissions.
- [ ] Studied administrative groups.
- [ ] Compared normal and elevated access.
- [ ] Reviewed sudo logs.
- [ ] Studied privilege management risks.
- [ ] Generated the privilege audit report.
- [ ] Completed lab documentation.

**LAB 05 STATUS: READY FOR PRACTICAL COMPLETION**
