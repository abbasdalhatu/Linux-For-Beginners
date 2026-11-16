# Module 10 – Lesson 14: Linux Networking Security

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain Linux network security.
2. Understand common network-based attacks.
3. Identify exposed network services.
4. Understand firewalls and packet filtering.
5. Understand SSH security.
6. Explain DNS security.
7. Understand network encryption.
8. Apply basic network security practices.
9. Inspect your Kali VM for basic network security issues.

---

## 2. What Is Linux Networking Security?

Linux networking security is the practice of protecting Linux systems and their network communications against unauthorized access, attacks, and data exposure.

It involves protecting:

- Network interfaces.
- Open ports.
- Network services.
- Remote connections.
- DNS configuration.
- Data transmitted over networks.
- Network access controls.

### Example

Imagine a Linux server running an SSH service.

If the service is exposed to the internet and uses weak passwords, attackers may attempt unauthorized access.

Network security helps reduce this risk through:

- Strong authentication.
- Firewall rules.
- Service restrictions.
- Security updates.
- Network monitoring.

---

## 3. Understanding the Network Attack Surface

An attack surface is the collection of points where an attacker might attempt to interact with a system.

Network attack surfaces may include:

- Open TCP ports.
- Open UDP ports.
- SSH services.
- Web servers.
- DNS services.
- Remote administration services.
- Exposed network interfaces.

The more unnecessary services a system exposes, the larger its potential attack surface.

### Inspect Your Network Attack Surface

Display listening services:

```bash
sudo ss -tulnp
```

Display network interfaces:

```bash
ip -br addr
```

Display active connections:

```bash
ss -tun
```

### Important

A listening port is not automatically a vulnerability.

You must investigate:

1. Which application owns the port?
2. Is the service required?
3. Is it exposed to the local network or a wider network?
4. Is it properly configured?
5. Is it running an updated version?

---

## 4. Common Network-Based Attacks

### 4.1 Port Scanning

Port scanning is the process of checking network ports to identify available services.

Attackers may use it to discover potential targets.

Security professionals also use authorized port scanning to audit systems.

Defensive measures:

- Close unnecessary ports.
- Restrict access to required services.
- Use firewall rules.
- Monitor unusual connection attempts.

### 4.2 Brute-Force Attacks

A brute-force attack attempts many possible passwords or authentication combinations.

SSH services can be targeted by automated login attempts.

Defensive measures:

- Use strong passwords.
- Prefer SSH key authentication where appropriate.
- Disable unnecessary remote access.
- Restrict access to trusted networks.
- Monitor authentication logs.

### 4.3 Denial-of-Service Attacks

A Denial-of-Service (DoS) attack attempts to make a system or service unavailable.

A Distributed Denial-of-Service (DDoS) attack uses multiple systems to generate attack traffic.

Possible effects:

- Service interruption.
- High bandwidth usage.
- Resource exhaustion.
- Increased latency.

Defensive measures:

- Network filtering.
- Traffic monitoring.
- Rate limiting.
- DDoS protection services.
- Capacity planning.

### 4.4 Man-in-the-Middle Attacks

A Man-in-the-Middle (MITM) attack occurs when an attacker intercepts or manipulates communication between parties.

Possible targets include:

- Public Wi-Fi connections.
- Unencrypted websites.
- Insecure network protocols.

Defensive measures:

- Use HTTPS.
- Validate TLS certificates.
- Use secure Wi-Fi.
- Avoid transmitting sensitive information over untrusted connections.
- Use secure remote access methods.

### 4.5 DNS Spoofing

DNS spoofing involves providing false DNS information to redirect users or systems to unintended destinations.

Defensive measures:

- Use trusted DNS resolvers.
- Protect DNS infrastructure.
- Monitor unexpected DNS configuration changes.
- Use DNSSEC where supported.
- Use HTTPS and validate certificates.

### 4.6 ARP Spoofing

ARP spoofing involves sending forged Address Resolution Protocol messages on a local IPv4 network.

An attacker may attempt to associate their MAC address with another device's IP address.

Possible consequences:

- Traffic interception.
- Traffic redirection.
- Communication disruption.

Defensive measures include:

- Network segmentation.
- Dynamic ARP Inspection on supported switches.
- Secure network access.
- Monitoring suspicious ARP behaviour.

---

## 5. Understanding Linux Firewalls

A firewall controls network traffic according to defined rules.

It can allow or block traffic based on factors such as:

- Source IP address.
- Destination IP address.
- Protocol.
- Source port.
- Destination port.
- Connection state.

Linux supports several packet-filtering technologies.

Examples include:

- Netfilter.
- nftables.
- iptables.
- UFW.

### Why Firewalls Matter

A firewall helps reduce unauthorized network access.

For example, a server may need to accept SSH connections but reject unnecessary incoming services.

A firewall can enforce this policy.

---

## 6. Understanding UFW

UFW stands for Uncomplicated Firewall.

It provides a simpler interface for managing firewall rules.

### Check Whether UFW Is Installed

```bash
command -v ufw
```

### Display Firewall Status

```bash
sudo ufw status
```

### Display Detailed Status

```bash
sudo ufw status verbose
```

### Understand Common UFW Commands

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

Deny a specific port:

```bash
sudo ufw deny 23/tcp
```

Enable the firewall:

```bash
sudo ufw enable
```

Disable the firewall:

```bash
sudo ufw disable
```

### Important Safety Warning

Do not blindly execute firewall-changing commands on your active VM.

Enabling a firewall with incorrect rules can interrupt network access.

If you ever administer a remote Linux server, ensure that the required management access is allowed before enabling firewall restrictions.

For this lesson, inspect firewall status only.

---

## 7. Understanding nftables

nftables is a Linux packet-filtering framework.

It is used to define rules controlling network traffic.

### Check nftables Status

```bash
systemctl status nftables
```

### Display Existing Rules

```bash
sudo nft list ruleset
```

If no rules are displayed, that does not automatically mean the entire system is insecure. Other firewall-management tools or network controls may be involved.

### Why nftables Is Important

It supports:

- Packet filtering.
- Network address translation.
- Connection tracking.
- Traffic control.
- Firewall rule management.

Modern Linux distributions commonly use nftables as the underlying packet-filtering framework.

---

## 8. Understanding SSH Security

SSH provides encrypted remote administration.

However, poor SSH configuration can create security risks.

### Common SSH Risks

- Weak passwords.
- Exposed private keys.
- Unauthorized login attempts.
- Outdated SSH software.
- Unnecessary remote access.

### Inspect SSH Service Status

```bash
systemctl status ssh
```

The service may be named differently on some distributions.

### Check SSH Listening Ports

```bash
sudo ss -tlnp | grep ssh
```

No result does not necessarily mean SSH is broken. It may simply not be running.

### Inspect SSH Configuration

```bash
sudo sshd -T
```

This displays effective SSH server configuration when the SSH server software is installed.

### Useful SSH Security Practices

1. Use strong authentication.
2. Protect private keys.
3. Prefer key-based authentication where appropriate.
4. Restrict access to authorized users.
5. Keep SSH updated.
6. Monitor authentication logs.
7. Disable SSH if it is unnecessary.

Do not change SSH settings on a system you cannot recover access to.

---

## 9. Understanding Encryption in Network Security

Encryption converts readable information into a protected form using cryptographic methods.

It helps prevent unauthorized parties from reading transmitted information.

### 9.1 HTTP vs HTTPS

HTTP does not provide transport encryption by itself.

HTTPS uses TLS to protect HTTP communication.

### 9.2 SSH Encryption

SSH encrypts remote administration sessions.

### 9.3 VPN Encryption

A Virtual Private Network (VPN) creates a protected communication tunnel between network endpoints.

VPNs can help protect traffic when using untrusted networks, depending on their configuration.

### 9.4 TLS

Transport Layer Security (TLS) protects communication between applications.

It supports:

- Confidentiality.
- Integrity.
- Authentication.

Encryption is not a substitute for secure system configuration. A vulnerable application can still be attacked even when its communication is encrypted.

---

## 10. DNS Security

DNS is important because systems depend on it to locate network services.

DNS security problems may include:

- DNS spoofing.
- DNS cache poisoning.
- Malicious DNS configuration.
- Unauthorized DNS changes.
- DNS tunneling.

### Inspect DNS Configuration

```bash
cat /etc/resolv.conf
```

### Inspect Name Resolution

```bash
getent hosts example.com
```

### Query DNS Records

```bash
dig example.com
```

### Security Recommendations

- Use trusted resolvers.
- Protect DNS configuration files.
- Monitor unexpected DNS changes.
- Use DNSSEC where appropriate.
- Investigate unusual DNS traffic.
- Validate TLS certificates when accessing websites.

---

## 11. Linux Network Security Inspection Commands

The following commands are useful for defensive inspection.

### Inspect Interfaces

```bash
ip -br addr
```

### Inspect Routing

```bash
ip route
```

### Inspect Listening Services

```bash
sudo ss -tulnp
```

### Inspect Active Connections

```bash
ss -tun
```

### Inspect Firewall Status

```bash
sudo ufw status verbose
```

### Inspect nftables Rules

```bash
sudo nft list ruleset
```

### Inspect SSH Service

```bash
systemctl status ssh
```

### Inspect DNS Configuration

```bash
cat /etc/resolv.conf
```

### Inspect ARP/Neighbor Information

```bash
ip neigh
```

These commands help you understand the security state of your Linux network environment.

---

## 12. Network Security Best Practices

### 12.1 Minimize Exposed Services

Only run services that are needed.

### 12.2 Use Strong Authentication

Use strong passwords and secure authentication methods.

### 12.3 Apply Security Updates

Keep your operating system and network software updated.

### 12.4 Use Firewalls

Restrict unnecessary incoming and outgoing connections.

### 12.5 Monitor Network Activity

Investigate unexpected connections and traffic patterns.

### 12.6 Secure Remote Access

Use SSH securely and restrict remote administration.

### 12.7 Protect DNS Settings

Prevent unauthorized modifications to DNS configuration.

### 12.8 Use Encryption

Prefer secure protocols for sensitive communication.

### 12.9 Apply Least Privilege

Give users and services only the permissions they need.

### 12.10 Maintain Logs

Logs help identify suspicious events and investigate incidents.

---

## 13. Practical Lab: Basic Linux Network Security Audit

### Lab Objective

Perform a basic defensive network security inspection of your own Kali VM.

This is an inspection-only exercise. Do not change firewall, SSH, routing, or network settings.

### Step 1: Identify Network Interfaces

```bash
ip -br addr
```

Record the active interfaces.

### Step 2: Inspect Listening Services

```bash
sudo ss -tulnp
```

Record:

- Listening TCP ports.
- Listening UDP ports.
- Associated processes.

### Step 3: Inspect Established Connections

```bash
ss -t state established
```

Observe any established TCP connections.

### Step 4: Inspect Firewall Status

```bash
sudo ufw status verbose
```

Record whether UFW is active, inactive, or unavailable.

### Step 5: Inspect nftables Rules

```bash
sudo nft list ruleset
```

Record whether rules are present.

### Step 6: Inspect SSH Status

```bash
systemctl status ssh
```

Record whether the SSH service is active, inactive, or unavailable.

### Step 7: Inspect SSH Configuration

```bash
sudo sshd -T
```

If the SSH server is not installed, record that fact rather than installing or enabling it for this exercise.

### Step 8: Inspect DNS Configuration

```bash
cat /etc/resolv.conf
```

Record the configured DNS information.

### Step 9: Inspect Neighbor Information

```bash
ip neigh
```

Observe any devices your VM has learned about on its local network.

### Step 10: Inspect Routing Information

```bash
ip route
```

Record the default route and connected networks.

---

## 14. Network Security Audit Report

Complete this table:

| Security Item | Observation |
|---|---|
| Active network interface | |
| Listening TCP ports | |
| Listening UDP ports | |
| Established connections | |
| UFW status | |
| nftables rules | |
| SSH service status | |
| SSH configuration available | |
| DNS configuration | |
| Neighbor entries | |
| Default gateway | |

### Analysis Questions

1. Which services are listening on your Kali VM?
2. Are all listening services necessary?
3. Is UFW active?
4. Are nftables rules present?
5. Is SSH running?
6. Which DNS resolver is configured?
7. What information does `ip neigh` reveal?
8. Which security improvements would you investigate first?

Do not label a system vulnerable simply because a firewall is inactive or a port is listening. Assess the complete network context.

---

## 15. Assessment Questions

1. What is Linux networking security?
2. What is an attack surface?
3. What is port scanning?
4. Explain a brute-force attack.
5. What is a Denial-of-Service attack?
6. What is a Man-in-the-Middle attack?
7. What is DNS spoofing?
8. What is ARP spoofing?
9. What is a firewall?
10. What does UFW stand for?
11. What is nftables?
12. Why is SSH security important?
13. What is the purpose of TLS?
14. Why should unnecessary services be disabled?
15. How does network monitoring improve security?

---

## 16. Important Commands to Remember

```bash
# Network interfaces
ip -br addr

# Routing table
ip route

# Listening services
sudo ss -tulnp

# Active connections
ss -tun

# Firewall status
sudo ufw status verbose

# nftables rules
sudo nft list ruleset

# SSH status
systemctl status ssh

# SSH effective configuration
sudo sshd -T

# DNS configuration
cat /etc/resolv.conf

# Neighbor information
ip neigh
```

---

## 17. Lesson Summary

In this lesson, you learned:

- Linux networking security protects systems and communications.
- The network attack surface includes exposed interfaces, ports, and services.
- Port scanning helps identify available services.
- Brute-force attacks target authentication.
- DoS and DDoS attacks disrupt availability.
- MITM attacks target communication between parties.
- DNS spoofing manipulates name-resolution information.
- ARP spoofing targets local IPv4 address resolution.
- Firewalls control network traffic.
- UFW provides simplified firewall administration.
- nftables is a Linux packet-filtering framework.
- SSH provides encrypted remote access.
- TLS protects network communication.
- Defensive monitoring helps identify suspicious activity.
- Least privilege and minimal service exposure improve security.

### Key Definitions

**Network Security:** Protecting network systems and communication from unauthorized access and attacks.

**Attack Surface:** All the points where a system may be exposed to threats.

**Firewall:** A security mechanism that filters network traffic according to rules.

**UFW:** Uncomplicated Firewall.

**nftables:** Linux packet-filtering and firewall framework.

**SSH:** Secure Shell.

**TLS:** Transport Layer Security.

**MITM:** Man-in-the-Middle attack.

**DoS:** Denial-of-Service attack.

**DDoS:** Distributed Denial-of-Service attack.

**ARP Spoofing:** Sending forged ARP messages to manipulate local network address resolution.

**DNS Spoofing:** Providing false DNS information to redirect network communication.

---

## 18. Knowledge Check

1. What is an attack surface?
2. Why should unnecessary network services be disabled?
3. What does a firewall do?
4. What is the difference between UFW and nftables?
5. Why is SSH considered safer than an unencrypted remote terminal protocol?
6. What is the purpose of TLS?
7. How does DNS spoofing affect network users?
8. What is ARP spoofing?
9. Which command displays listening network services?
10. Which command displays firewall status?
11. Why is monitoring established connections useful?
12. Explain five practices that improve Linux network security.

**End of Lesson 14 – Linux Networking Security**