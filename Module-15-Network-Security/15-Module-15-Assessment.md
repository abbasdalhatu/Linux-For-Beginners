# MODULE 15 – NETWORK SECURITY
## LESSON 15: FINAL MODULE ASSESSMENT

---

## 1. ASSESSMENT INFORMATION

**Module:** 15 – Network Security

**Assessment Type:** Theory, Practical Knowledge, and Security Scenarios

**Total Marks:** 100

**Pass Mark:** 50%

**Recommended Time:** 90–120 minutes

### Assessment Objectives

This assessment evaluates your understanding of:

- Network security fundamentals.
- Network threats and attacks.
- Network reconnaissance.
- ARP and MITM security.
- DNS and DHCP security.
- Secure network protocols.
- VPNs and secure tunneling.
- VLANs and network segmentation.
- Wireless security.
- Network monitoring and traffic analysis.

---

# SECTION A: MULTIPLE-CHOICE QUESTIONS

**20 Questions – 20 Marks**

Choose the correct answer.

### 1. What is the primary purpose of network security?

A. To increase screen resolution.

B. To protect network systems, data, and communication.

C. To install operating systems.

D. To increase keyboard speed.

### 2. What does CIA stand for in information security?

A. Control, Internet, Access.

B. Confidentiality, Integrity, Availability.

C. Computer, Information, Application.

D. Communication, Identification, Authentication.

### 3. Which device commonly filters network traffic according to security rules?

A. Monitor.

B. Keyboard.

C. Firewall.

D. Printer.

### 4. What is a vulnerability?

A. A security weakness.

B. A network cable.

C. A backup file.

D. An encryption algorithm.

### 5. Which attack attempts to intercept communication between two parties?

A. MITM.

B. Disk formatting.

C. File compression.

D. Defragmentation.

### 6. Which command displays IP addresses on Linux?

A. `pwd`

B. `ip addr`

C. `mkdir`

D. `whoami`

### 7. What does DNS do?

A. Encrypts hard drives.

B. Translates domain names into IP addresses.

C. Controls keyboard input.

D. Manages Linux users.

### 8. Which protocol is commonly used for secure remote terminal access?

A. FTP.

B. Telnet.

C. SSH.

D. HTTP.

### 9. What does DHCP provide to network devices?

A. Dynamic network configuration.

B. Antivirus software.

C. File compression.

D. Password encryption.

### 10. Which protocol commonly uses UDP ports 67 and 68 for IPv4 DHCP?

A. DNS.

B. DHCP.

C. SSH.

D. HTTPS.

### 11. What is the main purpose of a VPN?

A. To delete network logs.

B. To create a protected communication tunnel over another network.

C. To increase RAM.

D. To remove IP addresses.

### 12. Which VPN technology uses WireGuard's modern protocol design?

A. WireGuard.

B. FTP.

C. Telnet.

D. HTTP.

### 13. What does VLAN stand for?

A. Virtual Local Area Network.

B. Variable Linux Access Node.

C. Verified Local Application Network.

D. Virtual Link Access Number.

### 14. Which IEEE standard is commonly used for VLAN tagging?

A. 802.11.

B. 802.1Q.

C. 802.3af.

D. 802.15.

### 15. What is an access port?

A. A switch port normally assigned to one VLAN for an end device.

B. A wireless password.

C. A firewall rule.

D. A DNS record.

### 16. Which wireless security standard introduces SAE for Personal authentication?

A. WEP.

B. WPA.

C. WPA3.

D. HTTP.

### 17. What is an evil twin attack?

A. A malicious access point imitating a legitimate wireless network.

B. A computer backup.

C. A firewall update.

D. A password manager.

### 18. Which tool is widely used for graphical packet analysis?

A. Wireshark.

B. LibreOffice.

C. GIMP.

D. Calculator.

### 19. What does SIEM stand for?

A. Security Information and Event Management.

B. System Internet Encryption Method.

C. Secure Internal Ethernet Management.

D. Software Installation and Event Monitoring.

### 20. Which Linux command displays listening network sockets?

A. `ls`

B. `ss -tulpen`

C. `cd`

D. `touch`

---

# SECTION B: SHORT-ANSWER QUESTIONS

**10 Questions – 20 Marks**

Each question carries 2 marks.

### 1.
Define network security.

### 2.
Explain the difference between a threat, vulnerability, and risk.

### 3.
Differentiate between passive and active reconnaissance.

### 4.
What is ARP spoofing?

### 5.
Explain the purpose of DNSSEC.

### 6.
Differentiate between HTTP and HTTPS.

### 7.
Explain the difference between full tunneling and split tunneling.

### 8.
What is inter-VLAN routing?

### 9.
State four wireless network security controls.

### 10.
Explain why network monitoring is important.

---

# SECTION C: TECHNICAL QUESTIONS

**5 Questions – 20 Marks**

Each question carries 4 marks.

### 1. Linux Network Inspection

Explain what each command does:

```bash
ip addr
ip route
ip neigh
ss -tulpen
```

### 2. Network Threats

Explain four common network attacks and provide one defensive measure for each.

### 3. Secure Network Protocols

Explain the security differences between:

- HTTP and HTTPS.
- FTP and SFTP.
- Telnet and SSH.
- DNS and DNS over HTTPS.

### 4. VLAN Security

Explain four ways an organization can protect its VLAN infrastructure.

### 5. Traffic Analysis

What information can a security analyst obtain from a packet capture?

---

# SECTION D: SCENARIO-BASED QUESTIONS

**4 Questions – 20 Marks**

Each question carries 5 marks.

### Scenario 1: Unauthorized Network Access

A university discovers that students can access the finance department's internal servers.

**Questions:**

1. What is the likely network security problem?
2. What security control could separate these departments?
3. How can a firewall help?
4. What access policy would you recommend?
5. How would you verify that the security improvement works?

### Scenario 2: Suspicious DNS Activity

A company notices that one workstation repeatedly communicates with an unfamiliar domain.

**Questions:**

1. Why might this activity be suspicious?
2. Which tools could help investigate it?
3. What DNS information would you examine?
4. How could malware be involved?
5. What should the security team do next?

### Scenario 3: Wireless Security

A university uses one shared Wi-Fi password for students, lecturers, and administrators.

**Questions:**

1. Identify two security risks.
2. Recommend a better authentication method.
3. Explain the purpose of a guest network.
4. How could VLANs improve security?
5. What monitoring should the IT department implement?

### Scenario 4: Unusual Network Traffic

A workstation begins sending unusually large amounts of outbound traffic.

**Questions:**

1. What could cause this behavior?
2. Which Linux commands could help inspect the system?
3. How could packet analysis help?
4. What evidence should the security analyst preserve?
5. What steps should be taken if a compromise is confirmed?

---

# SECTION E: PRACTICAL KNOWLEDGE

**4 Tasks – 20 Marks**

Each task carries 5 marks.

Complete these exercises on your own Kali VM.

### Task 1: Network Configuration

Run:

```bash
ip -br addr
ip route
```

Identify:

- Network interface.
- IP address.
- Default gateway.

### Task 2: Listening Services

Run:

```bash
ss -tulpen
```

Identify:

- Listening TCP services.
- Listening UDP services.
- Any service that requires further investigation.

### Task 3: DNS Investigation

Run:

```bash
dig example.com
```

Identify:

- Domain queried.
- DNS response status.
- Returned IP address, if available.
- DNS server used.

### Task 4: Network Monitoring

Run:

```bash
ip -s link
ss -tn state established
```

Explain what the outputs tell you about your VM's network activity.

If a command returns no results, document that honestly.

---

# SECTION F: PROFESSIONAL REFLECTION

**Not graded – Career development**

Answer these questions honestly.

1. Which network security topic did you find most interesting?
2. Which topic was the most difficult?
3. Which tools can you now use confidently?
4. What practical skill improved the most?
5. Which network security topic do you want to study more deeply?
6. How would you apply network security knowledge in a real organization?

---

# 7. MARKING GUIDE

| Section | Marks |
|---|---:|
| A – Multiple Choice | 20 |
| B – Short Answers | 20 |
| C – Technical Questions | 20 |
| D – Scenarios | 20 |
| E – Practical Knowledge | 20 |
| **Total** | **100** |

### Performance Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Requires further study |

---

# 8. MODULE 15 COMPLETION CHECKLIST

- [ ] Lesson 01: Network Security Fundamentals.
- [ ] Lesson 02: Network Threats and Attacks.
- [ ] Lesson 03: Network Attack Surfaces.
- [ ] Lesson 04: Network Reconnaissance and Enumeration.
- [ ] Lesson 05: Common Network Attacks.
- [ ] Lesson 06: ARP Spoofing and MITM.
- [ ] Lesson 07: DNS Security and Attacks.
- [ ] Lesson 08: DHCP and IP Security.
- [ ] Lesson 09: Secure Network Protocols.
- [ ] Lesson 10: VPN and Secure Tunneling.
- [ ] Lesson 11: Network Segmentation and VLAN Security.
- [ ] Lesson 12: Wireless Network Security.
- [ ] Lesson 13: Network Monitoring and Traffic Analysis.
- [ ] Lesson 14: Practical Network Security Projects.
- [ ] Lesson 15: Final Module Assessment.

---

# 9. FINAL MODULE SUMMARY

You have studied the major concepts of network security, including network architecture, threats, vulnerabilities, reconnaissance, secure protocols, VPNs, VLANs, wireless security, and traffic monitoring.

You have also developed practical experience with Linux network inspection, packet analysis, DNS investigation, risk assessment, network architecture design, and basic monitoring automation.

**FINAL MESSAGE:**

Network security is not simply about stopping attacks. It is about designing, protecting, monitoring, and continuously improving network infrastructure.

A capable network security professional must understand how networks operate, how attacks affect them, how to recognize suspicious behavior, and how to implement appropriate defensive controls.

**END OF MODULE 15 – NETWORK SECURITY**