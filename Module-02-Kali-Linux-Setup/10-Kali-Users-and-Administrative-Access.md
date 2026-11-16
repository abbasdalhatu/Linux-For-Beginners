# LESSON 10: KALI USERS AND ADMINISTRATIVE ACCESS

**Module:** 02 – Kali Linux Setup  
**File:** `10-Kali-Users-and-Administrative-Access.md`

## 1. Introduction

Linux is a multi-user operating system.

This means multiple users can have accounts on the same computer, each with different permissions and levels of access.

Linux uses users, groups, and permissions to control who can access files, execute commands, and perform administrative operations.

In cybersecurity, understanding user accounts is important because attackers sometimes attempt to gain unauthorized access or escalate privileges.

In this lesson, we will learn how Linux manages users and administrative access.

## 2. Understanding Linux Users

A user is an account that allows someone or a system service to interact with Linux.

Every Linux user has a unique User ID (UID).

Linux commonly has three types of accounts:

| User type | Description |
|---|---|
| Root user | Has full administrative privileges |
| Normal user | Performs everyday activities |
| System user | Runs system services and background processes |

### Example

Our Kali installation has a normal account, such as:

`kaliuser`

This account allows us to use Kali without constantly operating with full administrative privileges.

## 3. Understanding the Root User

The root user is the superuser in Linux.

Root has extensive privileges over the operating system.

The root account has UID:

`0`

Root can:

- Install and remove software.
- Modify system configuration.
- Create and delete user accounts.
- Change file ownership and permissions.
- Manage system services.
- Access protected system files.

To check the root account information, run:

```bash
id root
```

You should see UID 0 in the output.

**Security warning:** Running everything as root is dangerous. A mistake or malicious command executed with root privileges can damage the entire system.

## 4. Understanding Sudo

`sudo` means "superuser do."

It allows an authorized user to execute a command with elevated privileges.

Example:

```bash
sudo apt update
```

This runs the package update command with administrative privileges.

To check whether your current account can use sudo, run:

```bash
sudo -l
```

This displays the commands your account is permitted to execute through sudo.

Depending on your configuration, it may request your password.

## 5. Understanding User IDs and Group IDs

Linux identifies users and groups using numerical identifiers.

- UID: User ID.
- GID: Group ID.

To view your current account information, run:

```bash
id
```

Example output:

```text
uid=1000(kaliuser) gid=1000(kaliuser) groups=1000(kaliuser),27(sudo)
```

The numbers and group memberships may differ on your system.

Explanation:

| Field | Meaning |
|---|---|
| uid | Your user identifier |
| gid | Your primary group identifier |
| groups | Groups your account belongs to |

## 6. Understanding Linux Groups

A group is a collection of user accounts.

Groups make it easier to manage permissions for multiple users.

For example, instead of giving several users individual access to a resource, an administrator can grant access to a group.

To display your groups, run:

```bash
groups
```

To inspect a particular user's groups, run:

```bash
groups kaliuser
```

Replace `kaliuser` with your actual username if different.

To display the system's group database:

```bash
cat /etc/group
```

## 7. Important User Management Files

Linux stores account and authentication information in specific files.

| File | Purpose |
|---|---|
| `/etc/passwd` | Stores user account information |
| `/etc/shadow` | Stores protected password hash information and password-aging data |
| `/etc/group` | Stores group information |
| `/etc/gshadow` | Stores protected group authentication information |

### Inspect the passwd file

```bash
cat /etc/passwd
```

Each line represents a user account.

A simplified entry looks like:

```text
kaliuser:x:1000:1000:Kali User:/home/kaliuser:/bin/bash
```

The fields represent:

1. Username.
2. Password placeholder.
3. UID.
4. Primary GID.
5. User information.
6. Home directory.
7. Login shell.

The `x` indicates that password information is stored separately, normally in `/etc/shadow`.

### Important security note

Never share the contents of `/etc/shadow` or real password hashes publicly.

## 8. Creating a New User

Linux provides commands for creating user accounts.

To create a user named `studentlab`, run:

```bash
sudo adduser studentlab
```

The command will ask you to:

1. Set a password.
2. Confirm the password.
3. Enter optional user information.

You can leave optional information blank.

Check the account:

```bash
id studentlab
```

Check its home directory:

```bash
ls -ld /home/studentlab
```

## 9. Adding a User to a Group

To add `studentlab` to the sudo group, run:

```bash
sudo usermod -aG sudo studentlab
```

Explanation:

| Option | Meaning |
|---|---|
| `usermod` | Modifies a user account |
| `-a` | Appends the user to existing supplementary groups |
| `-G` | Specifies supplementary groups |
| `sudo` | The group being added |

**Important:** Do not omit `-a` when adding a supplementary group with `-G`. Without it, existing supplementary group memberships can be replaced.

Group membership changes normally take effect when the user starts a new login session.

Verify the account's groups:

```bash
groups studentlab
```

## 10. Switching Between Users

Linux allows you to switch between user accounts.

To switch to `studentlab`, run:

```bash
su - studentlab
```

Enter the account's password when requested.

Check the active user:

```bash
whoami
```

To return to your original account, run:

```bash
exit
```

## 11. Understanding Important User Commands

| Command | Purpose |
|---|---|
| `whoami` | Displays the current username |
| `id` | Displays UID, GID, and groups |
| `who` | Shows logged-in users |
| `w` | Shows logged-in users and their activity |
| `groups` | Displays group memberships |
| `passwd` | Changes a user's password |
| `adduser` | Creates a user account |
| `usermod` | Modifies a user account |
| `userdel` | Deletes a user account |
| `su` | Switches to another user |
| `sudo` | Executes authorized commands with elevated privileges |

## 12. Removing a User

If you no longer need the practice account, you can remove it.

Run:

```bash
sudo deluser studentlab
```

This removes the account but normally leaves its home directory.

To remove the account and its home directory:

```bash
sudo deluser --remove-home studentlab
```

Be careful: removing a home directory permanently deletes the files stored there.

For our practical exercise, use the second command only after you have finished working with the test account.

## 13. Why User Management Matters in Cybersecurity

User management is an important part of Linux security.

Consider these situations:

**Scenario 1: Unauthorized access**

An attacker obtains access to an account that should not be accessible.

**Scenario 2: Privilege escalation**

An attacker attempts to gain privileges beyond those originally assigned to their account.

**Scenario 3: Weak account security**

A user chooses a weak password that makes unauthorized access easier.

**Scenario 4: Excessive privileges**

An administrator gives a normal user unnecessary administrative access.

Security professionals study these issues to understand how systems can be protected.

## 14. Practical Assignment

Complete the following exercises on your Kali VM:

- [ ] Run `whoami`.
- [ ] Run `id`.
- [ ] Run `groups`.
- [ ] Inspect `/etc/passwd`.
- [ ] Inspect `/etc/group`.
- [ ] Check your sudo permissions using `sudo -l`.
- [ ] Create a test user named `studentlab`.
- [ ] Check the test user's UID and GID.
- [ ] Add the test user to the sudo group.
- [ ] Verify the group membership.
- [ ] Switch to the test account using `su - studentlab`.
- [ ] Run `whoami` to verify the account switch.
- [ ] Return to your original account using `exit`.
- [ ] Remove the test account and its home directory when finished.

Do not modify the root account or your original Kali account during this exercise.

## 15. Knowledge Check

1. What is the root user in Linux?
2. What does UID mean?
3. What is the purpose of GID?
4. What does the `sudo` command do?
5. What is the difference between `/etc/passwd` and `/etc/shadow`?
6. What does `usermod -aG` do?
7. Why should normal users avoid unnecessary administrative privileges?
8. What is privilege escalation?

## 16. Lesson Summary

In this lesson, we learned about:

- Linux user accounts.
- The root user and UID 0.
- Administrative access using sudo.
- User IDs and Group IDs.
- Linux groups.
- User account configuration files.
- Creating and deleting users.
- Modifying group membership.
- Switching between accounts.

Understanding users and administrative privileges is fundamental to Linux system administration and ethical hacking.

**Next Lesson:** Lesson 11 – Kali Desktop and Terminal Customization.