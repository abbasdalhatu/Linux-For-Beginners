# MODULE 15 – NETWORK SECURITY
## LESSON 01: NETWORK SECURITY FUNDAMENTALS

---

## 1. Introduction to Network Security

Network security is the practice of protecting computer networks, connected devices, applications, and data against unauthorized access, misuse, modification, disruption, and destruction.

It involves technologies, policies, configurations, and procedures that keep network communications secure.

### Simple explanation

Imagine a university computer network:

- Students connect to the campus Wi-Fi.
- Lecturers access academic resources.
- Administrators manage student records.
- Servers store sensitive university information.

Without network security, an unauthorized person might access confidential records, intercept communications, or disrupt network services.

Network security helps prevent these incidents.

### Main objectives

1. Protect network resources.
2. Prevent unauthorized access.
3. Protect transmitted information.
4. Detect suspicious network activities.
5. Maintain network availability.
6. Respond to security incidents.

---

## 2. The CIA Triad

The CIA Triad is one of the most important foundations of cybersecurity.

CIA represents:

- Confidentiality
- Integrity
- Availability

### A. Confidentiality

Confidentiality ensures that information is accessible only to authorized individuals.

Examples:
- Password-protecting user accounts.
- Encrypting sensitive files.
- Restricting access to student records.
- Using secure communication protocols.

### B. Integrity

Integrity ensures that information remains accurate, complete, and protected against unauthorized modification.

Examples:
- File integrity monitoring.
- Digital signatures.
- Cryptographic hashes.
- Database access controls.

### C. Availability

Availability ensures that authorized users can access network resources when needed.

Examples:
- Regular backups.
- Redundant servers.
- Network monitoring.
- Protection against denial-of-service attacks.

### CIA Triad summary

| Principle | Main purpose | Example |
|---|---|---|
| Confidentiality | Prevent unauthorized disclosure | Encryption |
| Integrity | Prevent unauthorized modification | Hash verification |
| Availability | Keep services accessible | Backups |

---

## 3. Authentication, Authorization and Accounting (AAA)

AAA is a framework used to control and monitor access to network resources.

### A. Authentication

Authentication answers:

**Who are you?**

It verifies the identity of a user or device.

Examples:
- Username and password.
- Fingerprint authentication.
- Security keys.
- Multi-factor authentication (MFA).

### B. Authorization

Authorization answers:

**What are you allowed to do?**

It determines which resources an authenticated user can access.

Example:

A university student may view examination results but cannot modify the official examination database.

### C. Accounting

Accounting answers:

**What did you do?**

It records user activities for monitoring, auditing, and investigations.

Examples:
- Login timestamps.
- Records of administrative actions.
- Network access logs.
- Failed login attempts.

### AAA summary

| Component | Function |
|---|---|
| Authentication | Verifies identity |
| Authorization | Determines permissions |
| Accounting | Records activities |

---

## 4. Network Security Devices

Several devices and systems help protect computer networks.

### 4.1 Router

A router connects different networks and forwards packets between them.

Security responsibilities:
- Network address translation.
- Routing policies.
- Access control lists.
- Secure management configuration.

### 4.2 Switch

A switch connects devices within a local area network (LAN).

Security responsibilities:
- VLAN separation.
- Port security.
- Network access control.
- Monitoring suspicious connections.

### 4.3 Firewall

A firewall monitors and controls network traffic according to defined security rules.

It can allow or block traffic based on:
- Source IP address.
- Destination IP address.
- Port number.
- Protocol.
- Connection state.

### 4.4 IDS – Intrusion Detection System

An IDS monitors network or system activities and identifies suspicious behavior.

It generates alerts but normally does not automatically block the traffic.

### 4.5 IPS – Intrusion Prevention System

An IPS detects suspicious activities and can automatically block or prevent them.

### 4.6 Wireless Access Point

A wireless access point allows wireless devices to connect to a network.

Security responsibilities:
- WPA2 or WPA3 encryption.
- Strong wireless passwords.
- Guest network separation.
- Secure administration.

### 4.7 Proxy Server

A proxy acts as an intermediary between a client and another server.

It may provide:
- Content filtering.
- Access control.
- Logging.
- Privacy protection.

### 4.8 VPN Gateway

A VPN gateway provides a secure connection between networks or remote users.

It commonly uses encryption and authentication to protect communication over untrusted networks.

---

## 5. Types of Computer Networks and Security Zones

### LAN – Local Area Network

A LAN connects devices within a limited geographical area, such as an office, laboratory, or university building.

### WAN – Wide Area Network

A WAN connects networks across large geographical distances.

The internet is the most familiar example of a global network.

### WLAN – Wireless Local Area Network

A WLAN provides local network access through wireless communication technologies such as Wi-Fi.

### DMZ – Demilitarized Zone

A DMZ is a separate network segment used to host services that must be accessible from less-trusted networks.

Examples:
- Public web servers.
- Public DNS servers.
- Mail gateways.

A DMZ helps prevent direct exposure of internal systems.

### VLAN – Virtual Local Area Network

A VLAN logically separates devices within a switched network.

Example:

- VLAN 10 – Administration
- VLAN 20 – Students
- VLAN 30 – Servers
- VLAN 40 – Guests

VLAN separation helps reduce unnecessary communication between network groups.

**Important:** VLANs provide logical separation, but they must be combined with appropriate access controls and firewall rules.

---

## 6. Network Traffic Fundamentals

Network traffic refers to data moving between devices across a network.

Communication commonly involves:

1. A source device.
2. A destination device.
3. An IP address.
4. A communication protocol.
5. A port number when using transport-layer services.
6. Data packets.

### Common network protocols

| Protocol | Default port | Purpose |
|---|---:|---|
| HTTP | 80 | Web communication |
| HTTPS | 443 | Encrypted web communication |
| SSH | 22 | Secure remote administration |
| DNS | 53 | Domain name resolution |
| DHCP | 67/68 | Automatic IP configuration |
| FTP | 21 | File transfer control |
| SMTP | 25 | Email transmission |
| RDP | 3389 | Remote Desktop |

Ports shown are common defaults. Services can be configured to use different ports.

### Why protocols matter in security

Some protocols provide encryption and authentication, while others may transmit information without adequate protection.

For example:

- HTTP does not provide transport encryption by itself.
- HTTPS uses TLS to protect web communication.
- SSH provides encrypted remote administration.
- Telnet transmits data without modern transport encryption.

Security professionals must understand which protocols are being used and whether they are configured securely.

---

## 7. OSI Model and Network Security

The OSI model divides network communication into seven layers.

| Layer | Name | Security relevance |
|---|---|---|
| 7 | Application | Application security, HTTP, DNS |
| 6 | Presentation | Data encoding and encryption concepts |
| 5 | Session | Session management |
| 4 | Transport | TCP/UDP ports and filtering |
| 3 | Network | IP addressing and routing |
| 2 | Data Link | MAC addresses and VLAN security |
| 1 | Physical | Physical access and cable security |

### Example

When a user accesses a website using HTTPS:

- The application layer handles HTTP requests.
- TLS provides encryption and authentication.
- The transport layer commonly uses TCP.
- The network layer handles IP addressing.
- The data-link layer handles local network delivery.

Understanding the layers helps security analysts identify where a problem or attack may occur.

---

## 8. Threats, Vulnerabilities and Risks

These three concepts are related but have different meanings.

### Threat

A threat is anything capable of causing harm to a system or network.

Examples:
- Malware.
- Unauthorized users.
- Insider threats.
- Denial-of-service attacks.

### Vulnerability

A vulnerability is a weakness that could be exploited.

Examples:
- Outdated software.
- Weak passwords.
- Misconfigured firewalls.
- Unnecessary open services.

### Risk

Risk is the possibility of a threat exploiting a vulnerability and causing damage.

A simplified model is:

**Risk = Likelihood × Impact**

Example:

An outdated public-facing server may have:
- A vulnerability: unpatched software.
- A threat: an attacker attempting exploitation.
- A risk: service compromise or data exposure.

---

## 9. Attack Surface and Attack Vector

### Attack Surface

The attack surface is the total collection of points through which a system could potentially be accessed or attacked.

Examples:
- Open network ports.
- Web applications.
- Wireless networks.
- Remote access services.
- User accounts.

### Attack Vector

An attack vector is the method used to exploit a weakness or gain unauthorized access.

Examples:
- Phishing emails.
- Exploitation of vulnerable services.
- Weak passwords.
- Malicious attachments.
- Misconfigured remote access.

### Difference

| Attack surface | Attack vector |
|---|---|
| Potential points of exposure | Method used to attack |
| Describes where weaknesses exist | Describes how an attack occurs |

---

## 10. Types of Security Controls

Security controls are safeguards used to reduce security risks.

### Preventive controls

Designed to stop incidents before they occur.

Examples:
- Firewalls.
- Access control.
- Encryption.
- MFA.

### Detective controls

Designed to identify suspicious activities.

Examples:
- IDS.
- Security logs.
- Network monitoring.
- File integrity monitoring.

### Corrective controls

Designed to restore systems or correct problems after an incident.

Examples:
- Restoring backups.
- Applying security patches.
- Removing malware.

### Deterrent controls

Designed to discourage unauthorized behavior.

Examples:
- Warning banners.
- Security policies.
- Visible security monitoring notices.

### Compensating controls

Alternative safeguards used when the preferred security control cannot be implemented.

Example:

If an old device cannot support modern MFA, network isolation and strict access restrictions may reduce its exposure while a replacement is planned.

---

## 11. Important Network Security Principles

### 11.1 Least Privilege

Users and applications should receive only the permissions necessary to perform their tasks.

Example:

A student account should not have administrator privileges on a university server.

### 11.2 Defense in Depth

Defense in depth means using multiple layers of security instead of depending on one security control.

Example:

1. Firewall.
2. Network segmentation.
3. Authentication.
4. Endpoint protection.
5. IDS monitoring.
6. Backups.

If one control fails, other controls may still protect the environment.

### 11.3 Zero Trust

Zero Trust is a security approach based on the principle:

**Never automatically trust; always verify.**

Access decisions should consider identity, device security, permissions, and context.

### 11.4 Network Segmentation

Network segmentation divides a network into smaller sections.

Benefits:
- Limits unnecessary access.
- Reduces lateral movement.
- Improves monitoring.
- Helps contain incidents.

### 11.5 Secure Configuration

Systems should be configured according to security requirements.

Examples:
- Disable unnecessary services.
- Remove unused accounts.
- Use secure protocols.
- Apply updates.
- Restrict administrative access.

---

## 12. Common Network Security Tools

Security professionals use several tools to understand and protect networks.

| Tool | Main purpose |
|---|---|
| Wireshark | Graphical packet analysis |
| tcpdump | Command-line packet capture |
| Nmap | Network discovery and service auditing |
| UFW | Simplified Linux firewall management |
| nftables | Linux packet filtering |
| Suricata | Network threat detection and prevention |
| Snort | Network intrusion detection and prevention |

Tools must be used only on networks and systems you own or have explicit permission to assess.

---

# 13. PRACTICAL LAB: NETWORK SECURITY INSPECTION

## Lab objective

Inspect your Kali Linux virtual machine's network configuration and identify basic security information.

This lab is passive and does not require attacking or scanning another device.

### Step 1: Create a laboratory directory

```bash
mkdir -p ~/module15-lab/lesson01
cd ~/module15-lab/lesson01
```

### Step 2: Identify network interfaces

```bash
ip addr
```

Observe:
- Interface names.
- IPv4 addresses.
- IPv6 addresses.
- Interface state.
- MAC addresses.

### Step 3: Inspect routing information

```bash
ip route
```

Identify:
- Default gateway.
- Network interface used for routing.
- Connected network routes.

### Step 4: Inspect listening services

```bash
ss -tulpen
```

Understand the options:

| Option | Meaning |
|---|---|
| -t | TCP sockets |
| -u | UDP sockets |
| -l | Listening sockets |
| -p | Process information |
| -e | Extended information |
| -n | Numeric addresses and ports |

Some process details may require administrator privileges.

### Step 5: Inspect hostname information

```bash
hostname
hostname -I
```

The first command displays the system hostname. The second displays assigned IP addresses.

### Step 6: Inspect DNS configuration

```bash
cat /etc/resolv.conf
```

Observe the configured DNS resolver information.

### Step 7: Inspect local network connectivity

```bash
ip link
```

Check whether your network interface is operational.

If you know your own VM's configured default gateway, you may test basic connectivity to it:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Replace `YOUR_GATEWAY_IP` with the actual gateway address from `ip route`.

If ICMP is blocked, a failed ping does not necessarily mean the gateway is unavailable.

### Step 8: Save your inspection results

```bash
ip addr > network-interfaces.txt
ip route > routing-information.txt
ss -tulpen > listening-services.txt
hostname -I > ip-addresses.txt
```

List the generated files:

```bash
ls -l
```

---

# 14. PRACTICAL EXERCISE: CREATE A NETWORK ASSET INVENTORY

A network asset inventory records the devices and services that must be protected.

Create a file:

```bash
nano asset-inventory.txt
```

Enter this example and replace the sample information with your actual VM details:

```text
NETWORK ASSET INVENTORY

Asset 1:
Name: Kali Linux VM
Type: Virtual Machine
Operating System: Kali Linux
IP Address: Record from ip addr
Purpose: Cybersecurity laboratory
Owner: Lab administrator
Security Importance: High

Asset 2:
Name: Windows Host
Type: Physical Laptop
Operating System: Windows 11 Pro
Purpose: Main workstation
Security Importance: High
```

Save the file.

Remember: your Windows host and Kali VM are separate assets with different security boundaries.

---

# 15. PRACTICAL EXERCISE: CREATE A BASIC RISK REGISTER

A risk register records security risks and their possible consequences.

Create:

```bash
nano risk-register.txt
```

Enter:

```text
NETWORK SECURITY RISK REGISTER

Risk 1:
Description: Weak administrative password
Likelihood: Medium
Impact: High
Suggested Control: Strong password and MFA

Risk 2:
Description: Unnecessary network services
Likelihood: Medium
Impact: Medium
Suggested Control: Disable unused services

Risk 3:
Description: Outdated software
Likelihood: Medium
Impact: High
Suggested Control: Regular security updates

Risk 4:
Description: Unprotected network communication
Likelihood: Medium
Impact: High
Suggested Control: Use secure protocols and encryption
```

Save the file.

These are illustrative risks, not findings from an actual security assessment.

---

# 16. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is network security?
2. Explain the CIA Triad.
3. What is the difference between authentication and authorization?
4. What does accounting mean in AAA?
5. Explain the difference between a firewall, IDS, and IPS.
6. What is a DMZ?
7. What is a VLAN?
8. Explain the difference between a threat and a vulnerability.
9. What is an attack surface?
10. Explain defense in depth.
11. What is the principle of least privilege?
12. Why is HTTPS preferred over HTTP?
13. What is network segmentation?
14. What is the purpose of the `ss -tulpen` command?
15. Why should security professionals maintain a network asset inventory?

---

# 17. LESSON SUMMARY

In this lesson, you learned:

- The meaning and objectives of network security.
- The CIA Triad.
- Authentication, authorization, and accounting.
- Network security devices.
- LAN, WAN, WLAN, DMZ, and VLAN.
- Network traffic and common protocols.
- The OSI model and its security relevance.
- Threats, vulnerabilities, risks, attack surfaces, and attack vectors.
- Types of security controls.
- Least privilege, Zero Trust, and defense in depth.
- Common network security tools.
- How to inspect your own Kali VM's network configuration.
- How to create an asset inventory and risk register.

**Key takeaway:**

Network security is not simply about blocking attackers. It is about protecting information, controlling access, monitoring communication, reducing risk, and maintaining reliable network services.

**END OF LESSON 01**