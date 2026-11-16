
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 10: MODULE 08 FINAL PRACTICAL ASSESSMENT

## 1. Assessment Overview

Congratulations on reaching the final practical assessment of Module 08.

Throughout this module, you have studied Linux logging, authentication monitoring, SSH security monitoring, failed login investigation, process detection, log correlation, incident response, and security reporting.

This assessment combines those skills into a realistic security investigation.

You will act as a Junior Security Operations Center (SOC) Analyst investigating suspicious activity on a Linux server.

### Assessment Objectives

You must demonstrate the ability to:

1. Inspect Linux logging systems.
2. Locate authentication records.
3. Identify suspicious SSH activity.
4. Investigate failed login attempts.
5. Inspect processes and services.
6. Examine network connections.
7. Correlate events from different log sources.
8. Construct an incident timeline.
9. Assess incident severity and confidence.
10. Produce a professional security investigation report.

## 2. Assessment Rules

- Perform practical inspection only on your own Kali VM.
- Use fictional evidence for the incident investigation.
- Do not attack or scan public systems.
- Do not generate brute-force authentication attempts.
- Do not disable security controls.
- Do not delete or modify original system logs.
- Do not terminate unfamiliar processes.
- Do not change firewall or SSH configurations.

This assessment focuses on defensive cybersecurity skills.

## 3. Assessment Environment

Your practical environment:

```text
Operating System: Kali Linux
Virtualization: VMware
Host Operating System: Windows 11
Investigation Type: Defensive Security Investigation
Target: Your own Kali VM
```

Create your assessment directory:

```bash
mkdir -p ~/practical-labs/module08/assessment
```

Enter it:

```bash
cd ~/practical-labs/module08/assessment
```

Create an evidence directory:

```bash
mkdir -p evidence
```

Create a reports directory:

```bash
mkdir -p reports
```

## 4. TASK 1: Linux Logging System Inspection

**Objective:** Demonstrate knowledge of Linux logging infrastructure.

### Instructions

Inspect the available log directory:

```bash
sudo ls -lah /var/log/
```

Check journal storage:

```bash
sudo journalctl --disk-usage
```

Display recent system events:

```bash
sudo journalctl -n 30 --no-pager
```

Inspect warning-level messages:

```bash
sudo journalctl -p warning -n 20 --no-pager
```

### Questions

1. What is the purpose of `/var/log/`?
2. What is systemd-journald?
3. What does `journalctl` do?
4. Why are logs important in cybersecurity investigations?

### Expected Evidence

Record:

- Available log files.
- Journal storage information.
- Recent system events.
- Relevant warnings.

## 5. TASK 2: Authentication Investigation

**Objective:** Identify and interpret authentication events.

### Instructions

Inspect authentication records:

```bash
sudo journalctl -t sshd --no-pager
```

Search for successful authentication:

```bash
sudo journalctl --no-pager | grep -i accepted
```

Search for failed authentication:

```bash
sudo journalctl --no-pager | grep -iE 'failed password|authentication failure'
```

Search for invalid usernames:

```bash
sudo journalctl --no-pager | grep -i 'invalid user'
```

Inspect login history:

```bash
last
```

Inspect failed login history if available:

```bash
sudo lastb -n 10
```

### Questions

1. What is authentication?
2. What is the difference between successful and failed authentication?
3. What information can an authentication log reveal?
4. Why should repeated login failures be investigated?
5. Why is one failed login insufficient evidence of compromise?

### Expected Evidence

Record:

- Authentication events found.
- Relevant timestamps.
- Usernames.
- Source addresses, where available.
- Available login history.

If no matching SSH or failed-login events exist, record that clearly.

## 6. TASK 3: SSH Security Inspection

**Objective:** Examine SSH service and configuration information.

### Instructions

Check SSH service availability:

```bash
systemctl status ssh
```

If unavailable, inspect:

```bash
systemctl status sshd
```

Inspect listening TCP sockets:

```bash
sudo ss -tlnp
```

Inspect the SSH configuration file if present:

```bash
sudo less /etc/ssh/sshd_config
```

Inspect effective SSH configuration if the server utility is available:

```bash
sudo sshd -T
```

### Questions

1. What is SSH?
2. What is its default port?
3. What is public-key authentication?
4. What does `PermitRootLogin` control?
5. Why should SSH access be monitored?

### Expected Evidence

Record:

- SSH service availability.
- SSH service state.
- Relevant listening ports.
- Configuration file availability.
- Important security settings.

Do not enable SSH or change its configuration.

## 7. TASK 4: Process and Service Investigation

**Objective:** Demonstrate process and service inspection skills.

### Instructions

List running processes:

```bash
ps aux | head -n 30
```

Inspect process relationships:

```bash
ps -ef | head -n 30
```

Display the process tree:

```bash
pstree
```

Monitor processes:

```bash
top
```

Press `q` to exit.

Inspect running services:

```bash
systemctl list-units --type=service --state=running
```

Inspect systemd timers:

```bash
systemctl list-timers --all
```

### Questions

1. What is a PID?
2. What is a PPID?
3. Why are parent-child process relationships important?
4. How can suspicious processes be investigated?
5. Why should unfamiliar processes not be terminated immediately?

### Expected Evidence

Record:

- Five processes.
- Their PIDs.
- Their owners.
- Their commands.
- Relevant services.
- Any unusual observations.

## 8. TASK 5: Network Activity Inspection

**Objective:** Identify network sockets and active connections.

### Instructions

Inspect all TCP and UDP sockets:

```bash
sudo ss -tunap
```

Inspect listening TCP sockets:

```bash
sudo ss -tlnp
```

Inspect established TCP connections:

```bash
sudo ss -tnp state established
```

### Questions

1. What is a network socket?
2. What is a listening port?
3. What is an established connection?
4. Why should security analysts monitor network activity?
5. Why does a listening port not automatically indicate a security vulnerability?

### Expected Evidence

Record:

- Listening ports.
- Local addresses.
- Established connections.
- Associated processes, where available.

## 9. TASK 6: Log Correlation and Timeline Construction

**Objective:** Combine evidence from multiple sources.

Use the following fictional incident evidence.

### Evidence A: Authentication Logs

```text
09:00:00 Failed password for admin from 192.0.2.50
09:01:00 Failed password for admin from 192.0.2.50
09:02:00 Invalid user test from 192.0.2.50
09:05:00 Accepted publickey for analyst from 203.0.113.25
```

### Evidence B: Sudo Logs

```text
09:07:00 analyst executed /usr/bin/id
09:08:00 analyst executed /usr/bin/ss -tlnp
```

### Evidence C: System Logs

```text
09:10:00 example-worker.service started
09:12:00 example-worker.service reported warning
```

### Evidence D: Firewall Logs

```text
09:15:00 Connection allowed to TCP port 8080
```

All IP addresses are documentation-only example addresses.

### Investigation Questions

1. How many failed password events occurred?
2. Which username was attempted as an invalid user?
3. Which account authenticated successfully?
4. What authentication method was used?
5. Which commands were executed after authentication?
6. When did the service start?
7. What happened at 09:15?
8. Which events deserve further investigation?

### Build Your Timeline

| Time | Source | Event |
|---|---|---|
| 09:00 | Authentication | |
| 09:01 | Authentication | |
| 09:02 | Authentication | |
| 09:05 | Authentication | |
| 09:07 | Sudo | |
| 09:08 | Sudo | |
| 09:10 | System | |
| 09:12 | System | |
| 09:15 | Firewall | |

### Important

Do not automatically connect every event into one attack sequence.

Your job is to identify what the evidence establishes and what requires additional investigation.

## 10. TASK 7: Incident Severity and Risk Assessment

**Objective:** Evaluate the fictional incident.

Use this severity guide:

| Severity | Description |
|---|---|
| Critical | Extremely serious security impact |
| High | Significant security impact |
| Medium | Moderate security impact |
| Low | Limited security impact |
| Informational | Observation requiring awareness |

Evaluate these findings:

### Finding 1

Repeated failed SSH authentication attempts.

### Finding 2

Successful public-key authentication for analyst.

### Finding 3

Execution of two administrative inspection commands.

### Finding 4

Unexpected service startup.

### Finding 5

Firewall connection to port 8080.

For each finding, identify:

- Severity.
- Confidence.
- Potential impact.
- Supporting evidence.
- Additional investigation required.

Remember that confidence and severity are different measurements.

## 11. TASK 8: Evidence Integrity Verification

**Objective:** Demonstrate evidence integrity verification.

Create a practice evidence file:

```bash
echo "Module 08 assessment evidence" > evidence/sample-evidence.txt
```

Calculate its SHA-256 hash:

```bash
sha256sum evidence/sample-evidence.txt
```

Create a checksum record:

```bash
sha256sum evidence/sample-evidence.txt > evidence/evidence.sha256
```

Verify the checksum:

```bash
sha256sum -c evidence/evidence.sha256
```

Expected result:

```text
evidence/sample-evidence.txt: OK
```

### Questions

1. What is SHA-256?
2. Why do investigators calculate hashes?
3. What does a matching hash indicate?
4. What is chain of custody?

## 12. TASK 9: Incident Response Planning

**Objective:** Apply the incident response lifecycle.

For the fictional incident, develop a response plan covering:

### Preparation

What security controls should already be in place?

### Identification

How should the team validate the suspicious activity?

### Containment

What authorized actions could limit potential damage?

### Eradication

How should confirmed unauthorized activity be removed?

### Recovery

How should the server be validated before returning to normal operation?

### Lessons Learned

What improvements should the organization make?

Do not perform these actions on your actual VM. This is a planning exercise.

## 13. TASK 10: Final Security Investigation Report

**Objective:** Produce a complete professional investigation report.

Create the report:

```bash
nano reports/module08-final-investigation-report.txt
```

Use this template:

```text
==================================================
MODULE 08 - FINAL PRACTICAL ASSESSMENT
SECURITY INVESTIGATION REPORT
==================================================

1. EXECUTIVE SUMMARY

Investigation Title:
Investigation Date:
Target System:
Investigation Type:

Summary:

2. INVESTIGATION SCOPE

Objectives:
Time Range:
Systems Examined:
Limitations:

3. METHODOLOGY

Log Sources:
Commands Used:
Evidence Collection:
Investigation Techniques:

4. SYSTEM INSPECTION

Logging Status:
Authentication Records:
SSH Status:
Process Observations:
Network Observations:
Service Observations:

5. INCIDENT TIMELINE

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

6. INVESTIGATION FINDINGS

Finding 1:
Description:
Evidence:
Severity:
Confidence:

Finding 2:
Description:
Evidence:
Severity:
Confidence:

Finding 3:
Description:
Evidence:
Severity:
Confidence:

7. RISK ASSESSMENT

Potential Impact:
Affected Assets:
Security Weaknesses:
Remaining Risks:

8. INCIDENT RESPONSE PLAN

Preparation:
Identification:
Containment:
Eradication:
Recovery:
Lessons Learned:

9. EVIDENCE INTEGRITY

Evidence Files:
SHA-256 Hashes:
Collection Method:
Chain of Custody:

10. RECOMMENDATIONS

1.
2.
3.
4.
5.

11. CONCLUSION

Confirmed Facts:
Unverified Hypotheses:
Overall Assessment:
Further Investigation Required:

12. ASSESSMENT INFORMATION

Module: 08
Assessment: Final Practical Assessment
Student:
Completion Date:

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 14. Assessment Scoring Guide

<text id="score-note">Total: 100 marks</text>

| Assessment area | Marks |
|---|---:|
| Linux logging inspection | 10 |
| Authentication investigation | 10 |
| SSH security inspection | 10 |
| Process and service investigation | 10 |
| Network activity inspection | 10 |
| Log correlation and timeline | 15 |
| Severity and risk assessment | 10 |
| Evidence integrity | 5 |
| Incident response planning | 10 |
| Final investigation report | 10 |
| **Total** | **100** |

### Performance Levels

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Needs Improvement |

## 15. Final Knowledge Review

1. What is Linux logging?
2. What is systemd-journald?
3. How do you inspect failed SSH authentication?
4. What does `lastb` display?
5. What is a Linux process?
6. How do you inspect listening network ports?
7. What is log correlation?
8. What is an incident timeline?
9. What is the purpose of SHA-256?
10. What are the six stages of incident response?
11. What is the difference between severity and confidence?
12. Why must security reports distinguish facts from assumptions?

## 16. Final Completion Checklist

- [ ] Completed logging system inspection.
- [ ] Investigated authentication records.
- [ ] Inspected SSH security information.
- [ ] Investigated running processes.
- [ ] Inspected services and scheduled tasks.
- [ ] Examined network connections.
- [ ] Correlated fictional incident evidence.
- [ ] Constructed an incident timeline.
- [ ] Assessed severity and confidence.
- [ ] Verified a SHA-256 checksum.
- [ ] Developed an incident response plan.
- [ ] Created the final investigation report.

## 17. Module 08 Completion

Congratulations!

You have reached the end of Module 08: Log Analysis and Incident Response.

By completing this module, you have developed foundational defensive cybersecurity skills in:

- Linux log management.
- System event monitoring.
- Authentication investigation.
- SSH security monitoring.
- Failed login analysis.
- Process and service investigation.
- Network activity inspection.
- Log correlation.
- Incident response.
- Security investigation and reporting.

These skills are valuable for future work in:

- Security Operations Centers (SOC).
- Blue Team operations.
- Digital forensics.
- Incident response.
- Linux system administration.
- Security monitoring and auditing.

**Final Safety Reminder:** Keep your practical work inside your own authorized lab environment. Preserve evidence and avoid destructive changes.
