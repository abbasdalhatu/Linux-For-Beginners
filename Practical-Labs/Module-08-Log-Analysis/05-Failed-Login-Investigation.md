
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 05: FAILED LOGIN INVESTIGATION

## 1. Introduction

Failed login investigation is an important activity in cybersecurity.

Every time someone attempts to authenticate to a Linux system, the operating system may record information about that attempt.

These records can help security analysts identify:

- Repeated failed authentication.
- Attempts to access nonexistent accounts.
- Password guessing activity.
- Suspicious login patterns.
- Unexpected authentication activity.
- Possible unauthorized access.

A single failed login does not necessarily indicate an attack. Users can mistype passwords, forget credentials, or use outdated application settings.

The goal of an investigation is to examine the available evidence before reaching a conclusion.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Locate failed authentication records.
2. Search authentication logs.
3. Identify repeated failed login attempts.
4. Examine attempted usernames.
5. Identify source IP addresses.
6. Analyze login frequency and timing.
7. Correlate failed and successful authentication.
8. Distinguish suspicious patterns from confirmed compromise.
9. Produce a failed-login investigation report.

## 3. Understanding Failed Authentication

Authentication is the process of verifying a user's identity.

A failed authentication event occurs when an authentication attempt is unsuccessful.

Examples include:

- Incorrect password.
- Invalid username.
- Expired credentials.
- Incorrect SSH key.
- Account restrictions.
- Authentication policy rejection.

Linux systems may record these events in:

```text
/var/log/auth.log
```

Some Linux distributions use:

```text
/var/log/secure
```

Systems using systemd may also store authentication events in the system journal.

## 4. Practical Exercise 1: Locate Authentication Logs

Check whether the authentication log exists:

```bash
sudo ls -lah /var/log/auth.log
```

Check for an alternative:

```bash
sudo ls -lah /var/log/secure
```

Inspect authentication-related journal records:

```bash
sudo journalctl -t sshd --no-pager
```

You can also search the complete journal:

```bash
sudo journalctl --no-pager | grep -i authentication
```

Record which sources are available.

**Important:** A missing log file does not necessarily indicate a problem. Different distributions use different logging configurations.

## 5. Practical Exercise 2: Search Failed Password Events

Search the authentication log:

```bash
sudo grep -i 'failed password' /var/log/auth.log
```

If the file exists, display only the latest 20 matching records:

```bash
sudo grep -i 'failed password' /var/log/auth.log | tail -n 20
```

Search the system journal:

```bash
sudo journalctl --no-pager | grep -i 'failed password'
```

Observe:

- Timestamp.
- Username.
- Source IP address.
- Service involved.
- Number of matching events.

If no records appear, document that no matching events were available.

## 6. Practical Exercise 3: Search Invalid Username Attempts

An attacker may attempt to access accounts that do not exist.

Search for invalid usernames:

```bash
sudo grep -i 'invalid user' /var/log/auth.log
```

Search the journal:

```bash
sudo journalctl --no-pager | grep -i 'invalid user'
```

Record:

1. Attempted username.
2. Source address.
3. Timestamp.
4. Authentication service.

An invalid username attempt can be suspicious, but it is not proof of malicious activity.

## 7. Practical Exercise 4: Examine Failed Login History

Linux may maintain unsuccessful login records through the `btmp` accounting database.

Inspect it using:

```bash
sudo lastb
```

Display a limited number of entries:

```bash
sudo lastb -n 10
```

If the file is available, inspect its metadata:

```bash
sudo ls -lah /var/log/btmp
```

Understand the difference:

| Command | Purpose |
|---|---|
| `last` | Displays recorded login history |
| `lastb` | Displays recorded failed login attempts |
| `who` | Shows currently logged-in users |
| `w` | Displays logged-in users and their activity |

Some systems may not have failed-login accounting enabled.

Do not treat missing records as proof that no failed attempts occurred.

## 8. Practical Exercise 5: Identify Repeated Login Attempts

Security analysts look for patterns rather than isolated events.

Consider these fictional events:

```text
10:00:00 Failed password for admin from 192.0.2.50
10:00:04 Failed password for admin from 192.0.2.50
10:00:08 Failed password for root from 192.0.2.50
10:00:12 Failed password for test from 192.0.2.50
10:00:16 Failed password for guest from 192.0.2.50
```

Answer:

1. How many failed attempts occurred?
2. Which source IP generated the events?
3. How many different usernames were attempted?
4. What is the time interval between consecutive attempts?
5. Why might this pattern require investigation?

### Important Indicators

- Many failed attempts within a short period.
- Multiple usernames attempted from one source.
- Repeated attempts against privileged accounts.
- Repeated attempts outside expected working hours.
- Failed attempts followed by an unexpected successful login.

These are indicators for investigation, not automatic proof of compromise.

## 9. Practical Exercise 6: Investigate Attempted Usernames

Search for failed password events involving a particular account:

```bash
sudo grep -i 'failed password for admin' /var/log/auth.log
```

Search for root-related authentication failures:

```bash
sudo grep -i 'failed password for root' /var/log/auth.log
```

Search for invalid users:

```bash
sudo grep -i 'invalid user' /var/log/auth.log
```

Record:

- Accounts targeted.
- Whether privileged accounts were targeted.
- Whether nonexistent accounts were attempted.
- Whether the same username appears repeatedly.

Consider why attackers may target common account names such as `root`, `admin`, and `test`.

## 10. Practical Exercise 7: Investigate Source IP Addresses

An IP address may help investigators identify where an authentication attempt originated from, as observed by the server.

Search for a fictional or observed address:

```bash
sudo grep '192.0.2.50' /var/log/auth.log
```

Replace the example address with an address found in your own available records.

To count matching lines:

```bash
sudo grep -c '192.0.2.50' /var/log/auth.log
```

To display matching events:

```bash
sudo grep '192.0.2.50' /var/log/auth.log | tail -n 20
```

Investigate:

1. How many records contain the address?
2. Which accounts were targeted?
3. When did the activity occur?
4. Was there any successful authentication from that address?

**Important:** An observed source IP does not always identify the individual responsible. NAT, proxies, VPNs, shared networks, and spoofing in some network contexts can complicate attribution.

## 11. Practical Exercise 8: Correlate Failed and Successful Logins

One of the most important investigation techniques is event correlation.

Consider this fictional timeline:

```text
09:00 Failed password for admin from 192.0.2.50
09:01 Failed password for admin from 192.0.2.50
09:02 Failed password for root from 192.0.2.50
09:03 Accepted password for admin from 192.0.2.50
09:05 sudo: admin : COMMAND=/usr/bin/id
```

Analyze the sequence.

Questions:

1. What happened at 09:00?
2. What happened at 09:02?
3. What changed at 09:03?
4. What happened at 09:05?
5. Why should the successful login be investigated?
6. What additional evidence would help establish whether access was authorized?

### Investigation Principle

A successful login after repeated failures deserves attention.

However, it is not sufficient by itself to conclude that an account was compromised.

Verify the account owner's activity, source, authentication method, and subsequent commands.

## 12. Practical Exercise 9: Analyze a Fictional Incident

An organization reports unusual authentication activity.

The following fictional log records are supplied:

```text
08:30:00 Failed password for admin from 198.51.100.20
08:30:05 Failed password for admin from 198.51.100.20
08:30:10 Failed password for root from 198.51.100.20
08:30:15 Invalid user oracle from 198.51.100.20
08:31:00 Failed password for test from 198.51.100.20
08:32:00 Accepted publickey for analyst from 203.0.113.10
08:33:00 sudo: analyst : COMMAND=/usr/bin/whoami
08:35:00 Failed password for admin from 198.51.100.20
```

All IP addresses in this scenario are documentation-only example addresses.

### Investigation Tasks

**Task A: Count Events**

Count the failed password events.

Count the invalid username events.

**Task B: Identify Sources**

Identify the source responsible for repeated failures.

Identify the source associated with the successful login.

**Task C: Identify Accounts**

List every attempted username.

Identify the account that authenticated successfully.

**Task D: Build a Timeline**

Arrange the events chronologically.

**Task E: Assess Risk**

Explain why the repeated attempts deserve investigation.

Determine what additional evidence is required.

**Task F: Recommend Actions**

Suggest five defensive measures for the organization.

## 13. Practical Exercise 10: Create an Investigation Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module08/lab05
```

Enter it:

```bash
cd ~/practical-labs/module08/lab05
```

Create the report:

```bash
nano failed-login-investigation-report.txt
```

Use this template:

```text
==================================================
MODULE 08 - LAB 05
FAILED LOGIN INVESTIGATION REPORT
==================================================

1. INVESTIGATION DETAILS

Investigation Name:
Date:
System:
Log Sources:

2. FAILED AUTHENTICATION EVENTS

Total Events:
Observed Time Range:
Authentication Service:

3. TARGETED ACCOUNTS

Account 1:
Account 2:
Account 3:
Other Accounts:

4. SOURCE IP ANALYSIS

Source Address:
Number of Events:
Associated Accounts:
Observations:

5. TIMELINE

Event 1:
Event 2:
Event 3:
Event 4:

6. SUCCESSFUL LOGIN CORRELATION

Successful Authentication Found:
Account:
Source Address:
Authentication Method:
Related Activity:

7. SECURITY ASSESSMENT

Suspicious Patterns:
Potential Impact:
Evidence Limitations:
Confidence Level:

8. RECOMMENDATIONS

1.
2.
3.
4.
5.

9. CONCLUSION

Summary:
Further Investigation Required:

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 14. Important Defensive Recommendations

When an organization identifies suspicious failed-login activity, its security team may:

1. Verify the affected account with its owner.
2. Review successful authentication records.
3. Check privileged command activity.
4. Apply appropriate account protection measures.
5. Restrict unnecessary remote access.
6. Review firewall and authentication policies.
7. Enable appropriate alerting and monitoring.
8. Preserve relevant evidence.

Any containment action should follow the organization's incident-response procedure.

## 15. Knowledge Check

1. What is a failed authentication event?
2. Where can Linux authentication events be recorded?
3. What does `lastb` display?
4. What does an invalid username event indicate?
5. Why are repeated failed attempts suspicious?
6. How can you search for failed SSH passwords?
7. Why should successful logins be correlated with failed attempts?
8. What information can a source IP provide?
9. Why is a failed login not proof of compromise?
10. What is the purpose of an investigation report?

## 16. Lab Completion Checklist

- [ ] Located authentication log sources.
- [ ] Searched failed password events.
- [ ] Identified invalid username attempts.
- [ ] Inspected failed login history.
- [ ] Examined repeated login patterns.
- [ ] Investigated attempted usernames.
- [ ] Examined source IP addresses.
- [ ] Correlated failed and successful logins.
- [ ] Analyzed the fictional incident.
- [ ] Created the investigation report.
- [ ] Completed the knowledge check.

## 17. Expected Outcome

You should now be able to investigate failed authentication activity using Linux logs.

You should understand how to identify repeated login attempts, examine source addresses, investigate targeted accounts, and correlate authentication events.

**Safety reminder:** This lab uses read-only inspection and fictional incident data. Do not generate repeated failed logins, perform password guessing, or investigate systems without authorization.
