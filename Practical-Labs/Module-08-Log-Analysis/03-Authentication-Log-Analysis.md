
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 03: AUTHENTICATION LOG ANALYSIS

## 1. Introduction

Authentication is the process of verifying the identity of a user or system before granting access.

Linux uses authentication mechanisms to control access to:

- User accounts.
- SSH services.
- Sudo privileges.
- System resources.
- Applications.
- Administrative functions.

Authentication logs record important events associated with these activities.

For cybersecurity analysts, authentication logs are valuable because they can reveal suspicious login attempts, unusual account activity, and possible unauthorized access.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain Linux authentication.
2. Identify important authentication log locations.
3. Inspect successful and failed login events.
4. Understand SSH authentication records.
5. Investigate sudo activity.
6. Identify suspicious authentication patterns.
7. Search authentication logs using grep.
8. Produce an authentication investigation report.

## 3. Understanding Linux Authentication

Linux authentication verifies that a user has permission to access an account or resource.

Common authentication mechanisms include:

### A. Password Authentication

The user provides a password.

### B. SSH Key Authentication

A user authenticates using a cryptographic key pair.

### C. Sudo Authentication

A user authenticates when requesting elevated privileges, depending on sudo configuration and authentication caching.

### D. PAM Authentication

PAM stands for Pluggable Authentication Modules.

It provides a framework through which Linux applications can use configurable authentication mechanisms.

## 4. Important Authentication Files

Inspect these important locations:

| File | Purpose |
|---|---|
| `/var/log/auth.log` | Authentication events on Debian-based systems, when configured |
| `/var/log/secure` | Authentication events on some Red Hat-based systems |
| `/etc/passwd` | Account information |
| `/etc/shadow` | Password hashes and password-aging information |
| `/etc/group` | Group information |
| `/etc/pam.d/` | PAM configuration files |

Important:

`/etc/passwd` and `/etc/shadow` are account databases, not ordinary authentication event logs.

Never publish the contents of `/etc/shadow`.

## 5. Practical Exercise 1: Check Authentication Log Availability

Run:

```bash
sudo ls -lah /var/log/auth.log
```

If the file exists, inspect its latest entries:

```bash
sudo tail -n 30 /var/log/auth.log
```

If it does not exist, inspect:

```bash
sudo ls -lah /var/log/secure
```

You can also use the system journal:

```bash
sudo journalctl --no-pager | grep -iE 'authentication|login|sudo|sshd'
```

Record which logging method is available on your Kali system.

## 6. Practical Exercise 2: Understand Successful Login Events

Study this fictional log entry:

```text
Nov 15 09:20:10 kali sshd[1400]: Accepted publickey for analyst from 192.0.2.25 port 51000 ssh2
```

Interpretation:

| Field | Meaning |
|---|---|
| Nov 15 | Date |
| 09:20:10 | Time |
| kali | Hostname |
| sshd | SSH service |
| 1400 | Process ID |
| Accepted publickey | Successful key authentication |
| analyst | Username |
| 192.0.2.25 | Source IP |
| 51000 | Source port |

Questions:

1. Which service generated the event?
2. Was authentication successful?
3. Which account was used?
4. What was the source IP address?
5. Why should analysts verify whether this login was expected?

## 7. Practical Exercise 3: Understand Failed Login Events

Study this fictional entry:

```text
Nov 15 09:25:40 kali sshd[1500]: Failed password for invalid user admin from 192.0.2.50 port 52000 ssh2
```

Interpretation:

- Service: SSH.
- Event: Failed password.
- Username attempted: admin.
- Source IP: 192.0.2.50.
- Result: Authentication unsuccessful.

This could indicate a mistyped username, unauthorized access attempt, or automated activity.

A single failed login does not prove malicious activity.

Investigators should examine frequency, source, account context, and related events.

## 8. Practical Exercise 4: Search for Successful Logins

If `/var/log/auth.log` exists:

```bash
sudo grep -iE 'accepted|session opened' /var/log/auth.log
```

Alternatively, search the journal:

```bash
sudo journalctl --no-pager | grep -iE 'accepted|session opened'
```

Identify any successful authentication records.

Record:

- Timestamp.
- Username.
- Authentication method.
- Source address, if present.
- Service.

If no results appear, record that observation.

## 9. Practical Exercise 5: Search for Failed Logins

If available:

```bash
sudo grep -iE 'failed password|authentication failure|invalid user' /var/log/auth.log
```

Or use:

```bash
sudo journalctl --no-pager | grep -iE 'failed password|authentication failure|invalid user'
```

Investigate any matching events.

Record:

- Time of event.
- Username attempted.
- Source address.
- Number of matching events.
- Whether the activity appears unusual.

Do not repeatedly generate failed login attempts.

## 10. Practical Exercise 6: Inspect Sudo Activity

Sudo allows authorized users to execute commands with elevated privileges.

Authentication logs may record sudo activity.

Search available authentication records:

```bash
sudo journalctl --no-pager | grep -i sudo
```

If available:

```bash
sudo grep -i sudo /var/log/auth.log
```

Study the output.

Look for:

- Username.
- Terminal.
- Working directory.
- Command executed.
- Authentication result.

Why is this important?

Unexpected privilege elevation can indicate:

- Misuse of an authorized account.
- Unauthorized administrative activity.
- A compromised user account.
- Normal system administration.

Context is essential.

## 11. Practical Exercise 7: Inspect Login History

Linux provides commands for reviewing account login history.

### A. Last

```bash
last
```

This displays login history from the system's accounting database when available.

### B. Last with a Limited Number of Records

```bash
last -n 10
```

### C. Lastb

```bash
sudo lastb
```

This may display recorded failed login attempts if the relevant accounting data is available.

### D. Who

```bash
who
```

This displays currently logged-in users.

### E. W

```bash
w
```

This displays logged-in users and information about their activity.

Important:

These commands rely on login accounting records. Their output depends on system configuration and available historical data.

## 12. Practical Exercise 8: Understand Suspicious Authentication Patterns

Study these fictional situations.

### Scenario A: Repeated Failed Logins

An account receives 35 failed SSH login attempts within five minutes.

Possible explanations:

- Password guessing.
- Automated scanning.
- A misconfigured application.
- A legitimate user repeatedly entering an incorrect password.

Recommended investigation:

- Identify the source addresses.
- Examine timestamps.
- Check whether any successful login followed.
- Correlate with other system events.

### Scenario B: Login at an Unexpected Time

An administrative account logs in at 03:00 when the organization normally performs no maintenance.

Possible explanations:

- Scheduled maintenance.
- An authorized administrator working remotely.
- Unauthorized account use.

Recommended investigation:

- Verify the administrator's activity.
- Check the authentication method.
- Review sudo activity.
- Compare the event with approved maintenance schedules.

### Scenario C: Unknown Username

Repeated attempts use usernames that do not exist on the system.

Possible explanations:

- Automated username guessing.
- Misconfigured software.
- An incorrectly configured remote client.

Recommended investigation:

- Count attempts.
- Identify source addresses.
- Review SSH service records.
- Determine whether successful authentication occurred.

## 13. Practical Exercise 9: Create an Authentication Investigation Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module08/lab03
```

Enter it:

```bash
cd ~/practical-labs/module08/lab03
```

Create your report:

```bash
nano authentication-analysis-report.txt
```

Use this template:

```text
==================================================
MODULE 08 - LAB 03
AUTHENTICATION LOG ANALYSIS
==================================================

1. AUTHENTICATION LOG AVAILABILITY

Auth.log:
Secure Log:
Journal Records:

2. SUCCESSFUL LOGIN ANALYSIS

Timestamp:
Username:
Authentication Method:
Source Address:

3. FAILED LOGIN ANALYSIS

Timestamp:
Username Attempted:
Source Address:
Event Description:

4. SUDO ACTIVITY

User:
Command:
Timestamp:
Observations:

5. LOGIN HISTORY

Last Command Results:
Lastb Results:
Who Results:
W Results:

6. SECURITY INVESTIGATION

Suspicious Events:
Possible Explanations:
Supporting Evidence:
Additional Investigation:

7. RECOMMENDATIONS

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

## 14. Knowledge Check

1. What is authentication?
2. What is the purpose of PAM?
3. What information is stored in `/etc/passwd`?
4. Why must `/etc/shadow` be protected?
5. What does an SSH accepted-publickey event indicate?
6. What does a failed-password event indicate?
7. What is the purpose of sudo?
8. What does the `last` command display?
9. Why might repeated failed login attempts be suspicious?
10. Why should authentication events be correlated with other logs?

## 15. Lab Completion Checklist

- [ ] Checked authentication log availability.
- [ ] Inspected successful login records.
- [ ] Inspected failed login records.
- [ ] Searched for sudo activity.
- [ ] Examined login history.
- [ ] Studied suspicious authentication scenarios.
- [ ] Created `authentication-analysis-report.txt`.
- [ ] Completed the knowledge check.

## 16. Expected Outcome

You should now understand how Linux records authentication activity and how security analysts use these records to investigate account access.

You should also be able to distinguish successful authentication, failed authentication, and potentially suspicious login patterns.

Security analysts must avoid making conclusions from isolated events. Good investigations combine multiple records, timestamps, account information, and system context.
