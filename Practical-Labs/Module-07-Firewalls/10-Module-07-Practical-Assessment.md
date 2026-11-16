
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 10: MODULE 07 PRACTICAL ASSESSMENT

## 1. Assessment Overview

This is the final practical assessment for Module 07.

You will demonstrate your understanding of Linux firewall architecture, firewall policies, network traffic control, logging, monitoring, troubleshooting, and security design.

This assessment combines practical inspection with realistic security scenarios.

### Assessment Objectives

You must demonstrate that you can:

1. Explain Linux firewall architecture.
2. Inspect firewall configuration.
3. Identify listening services.
4. Analyze firewall policies.
5. Understand inbound and outbound traffic.
6. Interpret firewall logs.
7. Troubleshoot connectivity problems.
8. Design secure firewall policies.
9. Recommend defensive improvements.
10. Produce a professional firewall security report.

## 2. Assessment Safety Rules

This assessment is inspection-only on your active Kali VM.

- Do not flush firewall rules.
- Do not disable your firewall.
- Do not reset UFW.
- Do not expose SSH or other services to the public internet.
- Do not modify Windows Firewall.
- Do not scan public or unauthorized systems.
- Use fictional addresses for policy-design questions.

Your objective is to demonstrate knowledge, not to disrupt network connectivity.

---

# PART A – FIREWALL ENVIRONMENT INSPECTION

## Task 1: Inspect Network Interfaces

Run:

```bash
ip -br addr
```

Record:

- Interface names.
- IP addresses.
- Interface states.

## Task 2: Inspect Routing

Run:

```bash
ip route
```

Record:

- Default gateway.
- Default interface.
- Other available routes.

## Task 3: Inspect Listening Services

Run:

```bash
sudo ss -tulpn
```

Identify at least three entries if available.

Record:

- Protocol.
- Local address.
- Port.
- Process name, if available.

If fewer than three services are listening, document the actual result rather than inventing services.

## Task 4: Inspect Firewall Status

Run:

```bash
sudo ufw status verbose
```

Then:

```bash
sudo nft list ruleset
```

Finally:

```bash
sudo iptables -L -n -v
```

Record the results.

Some systems may have UFW inactive or no nftables rules configured. That is an observation, not automatically an error.

---

# PART B – FIREWALL KNOWLEDGE

Answer the following questions in your report.

1. What is a firewall?
2. What is the difference between a host-based firewall and a network firewall?
3. What is Netfilter?
4. What is nftables?
5. What is UFW?
6. What is the difference between ACCEPT, DROP, and REJECT?
7. Explain the INPUT, OUTPUT, and FORWARD chains.
8. What is the difference between TCP and UDP?
9. What is a default firewall policy?
10. What is the principle of least privilege?

---

# PART C – FIREWALL POLICY DESIGN

## Task 5: Design a Secure Web Server Firewall Policy

A fictional company operates a public web server.

Server information:

```text
Server IP: 10.20.30.10
HTTP: TCP 80
HTTPS: TCP 443
SSH: TCP 22
Database: TCP 3306

Administrator IP: 203.0.113.25
Application Server: 10.20.30.20
```

### Security Requirements

- Allow public HTTP and HTTPS traffic.
- Restrict SSH to the administrator IP.
- Permit database access only from the application server.
- Deny other unauthorized incoming connections.
- Keep security logging enabled.

### Your Task

Create a conceptual firewall policy table.

Include:

- Source.
- Destination.
- Protocol.
- Destination port.
- Action.
- Reason.

Do not apply these rules to your Kali VM.

---

# PART D – LOG ANALYSIS

## Task 6: Analyze Fictional Firewall Events

Study these fictional log entries:

```text
Nov 15 10:00:01 server kernel: BLOCK IN=eth0 SRC=198.51.100.25 DST=10.20.30.10 PROTO=TCP DPT=22
Nov 15 10:00:03 server kernel: BLOCK IN=eth0 SRC=198.51.100.40 DST=10.20.30.10 PROTO=TCP DPT=3306
Nov 15 10:00:08 server kernel: ALLOW IN=eth0 SRC=203.0.113.25 DST=10.20.30.10 PROTO=TCP DPT=22
Nov 15 10:00:12 server kernel: BLOCK OUT=eth0 DST=198.51.100.90 PROTO=UDP DPT=53
```

### Questions

1. Which source attempted to reach SSH?
2. Which source attempted to reach the database?
3. Which address was permitted to access SSH?
4. What type of traffic was blocked outbound?
5. Which event might require further investigation?
6. Why should administrators correlate firewall logs with system and application logs?

Remember that these entries are fictional and do not represent actual activity on your machine.

---

# PART E – FIREWALL TROUBLESHOOTING

## Task 7: Analyze a Connectivity Problem

A fictional Linux server has the following characteristics:

```text
Server IP: 10.20.30.10
Service: HTTPS
Port: TCP 443
Service Status: Running
Listening Port: 443
Firewall Default Incoming Policy: DENY
Matching Allow Rule: Not Found
```

### Questions

1. What is the likely reason HTTPS clients cannot connect?
2. Which evidence supports your conclusion?
3. What type of firewall rule might solve the problem?
4. How should the administrator test the correction?
5. Why should a rollback plan be prepared?

## Task 8: Diagnose a DNS Problem

A Linux workstation can communicate with its gateway but cannot resolve domain names.

### Questions

1. What is DNS?
2. Which command can test hostname resolution?
3. Which file may contain DNS configuration?
4. Which transport protocols and ports are commonly used by DNS?
5. What firewall-related issue could cause this problem?

---

# PART F – SECURITY AUDIT

## Task 9: Identify Security Risks

Analyze the following fictional server configuration:

```text
SSH: Publicly accessible
HTTP: Publicly accessible
HTTPS: Publicly accessible
MySQL: Publicly accessible
Telnet: Enabled
Firewall Logging: Disabled
Default Incoming Policy: ALLOW
```

Identify at least five security weaknesses.

For each weakness, provide:

- Finding.
- Risk.
- Recommended remediation.
- Priority: Critical, High, Medium, or Low.

Use realistic priorities based on the scenario and explain your reasoning.

---

# PART G – FINAL REPORT

## Task 10: Create Your Module 07 Assessment Report

Create your assessment directory:

```bash
mkdir -p ~/practical-labs/module07/assessment
```

Enter it:

```bash
cd ~/practical-labs/module07/assessment
```

Create your report:

```bash
nano module07-final-assessment.txt
```

Use the following structure:

```text
==================================================
MODULE 07 - FINAL PRACTICAL ASSESSMENT
FIREWALL CONFIGURATION AND TESTING
==================================================

STUDENT:
DATE:

PART A: FIREWALL ENVIRONMENT INSPECTION

Network Interfaces:
IP Addresses:
Default Gateway:
Listening Services:
UFW Status:
NFTABLES:
IPTABLES:

PART B: FIREWALL KNOWLEDGE

1.
2.
3.
4.
5.
6.
7.
8.
9.
10.

PART C: FIREWALL POLICY DESIGN

Policy Table:
Security Reasoning:

PART D: FIREWALL LOG ANALYSIS

SSH Event:
Database Event:
Permitted Event:
Outbound Event:
Additional Investigation:

PART E: TROUBLESHOOTING

HTTPS Problem:
Evidence:
Recommended Correction:
Testing:
Rollback:

DNS Problem:
Investigation:
Possible Cause:

PART F: SECURITY AUDIT

Finding 1:
Risk:
Priority:
Remediation:

Finding 2:
Risk:
Priority:
Remediation:

Finding 3:
Risk:
Priority:
Remediation:

Finding 4:
Risk:
Priority:
Remediation:

Finding 5:
Risk:
Priority:
Remediation:

PART G: FINAL REFLECTION

What I Learned:

My Strongest Area:

Area Requiring More Practice:

Five Important Firewall Security Principles:

==================================================
END OF ASSESSMENT
==================================================
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

---

# 3. Assessment Scoring Guide

| Assessment area | Marks |
|---|---:|
| Environment inspection | 15 |
| Firewall knowledge | 20 |
| Policy design | 20 |
| Log analysis | 15 |
| Troubleshooting | 15 |
| Security audit | 10 |
| Final report quality | 5 |
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

A score of 50 is the minimum passing mark for this assessment.

# 4. Final Completion Checklist

- [ ] Inspected network interfaces.
- [ ] Inspected routing information.
- [ ] Inspected listening services.
- [ ] Inspected UFW, nftables, and iptables.
- [ ] Completed firewall knowledge questions.
- [ ] Designed a secure web server policy.
- [ ] Analyzed fictional firewall logs.
- [ ] Completed troubleshooting scenarios.
- [ ] Identified security weaknesses.
- [ ] Created the final assessment report.

# 5. Module 07 Completion

Congratulations on reaching the final assessment.

After completing this lab, you should understand how Linux firewalls contribute to system security, network access control, monitoring, and incident investigation.

**Next Module: Module 08 – Log Analysis and Incident Response.**

You will learn how to investigate Linux logs, identify suspicious events, analyze authentication activity, recognize possible attacks, and prepare incident reports.
