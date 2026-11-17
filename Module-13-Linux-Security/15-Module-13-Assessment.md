---
---


# Module 13 – Lesson 15: Linux Security Assessment

## 1. Assessment Overview

This assessment evaluates your understanding of Linux security concepts and practical skills covered throughout Module 13.

### Topics Covered

1. Introduction to Linux Security.
2. Linux Security Principles.
3. User Account Security.
4. File Permissions and Security.
5. Sudo and Privilege Management.
6. Authentication and Password Security.
7. SSH Security Hardening.
8. Linux Firewall Fundamentals.
9. Security Updates and Patching.
10. Linux System Hardening.
11. Linux Security Auditing.
12. Intrusion Detection and Monitoring.
13. Malware and Rootkit Detection.
14. Practical Linux Security Projects.

### Assessment Objectives

You should demonstrate that you can:

- Explain important Linux security concepts.
- Identify common security weaknesses.
- Inspect Linux security configurations.
- Understand security monitoring.
- Identify malware indicators.
- Perform basic security assessments.
- Recommend appropriate security improvements.

---

# PART A: MULTIPLE-CHOICE QUESTIONS

Choose the correct answer.

### Question 1

What is the primary purpose of Linux security?

A. To increase screen brightness.

B. To protect system resources and information.

C. To increase file sizes.

D. To remove all system users.

### Question 2

Which principle states that users should receive only the permissions necessary to perform their tasks?

A. Defence in Depth.

B. Zero Trust.

C. Least Privilege.

D. Open Access.

### Question 3

Which Linux file stores information about user accounts?

A. `/etc/passwd`

B. `/etc/kernel`

C. `/etc/network`

D. `/etc/hosts`

### Question 4

Which command changes file permissions?

A. `passwd`

B. `chmod`

C. `systemctl`

D. `ipconfig`

### Question 5

What is the purpose of sudo?

A. To display network interfaces.

B. To grant controlled administrative privileges.

C. To delete all user accounts.

D. To update the kernel automatically.

### Question 6

Which file contains password hash information on traditional Linux systems?

A. `/etc/group`

B. `/etc/hostname`

C. `/etc/shadow`

D. `/etc/fstab`

### Question 7

Which protocol is commonly used for secure remote Linux administration?

A. FTP

B. Telnet

C. SSH

D. HTTP

### Question 8

Which Linux firewall management tool provides a simplified interface to firewall rules?

A. UFW

B. Nano

C. Grep

D. Tar

### Question 9

What does CVE stand for?

A. Common Vulnerabilities and Exposures.

B. Computer Verification Engine.

C. Central Virtual Environment.

D. Common Virus Examination.

### Question 10

What is Linux system hardening?

A. Making the computer physically heavier.

B. Reducing security weaknesses through secure configuration.

C. Removing the Linux kernel.

D. Increasing the number of open ports.

### Question 11

Which command displays listening network sockets?

A. `pwd`

B. `whoami`

C. `ss -tuln`

D. `mkdir`

### Question 12

What does IDS stand for?

A. Internet Data Storage.

B. Intrusion Detection System.

C. Internal Directory Service.

D. Internet Distribution Software.

### Question 13

Which system can automatically block detected malicious traffic when configured for prevention?

A. IPS

B. Text editor

C. File manager

D. Shell prompt

### Question 14

What is a rootkit?

A. A Linux text editor.

B. A malicious component designed to conceal activity and maintain unauthorised access.

C. A package manager.

D. A backup utility.

### Question 15

Which tool is commonly used for Linux malware scanning?

A. ClamAV

B. Nano

C. Ping

D. UFW

### Question 16

What is the main purpose of AIDE?

A. File integrity monitoring.

B. Creating user accounts.

C. Managing network routes.

D. Installing desktop themes.

### Question 17

What does SIEM stand for?

A. Security Information and Event Management.

B. System Internet Encryption Method.

C. Secure Internal Email Manager.

D. Software Installation and Execution Monitor.

### Question 18

Which tool is commonly used for network intrusion detection?

A. Snort

B. Chown

C. Touch

D. Cat

### Question 19

What is a false positive?

A. A legitimate activity incorrectly identified as suspicious.

B. A malicious activity successfully blocked.

C. A successful software installation.

D. An encrypted backup.

### Question 20

What should an administrator do first when a security incident is suspected?

A. Immediately delete all system logs.

B. Ignore the alert.

C. Follow the incident-response procedure and investigate the evidence.

D. Disable every security tool.

---

# PART B: SHORT-ANSWER QUESTIONS

Answer in your own words.

1. Define Linux security.
2. Explain the CIA triad.
3. What is defence in depth?
4. Explain the principle of least privilege.
5. Differentiate between authentication and authorisation.
6. Explain the difference between `chmod`, `chown`, and `chgrp`.
7. What is the purpose of `/etc/shadow`?
8. Explain the difference between SSH password authentication and public-key authentication.
9. What is the purpose of a firewall?
10. Explain the difference between IDS and IPS.
11. What is the purpose of security updates?
12. Explain Linux system hardening.
13. What is a security audit?
14. Explain the difference between HIDS and NIDS.
15. What is the purpose of security monitoring?
16. Explain signature-based and anomaly-based detection.
17. What is a rootkit?
18. Explain file integrity monitoring.
19. What is a SIEM platform?
20. Describe the six main stages of incident response.

---

# PART C: PRACTICAL COMMAND ASSESSMENT

Perform the following tasks on your Kali Linux virtual machine.

All activities must be performed on your own VM.

## Task 1: System Information

Display:

- Linux distribution.
- Kernel version.
- Hostname.

Commands:

```bash
cat /etc/os-release
uname -r
hostname
```

## Task 2: User Security

Display:

- Available user accounts.
- Group information.
- Sudo group membership.

Commands:

```bash
getent passwd
getent group
getent group sudo
```

## Task 3: File Permissions

Inspect:

```bash
ls -l /etc/passwd /etc/shadow /etc/group
```

Explain why sensitive account files require protection.

## Task 4: Process Inspection

Display running processes:

```bash
ps aux
```

Explain why process monitoring is important.

## Task 5: Network Security

Display listening ports:

```bash
sudo ss -tulpen
```

Explain why unnecessary listening services may increase security risks.

## Task 6: Firewall Inspection

Run:

```bash
sudo ufw status verbose
```

Explain the purpose of firewall rules.

## Task 7: Security Logs

Display recent system logs:

```bash
sudo journalctl -n 30 --no-pager
```

Explain how logs support incident investigations.

## Task 8: Security Updates

Display packages awaiting updates:

```bash
apt list --upgradable
```

Explain why security patches are important.

## Task 9: File Integrity

Create a test file:

```bash
mkdir -p ~/module13-lab/assessment
cd ~/module13-lab/assessment
echo "Linux security assessment" > test.txt
```

Generate a hash:

```bash
sha256sum test.txt
```

Modify the file:

```bash
echo "Controlled modification" >> test.txt
```

Generate another hash:

```bash
sha256sum test.txt
```

Explain why the hashes differ.

## Task 10: Security Report

Create:

```bash
nano module13-final-security-report.txt
```

Your report should contain:

```text
MODULE 13: LINUX SECURITY FINAL REPORT

1. Operating System Information

2. User Account Inspection

3. File Permission Inspection

4. Process Inspection

5. Network Security Inspection

6. Firewall Inspection

7. System Log Inspection

8. Security Update Inspection

9. File Integrity Test

10. Security Recommendations

11. Conclusion
```

Save your report.

---

# PART D: SCENARIO-BASED QUESTIONS

## Scenario 1: Unauthorised Login Attempts

You are monitoring a Linux server and discover hundreds of failed SSH login attempts.

Answer:

1. What type of attack might this indicate?
2. Which logs would you inspect?
3. What tools could help detect this activity?
4. What security improvements would you recommend?

## Scenario 2: Suspicious Network Port

You discover an unexpected listening port on a university Linux server.

Answer:

1. Which command would you use to investigate?
2. How would you identify the application using the port?
3. Why should you not immediately terminate the process?
4. What would you recommend if the service is unnecessary?

## Scenario 3: Unexpected File Modification

An important system configuration file has changed unexpectedly.

Answer:

1. What security technology could detect this?
2. How can hashes help investigate the modification?
3. What evidence should you preserve?
4. What should you investigate before restoring the file?

## Scenario 4: Suspected Rootkit

A security monitoring tool reports possible rootkit activity.

Answer:

1. What is a rootkit?
2. Why is it dangerous?
3. Which tools can help investigate it?
4. Why might ordinary local inspection tools be insufficient?
5. What should happen if compromise is confirmed?

## Scenario 5: Compromised Administrator Account

An attacker obtains access to an administrator account.

Answer:

1. What security principle may have been violated?
2. What potential damage could occur?
3. What immediate response steps should be considered?
4. How could stronger account security reduce this risk?

---

# PART E: FINAL PRACTICAL PROJECT

## Project Title: Linux Security Audit and Monitoring Toolkit

### Objective

Combine the skills from Module 13 into one organised project.

### Project Requirements

Your project should:

1. Collect Linux system information.
2. Inspect user accounts.
3. Inspect important file permissions.
4. Display running services.
5. Display listening network ports.
6. Inspect firewall status.
7. Collect recent system logs.
8. Display package updates.
9. Perform a SHA-256 file integrity demonstration.
10. Generate a final security report.

### Required Directory

```text
~/module13-lab/final-project/
```

### Recommended Structure

```text
final-project/
├── security-toolkit.sh
├── reports/
│   ├── system-report.txt
│   ├── user-report.txt
│   ├── network-report.txt
│   └── final-security-report.txt
├── integrity/
│   ├── test-file.txt
│   └── baseline.txt
└── documentation/
    └── project-documentation.md
```

### Project Documentation

Document:

- Project objective.
- Tools and commands used.
- Features.
- How to execute the toolkit.
- Sample findings.
- Limitations.
- Security recommendations.

### Safety Requirements

- Do not execute untrusted scripts as root.
- Do not change system-wide security configurations during this assessment.
- Do not disable firewall or authentication controls.
- Do not scan networks without permission.
- Store project outputs inside your own lab directory.

---

# 11. Final Assessment Checklist

- [ ] I understand Linux security principles.
- [ ] I understand user and privilege management.
- [ ] I understand authentication and password security.
- [ ] I understand file permissions.
- [ ] I understand SSH security.
- [ ] I understand firewall fundamentals.
- [ ] I understand security patching.
- [ ] I understand Linux system hardening.
- [ ] I can perform a basic security audit.
- [ ] I understand intrusion detection and monitoring.
- [ ] I understand malware and rootkit detection.
- [ ] I completed the practical projects.
- [ ] I understand incident response.
- [ ] I can document security findings professionally.

---

# 12. Module 13 Completion

Congratulations!

You have reached the end of Module 13 – Linux Security.

You have studied the foundational security practices used by Linux administrators and cybersecurity professionals.

Your next module will build on this knowledge by exploring firewall configuration and intrusion detection/prevention systems in greater practical depth.

**MODULE 13: LINUX SECURITY — COMPLETED.**
