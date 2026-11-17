---
---

# MODULE 15 – NETWORK SECURITY
## LESSON 06: ARP SPOOFING AND MAN-IN-THE-MIDDLE ATTACKS

---

## 1. Introduction

ARP spoofing and Man-in-the-Middle (MITM) attacks are important network security topics.

They involve weaknesses in network communication that can allow an attacker to intercept, redirect, or manipulate traffic.

Understanding these attacks helps cybersecurity professionals protect local networks, detect suspicious communication, and secure sensitive information.

In this lesson, we will focus on:

- How ARP works.
- ARP spoofing.
- MITM attacks.
- How attackers may exploit insecure communication.
- How to detect suspicious ARP activity.
- How to defend against traffic interception.

All practical exercises will be defensive and limited to your own Kali Linux VM.

---

## 2. Understanding ARP

ARP stands for Address Resolution Protocol.

It is used in IPv4 local networks to discover the MAC address associated with an IPv4 address on the local network.

### Why ARP is necessary

Computers use IP addresses to identify network destinations.

However, when communicating over an Ethernet network, devices also need MAC addresses for local frame delivery.

ARP helps connect these two addressing systems.

### Example

Imagine two devices:

**Computer A**

```text
IP Address: 192.168.1.10
MAC Address: AA:AA:AA:AA:AA:AA
```

**Computer B**

```text
IP Address: 192.168.1.20
MAC Address: BB:BB:BB:BB:BB:BB
```

Computer A wants to communicate with Computer B.

Computer A knows B's IP address but needs its MAC address.

It sends an ARP request asking:

"Who has 192.168.1.20?"

Computer B responds with its MAC address.

Computer A can now send Ethernet frames to Computer B.

---

## 3. How ARP Works

ARP communication commonly follows these steps:

### Step 1: ARP Request

A device broadcasts a request asking which device owns a particular IPv4 address.

### Step 2: ARP Reply

The device with the matching address responds with its MAC address.

### Step 3: ARP Cache Update

The requesting device stores the IP-to-MAC mapping temporarily in its neighbor cache.

### Step 4: Communication

The device uses the discovered MAC address for local network communication.

### ARP communication diagram

```text
Computer A
    |
    | ARP Request:
    | Who has 192.168.1.20?
    |
    v
Local Network
    |
    v
Computer B
    |
    | ARP Reply:
    | 192.168.1.20 is at BB:BB:BB:BB:BB:BB
    |
    v
Computer A
```

---

## 4. Understanding the ARP Cache

An ARP cache stores recently learned IPv4-to-MAC address mappings.

In Linux, the neighbor table can be inspected using:

```bash
ip neigh
```

Example output:

```text
192.168.1.1 dev eth0 lladdr AA:BB:CC:DD:EE:FF REACHABLE
```

Explanation:

| Field | Meaning |
|---|---|
| 192.168.1.1 | Neighbor IPv4 address |
| eth0 | Network interface |
| lladdr | Link-layer address |
| AA:BB:CC:DD:EE:FF | MAC address |
| REACHABLE | Neighbor is considered reachable |

### Common neighbor states

| State | Meaning |
|---|---|
| REACHABLE | Recently confirmed reachable |
| STALE | Mapping exists but has not recently been confirmed |
| DELAY | Waiting before probing a neighbor |
| PROBE | Actively checking neighbor reachability |
| INCOMPLETE | Address resolution is still in progress |
| FAILED | Resolution failed |

The neighbor table is useful when investigating local network communication.

---

## 5. What Is ARP Spoofing?

ARP spoofing is an attack in which false ARP information is introduced into a local network.

An attacker attempts to associate their MAC address with an IP address that belongs to another device.

### Example

A normal network has:

```text
Gateway IP: 192.168.1.1
Gateway MAC: AA:AA:AA:AA:AA:AA
```

An attacker attempts to make another device believe:

```text
Gateway IP: 192.168.1.1
Gateway MAC: CC:CC:CC:CC:CC:CC
```

The incorrect mapping may redirect traffic through the attacker's device.

### Why ARP spoofing is possible

Traditional ARP does not provide strong built-in authentication of ARP messages.

A device may accept an incorrect mapping if its network implementation and configuration permit it.

### Potential consequences

- Traffic interception.
- Communication disruption.
- Exposure of unencrypted information.
- Opportunities for further attacks.

---

## 6. Understanding a Man-in-the-Middle Attack

A Man-in-the-Middle attack occurs when an attacker places themselves between two communicating parties.

The attacker may attempt to observe or manipulate communication.

### Normal communication

```text
User
  |
  | Communication
  |
  v
Server
```

### MITM communication

```text
User
  |
  | Communication
  |
  v
Attacker
  |
  | Forwarded or manipulated communication
  |
  v
Server
```

The user and server may believe they are communicating directly even though an attacker is interfering.

### Possible objectives

- Collect sensitive information.
- Steal authentication tokens.
- Modify transmitted information.
- Redirect traffic.
- Impersonate a communication endpoint.

---

## 7. Common Types of MITM Attacks

### 7.1 ARP-Based MITM

An attacker manipulates local ARP mappings to redirect traffic.

### 7.2 Rogue Wi-Fi Access Point

An attacker creates an unauthorized wireless access point that imitates a legitimate network.

Users may connect to the wrong access point.

### 7.3 DNS-Based MITM

Manipulated DNS responses may redirect users to an attacker-controlled destination.

### 7.4 HTTPS Downgrade Attacks

An attacker attempts to weaken secure communication by interfering with protocol negotiation or redirecting users toward an insecure connection.

Modern HTTPS protections such as HSTS help reduce some downgrade risks.

### 7.5 Session Interception

An attacker attempts to obtain or reuse authentication session information.

Secure session management and encrypted communication help reduce this risk.

---

## 8. Risks of ARP Spoofing and MITM

These attacks can threaten several security principles.

| Security principle | Possible impact |
|---|---|
| Confidentiality | Sensitive information may be exposed |
| Integrity | Communication may be modified |
| Availability | Communication may be disrupted |

### Real-world example

Imagine a company employee accessing an internal application over an insecure network.

If communication is not adequately protected, an attacker who can intercept the traffic may be able to view or manipulate information.

Using TLS with proper certificate validation helps protect communication against this type of interception.

---

## 9. Detecting ARP Spoofing

Security professionals use several methods to identify suspicious ARP activity.

### 9.1 Inspect the Neighbor Table

```bash
ip neigh
```

Look for:
- Unexpected MAC address changes.
- Multiple IP addresses associated with an unexpected MAC.
- Unusual neighbor entries.

A single unusual entry is not proof of spoofing.

### 9.2 Monitor ARP Packets

A packet analyzer can help identify unusual ARP traffic.

Wireshark display filter:

```text
arp
```

This displays ARP packets captured by Wireshark.

### 9.3 Look for Duplicate IP Warnings

Operating systems and network tools may report duplicate IP address conflicts.

### 9.4 Compare Trusted Records

Compare observed gateway MAC addresses against records maintained by the network administrator.

### 9.5 Monitor Network Logs

Managed switches and network monitoring platforms may provide information about suspicious address changes.

---

## 10. Preventing ARP Spoofing

Organizations can use several controls.

### 10.1 Dynamic ARP Inspection (DAI)

DAI is a switch security feature that validates ARP packets using trusted IP-to-MAC bindings.

It can help block invalid ARP messages.

### 10.2 DHCP Snooping

DHCP snooping maintains information about DHCP-assigned IP and MAC address bindings.

This information can support Dynamic ARP Inspection.

### 10.3 Network Segmentation

Segmentation limits the scope of local network communication.

### 10.4 Static ARP Entries

Static mappings may be useful for selected critical devices.

However, they can create administration overhead and are not a complete solution for large networks.

### 10.5 Secure Switch Configuration

Configure switch security features and restrict unauthorized access to network ports.

---

## 11. Preventing Man-in-the-Middle Attacks

### Use HTTPS

HTTPS protects web communication using TLS.

### Validate Certificates

Never ignore certificate warnings without investigating them.

### Use Secure Wi-Fi

Use modern wireless encryption and trusted access points.

### Use VPNs Where Appropriate

VPNs create protected communication tunnels between endpoints and VPN gateways.

### Enable MFA

MFA adds protection when passwords or other credentials are compromised.

### Keep Systems Updated

Updates help address known software and protocol vulnerabilities.

### Use Secure Session Management

Applications should protect session tokens and prevent unauthorized reuse.

---

## 12. ARP Spoofing vs. MITM

These terms are related but not identical.

| ARP spoofing | MITM |
|---|---|
| A specific network manipulation technique | A broader attack category |
| Commonly affects local IPv4 networks | Can occur in several communication environments |
| Can redirect traffic | Intercepts or manipulates communication |
| Can be used to establish a MITM position | Can use ARP spoofing or other methods |

**Important:** Not every MITM attack uses ARP spoofing.

---

# 13. PRACTICAL LAB: INSPECT YOUR KALI VM ARP INFORMATION

## Lab objective

Inspect your own Kali VM's network neighbor table and understand how ARP information is represented.

This lab is passive. It does not send spoofed ARP messages or intercept another device's communication.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson06
cd ~/module15-lab/lesson06
```

### Step 2: Inspect network interfaces

```bash
ip addr
```

Identify your active network interface.

### Step 3: Inspect the routing table

```bash
ip route
```

Identify the default gateway, if one is configured.

### Step 4: Inspect the neighbor table

```bash
ip neigh
```

Record:
- Neighbor IP addresses.
- MAC addresses.
- Interface names.
- Neighbor states.

### Step 5: Display IPv4 neighbor information

```bash
ip -4 neigh show
```

### Step 6: Display IPv6 neighbor information

```bash
ip -6 neigh show
```

IPv6 uses Neighbor Discovery rather than ARP.

### Step 7: Repeat the inspection

```bash
ip neigh
```

Compare the results and note any changes.

### Step 8: Save the results

```bash
ip addr > interfaces.txt
ip route > routes.txt
ip neigh > neighbor-table.txt
ip -4 neigh show > ipv4-neighbors.txt
ip -6 neigh show > ipv6-neighbors.txt
```

List your files:

```bash
ls -lh
```

---

# 14. PRACTICAL LAB: PASSIVE ARP PACKET MONITORING

## Lab objective

Learn how to observe ARP packets on your own Kali VM interface.

Only capture traffic on an interface you own or are authorized to monitor.

### Step 1: Check whether tcpdump is available

```bash
command -v tcpdump
```

If it is unavailable, skip this exercise.

### Step 2: Identify the active interface

```bash
ip addr
```

Record the interface name.

### Step 3: Capture a small number of ARP packets

Replace `eth0` with your actual interface name:

```bash
sudo tcpdump -i eth0 -nn -c 10 arp
```

Explanation:

| Option | Meaning |
|---|---|
| `-i` | Select network interface |
| `-nn` | Avoid name and port resolution |
| `-c 10` | Stop after ten packets |
| `arp` | Capture ARP traffic |

If no ARP packets appear, that is not necessarily a problem. Your interface may simply have no ARP traffic during the capture window.

### Step 4: Save a packet capture

```bash
sudo tcpdump -i eth0 -nn -c 20 -w arp-capture.pcap arp
```

Replace `eth0` with your actual interface.

If the capture ends before reaching 20 packets, it may simply be because there was not enough ARP traffic.

### Step 5: Inspect the capture

If tcpdump is available:

```bash
tcpdump -nn -r arp-capture.pcap
```

### Step 6: Record observations

Create:

```bash
nano arp-analysis.txt
```

Use this template:

```text
ARP PACKET ANALYSIS

Interface:
Capture Date:

Number of Packets:
Observed Sender IPs:
Observed Sender MACs:
Observed Target IPs:

Unusual ARP Activity:
Not Observed / Requires Further Investigation

Conclusion:
```

Do not label normal ARP traffic as malicious without supporting evidence.

---

# 15. PRACTICAL PROJECT: MITM DEFENSE CHECKLIST

Create:

```bash
nano mitm-defense-checklist.txt
```

Enter:

```text
MITM DEFENSE CHECKLIST

1. HTTPS is enabled.
2. TLS certificates are valid.
3. Certificate warnings are investigated.
4. Secure wireless encryption is enabled.
5. Unauthorized access points are monitored.
6. Network segmentation is implemented.
7. MFA is enabled for important accounts.
8. Session cookies use secure settings.
9. Network devices receive security updates.
10. Suspicious ARP changes are investigated.

STATUS:
Record each item as:
Implemented
Not Implemented
Not Applicable
Requires Review
```

Save the file.

---

# 16. SCENARIO-BASED EXERCISE

Analyze the following situations.

For each one, identify:
1. The possible attack.
2. The affected network component.
3. The possible impact.
4. The appropriate defensive control.

### Scenario A

A user's computer suddenly associates the university gateway IP address with a different MAC address from the one recorded by the network administrator.

### Scenario B

An employee connects to a wireless network that looks like the company's official Wi-Fi but is actually unauthorized.

### Scenario C

A website presents an invalid TLS certificate, but the user continues to enter sensitive information.

### Scenario D

A security analyst notices repeated unexpected ARP messages on a local network.

Save your answers in:

```bash
nano mitm-scenarios.txt
```

---

# 17. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is ARP?
2. Why is ARP necessary in IPv4 local networks?
3. What is an ARP request?
4. What is an ARP reply?
5. What is an ARP cache?
6. Explain ARP spoofing.
7. What is a Man-in-the-Middle attack?
8. Mention four types of MITM attacks.
9. What are the possible impacts of ARP spoofing?
10. How can an administrator detect suspicious ARP behavior?
11. What is Dynamic ARP Inspection?
12. What is DHCP snooping?
13. How does HTTPS help protect against MITM attacks?
14. Differentiate between ARP spoofing and MITM.
15. What is the purpose of `ip neigh`?

---

# 18. LESSON SUMMARY

In this lesson, you learned:

- The purpose of Address Resolution Protocol.
- How ARP requests and replies work.
- How the ARP cache operates.
- ARP spoofing and its consequences.
- The meaning of Man-in-the-Middle attacks.
- Common MITM techniques.
- Methods of detecting suspicious ARP behavior.
- Dynamic ARP Inspection and DHCP snooping.
- HTTPS, TLS, VPN, and certificate validation.
- How to inspect your Kali VM's neighbor table.
- How to perform passive ARP packet monitoring.
- How to build a MITM defense checklist.

**KEY TAKEAWAY:**

ARP helps IPv4 devices communicate on local networks, but traditional ARP does not authenticate address mappings. Security professionals must understand this weakness and use appropriate monitoring, switch protections, encryption, and network segmentation to reduce the risk of traffic interception.

**END OF LESSON 06**