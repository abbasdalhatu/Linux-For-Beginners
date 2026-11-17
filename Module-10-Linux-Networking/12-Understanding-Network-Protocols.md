---
---

# Module 10 – Lesson 12: Understanding Network Protocols

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a network protocol is.
2. Understand the major network protocols.
3. Differentiate TCP and UDP.
4. Understand HTTP and HTTPS.
5. Explain DNS, DHCP, SSH, FTP, and ICMP.
6. Understand how protocols work together.
7. Identify common protocol ports.
8. Understand network protocols from a cybersecurity perspective.

---

## 2. What Is a Network Protocol?

A network protocol is a set of rules that determines how devices communicate and exchange information over a network.

Think of a protocol as a language.

For two people to communicate successfully, they need a language they both understand.

Similarly, computers need protocols to exchange information correctly.

Examples include:

- TCP
- UDP
- IP
- HTTP
- HTTPS
- DNS
- DHCP
- SSH
- ICMP

Without network protocols, computers would not have standardized rules for communication.

---

## 3. Why Are Network Protocols Important?

Network protocols provide rules for:

- Sending and receiving data.
- Identifying devices.
- Establishing communication.
- Detecting transmission errors.
- Routing information.
- Protecting transmitted information.
- Managing network connections.

For example, when you open a website:

1. DNS helps identify the website's IP address.
2. IP helps deliver packets to the destination.
3. TCP may establish a reliable connection.
4. TLS helps secure the connection.
5. HTTP transfers website information.

These protocols work together to make communication possible.

---

## 4. Understanding the TCP/IP Model

The TCP/IP model describes how network communication works.

It has four main layers.

| Layer | Main Responsibility | Examples |
|---|---|---|
| Application | Provides network services to applications | HTTP, DNS, SSH |
| Transport | Manages communication between applications | TCP, UDP |
| Internet | Handles IP addressing and routing | IPv4, IPv6, ICMP |
| Network Access | Handles communication over the local network | Ethernet, Wi-Fi |

### 4.1 Application Layer

This layer provides services that applications use to communicate.

Examples:

- HTTP
- HTTPS
- DNS
- SSH
- SMTP
- FTP

### 4.2 Transport Layer

This layer manages communication between applications.

Its two major protocols are:

- TCP
- UDP

### 4.3 Internet Layer

This layer handles logical addressing and packet routing.

Examples:

- IPv4
- IPv6
- ICMP

### 4.4 Network Access Layer

This layer handles the movement of data across the local network.

Examples:

- Ethernet
- Wi-Fi

---

## 5. Understanding TCP

TCP stands for Transmission Control Protocol.

TCP is a connection-oriented transport protocol that provides reliable, ordered data delivery.

### Main Features

- Connection-oriented communication.
- Reliable data delivery.
- Ordered data transmission.
- Error detection.
- Retransmission of lost data.
- Flow control.

### TCP Three-Way Handshake

TCP establishes a connection using a process called the three-way handshake.

The three steps are:

1. SYN
2. SYN-ACK
3. ACK

Diagram:

```text
Client                         Server
  |                               |
  |------------ SYN ------------>|
  |                               |
  |<---------- SYN-ACK -----------|
  |                               |
  |------------ ACK ------------>|
  |                               |
  |       Connection Established  |
```

### Explanation

**SYN:** The client requests to establish a connection.

**SYN-ACK:** The server acknowledges the request and responds.

**ACK:** The client confirms the response.

The connection is now established.

### Common Uses of TCP

- Website communication.
- Email transmission.
- File transfers.
- SSH administration.
- Database connections.

---

## 6. Understanding UDP

UDP stands for User Datagram Protocol.

UDP is a connectionless transport protocol.

Unlike TCP, UDP does not establish a connection through a three-way handshake.

### Main Features

- Connectionless communication.
- Low protocol overhead.
- No built-in retransmission guarantee.
- No guaranteed packet ordering.
- Suitable for time-sensitive communication.

### Common Uses

- DNS queries.
- Voice over IP.
- Online gaming.
- Live video and audio streaming.
- DHCP.

UDP is useful when reducing delay is important.

### TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Communication | Connection-oriented | Connectionless |
| Reliability | Reliable delivery mechanisms | No delivery guarantee |
| Ordering | Preserves byte-stream order | No ordering guarantee |
| Handshake | Three-way handshake | No TCP-style handshake |
| Overhead | Generally higher | Generally lower |
| Common use | Web, SSH, file transfer | DNS, gaming, voice |

Remember: UDP does not mean data can never arrive reliably. It means UDP itself does not provide TCP-style delivery guarantees.

---

## 7. Understanding IP

IP stands for Internet Protocol.

IP is responsible for addressing and routing packets between networks.

Two major versions exist:

### IPv4

IPv4 uses 32-bit addresses.

Example:

```text
192.168.1.10
```

### IPv6

IPv6 uses 128-bit addresses.

Example:

```text
2001:db8::10
```

### Important IP Responsibilities

- Logical addressing.
- Packet delivery across networks.
- Routing.
- Identifying source and destination addresses.

IP itself does not guarantee reliable delivery.

TCP or an application-level mechanism may provide reliability.

---

## 8. Understanding HTTP and HTTPS

### HTTP

HTTP stands for Hypertext Transfer Protocol.

It is used for communication between web clients and web servers.

Example:

```text
http://example.com
```

Common default port:

```text
80
```

HTTP does not provide transport encryption by itself.

### HTTPS

HTTPS stands for Hypertext Transfer Protocol Secure.

It uses HTTP over TLS to protect communication.

Example:

```text
https://example.com
```

Common default port:

```text
443
```

### Benefits of HTTPS

- Encrypts communication.
- Helps protect data from interception.
- Authenticates the server through certificates.
- Helps protect data integrity during transmission.

HTTPS does not guarantee that a website itself is trustworthy. A malicious website can also use HTTPS.

---

## 9. Understanding DNS

DNS stands for Domain Name System.

DNS translates domain names into IP addresses and supports other domain-related records.

Example:

```text
example.com
```

May resolve to an IPv4 address such as:

```text
93.184.216.34
```

Common DNS port:

```text
53
```

DNS commonly uses UDP for ordinary queries, but TCP is also used in certain situations.

### Useful Commands

```bash
getent hosts example.com
```

```bash
dig example.com
```

```bash
nslookup example.com
```

DNS is important because users generally remember domain names more easily than IP addresses.

---

## 10. Understanding DHCP

DHCP stands for Dynamic Host Configuration Protocol.

It automatically provides network configuration to clients.

DHCP can provide:

- IP address.
- Subnet information.
- Default gateway.
- DNS servers.
- Lease duration.

Common DHCP ports:

| Protocol | Port |
|---|---|
| UDP Server | 67 |
| UDP Client | 68 |

### DHCP DORA

1. Discover
2. Offer
3. Request
4. Acknowledge

DHCP reduces the amount of manual network configuration required.

---

## 11. Understanding SSH

SSH stands for Secure Shell.

It provides encrypted remote access to systems.

Common port:

```text
22
```

Example:

```bash
ssh username@192.168.1.10
```

This attempts to connect to the remote machine using SSH.

### Common SSH Uses

- Remote server administration.
- Secure command execution.
- Secure file transfer.
- Remote troubleshooting.

### Security Recommendations

- Use strong authentication.
- Protect private keys.
- Disable unnecessary remote access.
- Restrict access to trusted users.
- Keep SSH software updated.

Only connect to systems you own or are authorized to administer.

---

## 12. Understanding FTP and SFTP

### FTP

FTP stands for File Transfer Protocol.

It is used to transfer files between computers.

Traditional FTP commonly uses:

- TCP port 21 for control.
- TCP port 20 for active-mode data connections.

Traditional FTP does not encrypt credentials or data by default.

### SFTP

SFTP stands for SSH File Transfer Protocol.

It transfers files over SSH.

Its common port is:

```text
22
```

SFTP is different from FTPS, which uses FTP with TLS.

### Comparison

| Feature | FTP | SFTP |
|---|---|---|
| Security | Not encrypted by default | Protected through SSH |
| Common control port | TCP 21 | TCP 22 |
| Authentication | Depends on configuration | SSH authentication |
| Encryption | No default encryption | Encrypted connection |

---

## 13. Understanding ICMP

ICMP stands for Internet Control Message Protocol.

It is used for network diagnostics and error reporting.

Common uses include:

- Echo requests.
- Echo replies.
- Destination unreachable messages.
- Time exceeded messages.

The `ping` command commonly uses ICMP Echo Request and Echo Reply messages.

Example:

```bash
ping -c 4 1.1.1.1
```

ICMP does not use TCP or UDP ports.

### Traceroute

Traceroute helps identify network hops between a source and destination.

Example:

```bash
traceroute example.com
```

Depending on the implementation and options, traceroute may use UDP, ICMP, or TCP probes.

---

## 14. Common Network Protocol Ports

| Protocol / Service | Common Port | Transport |
|---|---:|---|
| FTP control | 21 | TCP |
| SSH | 22 | TCP |
| SMTP | 25 | TCP |
| DNS | 53 | UDP/TCP |
| DHCP server | 67 | UDP |
| DHCP client | 68 | UDP |
| HTTP | 80 | TCP |
| POP3 | 110 | TCP |
| NTP | 123 | UDP |
| IMAP | 143 | TCP |
| HTTPS | 443 | TCP |
| SMB | 445 | TCP |
| SMTP submission | 587 | TCP |
| IMAPS | 993 | TCP |
| POP3S | 995 | TCP |

These are common default ports. Actual deployments can use different ports.

---

## 15. How Network Protocols Work Together

Consider opening a secure website.

Example:

```text
https://example.com
```

A simplified communication process:

1. Your browser requests the IP address of the domain using DNS.
2. DNS returns an IP address.
3. Your device uses IP routing to reach the destination.
4. TCP establishes a connection when HTTP/1.1 or HTTP/2 is used over TCP.
5. TLS establishes secure communication.
6. HTTP sends the website request.
7. The server returns website content.
8. Your browser displays the page.

Important: HTTP/3 uses QUIC over UDP instead of TCP. QUIC includes transport security through TLS 1.3 integration.

---

## 16. Practical Lab: Investigating Network Protocols

### Lab Objective

Use Kali Linux to inspect common network protocols and identify how they operate.

This lab uses read-only inspection and ordinary network requests.

### Step 1: Inspect Listening TCP and UDP Sockets

```bash
sudo ss -tuln
```

Understand:

- `-t` displays TCP sockets.
- `-u` displays UDP sockets.
- `-l` displays listening sockets.
- `-n` displays numeric addresses and ports.

### Step 2: Inspect Active Network Connections

```bash
ss -tun
```

### Step 3: Investigate DNS

```bash
dig example.com
```

Observe:

- DNS server.
- Query type.
- Answer section.
- Returned IP address.
- Query time.

### Step 4: Investigate Name Resolution

```bash
getent hosts example.com
```

Compare the result with the DNS query.

### Step 5: Test ICMP

```bash
ping -c 4 1.1.1.1
```

Observe packet loss and response time.

### Step 6: Investigate HTTPS

```bash
curl -I https://example.com
```

Observe the HTTP response status and headers.

### Step 7: Investigate HTTP

```bash
curl -I http://example.com
```

Compare the response with HTTPS. The server may redirect HTTP to HTTPS.

### Step 8: Inspect Your Routing Information

```bash
ip route
```

Identify your default route.

### Step 9: Inspect Network Interfaces

```bash
ip -br addr
```

Identify your active network interface and its IP address.

### Step 10: Prepare Your Findings

Record:

- TCP sockets found.
- UDP sockets found.
- DNS query result.
- ICMP test result.
- HTTPS response status.
- Default gateway.
- Active network interface.

---

## 17. Cybersecurity: Network Protocol Risks

Understanding protocols is important in cybersecurity because attackers may exploit weak or misconfigured network services.

### Common Risks

**1. Unencrypted HTTP**

Sensitive information may be exposed when transmitted without encryption.

**2. Insecure FTP**

Traditional FTP can expose credentials and file contents.

**3. Weak SSH Authentication**

Weak passwords or exposed private keys can increase unauthorized access risk.

**4. DNS Attacks**

DNS may be targeted through spoofing, cache poisoning, or malicious resolver manipulation.

**5. Exposed Network Services**

Unnecessary listening services increase the system's attack surface.

**6. Insecure Protocol Configuration**

Outdated or weak protocol configurations can expose systems to attacks.

### Defensive Practices

- Prefer HTTPS over HTTP.
- Use SFTP instead of traditional FTP when appropriate.
- Use strong SSH authentication.
- Monitor DNS configuration.
- Disable unnecessary services.
- Restrict network access using firewalls.
- Keep network software updated.
- Monitor listening ports.

---

## 18. Important Commands to Remember

```bash
# Inspect network sockets
ss -tuln

# Inspect active connections
ss -tun

# DNS lookup
dig example.com

# Resolve hostname
getent hosts example.com

# ICMP connectivity test
ping -c 4 1.1.1.1

# Inspect HTTPS response
curl -I https://example.com

# Inspect routing table
ip route

# Inspect network interfaces
ip -br addr

# Display listening TCP and UDP sockets
sudo ss -tulnp
```

---

## 19. Assessment Questions

1. What is a network protocol?
2. Why are network protocols important?
3. Name the four layers of the TCP/IP model.
4. What is the difference between TCP and UDP?
5. Explain the TCP three-way handshake.
6. What is the main purpose of IP?
7. What is the difference between HTTP and HTTPS?
8. What does DNS do?
9. What is DHCP used for?
10. What is SSH, and which port does it commonly use?
11. What is the difference between FTP and SFTP?
12. What is ICMP used for?
13. Which command can display listening TCP and UDP sockets?
14. Why is an unencrypted protocol dangerous?
15. How can network protocol knowledge help a cybersecurity professional?

---

## 20. Lesson Summary

In this lesson, you learned:

- Network protocols define communication rules.
- TCP/IP is a major networking model.
- TCP provides reliable, ordered delivery.
- UDP provides connectionless communication.
- IP handles addressing and routing.
- HTTP and HTTPS support web communication.
- DNS resolves domain names.
- DHCP provides automatic network configuration.
- SSH provides secure remote access.
- FTP and SFTP support file transfer.
- ICMP supports diagnostics and error reporting.
- Ports identify services associated with network communication.
- Protocol security is essential for protecting systems.

### Key Definitions

**Network Protocol:** A set of rules governing communication between devices.

**TCP:** Transmission Control Protocol.

**UDP:** User Datagram Protocol.

**IP:** Internet Protocol.

**HTTP:** Hypertext Transfer Protocol.

**HTTPS:** HTTP protected using TLS.

**DNS:** Domain Name System.

**DHCP:** Dynamic Host Configuration Protocol.

**SSH:** Secure Shell.

**ICMP:** Internet Control Message Protocol.

**SFTP:** SSH File Transfer Protocol.

**Port:** A logical communication endpoint used by network services.

---

## 21. Knowledge Check

1. Which protocol provides reliable, ordered delivery?
2. Which protocol is connectionless: TCP or UDP?
3. Which port is commonly used by HTTPS?
4. Which protocol translates domain names into IP addresses?
5. Which protocol provides secure remote terminal access?
6. Which command can test ICMP connectivity?
7. Which protocol is commonly used by DHCP?
8. Why is SFTP generally safer than traditional FTP?
9. Does ICMP use TCP or UDP ports?
10. How do DNS, IP, TCP, TLS, and HTTP work together when opening a secure website?

**End of Lesson 12 – Understanding Network Protocols**