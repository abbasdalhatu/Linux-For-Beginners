---
---

# Module 10 – Lesson 09: Understanding Ports and Sockets

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand network ports.
2. Explain the difference between IP addresses and ports.
3. Understand TCP and UDP.
4. Identify common network ports and their services.
5. Understand sockets and socket connections.
6. Inspect listening ports in Linux.
7. Identify active network connections.
8. Understand the security risks of exposed ports.

---

## 2. What Is a Network Port?

A network port is a logical communication endpoint used by applications and services.

When multiple applications communicate over a network, ports help the operating system deliver incoming traffic to the correct application.

For example:

- A web server may use port 80.
- A secure web server may use port 443.
- An SSH server commonly uses port 22.
- A DNS service commonly uses port 53.

### Simple Analogy

Imagine a university building.

- The IP address represents the building's street address.
- The port number represents a particular office inside the building.
- The application or service represents the department operating in that office.

The IP address helps identify the destination device, while the port identifies the relevant application endpoint.

### Example

```text
192.168.1.10:22
```

This represents:

- IP address: `192.168.1.10`
- Port: `22`

The destination is a service endpoint on that device, commonly SSH.

---

## 3. Understanding Port Numbers

Port numbers are 16-bit values.

Their range is:

```text
0 – 65535
```

The port-number ranges are divided into three categories.

| Range | Category | Description |
|---|---|---|
| 0–1023 | Well-known ports | Common standard services |
| 1024–49151 | Registered ports | Registered application services |
| 49152–65535 | Dynamic/private ports | Commonly used for temporary client-side connections |

These ranges are defined by IANA.

### Example

```text
Port 80 = HTTP
Port 443 = HTTPS
Port 22 = SSH
```

A client may use a temporary high-numbered source port when connecting to a server.

---

## 4. Understanding TCP

TCP stands for Transmission Control Protocol.

It is a connection-oriented transport-layer protocol.

TCP provides:

- Reliable delivery.
- Ordered data transmission.
- Error detection.
- Retransmission of lost data.
- Flow control.
- Congestion control.

### How TCP Works

TCP commonly establishes a connection using a three-way handshake.

The three stages are:

1. SYN
2. SYN-ACK
3. ACK

### TCP Three-Way Handshake

```text
Client                         Server
  |                               |
  |----------- SYN -------------->|
  |                               |
  |<--------- SYN-ACK ------------|
  |                               |
  |----------- ACK -------------->|
  |                               |
  |        Connection Established |
```

### Explanation

**Step 1 – SYN**

The client requests to establish a TCP connection.

**Step 2 – SYN-ACK**

The server acknowledges the request and responds.

**Step 3 – ACK**

The client acknowledges the server's response.

The TCP connection is established.

---

## 5. Understanding UDP

UDP stands for User Datagram Protocol.

It is a connectionless transport-layer protocol.

Unlike TCP, UDP does not establish a connection using the TCP three-way handshake.

UDP does not guarantee delivery, ordering, or retransmission at the transport layer.

It is useful when low overhead and timely delivery are important.

Common examples include:

- DNS queries.
- Voice communication.
- Video streaming.
- Online gaming.
- Real-time applications.

### TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Delivery guarantee | Reliable transport | No delivery guarantee |
| Ordering | Preserves byte-stream order | No ordering guarantee |
| Retransmission | Supported | Not provided by UDP itself |
| Overhead | Generally higher | Generally lower |
| Examples | HTTP/HTTPS over TCP, SSH | DNS, voice and streaming applications |

Important: DNS commonly uses UDP, but DNS can also use TCP.

---

## 6. Common Network Ports

Cybersecurity professionals must understand common ports because they help identify network services.

| Port | Protocol | Common Service |
|---|---|---|
| 20 | TCP | FTP data |
| 21 | TCP | FTP control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67 | UDP | DHCP server |
| 68 | UDP | DHCP client |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP |
| 389 | TCP/UDP | LDAP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 587 | TCP | SMTP submission |
| 993 | TCP | IMAPS |
| 995 | TCP | POP3S |
| 3306 | TCP | MySQL |
| 3389 | TCP/UDP | RDP |
| 5432 | TCP | PostgreSQL |
| 8080 | TCP | Common alternative HTTP port |

A service may use a different port depending on its configuration.

Also, newer protocols can use different transports. For example, HTTP/3 uses QUIC over UDP.

---

## 7. Understanding Sockets

A socket is a software communication endpoint.

It allows applications to send and receive data.

A network socket is commonly identified by:

- Transport protocol.
- Local IP address.
- Local port.
- Remote IP address.
- Remote port.

For example:

```text
TCP 192.168.1.10:50000 → 93.184.216.34:443
```

This represents a TCP connection from a local client port to a remote HTTPS port.

### Types of Sockets

**1. Stream Sockets**

Commonly associated with TCP.

They provide a reliable, ordered byte stream.

**2. Datagram Sockets**

Commonly associated with UDP.

They transmit individual datagrams without a connection-oriented transport guarantee.

**3. Unix Domain Sockets**

Used for communication between processes on the same machine.

Example:

```text
/run/docker.sock
```

Unix domain sockets do not require ordinary IP networking.

---

## 8. Understanding Listening Ports

A listening port is a port on which a service is waiting for incoming connections or datagrams.

For example, an SSH server may listen on TCP port 22.

A listening port does not automatically mean that the service is exposed to the public internet.

Its actual accessibility depends on:

- The interface it binds to.
- Routing.
- Firewall rules.
- Network security controls.
- Virtualization and network configuration.

### Example

```text
0.0.0.0:22
```

This generally indicates that the service is listening on all available IPv4 interfaces.

```text
127.0.0.1:22
```

This indicates that the service is listening only on the IPv4 loopback interface.

For IPv6, you may encounter:

```text
[::]:22
```

This generally indicates listening on IPv6 wildcard addresses.

---

## 9. Linux Commands for Inspecting Ports

Linux provides several commands for inspecting network sockets and ports.

### 9.1 Using `ss`

`ss` stands for Socket Statistics.

It is a modern utility for examining socket information.

Display listening TCP and UDP sockets:

```bash
ss -tuln
```

Options:

| Option | Meaning |
|---|---|
| `-t` | TCP |
| `-u` | UDP |
| `-l` | Listening sockets |
| `-n` | Numeric addresses and ports |

### 9.2 Display All TCP Connections

```bash
ss -tan
```

### 9.3 Display All UDP Sockets

```bash
ss -uan
```

### 9.4 Display Listening TCP Ports

```bash
ss -tln
```

### 9.5 Display Listening UDP Ports

```bash
ss -uln
```

### 9.6 Display Processes Associated with Sockets

```bash
sudo ss -tulpn
```

The `-p` option displays process information where available.

### 9.7 Display Established Connections

```bash
ss -tn state established
```

This displays established TCP connections.

---

## 10. Understanding Netstat

`netstat` is a traditional networking utility used to inspect network connections, routing information, and network statistics.

Example:

```bash
netstat -tuln
```

However, `netstat` is considered a legacy tool on many Linux systems.

The modern `ss` utility is generally preferred.

---

## 11. Testing TCP Ports with Netcat

Netcat (`nc`) is a networking utility used to read and write data over network connections.

It can help test whether a TCP port accepts connections.

### Example: Test HTTPS Port

```bash
nc -vz example.com 443
```

Options:

- `-v`: Verbose output.
- `-z`: Scan mode without sending application data.

### Example: Test SSH Port

```bash
nc -vz YOUR_SERVER_IP 22
```

Only test systems you own or have permission to assess.

A successful connection means the TCP connection was accepted. It does not prove that the application is secure or functioning correctly.

---

## 12. Understanding Port Security

Open ports are important from a cybersecurity perspective.

Every network-accessible service may introduce a potential attack surface.

### Common Risks

**1. Unnecessary Open Ports**

Services that are not needed may increase exposure.

**2. Weak Authentication**

An exposed service with weak credentials may be vulnerable to unauthorized access.

**3. Outdated Services**

Old software may contain known security vulnerabilities.

**4. Insecure Protocols**

Some protocols transmit information without adequate protection.

Example: Telnet does not provide encryption by itself.

**5. Misconfigured Services**

A service may listen on an unintended network interface.

### Security Best Practices

- Disable unnecessary services.
- Keep software updated.
- Use strong authentication.
- Prefer SSH over Telnet.
- Apply appropriate firewall rules.
- Restrict administrative services to trusted networks.
- Monitor unexpected listening ports.
- Use encryption where appropriate.

---

## 13. Practical Lab: Inspecting Ports and Sockets in Kali Linux

### Lab Objective

Use Linux networking utilities to identify listening sockets and understand active connections.

This lab focuses on inspection and safe testing.

### Step 1: Display Listening TCP and UDP Sockets

```bash
ss -tuln
```

Record any listening ports.

### Step 2: Display Listening TCP Ports

```bash
ss -tln
```

### Step 3: Display Listening UDP Ports

```bash
ss -uln
```

### Step 4: Display All TCP Connections

```bash
ss -tan
```

### Step 5: Display Established TCP Connections

```bash
ss -tn state established
```

### Step 6: Identify Processes Associated with Sockets

```bash
sudo ss -tulpn
```

Observe the process names and port numbers.

### Step 7: Inspect Your Localhost

```bash
ss -tuln
```

Look for addresses such as:

```text
127.0.0.1
[::1]
```

These represent IPv4 and IPv6 loopback addresses.

### Step 8: Test an HTTPS Port

```bash
nc -vz example.com 443
```

Observe whether the TCP connection succeeds.

### Step 9: Check the SSH Port on Your Own VM

```bash
nc -vz 127.0.0.1 22
```

If no SSH service is listening locally, the connection may fail. This is normal.

### Step 10: Compare TCP and UDP Information

```bash
ss -tan
```

Then:

```bash
ss -uan
```

Explain the difference between the displayed socket information.

---

## 14. Practical Analysis Table

Record your observations:

| Investigation | Result |
|---|---|
| Listening TCP ports | |
| Listening UDP ports | |
| Established TCP connections | |
| Processes using sockets | |
| Localhost listening services | |
| HTTPS TCP test | |
| Local SSH test | |

---

## 15. Assessment Questions

Answer the following:

1. What is a network port?
2. What is the range of TCP and UDP port numbers?
3. What is the difference between TCP and UDP?
4. What are the three steps of the TCP three-way handshake?
5. Which port is commonly used by SSH?
6. Which port is commonly used by HTTPS over TCP?
7. What is a socket?
8. What is the difference between a listening port and an established connection?
9. What does `ss -tuln` display?
10. What does `0.0.0.0:22` generally indicate?
11. Why are unnecessary open ports a security risk?
12. Why is SSH preferred over Telnet for remote administration?

---

## 16. Important Commands to Remember

```bash
ss -tuln
ss -tln
ss -uln
ss -tan
ss -tn state established
sudo ss -tulpn

nc -vz example.com 443
nc -vz 127.0.0.1 22

netstat -tuln
```

---

## 17. Lesson Summary

In this lesson, you learned:

- Network ports identify application communication endpoints.
- Port numbers range from 0 to 65535.
- TCP provides reliable, ordered communication.
- UDP provides connectionless datagram delivery.
- A socket is a software communication endpoint.
- Listening ports indicate services waiting for traffic.
- `ss` is the preferred modern Linux socket inspection tool.
- Netcat can test TCP connectivity.
- Unnecessary or poorly secured services increase attack surface.

### Key Definitions

**Port:** A logical communication endpoint identified by a port number.

**TCP:** Transmission Control Protocol, a connection-oriented transport protocol.

**UDP:** User Datagram Protocol, a connectionless transport protocol.

**Socket:** A software endpoint used for communication between applications.

**Listening Port:** A port on which a service is waiting for incoming traffic.

**Established Connection:** A TCP connection that has completed its connection-establishment process.

**Attack Surface:** The set of points through which an attacker may attempt to interact with or compromise a system.

---

## 18. Knowledge Check

1. What is the purpose of a port number?
2. Which protocol uses a three-way handshake?
3. Which port is associated with DNS?
4. Which command displays listening TCP sockets?
5. What is the purpose of Netcat?
6. What does a listening port tell you?
7. Why should unnecessary network services be disabled?
8. What is the difference between a TCP socket and a Unix domain socket?

**End of Lesson 09 – Understanding Ports and Sockets**