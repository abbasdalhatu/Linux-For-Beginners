# Module 14 – Lesson 15: Firewalls and IDS/IPS Assessment

## 1. Assessment Overview

This assessment evaluates your understanding of Linux firewalls, intrusion detection, intrusion prevention, and security monitoring.

### Assessment Objectives

By completing this assessment, you should demonstrate your ability to:

1. Explain firewall fundamentals.
2. Understand Linux firewall architecture.
3. Use UFW, iptables, and nftables for inspection.
4. Explain firewall rules and policies.
5. Understand inbound and outbound traffic.
6. Analyse firewall logs.
7. Explain IDS and IPS technologies.
8. Understand Snort and Suricata.
9. Explain SIEM and security event correlation.
10. Apply your knowledge to practical cybersecurity scenarios.

### Assessment Structure

| Section | Topic | Marks |
|---|---|---:|
| A | Multiple Choice Questions | 20 |
| B | Short Answer Questions | 20 |
| C | Technical Questions | 20 |
| D | Practical Scenarios | 25 |
| E | Practical Project Review | 15 |
| **Total** | | **100** |

Suggested pass mark: 50%.

---

# SECTION A: MULTIPLE CHOICE QUESTIONS

**20 Questions – 1 Mark Each**

Choose the correct answer.

### 1. What is the primary purpose of a firewall?

A. Create computer programs.

B. Control network traffic according to security policies.

C. Increase CPU speed.

D. Store passwords.

### 2. Which Linux framework provides packet filtering?

A. Netfilter.

B. Python.

C. Apache.

D. PostgreSQL.

### 3. Which command displays UFW status?

A. `ufw show`

B. `ufw status`

C. `ufw check`

D. `ufw display`

### 4. Which command displays the nftables ruleset?

A. `nft show`

B. `nft rules`

C. `nft list ruleset`

D. `nft display`

### 5. Which iptables chain handles traffic destined for the local system?

A. OUTPUT.

B. FORWARD.

C. INPUT.

D. POSTROUTING.

### 6. What does the DROP target do?

A. Accepts traffic.

B. Silently discards matching traffic.

C. Creates a new connection.

D. Records a username.

### 7. Which port is commonly associated with SSH?

A. 21.

B. 22.

C. 25.

D. 80.

### 8. What is an inbound connection?

A. Traffic entering a protected system.

B. Traffic leaving a protected system.

C. A deleted file.

D. A local password.

### 9. What is firewall logging used for?

A. Changing desktop themes.

B. Recording relevant traffic and policy events.

C. Installing applications.

D. Compressing files.

### 10. What does IDS stand for?

A. Internet Data Service.

B. Intrusion Detection System.

C. Internal Database Software.

D. Information Development Service.

### 11. What is the primary role of an IDS?

A. Detect suspicious activity.

B. Automatically install applications.

C. Replace an operating system.

D. Increase storage.

### 12. Which of the following is a network intrusion detection engine?

A. Suricata.

B. LibreOffice.

C. VLC.

D. GIMP.

### 13. Which organisation develops Suricata?

A. Microsoft.

B. Open Information Security Foundation.

C. Oracle.

D. Mozilla.

### 14. Which file commonly contains Suricata EVE JSON events?

A. `/etc/passwd`

B. `/var/log/suricata/eve.json`

C. `/boot/grub/grub.cfg`

D. `/etc/hostname`

### 15. What does IPS stand for?

A. Internet Protection Software.

B. Intrusion Prevention System.

C. Internal Packet Storage.

D. Information Processing Service.

### 16. What is the difference between IDS and IPS?

A. IDS detects; IPS can detect and prevent matching traffic.

B. IDS stores files; IPS deletes files.

C. They are exactly the same.

D. IPS only monitors keyboards.

### 17. What is a false positive?

A. A real attack that was successfully blocked.

B. Legitimate activity incorrectly identified as malicious.

C. A missing network cable.

D. An operating system update.

### 18. What does SIEM stand for?

A. Security Information and Event Management.

B. System Internet Encryption Method.

C. Secure Internal Email Manager.

D. Software Installation Event Monitor.

### 19. What is event correlation?

A. Deleting old logs.

B. Identifying relationships between multiple security events.

C. Formatting a hard drive.

D. Changing network passwords.

### 20. Which of the following is an open-source security monitoring platform with SIEM capabilities?

A. Wazuh.

B. Microsoft Word.

C. Paint.

D. Calculator.

---

# SECTION B: SHORT ANSWER QUESTIONS

**10 Questions – 2 Marks Each**

Answer each question clearly.

1. Define a firewall.
2. Explain the difference between stateful and stateless firewalls.
3. What is Netfilter?
4. Explain the purpose of the INPUT, OUTPUT, and FORWARD chains.
5. What is the difference between UFW and iptables?
6. Explain the purpose of Suricata detection rules.
7. What is the difference between signature-based and anomaly-based detection?
8. Define an inline IPS.
9. Explain fail-open and fail-close.
10. What is the purpose of a SIEM platform?

---

# SECTION C: TECHNICAL QUESTIONS

**5 Questions – 4 Marks Each**

### 1. Explain the following Netfilter hooks:

- PREROUTING.
- INPUT.
- FORWARD.
- OUTPUT.
- POSTROUTING.

### 2. Explain the following iptables targets:

- ACCEPT.
- DROP.
- REJECT.
- LOG.

### 3. Study the Suricata rule below:

```suricata
alert tcp any any -> 192.168.1.20 22 (msg:"SSH Traffic Detected"; sid:1000001; rev:1;)
```

Identify:

a. Action.

b. Protocol.

c. Source address.

d. Destination address.

e. Destination port.

f. Message.

g. Signature ID.

h. Revision.

### 4. Explain five important fields found in a Suricata EVE JSON alert.

### 5. Describe how a firewall, IDS, IPS, and SIEM work together to protect an organisation.

---

# SECTION D: PRACTICAL SCENARIOS

**5 Questions – 5 Marks Each**

### Scenario 1: Unnecessary Open Ports

You are auditing a Linux server.

You discover several listening services, including one that the organisation does not recognise.

**Questions:**

1. Which command would you use to inspect listening services?
2. What information would you collect?
3. How would you determine whether the service is necessary?
4. What security risks could an unnecessary service create?
5. What would you recommend?

### Scenario 2: Suspicious SSH Activity

An IDS generates repeated alerts involving SSH traffic from one source address.

**Questions:**

1. What does SSH normally use?
2. Which logs would you examine?
3. How would you investigate the source address?
4. What evidence would you collect?
5. When would you recommend blocking the source?

### Scenario 3: IPS Blocking Legitimate Traffic

An IPS begins blocking legitimate communication between employees and an internal application.

**Questions:**

1. What security problem might be occurring?
2. Define a false positive.
3. How would you investigate the blocking rule?
4. What could happen if the rule remains unchanged?
5. How would you correct the problem safely?

### Scenario 4: Multiple Security Alerts

A SIEM receives:

- Several failed login attempts.
- An unusual network connection.
- A firewall alert.
- A Suricata alert.

All events involve the same source address.

**Questions:**

1. What is event correlation?
2. Why are these events worth investigating together?
3. Which information should be compared?
4. What could the combined events indicate?
5. What should the security analyst do next?

### Scenario 5: Network Security Design

A small organisation wants to protect:

- A public web server.
- An internal database.
- Employee computers.
- An administrative workstation.

**Questions:**

1. Where would you place the firewall?
2. Why should the database be protected from direct public access?
3. What role would an IDS perform?
4. What role would an IPS perform?
5. How could a SIEM improve monitoring?

---

# SECTION E: PRACTICAL PROJECT REVIEW

**15 Marks**

Review the six projects completed in Lesson 14.

| Project | Review requirement | Marks |
|---|---|---:|
| Project 1 | Firewall audit report | 3 |
| Project 2 | Firewall policy design | 2 |
| Project 3 | IDS alert investigation | 2 |
| Project 4 | Suricata log analysis | 3 |
| Project 5 | IPS simulation | 2 |
| Project 6 | Security monitoring dashboard | 3 |
| **Total** | | **15** |

For each project, confirm that:

- The required files exist.
- The commands or Python programs were executed.
- The results were examined.
- Findings were documented.
- Recommendations were included where appropriate.

---

# 2. Assessment Marking Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| 0–49 | Needs improvement |

These are suggested course assessment bands.

---

# 3. Final Module 14 Competency Checklist

Mark each skill when you can confidently perform or explain it.

## Firewall Fundamentals

- [ ] Explain firewall operation.
- [ ] Understand firewall types.
- [ ] Explain Netfilter.
- [ ] Understand firewall chains and hooks.
- [ ] Inspect UFW.
- [ ] Inspect iptables.
- [ ] Inspect nftables.
- [ ] Understand firewall policies.
- [ ] Explain inbound and outbound traffic.
- [ ] Analyse firewall logs.

## IDS and IPS

- [ ] Explain IDS architecture.
- [ ] Understand Snort.
- [ ] Understand Suricata.
- [ ] Read basic detection rules.
- [ ] Understand EVE JSON.
- [ ] Explain IPS deployment.
- [ ] Understand inline inspection.
- [ ] Explain false positives and false negatives.

## Security Monitoring

- [ ] Explain SIEM.
- [ ] Understand event correlation.
- [ ] Understand Wazuh architecture.
- [ ] Analyse security logs.
- [ ] Build a basic Python log analyser.
- [ ] Explain incident response.

## Practical Projects

- [ ] Complete firewall security audit.
- [ ] Design a firewall policy.
- [ ] Investigate IDS alerts.
- [ ] Analyse Suricata logs.
- [ ] Build an IPS simulation.
- [ ] Build a security monitoring dashboard.

---

# 4. Final Module Summary

Congratulations on reaching the end of Module 14.

You have studied:

1. Firewall Fundamentals.
2. Linux Firewall Architecture.
3. UFW Firewall Management.
4. iptables Fundamentals.
5. nftables Fundamentals.
6. Firewall Rules and Policies.
7. Inbound and Outbound Traffic Control.
8. Firewall Logging and Monitoring.
9. Intrusion Detection Systems.
10. Snort Intrusion Detection.
11. Suricata Intrusion Detection.
12. Intrusion Prevention Systems.
13. IDS/IPS Integration and SIEM.
14. Practical Firewall and IDS/IPS Projects.
15. Module Assessment.

## What You Should Understand Now

You should be able to explain how Linux firewalls control traffic, how IDS platforms identify suspicious activity, how IPS platforms prevent matching threats, and how SIEM systems centralise security information.

**Module 14 is now complete.**

## Next Module

# Module 15 – Network Security

The next module will build on your firewall and IDS/IPS knowledge by exploring broader network security concepts, network attacks, defensive architecture, and secure network communication.