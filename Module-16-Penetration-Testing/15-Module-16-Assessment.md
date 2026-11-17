---
---


# MODULE 16 – LESSON 15
# PENETRATION TESTING FINAL ASSESSMENT

## 1. Assessment Information

**Module:** 16 – Penetration Testing

**Assessment Type:** Theory and Practical

**Total Marks:** 100

**Pass Mark:** 50%

**Recommended Duration:** 2–3 Hours

### Assessment Objectives

This assessment evaluates your understanding of:

- Penetration testing fundamentals.
- Authorization and scope.
- Penetration testing methodology.
- Information gathering and OSINT.
- Network scanning fundamentals.
- Service identification.
- Enumeration.
- Vulnerability assessment.
- Vulnerability validation and risk.
- Web application security.
- Password security.
- Exploitation fundamentals.
- Evidence handling.
- Penetration testing reports.

## 2. Section A – Multiple Choice Questions

**20 Questions × 2 Marks = 40 Marks**

Choose the correct answer.

### Question 1

What is penetration testing?

A. Installing an operating system.

B. An authorized security assessment that identifies and evaluates weaknesses.

C. Deleting system files.

D. Developing mobile applications.

### Question 2

What must be obtained before conducting a penetration test?

A. A new laptop.

B. A public IP address.

C. Proper authorization.

D. An administrator password.

### Question 3

What is the purpose of a Rules of Engagement document?

A. To define testing permissions, scope, and restrictions.

B. To install software.

C. To create user accounts.

D. To configure a database.

### Question 4

Which phase involves collecting information about a target?

A. Reporting.

B. Reconnaissance.

C. Remediation.

D. Recovery.

### Question 5

Which Linux command displays network interfaces and IP addresses?

A. `ls`

B. `ip addr`

C. `mkdir`

D. `chmod`

### Question 6

Which command displays listening TCP and UDP sockets?

A. `ss -tuln`

B. `pwd`

C. `whoami`

D. `cat`

### Question 7

What is enumeration?

A. Removing vulnerabilities.

B. Collecting detailed information about discovered systems and services.

C. Formatting a disk.

D. Encrypting a file.

### Question 8

What is a vulnerability?

A. A system weakness that could affect security.

B. A strong password.

C. A network cable.

D. A software license.

### Question 9

What does CVSS measure?

A. Internet speed.

B. Computer storage.

C. Vulnerability severity.

D. Processor performance.

### Question 10

What is a false positive?

A. A vulnerability that has been fixed.

B. A reported vulnerability that is not actually present.

C. A successful attack.

D. A system backup.

### Question 11

What does HTTPS use to protect web communication?

A. TLS.

B. FTP.

C. Telnet.

D. DNS.

### Question 12

Which OWASP category describes unauthorized access to resources?

A. Broken Access Control.

B. Security Logging and Monitoring Failures.

C. Insecure Design.

D. Cryptographic Failures.

### Question 13

What is password hashing?

A. Recovering a forgotten password.

B. Transforming a password into a derived value for verification.

C. Sending passwords through email.

D. Encrypting an entire hard drive.

### Question 14

Which is an appropriate password-hashing algorithm?

A. MD5 alone.

B. Plaintext.

C. Argon2id.

D. Base64.

### Question 15

What is credential stuffing?

A. Using exposed username-password combinations against other services.

B. Creating a new user account.

C. Encrypting credentials.

D. Updating an application.

### Question 16

What is privilege escalation?

A. Reducing user permissions.

B. Obtaining permissions beyond those originally intended.

C. Deleting user accounts.

D. Changing a desktop theme.

### Question 17

What is the purpose of a VMware snapshot?

A. To increase internet speed.

B. To create a recovery point for a virtual machine.

C. To change an IP address.

D. To scan a network.

### Question 18

What is chain of custody?

A. A list of installed packages.

B. A record of how evidence is collected, stored, accessed, and transferred.

C. A network configuration.

D. A password policy.

### Question 19

Which command calculates a SHA-256 file hash?

A. `sha256sum`

B. `ip route`

C. `ss -tuln`

D. `uname`

### Question 20

What is the primary purpose of a penetration testing report?

A. To hide vulnerabilities.

B. To document findings, risks, evidence, and recommendations.

C. To delete the target system.

D. To publish passwords.

## 3. Section B – Short Answer Questions

**10 Questions × 3 Marks = 30 Marks**

Answer each question clearly.

1. Define penetration testing and explain two reasons organizations perform it.

2. Differentiate between vulnerability assessment and penetration testing.

3. Explain the importance of authorization and scope.

4. Describe the difference between reconnaissance and enumeration.

5. Explain the purpose of service and version identification.

6. Differentiate between a false positive and a false negative.

7. Explain three common web application vulnerabilities.

8. Differentiate between password hashing and encryption.

9. Explain vertical and horizontal privilege escalation.

10. Describe the importance of evidence integrity and chain of custody.

## 4. Section C – Practical Assessment

**3 Questions × 10 Marks = 30 Marks**

Complete the following practical tasks on your own Kali Linux virtual machine.

Do not scan or exploit unauthorized systems.

### Practical 1: Linux System and Network Inspection

**10 Marks**

Create a directory:

```bash
mkdir -p ~/module16-lab/final-assessment/practical1
cd ~/module16-lab/final-assessment/practical1
```

Run:

```bash
cat /etc/os-release
```

```bash
uname -a
```

```bash
ip addr
```

```bash
ip route
```

```bash
ss -tuln
```

Record the following:

1. Operating system.
2. Kernel version.
3. Network interface.
4. IP address.
5. Default route, if present.
6. Listening TCP and UDP services.

**Marking Guide:**

| Requirement | Marks |
|---|---:|
| Operating system identification | 2 |
| Kernel identification | 1 |
| Network interface and IP | 2 |
| Routing information | 2 |
| Listening services | 3 |
| **Total** | **10** |

### Practical 2: Evidence Integrity Verification

**10 Marks**

Create a directory:

```bash
mkdir -p ~/module16-lab/final-assessment/practical2
cd ~/module16-lab/final-assessment/practical2
```

Create a sample evidence file:

```bash
echo "Module 16 final assessment evidence." > evidence.txt
```

Calculate its SHA-256 hash:

```bash
sha256sum evidence.txt
```

Save the hash:

```bash
sha256sum evidence.txt > evidence.sha256
```

Verify the evidence:

```bash
sha256sum -c evidence.sha256
```

Modify the file:

```bash
echo "Additional information." >> evidence.txt
```

Verify it again:

```bash
sha256sum -c evidence.sha256
```

Record:

1. Original hash.
2. First verification result.
3. Second verification result.
4. Why the second verification failed.
5. Why evidence integrity matters.

**Marking Guide:**

| Requirement | Marks |
|---|---:|
| Correct file creation | 2 |
| SHA-256 calculation | 2 |
| Successful initial verification | 2 |
| Demonstration of modified-file failure | 2 |
| Explanation of integrity | 2 |
| **Total** | **10** |

### Practical 3: Penetration Testing Report

**10 Marks**

Create a directory:

```bash
mkdir -p ~/module16-lab/final-assessment/practical3
cd ~/module16-lab/final-assessment/practical3
```

Create:

```bash
nano final-assessment-report.txt
```

Use this structure:

```text
MODULE 16 - FINAL PENETRATION TESTING ASSESSMENT

1. ASSESSMENT INFORMATION

Assessment Name:
Date:
Testing Machine:
Authorized Scope:

2. SYSTEM INFORMATION

Operating System:
Kernel:
Network Interface:
IP Address:

3. SECURITY OBSERVATIONS

Observation 1:
Description:
Evidence:

Observation 2:
Description:
Evidence:

Observation 3:
Description:
Evidence:

4. RISK ANALYSIS

Finding:
Potential Impact:
Severity:
Recommended Action:

5. EVIDENCE INTEGRITY

Evidence File:
Hash Algorithm:
Verification Result:

6. SECURITY RECOMMENDATIONS

Recommendation 1:
Recommendation 2:
Recommendation 3:

7. CONCLUSION

Overall Assessment:
Important Findings:
Recommended Next Steps:
```

Use actual observations from your own Kali VM. Do not invent vulnerabilities or claim to have exploited a system.

**Marking Guide:**

| Requirement | Marks |
|---|---:|
| Assessment information | 1 |
| System information | 2 |
| Security observations | 2 |
| Risk analysis | 2 |
| Evidence integrity | 1 |
| Recommendations and conclusion | 2 |
| **Total** | **10** |

## 5. Assessment Submission Checklist

- [ ] Section A: 20 multiple-choice questions.
- [ ] Section B: 10 short-answer questions.
- [ ] Section C Practical 1: Linux system and network inspection.
- [ ] Section C Practical 2: Evidence integrity verification.
- [ ] Section C Practical 3: Penetration testing report.

## 6. Grading

| Score | Result |
|---|---|
| 80–100 | Excellent |
| 70–79 | Very Good |
| 60–69 | Good |
| 50–59 | Pass |
| 0–49 | Not Yet Passed |

**Pass mark: 50/100**

## 7. Final Assessment Advice

1. Read each question carefully.
2. Use precise cybersecurity terminology.
3. Explain concepts in your own words.
4. Show your practical command outputs.
5. Keep your evidence organized.
6. Distinguish confirmed observations from hypothetical findings.
7. Follow authorization and lab-safety requirements.

## 8. Module 16 Completion

Upon completing this assessment, you should be able to:

- Explain penetration testing principles.
- Define scope and authorization.
- Perform basic information gathering.
- Understand network scanning and enumeration.
- Identify and assess vulnerabilities.
- Understand web application security.
- Explain password security.
- Understand exploitation fundamentals.
- Preserve digital evidence.
- Prepare professional penetration testing reports.

### Final Message

A professional penetration tester must combine technical knowledge, critical thinking, authorization, discipline, and responsible reporting.

**Remember: The goal is not simply to find weaknesses. The goal is to help make systems more secure.**

**NEXT MODULE: MODULE 17 – ADVANCED LINUX**
