# Module 14 – Lesson 01: Firewall Fundamentals

## 1. Introduction to Firewalls

A **firewall** is a security system that monitors and controls incoming and outgoing network traffic according to predefined security rules.

Think of a firewall as a security guard at the entrance of a university.

- It checks who is entering.
- It checks who is leaving.
- It decides which traffic is allowed.
- It blocks traffic that violates its security rules.

In computing, a firewall protects devices, servers, and networks against unauthorised network access.

### Real-World Example

Imagine a university network with:

- Student computers.
- Administrative computers.
- A web server.
- A database server.

The university may configure its firewall to:

- Allow students to access the university website.
- Allow authorised administrators to access management systems.
- Prevent students from directly accessing the database server.
- Block unnecessary incoming connections.

This reduces the university's attack surface.

---

## 2. Why Are Firewalls Important?

Firewalls help organisations:

1. Prevent unauthorised network access.
2. Control incoming and outgoing connections.
3. Reduce exposure to network attacks.
4. Protect servers and internal networks.
5. Restrict access to sensitive services.
6. Monitor suspicious network activity.
7. Enforce organisational security policies.

### Cybersecurity Application

A cybersecurity analyst may investigate a server with several exposed network services.

The analyst can recommend firewall rules that allow only the services required for normal operations.

---

## 3. How a Firewall Works

A firewall examines network traffic and compares it against configured rules.

A simplified process:

1. A device sends or receives network traffic.
2. The traffic reaches the firewall.
3. The firewall examines relevant information.
4. It compares the traffic against its rules.
5. It allows, blocks, rejects, or logs the traffic according to policy.

### Example

Suppose a Linux server has these rules:

| Traffic | Firewall decision |
|---|---|
| HTTPS traffic on TCP port 443 | Allow |
| SSH traffic on TCP port 22 from an authorised IP | Allow |
| Unauthorised connection to port 3306 | Block |
| Unnecessary incoming connection | Block |

The actual decision depends on the complete firewall configuration and rule order.

---

## 4. Types of Firewalls

### A. Packet-Filtering Firewall

Examines individual packets using information such as:

- Source IP address.
- Destination IP address.
- Source port.
- Destination port.
- Network protocol.

Example:

A firewall allows TCP traffic to port 443 but blocks traffic to port 23.

### B. Stateful Firewall

Tracks the state of network connections.

For example, when a computer initiates an allowed connection to a web server, the firewall can recognise the returning traffic as belonging to that established connection.

Stateful firewalls provide more context than simple stateless packet filtering.

### C. Application-Layer Firewall

Examines application-level traffic.

It may inspect protocols such as:

- HTTP.
- HTTPS, where inspection is supported and configured.
- DNS.
- SMTP.

Application-layer controls can identify certain suspicious application behaviours that ordinary packet filtering may not detect.

### D. Next-Generation Firewall (NGFW)

A next-generation firewall combines traditional firewall capabilities with additional security features.

These may include:

- Deep packet inspection.
- Application awareness.
- Intrusion prevention.
- Threat intelligence.
- Advanced traffic analysis.

---

## 5. Hardware and Software Firewalls

### Hardware Firewall

A physical device designed to protect networks.

Examples include dedicated enterprise firewall appliances.

Common deployment locations:

- Between an organisation's internal network and the internet.
- Between separate internal network segments.
- At a data centre network boundary.

### Software Firewall

A firewall implemented through software on a computer or server.

Examples:

- UFW.
- iptables.
- nftables.
- Windows Defender Firewall.

Linux administrators frequently use software firewalls to protect individual servers.

---

## 6. Understanding Network Traffic Directions

Firewalls commonly control three traffic directions.

### A. Incoming Traffic (Inbound)

Traffic entering a device or network.

Example:

A remote computer attempts to connect to your Linux SSH server.

### B. Outgoing Traffic (Outbound)

Traffic leaving a device or network.

Example:

Your Kali Linux VM connects to a package repository to download updates.

### C. Forwarded Traffic

Traffic passing through a device that is routing packets between different networks.

Example:

A Linux machine acts as a router between two network segments.

### Summary

| Direction | Meaning |
|---|---|
| Inbound | Traffic entering |
| Outbound | Traffic leaving |
| Forwarded | Traffic passing through |

---

## 7. Understanding Ports and Protocols

A firewall often uses network ports and protocols to control connections.

A port identifies a service endpoint on a computer.

Common examples:

| Port | Protocol | Common service |
|---|---|---|
| 22 | TCP | SSH |
| 53 | UDP/TCP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 25 | TCP | SMTP |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |

Important: A port being open does not automatically mean a system is compromised. The security question is whether the service is necessary, properly configured, and appropriately restricted.

### Example

A web server may need:

- TCP port 80 for HTTP.
- TCP port 443 for HTTPS.

It may not need:

- TCP port 23 for Telnet.
- TCP port 3306 exposed to the public internet.

A firewall can restrict unnecessary exposure.

---

## 8. Firewall Policies

A firewall policy defines how traffic should be handled.

Three common actions are:

### ACCEPT

Allows matching traffic.

### DROP

Silently discards matching traffic.

### REJECT

Blocks matching traffic and sends a response indicating that the connection was refused.

### Default Policies

Two common approaches are:

**Default Allow:** Traffic is permitted unless a rule blocks it.

**Default Deny:** Traffic is blocked unless a rule explicitly permits it.

A default-deny approach is commonly preferred for inbound traffic because it follows the principle of least privilege.

However, policies must be designed carefully to avoid blocking legitimate services.

---

## 9. Linux Firewall Technologies

Linux provides several firewall technologies.

### UFW

Uncomplicated Firewall.

A simplified firewall management interface, commonly used on Ubuntu and other Debian-based systems.

### iptables

A traditional command-line interface for configuring Linux packet-filtering rules.

### nftables

A modern Linux packet-filtering framework that provides a replacement for much of the traditional iptables functionality.

### Netfilter

The Linux kernel framework that provides packet filtering, connection tracking, NAT, and related networking capabilities.

Relationship:

- Netfilter provides kernel-level networking functionality.
- iptables and nftables provide interfaces for managing firewall rules.
- UFW provides a simplified management interface.

We will study these technologies in detail in the coming lessons.

---

## 10. Firewall Security Best Practices

1. Use default-deny policies for inbound traffic where appropriate.
2. Allow only required services.
3. Restrict administrative access to trusted sources.
4. Review firewall rules regularly.
5. Remove obsolete rules.
6. Monitor firewall logs.
7. Protect both IPv4 and IPv6 traffic.
8. Document firewall changes.
9. Test rule changes carefully.
10. Maintain a recovery method before changing remote firewall configurations.

### Important Warning

Do not randomly change firewall rules on your main Windows computer or VMware network interfaces.

Our practical exercises will be performed carefully inside your Kali virtual machine.

---

## 11. Practical Lab: Inspecting Network Exposure

### Objective

Learn how to inspect your Kali Linux VM's network interfaces, IP configuration, listening ports, and existing firewall status without modifying firewall rules.

### Step 1: Display Network Interfaces

```bash
ip addr
```

This displays network interfaces and their IP addresses.

### Step 2: Display Routing Information

```bash
ip route
```

This displays the routing table.

### Step 3: Display Listening Ports

```bash
sudo ss -tuln
```

Explanation:

- `-t`: TCP sockets.
- `-u`: UDP sockets.
- `-l`: Listening sockets.
- `-n`: Numeric addresses and ports.

### Step 4: Inspect UFW Status

```bash
sudo ufw status verbose
```

This displays the current UFW firewall status, if UFW is installed.

### Step 5: Inspect nftables Rules

```bash
sudo nft list ruleset
```

This displays the currently configured nftables ruleset.

An empty output may indicate that no nftables rules are currently configured.

### Step 6: Inspect iptables Rules

```bash
sudo iptables -L -n -v
```

This displays IPv4 firewall chains and rules.

These inspection commands do not intentionally change firewall configuration.

### Step 7: Create a Lab Report

```bash
mkdir -p ~/module14-lab/lesson01
cd ~/module14-lab/lesson01
nano firewall-inspection-report.txt
```

Record:

```text
MODULE 14 – LESSON 01
FIREWALL FUNDAMENTALS

1. Network Interfaces:

2. IP Configuration:

3. Routing Information:

4. Listening Ports:

5. UFW Status:

6. nftables Rules:

7. iptables Rules:

8. Security Observations:

9. Recommendations:

10. Conclusion:
```

Save the file.

---

## 12. Knowledge Check

Answer these questions to test your understanding.

1. What is a firewall?
2. Explain four reasons firewalls are important.
3. Differentiate between packet-filtering and stateful firewalls.
4. What is the difference between hardware and software firewalls?
5. Explain inbound, outbound, and forwarded traffic.
6. What is the difference between DROP and REJECT?
7. Why is default-deny commonly used for inbound traffic?
8. What is the relationship between Netfilter, iptables, and nftables?
9. Why should unnecessary network ports be restricted?
10. Which command displays listening TCP and UDP sockets?

### Practical Challenge

Inspect your Kali VM and identify:

- Its active network interface.
- Its IP address.
- Its default gateway.
- Its listening ports.
- Whether UFW is active.
- Whether nftables has configured rules.

Write your observations in your lab report.

---

## 13. Lesson Summary

In this lesson, you learned:

- What firewalls are.
- Why firewalls are important.
- How firewalls process network traffic.
- Types of firewalls.
- Hardware versus software firewalls.
- Inbound, outbound, and forwarded traffic.
- Network ports and protocols.
- Firewall actions and default policies.
- Linux firewall technologies.
- Basic firewall inspection commands.

**Next Lesson:** Module 14 – Lesson 02: Linux Firewall Architecture.
