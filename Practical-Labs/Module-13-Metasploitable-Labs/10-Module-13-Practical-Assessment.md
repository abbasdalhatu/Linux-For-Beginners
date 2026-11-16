
# MODULE 13 – METASPLOITABLE LABS

# LESSON 10: FINAL PRACTICAL ASSESSMENT

## 1. Assessment Overview

This assessment evaluates your understanding of the Metasploitable 2 penetration testing lab.

You will demonstrate your ability to:

1. Configure an isolated virtual lab.
2. Identify the target system.
3. Discover network services.
4. Analyze open ports.
5. Identify potential vulnerabilities.
6. Research vulnerabilities using CVE and CVSS.
7. Navigate Metasploit.
8. Perform safe vulnerability validation.
9. Collect and preserve evidence.
10. Prepare a professional security assessment report.

### Assessment Rules

- Use only your own Kali and Metasploitable virtual machines.
- Keep the target on the VMware Host-only network.
- Do not scan public IP addresses.
- Do not test systems without authorization.
- Do not use stealth or evasion techniques.
- Do not access real user data.
- Do not create persistence mechanisms.
- Do not execute destructive exploits.

This assessment emphasizes safe testing, technical understanding, and professional documentation.

---

## 2. Assessment Workspace

Create your final assessment directory:

```bash
mkdir -p ~/practical-labs/module13/final-assessment
cd ~/practical-labs/module13/final-assessment
```

Create these files:

```bash
touch lab-configuration.md
touch discovery-report.md
touch service-analysis.md
touch vulnerability-assessment.md
touch metasploit-review.md
touch evidence-report.md
touch final-security-report.md
```

Your directory should look like:

```text
final-assessment/
├── lab-configuration.md
├── discovery-report.md
├── service-analysis.md
├── vulnerability-assessment.md
├── metasploit-review.md
├── evidence-report.md
└── final-security-report.md
```

---

# PART A – LAB CONFIGURATION

## Task 1: Verify the Virtual Lab

Start:

- Kali Linux VM.
- Metasploitable 2 VM.

Inspect Kali networking:

```bash
ip -br addr
```

Inspect routing:

```bash
ip route
```

Confirm your VMware network configuration.

### Requirements

Document:

- Kali IP address.
- Metasploitable IP address.
- VMware network mode.
- Network isolation status.
- Date of assessment.

Do not continue if the target is not isolated as intended.

### Save in:

`lab-configuration.md`

---

# PART B – HOST DISCOVERY

## Task 2: Verify Target Connectivity

Set your actual target IP:

```bash
TARGET=YOUR_METASPLOITABLE_IP
```

Test connectivity:

```bash
ping -c 4 "$TARGET"
```

Perform a host discovery check:

```bash
nmap -sn "$TARGET"
```

Save the output:

```bash
nmap -sn "$TARGET" -oN discovery-scan.txt
```

### Requirements

Document:

- Target IP.
- Whether the host responded.
- Discovery method.
- Scan date.
- Any limitations.

### Save in:

`discovery-report.md`

---

# PART C – SERVICE AND PORT ANALYSIS

## Task 3: Perform a Controlled Port Scan

Run a limited TCP port scan:

```bash
nmap --top-ports 20 "$TARGET" -oN top-ports.txt
```

Review the output:

```bash
cat top-ports.txt
```

Identify the open ports.

Next, perform targeted service detection on ports discovered in your previous lessons.

Example:

```bash
nmap -sV -p 21,22,23,80 "$TARGET" -oN service-detection.txt
```

Only examine ports relevant to your actual findings.

### Requirements

Document:

- Port numbers.
- Protocols.
- Service names.
- Detected versions.
- Security concerns.
- Recommended defensive controls.

### Save in:

`service-analysis.md`

---

# PART D – VULNERABILITY ASSESSMENT

## Task 4: Research Potential Weaknesses

Select at least one service discovered during your assessment.

Research its software version using:

- https://nvd.nist.gov/
- https://www.cve.org/
- Official vendor advisories.

Record:

- Software name.
- Detected version.
- Potential weakness.
- Related CVE, if applicable.
- CVSS score, if available.
- Affected version range.
- Verification status.
- Recommended remediation.

Remember:

A service being open does not prove that it is vulnerable.

A software version match is not sufficient proof without checking the affected version range and patch status.

### Save in:

`vulnerability-assessment.md`

---

# PART E – METASPLOIT FRAMEWORK REVIEW

## Task 5: Demonstrate Metasploit Familiarity

Start Metasploit:

```bash
msfconsole
```

Run:

```text
help
version
search ssh_version
use auxiliary/scanner/ssh/ssh_version
info
show options
back
exit
```

### Requirements

Document:

- Metasploit version.
- Module searched.
- Module category.
- Module description.
- Important options.
- What the module is designed to do.

Do not execute the module or any exploit.

### Save in:

`metasploit-review.md`

---

# PART F – EVIDENCE COLLECTION

## Task 6: Organize Assessment Evidence

Create:

```bash
mkdir -p evidence
```

Copy your relevant scan reports into the evidence directory.

For example:

```bash
cp discovery-scan.txt evidence/
cp top-ports.txt evidence/
cp service-detection.txt evidence/
```

If you used different filenames, copy the actual files.

Generate SHA-256 hashes:

```bash
cd evidence
sha256sum * > SHA256SUMS.txt
```

Verify the files:

```bash
sha256sum -c SHA256SUMS.txt
```

### Requirements

Document:

- Evidence filenames.
- Evidence descriptions.
- SHA-256 hashes.
- Integrity verification results.
- Any missing evidence.

### Save in:

`evidence-report.md`

---

# PART G – FINAL SECURITY REPORT

## Task 7: Prepare Your Professional Report

Open:

```bash
nano final-security-report.md
```

Use this structure:

```markdown
# METASPLOITABLE 2 SECURITY ASSESSMENT REPORT

## 1. Executive Summary

Summarize the purpose and outcome of the assessment.

## 2. Assessment Scope

Target:
Target IP:
Testing Machine:
Network Mode:
Assessment Date:

## 3. Methodology

Describe:

- Lab configuration
- Host discovery
- Port scanning
- Service identification
- Vulnerability research
- Metasploit module review
- Evidence collection

## 4. Discovered Services

List the services and open ports identified.

## 5. Vulnerability Findings

### Finding 1

Title:
Affected Service:
Port:
Evidence:
Related CVE:
Severity:
Verification Status:
Potential Impact:
Recommendation:

## 6. Metasploit Review

Describe the modules researched and the commands used.

## 7. Evidence Integrity

Describe the SHA-256 verification results.

## 8. Security Recommendations

List the most important defensive improvements.

## 9. Assessment Limitations

Describe what was not tested.

## 10. Conclusion

Summarize the overall security assessment.
```

Use factual observations only.

Do not claim successful exploitation, unauthorized access, or privilege escalation because this assessment does not require those activities.

---

# PART H – THEORETICAL EXAMINATION

Answer these questions in your notebook or in a separate file.

## Section A – Fundamentals

1. What is Metasploitable 2?
2. Why is network isolation important?
3. What is the difference between NAT and Host-only networking?
4. What is host discovery?
5. What is a network port?

## Section B – Enumeration

6. What does `nmap -sn` do?
7. What does `nmap -sV` do?
8. What is the difference between an open and filtered port?
9. What is a service banner?
10. Why is service version detection important?

## Section C – Vulnerability Assessment

11. Define vulnerability.
12. What is CVE?
13. What is CVSS?
14. What is CWE?
15. Explain false positives and false negatives.

## Section D – Metasploit

16. What is MSFconsole?
17. What is an exploit module?
18. What is a payload?
19. What is an auxiliary module?
20. What is Meterpreter?

## Section E – Evidence and Reporting

21. What is post-exploitation?
22. What is privilege escalation?
23. What is lateral movement?
24. Why is SHA-256 useful?
25. What makes a professional security report reliable?

---

# PART I – PRACTICAL SCENARIOS

## Scenario 1: Insecure Remote Access

Your scan identifies Telnet running on TCP port 23.

Questions:

1. What security concern does this create?
2. Why is Telnet considered insecure?
3. What secure alternative could replace it?
4. What evidence would you collect?
5. What remediation would you recommend?

## Scenario 2: Exposed Database

Your scan identifies a database service listening on TCP port 3306.

Questions:

1. Which database commonly uses this port?
2. Does an open database port prove a vulnerability?
3. What risks can unnecessary database exposure create?
4. How should database access be restricted?

## Scenario 3: Outdated Software

Your service scan identifies an old software version.

Questions:

1. How would you research its vulnerabilities?
2. What is the purpose of CVE?
3. Why must the affected version range be verified?
4. How would you prioritize remediation?

## Scenario 4: Evidence Integrity

You collect three scan reports and calculate their SHA-256 hashes.

Questions:

1. What does SHA-256 produce?
2. Why should original evidence be preserved?
3. What does a successful hash verification indicate?
4. Why should hash records also be protected?

---

# PART J – FINAL ASSESSMENT CHECKLIST

Use this checklist to evaluate your progress.

- [ ] Lab isolation verified.
- [ ] Kali and Metasploitable IP addresses documented.
- [ ] Target connectivity tested.
- [ ] Host discovery completed.
- [ ] Controlled port scan completed.
- [ ] Service identification completed.
- [ ] At least one software version researched.
- [ ] Vulnerability findings documented accurately.
- [ ] Metasploit module inspected.
- [ ] Evidence organized.
- [ ] SHA-256 hashes generated.
- [ ] Evidence integrity verified.
- [ ] Final security report completed.
- [ ] Theoretical examination completed.
- [ ] Practical scenarios reviewed.

---

# PART K – ASSESSMENT SCORING

| Assessment Area | Marks |
|---|---:|
| Lab configuration and isolation | 10 |
| Host discovery | 10 |
| Port and service analysis | 15 |
| Vulnerability research | 20 |
| Metasploit fundamentals | 10 |
| Evidence collection and integrity | 15 |
| Final security report | 10 |
| Theory and practical scenarios | 10 |
| **TOTAL** | **100** |

### Performance Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Needs Improvement |

---

# MODULE 13 COMPLETION

Upon completing this assessment, you should be able to explain and demonstrate the fundamentals of an isolated penetration testing lab.

You have studied:

1. Metasploitable lab architecture.
2. VMware network isolation.
3. Metasploitable setup.
4. Host discovery and enumeration.
5. Service and port analysis.
6. Vulnerability identification.
7. Metasploit framework fundamentals.
8. Safe exploitation and validation.
9. Post-exploitation concepts and evidence collection.
10. Final security assessment and reporting.

## Final Professional Reminder

A professional penetration tester does not simply run tools.

A professional understands:

- What is being tested.
- Why it is being tested.
- Whether authorization exists.
- What evidence supports the findings.
- What risks the testing creates.
- How to recommend effective remediation.

**Your objective is to become a security professional who can identify weaknesses, validate findings responsibly, preserve evidence, and communicate security risks clearly.**

---

# END OF MODULE 13
