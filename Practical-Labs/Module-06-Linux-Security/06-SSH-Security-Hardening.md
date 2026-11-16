
# PRACTICAL LABS
# MODULE 06 – LINUX SECURITY

## LAB 06: SSH SECURITY HARDENING

### 1. Introduction to SSH

SSH stands for Secure Shell.

It is a network protocol used to securely access and administer remote computers.

SSH allows administrators to:

- Log into remote Linux systems.
- Execute remote commands.
- Transfer files securely.
- Manage servers.
- Perform remote maintenance.
- Establish encrypted communication sessions.

SSH is widely used in system administration, cloud computing, cybersecurity, and software development.

### 2. How SSH Works

SSH uses client-server architecture.

The two main components are:

**SSH Client:** The computer or application initiating the connection.

**SSH Server:** The computer accepting SSH connections.

Example:

```text
Your Computer (SSH Client)
          |
          | Encrypted SSH Connection
          |
          v
Remote Linux Computer (SSH Server)
```

The default SSH port is:

```text
TCP 22
```

An administrator can configure SSH to use another port, but changing the port alone does not provide strong security.

### 3. SSH Authentication Methods

SSH supports several authentication methods.

#### A. Password Authentication

The user authenticates using a username and password.

Example:

```bash
ssh username@192.168.1.100
```

#### B. Public-Key Authentication

Public-key authentication uses a cryptographic key pair:

- Private key.
- Public key.

The private key remains with the client.

The public key is installed on the server.

The private key should never be shared.

#### C. Multi-Factor Authentication

MFA requires additional verification beyond a password or SSH key.

Examples include an authenticator application or hardware security key, depending on the SSH configuration.

### 4. Understanding SSH Key Pairs

An SSH key pair contains two related cryptographic keys.

<text weight="semibold">Private key</text>

- Kept secret.
- Stored on the client.
- Used to prove possession of the key.

<text weight="semibold">Public key</text>

- Can be shared.
- Installed on the server.
- Used by the server to verify authentication.

Common key algorithms include:

- Ed25519.
- RSA.
- ECDSA.

Ed25519 is a widely used modern choice where supported.

### 5. SSH Security Risks

Poorly configured SSH services may be exposed to:

1. Password guessing.
2. Brute-force attempts.
3. Unauthorized remote access.
4. Stolen private keys.
5. Weak cryptographic settings.
6. Unnecessary public exposure.
7. Poor access-control configuration.

SSH hardening reduces these risks.

### 6. Create Your Lab Directory

Run:

```bash
mkdir -p ~/practical-labs/module06/lab06
cd ~/practical-labs/module06/lab06
```

### 7. Inspect SSH Installation

#### Task 1: Check the SSH Client

Run:

```bash
ssh -V
```

This displays the installed SSH client version.

#### Task 2: Check SSH Server Package Status

Run:

```bash
dpkg -s openssh-server
```

If the package is not installed, record that fact.

Installing the SSH server is not required for this lab.

#### Task 3: Check SSH Service Status

Run:

```bash
systemctl status ssh --no-pager
```

If the service does not exist, record that it is not installed.

Do not start the service just for inspection.

#### Task 4: Inspect SSH Listening Ports

Run:

```bash
sudo ss -tulpn | grep -E 'ssh|:22'
```

This checks for SSH-related listening sockets.

No output may simply mean that the SSH server is not running.

### 8. Understand SSH Configuration Files

The main SSH server configuration file is:

```text
/etc/ssh/sshd_config
```

The SSH client configuration file is:

```text
/etc/ssh/ssh_config
```

Additional server configuration files may be stored in:

```text
/etc/ssh/sshd_config.d/
```

Inspect the main server configuration if it exists:

```bash
sudo ls -l /etc/ssh/sshd_config
```

Read the active-looking configuration directives:

```bash
sudo grep -Ev '^[[:space:]]*(#|$)' /etc/ssh/sshd_config
```

Inspect additional configuration files:

```bash
sudo ls -la /etc/ssh/sshd_config.d/
```

Configuration files may contain comments and defaults. The effective configuration should be checked with `sshd -T` when the server is installed.

### 9. Important SSH Security Directives

| Directive | Purpose |
|---|---|
| `PermitRootLogin` | Controls direct root login |
| `PasswordAuthentication` | Controls password-based authentication |
| `PubkeyAuthentication` | Controls public-key authentication |
| `PermitEmptyPasswords` | Controls empty-password login |
| `MaxAuthTries` | Limits authentication attempts per connection |
| `AllowUsers` | Restricts permitted usernames |
| `AllowGroups` | Restricts permitted groups |
| `X11Forwarding` | Controls X11 forwarding |
| `LoginGraceTime` | Limits time allowed for authentication |
| `Port` | Configures the SSH listening port |

These settings must be chosen according to operational requirements.

For example, disabling password authentication before confirming that key-based access works can lock an administrator out.

### 10. Practical Exercise – Inspect Effective SSH Configuration

If the SSH server binary is installed, run:

```bash
sudo sshd -T
```

This displays effective SSH server configuration.

To inspect selected security-related directives:

```bash
sudo sshd -T | grep -E 'permitrootlogin|passwordauthentication|pubkeyauthentication|permitemptypasswords|maxauthtries|x11forwarding|port'
```

If `sshd` is unavailable, record that the server component is not installed.

Do not confuse SSH client configuration with SSH server configuration.

### 11. Understand SSH Host Keys

SSH servers use host keys to identify themselves to clients.

Host keys help clients recognize whether they are connecting to the expected server.

Common host-key files include:

```text
/etc/ssh/ssh_host_ed25519_key
/etc/ssh/ssh_host_ed25519_key.pub
```

The private host key must remain protected.

Inspect available host-key filenames without displaying private key contents:

```bash
sudo find /etc/ssh -maxdepth 1 -type f -name 'ssh_host_*' -printf '%f\n'
```

Never copy private host keys into your report.

### 12. Understand SSH Client Known Hosts

SSH clients maintain a list of previously trusted server host keys.

A common location is:

```text
~/.ssh/known_hosts
```

Inspect the file's existence and permissions:

```bash
ls -l ~/.ssh/known_hosts
```

If the file does not exist, that is normal for a system that has not established SSH connections.

A changed host-key warning should be investigated rather than ignored.

It may indicate a legitimate server replacement, but it can also indicate an interception attempt.

### 13. Generate an SSH Key Pair Safely

You can practise key generation locally without connecting to another computer.

Run:

```bash
ssh-keygen -t ed25519 -f ~/practical-labs/module06/lab06/lab_ed25519
```

When prompted, enter a passphrase for the private key.

The command creates:

```text
lab_ed25519
lab_ed25519.pub
```

The first file is the private key.

The second file is the public key.

Inspect the public key:

```bash
cat ~/practical-labs/module06/lab06/lab_ed25519.pub
```

Inspect file permissions:

```bash
ls -l ~/practical-labs/module06/lab06/lab_ed25519*
```

The private key should have restrictive permissions.

If necessary, set them:

```bash
chmod 600 ~/practical-labs/module06/lab06/lab_ed25519
chmod 644 ~/practical-labs/module06/lab06/lab_ed25519.pub
```

Never publish or share the private key.

### 14. Understand SSH Key Fingerprints

A fingerprint is a compact representation of a public key.

Run:

```bash
ssh-keygen -lf ~/practical-labs/module06/lab06/lab_ed25519.pub
```

Record the key type and fingerprint.

Fingerprints help administrators verify key identity.

### 15. Understand SSH Connection Syntax

General syntax:

```bash
ssh USERNAME@SERVER_IP
```

For a non-default port:

```bash
ssh -p PORT USERNAME@SERVER_IP
```

To specify a private key:

```bash
ssh -i PRIVATE_KEY USERNAME@SERVER_IP
```

These commands demonstrate syntax only.

Do not attempt to connect to systems you do not own or have permission to administer.

### 16. Safe SSH Hardening Principles

A secure SSH configuration should consider:

1. Restricting administrative access.
2. Using public-key authentication where appropriate.
3. Protecting private keys with suitable permissions and passphrases.
4. Disabling direct root login where operationally appropriate.
5. Restricting permitted users or groups.
6. Monitoring authentication logs.
7. Keeping OpenSSH updated.
8. Limiting network exposure.
9. Verifying configuration before restarting services.
10. Maintaining a recovery path.

For a server administrator, always retain a working administrative session when testing SSH changes remotely.

### 17. Inspect SSH Authentication Logs

Use:

```bash
sudo journalctl -u ssh -n 50 --no-pager
```

If the service is not installed or running, the journal may have no matching entries.

You can also inspect general authentication-related events:

```bash
sudo journalctl -b --no-pager | grep -Ei 'ssh|sshd|authentication' | tail -30
```

Record relevant findings without copying credentials or private keys.

### 18. Generate an SSH Security Audit Report

Create:

```bash
nano ssh-security-audit.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module06/lab06/ssh-security-report.txt"

{
    echo "SSH SECURITY AUDIT REPORT"
    echo "========================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "SSH CLIENT VERSION"
    ssh -V 2>&1
    echo

    echo "OPENSSH SERVER PACKAGE"
    dpkg -s openssh-server 2>/dev/null |
        grep -E '^(Package|Status|Version)'
    echo

    echo "SSH SERVICE STATUS"
    systemctl status ssh --no-pager 2>&1 | head -20
    echo

    echo "SSH LISTENING SOCKETS"
    sudo ss -tulpn | grep -E 'ssh|:22' || echo "No matching SSH socket found."
    echo

    echo "SERVER CONFIGURATION FILE"
    if [ -f /etc/ssh/sshd_config ]; then
        sudo grep -Ev '^[[:space:]]*(#|$)' /etc/ssh/sshd_config
    else
        echo "SSH server configuration not found."
    fi
    echo

    echo "EFFECTIVE SERVER CONFIGURATION"
    if command -v sshd >/dev/null 2>&1; then
        sudo sshd -T 2>/dev/null |
            grep -E 'permitrootlogin|passwordauthentication|pubkeyauthentication|permitemptypasswords|maxauthtries|x11forwarding|port'
    else
        echo "SSH server binary not installed."
    fi
    echo

    echo "SSH HOST KEY FILENAMES"
    sudo find /etc/ssh -maxdepth 1 -type f \
        -name 'ssh_host_*' -printf '%f\n' 2>/dev/null
    echo

    echo "RECENT SSH JOURNAL EVENTS"
    sudo journalctl -u ssh -n 30 --no-pager 2>&1

} > "$REPORT" 2>&1

echo "SSH security report generated:"
echo "$REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ssh-security-audit.sh
```

Run:

```bash
./ssh-security-audit.sh
```

Inspect:

```bash
less ssh-security-report.txt
```

Press `q` to exit.

### 19. Knowledge Check

1. What does SSH stand for?
2. What is the purpose of SSH?
3. What is the default SSH port?
4. What is the difference between an SSH client and server?
5. What is password authentication?
6. What is public-key authentication?
7. What is the difference between a public key and private key?
8. What is the purpose of `sshd_config`?
9. What does `PermitRootLogin` control?
10. What does `PasswordAuthentication` control?
11. What are SSH host keys?
12. What is the purpose of `known_hosts`?
13. Why should private keys be protected?
14. Why should SSH authentication logs be monitored?
15. Why should administrators verify configuration before restarting SSH?

### 20. Lab Documentation

Create:

```bash
nano lab06-report.txt
```

Record:

```text
MODULE 06 – LAB 06
SSH SECURITY HARDENING

1. SSH client version:
2. Is openssh-server installed?
3. SSH service status:
4. Is an SSH listening socket present?
5. SSH server configuration file:
6. Effective root login setting, if available:
7. Effective password authentication setting, if available:
8. Public-key authentication setting:
9. Number and types of host keys observed:
10. Was an Ed25519 key pair generated?
11. Private key permissions:
12. Public key fingerprint:
13. Where are SSH authentication events recorded?
14. Five SSH security best practices:
15. What did I learn?

SSH SECURITY REPORT:
Generated successfully: YES / NO

LAB STATUS:
COMPLETED / NOT COMPLETED
```

### 21. Completion Checklist

- [ ] Understood SSH.
- [ ] Learned SSH client-server architecture.
- [ ] Studied SSH authentication methods.
- [ ] Understood public and private keys.
- [ ] Inspected SSH installation.
- [ ] Inspected SSH service status.
- [ ] Inspected SSH configuration.
- [ ] Learned important SSH security directives.
- [ ] Inspected host-key filenames.
- [ ] Generated an Ed25519 key pair.
- [ ] Inspected key permissions.
- [ ] Generated a public-key fingerprint.
- [ ] Reviewed SSH logs.
- [ ] Generated the SSH security audit report.
- [ ] Completed lab documentation.

**LAB 06 STATUS: READY FOR PRACTICAL COMPLETION**
