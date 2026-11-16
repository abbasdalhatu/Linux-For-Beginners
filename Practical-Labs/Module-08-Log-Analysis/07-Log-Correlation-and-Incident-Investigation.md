
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 07: LOG CORRELATION AND INCIDENT INVESTIGATION

## 1. Introduction

During a cybersecurity incident, important evidence is usually distributed across multiple log sources.

For example:

- Authentication logs record login attempts.
- SSH logs record remote-access activity.
- System logs record operating system events.
- Sudo logs record privilege-related activity.
- Firewall logs record network traffic decisions.
- Application logs record application behavior.
- Kernel logs record low-level system events.

Examining only one log source may not reveal the complete incident.

**Log correlation** is the process of comparing events from multiple sources to identify relationships and understand what happened.

This technique is essential in Security Operations Centers (SOCs), digital forensics, and incident response.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain log correlation.
2. Identify important Linux log sources.
3. Collect relevant log events.
4. Compare timestamps across logs.
5. Construct an incident timeline.
6. Correlate authentication and privilege events.
7. Understand time zones and clock synchronization.
8. Preserve investigation evidence.
9. Distinguish confirmed facts from assumptions.
10. Produce a structured incident investigation report.

## 3. Understanding Log Correlation

Imagine that a Linux server experiences suspicious activity.

Different logs record different parts of the event.

| Log source | Information |
|---|---|
| Authentication logs | Login attempts |
| SSH logs | Remote authentication |
| Sudo logs | Privileged commands |
| System logs | Operating system events |
| Kernel logs | Kernel-level events |
| Firewall logs | Network filtering activity |
| Application logs | Application behavior |

When these records are examined together, investigators can build a more complete understanding of an incident.

### Example

```text
09:00 - Failed SSH login
09:01 - Failed SSH login
09:02 - Successful SSH login
09:03 - Sudo command executed
09:04 - New service started
```

Individually, these events may appear unrelated.

Together, they may describe a sequence that requires investigation.

## 4. Practical Exercise 1: Identify Available Log Sources

Inspect the log directory:

```bash
sudo ls -lah /var/log/
```

Look for:

```text
auth.log
syslog
kern.log
boot.log
```

Not every Linux distribution uses these exact files.

Inspect journal availability:

```bash
sudo journalctl --disk-usage
```

Check the system journal:

```bash
sudo journalctl -n 30 --no-pager
```

Record:

- Available log files.
- Available journal records.
- Any missing sources.
- Relevant observations.

## 5. Practical Exercise 2: Collect Authentication Events

Search for successful authentication:

```bash
sudo journalctl --no-pager | grep -i 'accepted'
```

Search for failed authentication:

```bash
sudo journalctl --no-pager | grep -iE 'failed password|authentication failure'
```

Search for invalid usernames:

```bash
sudo journalctl --no-pager | grep -i 'invalid user'
```

If available, inspect the authentication log:

```bash
sudo grep -iE 'accepted|failed password|invalid user' /var/log/auth.log
```

Record relevant:

- Timestamps.
- Usernames.
- Source addresses.
- Authentication methods.
- Service names.

## 6. Practical Exercise 3: Collect Sudo Activity

Sudo allows authorized users to execute commands with elevated privileges.

These activities can be important during an investigation.

Search the journal:

```bash
sudo journalctl --no-pager | grep -i sudo
```

Search the authentication log:

```bash
sudo grep -i sudo /var/log/auth.log
```

Example:

```text
Nov 15 09:03:00 server sudo: analyst : COMMAND=/usr/bin/id
```

Interpretation:

- User: analyst.
- Command: `/usr/bin/id`.
- Timestamp: 09:03.

Investigate whether the command was expected.

Remember that the exact information recorded depends on system configuration.

## 7. Practical Exercise 4: Inspect System and Kernel Events

System events may provide context about what happened before or after an authentication event.

Inspect recent system warnings:

```bash
sudo journalctl -p warning --since today --no-pager
```

Inspect kernel messages:

```bash
sudo journalctl -k -n 50 --no-pager
```

Inspect service failures:

```bash
sudo journalctl --no-pager | grep -iE 'failed|failure|error'
```

Look for:

- Service startup.
- Service shutdown.
- Unexpected errors.
- Kernel warnings.
- Failed operations.

Do not automatically interpret every error as an attack.

## 8. Practical Exercise 5: Investigate Events Within a Time Range

Time filtering helps investigators narrow down relevant events.

Display events from a particular date:

```bash
sudo journalctl --since "2026-11-15" --until "2026-11-16" --no-pager
```

Display events from a particular time range:

```bash
sudo journalctl --since "09:00:00" --until "10:00:00" --no-pager
```

Inspect authentication-related records within a time range:

```bash
sudo journalctl --since "09:00:00" --until "10:00:00" --no-pager | grep -iE 'ssh|sudo|authentication'
```

The example date and times are illustrative. Use dates and times appropriate to your own logs.

Record:

1. Investigation start time.
2. Investigation end time.
3. Relevant events.
4. Events requiring further examination.

## 9. Practical Exercise 6: Understand Time Zones

Timestamps are essential when correlating logs.

However, different systems may record time differently.

For example:

- UTC.
- Local system time.
- Application-specific time.
- Time without an explicit time zone.

Inspect the system clock:

```bash
date
```

Display UTC:

```bash
date -u
```

Inspect time synchronization:

```bash
timedatectl status
```

Look for:

- Local time.
- Universal time.
- Time zone.
- System clock synchronization.
- NTP service status.

### Why Time Zones Matter

Imagine that an authentication log records an event in UTC while an application log records local time.

Without accounting for the difference, an analyst may incorrectly conclude that the events occurred at different times.

Always normalize timestamps before constructing a timeline.

## 10. Practical Exercise 7: Inspect Firewall and Network Evidence

Firewall logs may help explain whether network connections were allowed or blocked.

Inspect available firewall logging information:

```bash
sudo journalctl --no-pager | grep -iE 'ufw|nftables|iptables|firewall'
```

If UFW is installed, inspect its status:

```bash
sudo ufw status verbose
```

If the system uses nftables, inspect the ruleset:

```bash
sudo nft list ruleset
```

These commands inspect existing information.

Do not change firewall rules during this lab.

Record:

- Whether firewall logging is available.
- Any relevant allowed or blocked events.
- Associated timestamps.
- Relevant addresses and ports.

A missing firewall log entry does not establish that no connection occurred.

## 11. Practical Exercise 8: Build an Incident Timeline

A timeline organizes events chronologically.

Consider this fictional evidence:

```text
09:00:00 SSH failed password for admin from 198.51.100.20
09:01:00 SSH failed password for admin from 198.51.100.20
09:02:00 SSH accepted publickey for analyst from 203.0.113.10
09:03:00 Sudo command executed by analyst: /usr/bin/id
09:04:00 Service example-worker started
09:05:00 Firewall logged a connection to port 8080
```

Create a table:

| Time | Source | Event | Interpretation |
|---|---|---|---|
| 09:00 | SSH | Failed login | Authentication failure |
| 09:01 | SSH | Failed login | Repeated failure |
| 09:02 | SSH | Successful login | Analyst authenticated |
| 09:03 | Sudo | Command executed | Privileged activity |
| 09:04 | System | Service started | Service activity |
| 09:05 | Firewall | Port 8080 connection | Network activity |

### Investigation Questions

1. Which event occurred first?
2. Which account authenticated successfully?
3. Was the successful login from the same address as the failed attempts?
4. What happened after authentication?
5. Which events should be investigated together?
6. What evidence would help establish whether the service startup was authorized?

## 12. Practical Exercise 9: Evidence Preservation

Incident investigation requires careful evidence handling.

Evidence should be protected against accidental modification.

Important principles:

### A. Preserve Original Evidence

Avoid editing original logs.

### B. Document Collection

Record:

- Evidence source.
- Collection date and time.
- Person collecting the evidence.
- Collection method.
- File name.

### C. Create Working Copies

Analyze copies rather than modifying original evidence.

### D. Calculate Integrity Hashes

A cryptographic hash helps identify whether a file has changed.

For example:

```bash
sha256sum evidence.log
```

The output contains a SHA-256 hash.

If you have a copied evidence file:

```bash
sha256sum evidence-copy.log
```

Compare the hashes.

Matching hashes indicate that the files have the same content with extremely high confidence.

A hash does not prove who created the file or whether the original evidence was trustworthy.

### E. Maintain Chain of Custody

Chain of custody documents who collected, handled, transferred, and examined evidence.

This helps maintain accountability and evidence integrity.

## 13. Practical Exercise 10: Complete a Fictional Incident Investigation

An organization reports suspicious activity on a Linux server.

The investigation team provides these fictional records:

### Authentication Log

```text
08:55:00 Failed password for admin from 192.0.2.40
08:56:00 Failed password for admin from 192.0.2.40
08:57:00 Invalid user test from 192.0.2.40
09:00:00 Accepted publickey for analyst from 203.0.113.15
```

### Sudo Log

```text
09:02:00 analyst executed /usr/bin/id
09:03:00 analyst executed /usr/bin/systemctl status cron
```

### System Log

```text
09:04:00 example-worker.service started
09:05:00 example-worker.service reported warning
```

### Firewall Log

```text
09:06:00 Connection allowed to TCP port 8080
```

All IP addresses are documentation-only examples.

### Investigation Tasks

**Task A: Build the Timeline**

Arrange every event chronologically.

**Task B: Identify Authentication Activity**

Identify failed attempts and successful authentication.

**Task C: Correlate User Activity**

Determine which sudo events occurred after the successful login.

**Task D: Investigate Service Activity**

Identify when the service started and when the warning appeared.

**Task E: Examine Network Activity**

Identify the firewall event and its timestamp.

**Task F: Assess the Incident**

Answer:

1. What activity appears suspicious?
2. Which events may be related?
3. What is confirmed by the supplied evidence?
4. What remains uncertain?
5. What additional evidence should be collected?

**Task G: Recommend Actions**

Provide five defensive recommendations.

Do not assume that the successful login, service startup, and firewall event are connected without supporting evidence.

## 14. Practical Exercise 11: Create an Incident Investigation Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module08/lab07
```

Enter it:

```bash
cd ~/practical-labs/module08/lab07
```

Create the report:

```bash
nano log-correlation-investigation-report.txt
```

Use this template:

```text
==================================================
MODULE 08 - LAB 07
LOG CORRELATION AND INCIDENT INVESTIGATION
==================================================

1. INVESTIGATION DETAILS

Investigation Name:
System:
Investigation Date:
Investigation Time Range:

2. LOG SOURCES

Authentication Logs:
SSH Logs:
Sudo Logs:
System Logs:
Kernel Logs:
Firewall Logs:
Other Sources:

3. TIMELINE

Event 1:
Timestamp:
Source:
Description:

Event 2:
Timestamp:
Source:
Description:

Event 3:
Timestamp:
Source:
Description:

Event 4:
Timestamp:
Source:
Description:

4. EVENT CORRELATION

Related Events:
Common Accounts:
Source Addresses:
Related Services:
Observations:

5. TIME ANALYSIS

System Time Zone:
UTC Time:
Synchronization Status:
Timestamp Differences:

6. EVIDENCE PRESERVATION

Evidence Files:
Collection Method:
SHA-256 Hashes:
Chain of Custody Notes:

7. INCIDENT ASSESSMENT

Confirmed Facts:
Suspicious Indicators:
Unverified Hypotheses:
Evidence Limitations:
Confidence Level:

8. RECOMMENDATIONS

1.
2.
3.
4.
5.

9. CONCLUSION

Investigation Summary:
Further Investigation Required:

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 15. Knowledge Check

1. What is log correlation?
2. Why should analysts examine multiple log sources?
3. What information can authentication logs provide?
4. Why is sudo activity important?
5. What does `journalctl --since` do?
6. Why must time zones be considered?
7. What is an incident timeline?
8. What is evidence preservation?
9. What is a SHA-256 hash used for?
10. What does chain of custody mean?

## 16. Lab Completion Checklist

- [ ] Identified available log sources.
- [ ] Collected authentication events.
- [ ] Examined sudo activity.
- [ ] Inspected system and kernel events.
- [ ] Filtered logs by time.
- [ ] Checked system time and synchronization.
- [ ] Inspected firewall evidence.
- [ ] Built an incident timeline.
- [ ] Studied evidence preservation.
- [ ] Analyzed the fictional incident.
- [ ] Created the investigation report.
- [ ] Completed the knowledge check.

## 17. Expected Outcome

You should now understand how to combine evidence from different Linux log sources to investigate security incidents.

You have also learned the importance of accurate timestamps, evidence preservation, cryptographic hashes, and professional incident documentation.

**Safety reminder:** This lab is inspection-only. Do not edit original logs, disable logging, modify firewall rules, or interfere with running services.
