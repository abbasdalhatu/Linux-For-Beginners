
# MODULE 15 – CAPSTONE PROJECT

# LESSON 04: SECURITY ASSESSMENT AND ANALYSIS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain security assessment.
2. Understand vulnerabilities, threats, and risks.
3. Analyze network discovery results.
4. Identify potentially insecure services.
5. Understand CVE, CVSS, and CWE.
6. Prioritize security weaknesses.
7. Create a vulnerability register.
8. Prepare a security assessment report.

---

## 2. What Is a Security Assessment?

A security assessment is a structured examination of systems, applications, networks, and configurations to identify security weaknesses.

The purpose is to understand how well a system is protected and determine what improvements are necessary.

### Example

Imagine an organization operates a Linux server with the following services:

- FTP.
- SSH.
- Telnet.
- HTTP.
- Database service.

A security assessor may investigate:

- Whether unnecessary services are running.
- Whether authentication is secure.
- Whether software is outdated.
- Whether services expose sensitive information.
- Whether firewall rules are appropriate.

The objective is to identify risks and recommend improvements.

---

## 3. Understanding Important Security Terms

### 3.1 Asset

An asset is something valuable that requires protection.

Examples:

- Servers.
- Computers.
- Databases.
- User accounts.
- Business documents.
- Network infrastructure.

### 3.2 Threat

A threat is something capable of causing harm to an asset.

Examples:

- Malware.
- Unauthorized access.
- Phishing.
- Insider misuse.
- Hardware failure.

### 3.3 Vulnerability

A vulnerability is a weakness that could be exploited or otherwise cause a security problem.

Examples:

- Weak passwords.
- Outdated software.
- Incorrect permissions.
- Unnecessary exposed services.
- Insecure configurations.

### 3.4 Risk

Risk describes the potential impact and likelihood of an adverse event.

A simple conceptual relationship is:

\[
Risk \approx Likelihood \times Impact
\]

This is a simplified risk model. Organizations may use more detailed methods.

---

## 4. Vulnerability vs Threat vs Risk

| Concept | Meaning | Example |
|---|---|---|
| Threat | Potential source of harm | Malware |
| Vulnerability | Security weakness | Outdated service |
| Risk | Potential resulting loss | Unauthorized system access |

### Practical Example

Suppose a server runs an outdated service.

- Asset: Linux server.
- Threat: Unauthorized attacker.
- Vulnerability: Software weakness.
- Risk: Possible unauthorized access or service disruption.

Understanding these differences is essential for professional security analysis.

---

## 5. Common Security Weaknesses

### 5.1 Weak Authentication

Examples:

- Default credentials.
- Weak passwords.
- Shared accounts.
- Lack of multi-factor authentication.

### 5.2 Unnecessary Open Ports

Services may expose network functionality that an organization does not need.

Examples:

- Unused FTP.
- Unnecessary Telnet.
- Unneeded database listeners.

An open port is not automatically vulnerable. The service and its configuration must be examined.

### 5.3 Outdated Software

Unsupported or unpatched software may contain known vulnerabilities.

### 5.4 Insecure Configuration

Examples:

- Excessive permissions.
- Unnecessary services.
- Insecure firewall rules.
- Poor access controls.

### 5.5 Insufficient Monitoring

Without appropriate logging and monitoring, suspicious activity may remain undetected.

---

## 6. Understanding CVE

CVE stands for Common Vulnerabilities and Exposures.

It provides standardized identifiers for publicly disclosed cybersecurity vulnerabilities.

Example format:

```text
CVE-2021-44228
```

A CVE identifier helps security professionals refer to a specific disclosed vulnerability.

### Why CVE Matters

It helps with:

- Vulnerability research.
- Security advisories.
- Patch management.
- Incident response.
- Vulnerability reporting.

A CVE identifier alone does not prove that a particular system is vulnerable.

You must check whether the affected product and version match the system under examination.

---

## 7. Understanding CVSS

CVSS stands for Common Vulnerability Scoring System.

It provides a standardized way to communicate the severity of vulnerabilities.

CVSS v3.1 commonly uses these severity ranges:

| Score | Severity |
|---|---|
| 0.0 | None |
| 0.1–3.9 | Low |
| 4.0–6.9 | Medium |
| 7.0–8.9 | High |
| 9.0–10.0 | Critical |

CVSS v4.0 has its own scoring framework and terminology, so always identify which version is being used.

### Important

CVSS measures vulnerability severity. It does not automatically represent the full business risk of an organization's specific environment.

---

## 8. Understanding CWE

CWE stands for Common Weakness Enumeration.

It provides standardized classifications of software and hardware weaknesses.

Examples include:

- CWE-79: Cross-Site Scripting.
- CWE-89: SQL Injection.
- CWE-287: Improper Authentication.
- CWE-269: Improper Privilege Management.

### CVE vs CWE

| CVE | CWE |
|---|---|
| Identifies a specific disclosed vulnerability | Classifies a type of weakness |
| Example: CVE-2021-44228 | Example: CWE-79 |
| Used to identify vulnerability records | Used to categorize weakness patterns |

---

## 9. Security Assessment Methodology

A structured assessment follows these steps.

### Step 1: Define Scope

Identify:

- Authorized target.
- Network boundaries.
- Assessment objectives.
- Testing restrictions.

### Step 2: Collect Information

Review:

- IP addresses.
- Open ports.
- Running services.
- Service versions.
- System configuration.

### Step 3: Identify Potential Weaknesses

Look for:

- Outdated services.
- Unnecessary ports.
- Insecure protocols.
- Weak authentication configurations.
- Excessive permissions.

### Step 4: Validate Findings

Confirm that the suspected weakness actually applies.

Validation may involve:

- Checking the exact product and version.
- Reviewing trusted vendor advisories.
- Examining configuration.
- Confirming patch status.
- Reviewing supporting evidence.

Do not assume that a banner alone proves exploitability.

### Step 5: Assess Risk

Consider:

- Likelihood.
- Business impact.
- Exposure.
- Asset importance.
- Existing security controls.

### Step 6: Recommend Remediation

Suggest appropriate improvements.

### Step 7: Document Results

Prepare a vulnerability register and security assessment report.

---

## 10. Practical Lab A: Review Your Nmap Results

Open the discovery report created in Lesson 03.

```bash
nano ~/practical-labs/module15/discovery/results/target-assessment.nmap
```

Review the discovered services.

For each open port, record:

1. Port number.
2. Service name.
3. Version information, if available.
4. Whether the service is expected.
5. Possible security concerns.
6. Further validation required.

Do not invent findings. Use only the results you actually collected.

---

## 11. Practical Lab B: Identify Potentially Insecure Services

Review the following examples.

| Service | Potential Concern |
|---|---|
| FTP | May transmit credentials without encryption |
| Telnet | Sends traffic without modern transport encryption |
| SSH | Requires secure configuration and authentication |
| HTTP | Traffic may lack transport encryption |
| SMB | Requires appropriate access controls and secure configuration |
| Database service | Should not be exposed unnecessarily |

### Your Task

Identify which of these services appeared in your actual Nmap results.

For each service, write:

- Observed service.
- Potential security concern.
- Evidence source.
- Recommended review.

Remember: The presence of a service does not automatically prove a vulnerability.

---

## 12. Practical Lab C: Research a Potential Vulnerability

Choose one service version actually identified in your scan.

Use a trusted source to research its security status, such as:

- NIST National Vulnerability Database: https://nvd.nist.gov/
- Official vendor security advisories.
- Official project security documentation.

Check:

1. Exact product name.
2. Exact version.
3. Whether a relevant CVE exists.
4. Whether the installed version is affected.
5. Whether a security patch is available.
6. What mitigation the vendor recommends.

Do not install or execute an exploit.

If you cannot establish that the version is affected, record the finding as requiring further validation.

---

## 13. Practical Lab D: Create a Vulnerability Register

Create a directory:

```bash
mkdir -p ~/practical-labs/module15/security-assessment
```

Create the register:

```bash
cat > ~/practical-labs/module15/security-assessment/vulnerability-register.csv <<'EOF'
Finding ID,Asset,Observed Service,Potential Weakness,Evidence,Severity,Status,Recommendation
VUL-001,Metasploitable 2,FTP,Review transport security,Nmap scan,To assess,Unconfirmed,Review service configuration
VUL-002,Metasploitable 2,Telnet,Unencrypted remote access,Nmap scan,To assess,Unconfirmed,Replace with secure remote administration
VUL-003,Metasploitable 2,HTTP,Review application security,Nmap scan,To assess,Unconfirmed,Review application and transport security
EOF
```

These are example assessment entries. Verify whether each service was actually observed before treating it as a finding.

Open the register:

```bash
nano ~/practical-labs/module15/security-assessment/vulnerability-register.csv
```

Replace or remove entries that do not match your actual evidence.

---

## 14. Practical Lab E: Create a Risk Assessment

Create:

```bash
cat > ~/practical-labs/module15/security-assessment/risk-assessment.md <<'EOF'
# SECURITY RISK ASSESSMENT

## Project
Isolated Cybersecurity Assessment and Defense Lab

## Assessment Scope
Authorized Metasploitable 2 virtual machine.

## Risk Method
Qualitative assessment based on likelihood and impact.

## Finding 1
Asset: Metasploitable 2
Weakness: Potential insecure remote access service
Likelihood: To be assessed
Impact: Potential unauthorized access
Risk: Requires validation

## Finding 2
Asset: Metasploitable 2
Weakness: Potential outdated service
Likelihood: To be assessed
Impact: Depends on applicable vulnerability
Risk: Requires version and advisory verification

## Limitations
Findings are not confirmed vulnerabilities unless
supported by sufficient technical evidence.

## Recommendation
Validate service configurations and versions.
Prioritize weaknesses based on actual exposure,
likelihood, and impact.
EOF
```

Edit the report with your actual observations.

---

## 15. Practical Lab F: Generate a Security Assessment Report

Create:

```bash
cat > ~/practical-labs/module15/security-assessment/security-assessment-report.md <<'EOF'
# SECURITY ASSESSMENT REPORT

## 1. Project Information

Project:
Isolated Cybersecurity Assessment and Defense Lab

Assessment:
Security Assessment and Analysis

Target:
Authorized Metasploitable 2 virtual machine

## 2. Scope

The assessment was restricted to the isolated
VMware Host-only laboratory.

## 3. Methodology

- Review of network discovery results.
- Review of open ports.
- Service identification.
- Version information analysis.
- Security advisory research.
- Risk assessment.

## 4. Findings

[Enter only findings supported by your evidence.]

## 5. Potential Security Concerns

[Describe observed services and their security implications.]

## 6. Vulnerability Validation

[Record confirmed, unconfirmed, and not-applicable findings.]

## 7. Risk Assessment

[Document likelihood, impact, and priority.]

## 8. Recommendations

- Remove unnecessary services.
- Use secure communication protocols.
- Update affected software.
- Apply appropriate access controls.
- Improve monitoring.

## 9. Limitations

The assessment was limited to the authorized training target.

No destructive exploitation was performed.

## 10. Conclusion

The assessment identified areas requiring security review
and established recommendations for the next hardening phase.
EOF
```

---

## 16. Understanding Remediation Priorities

Not every security weakness needs the same response.

A useful prioritization approach:

| Priority | Typical Response |
|---|---|
| Critical | Immediate attention |
| High | Urgent remediation |
| Medium | Planned remediation |
| Low | Routine improvement |

Actual priority should reflect the organization's risk, asset importance, exposure, and available controls.

---

## 17. Knowledge Check

1. What is a security assessment?
2. Explain the difference between a threat and a vulnerability.
3. What is risk?
4. What does CVE stand for?
5. What is CVSS used for?
6. What does CWE classify?
7. Why is an open port not automatically a vulnerability?
8. Why must service versions be validated?
9. What information belongs in a vulnerability register?
10. Why should security findings be prioritized?

---

## 18. Practical Completion Checklist

- [ ] I reviewed my Nmap results.
- [ ] I identified observed services.
- [ ] I documented potential security concerns.
- [ ] I researched at least one service or version.
- [ ] I understand CVE, CVSS, and CWE.
- [ ] I created a vulnerability register.
- [ ] I created a risk assessment.
- [ ] I created a security assessment report.
- [ ] I separated confirmed findings from unconfirmed observations.
- [ ] I stayed within my authorized isolated laboratory.

---

## 19. Key Takeaway

Security assessment is not simply finding open ports or running vulnerability scanners.

A professional assessor must understand the evidence, validate potential weaknesses, evaluate their risks, and recommend practical remediation.

**Remember: A suspected vulnerability is not a confirmed vulnerability until sufficient evidence supports it.**
