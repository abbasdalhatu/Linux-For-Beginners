---
---

# MODULE 15 – NETWORK SECURITY
## LESSON 02: NETWORK THREATS AND ATTACKS

---

## 1. Introduction

A network threat is any potential danger that could compromise the confidentiality, integrity, or availability of network systems and information.

Network attacks occur when someone or something attempts to exploit a weakness in a network, device, application, or security configuration.

Examples include:

- Malware infections.
- Denial-of-service attacks.
- Phishing.
- Unauthorized network access.
- Password attacks.
- Man-in-the-middle attacks.
- DNS manipulation.

Understanding network threats helps cybersecurity professionals identify risks, implement defenses, and respond to incidents.

## 2. Threat, Attack, Vulnerability, and Exploit

These terms are related but have different meanings.

### 2.1 Threat

A threat is a potential source of harm.

Example: A cybercriminal attempting to access a company's network.

### 2.2 Attack

An attack is an attempt to compromise a system or network.

Example: Attempting to exploit an exposed service.

### 2.3 Vulnerability

A vulnerability is a weakness that could be exploited.

Example: An outdated server running software with a known security flaw.

### 2.4 Exploit

An exploit is a technique, procedure, or piece of code used to take advantage of a vulnerability.

Example: A specially crafted request that triggers a software vulnerability.

### Comparison

| Term | Meaning |
|---|---|
| Threat | Potential source of harm |
| Attack | Attempt to cause harm or gain unauthorized access |
| Vulnerability | Weakness in a system |
| Exploit | Method used to take advantage of a weakness |

## 3. Categories of Network Threats

Network threats can be divided into several categories.

### 3.1 Malware Threats

Malware means malicious software designed to damage systems, steal information, disrupt services, or gain unauthorized access.

Common types include:

**Virus:** Malicious code that attaches itself to files or programs and spreads when they execute.

**Worm:** Malware capable of spreading between systems, often through network connections, without requiring the same user actions as a traditional virus.

**Trojan:** Malicious software disguised as legitimate software.

**Ransomware:** Malware that encrypts or otherwise makes data inaccessible and demands payment.

**Spyware:** Software designed to secretly collect information.

**Rootkit:** Software designed to conceal malicious activity or maintain privileged access.

**Botnet malware:** Malware that makes infected devices part of a remotely controlled network.

### 3.2 Social Engineering Threats

Social engineering manipulates people into revealing information or performing unsafe actions.

Examples:

- Phishing emails.
- Fake technical support calls.
- Impersonation.
- Fraudulent login pages.
- Messages pretending to come from university administrators.

The attacker targets human trust rather than only technical weaknesses.

### 3.3 Network-Based Threats

These threats target network communication or network infrastructure.

Examples:

- Denial-of-service attacks.
- Man-in-the-middle attacks.
- ARP spoofing.
- DNS spoofing.
- Unauthorized access.
- Traffic interception.

### 3.4 Insider Threats

An insider threat comes from someone with legitimate access to an organization or its systems.

Examples:

- An employee stealing confidential information.
- An administrator misusing privileges.
- A staff member accidentally exposing sensitive records.

Insider threats can be intentional or accidental.

### 3.5 Physical Threats

Physical threats involve direct access to devices or infrastructure.

Examples:

- Theft of network equipment.
- Unauthorized access to server rooms.
- Damage to network cables.
- Unauthorized connection of devices to network ports.

---

## 4. Passive and Active Attacks

Network attacks can also be classified according to how they interact with a target.

### 4.1 Passive Attacks

A passive attack involves observing or collecting information without intentionally modifying the communication or system.

Examples:

- Eavesdropping.
- Traffic analysis.
- Unauthorized packet observation.

**Main objective:** Obtain information.

**Security impact:** Primarily threatens confidentiality.

### 4.2 Active Attacks

An active attack attempts to modify, disrupt, inject, or manipulate network communication or systems.

Examples:

- Denial-of-service attacks.
- Message modification.
- Session hijacking.
- Unauthorized command execution.

**Main objective:** Change or disrupt system behavior.

**Security impact:** Can affect confidentiality, integrity, and availability.

### Comparison

| Passive attack | Active attack |
|---|---|
| Observes communication | Alters or disrupts communication |
| Often difficult to detect | May create observable changes |
| Threatens confidentiality | Can threaten all CIA principles |
| Example: Eavesdropping | Example: DoS |

---

## 5. Common Network Attacks

### 5.1 Denial-of-Service (DoS)

A DoS attack attempts to make a service or network resource unavailable to legitimate users.

Example:

An attacker sends excessive requests to a web server, overwhelming its resources.

Possible consequences:
- Slow service.
- Failed connections.
- Application downtime.
- Financial losses.

### 5.2 Distributed Denial-of-Service (DDoS)

A DDoS attack uses multiple devices or sources to overwhelm a target.

Difference:

- DoS: Attack traffic may originate from one source.
- DDoS: Attack traffic originates from multiple sources.

DDoS attacks are often harder to mitigate because traffic is distributed.

### 5.3 Man-in-the-Middle (MITM)

A MITM attack occurs when an attacker positions themselves between two communicating parties and intercepts or manipulates their communication.

Possible consequences:
- Information exposure.
- Session compromise.
- Communication manipulation.

Defenses:
- HTTPS and TLS.
- Certificate validation.
- Secure Wi-Fi.
- VPNs where appropriate.
- Network monitoring.

### 5.4 ARP Spoofing

ARP spoofing involves sending falsified Address Resolution Protocol messages to associate an attacker's MAC address with another device's IP address.

It can enable traffic interception on a local network.

Defenses:
- Dynamic ARP Inspection where supported.
- Secure switch configuration.
- Network segmentation.
- Monitoring unusual ARP changes.

### 5.5 DNS Spoofing

DNS spoofing involves manipulating DNS responses or records to redirect users to an incorrect destination.

Possible consequences:
- Fake websites.
- Credential theft.
- Traffic redirection.

Defenses:
- DNSSEC validation where available.
- Secure DNS infrastructure.
- Monitoring DNS changes.
- HTTPS certificate validation.

### 5.6 Password Attacks

Password attacks attempt to obtain or misuse account credentials.

Examples:
- Brute-force attacks.
- Dictionary attacks.
- Password spraying.
- Credential stuffing.

Defenses:
- Strong unique passwords.
- MFA.
- Rate limiting.
- Account lockout policies.
- Monitoring failed logins.

### 5.7 Session Hijacking

Session hijacking occurs when an attacker obtains or takes control of a valid user session.

Defenses:
- Secure session cookies.
- HTTPS.
- Short session lifetimes for sensitive services.
- Session rotation after authentication.
- MFA and reauthentication for sensitive actions.

---

## 6. Network Attack Classification

| Attack | Main target | Main risk |
|---|---|---|
| DoS | Network service | Availability |
| DDoS | Network service | Availability |
| MITM | Communication | Confidentiality and integrity |
| ARP spoofing | Local network | Traffic interception |
| DNS spoofing | Name resolution | Redirection |
| Password attack | User accounts | Unauthorized access |
| Session hijacking | User sessions | Account compromise |
| Malware | Devices and systems | Multiple security impacts |

---

## 7. Common Sources of Network Threats

### External attackers

Individuals or groups operating outside an organization.

### Malicious insiders

Authorized users who intentionally misuse their access.

### Accidental insiders

Users who unintentionally create security problems.

### Organized cybercrime groups

Groups that conduct attacks for financial gain.

### Nation-state actors

Actors associated with governments or state interests.

### Hacktivists

Individuals or groups conducting cyber activities to promote political or social causes.

### Automated threats

Malware, bots, and automated tools that search for vulnerable systems.

---

## 8. Indicators of Network Attacks

Security analysts monitor indicators that may suggest suspicious network activity.

Examples:

1. Unexpected spikes in network traffic.
2. Repeated failed login attempts.
3. Unknown devices appearing on the network.
4. Unexpected open services.
5. Unusual outbound connections.
6. Unexpected changes to DNS configuration.
7. Repeated firewall-denied connections.
8. Unusual ARP table changes.
9. Unexpected account privilege changes.
10. Connections to suspicious destinations.

**Important:** An indicator is not automatic proof of an attack. Analysts investigate and correlate multiple observations before reaching a conclusion.

---

## 9. Methods of Preventing Network Attacks

### 9.1 Strong Authentication

Use:
- Strong unique passwords.
- Multi-factor authentication.
- Secure administrative accounts.

### 9.2 Firewall Protection

Firewalls control network communication according to security rules.

### 9.3 Network Segmentation

Separate sensitive systems from less-trusted devices.

### 9.4 Regular Patching

Update operating systems, applications, and network devices to fix known vulnerabilities.

### 9.5 Encryption

Protect sensitive information during transmission and storage.

### 9.6 IDS and IPS

Use detection and prevention systems to identify suspicious network activities.

### 9.7 Backups

Maintain protected backups to support recovery after ransomware or other incidents.

### 9.8 Security Awareness

Train users to identify phishing, impersonation, suspicious attachments, and fraudulent requests.

---

## 10. Threat Modeling

Threat modeling is the process of identifying possible threats, understanding how they could affect a system, and selecting suitable defenses.

A basic threat modeling process:

1. Identify important assets.
2. Understand the system architecture.
3. Identify possible threats.
4. Identify vulnerabilities.
5. Estimate likelihood and impact.
6. Select security controls.
7. Review and update the model.

### Example: University Student Portal

| Asset | Threat | Possible impact | Defense |
|---|---|---|---|
| Student accounts | Password attacks | Account compromise | MFA |
| Student database | Unauthorized access | Data exposure | Access controls |
| Web server | DoS | Service interruption | DDoS protection |
| DNS service | DNS manipulation | Traffic redirection | DNS security |
| Admin accounts | Insider misuse | Unauthorized changes | Least privilege and auditing |

---

# 11. PRACTICAL LAB: IDENTIFY NETWORK THREAT INDICATORS

## Lab objective

Learn to inspect your Kali VM for basic network information and recognize potential indicators of suspicious activity.

This is a defensive inspection lab. It does not involve attacking any device.

### Step 1: Create the lesson directory

```bash
mkdir -p ~/module15-lab/lesson02
cd ~/module15-lab/lesson02
```

### Step 2: Inspect network interfaces

```bash
ip addr
```

Record:
- Interface names.
- Assigned IP addresses.
- Interface state.

### Step 3: Inspect network connections

```bash
ss -tun
```

This displays TCP and UDP socket information.

Observe:
- Local addresses.
- Remote addresses.
- Connection states.

### Step 4: Inspect listening services

```bash
ss -tulpen
```

Look for services listening for incoming connections.

Do not assume that every listening port is malicious. Determine whether the service is expected.

### Step 5: Inspect the routing table

```bash
ip route
```

Check whether the default gateway and network routes match your VM's expected configuration.

### Step 6: Inspect recent authentication logs

On Kali, authentication events may be recorded in system logs.

Try:

```bash
sudo journalctl -u ssh --no-pager -n 30
```

If the SSH service has no logs, that may simply mean it is not installed, running, or logging through that unit.

You can also inspect general recent system messages:

```bash
sudo journalctl --no-pager -n 30
```

Look for:
- Failed authentication messages.
- Unexpected service activity.
- Repeated errors.

### Step 7: Save the inspection results

```bash
ip addr > interfaces.txt
ip route > routes.txt
ss -tun > network-connections.txt
ss -tulpen > listening-services.txt
sudo journalctl --no-pager -n 30 > recent-system-events.txt
```

Check your files:

```bash
ls -lh
```

---

# 12. PRACTICAL EXERCISE: CREATE A THREAT REGISTER

Create:

```bash
nano threat-register.txt
```

Enter:

```text
NETWORK THREAT REGISTER

Threat 1:
Name: Malware
Category: Malicious Software
Target: Network-connected devices
Impact: Data theft and system compromise
Defense: Endpoint protection and patching

Threat 2:
Name: Denial of Service
Category: Network Attack
Target: Network services
Impact: Service unavailability
Defense: Traffic filtering and DDoS protection

Threat 3:
Name: Phishing
Category: Social Engineering
Target: Network users
Impact: Credential theft
Defense: User awareness and MFA

Threat 4:
Name: Man-in-the-Middle
Category: Communication Attack
Target: Network traffic
Impact: Information interception
Defense: TLS and secure network configuration

Threat 5:
Name: Unauthorized Access
Category: Access Control Threat
Target: Network resources
Impact: Data exposure
Defense: Strong authentication and least privilege
```

Save the file.

---

# 13. PRACTICAL EXERCISE: THREAT ANALYSIS

For each scenario, identify:

1. The threat.
2. The affected asset.
3. The possible vulnerability.
4. The potential impact.
5. A suitable defense.

### Scenario A

A university employee receives an email pretending to come from the IT department. The email requests the employee's login credentials.

### Scenario B

A public web server becomes unavailable after receiving an unusually large volume of requests.

### Scenario C

A company discovers that an outdated server is running software with a known security vulnerability.

### Scenario D

An employee uses administrator privileges to access confidential records unrelated to their job.

Write your answers in:

```bash
nano threat-analysis.txt
```

---

# 14. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is a network threat?
2. Differentiate between a threat, attack, vulnerability, and exploit.
3. What is malware?
4. Explain the difference between a virus and a worm.
5. What is a Trojan?
6. Differentiate between DoS and DDoS.
7. What is a Man-in-the-Middle attack?
8. Explain ARP spoofing.
9. What is DNS spoofing?
10. Differentiate between passive and active attacks.
11. What is an insider threat?
12. Mention five indicators of suspicious network activity.
13. What is threat modeling?
14. Explain three ways to prevent network attacks.
15. Why is network monitoring important?

---

# 15. LESSON SUMMARY

In this lesson, you learned:

- The meaning of network threats and attacks.
- The difference between threats, attacks, vulnerabilities, and exploits.
- Malware and social engineering threats.
- Insider, physical, and network-based threats.
- Passive and active attacks.
- DoS, DDoS, MITM, ARP spoofing, and DNS spoofing.
- Password attacks and session hijacking.
- Common indicators of suspicious activity.
- Network attack prevention methods.
- Threat modeling.
- How to perform basic defensive network inspection in Kali Linux.

**Key takeaway:**

A cybersecurity professional must understand how attacks happen, recognize warning signs, assess risks, and apply appropriate defenses before a small weakness becomes a serious security incident.

**END OF LESSON 02**