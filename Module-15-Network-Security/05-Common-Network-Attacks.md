---
---

# MODULE 15 – NETWORK SECURITY
## LESSON 05: COMMON NETWORK ATTACKS

---

## 1. Introduction

A network attack is an attempt to gain unauthorized access, steal information, manipulate communication, or disrupt network services.

Cybersecurity professionals must understand common network attacks to recognize their warning signs and implement appropriate defenses.

In this lesson, we will study common attacks from a defensive perspective, including their methods, impacts, detection, and prevention.

---

## 2. Denial-of-Service (DoS) Attack

A Denial-of-Service attack attempts to make a network service unavailable to legitimate users.

### How it works

An attacker overwhelms a server or network resource with excessive requests or traffic.

This can exhaust:
- CPU resources.
- Memory.
- Network bandwidth.
- Available connections.

### Example

A university student portal normally handles 500 simultaneous users.

Suddenly, it receives an unusually large volume of requests, causing legitimate students to experience slow loading or failed connections.

### Possible impacts

- Website downtime.
- Lost productivity.
- Financial losses.
- Interrupted services.

### Detection

Security analysts may observe:
- Unusual traffic spikes.
- Increased server resource usage.
- High connection counts.
- Increased response times.
- Repeated connection failures.

### Prevention

- Traffic filtering.
- Rate limiting.
- DDoS protection services.
- Network monitoring.
- Redundant infrastructure.
- Incident response procedures.

---

## 3. Distributed Denial-of-Service (DDoS)

A Distributed Denial-of-Service attack is similar to DoS, but traffic comes from multiple sources.

Attackers may use compromised devices in a botnet.

### How it works

1. Devices become infected or compromised.
2. The attacker controls those devices.
3. Multiple devices send traffic toward a target.
4. The target becomes overwhelmed.
5. Legitimate users experience service disruption.

### DoS vs. DDoS

| DoS | DDoS |
|---|---|
| Often originates from one source | Uses multiple sources |
| May be easier to isolate | More difficult to distinguish legitimate traffic |
| Limited distribution | Distributed traffic sources |

### Defensive measures

- DDoS mitigation services.
- Traffic analysis.
- Rate limiting.
- Network filtering.
- Upstream provider coordination.

---

## 4. Man-in-the-Middle (MITM) Attack

A Man-in-the-Middle attack occurs when an attacker intercepts communication between two parties.

### Example

A user connects to an unsafe public wireless network and communicates with a service that does not properly protect the connection.

An attacker positioned on the communication path may be able to observe or manipulate unprotected traffic.

### Potential impacts

- Information theft.
- Session compromise.
- Communication manipulation.
- Exposure of sensitive information.

### Common MITM techniques

- ARP spoofing.
- Rogue wireless access points.
- DNS manipulation.
- Downgrade attacks.
- Session hijacking.

### Prevention

- Use HTTPS and TLS.
- Validate digital certificates.
- Avoid untrusted wireless networks.
- Use secure VPN connections where appropriate.
- Keep devices updated.
- Enable MFA.

---

## 5. ARP Spoofing Attack

ARP stands for Address Resolution Protocol.

ARP helps devices associate IPv4 addresses with MAC addresses on a local network.

### Normal ARP operation

Imagine:

```text
Device A:
IP Address: 192.168.1.10
MAC Address: AA:AA:AA:AA:AA:AA

Device B:
IP Address: 192.168.1.20
MAC Address: BB:BB:BB:BB:BB:BB
```

Device A needs to communicate with Device B.

It uses ARP to discover Device B's MAC address.

### ARP spoofing

An attacker sends false ARP information to associate an incorrect MAC address with a legitimate IP address.

This may redirect local network traffic through the attacker's device.

### Potential impacts

- Traffic interception.
- Communication manipulation.
- Session exposure.
- Network disruption.

### Detection

- Unexpected IP-to-MAC address changes.
- Duplicate IP address warnings.
- Unusual ARP traffic.
- Unexpected changes in neighbor tables.

### Prevention

- Dynamic ARP Inspection.
- Static ARP entries for selected critical systems where appropriate.
- Network segmentation.
- Secure switch configuration.
- Monitoring ARP behavior.

---

## 6. DNS Spoofing and DNS Poisoning

DNS translates domain names into IP addresses.

For example:

```text
example.com → IP address
```

DNS spoofing involves providing false DNS information to redirect a user or system.

DNS cache poisoning is a specific type of DNS manipulation where false information is inserted into a DNS resolver's cache.

### Example

A user attempts to access a legitimate website.

A manipulated DNS response directs the user to an attacker-controlled destination instead.

### Potential impacts

- Phishing.
- Credential theft.
- Traffic redirection.
- Exposure to malicious websites.

### Prevention

- DNSSEC validation.
- Secure DNS resolver configuration.
- Monitoring DNS changes.
- HTTPS certificate verification.
- Secure DNS infrastructure.

---

## 7. Password Attacks

Password attacks attempt to obtain, guess, or misuse account credentials.

### 7.1 Brute-Force Attack

A brute-force attack systematically tries possible password combinations.

### 7.2 Dictionary Attack

A dictionary attack tries passwords from a prepared list of common words or known passwords.

### 7.3 Password Spraying

Password spraying attempts a small number of commonly used passwords against many accounts.

### 7.4 Credential Stuffing

Credential stuffing uses previously exposed username and password combinations against other services.

It takes advantage of password reuse.

### Defenses

- Strong, unique passwords.
- Multi-factor authentication.
- Login rate limiting.
- Account lockout policies.
- Detection of unusual login behavior.
- Password managers.

---

## 8. Session Hijacking

A session represents an authenticated interaction between a user and an application.

Session hijacking occurs when an attacker obtains or takes control of another user's valid session.

### Example

A web application uses a session cookie to remember that a user has authenticated.

If that session token is exposed, an attacker may attempt to reuse it.

### Potential impacts

- Unauthorized account access.
- Data exposure.
- Unauthorized transactions.
- Account manipulation.

### Prevention

- HTTPS everywhere.
- Secure and HttpOnly cookies.
- Appropriate SameSite cookie settings.
- Session expiration.
- Session rotation after authentication.
- Reauthentication for sensitive actions.

---

## 9. Port Scanning and Service Discovery

Port scanning is a technique used to identify network ports that are open, closed, or filtered.

It is commonly used by:
- Network administrators.
- Security auditors.
- Penetration testers.
- Attackers.

### Why it matters

Open ports may reveal services that require security review.

For example:

| Port | Service | Security consideration |
|---:|---|---|
| 22 | SSH | Restrict remote administration |
| 23 | Telnet | Avoid insecure remote access |
| 80 | HTTP | Review transport security |
| 443 | HTTPS | Maintain secure TLS configuration |
| 3389 | RDP | Restrict remote desktop access |

### Defensive measures

- Disable unnecessary services.
- Apply firewall rules.
- Monitor unexpected discovery activity.
- Restrict administrative interfaces.
- Maintain accurate service inventories.

**Important:** Port scanning is not automatically malicious. It is also a legitimate administrative and auditing activity when authorized.

---

## 10. MAC Flooding

MAC flooding targets the MAC address table of certain network switches.

An attacker attempts to overwhelm the switch's address-learning capacity.

Depending on the switch and configuration, this may cause unusual forwarding behavior and increase the possibility of traffic exposure.

### Defenses

- Switch port security.
- MAC address limits.
- Network monitoring.
- VLAN segmentation.
- Modern switch configurations.

---

## 11. DHCP Starvation and Rogue DHCP Servers

### DHCP Starvation

DHCP starvation attempts to exhaust a DHCP server's available address pool by generating excessive address allocation requests.

This may prevent legitimate devices from receiving IP addresses.

### Rogue DHCP Server

A rogue DHCP server is an unauthorized server that provides incorrect network configuration information.

It may distribute:
- Incorrect gateways.
- Unauthorized DNS servers.
- Incorrect IP settings.

### Defenses

- DHCP snooping.
- Trusted switch ports.
- Network monitoring.
- Secure switch configuration.
- DHCP server access controls.

---

## 12. Replay Attacks

A replay attack involves capturing valid communication data and retransmitting it later to repeat an action or impersonate a legitimate message.

### Example

A poorly designed authentication system accepts a previously captured authentication message without verifying that it is fresh.

### Prevention

- Nonces.
- Timestamps.
- Session identifiers.
- Challenge-response authentication.
- Properly designed cryptographic protocols.

---

## 13. Comparing Common Network Attacks

| Attack | Primary target | Main objective |
|---|---|---|
| DoS | Network service | Disrupt availability |
| DDoS | Network service | Disrupt availability |
| MITM | Communication | Intercept or manipulate traffic |
| ARP spoofing | Local network | Redirect local traffic |
| DNS spoofing | DNS resolution | Redirect users |
| Brute force | User account | Guess credentials |
| Session hijacking | Authenticated session | Take over a session |
| MAC flooding | Network switch | Disrupt MAC learning |
| DHCP starvation | DHCP service | Exhaust address allocation |
| Replay | Authentication or communication | Reuse captured messages |

---

## 14. Network Attack Defense Strategy

A strong defense uses multiple layers of protection.

### Layer 1: Network Protection

- Firewalls.
- Network segmentation.
- Secure routing.
- Access control lists.

### Layer 2: Endpoint Protection

- Antivirus and endpoint detection.
- Security updates.
- Host firewalls.
- Secure configurations.

### Layer 3: Identity Protection

- MFA.
- Strong passwords.
- Least privilege.
- Account monitoring.

### Layer 4: Detection

- IDS and IPS.
- Security event logging.
- Network traffic analysis.
- Alert investigation.

### Layer 5: Recovery

- Backups.
- Incident response plans.
- System restoration.
- Post-incident review.

---

# 15. PRACTICAL LAB: DEFENSIVE NETWORK ATTACK INDICATOR INSPECTION

## Lab objective

Inspect your own Kali VM for basic network indicators that may be useful during a defensive security assessment.

This lab does not generate attack traffic or perform interception.

### Step 1: Create the directory

```bash
mkdir -p ~/module15-lab/lesson05
cd ~/module15-lab/lesson05
```

### Step 2: Inspect network interfaces

```bash
ip addr
```

Identify your active interface.

### Step 3: Inspect ARP and neighbor information

```bash
ip neigh
```

Record:
- Neighbor IP addresses.
- MAC addresses.
- Neighbor states.

An incomplete or empty neighbor table is not proof of an attack.

### Step 4: Inspect listening services

```bash
ss -tulpen
```

Record services listening for incoming connections.

### Step 5: Inspect current network connections

```bash
ss -tun
```

Review local and remote addresses.

### Step 6: Inspect recent system logs

```bash
sudo journalctl --no-pager -n 50
```

Look for unusual service failures or authentication events.

### Step 7: Inspect DNS configuration

```bash
cat /etc/resolv.conf
```

Record the configured DNS resolver information.

### Step 8: Save the results

```bash
ip addr > interfaces.txt
ip neigh > neighbor-table.txt
ss -tulpen > listening-services.txt
ss -tun > network-connections.txt
cat /etc/resolv.conf > dns-configuration.txt
sudo journalctl --no-pager -n 50 > system-events.txt
```

Review the files:

```bash
ls -lh
```

---

# 16. PRACTICAL PROJECT: NETWORK ATTACK DEFENSE MATRIX

Create:

```bash
nano attack-defense-matrix.txt
```

Enter:

```text
NETWORK ATTACK DEFENSE MATRIX

ATTACK 1:
Name: Denial-of-Service
Detection: Unusual traffic volume
Defense: Rate limiting and traffic filtering

ATTACK 2:
Name: Man-in-the-Middle
Detection: Unexpected certificate warnings
Defense: TLS and certificate validation

ATTACK 3:
Name: ARP Spoofing
Detection: Unexpected IP-to-MAC changes
Defense: Dynamic ARP Inspection

ATTACK 4:
Name: DNS Spoofing
Detection: Unexpected DNS responses
Defense: DNSSEC validation and secure DNS

ATTACK 5:
Name: Password Attacks
Detection: Repeated failed logins
Defense: MFA and login rate limiting

ATTACK 6:
Name: DHCP Starvation
Detection: Unusual DHCP allocation activity
Defense: DHCP snooping

ATTACK 7:
Name: Session Hijacking
Detection: Suspicious session reuse
Defense: Secure cookies and session management
```

Save the file.

---

# 17. SCENARIO-BASED EXERCISE

Analyze each scenario.

For every scenario, identify:

1. The likely attack.
2. The targeted asset.
3. The possible security impact.
4. At least two defensive measures.

### Scenario A

A university website becomes unavailable after receiving an unusually large number of requests from many different IP addresses.

### Scenario B

A student notices that the MAC address associated with the university gateway has unexpectedly changed.

### Scenario C

An employee receives repeated login alerts from unfamiliar locations.

### Scenario D

A user is redirected to an unfamiliar website after entering a legitimate domain name.

### Scenario E

A network administrator discovers that unauthorized devices are distributing IP configuration information to users.

Save your answers in:

```bash
nano attack-scenarios.txt
```

---

# 18. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is a Denial-of-Service attack?
2. Differentiate between DoS and DDoS.
3. Explain a Man-in-the-Middle attack.
4. What is ARP spoofing?
5. Explain DNS spoofing.
6. What is the difference between brute-force and dictionary attacks?
7. What is credential stuffing?
8. Explain session hijacking.
9. What is port scanning?
10. What is MAC flooding?
11. Explain DHCP starvation.
12. What is a rogue DHCP server?
13. What is a replay attack?
14. Mention five methods of defending against network attacks.
15. Why is defense in depth important?

---

# 19. LESSON SUMMARY

In this lesson, you learned:

- Denial-of-Service and Distributed Denial-of-Service attacks.
- Man-in-the-Middle attacks.
- ARP spoofing.
- DNS spoofing and cache poisoning.
- Password attacks.
- Session hijacking.
- Port scanning and service discovery.
- MAC flooding.
- DHCP starvation and rogue DHCP servers.
- Replay attacks.
- Detection and prevention techniques.
- How to inspect your Kali VM defensively.
- How to create an attack-defense matrix.

**KEY TAKEAWAY:**

Understanding common network attacks is essential for building effective defenses. Security professionals must recognize attack indicators, understand their impact, and use multiple security controls to prevent, detect, and respond to threats.

**END OF LESSON 05**