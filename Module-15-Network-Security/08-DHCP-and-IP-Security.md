# MODULE 15 – NETWORK SECURITY
## LESSON 08: DHCP AND IP SECURITY

---

## 1. Introduction

Every device connected to an IP network needs network configuration to communicate.

This configuration commonly includes:

- IP address.
- Subnet mask or prefix length.
- Default gateway.
- DNS server.
- Lease duration (when assigned dynamically).

Two important technologies involved in network addressing are:

1. DHCP – Dynamic Host Configuration Protocol.
2. IP – Internet Protocol.

DHCP automatically provides network configuration, while IP enables devices to communicate across networks.

However, attackers may exploit weaknesses in DHCP services, IP configuration, and network addressing.

In this lesson, you will learn how these technologies work and how to secure them.

---

## 2. What Is DHCP?

DHCP stands for Dynamic Host Configuration Protocol.

It automatically assigns network configuration to devices.

Without DHCP, administrators may need to configure each device manually.

### Example

Imagine a university network with 500 computers.

Manually configuring every computer would take considerable time.

DHCP allows the network to automatically assign IP addresses and other configuration details.

### Information DHCP can provide

| Configuration | Purpose |
|---|---|
| IP address | Identifies a device on the network |
| Subnet mask | Defines the network portion of an IPv4 address |
| Default gateway | Provides a route to other networks |
| DNS server | Resolves domain names |
| Lease duration | Defines how long an address assignment remains valid |

---

## 3. How DHCP Works: DORA Process

DHCP commonly uses a four-step process called DORA.

DORA means:

- Discover
- Offer
- Request
- Acknowledge

### Step 1: Discover

The client broadcasts a DHCP Discover message to locate DHCP servers.

### Step 2: Offer

A DHCP server responds with an available IP address and configuration information.

### Step 3: Request

The client requests the offered address.

### Step 4: Acknowledge

The DHCP server confirms the assignment.

### DORA diagram

```text
DHCP Client                 DHCP Server
     |                            |
     |------ DISCOVER ------------>|
     |                            |
     |<--------- OFFER ------------|
     |                            |
     |------ REQUEST ------------->|
     |                            |
     |<--------- ACK ---------------|
     |                            |
     v                            v
 Network Configuration       Address Assigned
```

This process allows a device to obtain network configuration automatically.

---

## 4. DHCP Ports and Protocols

DHCP for IPv4 uses UDP.

| Device | UDP Port |
|---|---:|
| DHCP Client | 68 |
| DHCP Server | 67 |

A client commonly sends its initial DHCP Discover using source port 68 and destination port 67.

The server responds using source port 67 and destination port 68.

DHCP for IPv6 uses different ports:

| Purpose | UDP Port |
|---|---:|
| DHCPv6 Client | 546 |
| DHCPv6 Server | 547 |

---

## 5. DHCP Lease

A DHCP lease is a temporary assignment of an IP address to a client.

### Example

A DHCP server assigns:

```text
IP Address: 192.168.1.50
Lease Duration: 24 Hours
```

The device can use that address during the lease period.

Before the lease expires, the client may attempt to renew it.

### Why leases are useful

- Addresses can be reused.
- Network administration becomes easier.
- Temporary devices can connect without permanent manual configuration.
- Administrators can manage address allocation centrally.

---

## 6. Static IP vs Dynamic IP

| Static IP | Dynamic IP |
|---|---|
| Manually configured or reserved | Automatically assigned |
| Usually remains consistent | May change |
| Useful for selected infrastructure | Useful for ordinary client devices |
| Requires careful administration | Easier to manage at scale |

### Example static configuration

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
Gateway: 192.168.1.1
DNS: 1.1.1.1
```

These are example values. Do not apply them to your actual network without confirming its configuration.

---

## 7. What Is IP Security?

IP security involves protecting IP-based communication and network addressing against unauthorized access, manipulation, and disruption.

Security controls may protect:

- IP address allocation.
- Routing.
- Network traffic.
- Source and destination information.
- Communication integrity.
- Network availability.

IP security involves more than assigning an IP address correctly.

It also includes firewall policies, routing protections, network segmentation, monitoring, and secure protocols.

---

## 8. Common DHCP Attacks

### 8.1 Rogue DHCP Server

A rogue DHCP server is an unauthorized server that provides incorrect network configuration.

For example, it may supply:

- An incorrect default gateway.
- An unauthorized DNS server.
- An incorrect network configuration.

This can redirect traffic or disrupt communication.

### 8.2 DHCP Starvation

DHCP starvation is an attack that attempts to exhaust the available DHCP address pool.

If the pool becomes exhausted, legitimate devices may be unable to obtain IP addresses.

### 8.3 DHCP Spoofing

DHCP spoofing involves sending unauthorized DHCP responses that attempt to mislead clients.

A rogue DHCP server is a common way this can happen.

### 8.4 DHCP-Based Denial of Service

An attacker may disrupt DHCP availability by exhausting resources or interfering with the DHCP service.

---

## 9. Defending Against DHCP Attacks

### 9.1 DHCP Snooping

DHCP snooping is a switch security feature that distinguishes trusted and untrusted network ports.

It can help block unauthorized DHCP server messages arriving through untrusted ports.

### How it works

- Trusted ports may connect to authorized DHCP servers.
- Untrusted ports normally connect to client devices.
- Unauthorized DHCP server responses can be blocked.

### 9.2 Port Security

Switch port security helps control which devices can connect through a switch port.

### 9.3 Network Monitoring

Administrators should monitor:

- DHCP server activity.
- Unexpected DHCP offers.
- IP address pool utilization.
- Repeated address assignment failures.

### 9.4 Network Segmentation

Separating networks can reduce the scope of DHCP-related incidents.

### 9.5 Secure DHCP Configuration

Use appropriate address pools, reservations, access restrictions, and logging.

---

## 10. Understanding IP Address Spoofing

IP spoofing occurs when a sender forges the source IP address in an IP packet.

### Example

An attacker sends a packet that appears to originate from:

```text
Source IP: 192.168.1.100
```

However, the actual sender may be a different device.

### Why attackers use IP spoofing

- Hide the apparent source of traffic.
- Impersonate another source address.
- Support certain denial-of-service attacks.
- Attempt to bypass poorly designed filtering rules.

### Important limitation

A forged source IP does not automatically allow an attacker to receive replies sent to that address.

This is one reason IP spoofing is commonly associated with attacks where a response is unnecessary or where other weaknesses are involved.

---

## 11. IP Spoofing Detection and Prevention

### Ingress Filtering

Ingress filtering checks traffic entering a network and may block packets with source addresses that should not arrive through that interface.

### Egress Filtering

Egress filtering checks traffic leaving a network and can prevent internal systems from sending packets with unauthorized source addresses.

### Stateful Firewalls

Stateful firewalls track network connections and can enforce policies based on connection state.

### Network Monitoring

Security analysts can investigate unusual source addresses, traffic patterns, and routing behavior.

---

## 12. IPsec Fundamentals

IPsec means Internet Protocol Security.

It is a collection of protocols and mechanisms used to protect IP communication.

IPsec can provide:

- Authentication.
- Integrity.
- Confidentiality.
- Protection against replay attacks.

It is commonly used in VPN implementations.

### Main IPsec components

#### 12.1 Authentication Header (AH)

AH provides authentication and integrity protection.

It does not provide encryption for confidentiality.

#### 12.2 Encapsulating Security Payload (ESP)

ESP can provide confidentiality through encryption and can also provide integrity and authentication.

ESP is widely used in IPsec VPNs.

### IPsec modes

| Mode | Description |
|---|---|
| Transport mode | Primarily protects the IP packet payload |
| Tunnel mode | Encapsulates and protects an entire original IP packet |

### Example

A company may use an IPsec VPN to protect communication between two office networks across the internet.

---

## 13. DHCP vs DNS vs IP

These technologies work together but have different responsibilities.

| Technology | Main responsibility |
|---|---|
| DHCP | Assigns network configuration |
| DNS | Resolves domain names |
| IP | Enables addressing and packet delivery |

### Example

When a laptop joins a university network:

1. DHCP provides its IP configuration.
2. IP enables communication.
3. DNS resolves requested domain names.

Understanding these differences is important for troubleshooting network problems.

---

# 14. PRACTICAL LAB 1: INSPECT DHCP AND IP CONFIGURATION

## Objective

Inspect your own Kali VM's network configuration without changing its settings.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson08
cd ~/module15-lab/lesson08
```

### Step 2: Inspect network interfaces

```bash
ip addr
```

Record:

- Interface name.
- IPv4 address.
- IPv6 address, if present.
- Interface state.

### Step 3: Inspect routes

```bash
ip route
```

Identify the default gateway if one exists.

### Step 4: Inspect network configuration

```bash
ip -br addr
```

This gives a shorter interface summary.

### Step 5: Inspect DHCP-related processes

```bash
ps aux | grep -E 'dhclient|NetworkManager|systemd-networkd' | grep -v grep
```

This may help identify network management components.

The absence of a `dhclient` process does not prove DHCP is not being used. NetworkManager and other components may manage DHCP internally.

### Step 6: Inspect DNS settings

```bash
cat /etc/resolv.conf
```

If available:

```bash
resolvectl status
```

### Step 7: Save the results

```bash
ip addr > ip-addresses.txt
ip route > routing-table.txt
ip -br addr > interface-summary.txt
cat /etc/resolv.conf > dns-configuration.txt
```

---

# 15. PRACTICAL LAB 2: UNDERSTANDING DHCP TRAFFIC

## Objective

Learn how DHCP traffic can be identified through passive packet observation.

This exercise is restricted to your own Kali VM.

### Step 1: Check tcpdump

```bash
command -v tcpdump
```

### Step 2: Identify the network interface

```bash
ip addr
```

### Step 3: Capture DHCP traffic

Replace `eth0` with your actual interface:

```bash
sudo tcpdump -i eth0 -nn -c 10 'udp port 67 or udp port 68'
```

This filters traffic associated with DHCP for IPv4.

If there is no DHCP activity during the capture, you may see no packets. That is normal.

### Step 4: Save a capture if DHCP traffic is available

```bash
sudo tcpdump -i eth0 -nn -c 20 -w dhcp-capture.pcap 'udp port 67 or udp port 68'
```

Do not force a DHCP lease renewal merely to generate traffic. The purpose of this lab is passive inspection.

### Step 5: Inspect the capture

```bash
tcpdump -nn -r dhcp-capture.pcap
```

Record any visible DHCP message types and addresses.

---

# 16. PRACTICAL PROJECT: DHCP AND IP SECURITY REPORT

Create:

```bash
nano dhcp-ip-security-report.txt
```

Use this template:

```text
DHCP AND IP SECURITY REPORT

1. ENVIRONMENT
Operating System:
Network Interface:
Date:

2. IP CONFIGURATION
IPv4 Address:
IPv6 Address:
Default Gateway:
Interface State:

3. DHCP INVESTIGATION
DHCP Process or Manager:
DHCP Traffic Observed:
DHCP Server Information:

4. DNS CONFIGURATION
Configured Resolver:

5. SECURITY ANALYSIS
Unexpected Configuration:
Potential Rogue DHCP Indicators:
Potential IP Spoofing Indicators:
Addressing Problems:

6. DEFENSIVE RECOMMENDATIONS
Enable DHCP Snooping:
Use Switch Port Security:
Apply Ingress Filtering:
Apply Egress Filtering:
Monitor DHCP Activity:
Review Firewall Policies:

7. CONCLUSION
```

Save your report.

---

## 17. Scenario-Based Exercise

Analyze each situation.

For each one, identify the attack, possible impact, and appropriate defense.

### Scenario A

A laptop joins a university network and receives an unexpected gateway address from an unknown DHCP server.

### Scenario B

A network administrator discovers that the DHCP address pool has been exhausted and new devices cannot connect.

### Scenario C

A network monitoring system detects packets claiming to originate from IP addresses that should never be used on the external interface.

### Scenario D

A company wants to protect communication between its Kano and Abuja offices over the public internet.

---

## 18. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What does DHCP stand for?
2. What is the main purpose of DHCP?
3. Explain the DORA process.
4. Which UDP ports are used by DHCP for IPv4?
5. What is a DHCP lease?
6. Differentiate between static and dynamic IP addresses.
7. What is a rogue DHCP server?
8. Explain DHCP starvation.
9. What is DHCP snooping?
10. What is IP spoofing?
11. Why might an attacker forge a source IP address?
12. What is ingress filtering?
13. What is egress filtering?
14. What does IPsec stand for?
15. What is the difference between AH and ESP?
16. Differentiate between IPsec transport mode and tunnel mode.
17. Explain the relationship between DHCP, DNS, and IP.
18. Why is network segmentation important?
19. How can administrators detect unauthorized DHCP activity?
20. Mention five ways to improve DHCP and IP security.

---

## 19. LESSON SUMMARY

In this lesson, you learned:

- The purpose of DHCP.
- How the DORA process works.
- DHCP ports and leases.
- Static and dynamic IP addressing.
- The meaning of IP security.
- Rogue DHCP servers.
- DHCP starvation and spoofing.
- DHCP snooping and port security.
- IP address spoofing.
- Ingress and egress filtering.
- IPsec, AH, and ESP.
- How to inspect your Kali VM's network configuration.
- How to identify DHCP traffic.
- How to prepare a DHCP and IP security report.

**KEY TAKEAWAY:**

DHCP provides automatic network configuration, while IP enables network communication. Protecting both requires secure DHCP services, trusted network infrastructure, appropriate filtering, monitoring, and strong network security policies.

**END OF LESSON 08**