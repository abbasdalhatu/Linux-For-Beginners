
# MODULE 15 – CAPSTONE PROJECT

# LESSON 10: CAPSTONE FINAL PRACTICAL ASSESSMENT

## 1. Assessment Overview

### Project Title

**ISOLATED CYBERSECURITY ASSESSMENT, DEFENSE AND DIGITAL FORENSICS LAB**

### Assessment Objective

This final assessment evaluates your ability to plan, perform, document, and report a controlled cybersecurity assessment using your isolated virtual laboratory.

You will demonstrate practical understanding of:

1. Laboratory architecture.
2. System discovery and enumeration.
3. Security assessment.
4. System hardening.
5. Network monitoring and detection.
6. Digital forensics.
7. Security automation.
8. Investigation and reporting.

### Laboratory Environment

| Component | Purpose |
|---|---|
| Windows 11 | Main host computer |
| VMware | Virtualization platform |
| Kali Linux | Security assessment workstation |
| Metasploitable 2 | Intentionally vulnerable training target |
| Host-only network | Isolated communication between VMs |

**Safety requirement:** All network testing must remain within your isolated laboratory. Do not scan public IP addresses, university networks, or systems belonging to other people.

---

# PART A: LABORATORY VERIFICATION

## 2. Task 1 – Verify the Virtual Lab

On Kali Linux, execute:

```bash
hostnamectl
```

```bash
ip -br addr
```

```bash
ip route
```

On Metasploitable 2:

```bash
ifconfig
```

### Requirements

Record:

- Kali Linux hostname.
- Kali Host-only IP address.
- Metasploitable 2 IP address.
- Host-only subnet.
- VMware network configuration.

Confirm that Metasploitable 2 is connected only to the Host-only network.

### Expected Evidence

Create:

```text
lab-verification.txt
```

Save it inside:

```text
~/practical-labs/module15/
```

---

# PART B: SYSTEM DISCOVERY AND ENUMERATION

## 3. Task 2 – Discover the Target

Use the actual Metasploitable 2 IP address.

Test connectivity:

```bash
ping -c 4 TARGET_IP
```

Perform host discovery:

```bash
sudo nmap -sn TARGET_IP
```

Perform a limited TCP scan:

```bash
sudo nmap -sS --top-ports 20 TARGET_IP
```

Identify service versions:

```bash
sudo nmap -sV --top-ports 20 TARGET_IP
```

Save the results:

```bash
mkdir -p ~/practical-labs/module15/final-assessment/discovery
```

```bash
sudo nmap -sV --top-ports 20 TARGET_IP -oA ~/practical-labs/module15/final-assessment/discovery/final-scan
```

### Requirements

Document:

1. Target IP address.
2. Host discovery result.
3. Open TCP ports.
4. Identified services.
5. Available version information.
6. Any scan limitations.

Do not treat an open port as proof of a vulnerability.

---

# PART C: SECURITY ASSESSMENT

## 4. Task 3 – Analyze Potential Weaknesses

Review the scan output.

Identify at least three observed services, if available.

For each service, document:

- Service name.
- Port number.
- Version information.
- Potential security concern.
- Evidence.
- Further validation required.

Create:

```bash
mkdir -p ~/practical-labs/module15/final-assessment/security
```

Create the report:

```bash
nano ~/practical-labs/module15/final-assessment/security/final-vulnerability-assessment.md
```

Use this structure:

```markdown
# FINAL VULNERABILITY ASSESSMENT

## Target
Metasploitable 2

## Finding 1
Service:
Port:
Version:
Potential Weakness:
Evidence:
Validation Status:
Recommendation:

## Finding 2
Service:
Port:
Version:
Potential Weakness:
Evidence:
Validation Status:
Recommendation:

## Finding 3
Service:
Port:
Version:
Potential Weakness:
Evidence:
Validation Status:
Recommendation:

## Overall Assessment
[Write your conclusion.]
```

Research one applicable service version using a trusted vendor advisory or vulnerability database.

Do not execute exploits.

---

# PART D: SYSTEM HARDENING

## 5. Task 4 – Perform a Hardening Review

Inspect Kali Linux:

```bash
whoami
```

```bash
id
```

```bash
sudo ss -tulpn
```

```bash
systemctl list-units --type=service --state=running
```

```bash
sudo ufw status verbose
```

```bash
sudo nft list ruleset
```

Review file permissions:

```bash
ls -la ~/practical-labs/module15
```

### Requirements

Document:

- User privileges.
- Running services.
- Firewall status.
- File permission observations.
- Software update status.
- Recommended security improvements.

Create:

```text
final-hardening-review.md
```

Save it in:

```text
~/practical-labs/module15/final-assessment/security/
```

This assessment is primarily an inspection exercise. Do not make risky changes to your Kali installation or disable existing protections.

---

# PART E: NETWORK MONITORING

## 6. Task 5 – Capture and Examine Network Traffic

Identify your actual Host-only interface:

```bash
ip -br addr
```

Capture a small number of packets:

```bash
sudo tcpdump -i INTERFACE -c 30 -w ~/practical-labs/module15/final-assessment/final-capture.pcap
```

Generate a few connectivity test packets against your authorized target from another terminal:

```bash
ping -c 4 TARGET_IP
```

Review the capture:

```bash
tcpdump -r ~/practical-labs/module15/final-assessment/final-capture.pcap
```

### Requirements

Record:

- Capture interface.
- Packet count.
- Protocols observed.
- Source and destination addresses.
- Capture limitations.

Calculate a SHA-256 hash:

```bash
sha256sum ~/practical-labs/module15/final-assessment/final-capture.pcap
```

Save the hash:

```bash
sha256sum ~/practical-labs/module15/final-assessment/final-capture.pcap > ~/practical-labs/module15/final-assessment/final-capture.sha256
```

Verify:

```bash
sha256sum -c ~/practical-labs/module15/final-assessment/final-capture.sha256
```

---

# PART F: DIGITAL FORENSICS

## 7. Task 6 – Collect and Preserve Evidence

Create:

```bash
mkdir -p ~/practical-labs/module15/final-assessment/forensics/evidence
```

Collect basic system information:

```bash
{
  echo "FINAL FORENSIC COLLECTION"
  date -Is
  hostname
  whoami
  ip -br addr
  uptime
} > ~/practical-labs/module15/final-assessment/forensics/evidence/system-info.txt
```

Copy the final scan results:

```bash
cp ~/practical-labs/module15/final-assessment/discovery/final-scan.nmap ~/practical-labs/module15/final-assessment/forensics/evidence/
```

Copy the packet capture:

```bash
cp ~/practical-labs/module15/final-assessment/final-capture.pcap ~/practical-labs/module15/final-assessment/forensics/evidence/
```

Calculate hashes:

```bash
cd ~/practical-labs/module15/final-assessment/forensics/evidence
sha256sum * > evidence-hashes.sha256
```

Verify:

```bash
sha256sum -c evidence-hashes.sha256
```

### Requirements

Create:

```text
evidence-inventory.md
```

Document:

- Evidence ID.
- File name.
- Collection time.
- Source.
- SHA-256 hash.
- Storage location.

Keep original evidence files unchanged after collection. Analyze separate copies where necessary.

---

# PART G: SECURITY AUTOMATION

## 8. Task 7 – Execute Your Automation Toolkit

Navigate to your existing automation directory:

```bash
cd ~/practical-labs/module15/automation
```

Run syntax checks:

```bash
for script in *.sh; do
    bash -n "$script" || exit 1
done
```

Execute the main toolkit:

```bash
./run-security-checks.sh
```

Review the generated reports:

```bash
find reports -type f
```

Generate a final toolkit integrity record:

```bash
sha256sum *.sh > final-toolkit-hashes.sha256
```

Verify:

```bash
sha256sum -c final-toolkit-hashes.sha256
```

### Requirements

Document:

- Scripts executed.
- Syntax check results.
- Generated reports.
- Integrity verification.
- Errors or limitations encountered.

Create:

```text
automation-assessment.md
```

Save it in:

```text
~/practical-labs/module15/final-assessment/
```

---

# PART H: FINAL INVESTIGATION REPORT

## 9. Task 8 – Prepare the Final Capstone Report

Create:

```bash
nano ~/practical-labs/module15/final-assessment/final-capstone-report.md
```

Use the following structure:

```markdown
# FINAL CYBERSECURITY CAPSTONE REPORT

## 1. Project Information

Project:
Isolated Cybersecurity Assessment,
Defense and Digital Forensics Lab

Student:
[Your Name]

Assessment Environment:
Kali Linux and Metasploitable 2

Virtualization:
VMware

## 2. Executive Summary

[Summarize the project.]

## 3. Laboratory Architecture

[Describe the isolated virtual environment.]

## 4. Scope and Authorization

[Explain the authorized assessment scope.]

## 5. Discovery and Enumeration

[Summarize actual discovery results.]

## 6. Security Assessment

[Document evidence-supported findings.]

## 7. Hardening Review

[Summarize security improvements.]

## 8. Network Monitoring

[Describe traffic capture and analysis.]

## 9. Digital Forensics

[Describe evidence collected and preserved.]

## 10. Security Automation

[Describe the scripts developed and tested.]

## 11. Risk Assessment

[Summarize identified risks.]

## 12. Recommendations

[Provide practical security recommendations.]

## 13. Limitations

[Explain limitations of the assessment.]

## 14. Conclusion

[Write your final conclusion.]

## 15. Evidence Appendix

[List supporting evidence files and hashes.]
```

Write the report professionally and use actual results from your project.

---

# PART I: FINAL PRACTICAL REVIEW

## 10. Project Quality Checklist

| Assessment Area | Required Evidence |
|---|---|
| Lab architecture | Network architecture documentation |
| Discovery | Nmap results |
| Security assessment | Vulnerability assessment report |
| Hardening | Hardening review |
| Monitoring | PCAP and monitoring report |
| Forensics | Evidence inventory and hashes |
| Automation | Working Bash scripts |
| Investigation | Timeline and correlation report |
| Final reporting | Final capstone report |

### Final Directory Structure

Your completed project should resemble:

```text
module15/
│
├── discovery/
│   └── results/
│
├── security-assessment/
│
├── hardening/
│
├── monitoring/
│
├── forensics/
│
├── automation/
│
├── final-investigation/
│
└── final-assessment/
    ├── discovery/
    ├── security/
    ├── forensics/
    │   └── evidence/
    ├── final-capture.pcap
    ├── final-capture.sha256
    ├── automation-assessment.md
    └── final-capstone-report.md
```

Some directories may contain additional files created in earlier lessons.

---

# PART J: THEORETICAL EXAMINATION

## 11. Section A – Multiple Choice Questions

Choose the correct answer.

**1. What is the primary purpose of system discovery?**

A. Delete unnecessary files  
B. Identify systems in a network  
C. Encrypt all network traffic  
D. Create user accounts  

**2. What does Nmap primarily help with?**

A. Graphic design  
B. Network discovery and security auditing  
C. Video editing  
D. Word processing  

**3. What does CVE identify?**

A. A specific disclosed vulnerability  
B. A network interface  
C. A Linux user account  
D. A firewall rule  

**4. What does the principle of least privilege mean?**

A. Give everyone administrator access  
B. Disable all accounts  
C. Give users only the permissions they need  
D. Allow unrestricted network access  

**5. Which tool captures network packets from the command line?**

A. chmod  
B. tcpdump  
C. passwd  
D. mkdir  

**6. What is an IDS?**

A. Intrusion Detection System  
B. Internet Distribution Service  
C. Internal Data Storage  
D. Information Development Software  

**7. What does SHA-256 help verify?**

A. Screen resolution  
B. File integrity  
C. Network speed  
D. User privileges  

**8. What does chain of custody document?**

A. Software installation  
B. Evidence handling history  
C. Network routing  
D. Password complexity  

**9. What is the purpose of Bash automation?**

A. Automate repetitive tasks  
B. Replace every operating system  
C. Disable all services  
D. Remove all logs  

**10. What should a professional security report contain?**

A. Unsupported assumptions  
B. Evidence, findings, limitations, and recommendations  
C. Only screenshots  
D. Only tool names  

---

## 12. Section B – Short Answer Questions

Answer in your own words.

1. Explain the difference between system discovery and enumeration.
2. Describe the purpose of a vulnerability assessment.
3. Explain CVE, CVSS, and CWE.
4. Describe three system hardening techniques.
5. Explain the difference between IDS and IPS.
6. Why is network traffic monitoring important?
7. Describe the stages of a digital forensic investigation.
8. What is chain of custody?
9. Explain how Bash scripts support security operations.
10. Why is evidence correlation important during investigations?

---

## 13. Section C – Scenario-Based Questions

### Scenario 1: Suspicious Network Service

An organization discovers an unexpected open port on its Linux server.

Questions:

1. What information should the administrator collect?
2. How can the running service be identified?
3. What should be checked before disabling it?
4. How should the finding be documented?

### Scenario 2: Repeated Failed Logins

A Linux server records numerous failed SSH login attempts.

Questions:

1. Which logs should be inspected?
2. What information should be collected?
3. How can the administrator determine whether a login succeeded?
4. What security improvements could be recommended?

### Scenario 3: Evidence Integrity

An investigator calculates a SHA-256 hash for a collected file. Later, the verification fails.

Questions:

1. What does this result indicate?
2. What should the investigator do?
3. Why should the original evidence be preserved?
4. Why should the investigator document the discrepancy?

### Scenario 4: Vulnerable Service

A security assessor discovers an outdated service version.

Questions:

1. How should the assessor validate the potential vulnerability?
2. Which vulnerability references may be useful?
3. How should the risk be evaluated?
4. What remediation should be recommended?

---

# PART K: FINAL PROJECT SUBMISSION

## 14. Submission Requirements

Your final capstone should contain:

1. Laboratory architecture documentation.
2. Nmap discovery results.
3. Security assessment report.
4. Vulnerability register.
5. Hardening review.
6. Network monitoring report.
7. Packet capture.
8. Evidence inventory.
9. Chain-of-custody record.
10. Security automation scripts.
11. Investigation timeline.
12. Evidence correlation report.
13. Final capstone report.

Ensure that the project does not contain passwords, private keys, or unnecessary personal information.

---

## 15. Final Assessment Checklist

- [ ] I verified my virtual laboratory.
- [ ] I completed authorized system discovery.
- [ ] I analyzed observed network services.
- [ ] I completed a security assessment.
- [ ] I reviewed system hardening.
- [ ] I captured and analyzed network traffic.
- [ ] I collected digital evidence.
- [ ] I verified evidence integrity.
- [ ] I executed my automation toolkit.
- [ ] I prepared an investigation timeline.
- [ ] I completed the evidence correlation report.
- [ ] I prepared the final capstone report.
- [ ] I reviewed the theoretical examination.

---

## 16. Final Reflection

Answer these questions in your notebook:

1. What was the most difficult part of this capstone?
2. Which cybersecurity tool did you find most useful?
3. Which practical skill improved the most?
4. What would you improve in your laboratory?
5. Which cybersecurity specialization would you like to explore next?

---

# FINAL MESSAGE

Congratulations on reaching the final assessment of Module 15.

You have progressed from Linux fundamentals through networking, system administration, cybersecurity, vulnerability assessment, penetration-testing laboratory work, digital forensics, and security automation.

The purpose of this capstone is to demonstrate that you can combine these skills into a structured and documented security project.

**A cybersecurity professional does not simply run tools. A professional understands the system, protects the environment, evaluates evidence, identifies risks, and communicates findings responsibly.**
