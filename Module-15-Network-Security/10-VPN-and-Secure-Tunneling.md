# MODULE 15 – NETWORK SECURITY
## LESSON 10: VPN AND SECURE TUNNELING

---

## 1. Introduction

When information travels across the internet, it may pass through networks that the sender does not control.

This creates security risks, especially when users access sensitive information over public Wi-Fi or communicate with private organizational networks.

A Virtual Private Network (VPN) helps protect communication by creating a secure connection between endpoints over an existing network.

In this lesson, you will learn:

- What VPNs are.
- How VPN tunnels work.
- Different VPN types.
- Common VPN protocols.
- VPN authentication and encryption.
- VPN security risks.
- How to inspect VPN-related configuration in Kali Linux.

---

## 2. What Is a VPN?

VPN stands for Virtual Private Network.

A VPN creates a protected logical connection across an existing network, such as the internet.

It allows users or networks to communicate through an encrypted tunnel.

### Example

Imagine an employee working from home who needs to access a company's internal server.

Instead of exposing the internal server directly to the public internet, the company can provide an authorized VPN connection.

The employee connects to the VPN gateway and accesses permitted internal resources.

### Basic VPN diagram

```text
Remote Employee
      |
      |
      v
Internet
      |
      | Encrypted VPN Tunnel
      |
      v
Company VPN Gateway
      |
      |
      v
Internal Company Network
```

### Why VPNs are used

- Secure remote access.
- Site-to-site communication.
- Protection over untrusted networks.
- Access to authorized private resources.
- Secure communication between offices.

---

## 3. What Is a VPN Tunnel?

A VPN tunnel is a logical communication path that carries traffic between VPN endpoints.

The tunnel protects the traffic according to the VPN protocol and its configuration.

### How tunneling works

1. A VPN client connects to a VPN server or gateway.
2. The endpoints authenticate each other.
3. They establish cryptographic parameters and session keys.
4. Traffic is encapsulated and protected.
5. The protected packets travel across the underlying network.
6. The receiving endpoint processes and forwards the traffic.

### Important terms

| Term | Meaning |
|---|---|
| VPN Client | Software or device initiating a VPN connection |
| VPN Server | Endpoint accepting VPN connections |
| VPN Gateway | Device connecting VPN users or networks to another network |
| Tunnel | Logical protected communication path |
| Encapsulation | Wrapping one packet inside another |
| Encryption | Transforming information to protect confidentiality |

---

## 4. Types of VPNs

### 4.1 Remote Access VPN

Remote access VPN connects an individual device to a private network.

Example:

An employee connects from home to an office network.

```text
Employee Laptop
      |
      v
Internet
      |
      v
VPN Gateway
      |
      v
Company Network
```

### 4.2 Site-to-Site VPN

Site-to-site VPN connects two or more networks.

Example:

A company connects its Kano office to its Abuja office.

```text
Kano Office Network
        |
        v
   VPN Gateway
        |
        | Protected Tunnel
        |
   VPN Gateway
        |
        v
Abuja Office Network
```

### 4.3 Client-to-Site VPN

This is another commonly used name for remote access VPN, where a client connects to an organization's network.

### 4.4 Host-to-Host VPN

A host-to-host VPN establishes protected communication directly between two individual systems.

---

## 5. Common VPN Protocols

Different VPN technologies use different security mechanisms.

### 5.1 IPsec

IPsec stands for Internet Protocol Security.

It protects IP communication and is commonly used for site-to-site VPNs.

Important components:

- AH – Authentication Header.
- ESP – Encapsulating Security Payload.
- IKE – Internet Key Exchange.

ESP is widely used because it supports encryption and can provide integrity and authentication.

### 5.2 OpenVPN

OpenVPN is an open-source VPN solution.

It uses TLS-based cryptographic mechanisms for authentication and secure communication.

Common configurations use:

- UDP.
- TCP.

Its default port is often UDP 1194, but administrators can configure other ports.

### 5.3 WireGuard

WireGuard is a modern VPN protocol designed around a relatively small codebase and modern cryptographic primitives.

It is used for:

- Remote access.
- Site-to-site networking.
- Secure communication between servers.

Its commonly used default UDP port is 51820, although it can be configured differently.

### 5.4 L2TP/IPsec

L2TP/IPsec combines:

- Layer 2 Tunneling Protocol.
- IPsec security.

L2TP provides tunneling, while IPsec provides security protections.

### Comparison

| VPN technology | Main characteristics |
|---|---|
| IPsec | Network-layer security |
| OpenVPN | TLS-based VPN solution |
| WireGuard | Modern, relatively lightweight VPN protocol |
| L2TP/IPsec | Tunneling combined with IPsec protection |

---

## 6. Encryption in VPNs

Encryption is one of the main security properties of a VPN.

It transforms readable information into ciphertext.

### Example

Original information:

```text
username=student
```

Encrypted information may look like:

```text
8f2a91c7d4e6...
```

Without the required cryptographic key, the protected information should not be readable.

### Common cryptographic concepts

#### Symmetric Encryption

Uses the same secret key for encryption and decryption.

Example:

- AES.

#### Asymmetric Cryptography

Uses a public and private key pair.

It can support authentication and key establishment.

Examples:

- RSA.
- Elliptic-curve cryptography.

#### Hash Functions

Produce fixed-length digests from input data.

Examples:

- SHA-256.
- SHA-384.

Hashing is not encryption because it is not designed to be reversed using a decryption key.

---

## 7. VPN Authentication

Before a VPN connection is established, the endpoints normally need to authenticate.

Common methods include:

### Username and Password

Users authenticate using account credentials.

### Digital Certificates

Certificates can authenticate devices or users.

### Pre-Shared Keys

A shared secret may be configured on both VPN endpoints.

### Multi-Factor Authentication

MFA requires additional authentication beyond a single password.

### Why authentication matters

Without proper authentication, unauthorized users may gain access to protected network resources.

---

## 8. VPN Security Risks

VPNs improve communication security, but they are not a complete security solution.

### 8.1 Weak Authentication

Weak passwords or poorly protected keys may allow unauthorized VPN access.

### 8.2 Misconfiguration

Incorrect routing, access policies, or encryption settings may expose sensitive resources.

### 8.3 Vulnerable VPN Software

Unpatched VPN gateways may contain security vulnerabilities.

### 8.4 Compromised Endpoints

If a user's device is infected or compromised, the attacker may exploit the VPN connection.

### 8.5 Excessive Access

Giving every VPN user unrestricted access increases organizational risk.

### 8.6 DNS Leaks

DNS queries may use an unexpected resolver rather than the intended VPN DNS service.

### 8.7 Traffic Leaks

Some traffic may bypass the VPN tunnel if routing and firewall controls are incorrectly configured.

---

## 9. VPN Security Best Practices

Organizations should:

1. Use modern VPN protocols.
2. Enable strong authentication.
3. Use MFA where supported.
4. Keep VPN software updated.
5. Protect private keys and certificates.
6. Apply least-privilege access policies.
7. Monitor VPN login activity.
8. Disable unused VPN accounts.
9. Review VPN configuration regularly.
10. Use endpoint security controls.
11. Restrict access to sensitive internal networks.
12. Investigate unusual login attempts.

### Zero Trust and VPNs

Zero Trust means access should not be automatically trusted simply because a user is connected to a private network.

Users and devices should be verified, and access should be limited to authorized resources.

A VPN can be part of a Zero Trust architecture, but VPN connectivity alone does not implement Zero Trust.

---

## 10. Split Tunneling vs Full Tunneling

These are two common VPN traffic-routing configurations.

### Full Tunneling

All relevant device traffic is routed through the VPN.

```text
Device
  |
  v
VPN Tunnel
  |
  v
VPN Gateway
  |
  v
Destination
```

Advantages:

- Centralized traffic inspection.
- Consistent security policies.
- Reduced direct exposure to untrusted networks.

Disadvantages:

- May increase VPN bandwidth requirements.
- Can add latency.
- Places more load on the VPN gateway.

### Split Tunneling

Only selected traffic uses the VPN.

Other traffic travels through the normal network connection.

```text
                 Device
                   |
          +--------+--------+
          |                 |
          v                 v
    VPN Tunnel        Normal Network
          |                 |
          v                 v
   Private Network      Internet
```

Advantages:

- Reduces VPN bandwidth usage.
- Can improve performance.

Disadvantages:

- More complicated security management.
- Some traffic may bypass organizational inspection.

---

## 11. VPN vs Proxy

A VPN and a proxy are not the same.

| VPN | Proxy |
|---|---|
| Can protect traffic at the network or IP layer | Usually handles selected application traffic |
| Can create an encrypted tunnel | Encryption depends on proxy type and connection |
| Can route multiple applications | May only handle one application |
| Commonly used for private network access | Commonly used for intermediary requests |

An HTTPS proxy can encrypt the connection between a client and proxy, but a proxy does not automatically provide the same network-level protection as a VPN.

---

## 12. Important VPN Ports

| VPN technology | Common ports |
|---|---|
| IPsec IKE | UDP 500 |
| IPsec NAT Traversal | UDP 4500 |
| IPsec ESP | IP protocol 50 |
| OpenVPN | UDP 1194 commonly |
| WireGuard | UDP 51820 by default |
| L2TP | UDP 1701 |

Ports and protocol configurations can vary.

Remember that ESP is an IP protocol, not a TCP or UDP port.

---

# 13. PRACTICAL LAB 1: INSPECT VPN-RELATED NETWORK CONFIGURATION

## Objective

Inspect the network interfaces and routing configuration of your own Kali VM.

This is an inspection-only exercise. Do not change VPN or routing settings.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson10
cd ~/module15-lab/lesson10
```

### Step 2: Inspect network interfaces

```bash
ip addr
```

Look for interfaces such as:

- `eth0`
- `ens33`
- `tun0`
- `wg0`

A `tun` interface may be used by some VPN configurations.

A `wg` interface may be used by WireGuard.

Their presence does not automatically prove that a VPN connection is active.

### Step 3: Inspect routing information

```bash
ip route
```

### Step 4: Inspect IPv6 routes

```bash
ip -6 route
```

### Step 5: Inspect listening services

```bash
sudo ss -tulpen
```

### Step 6: Check VPN tools if installed

```bash
command -v openvpn
command -v wg
command -v ipsec
```

If a command is missing, record it as unavailable. Do not install anything just for this exercise.

### Step 7: Save your results

```bash
ip addr > interfaces.txt
ip route > ipv4-routes.txt
ip -6 route > ipv6-routes.txt
sudo ss -tulpen > listening-services.txt
```

---

# 14. PRACTICAL LAB 2: UNDERSTAND A VPN ROUTING TABLE

## Objective

Learn how to identify default routes and potential tunnel interfaces.

### Step 1: Display the IPv4 routing table

```bash
ip route
```

Example:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link
```

Explanation:

| Component | Meaning |
|---|---|
| `default` | Default route |
| `via 192.168.1.1` | Gateway |
| `dev eth0` | Interface used |
| `192.168.1.0/24` | Local network |

### Step 2: Check which route would be used for a destination

```bash
ip route get 1.1.1.1
```

This displays the route selected by the operating system for that destination.

### Step 3: Inspect available tunnel interfaces

```bash
ip -br link
```

Look for interfaces whose names suggest tunnel functionality.

### Step 4: Record your observations

Create:

```bash
nano vpn-routing-analysis.txt
```

Use this template:

```text
VPN ROUTING ANALYSIS

1. Network Interfaces:

2. Default IPv4 Route:

3. Default IPv6 Route:

4. Route to 1.1.1.1:

5. Tunnel Interfaces:

6. VPN Tools Available:

7. Observations:

8. Conclusion:
```

Do not conclude that a system is using a VPN merely because a tunnel interface exists.

---

# 15. PRACTICAL PROJECT: VPN SECURITY AUDIT

Create:

```bash
nano vpn-security-audit.txt
```

Use this template:

```text
VPN SECURITY AUDIT

1. ENVIRONMENT
Operating System:
Date:

2. VPN CONFIGURATION
VPN Technology:
VPN Client:
VPN Gateway:
Tunnel Interface:

3. AUTHENTICATION
Strong Authentication:
MFA:
Certificate Validation:
Private Key Protection:

4. ROUTING
Default Route:
Tunnel Route:
Split Tunneling:
DNS Configuration:

5. SECURITY RISKS
Weak Credentials:
Outdated Software:
Excessive Access:
DNS Leaks:
Traffic Leaks:

6. RECOMMENDATIONS
Use Modern Protocols:
Enable MFA:
Apply Least Privilege:
Update VPN Software:
Monitor Login Activity:
Protect Cryptographic Keys:

7. CONCLUSION
```

If your Kali VM does not use a VPN, mark VPN-specific fields as `Not Configured` or `Not Applicable`. Do not invent results.

---

## 16. Scenario-Based Exercise

Analyze the following situations.

For each one, identify the VPN type, possible security risk, and appropriate control.

### Scenario A

An employee working from home needs secure access to the company's internal Linux server.

### Scenario B

A company wants to connect its Kano office network with its Abuja office network.

### Scenario C

An organization allows all remote VPN users unrestricted access to every internal server.

### Scenario D

An employee connects to a VPN but some internet traffic bypasses the tunnel.

### Scenario E

A company discovers that its VPN gateway has not received security updates for several months.

---

## 17. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What does VPN stand for?
2. What is a VPN tunnel?
3. Explain how a VPN works.
4. Differentiate between remote access and site-to-site VPNs.
5. What is IPsec?
6. What is OpenVPN?
7. What is WireGuard?
8. What is encryption?
9. Differentiate between symmetric and asymmetric cryptography.
10. What is VPN authentication?
11. Why is MFA important for VPN access?
12. What is split tunneling?
13. What is full tunneling?
14. Differentiate between a VPN and a proxy.
15. What is a DNS leak?
16. Why should VPN software be updated?
17. What is the principle of least privilege?
18. Which UDP ports are commonly used by IPsec IKE?
19. What does `ip route` display?
20. Why is a VPN not a complete cybersecurity solution?

---

## 18. LESSON SUMMARY

In this lesson, you learned:

- What VPNs are.
- How VPN tunnels work.
- Remote access VPNs.
- Site-to-site VPNs.
- Host-to-host VPNs.
- IPsec, OpenVPN, WireGuard, and L2TP/IPsec.
- Encryption and authentication.
- VPN security risks.
- Split tunneling and full tunneling.
- VPN versus proxy.
- Common VPN ports.
- How to inspect interfaces and routes in Kali Linux.
- How to prepare a VPN security audit.

**KEY TAKEAWAY:**

VPNs provide protected communication across untrusted networks, but their security depends on strong authentication, correct routing, secure configuration, updated software, and appropriate access controls.

**END OF LESSON 10**