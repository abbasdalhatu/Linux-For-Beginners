
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 04: SSH SECURITY MONITORING

## 1. Introduction

SSH stands for Secure Shell.

It is a network protocol used to securely access and administer remote systems.

Linux administrators commonly use SSH to:

- Manage remote servers.
- Execute commands remotely.
- Transfer files securely.
- Administer network infrastructure.
- Perform authorized system maintenance.

Because SSH provides remote access, it is an important target for attackers.

Security analysts monitor SSH logs to identify suspicious authentication attempts, unusual connections, and possible unauthorized access.

In this lab, you will learn how to inspect SSH activity and investigate SSH-related security events.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain SSH and its security importance.
2. Identify SSH service information.
3. Inspect SSH-related logs.
4. Recognize successful and failed SSH authentication.
5. Identify suspicious connection patterns.
6. Understand SSH authentication methods.
7. Inspect SSH configuration safely.
8. Prepare an SSH security monitoring report.

## 3. Understanding SSH

SSH is a cryptographic network protocol used for secure communication between computers.

Its default TCP port is:

```text id="mof8la"
22
```

Example SSH command:

```bash id="sk7rca"
ssh username@server-address
```

This command attempts to establish an SSH connection to the specified server.

SSH provides:

- Encryption.
- Server authentication.
- User authentication.
- Data integrity.
- Secure remote administration.

Never attempt to access a system through SSH without authorization.

## 4. Common SSH Authentication Methods

### A. Password Authentication

The user provides an account password.

### B. Public-Key Authentication

SSH uses a cryptographic key pair:

- Private key: kept secret by the user.
- Public key: placed on the remote server.

Public-key authentication is commonly preferred for secure administration when configured correctly.

### C. Multi-Factor Authentication

An additional authentication factor is required beyond the primary credential.

This provides another layer of protection.

## 5. Practical Exercise 1: Check SSH Service Availability

Check whether the SSH service exists:

```bash id="jhwg19"
systemctl list-unit-files | grep -E '^ssh(d)?\.service'
```

Check whether it is currently running:

```bash id="xck2zm"
systemctl status ssh
```

If the service is not found, inspect:

```bash id="r8b3n2"
systemctl status sshd
```

A Kali installation may not have the SSH server installed or running by default.

This is normal.

Do not install or enable SSH just to complete this inspection exercise.

Record:

- Whether the SSH service exists.
- Whether it is active.
- Whether it is enabled.
- Any relevant service messages.

## 6. Practical Exercise 2: Inspect SSH Listening Ports

Run:

```bash id="ogq4ub"
sudo ss -tlnp | grep ':22'
```

This checks for TCP listeners associated with port 22.

If SSH is listening, identify:

- Local IP address.
- Port number.
- Process name.
- Process ID, if available.

Interpretation:

```text id="w5v4ft"
127.0.0.1:22
```

Indicates a listener bound to the IPv4 loopback interface.

```text id="7o2v4b"
0.0.0.0:22
```

Indicates a listener bound to all IPv4 interfaces.

A listening port does not automatically mean that it is reachable from another system. Routing and firewall policies also matter.

## 7. Practical Exercise 3: Locate SSH Authentication Logs

Check for the authentication log:

```bash id="tfr0vp"
sudo ls -lah /var/log/auth.log
```

If it exists:

```bash id="0eh89g"
sudo grep -i sshd /var/log/auth.log | tail -n 30
```

Alternatively, inspect the journal:

```bash id="x1aqiw"
sudo journalctl --no-pager | grep -i sshd
```

Inspect the SSH service journal if the service exists:

```bash id="u15o2c"
sudo journalctl -u ssh --no-pager
```

If the service is called `sshd`:

```bash id="l67o3s"
sudo journalctl -u sshd --no-pager
```

Record which logging source is available.

## 8. Practical Exercise 4: Understand Important SSH Log Events

Security analysts commonly investigate the following SSH messages.

| Log message | Meaning |
|---|---|
| Accepted password | Password authentication succeeded |
| Accepted publickey | Public-key authentication succeeded |
| Failed password | Password authentication failed |
| Invalid user | Attempted username does not exist |
| Connection closed | SSH connection was closed |
| Connection reset | Connection was unexpectedly reset |
| Authentication failure | Authentication was unsuccessful |

Study these fictional examples.

### Example A: Successful SSH Login

```text id="1w6n8p"
Nov 15 10:00:00 server sshd[1500]: Accepted publickey for analyst from 192.0.2.10 port 50000 ssh2
```

Interpretation:

- Authentication succeeded.
- Account: analyst.
- Method: public key.
- Source: 192.0.2.10.

### Example B: Failed SSH Login

```text id="r2q6nv"
Nov 15 10:02:00 server sshd[1505]: Failed password for admin from 192.0.2.50 port 50001 ssh2
```

Interpretation:

- Authentication failed.
- Account attempted: admin.
- Source: 192.0.2.50.

### Example C: Invalid Username

```text id="5t64qj"
Nov 15 10:03:00 server sshd[1510]: Invalid user guest from 192.0.2.70 port 50002
```

Interpretation:

- An account named guest was attempted.
- The system reported that the account was invalid.

These IP addresses are fictional documentation addresses.

## 9. Practical Exercise 5: Search for Successful SSH Authentication

Run:

```bash id="p1oj4p"
sudo journalctl --no-pager | grep -i 'accepted'
```

Narrow the search to SSH:

```bash id="13hjui"
sudo journalctl --no-pager | grep -i 'sshd' | grep -i 'accepted'
```

If `/var/log/auth.log` exists:

```bash id="qfdm54"
sudo grep -i 'accepted' /var/log/auth.log
```

Record:

- Username.
- Timestamp.
- Authentication method.
- Source IP, if present.

No matching records may simply mean that no SSH login occurred during the available logging period.

## 10. Practical Exercise 6: Search for Failed SSH Attempts

Run:

```bash id="a92avk"
sudo journalctl --no-pager | grep -i 'failed password'
```

Search for invalid usernames:

```bash id="bc0n22"
sudo journalctl --no-pager | grep -i 'invalid user'
```

If authentication logs are available:

```bash id="mtw0fy"
sudo grep -iE 'failed password|invalid user' /var/log/auth.log
```

Investigate:

1. How many matching events exist?
2. Are multiple usernames involved?
3. Do the events originate from the same address?
4. Do the events occur close together?
5. Did a successful login follow?

Do not assume that every failed login is an attack.

## 11. Practical Exercise 7: Inspect SSH Configuration

The main SSH server configuration file is usually:

```text id="b8vw0j"
/etc/ssh/sshd_config
```

Inspect it if it exists:

```bash id="3sw1oq"
sudo ls -lah /etc/ssh/sshd_config
```

Read the configuration:

```bash id="3z6l3c"
sudo less /etc/ssh/sshd_config
```

Press `q` to exit.

Look for these directives:

| Directive | Purpose |
|---|---|
| Port | SSH listening port |
| PermitRootLogin | Controls direct root login |
| PasswordAuthentication | Controls password authentication |
| PubkeyAuthentication | Controls public-key authentication |
| AllowUsers | Restricts permitted accounts |
| MaxAuthTries | Limits authentication attempts per connection |

Important:

A commented configuration line beginning with `#` may indicate a default or example setting rather than an active explicit setting.

Effective SSH configuration can also be inspected with:

```bash id="z3m9ba"
sudo sshd -T
```

This command requires the SSH server's configuration utility to be installed.

Do not edit or reload SSH configuration during this lab.

## 12. Practical Exercise 8: Understand SSH Security Hardening

Consider these defensive practices.

### 1. Prefer Public-Key Authentication

Use appropriately protected SSH keys.

### 2. Protect Private Keys

Private keys should not be shared or stored in publicly accessible locations.

### 3. Restrict Administrative Access

Permit only authorized accounts and trusted management networks where appropriate.

### 4. Protect Root Access

Avoid unnecessary direct root login.

### 5. Use Strong Authentication

Combine secure credentials with additional authentication factors where possible.

### 6. Monitor Authentication Events

Review successful and failed login activity.

### 7. Maintain Software Updates

Keep SSH software patched.

### 8. Limit Exposure

Do not expose administrative services unnecessarily.

## 13. Practical Exercise 9: Analyze a Fictional SSH Security Incident

An organization reports unusual SSH activity.

The following events were recorded:

```text id="b3vyq9"
10:00:00 Failed password for admin from 192.0.2.50
10:00:04 Failed password for admin from 192.0.2.50
10:00:08 Failed password for root from 192.0.2.50
10:00:12 Failed password for test from 192.0.2.50
10:01:00 Accepted publickey for analyst from 203.0.113.25
10:02:00 Failed password for guest from 192.0.2.50
```

### Investigation Questions

1. Which source generated repeated failed attempts?
2. How many failed authentication events are shown?
3. Which account successfully authenticated?
4. What authentication method was used for the successful login?
5. Why is the repeated activity suspicious?
6. What additional logs should be examined?
7. What defensive actions should the organization consider?

### Recommended Investigation

- Correlate events by source IP.
- Compare timestamps.
- Verify the successful login with the account owner.
- Inspect sudo activity.
- Review other system logs.
- Check whether account privileges were changed.
- Preserve relevant records.

Do not claim that the server was compromised based only on these events.

## 14. Practical Exercise 10: Create an SSH Monitoring Report

Create your lab directory:

```bash id="x7ndxj"
mkdir -p ~/practical-labs/module08/lab04
```

Enter it:

```bash id="24r6wu"
cd ~/practical-labs/module08/lab04
```

Create your report:

```bash id="5br3oo"
nano ssh-security-monitoring-report.txt
```

Use this template:

```text id="ps8p4v"
==================================================
MODULE 08 - LAB 04
SSH SECURITY MONITORING
==================================================

1. SSH SERVICE STATUS

Service Available:
Service State:
Service Enabled:

2. SSH NETWORK INSPECTION

Listening Port:
Local Address:
Process:
Observations:

3. SSH LOG SOURCES

Authentication Log:
Journal Records:
Available Evidence:

4. SUCCESSFUL AUTHENTICATION

Timestamp:
Username:
Authentication Method:
Source Address:

5. FAILED AUTHENTICATION

Timestamp:
Username:
Source Address:
Number of Events:

6. SSH CONFIGURATION REVIEW

Configuration File:
Root Login Setting:
Password Authentication:
Public-Key Authentication:
Other Security Settings:

7. INCIDENT ANALYSIS

Repeated Attempts:
Suspicious Source:
Successful Login:
Possible Explanation:

8. RECOMMENDATIONS

1.
2.
3.
4.
5.

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 15. Knowledge Check

1. What does SSH stand for?
2. What is the default SSH port?
3. What is the difference between password and public-key authentication?
4. What does `sshd` represent?
5. What does an accepted-publickey event mean?
6. What does a failed-password event indicate?
7. Where is the main SSH server configuration file?
8. What is the purpose of `PermitRootLogin`?
9. Why should SSH logs be monitored?
10. Why is a failed login not sufficient evidence of compromise?

## 16. Lab Completion Checklist

- [ ] Checked SSH service availability.
- [ ] Inspected TCP port 22.
- [ ] Located SSH authentication logs.
- [ ] Identified successful authentication events.
- [ ] Identified failed authentication events.
- [ ] Inspected SSH configuration.
- [ ] Reviewed SSH hardening principles.
- [ ] Analyzed the fictional incident.
- [ ] Created `ssh-security-monitoring-report.txt`.
- [ ] Completed the knowledge check.

## 17. Expected Outcome

You should now understand how SSH authentication activity is recorded and how security analysts investigate suspicious remote-access events.

You should also understand how secure authentication, access restrictions, and log monitoring contribute to server protection.

**Safety reminder:** This lab is inspection-only. Do not enable SSH, modify its configuration, expose port 22, or attempt authentication against systems without explicit authorization.
