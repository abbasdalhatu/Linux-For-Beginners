
# MODULE 08 – LOG ANALYSIS AND INCIDENT RESPONSE

# LAB 08: INCIDENT RESPONSE FUNDAMENTALS

## 1. Introduction

Incident response is the organized process of identifying, investigating, managing, and recovering from cybersecurity incidents.

A security incident may involve:

- Unauthorized access.
- Malware infection.
- Data exposure.
- Account compromise.
- Suspicious network activity.
- Unauthorized privilege escalation.
- Service disruption.
- Data destruction.

When an incident occurs, an organization must respond carefully.

Poor incident handling can cause additional damage, destroy evidence, or interrupt important business operations.

This lab introduces the incident response lifecycle and teaches you how security teams manage incidents professionally.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Define a cybersecurity incident.
2. Explain the incident response lifecycle.
3. Identify and classify security incidents.
4. Understand incident severity.
5. Explain containment, eradication, and recovery.
6. Understand evidence preservation.
7. Develop a basic incident response plan.
8. Document incident activities.
9. Analyze a fictional security incident.
10. Produce an incident response report.

## 3. What Is a Cybersecurity Incident?

A cybersecurity incident is an event that threatens or violates the confidentiality, integrity, or availability of information systems or data.

These three security principles are known as the CIA Triad.

### A. Confidentiality

Information should only be accessible to authorized individuals.

Example: An unauthorized person accesses private student records.

### B. Integrity

Information should remain accurate and protected from unauthorized modification.

Example: An attacker changes financial records without permission.

### C. Availability

Systems and information should remain accessible when needed.

Example: A denial-of-service incident prevents students from accessing a university portal.

## 4. Understanding the Incident Response Lifecycle

A common incident response lifecycle contains six major stages:

1. Preparation.
2. Identification.
3. Containment.
4. Eradication.
5. Recovery.
6. Lessons Learned.

<text id="lifecycle-note">The stages form a continuous improvement process. Organizations may revisit earlier stages as new evidence becomes available.</text>

### Stage 1: Preparation

Preparation means getting ready before an incident occurs.

Activities include:

- Creating incident response procedures.
- Training security personnel.
- Establishing communication channels.
- Maintaining backups.
- Configuring monitoring systems.
- Identifying critical assets.
- Preparing evidence collection procedures.

Example:

An organization creates an incident response team and documents who should be contacted when a server is compromised.

### Stage 2: Identification

Identification means determining whether a security incident has occurred.

Activities include:

- Reviewing security alerts.
- Examining system logs.
- Investigating suspicious login activity.
- Checking unusual processes.
- Reviewing network traffic.
- Validating security reports.

Example:

A monitoring system reports repeated SSH login failures followed by an unexpected successful login.

The security team investigates whether the activity is authorized.

### Stage 3: Containment

Containment limits the spread or impact of an incident.

Possible defensive measures include:

- Restricting affected accounts.
- Isolating an affected device.
- Blocking confirmed malicious traffic.
- Restricting access to compromised services.
- Preserving relevant evidence.

Containment must be planned carefully.

For example, disconnecting a production server may interrupt critical business operations.

Do not perform containment actions on your actual VM or network during this lab.

### Stage 4: Eradication

Eradication removes the cause of the incident.

Activities may include:

- Removing confirmed malware.
- Eliminating unauthorized accounts.
- Removing malicious scheduled tasks.
- Correcting exploited vulnerabilities.
- Replacing compromised credentials.
- Applying security patches.

Eradication should only occur after appropriate investigation and evidence preservation.

### Stage 5: Recovery

Recovery restores affected systems to normal operation.

Activities include:

- Restoring verified backups.
- Rebuilding affected systems.
- Validating system integrity.
- Testing applications.
- Monitoring for recurring activity.
- Confirming that security controls work.

### Stage 6: Lessons Learned

After the incident, the organization reviews its response.

Questions include:

- What happened?
- How was the incident detected?
- What was affected?
- How did the response team perform?
- What should be improved?
- How can similar incidents be prevented?

The organization documents findings and updates its security procedures.

## 5. Practical Exercise 1: Identify Security Incidents

Study the following scenarios.

| Scenario | Possible incident |
|---|---|
| Unauthorized access to a server | Account compromise |
| Malware discovered on a workstation | Malware infection |
| Student records exposed publicly | Data exposure |
| Website becomes unavailable | Service disruption |
| Unauthorized changes to financial data | Data integrity incident |
| Repeated suspicious SSH activity | Possible unauthorized access attempt |

For each scenario, identify:

1. What happened?
2. Which security principle is affected?
3. What evidence would you collect?
4. Who should be informed?

Remember that an alert or suspicious event requires validation before being classified as a confirmed incident.

## 6. Practical Exercise 2: Understand Incident Severity

Organizations classify incidents according to their potential impact and urgency.

A simple example:

### Critical

Potentially severe impact on critical systems, sensitive information, or essential operations.

### High

Significant security impact requiring urgent investigation.

### Medium

Limited but meaningful security impact requiring investigation.

### Low

Minor security event with limited impact.

### Informational

An event recorded for awareness or routine monitoring.

Severity depends on factors such as:

- Number of affected systems.
- Sensitivity of affected data.
- Business impact.
- Whether unauthorized access is confirmed.
- Whether the incident is ongoing.
- Availability of recovery options.

### Exercise

Classify the following fictional events:

1. A user mistypes a password once.
2. An administrator's account is confirmed compromised.
3. A critical university database becomes unavailable.
4. A monitoring tool records an unusual but unexplained process.
5. Sensitive records are confirmed to have been accessed by an unauthorized person.

Explain your reasoning.

## 7. Practical Exercise 3: Understand Incident Response Roles

An incident response team may include:

| Role | Responsibility |
|---|---|
| Incident Manager | Coordinates incident response |
| Security Analyst | Investigates alerts and evidence |
| System Administrator | Manages affected systems |
| Network Administrator | Investigates network activity |
| Digital Forensics Analyst | Collects and examines evidence |
| Management | Approves major operational decisions |
| Communications Team | Handles approved internal and external communication |

In smaller organizations, one person may perform multiple roles.

### Exercise

Imagine a university discovers suspicious access to its student database.

Identify:

- Who should coordinate the response?
- Who should investigate the logs?
- Who should assess database security?
- Who should approve major operational changes?
- Who should handle official communications?

## 8. Practical Exercise 4: Prepare an Incident Response Plan

A basic incident response plan should contain:

### 1. Purpose

Explain why the plan exists.

### 2. Scope

Identify the systems, users, and departments covered.

### 3. Responsibilities

Define the roles of incident response personnel.

### 4. Reporting Procedure

Explain how users report suspected incidents.

### 5. Identification

Describe how incidents are detected and validated.

### 6. Containment

Define approved ways to limit impact.

### 7. Eradication

Explain how confirmed threats are removed.

### 8. Recovery

Document system restoration and validation procedures.

### 9. Evidence Handling

Explain how evidence is collected, preserved, and documented.

### 10. Lessons Learned

Describe how the organization improves after an incident.

## 9. Practical Exercise 5: Incident Documentation

Every significant incident should have a record.

A useful incident record includes:

- Incident reference number.
- Date and time detected.
- Reporting person or system.
- Affected assets.
- Description.
- Severity.
- Evidence collected.
- Investigation findings.
- Response actions.
- Recovery status.
- Final outcome.

Example:

```text id="2tr5ef"
INCIDENT ID: INC-2026-001

TITLE:
Suspicious SSH Authentication Activity

DETECTED:
15 November 2026, 09:00

AFFECTED SYSTEM:
Linux server

INITIAL OBSERVATION:
Repeated failed SSH login attempts.

SEVERITY:
Pending investigation.

EVIDENCE:
Authentication logs and SSH records.

STATUS:
Under investigation.
```

The example is fictional.

## 10. Practical Exercise 6: Evidence Collection During an Incident

Evidence collection is an important part of incident response.

Potential evidence includes:

- Authentication logs.
- System logs.
- Application logs.
- Firewall logs.
- Network connection records.
- Process information.
- File metadata.
- File hashes.
- System configuration.

Good evidence-handling practices include:

1. Record the collection time.
2. Record the source system.
3. Identify the person collecting evidence.
4. Preserve original records.
5. Work with verified copies.
6. Calculate cryptographic hashes where appropriate.
7. Maintain chain-of-custody documentation.

Never delete or alter original evidence simply to make an investigation easier.

## 11. Practical Exercise 7: Develop an Incident Response Playbook

A playbook is a practical set of procedures for handling a particular type of incident.

Create a simple playbook for suspicious SSH activity.

### Step 1: Detection

Review authentication alerts and SSH logs.

### Step 2: Validation

Determine whether the activity is genuine and whether successful authentication occurred.

### Step 3: Scope

Identify affected accounts, systems, and time ranges.

### Step 4: Evidence Collection

Preserve authentication records, relevant system logs, and other supporting evidence.

### Step 5: Containment Planning

Recommend appropriate actions based on verified findings and organizational approval.

### Step 6: Eradication Planning

Identify and address confirmed causes of unauthorized access.

### Step 7: Recovery

Validate affected systems and restore normal operation.

### Step 8: Lessons Learned

Document improvements to authentication, monitoring, and access controls.

This exercise is planning-only. Do not modify real accounts or network access.

## 12. Practical Exercise 8: Analyze a Fictional Security Incident

A university IT department reports suspicious activity on a Linux server.

The following evidence is supplied:

### Authentication Records

```text id="xv1p8v"
09:00 Failed password for admin from 192.0.2.50
09:01 Failed password for admin from 192.0.2.50
09:02 Accepted password for admin from 192.0.2.50
```

### Privilege Activity

```text id="8v7c6b"
09:04 admin executed /usr/bin/id
09:05 admin executed /usr/bin/systemctl status cron
```

### System Activity

```text id="2qv2mq"
09:07 Unusual service startup recorded
09:08 System warning recorded
```

All addresses are fictional documentation examples.

### Investigation Tasks

**Task A: Identify the Incident**

What suspicious activity was reported?

**Task B: Identify the Evidence**

Which authentication and system records are relevant?

**Task C: Assess the Impact**

What systems or accounts may be affected?

**Task D: Assign Severity**

Would you initially classify this as low, medium, high, or critical? Explain that this is a preliminary assessment based on limited evidence.

**Task E: Plan Containment**

What containment measures could the authorized incident response team consider?

**Task F: Plan Eradication**

What should be verified before removing suspected unauthorized activity?

**Task G: Plan Recovery**

How would the university confirm that the server is safe to return to normal operation?

**Task H: Lessons Learned**

What security improvements would you recommend?

Do not conclude that the administrator account was compromised solely from the supplied records.

## 13. Practical Exercise 9: Create an Incident Response Report

Create your lab directory:

```bash id="a5bn19"
mkdir -p ~/practical-labs/module08/lab08
```

Enter it:

```bash id="4v7uox"
cd ~/practical-labs/module08/lab08
```

Create your report:

```bash id="9m8crd"
nano incident-response-report.txt
```

Use this template:

```text id="m4v2sg"
==================================================
MODULE 08 - LAB 08
INCIDENT RESPONSE FUNDAMENTALS
==================================================

1. INCIDENT DETAILS

Incident ID:
Incident Title:
Date Detected:
Affected System:
Reported By:

2. INCIDENT DESCRIPTION

What Happened:
Initial Indicators:
Affected Assets:

3. INCIDENT CLASSIFICATION

Incident Type:
Severity:
Reason for Classification:

4. IDENTIFICATION

Detection Method:
Evidence Collected:
Confirmed Findings:
Unverified Findings:

5. CONTAINMENT PLAN

Recommended Actions:
Required Approval:
Potential Operational Impact:

6. ERADICATION PLAN

Confirmed Cause:
Removal Strategy:
Security Improvements:

7. RECOVERY PLAN

Recovery Requirements:
Validation Steps:
Monitoring Requirements:

8. EVIDENCE MANAGEMENT

Evidence Sources:
Collection Time:
Evidence Hashes:
Chain of Custody:

9. LESSONS LEARNED

Security Weaknesses:
Response Improvements:
Preventive Recommendations:

10. FINAL ASSESSMENT

Incident Status:
Remaining Risks:
Further Investigation Required:

==================================================
END OF REPORT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 14. Knowledge Check

1. What is incident response?
2. What are the six incident response lifecycle stages?
3. What is preparation?
4. What happens during identification?
5. What is containment?
6. What is eradication?
7. Why is recovery important?
8. What is the purpose of lessons learned?
9. Why must evidence be preserved?
10. What information should an incident report contain?

## 15. Lab Completion Checklist

- [ ] Understood the CIA Triad.
- [ ] Studied the incident response lifecycle.
- [ ] Identified different incident types.
- [ ] Examined incident severity.
- [ ] Understood incident response roles.
- [ ] Prepared an incident response plan.
- [ ] Studied evidence collection.
- [ ] Developed an SSH incident playbook.
- [ ] Analyzed the fictional university incident.
- [ ] Created the incident response report.
- [ ] Completed the knowledge check.

## 16. Expected Outcome

You should now understand how cybersecurity teams prepare for, identify, contain, eradicate, and recover from security incidents.

You should also understand why incident response requires coordination, evidence preservation, documentation, and careful decision-making.

**Safety reminder:** This lab is planning and analysis only. Do not isolate your actual VM, disable services, delete accounts, remove files, or make changes to a real system.
