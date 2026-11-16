
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 09: SECURITY INVESTIGATION AND REPORTING

## 1. Introduction

Cybersecurity investigations require more than identifying suspicious activity.

A professional security analyst must collect evidence, examine events, determine what happened, assess the impact, and communicate findings clearly.

This process is called security investigation and reporting.

Security reports are used by:

- Security Operations Center (SOC) analysts.
- Incident response teams.
- Digital forensic investigators.
- System administrators.
- Cybersecurity consultants.
- Management and technical teams.

A good report helps decision-makers understand security problems and choose appropriate corrective actions.

In this lab, you will learn how to conduct a structured investigation and produce a professional cybersecurity report.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Define a security investigation.
2. Establish an investigation scope.
3. Identify relevant evidence.
4. Examine security events.
5. Construct an incident timeline.
6. Assess security findings.
7. Determine incident severity.
8. Distinguish facts from assumptions.
9. Preserve evidence integrity.
10. Produce a professional investigation report.

## 3. Understanding Security Investigations

A security investigation is a structured examination of an event, system, or activity to determine whether a security issue exists.

Example:

A Linux server reports repeated failed SSH login attempts.

A security analyst investigates:

1. When the attempts occurred.
2. Which accounts were targeted.
3. Which addresses were involved.
4. Whether authentication succeeded.
5. Whether privileged commands were executed.
6. Whether other suspicious activity followed.

The objective is to establish what the evidence supports.

## 4. Practical Exercise 1: Define Investigation Scope

Before starting an investigation, establish its scope.

Scope defines what will and will not be examined.

A basic investigation scope contains:

| Element | Description |
|---|---|
| Target system | System being investigated |
| Time range | Period being examined |
| Incident type | Nature of suspected activity |
| Evidence sources | Logs and other records |
| Objectives | Questions the investigation must answer |
| Limitations | Missing records or technical restrictions |

### Example

```text
INVESTIGATION:
Suspicious SSH Activity

TARGET:
Linux Server

TIME RANGE:
09:00 - 10:00

OBJECTIVES:
Identify failed login attempts,
successful authentication,
and subsequent privileged activity.

EVIDENCE:
Authentication logs,
system logs,
sudo records.
```

### Exercise

Create a fictional investigation scope for a Linux server experiencing unusual authentication activity.

## 5. Practical Exercise 2: Identify Evidence Sources

Security analysts may collect evidence from different locations.

### A. Authentication Evidence

Common source:

```text
/var/log/auth.log
```

Alternative:

```text
/var/log/secure
```

### B. System Evidence

Common source:

```text
/var/log/syslog
```

Alternative:

```text
/var/log/messages
```

### C. Kernel Evidence

Inspect kernel journal records:

```bash
sudo journalctl -k --no-pager
```

### D. SSH Evidence

Search for SSH events:

```bash
sudo journalctl --no-pager | grep -i sshd
```

### E. Sudo Evidence

Search for sudo activity:

```bash
sudo journalctl --no-pager | grep -i sudo
```

### F. Network Evidence

Inspect network connections:

```bash
sudo ss -tunap
```

Record the evidence sources available on your Kali VM.

Not every source will be present on every distribution.

## 6. Practical Exercise 3: Collect Investigation Evidence

Create a dedicated investigation directory:

```bash
mkdir -p ~/practical-labs/module08/lab09/evidence
```

Enter it:

```bash
cd ~/practical-labs/module08/lab09
```

Inspect the available journal:

```bash
sudo journalctl -n 50 --no-pager
```

Inspect authentication records if available:

```bash
sudo tail -n 50 /var/log/auth.log
```

For a system that does not use `auth.log`, use the journal instead.

### Important Evidence Collection Rules

- Do not edit original logs.
- Do not delete records.
- Do not change system configuration to create evidence.
- Record where evidence originated.
- Record the collection date and time.
- Preserve relevant context.

For this lab, inspect records without exporting sensitive information unnecessarily.

## 7. Practical Exercise 4: Build an Investigation Timeline

A timeline arranges relevant events chronologically.

Consider this fictional scenario:

```text
08:50:00 Failed SSH login for admin
08:51:00 Failed SSH login for admin
08:52:00 Invalid SSH username test
08:55:00 Successful SSH login for analyst
08:57:00 Sudo command executed by analyst
09:00:00 Service startup recorded
09:02:00 System warning recorded
```

Construct this table:

| Time | Event | Evidence source |
|---|---|---|
| 08:50 | Failed SSH login | Authentication |
| 08:51 | Failed SSH login | Authentication |
| 08:52 | Invalid username | Authentication |
| 08:55 | Successful login | Authentication |
| 08:57 | Sudo activity | Authentication |
| 09:00 | Service startup | System |
| 09:02 | System warning | System |

### Investigation Questions

1. What happened first?
2. When did successful authentication occur?
3. What happened after authentication?
4. Which events deserve further investigation?
5. What additional evidence is required?

A timeline helps identify possible relationships between events, but chronological proximity alone does not prove causation.

## 8. Practical Exercise 5: Distinguish Facts from Assumptions

This is one of the most important professional investigation skills.

### Facts

Facts are statements directly supported by reliable evidence.

Example:

```text
The authentication log contains
two failed SSH login events at 08:50
and 08:51.
```

### Assumptions

Assumptions are conclusions that have not been established by available evidence.

Example:

```text
The server was definitely attacked
by a criminal organization.
```

The second statement cannot be justified by two failed login records alone.

### Exercise

Classify the following statements as FACT or UNVERIFIED HYPOTHESIS.

1. The authentication log recorded three failed login attempts.
2. The source IP belongs to a criminal hacker.
3. A successful login was recorded at 08:55.
4. The account owner intentionally gave an attacker access.
5. A service startup event occurred at 09:00.
6. The service startup was definitely caused by an attacker.

Explain your answers.

## 9. Practical Exercise 6: Assess Security Findings

A finding is an issue or observation identified during an investigation.

Examples:

- Repeated failed authentication.
- Unexpected privileged activity.
- Unusual service startup.
- Weak access controls.
- Missing security logs.
- Unauthorized account configuration.

A useful finding contains:

1. Finding title.
2. Description.
3. Supporting evidence.
4. Potential impact.
5. Severity.
6. Confidence level.
7. Recommendation.

### Example Finding

```text
FINDING:
Repeated SSH Authentication Failures

DESCRIPTION:
Multiple failed SSH login attempts were
recorded within a short time period.

EVIDENCE:
Authentication records showing repeated
failures from the same source address.

POTENTIAL IMPACT:
Possible unauthorized access attempts.

SEVERITY:
Medium - Preliminary.

CONFIDENCE:
High that the failures occurred.
Unknown whether an attack succeeded.

RECOMMENDATION:
Review authentication activity,
verify affected accounts, and improve
remote-access monitoring.
```

## 10. Practical Exercise 7: Understand Severity and Confidence

Severity and confidence are different concepts.

### Severity

Severity describes the potential seriousness of a finding.

| Level | Meaning |
|---|---|
| Critical | Extremely serious potential impact |
| High | Significant potential impact |
| Medium | Moderate potential impact |
| Low | Limited potential impact |
| Informational | Observation requiring awareness |

### Confidence

Confidence describes how strongly the evidence supports a finding.

| Level | Meaning |
|---|---|
| High | Strong supporting evidence |
| Medium | Some supporting evidence |
| Low | Limited or uncertain evidence |

Example:

An analyst may have:

- High confidence that repeated login failures occurred.
- Low confidence that the attempts resulted in unauthorized access.

Never confuse evidence confidence with incident severity.

## 11. Practical Exercise 8: Calculate Evidence Hashes

Cryptographic hashes help verify whether evidence files have changed.

Create a sample report file:

```bash
echo "Investigation evidence sample" > sample-evidence.txt
```

Calculate its SHA-256 hash:

```bash
sha256sum sample-evidence.txt
```

Save the output in your notes.

Calculate the hash again:

```bash
sha256sum sample-evidence.txt
```

Compare the results.

The same unchanged file should produce the same SHA-256 hash.

You can also verify a hash using a checksum file.

Create one:

```bash
sha256sum sample-evidence.txt > evidence.sha256
```

Verify it:

```bash
sha256sum -c evidence.sha256
```

Expected result:

```text
sample-evidence.txt: OK
```

A matching hash supports file integrity verification. It does not prove that the original evidence was authentic or that it was collected correctly.

## 12. Practical Exercise 9: Analyze a Fictional Security Investigation

An organization requests an investigation into unusual activity on a Linux server.

The following fictional evidence is supplied.

### Authentication Evidence

```text
10:00 Failed password for admin from 192.0.2.40
10:01 Failed password for admin from 192.0.2.40
10:02 Invalid user test from 192.0.2.40
10:05 Accepted publickey for analyst from 203.0.113.15
```

### Sudo Evidence

```text
10:07 analyst executed /usr/bin/id
10:08 analyst executed /usr/bin/ss -tlnp
```

### System Evidence

```text
10:10 example-worker.service started
10:12 example-worker.service reported warning
```

### Network Evidence

```text
10:15 Connection allowed to TCP port 8080
```

All IP addresses are documentation-only example addresses.

### Investigation Tasks

**Task A: Define Scope**

Identify the target system, time range, and investigation objectives.

**Task B: Identify Evidence**

List all available evidence sources.

**Task C: Construct Timeline**

Arrange all supplied events chronologically.

**Task D: Identify Findings**

Identify at least three findings.

**Task E: Assess Severity**

Assign a preliminary severity to each finding.

**Task F: Assess Confidence**

Identify which findings are directly supported by evidence and which remain uncertain.

**Task G: Recommend Actions**

Provide five practical security recommendations.

**Task H: Write a Conclusion**

Summarize what the evidence establishes and what still requires investigation.

Do not assume that the successful login, service startup, and network connection are related without additional evidence.

## 13. Practical Exercise 10: Write a Professional Security Investigation Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module08/lab09
```

Enter it:

```bash
cd ~/practical-labs/module08/lab09
```

Create the report:

```bash
nano security-investigation-report.txt
```

Use this professional template:

```text
==================================================
MODULE 08 - LAB 09
SECURITY INVESTIGATION REPORT
==================================================

1. EXECUTIVE SUMMARY

Investigation Title:
Investigation Date:
Target System:
Incident Type:

Summary of Findings:

2. INVESTIGATION SCOPE

Investigation Objectives:
Time Range:
Systems Examined:
Limitations:

3. METHODOLOGY

Log Sources:
Commands Used:
Evidence Collection Method:
Analysis Techniques:

4. EVIDENCE SUMMARY

Evidence 1:
Source:
Description:
Hash:

Evidence 2:
Source:
Description:
Hash:

Evidence 3:
Source:
Description:
Hash:

5. EVENT TIMELINE

Event 1:
Timestamp:
Description:

Event 2:
Timestamp:
Description:

Event 3:
Timestamp:
Description:

Event 4:
Timestamp:
Description:

6. FINDINGS

Finding 1:
Description:
Supporting Evidence:
Potential Impact:
Severity:
Confidence:

Finding 2:
Description:
Supporting Evidence:
Potential Impact:
Severity:
Confidence:

7. RISK ASSESSMENT

Affected Assets:
Potential Impact:
Existing Security Controls:
Remaining Risks:

8. RECOMMENDATIONS

1.
2.
3.
4.
5.

9. CONCLUSION

Confirmed Facts:
Unverified Hypotheses:
Overall Assessment:
Further Investigation Required:

10. REPORT INFORMATION

Prepared By:
Report Version:
Report Date:

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 14. Professional Reporting Principles

A good cybersecurity report should be:

### Accurate

Every statement should be supported by evidence.

### Clear

Use language that technical and nontechnical readers can understand.

### Objective

Avoid personal opinions and unsupported accusations.

### Structured

Use headings, timelines, findings, and recommendations.

### Reproducible

Document the methods and evidence sources so another analyst can understand the investigation.

### Confidential

Protect sensitive information and share reports only with authorized recipients.

## 15. Knowledge Check

1. What is a security investigation?
2. What is investigation scope?
3. What is evidence collection?
4. What is an incident timeline?
5. What is the difference between a fact and an assumption?
6. What is a security finding?
7. What is the difference between severity and confidence?
8. Why are SHA-256 hashes useful?
9. What makes a cybersecurity report professional?
10. Why should investigators avoid unsupported conclusions?

## 16. Lab Completion Checklist

- [ ] Defined investigation scope.
- [ ] Identified available evidence sources.
- [ ] Inspected relevant logs.
- [ ] Built an incident timeline.
- [ ] Distinguished facts from assumptions.
- [ ] Assessed security findings.
- [ ] Understood severity and confidence.
- [ ] Calculated SHA-256 hashes.
- [ ] Analyzed the fictional investigation.
- [ ] Created a professional investigation report.
- [ ] Completed the knowledge check.

## 17. Expected Outcome

You should now understand how to conduct a structured security investigation and communicate findings professionally.

You have learned how to establish scope, collect evidence, construct timelines, assess findings, calculate hashes, and produce an investigation report.

**Safety reminder:** This lab is read-only except for creating your own practice files. Never modify original evidence or investigate systems without authorization.
