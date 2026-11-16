
# MODULE 16 – LESSON 14
# PENETRATION TESTING REPORT AND PRACTICAL PROJECTS

## 1. Introduction

A penetration testing report is a professional document that explains the security assessment performed on a system, the vulnerabilities discovered, their potential impact, and recommendations for fixing them.

A penetration test is not complete until its findings are properly documented and communicated.

A good report helps an organization understand:

- What was tested.
- Why it was tested.
- Which security weaknesses were identified.
- How serious the weaknesses are.
- What evidence supports each finding.
- How the organization can improve its security.

### Simple Explanation

Imagine that a security professional examines a university portal and discovers that students may be able to access records belonging to other students.

The professional must prepare a report explaining:

1. What the problem is.
2. How it was identified.
3. What information could be exposed.
4. How serious the problem is.
5. How developers should fix it.

That document is the penetration testing report.

## 2. Objectives of a Penetration Testing Report

A professional report should:

1. Communicate security findings clearly.
2. Explain technical risks.
3. Provide evidence.
4. Prioritize vulnerabilities.
5. Recommend appropriate remediation.
6. Support management decisions.
7. Provide a record for future assessments.
8. Help verify that security improvements were successful.

## 3. Types of Penetration Testing Reports

There are two major audiences for penetration testing reports.

### A. Executive Report

An executive report is written for management and decision-makers.

It focuses on:
- Overall security posture.
- Major risks.
- Business impact.
- Important recommendations.
- Remediation priorities.

It avoids unnecessary technical detail.

### B. Technical Report

A technical report is written for IT staff, developers, system administrators, and security professionals.

It includes:
- Technical findings.
- Affected systems.
- Evidence.
- Vulnerability details.
- Severity ratings.
- Reproduction information within the authorized scope.
- Remediation instructions.

### Comparison

| Executive Report | Technical Report |
|---|---|
| Management audience | Technical audience |
| Business risks | Technical weaknesses |
| High-level findings | Detailed findings |
| Strategic recommendations | Specific remediation steps |
| Concise | More detailed |

A complete assessment may include both reports.

## 4. Structure of a Professional Penetration Testing Report

A professional report commonly contains the following sections.

### 1. Cover Page

Include:
- Assessment title.
- Organization name.
- Report date.
- Report author.
- Document classification.

### 2. Executive Summary

Explain the purpose, scope, and overall results of the assessment.

### 3. Scope and Methodology

Document:
- Authorized targets.
- Excluded systems.
- Testing period.
- Testing methodology.
- Restrictions.
- Tools used.

### 4. Assessment Results

Summarize the findings and their severity.

### 5. Detailed Findings

Explain each vulnerability separately.

### 6. Risk Analysis

Describe the likelihood and impact of each finding.

### 7. Recommendations

Provide practical remediation guidance.

### 8. Conclusion

Summarize the overall security posture and next steps.

### 9. Appendices

Include supporting information such as:
- Evidence references.
- Tool versions.
- Testing records.
- Technical observations.
- Glossary.

## 5. Understanding Vulnerability Severity

Security findings should be prioritized according to their risk.

A common severity classification is:

| Severity | Meaning |
|---|---|
| Critical | Extremely serious risk requiring urgent attention |
| High | Significant risk requiring prompt remediation |
| Medium | Meaningful risk that should be addressed |
| Low | Limited risk that should be corrected |
| Informational | Observation or improvement opportunity |

Severity should be supported by evidence and context.

Do not classify every finding as critical.

## 6. Understanding CVSS

CVSS stands for Common Vulnerability Scoring System.

It provides a standardized method for communicating vulnerability severity.

CVSS v4.0 uses a numerical score from 0.0 to 10.0.

Common severity ranges are:

| Score | Severity |
|---|---|
| 0.0 | None |
| 0.1–3.9 | Low |
| 4.0–6.9 | Medium |
| 7.0–8.9 | High |
| 9.0–10.0 | Critical |

CVSS is a vulnerability-severity framework, not a complete business-risk assessment.

Organizations should also consider:
- Asset importance.
- Exposure.
- Existing security controls.
- Business consequences.
- Threat activity.
- Remediation difficulty.

## 7. Understanding Risk Prioritization

Risk prioritization helps organizations decide which weaknesses to fix first.

A simple conceptual model is:

\[
Risk = Likelihood \times Impact
\]

For example:

| Finding | Likelihood | Impact | Priority |
|---|---|---|---|
| Exposed administrator account | High | High | Critical priority |
| Weak internal password policy | Medium | High | High priority |
| Missing informational header | Low | Low | Low priority |

This is a simplified model. Real assessments may use more detailed scoring methods.

## 8. Writing a Good Vulnerability Finding

Every technical finding should contain:

### Finding Title

A short description of the problem.

### Description

Explain what the vulnerability is.

### Affected Asset

Identify the affected application, server, or component.

### Evidence

Include relevant observations that support the finding.

### Impact

Explain what could happen if the weakness is exploited.

### Severity

Assign an appropriate severity with justification.

### Recommendation

Explain how the organization should fix the problem.

### Retest Result

Document whether the issue was successfully resolved.

## 9. Example of a Professional Finding

The following is a fictional training example.

### Finding: Weak Password Policy

**Description:**

The hypothetical application does not enforce an adequate password policy.

**Affected Asset:**

University student portal – fictional training application.

**Evidence:**

A documented configuration review indicates that the application accepts passwords shorter than the organization's proposed minimum.

**Potential Impact:**

Users may choose easily guessed passwords, increasing the possibility of account compromise.

**Severity:**

Medium – based on the hypothetical environment and available evidence.

**Recommendation:**

- Enforce an appropriate minimum password length.
- Block commonly used and compromised passwords.
- Support password managers.
- Enable multifactor authentication.
- Apply login rate limiting.

**Retest Status:**

Not yet tested.

## 10. Evidence Presentation

Evidence should be clear, relevant, and securely handled.

Examples include:

- Screenshots.
- Sanitized command output.
- Configuration excerpts.
- Application logs.
- Hash verification records.

### Evidence Rules

1. Remove unnecessary personal information.
2. Never expose real passwords or private keys.
3. Label screenshots clearly.
4. Keep original evidence protected.
5. Reference evidence using unique identifiers.
6. Do not fabricate evidence.
7. Distinguish observations from assumptions.

## 11. Remediation Recommendations

A useful report should provide practical solutions.

Examples:

| Vulnerability | Recommended Remediation |
|---|---|
| Weak passwords | Enforce secure password policies |
| Broken access control | Implement server-side authorization |
| Outdated software | Apply security updates |
| Insecure configuration | Use secure configuration baselines |
| Exposed sensitive files | Restrict access and remove unnecessary exposure |
| Weak session management | Use secure session controls |
| Insufficient monitoring | Implement security logging and alerting |

Recommendations should be specific enough for the responsible technical team to act on.

## 12. Practical Project 1: Local Linux Security Assessment

### Objective

Conduct a basic security review of your own Kali Linux virtual machine.

This project is an assessment of your local environment, not an attempt to compromise another system.

### Step 1: Create Your Project Directory

```bash
mkdir -p ~/module16-lab/lesson14/project1
cd ~/module16-lab/lesson14/project1
```

### Step 2: Record System Information

```bash
cat /etc/os-release
```

```bash
uname -a
```

### Step 3: Review Network Configuration

```bash
ip addr
```

```bash
ip route
```

### Step 4: Review Listening Services

```bash
ss -tuln
```

Identify:
- Listening TCP ports.
- Listening UDP ports.
- Local services.
- Unexpected listening services, if any.

Do not assume a service is vulnerable merely because it is listening.

### Step 5: Review Installed Packages

```bash
dpkg -l | less
```

Check package update information:

```bash
apt list --upgradable
```

Do not install updates as part of this assessment unless you have decided to do so separately.

### Step 6: Review Local Account Information

```bash
getent passwd
```

Review the output carefully.

Do not expose account information unnecessarily in a public report.

### Step 7: Document Findings

Record actual observations.

For every potential finding, include:
- Description.
- Evidence.
- Potential impact.
- Severity.
- Recommendation.

## 13. Practical Project 2: Web Application Security Review

### Objective

Prepare a security review of a fictional university portal.

The application contains:

- Student login.
- Course registration.
- Results checking.
- Profile management.
- Administrator dashboard.

### Assessment Questions

**Authentication**
- Are passwords securely stored?
- Is multifactor authentication available?
- Are login attempts appropriately protected?

**Authorization**
- Can students access only their own records?
- Are administrative functions restricted?
- Are permissions checked on the server?

**Session Management**
- Are session cookies protected?
- Are sessions invalidated after logout?
- Are session identifiers handled securely?

**Input Validation**
- Is user input validated?
- Are database queries parameterized?
- Is untrusted content safely handled?

**Configuration**
- Is production debugging disabled?
- Are unnecessary services disabled?
- Are software components maintained?

Write at least five hypothetical findings and propose a remediation for each.

Clearly label the project as a fictional assessment.

## 14. Practical Project 3: Evidence Integrity and Reporting

### Objective

Create a small evidence package for a security assessment.

Create the directory:

```bash
mkdir -p ~/module16-lab/lesson14/project3/evidence
cd ~/module16-lab/lesson14/project3
```

Create a sample observation:

```bash
echo "Local training environment inspected successfully." > evidence/observation.txt
```

Calculate its hash:

```bash
sha256sum evidence/observation.txt
```

Save the hash:

```bash
sha256sum evidence/observation.txt > evidence/observation.sha256
```

Verify it:

```bash
sha256sum -c evidence/observation.sha256
```

Record:
- Evidence ID.
- Evidence description.
- Collection date.
- Collection method.
- SHA-256 hash.
- Storage location.

## 15. Practical Project 4: Vulnerability Prioritization

Consider these fictional findings:

| ID | Finding | Impact |
|---|---|---|
| F-01 | Exposed administrator credentials | Unauthorized administrative access |
| F-02 | Outdated application component | Potential software compromise |
| F-03 | Missing security header | Reduced browser-side protection |
| F-04 | Weak user access controls | Unauthorized information access |
| F-05 | Missing security monitoring | Delayed incident detection |

Your task:

1. Assign a severity to each finding.
2. Explain your reasoning.
3. Recommend remediation.
4. Decide which finding should be addressed first.
5. Explain why your prioritization makes sense.

Remember that severity depends on context and supporting evidence.

## 16. Create Your Final Penetration Testing Report

Create the report:

```bash
nano penetration-testing-final-report.txt
```

Use this structure:

```text
MODULE 16 - LESSON 14
PENETRATION TESTING FINAL REPORT

DOCUMENT INFORMATION

Report Title:
Assessment Name:
Organization:
Prepared By:
Report Date:
Classification:

1. EXECUTIVE SUMMARY

Assessment Purpose:
Overall Security Posture:
Major Findings:
Key Recommendations:

2. SCOPE AND AUTHORIZATION

Authorized Targets:
Excluded Targets:
Testing Period:
Authorization:
Restrictions:

3. METHODOLOGY

Information Gathering:
Network Assessment:
Service Identification:
Vulnerability Assessment:
Web Application Review:
Risk Analysis:

4. TOOLS USED

Tool 1:
Purpose:
Version:

Tool 2:
Purpose:
Version:

5. FINDINGS SUMMARY

Critical:
High:
Medium:
Low:
Informational:

6. DETAILED FINDINGS

Finding ID:
Finding Title:
Description:
Affected Asset:
Evidence:
Potential Impact:
Severity:
Recommendation:
Retest Status:

7. RISK PRIORITIZATION

Highest Priority Finding:
Reason:
Recommended Remediation Timeline:

8. EVIDENCE HANDLING

Evidence Location:
Integrity Verification:
Access Controls:
Data Classification:

9. CONCLUSION

Overall Assessment:
Major Security Concerns:
Recommended Next Steps:

10. APPENDICES

Command References:
Evidence References:
Additional Notes:
```

Use real observations from your own local assessment where applicable. Mark hypothetical findings clearly.

## 17. Common Reporting Mistakes

1. Writing reports that are difficult to understand.
2. Including findings without evidence.
3. Exaggerating vulnerability severity.
4. Ignoring business impact.
5. Providing vague remediation advice.
6. Including sensitive information unnecessarily.
7. Failing to identify affected assets.
8. Confusing confirmed findings with theoretical risks.
9. Omitting the testing scope.
10. Failing to record retest results.

## 18. Knowledge Check

Answer these questions in your notebook.

1. What is a penetration testing report?
2. What is the purpose of an executive summary?
3. What is the difference between an executive and technical report?
4. What information belongs in the scope section?
5. What does CVSS stand for?
6. What is the purpose of vulnerability severity ratings?
7. What is risk prioritization?
8. What should a vulnerability finding contain?
9. Why is evidence important?
10. What is remediation?
11. What is vulnerability retesting?
12. Why should reports avoid unnecessary sensitive information?
13. What is the difference between a confirmed vulnerability and a theoretical risk?
14. Why should findings be prioritized?
15. What makes a security report professional?

## 19. Practical Thinking Questions

1. Why might management prefer an executive report?
2. Why do developers need technical findings?
3. Why should CVSS not be treated as the complete measure of business risk?
4. Why is a clear remediation recommendation important?
5. What could happen if a security report contains inaccurate evidence?
6. Why should a report identify the affected asset?
7. Why should a tester document excluded systems?
8. How can retesting demonstrate that a security improvement worked?

## 20. Lesson Summary

In this lesson, you learned:

- Penetration testing reports.
- Executive and technical reporting.
- Vulnerability severity.
- CVSS.
- Risk prioritization.
- Writing vulnerability findings.
- Evidence presentation.
- Remediation recommendations.
- Local Linux security assessment.
- Web application security review.
- Evidence integrity.
- Practical security projects.

### Key Takeaway

A penetration test provides value when its findings are communicated accurately and organizations can use the results to reduce security risks.

**NEXT LESSON: MODULE 16 – LESSON 15: MODULE 16 FINAL ASSESSMENT**
