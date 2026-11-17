---
---

# Module 10 – Lesson 01: Introduction to Linux Networking

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define computer networking.
2. Explain how Linux participates in a network.
3. Understand the basic components of networking.
4. Differentiate between LAN, WAN, and the Internet.
5. Understand client-server communication.
6. Identify common Linux networking tools.
7. Explain why networking is important in cybersecurity.

---

## 2. What Is Computer Networking?

Computer networking is the process of connecting two or more computing devices so they can communicate and share resources.

These devices may include:

- Desktop computers.
- Laptops.
- Smartphones.
- Servers.
- Printers.
- Routers.
- Network switches.

### Simple Example

Imagine you have two computers:

- Computer A
- Computer B

If both computers are connected to the same network, they may communicate and exchange files.

This is computer networking.

### Real-World Examples

- Sending messages through WhatsApp.
- Opening a website using Chrome.
- Sharing files between computers.
- Connecting to university Wi-Fi.
- Accessing a remote Linux server.
- Watching videos on YouTube.

All these activities involve networking.

---

## 3. How Does Linux Participate in Networking?

Linux is widely used in networking because it provides powerful tools for communication, configuration, monitoring, and troubleshooting.

Linux can function as:

### A. Client

A client is a device or application that requests a service.

Example:

Your Kali Linux machine requests a webpage from a web server.

### B. Server

A server provides services to other devices.

Examples include:

- Web servers.
- File servers.
- DNS servers.
- Email servers.
- Database servers.

### C. Router

A Linux machine can be configured to forward network traffic between different networks.

### D. Security Monitoring System

Linux can run security tools that monitor network activity and detect suspicious communication.

Examples:

- Wireshark.
- tcpdump.
- Snort.
- Suricata.

---

## 4. Basic Components of a Computer Network

A network contains several important components.

### 4.1 Network Interface Card (NIC)

A Network Interface Card allows a computer to connect to a network.

It may support:

- Ethernet connections.
- Wireless connections.

Linux identifies network interfaces using names such as:

```bash
eth0
ens33
enp0s3
wlan0
```

The exact name depends on the system.

### 4.2 IP Address

An IP address identifies a network interface using an address assigned to it.

Example IPv4 address:

```text
192.168.1.10
```

IP addresses allow devices to communicate across networks.

We will study IP addressing in detail in Lesson 03.

### 4.3 MAC Address

A MAC address is a hardware-level network identifier associated with a network interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

MAC addresses are commonly represented using six pairs of hexadecimal characters.

### 4.4 Router

A router connects different IP networks and forwards packets between them.

Example:

Your home router connects your local network to your Internet Service Provider.

### 4.5 Switch

A network switch connects devices within a local network.

It uses MAC addresses to forward Ethernet frames to the appropriate devices.

### 4.6 Firewall

A firewall controls network traffic according to security rules.

It can allow or block traffic based on criteria such as:

- Source IP address.
- Destination IP address.
- Protocol.
- Port number.
- Connection state.

---

## 5. Types of Computer Networks

### 5.1 LAN – Local Area Network

A LAN connects devices within a limited geographical area.

Examples:

- A university computer laboratory.
- An office network.
- A home Wi-Fi network.

### 5.2 WAN – Wide Area Network

A WAN connects networks across large geographical distances.

Examples:

- Networks connecting different company branches.
- Telecommunications networks.
- The Internet.

### 5.3 MAN – Metropolitan Area Network

A MAN connects multiple networks across a city or metropolitan area.

Example:

A network connecting several university campuses within one city.

### 5.4 PAN – Personal Area Network

A PAN connects personal devices over a short distance.

Example:

A smartphone connected to wireless earbuds using Bluetooth.

---

## 6. Understanding Client-Server Communication

Client-server communication is a networking model in which a client requests a service and a server provides it.

### Example: Opening a Website

When you open a website:

1. Your browser acts as a client.
2. Your computer identifies the website's IP address using DNS.
3. Your computer establishes a network connection to the server.
4. Your browser sends an HTTP or HTTPS request.
5. The server processes the request.
6. The server sends a response.
7. Your browser displays the webpage.

### Simple Diagram

```text
+------------------+
| Client Computer  |
|                  |
| Web Browser      |
+--------+---------+
         |
         | Network Request
         |
         v
+------------------+
| Network Router   |
+--------+---------+
         |
         | Internet
         |
         v
+------------------+
| Web Server       |
|                  |
| Website Content  |
+------------------+
```

---

## 7. Introduction to Network Protocols

A network protocol is a set of rules that controls how devices communicate.

Common networking protocols include:

| Protocol | Purpose |
|---|---|
| IP | Addressing and packet routing |
| TCP | Reliable transport communication |
| UDP | Connectionless transport communication |
| HTTP | Web communication |
| HTTPS | Encrypted web communication |
| DNS | Domain name resolution |
| DHCP | Automatic IP configuration |
| SSH | Secure remote administration |
| ICMP | Network diagnostic and error messages |

We will explore these protocols further in later lessons.

---

## 8. Introduction to Linux Networking Commands

Linux provides commands for examining and troubleshooting network connections.

Here are some important commands:

| Command | Purpose |
|---|---|
| `ip addr` | Displays network interfaces and IP addresses |
| `ip link` | Displays network interface information |
| `ip route` | Displays routing information |
| `ping` | Tests network reachability |
| `ss` | Displays socket information |
| `ip neigh` | Displays neighboring network devices |
| `hostname` | Displays or manages the system hostname |
| `getent hosts` | Checks name resolution |
| `curl` | Makes requests to network services |
| `traceroute` | Helps examine network paths |

Do not worry about memorizing everything now. We will practise these commands throughout Module 10.

---

## 9. Why Linux Networking Matters in Cybersecurity

Networking is one of the most important foundations of cybersecurity.

A cybersecurity professional needs to understand how devices communicate before investigating network threats.

### Practical Cybersecurity Applications

**1. Network Reconnaissance**

Understanding network addressing and connectivity helps security professionals map authorized systems.

**2. Traffic Analysis**

Tools such as Wireshark and tcpdump help inspect network traffic.

**3. Firewall Administration**

Networking knowledge helps security professionals create traffic filtering rules.

**4. Intrusion Detection**

Systems such as Snort and Suricata inspect network traffic for suspicious activity.

**5. Incident Response**

Network logs and connections can help investigators understand how an incident occurred.

**6. Server Security**

Linux networking knowledge helps administrators secure services and restrict unnecessary network exposure.

---

## 10. Practical Lab: Exploring Your Kali Linux Network

### Lab Objective

Explore the network configuration of your Kali Linux virtual machine.

We will only inspect network information. We will not change network settings.

### Step 1: Open Kali Terminal

Start your Kali Linux virtual machine in VMware.

Open the terminal.

### Step 2: Display Your Hostname

Run:

```bash
hostname
```

This displays your Linux machine's hostname.

### Step 3: Display Network Interfaces

Run:

```bash
ip link
```

Observe the network interfaces available on your machine.

### Step 4: Display IP Addresses

Run:

```bash
ip addr
```

Look for an IPv4 address associated with your active network interface.

An address might look like:

```text
192.168.100.25
```

Your actual address may be different.

### Step 5: Display Routing Information

Run:

```bash
ip route
```

Look for a line beginning with:

```text
default via
```

This normally identifies your default gateway.

### Step 6: Check Network Connectivity

Run:

```bash
ping -c 4 1.1.1.1
```

This sends four ICMP echo requests to Cloudflare's public DNS address.

If you receive replies, basic IP connectivity is working.

If you receive errors, your VM may have a network configuration or connectivity issue.

### Step 7: Test DNS Resolution

Run:

```bash
getent hosts example.com
```

This checks whether your system can resolve the domain name.

If an IP address is returned, name resolution is working.

### Step 8: Display Listening and Active Sockets

Run:

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

---

## 11. Practical Lab Questions

Write your answers in your notebook.

1. What is the hostname of your Kali Linux machine?
2. How many network interfaces are displayed by `ip link`?
3. Which interface is currently active?
4. What IPv4 address is assigned to your active interface?
5. What is your default gateway?
6. Did the ping test receive replies?
7. Did `getent hosts example.com` return an IP address?
8. What is the purpose of the `ss -tuln` command?

Do not share public IP addresses, private network details, or sensitive infrastructure information unnecessarily.

---

## 12. Important Commands to Remember

```bash
hostname
ip link
ip addr
ip route
ping -c 4 1.1.1.1
getent hosts example.com
ss -tuln
```

---

## 13. Lesson Summary

In this lesson, you learned:

- Computer networking connects devices for communication.
- Linux can operate as a client, server, router, and security monitoring system.
- Network interfaces provide connectivity.
- IP and MAC addresses help identify devices and interfaces.
- Routers connect different networks.
- Switches connect devices within local networks.
- LAN, WAN, MAN, and PAN describe different network scopes.
- Protocols define communication rules.
- Linux provides powerful networking commands.
- Networking knowledge is essential for cybersecurity.

### Key Definitions

**Computer Network:** A collection of connected devices that communicate and share resources.

**Network Interface:** A physical or virtual connection through which a device communicates over a network.

**IP Address:** A logical network address assigned to an interface.

**MAC Address:** A link-layer hardware identifier associated with a network interface.

**Router:** A device that forwards packets between IP networks.

**Protocol:** A set of rules governing network communication.

---

## 14. Knowledge Check

Answer these questions without looking at the lesson.

1. What is computer networking?
2. What is the difference between a router and a switch?
3. What is the difference between LAN and WAN?
4. What is the purpose of an IP address?
5. What does DNS do?
6. What is the difference between a client and a server?
7. Which Linux command displays IP addresses?
8. Why is networking important in cybersecurity?

**End of Lesson 01 – Introduction to Linux Networking**