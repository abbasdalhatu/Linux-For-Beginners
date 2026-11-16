
# Module 12 – Lesson 13: Remote System Administration with SSH

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain remote system administration.
2. Understand SSH and how it works.
3. Identify the components of an SSH connection.
4. Connect to a remote Linux computer.
5. Understand SSH authentication methods.
6. Generate and manage SSH keys.
7. Understand SSH configuration files.
8. Transfer files securely using SCP and SFTP.
9. Understand SSH security practices.
10. Perform a basic SSH security inspection.

---

## 2. What Is Remote System Administration?

Remote system administration is the process of managing a computer from another computer through a network.

An administrator does not need to sit physically in front of the server.

For example, imagine a company has a Linux server located in another city.

An administrator can use a laptop to:

- Connect to the server.
- Inspect system resources.
- Manage files.
- Install software.
- Monitor services.
- Troubleshoot problems.
- Perform authorised maintenance.

This saves time and makes managing multiple computers easier.

---

## 3. What Is SSH?

SSH stands for **Secure Shell**.

It is a network protocol used to establish an encrypted connection between two computers.

SSH commonly uses TCP port:

```text
22
```

SSH provides:

- Encrypted communication.
- Remote command execution.
- User authentication.
- Secure file transfers.
- Protection against many network interception attacks.

### Example

A computer administrator wants to access a Linux server.

The administrator runs:

```bash
ssh username@192.168.1.100
```

The command requests a connection to:

- Username: `username`
- Server IP: `192.168.1.100`
- Default SSH port: `22`

The address is illustrative.

---

## 4. Understanding How SSH Works

An SSH connection involves two main systems.

### A. SSH Client

The SSH client is the computer initiating the connection.

Examples:

- Kali Linux terminal.
- Ubuntu terminal.
- Windows PowerShell.
- macOS terminal.

### B. SSH Server

The SSH server is the computer accepting incoming SSH connections.

On Linux, the OpenSSH server commonly provides this functionality through the `sshd` service.

### Connection process

1. The client contacts the SSH server.
2. The server presents its host key.
3. The client verifies the server's identity.
4. Both systems establish encrypted communication.
5. The client authenticates the user.
6. The server grants access if authentication succeeds.

### Simple diagram

```text
+-------------------+       Encrypted SSH       +-------------------+
|                   |  ----------------------->  |                   |
|   SSH CLIENT      |                            |   SSH SERVER      |
|                   |  <-----------------------  |                   |
|   Administrator   |       SSH Responses        |   Remote Linux    |
|   Laptop          |                            |   Computer        |
+-------------------+                            +-------------------+
```

---

## 5. Checking SSH Availability

Before connecting to a remote computer, inspect the SSH client.

### Check SSH version

```bash
ssh -V
```

This displays the installed SSH client version.

### Locate the SSH executable

```bash
command -v ssh
```

### Check the SSH server service

```bash
systemctl status ssh
```

The SSH server may not be installed or active in your Kali VM. That is acceptable.

### Check whether port 22 is listening locally

```bash
ss -tuln | grep ':22'
```

No output may simply mean that no service is listening on that port.

**Important:** An SSH client can connect to remote systems even when the local computer does not run an SSH server.

---

## 6. Connecting to a Remote Linux Computer

The general SSH command is:

```bash
ssh username@server_ip
```

Example:

```bash
ssh admin@192.168.1.100
```

This attempts to connect to the computer at `192.168.1.100` as user `admin`.

You must have:

- A reachable server.
- An SSH server running on the destination.
- An authorised account.
- A valid authentication method.
- Permission to access the computer.

### Connecting through a different port

If the SSH server uses a non-default port:

```bash
ssh -p 2222 username@server_ip
```

The `-p` option specifies the destination port.

Only use this against a system you own or are authorised to administer.

---

## 7. Understanding SSH Authentication

Authentication verifies that the person connecting is authorised.

SSH supports several authentication methods.

### A. Password Authentication

The user provides an account password.

Example:

```bash
ssh username@server_ip
```

The server may prompt for the password.

### B. Public-Key Authentication

This method uses a cryptographic key pair.

There are two keys:

- Private key: Kept secret on the client.
- Public key: Installed or registered on the server.

The private key proves possession of the corresponding public key without transmitting the private key itself.

### C. Multi-Factor Authentication

Some SSH environments require more than one authentication factor.

For example:

- An SSH key.
- A one-time authentication code.

This adds another layer of protection.

---

## 8. Understanding SSH Key Pairs

SSH keys provide a strong authentication method.

A common modern choice is Ed25519.

Generate an Ed25519 key pair:

```bash
ssh-keygen -t ed25519
```

The command asks where to save the key and whether to protect it with a passphrase.

For this exercise, use a dedicated filename in your lesson directory and set a passphrase.

Example key filenames:

```text
id_ed25519
id_ed25519.pub
```

### Important distinction

| File | Purpose |
|---|---|
| `id_ed25519` | Private key |
| `id_ed25519.pub` | Public key |

**Never share your private key.**

A public key is designed to be shared with the server administrator.

### Display your public key

```bash
cat id_ed25519.pub
```

### Display the public key fingerprint

```bash
ssh-keygen -lf id_ed25519.pub
```

The fingerprint is a compact representation used to identify a key.

### Inspect key permissions

```bash
ls -l id_ed25519*
```

Private keys should be accessible only to their owner.

A typical private-key permission is:

```text
-rw-------
```

This corresponds to permission mode `600`.

---

## 9. Understanding known_hosts

SSH maintains a file containing information about remote server host keys.

The usual location is:

```text
~/.ssh/known_hosts
```

This helps the client recognise servers it has connected to before.

### Why is this important?

Imagine an attacker attempts to impersonate a server.

SSH host-key verification helps detect unexpected server identity changes.

When connecting to a server for the first time, SSH may ask whether you trust its host key.

Verify the fingerprint through a trusted, independent channel before accepting it.

### Inspect known_hosts

```bash
ls -l ~/.ssh/known_hosts
```

The file may not exist if you have never connected to an SSH server.

Do not blindly accept changed host keys.

A changed key may indicate a legitimate server replacement, but it could also indicate a security problem.

---

## 10. Understanding SSH Configuration Files

SSH uses configuration files to control connection behaviour.

### A. Client configuration

Location:

```text
~/.ssh/config
```

This file stores client-side connection preferences.

Example:

```text
Host university-server
    HostName 192.168.1.100
    User student
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

This is an illustrative configuration.

After saving an appropriate configuration, a user may connect using:

```bash
ssh university-server
```

### B. Server configuration

Location:

```text
/etc/ssh/sshd_config
```

This file controls many SSH server settings.

Inspect it:

```bash
sudo cat /etc/ssh/sshd_config
```

You may also see configuration files under:

```text
/etc/ssh/sshd_config.d/
```

### Important security settings

Examples include:

| Setting | Purpose |
|---|---|
| `PermitRootLogin` | Controls direct root login |
| `PasswordAuthentication` | Controls password authentication |
| `PubkeyAuthentication` | Controls public-key authentication |
| `AllowUsers` | Restricts which users can log in |
| `Port` | Configures the listening port |

These settings depend on the SSH server configuration and distribution.

Do not modify SSH server configuration during this lesson.

---

## 11. Secure File Transfers with SCP

SCP stands for Secure Copy Protocol.

It allows files to be copied between computers using SSH transport.

### Copy a local file to a remote server

```bash
scp report.txt username@server_ip:/home/username/
```

### Copy a remote file to your local computer

```bash
scp username@server_ip:/home/username/report.txt .
```

### Copy a directory

```bash
scp -r project/ username@server_ip:/home/username/
```

The `-r` option copies directories recursively.

### Important

Use SCP only with systems where you have authorised access.

Verify source and destination paths before transferring important data.

---

## 12. Secure File Transfers with SFTP

SFTP stands for SSH File Transfer Protocol.

It provides file-management operations over an SSH connection.

Connect using:

```bash
sftp username@server_ip
```

Common SFTP commands include:

| Command | Purpose |
|---|---|
| `ls` | List remote files |
| `pwd` | Show remote directory |
| `cd` | Change remote directory |
| `lpwd` | Show local directory |
| `lls` | List local files |
| `get` | Download a file |
| `put` | Upload a file |
| `mkdir` | Create a remote directory |
| `exit` | Close the connection |

Example:

```text
sftp> ls
sftp> pwd
sftp> exit
```

SFTP is useful for transferring files securely without opening an ordinary unencrypted file-transfer connection.

---

## 13. SSH Security Best Practices

SSH is powerful, but poor configuration can expose systems.

Important security practices include:

### 1. Use Strong Authentication

Prefer properly managed public-key authentication where appropriate.

Protect private keys with strong passphrases.

### 2. Protect Private Keys

Never:

- Upload private keys to public repositories.
- Send private keys through ordinary messages.
- Share private keys with other users.
- Store private keys in publicly accessible folders.

### 3. Verify Server Host Keys

Check server fingerprints before trusting a new or changed host key.

### 4. Restrict User Access

Allow only authorised accounts to connect.

### 5. Avoid Unnecessary Root Login

Use individual accounts and controlled administrative privileges.

### 6. Keep OpenSSH Updated

Security updates help protect against known vulnerabilities.

### 7. Monitor Authentication Logs

Review SSH login activity and investigate unexpected attempts.

### 8. Limit Network Exposure

Make SSH accessible only to trusted networks or authorised remote users where practical.

Changing the SSH port alone is not a replacement for strong authentication and access control.

---

## 14. Practical Lab: SSH Client Inspection and Key Management

### Objective

Inspect the SSH client, generate a dedicated key pair, examine its fingerprint, and document SSH security information.

This lab does not require exposing your Kali VM to a network or enabling an SSH server.

### Step 1: Create your lesson directory

```bash
mkdir -p ~/module12-lab/lesson13
cd ~/module12-lab/lesson13
```

### Step 2: Inspect the SSH client

```bash
ssh -V
```

### Step 3: Locate the SSH client

```bash
command -v ssh
```

### Step 4: Inspect the SSH directory

```bash
ls -lah ~/.ssh
```

The directory may not exist yet.

### Step 5: Generate a dedicated Ed25519 key pair

```bash
ssh-keygen -t ed25519 -f ~/module12-lab/lesson13/lab_ed25519 -C "module13-lab"
```

When prompted:

1. Confirm the filename.
2. Enter a passphrase.
3. Confirm the passphrase.

Use a passphrase that is not reused for other accounts.

### Step 6: Verify the generated files

```bash
ls -l ~/module12-lab/lesson13/
```

You should see:

```text
lab_ed25519
lab_ed25519.pub
```

### Step 7: Inspect the public key

```bash
cat ~/module12-lab/lesson13/lab_ed25519.pub
```

Do not display or share the private key contents.

### Step 8: Inspect the fingerprint

```bash
ssh-keygen -lf ~/module12-lab/lesson13/lab_ed25519.pub
```

Record the fingerprint.

### Step 9: Verify private-key permissions

```bash
stat -c '%a %n' ~/module12-lab/lesson13/lab_ed25519
```

The expected permission mode is:

```text
600
```

If it differs, correct it:

```bash
chmod 600 ~/module12-lab/lesson13/lab_ed25519
```

### Step 10: Inspect SSH client configuration

```bash
ssh -G example.com | head -n 20
```

This displays the effective client configuration for the example hostname.

It does not establish a connection to that server.

### Step 11: Inspect SSH server configuration

```bash
sudo sshd -T
```

If `sshd` is installed, this displays effective server configuration values.

If the command is unavailable, record that the SSH server component is not installed.

Do not install or enable it solely for this exercise.

### Step 12: Inspect listening SSH ports

```bash
ss -tuln | grep ':22'
```

No output is acceptable if no SSH server is listening.

---

## 15. Practical Lab: Generate an SSH Administration Report

Run:

```bash
{
    echo "LINUX SSH ADMINISTRATION REPORT"
    echo "==============================="
    echo
    echo "Date:"
    date
    echo
    echo "Hostname:"
    hostname
    echo
    echo "SSH Client Version:"
    ssh -V 2>&1
    echo
    echo "SSH Client Location:"
    command -v ssh
    echo
    echo "SSH Directory:"
    ls -lah ~/.ssh 2>&1
    echo
    echo "Lab Public Key Fingerprint:"
    ssh-keygen -lf ~/module12-lab/lesson13/lab_ed25519.pub
    echo
    echo "Private Key Permissions:"
    stat -c '%a %n' ~/module12-lab/lesson13/lab_ed25519
    echo
    echo "SSH Listening Ports:"
    ss -tuln | grep ':22' || echo "No local listener detected"
} > ssh-report.txt
```

Display your report:

```bash
cat ssh-report.txt
```

Your report should be saved at:

```text
~/module12-lab/lesson13/ssh-report.txt
```

---

## 16. Cybersecurity Connection: SSH and Remote Access

SSH is widely used in cybersecurity and system administration.

Security professionals use SSH to:

- Administer authorised Linux servers.
- Transfer investigation files.
- Manage remote security tools.
- Inspect system logs.
- Perform incident response.
- Access isolated laboratory systems.

However, attackers may also attempt to abuse exposed SSH services.

Common SSH-related threats include:

- Password guessing.
- Stolen private keys.
- Unauthorised account access.
- Man-in-the-middle attacks.
- Misconfigured root access.
- Weak access-control policies.

### Defensive investigation

Administrators should:

1. Monitor authentication activity.
2. Investigate repeated failed login attempts.
3. Verify host-key changes.
4. Review authorised accounts.
5. Protect private keys.
6. Update SSH software.
7. Restrict unnecessary network access.

Only test SSH access on systems you own or have explicit permission to administer.

---

## 17. Lesson Summary

You learned:

1. Remote administration allows computers to be managed over networks.
2. SSH stands for Secure Shell.
3. SSH commonly uses TCP port 22.
4. SSH provides encrypted remote communication.
5. SSH clients connect to SSH servers.
6. SSH supports password and public-key authentication.
7. Ed25519 is a modern SSH key type.
8. Private keys must remain secret.
9. `known_hosts` helps identify remote servers.
10. SCP and SFTP support secure file transfers.
11. SSH configuration affects client and server behaviour.
12. SSH security requires strong authentication and access control.

---

## 18. Knowledge Check

1. What does SSH stand for?
2. What is the default SSH port?
3. What is the difference between an SSH client and an SSH server?
4. What is the purpose of SSH encryption?
5. What is public-key authentication?
6. What is the difference between a private key and a public key?
7. What is the purpose of `known_hosts`?
8. What is the purpose of `sshd_config`?
9. What is the difference between SCP and SFTP?
10. Why should private keys be protected with passphrases?
11. Why should SSH host-key changes be investigated?
12. Why is remote-access security important?

---

## 19. Practical Completion Checklist

- [ ] I understand remote system administration.
- [ ] I understand SSH client-server communication.
- [ ] I inspected the SSH client.
- [ ] I understand SSH authentication methods.
- [ ] I generated an Ed25519 key pair.
- [ ] I inspected my public key.
- [ ] I verified the public-key fingerprint.
- [ ] I checked private-key permissions.
- [ ] I inspected SSH configuration information.
- [ ] I understand SCP and SFTP.
- [ ] I generated my SSH administration report.
- [ ] I understand SSH security risks and defensive practices.

**Lesson 13 is complete when you understand the concepts, generate your SSH key pair, and successfully produce your administration report.**
