
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 01: NETWORKING FUNDAMENTALS

### 1. Lab Overview

Computer networks allow devices to communicate and exchange information.

Linux networking is especially important in system administration and cybersecurity because Linux servers commonly provide services such as:

- Web hosting.
- DNS resolution.
- File sharing.
- Database access.
- Remote administration.
- Network security monitoring.

As a cybersecurity professional, you must understand how devices communicate before you can effectively investigate network problems or analyze suspicious activity.

In this lab, you will learn the foundations of computer networking and use Linux commands to inspect your Kali VM's network configuration.

**Lab environment:** Kali Linux running inside VMware.

**Network safety:** Use your own VM and its existing network configuration. Do not scan or probe devices outside your authorized lab.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain what a computer network is.
2. Understand LAN, WAN, and the internet.
3. Explain the client-server model.
4. Understand IP and MAC addresses.
5. Identify common network protocols.
6. Explain the OSI and TCP/IP models.
7. Inspect Linux network information.
8. Understand the role of VMware NAT and Host-only networking.

### 3. What Is Computer Networking?

A computer network is a group of connected devices that communicate and share resources.

Examples of network devices include:

- Desktop computers.
- Laptops.
- Smartphones.
- Servers.
- Routers.
- Switches.
- Firewalls.
- Printers.

Consider a simple example:

You open a website using your laptop.

1. Your laptop connects to a network.
2. It obtains or uses an IP configuration.
3. It sends a request toward the website's server.
4. Network devices forward the request.
5. The server processes the request.
6. The server sends a response.
7. Your browser displays the webpage.

This is an example of network communication.

### 4. Types of Computer Networks

**A. LAN – Local Area Network**

A LAN connects devices within a limited geographical area.

Examples:

- A university computer laboratory.
- An office network.
- A home Wi-Fi network.
- Your isolated VMware lab.

**B. WAN – Wide Area Network**

A WAN connects networks across large geographical areas.

Examples:

- Networks connecting different company branches.
- Telecommunications networks.
- The internet infrastructure.

**C. MAN – Metropolitan Area Network**

A MAN connects networks across a city or metropolitan region.

Example:

A network connecting several university campuses within the same city.

**D. PAN – Personal Area Network**

A PAN connects personal devices over a short distance.

Example:

A smartphone connected to wireless headphones through Bluetooth.

### 5. Understanding the Client-Server Model

The client-server model describes communication between a client and a server.

**Client:** A device or application requesting a service.

**Server:** A device or application providing that service.

Example:

When you visit a website:

- Your browser is the client.
- The web server provides the webpage.
- HTTP or HTTPS is commonly used for communication.

Other examples:

| Client | Server |
|---|---|
| Web browser | Web server |
| Email application | Mail server |
| SSH client | SSH server |
| DNS resolver | DNS server |
| Database application | Database server |

### 6. Understanding IP Addresses

An IP address identifies a network interface using the Internet Protocol.

There are two major versions.

**A. IPv4**

IPv4 addresses contain 32 bits.

They are commonly written as four decimal numbers separated by dots.

Example:

```text
192.168.1.10
```

Each decimal section represents eight bits and ranges from 0 to 255.

**B. IPv6**

IPv6 addresses contain 128 bits.

They are represented using hexadecimal notation.

Example:

```text
2001:db8::10
```

IPv6 was developed to provide a much larger address space than IPv4.

Inspect your Kali IP configuration:

```bash
ip addr
```

Or:

```bash
ip -br addr
```

The output may show interfaces such as:

- `lo` – Loopback interface.
- `eth0` – A possible Ethernet interface.
- `ens33` – Another possible Ethernet interface name.

Your actual interface name may differ.

### 7. Understanding MAC Addresses

A MAC address is a link-layer address associated with a network interface.

Example:

```text
00:0c:29:ab:cd:ef
```

A MAC address is commonly represented using six hexadecimal pairs.

Inspect your network interfaces:

```bash
ip link
```

Inspect a particular interface:

```bash
ip link show eth0
```

Replace `eth0` with the interface name shown on your system.

**IP address vs MAC address**

| IP Address | MAC Address |
|---|---|
| Used for network-layer addressing | Used for link-layer addressing |
| IPv4 or IPv6 format | Usually hexadecimal |
| Can change depending on network configuration | Often assigned to the interface, but can be changed or randomized |
| Used for routing between networks | Used for local-link delivery |

A MAC address is not a reliable identity for a person or device across every network because it can be randomized or changed.

### 8. Understanding Common Network Protocols

A protocol is a set of rules that allows devices to communicate.

Important networking protocols include:

| Protocol | Purpose |
|---|---|
| IP | Provides network addressing and routing |
| TCP | Provides reliable, ordered transport |
| UDP | Provides connectionless transport |
| HTTP | Transfers web content |
| HTTPS | Protects HTTP using TLS |
| DNS | Resolves domain names |
| DHCP | Automatically provides IP configuration |
| SSH | Secure remote administration |
| ICMP | Supports network control and diagnostic messages |
| ARP | Resolves IPv4 addresses to link-layer addresses on local networks |

Common port numbers:

| Port | Protocol or service |
|---|---|
| 20/21 | FTP data/control |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 67/68 | DHCP |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3306 | MySQL |

Important: A port number identifies a transport-layer endpoint, not necessarily a particular application. Services can use non-standard ports.

### 9. Understanding the OSI Model

The OSI model divides network communication into seven conceptual layers.

| Layer | Name | Main Function |
|---|---|---|
| 7 | Application | Network services used by applications |
| 6 | Presentation | Data representation and transformation |
| 5 | Session | Manages communication sessions |
| 4 | Transport | End-to-end transport |
| 3 | Network | Logical addressing and routing |
| 2 | Data Link | Local-link delivery |
| 1 | Physical | Transmission of raw bits |

A simple way to remember the layers:

**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing.

From Layer 7 down to Layer 1.

Cybersecurity applications:

- Layer 1: Physical access and cable security.
- Layer 2: Switch security and MAC-based controls.
- Layer 3: IP routing and network segmentation.
- Layer 4: TCP/UDP port exposure.
- Layer 7: Web application security.

### 10. Understanding the TCP/IP Model

The TCP/IP model describes networking using four commonly taught layers.

| TCP/IP Layer | Related OSI Layers |
|---|---|
| Application | 5, 6, 7 |
| Transport | 4 |
| Internet | 3 |
| Network Access | 1, 2 |

Examples:

- Application: HTTP, DNS, SSH.
- Transport: TCP, UDP.
- Internet: IPv4, IPv6, ICMP.
- Network Access: Ethernet, Wi-Fi.

The TCP/IP model is closely associated with real-world internet communication.

### 11. Understanding VMware Networking

Your Kali Linux system runs inside VMware, so its networking is virtualized.

VMware provides different network modes.

**A. NAT Mode**

NAT allows your virtual machine to access external networks through the host's network connection.

Example:

```text
Kali VM
   |
VMware NAT
   |
Windows Host
   |
Router
   |
Internet
```

Typical uses:

- Updating Kali.
- Downloading approved packages.
- Accessing websites.
- General lab internet access.

**B. Host-only Mode**

Host-only networking creates a private network between the host and virtual machines attached to that network.

It is useful for isolated labs.

Example:

```text
Windows Host
     |
Host-only Network
     |
Kali VM
     |
Metasploitable VM
```

This is suitable for your future attack-and-defense exercises.

**C. Bridged Mode**

Bridged networking connects the VM to the physical network as another device.

It can expose the VM to the same network as other physical devices.

For this course, use NAT for normal updates and Host-only for isolated security labs. Avoid Bridged Mode unless a specific authorized exercise requires it.

### 12. Practical Exercise 1 – Identify Your Network Interfaces

Run:

```bash
ip link
```

Then:

```bash
ip -br link
```

Identify:

- Loopback interface.
- Main network interface.
- Interface state.
- MAC address.

The interface state may be `UP`, `DOWN`, or another reported state.

### 13. Practical Exercise 2 – Inspect IP Configuration

Run:

```bash
ip addr
```

Then:

```bash
ip -br addr
```

Identify:

- IPv4 address, if assigned.
- IPv6 address, if assigned.
- Network interface name.
- Loopback address.

Remember that not every interface needs an IPv4 address.

### 14. Practical Exercise 3 – Inspect Routing Information

Run:

```bash
ip route
```

Then:

```bash
ip -6 route
```

Look for:

- Default route.
- Gateway address.
- Network interface.
- Connected routes.

A default route is generally used when a destination does not match a more specific routing entry.

### 15. Practical Exercise 4 – Inspect Network Statistics

Run:

```bash
ip -s link
```

Inspect:

- Received packets.
- Transmitted packets.
- Received bytes.
- Transmitted bytes.
- Errors.
- Dropped packets.

These statistics help administrators identify possible network interface problems.

### 16. Practical Exercise 5 – Inspect DNS Configuration

Run:

```bash
cat /etc/resolv.conf
```

Inspect the configured name servers and other available DNS configuration.

Your system may use a local DNS stub resolver, so the displayed address may not be the actual upstream DNS server.

### 17. Practical Exercise 6 – Inspect Your VMware Network Mode

Inside VMware:

1. Shut down Kali if you need to change its virtual network adapter settings.
2. Open the Kali virtual machine settings.
3. Select Network Adapter.
4. Observe whether NAT, Host-only, or Bridged is selected.
5. Do not change the setting unless necessary.
6. Start Kali again if you shut it down.

Record the current network mode.

For this lab, no network mode change is required.

### 18. Basic Network Troubleshooting Concepts

Network problems can occur at different layers.

| Problem | Possible Cause |
|---|---|
| No network interface | Virtual adapter or driver issue |
| No IP address | DHCP or configuration issue |
| Cannot reach gateway | Local network issue |
| Can reach IP but not domain | Possible DNS issue |
| Connection refused | Service not accepting connections |
| Connection timed out | Destination or network not responding |
| High packet loss | Network congestion or connectivity issue |

A good administrator investigates problems systematically instead of randomly changing network settings.

### 19. Lab Documentation

Create a report:

```bash
nano ~/practical-labs/module05/lab01/lab01-report.txt
```

If the directory does not exist:

```bash
mkdir -p ~/practical-labs/module05/lab01
```

Record:

```text
MODULE 05 – LAB 01
NETWORKING FUNDAMENTALS

1. Linux distribution:
2. Main network interface:
3. MAC address:
4. IPv4 address:
5. IPv6 address:
6. Default gateway:
7. DNS configuration:
8. VMware network mode:
9. Difference between TCP and UDP:
10. Difference between IP and MAC addresses:
11. What is the OSI model?
12. What is the purpose of DNS?
13. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 20. Knowledge Check

1. What is a computer network?
2. What is the difference between LAN and WAN?
3. Explain the client-server model.
4. What is an IPv4 address?
5. What is an IPv6 address?
6. What is a MAC address?
7. What is the difference between TCP and UDP?
8. What is the purpose of DNS?
9. Name the seven OSI layers.
10. What is the difference between VMware NAT and Host-only networking?

### 21. Lab Completion Checklist

- [ ] Understood computer networking fundamentals.
- [ ] Identified network types.
- [ ] Studied the client-server model.
- [ ] Understood IPv4 and IPv6.
- [ ] Inspected MAC addresses.
- [ ] Studied common network protocols.
- [ ] Reviewed the OSI model.
- [ ] Reviewed the TCP/IP model.
- [ ] Inspected Linux network interfaces.
- [ ] Inspected IP configuration.
- [ ] Inspected routing information.
- [ ] Inspected network statistics.
- [ ] Examined DNS configuration.
- [ ] Identified VMware network mode.
- [ ] Completed lab documentation.

**LAB 01 STATUS: READY FOR PRACTICAL COMPLETION**
