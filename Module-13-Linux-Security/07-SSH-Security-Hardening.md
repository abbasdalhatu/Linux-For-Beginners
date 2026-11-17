---
---


# Module 13 – Lesson 07: SSH Security Hardening

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain SSH and its purpose.
2. Understand how SSH connections work.
3. Identify SSH security risks.
4. Understand password-based and key-based authentication.
5. Generate and inspect SSH keys.
6. Understand SSH server configuration.
7. Explain SSH hardening techniques.
8. Inspect SSH service status and logs.
9. Identify suspicious SSH activity.
10. Perform a basic SSH security audit.

---

## 2. What Is SSH?

SSH stands for **Secure Shell**.

It is a network protocol that allows users to securely access and manage remote computers over an unsecured network.

SSH encrypts communication between the client and server.

### Example

Imagine you are managing a Linux server located in another building.

Instead of physically accessing that computer, you can connect remotely using SSH.

```bash
ssh username@server-ip
```

Example:

```bash
ssh student@192.168.1.20
```

The command attempts to establish an SSH connection to the computer at that address.

The connection normally uses TCP port 22 unless configured otherwise.

### Common SSH Uses

- Remote Linux administration.
- Secure command execution.
- Secure file transfer.
- Remote server maintenance.
- Git repository access.
- Automated administration.

---

## 3. Understanding SSH Architecture

SSH operates using two main components.

### SSH Client

The client initiates a connection.

Examples include:

- `ssh`
- `scp`
- `sftp`

### SSH Server

The server receives SSH connection requests.

The common OpenSSH server daemon is:

```text
sshd
```

### Connection Diagram

```text
+--------------------+
|   SSH Client       |
|                    |
|   Kali Terminal    |
+---------+----------+
          |
          | Encrypted SSH Connection
          | TCP Port 22 (default)
          |
+---------v----------+
|   SSH Server       |
|                    |
|   sshd             |
+--------------------+
```

### Important Security Features

SSH provides:

1. Encryption.
2. Server authentication.
3. Client authentication.
4. Data integrity.
5. Secure remote communication.

---

## 4. Understanding SSH Authentication

SSH supports multiple authentication methods.

Two important methods are password authentication and public-key authentication.

### 4.1 Password Authentication

The client authenticates using an account password.

Example:

```bash
ssh student@192.168.1.20
```

The server may request the account password.

Security concern:

Passwords can be exposed through phishing, reuse, weak credentials, or other attacks.

### 4.2 Public-Key Authentication

Public-key authentication uses a cryptographic key pair.

The two components are:

- Private key: kept secret by the user.
- Public key: installed on the remote server.

The client proves possession of the private key without sending it to the server.

### Key Pair Structure

```text
SSH KEY PAIR
    |
    +---- Private Key
    |       |
    |       +---- Keep Secret
    |
    +---- Public Key
            |
            +---- Install on Authorised Server
```

Public-key authentication can reduce dependence on passwords, especially when private keys are protected with strong passphrases.

---

## 5. Understanding SSH Key Algorithms

Common SSH key types include:

| Algorithm | Description |
|---|---|
| Ed25519 | Modern, efficient elliptic-curve SSH key type |
| RSA | Widely supported public-key algorithm |
| ECDSA | Elliptic-curve digital signature algorithm |
| DSA | Legacy algorithm that should generally be avoided |

For a new OpenSSH key, Ed25519 is usually a good choice where supported.

### Generate an SSH Key

Use:

```bash
ssh-keygen -t ed25519 -C "linux-security-lab"
```

You will be asked where to save the key.

The default location is usually:

```text
~/.ssh/id_ed25519
```

The public key is:

```text
~/.ssh/id_ed25519.pub
```

Set a strong passphrase when prompted.

**Important:** Never share your private key.

For this lesson, you do not need to create a key unless you want to practise. The lab will inspect your existing SSH setup without changing it.

### Inspect Existing SSH Keys

```bash
ls -la ~/.ssh/
```

To inspect only public key files:

```bash
find ~/.ssh -maxdepth 1 -type f -name "*.pub" -print
```

Never display private key contents in a report.

---

## 6. SSH Configuration Files

OpenSSH uses different configuration files for client and server settings.

### Client Configuration

```text
~/.ssh/config
```

This controls SSH client preferences for remote connections.

### System-Wide Client Configuration

```text
/etc/ssh/ssh_config
```

### SSH Server Configuration

```text
/etc/ssh/sshd_config
```

The server configuration controls how the SSH server operates.

Examples of configurable settings include:

- Port.
- Root login policy.
- Password authentication.
- Public-key authentication.
- Allowed users.
- Login grace time.
- Maximum authentication attempts.

Do not edit these files during this lesson.

---

## 7. Important SSH Server Security Settings

The following are examples of security-related OpenSSH server directives.

### 7.1 PermitRootLogin

Controls whether the root account may log in directly through SSH.

A common hardened setting is:

```text
PermitRootLogin no
```

This encourages administrators to use individual accounts and controlled privilege elevation.

### 7.2 PasswordAuthentication

Controls password-based SSH authentication.

Example:

```text
PasswordAuthentication no
```

This may be appropriate when public-key authentication has already been properly configured and tested.

Do not disable password authentication on a remote server before confirming that another working access method exists.

### 7.3 PubkeyAuthentication

Controls public-key authentication.

Example:

```text
PubkeyAuthentication yes
```

### 7.4 MaxAuthTries

Controls the maximum number of authentication attempts permitted per connection.

Example:

```text
MaxAuthTries 3
```

### 7.5 AllowUsers

Restricts which usernames may log in through SSH.

Example:

```text
AllowUsers student administrator
```

These are configuration examples, not instructions to apply them blindly.

The correct settings depend on the server's users, access requirements, and security policy.

---

## 8. SSH Security Hardening

SSH hardening means reducing the risk of unauthorised access and protecting remote administration.

### Important Hardening Measures

1. Prefer public-key authentication where practical.
2. Protect private keys with strong passphrases.
3. Disable direct root login where appropriate.
4. Restrict access to authorised accounts.
5. Keep OpenSSH updated.
6. Monitor authentication logs.
7. Use firewalls to restrict SSH access.
8. Avoid exposing SSH unnecessarily to public networks.
9. Use MFA where supported and required.
10. Protect SSH configuration files.
11. Validate configuration changes before restarting the service.
12. Maintain an alternative administrative access method.

### Why Changing the SSH Port Is Not Enough

Some administrators change SSH from port 22 to another port.

This can reduce automated noise from basic scans, but it does not replace authentication security.

Strong credentials, access restrictions, patching, and monitoring are more important.

---

## 9. Understanding SSH File Permissions

SSH relies on appropriate file permissions to protect private keys and authentication configuration.

Typical examples include:

| Path | Common Permission |
|---|---|
| `~/.ssh/` | 700 |
| `~/.ssh/id_ed25519` | 600 |
| `~/.ssh/id_ed25519.pub` | 644 |
| `~/.ssh/authorized_keys` | 600 |

Actual requirements can vary by implementation and configuration.

### Inspect Your SSH Directory

```bash
ls -ld ~/.ssh
```

### Inspect SSH Files

```bash
ls -la ~/.ssh/
```

### Why This Matters

If private keys are readable by unauthorised users, attackers may be able to steal them and impersonate the key owner.

Do not recursively change permissions on your entire home directory.

---

## 10. SSH Service Management

On systems using systemd, the SSH server is commonly managed through `systemctl`.

The service name may be `ssh` or `sshd`, depending on the distribution.

### Check SSH Service Status

Try:

```bash
systemctl status ssh --no-pager
```

If that service name does not exist, try:

```bash
systemctl status sshd --no-pager
```

### Check Installed OpenSSH Packages

```bash
dpkg -l | grep -E 'openssh-client|openssh-server'
```

### Check Whether Port 22 Is Listening

```bash
sudo ss -tlnp | grep ':22'
```

If there is no output, it does not necessarily mean SSH is broken. The server may not be installed, running, or listening on the default port.

Do not start or enable an SSH server just to complete this inspection lesson.

---

## 11. SSH Logs and Monitoring

SSH logs can reveal connection attempts and authentication events.

On Debian-based systems, relevant information may appear in:

```text
/var/log/auth.log
```

The system journal may also contain SSH events.

### Inspect SSH Logs

```bash
sudo journalctl -b --no-pager | grep -Ei 'sshd|ssh' | tail -n 30
```

If available:

```bash
sudo grep -Ei 'sshd|ssh' /var/log/auth.log | tail -n 30
```

### Events Worth Investigating

- Repeated failed authentication.
- Successful login from an unexpected address.
- Attempts to log in as root.
- Unexpected account names.
- Unusual login times.
- Unexpected changes to SSH configuration.

A single failed attempt is not proof of an attack. Look for patterns and supporting evidence.

---

## 12. Common SSH Attacks

### 12.1 SSH Brute-Force Attacks

An attacker repeatedly attempts to authenticate using different passwords.

### 12.2 Credential Theft

An attacker steals a user's password or private SSH key.

### 12.3 Man-in-the-Middle Attacks

An attacker attempts to intercept or manipulate communication between a client and server.

SSH host-key verification helps protect against connecting to an impersonated server.

### 12.4 Weak SSH Configuration

An improperly configured server may permit unnecessary or insecure access.

### 12.5 Unauthorised SSH Key Installation

An attacker who gains access to an account may add an unauthorised public key to its `authorized_keys` file.

### Defensive Measures

- Verify server host keys.
- Protect private keys.
- Restrict SSH access.
- Monitor authentication events.
- Review authorised public keys.
- Maintain updated OpenSSH packages.
- Apply least privilege.

---

# 13. Practical Lab: SSH Security Audit

## Lab Objective

Inspect your Kali Linux SSH environment and identify its current security configuration.

This lesson is inspection-only.

Do not change SSH settings, restart services, expose ports, or attempt connections to systems without permission.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson07
cd ~/module13-lab/lesson07
```

### Step 2: Check Installed SSH Packages

```bash
dpkg -l | grep -E 'openssh-client|openssh-server'
```

### Step 3: Check SSH Service Status

```bash
systemctl status ssh --no-pager
```

If the service name is unavailable:

```bash
systemctl status sshd --no-pager
```

### Step 4: Inspect SSH Server Configuration Permissions

```bash
ls -l /etc/ssh/sshd_config
```

If the file exists, inspect relevant directives without modifying it:

```bash
grep -Ei '^[[:space:]]*(Port|PermitRootLogin|PasswordAuthentication|PubkeyAuthentication|MaxAuthTries|AllowUsers)' /etc/ssh/sshd_config
```

Remember that included configuration files, default values, and other settings may affect the effective configuration.

### Step 5: Inspect Your SSH Client Directory

```bash
ls -ld ~/.ssh
```

### Step 6: Inspect SSH Public Keys

```bash
find ~/.ssh -maxdepth 1 -type f -name "*.pub" -print
```

Do not display private key files.

### Step 7: Check SSH Listening Ports

```bash
sudo ss -tlnp | grep -E 'ssh|:22'
```

### Step 8: Inspect SSH Logs

```bash
sudo journalctl -b --no-pager | grep -Ei 'sshd|ssh' | tail -n 30
```

If available:

```bash
sudo grep -Ei 'sshd|ssh' /var/log/auth.log | tail -n 30
```

### Step 9: Generate Your SSH Security Report

Run:

```bash
{
    echo "LINUX SSH SECURITY AUDIT"
    echo "========================"
    echo
    echo "INSTALLED SSH PACKAGES"
    dpkg -l | grep -E 'openssh-client|openssh-server'
    echo
    echo "SSH SERVICE STATUS"
    systemctl status ssh --no-pager 2>&1
    echo
    echo "SSH SERVER CONFIGURATION PERMISSIONS"
    ls -l /etc/ssh/sshd_config 2>&1
    echo
    echo "SELECTED SSH SERVER DIRECTIVES"
    grep -Ei '^[[:space:]]*(Port|PermitRootLogin|PasswordAuthentication|PubkeyAuthentication|MaxAuthTries|AllowUsers)' /etc/ssh/sshd_config 2>&1
    echo
    echo "SSH DIRECTORY PERMISSIONS"
    ls -ld ~/.ssh 2>&1
    echo
    echo "SSH PUBLIC KEY FILES"
    find ~/.ssh -maxdepth 1 -type f -name "*.pub" -print 2>&1
    echo
    echo "SSH LISTENING PORTS"
    sudo ss -tlnp | grep -E 'ssh|:22'
    echo
    echo "RECENT SSH EVENTS"
    sudo journalctl -b --no-pager | grep -Ei 'sshd|ssh' | tail -n 30
} > ssh-security-report.txt 2>&1
```

Inspect your report:

```bash
cat ssh-security-report.txt
```

If your SSH server is not installed or running, record that as an observation rather than treating it as a failure.

### Expected Files

```text
lesson07/
└── ssh-security-report.txt
```

---

## 14. Real-World Cybersecurity Application

Imagine you are responsible for securing a university's Linux server.

Your audit discovers:

- Direct root SSH login is permitted.
- Password authentication is enabled.
- Several accounts have unnecessary SSH access.
- Authentication failures are not regularly reviewed.
- Private SSH keys are poorly protected.

Your recommendations should include:

1. Restrict direct root login.
2. Review authorised SSH users.
3. Introduce properly tested public-key authentication.
4. Protect private keys.
5. Review authentication logs.
6. Apply security updates.
7. Validate configuration changes.
8. Maintain an alternative recovery access method.

Always test changes in a controlled environment before applying them to production systems.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What does SSH stand for?
2. What is the primary purpose of SSH?
3. What is the default SSH port?
4. What is the difference between an SSH client and server?
5. What is `sshd`?
6. Differentiate between password and public-key authentication.
7. What is the purpose of an SSH private key?
8. What is the purpose of an SSH public key?
9. What is Ed25519?
10. What is the purpose of `sshd_config`?
11. What does `PermitRootLogin` control?
12. Why might administrators disable password authentication?
13. Why is changing the default SSH port insufficient by itself?
14. What is an SSH brute-force attack?
15. Why is SSH host-key verification important?
16. Why must private SSH keys be protected?
17. How can administrators inspect SSH service status?
18. Identify five SSH hardening measures.

---

## 16. Practical Completion Checklist

- [ ] I understand SSH.
- [ ] I understand SSH client-server architecture.
- [ ] I understand password authentication.
- [ ] I understand public-key authentication.
- [ ] I understand SSH key algorithms.
- [ ] I understand SSH configuration files.
- [ ] I understand SSH hardening.
- [ ] I can inspect SSH service status.
- [ ] I can inspect SSH-related logs.
- [ ] I understand common SSH attacks.
- [ ] I completed the SSH security audit.
- [ ] I generated my SSH security report.

**Lesson 07 Complete: SSH Security Hardening.**
