# MODULE 16 – PENETRATION TESTING
## LESSON 03: PENETRATION TESTING METHODOLOGY

## 1. Introduction

A penetration testing methodology is an organized process that guides security professionals through an assessment.

Instead of randomly running tools, a professional tester follows a structured approach.

A methodology helps ensure that:
- Testing is organized.
- Assessment objectives are achieved.
- Activities remain within authorization.
- Evidence is properly collected.
- Risks are evaluated consistently.
- Results can be understood and reproduced.

**Important:** A penetration tester needs a repeatable process, not just a collection of tools.

## 2. What Is a Penetration Testing Framework?

A penetration testing framework is a documented set of procedures, principles, and techniques used to guide security assessments.

Frameworks help testers work consistently and professionally.

Common examples include:

| Framework | Main Focus |
|---|---|
| PTES | Technical penetration testing process |
| NIST SP 800-115 | Technical security testing and assessment |
| OWASP Web Security Testing Guide | Web application security testing |
| OSSTMM | Structured security testing methodology |
| MITRE ATT&CK | Classification of adversary tactics and techniques |

These resources serve different purposes. They are not interchangeable.

## 3. PTES – Penetration Testing Execution Standard

PTES is a methodology that organizes penetration testing into seven main phases.

### Phase 1: Pre-engagement Interactions

This phase happens before technical testing begins.

Activities include:
- Discussing assessment objectives.
- Obtaining authorization.
- Defining scope.
- Establishing rules of engagement.
- Agreeing on timelines.
- Identifying communication contacts.

**Objective:** Make sure everyone understands what will be tested and under what conditions.

### Phase 2: Intelligence Gathering

The tester collects relevant information about authorized targets.

Examples:
- Domain information.
- IP address information.
- Technology documentation.
- Network diagrams.
- Publicly available organizational information.

**Objective:** Understand the target environment before deeper assessment.

### Phase 3: Threat Modeling

Threat modeling identifies possible threats, valuable assets, attack paths, and potential impacts.

For example, a university portal may contain:
- Student records.
- Staff accounts.
- Examination information.
- Financial records.

A threat model helps identify which assets require the strongest protection.

**Objective:** Understand what needs protection and what could go wrong.

### Phase 4: Vulnerability Analysis

The tester identifies and evaluates possible security weaknesses.

Activities may include:
- Service identification.
- Configuration review.
- Software version checks.
- Vulnerability research.
- Risk evaluation.

**Objective:** Determine which weaknesses require further investigation.

### Phase 5: Exploitation

This phase involves controlled validation of selected vulnerabilities.

The tester determines whether a weakness can actually be exploited within the approved scope.

Activities must:
- Follow authorization.
- Use controlled methods.
- Minimize impact.
- Avoid unnecessary data access.
- Stop when a safety condition is triggered.

**Objective:** Validate security weaknesses safely.

### Phase 6: Post-Exploitation

After a weakness is validated, the tester evaluates its potential impact without exceeding authorization.

This may include determining:
- What level of access was obtained.
- Which security controls were affected.
- Whether sensitive resources could be exposed.
- What business consequences might result.

The tester should not collect unnecessary data or expand access beyond the agreed limits.

**Objective:** Understand the significance of a verified weakness.

### Phase 7: Reporting

The final phase communicates the assessment results.

A professional report includes:
- Executive summary.
- Assessment scope.
- Methodology.
- Findings.
- Supporting evidence.
- Risk ratings.
- Recommendations.
- Limitations.
- Retesting information, when applicable.

**Objective:** Help the organization understand and fix security problems.

## 4. The Penetration Testing Workflow

The following is a simplified professional workflow:

```text
PRE-ENGAGEMENT
      |
      v
INTELLIGENCE GATHERING
      |
      v
THREAT MODELING
      |
      v
VULNERABILITY ANALYSIS
      |
      v
CONTROLLED EXPLOITATION
      |
      v
POST-EXPLOITATION ANALYSIS
      |
      v
REPORTING
      |
      v
REMEDIATION AND RETESTING
```

The process may be iterative. For example, new information discovered during vulnerability analysis may require additional authorized information gathering.

## 5. MITRE ATT&CK Framework

MITRE ATT&CK is a knowledge base that describes tactics and techniques associated with real-world adversary behavior.

It helps cybersecurity professionals understand how attackers operate and how defenders can detect their activities.

### Important Concepts

**Tactic:** The objective an attacker is trying to achieve.

**Technique:** The method used to achieve that objective.

**Sub-technique:** A more specific variation of a technique.

**Procedure:** A particular implementation of a technique.

### Example Tactics

| Tactic | Meaning |
|---|---|
| Reconnaissance | Gathering information about a target |
| Initial Access | Attempting to gain an entry point |
| Execution | Running code or commands |
| Persistence | Maintaining access |
| Privilege Escalation | Obtaining higher permissions |
| Defense Evasion | Attempting to avoid security controls |
| Credential Access | Attempting to obtain credentials |
| Discovery | Learning about the environment |
| Lateral Movement | Moving between systems |
| Collection | Gathering information of interest |
| Exfiltration | Moving data outside an environment |
| Impact | Affecting system availability or integrity |

These are descriptions of adversary behavior for analysis and defense, not permission to perform those actions.

### Why Is MITRE ATT&CK Important?

It helps security teams:
1. Understand attacker behavior.
2. Organize threat intelligence.
3. Improve detection rules.
4. Identify security monitoring gaps.
5. Map incidents to known techniques.
6. Improve security awareness and response.

## 6. OWASP Web Security Testing Guide

OWASP provides guidance for assessing web application security.

The OWASP Web Security Testing Guide (WSTG) covers areas such as:

- Information gathering.
- Configuration and deployment management.
- Identity management.
- Authentication.
- Authorization.
- Session management.
- Input validation.
- Error handling.
- Business logic testing.
- Client-side testing.

### Example: Authentication Testing

A tester may review an authorized test application to determine whether:
- Password policies are appropriate.
- Authentication errors reveal sensitive information.
- Session expiration works correctly.
- Account recovery controls are secure.

The objective is to identify weaknesses and recommend improvements.

## 7. Risk-Based Testing

Not every vulnerability has the same level of importance.

Risk-based testing prioritizes weaknesses according to their potential impact and likelihood.

### Example

Imagine an organization has two findings:

**Finding A:** An informational banner reveals a software product name.

**Finding B:** An access-control weakness may expose confidential student records.

Finding B should normally receive much higher priority because of its potential impact.

### Common Risk Factors

- Likelihood of exploitation.
- Potential business impact.
- Sensitivity of affected data.
- Number of affected systems.
- Existing security controls.
- Ease of remediation.

## 8. Common Penetration Testing Deliverables

A professional engagement produces documentation.

| Deliverable | Purpose |
|---|---|
| Scope document | Defines approved targets |
| Rules of Engagement | Establishes testing conditions |
| Test plan | Organizes assessment activities |
| Evidence log | Records relevant observations |
| Vulnerability register | Tracks identified weaknesses |
| Technical report | Explains detailed findings |
| Executive report | Summarizes business risks |
| Retest report | Confirms remediation results |

## 9. Practical Exercise – Build a Methodology Checklist

You will create a checklist for your own authorized VMware lab.

### Step 1: Create the lesson workspace

```bash
mkdir -p ~/module16-lab/lesson03
cd ~/module16-lab/lesson03
```

### Step 2: Create the methodology checklist

```bash
nano methodology-checklist.txt
```

Enter:

```text
PENETRATION TESTING METHODOLOGY CHECKLIST

PROJECT:
Local VMware Security Lab

PHASE 1: PRE-ENGAGEMENT
[ ] Confirm authorization
[ ] Define scope
[ ] Review Rules of Engagement
[ ] Establish stop conditions

PHASE 2: INTELLIGENCE GATHERING
[ ] Identify approved targets
[ ] Review available documentation
[ ] Record relevant information

PHASE 3: THREAT MODELING
[ ] Identify important assets
[ ] Identify possible threats
[ ] Consider potential impact

PHASE 4: VULNERABILITY ANALYSIS
[ ] Review system configuration
[ ] Identify available services
[ ] Research possible weaknesses
[ ] Prioritize findings

PHASE 5: CONTROLLED VALIDATION
[ ] Confirm authorization
[ ] Validate selected weaknesses safely
[ ] Avoid unnecessary changes
[ ] Record evidence

PHASE 6: POST-TEST ANALYSIS
[ ] Evaluate potential impact
[ ] Review security controls
[ ] Document limitations

PHASE 7: REPORTING
[ ] Prepare executive summary
[ ] Document technical findings
[ ] Assign risk ratings
[ ] Recommend remediation
[ ] Prepare final report

FINAL REVIEW
[ ] Confirm testing stayed within scope
[ ] Secure assessment evidence
[ ] Document outstanding issues
```

Save with `CTRL + O`, press `ENTER`, then `CTRL + X`.

### Step 3: View your checklist

```bash
cat methodology-checklist.txt
```

### Step 4: Understand the workflow

For now, do not scan or exploit any machine. This exercise is about learning the professional process.

## 10. Knowledge Check

**Question 1:** How many main phases are in the PTES methodology?

A. Four  
B. Five  
C. Seven  
D. Ten  

**Question 2:** Which phase identifies valuable assets and possible threats?

A. Reporting  
B. Threat Modeling  
C. Retesting  
D. Remediation  

**Question 3:** What is the main purpose of MITRE ATT&CK?

A. Managing computer hardware  
B. Describing adversary tactics and techniques  
C. Creating user accounts  
D. Installing Linux packages  

**Question 4:** Which resource focuses on web application security testing?

A. OWASP WSTG  
B. DHCP  
C. SSH  
D. UFW  

**Question 5:** What does risk-based testing help determine?

A. Which findings should receive priority  
B. Which files should be deleted  
C. Which passwords should be shared  
D. Which systems should be damaged  

## 11. Practical Thinking Questions

Answer these in your notebook:

1. What is a penetration testing methodology?
2. Explain the seven PTES phases.
3. What is the difference between a tactic and a technique?
4. Why is threat modeling important?
5. What is the purpose of the OWASP WSTG?
6. Why should vulnerabilities be prioritized according to risk?
7. What is the difference between an executive report and a technical report?

## 12. Lesson Summary

In this lesson, you learned:

- The meaning of penetration testing methodology.
- The seven phases of PTES.
- The purpose of MITRE ATT&CK.
- The role of OWASP WSTG.
- Threat modeling and risk-based testing.
- Common professional assessment deliverables.
- How to create a penetration testing methodology checklist.

### Key Principle

**A professional penetration test follows a structured methodology: plan carefully, gather information, assess risks, validate weaknesses safely, and report findings clearly.**

**End of Lesson 03 – Penetration Testing Methodology**