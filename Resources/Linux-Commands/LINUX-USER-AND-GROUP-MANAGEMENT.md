
# LINUX USER AND GROUP MANAGEMENT

## 1. What Is a Linux User?

A Linux user is an account that allows someone or a service to access and use the operating system.

Each user has:
- A username.
- A User ID (UID).
- A home directory (for normal users).
- Permissions and access rights.

## 2. Types of Users

### Root user
The root user is the administrator account with extensive system privileges.

### Standard user
A normal account used for everyday activities.

### System user
An account created to run system services and applications.

## 3. Display User Information

Show the current username:
```bash
whoami
```

Display user and group IDs:
```bash
id
```

Display logged-in users:
```bash
who
```

Show login history:
```bash
last
```

Display information about a user:
```bash
id username
```

## 4. Creating Users

Create a user:
```bash
sudo useradd username
```

Create a user with a home directory:
```bash
sudo useradd -m username
```

Set a user's password:
```bash
sudo passwd username
```

## 5. Modifying Users

Change a username:
```bash
sudo usermod -l newname oldname
```

Add a user to a supplementary group:
```bash
sudo usermod -aG groupname username
```

Change a user's login shell:
```bash
sudo usermod -s /bin/bash username
```

## 6. Deleting Users

Delete a user:
```bash
sudo userdel username
```

Delete a user and their home directory:
```bash
sudo userdel -r username
```

Be careful when deleting accounts because their files and access may be affected.

## 7. Group Management

Groups allow administrators to assign permissions to multiple users.

Create a group:
```bash
sudo groupadd developers
```

Display groups:
```bash
groups
```

Display a particular user's groups:
```bash
groups username
```

Add a user to a group:
```bash
sudo usermod -aG developers username
```

Delete a group:
```bash
sudo groupdel developers
```

## 8. Important Account Files

| File | Purpose |
|---|---|
| /etc/passwd | Stores user account information |
| /etc/shadow | Stores password hashes and password aging information |
| /etc/group | Stores group information |
| /etc/gshadow | Stores protected group information |

Important: These files contain sensitive account information. Do not modify them manually without understanding the consequences.

## 9. Understanding Sudo

`sudo` allows an authorized user to execute a command with elevated privileges.

Example:
```bash
sudo apt update
```

Check sudo privileges:
```bash
sudo -l
```

Only grant administrative privileges to trusted accounts.

## 10. Cybersecurity Applications

User and group management helps security professionals:

- Control access to systems.
- Apply least-privilege principles.
- Identify unauthorized accounts.
- Investigate suspicious login activity.
- Restrict access to sensitive files.
- Manage administrative privileges.

## 11. Security Best Practices

- Use strong, unique passwords.
- Avoid using root for everyday tasks.
- Give users only the permissions they need.
- Remove unused accounts.
- Review group memberships regularly.
- Monitor login activity.
- Protect sensitive account files.

## 12. Practice Exercises

1. Display your current username.
2. Display your UID and GID.
3. Explain the purpose of the root account.
4. Explain the difference between a user and a group.
5. What does `sudo` do?
6. Name three important Linux account files.

## Summary

Linux user and group management controls who can access a system and what they can do. It is an essential part of Linux administration and cybersecurity.
