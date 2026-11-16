
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 07: PORTS, SOCKETS AND CONNECTIONS

### 1. Lab Overview

Computers use network ports to identify specific services and applications communicating over a network.

For example:

- HTTP commonly uses port 80.
- HTTPS commonly uses port 443.
- SSH commonly uses port 22.
- DNS commonly uses port 53.

Ports help operating systems deliver network traffic to the correct application.

In cybersecurity, understanding ports and sockets is essential for:

- Network administration.
- Service monitoring.
- Firewall configuration.
- Security auditing.
- Incident response.
- Network troubleshooting.
- Understanding exposed services.

In this lab, you will inspect listening ports, identify network connections, understand TCP and UDP sockets, and examine which applications are communicating.

**Lab environment:** Kali Linux VM.

**Safety:** This lab focuses on inspecting your own system. Do not probe external systems or attempt to access services without authorization.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain network ports.
2. Understand sockets.
3. Differentiate TCP and UDP.
4. Identify common service ports.
5. Inspect listening ports.
6. Identify established connections.
7. Use the `ss` command.
8. Understand the purpose of `lsof`.
9. Identify services associated with ports.
10. Generate a port and connection diagnostic report.

### 3. What Is a Network Port?

A network port is a logical number used to identify a service or application endpoint.

TCP and UDP use port numbers ranging from:

```text
0 – 65535
```

Example:

```text
192.168.100.20:22
```

This represents:

- IP address: `192.168.100.20`
- Port: `22`

Port 22 is commonly associated with SSH.

### 4. Port Number Categories

Port numbers are divided into three major categories.

| Category | Range | Description |
|---|---|---|
| Well-known ports | 0–1023 | Common standard services |
| Registered ports | 1024–49151 | Registered application services |
| Dynamic/private ports | 49152–65535 | Commonly used for temporary client connections |

Some operating systems and applications use different ephemeral port ranges.

### 5. Common Network Ports

| Port | Protocol | Common Service |
|---:|---|---|
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
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 587 | TCP | SMTP submission |
| 993 | TCP | IMAPS |
| 995 | TCP | POP3S |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |
| 5432 | TCP | PostgreSQL |
| 8080 | TCP | Alternative HTTP |

Important: A port number does not prove which service is actually running. Services can use non-standard ports.

### 6. What Is TCP?

TCP stands for Transmission Control Protocol.

TCP is connection-oriented.

It establishes a connection before exchanging application data.

Important characteristics:

- Reliable delivery.
- Packet ordering.
- Error detection.
- Retransmission.
- Flow control.

Examples of services that commonly use TCP include:

- SSH.
- HTTPS.
- SMTP.
- FTP.

### 7. What Is UDP?

UDP stands for User Datagram Protocol.

UDP is connectionless.

It does not establish a TCP-style connection before sending datagrams.

Characteristics:

- Lower protocol overhead.
- No built-in delivery guarantee.
- No built-in retransmission.
- No built-in packet ordering.

Common applications include:

- DNS queries.
- DHCP.
- Voice and video applications.
- Online gaming.
- Some streaming protocols.

### 8. TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Delivery guarantee | Yes, at transport layer | No |
| Ordering | Yes | No |
| Retransmission | Supported | Not built in |
| Overhead | Generally higher | Generally lower |
| Typical use | Web, SSH, email | DNS, voice, gaming |

### 9. What Is a Socket?

A socket is an endpoint used by an application for network communication.

A network connection is commonly identified by:

- Source IP address.
- Source port.
- Destination IP address.
- Destination port.
- Transport protocol.

Example:

```text
192.168.100.20:51500
        |
        v
203.0.113.10:443
```

This is an illustrative connection.

The client uses a temporary source port and communicates with a destination service on port 443.

### 10. What Is a Listening Port?

A listening port is a port on which an application is waiting for incoming connections or datagrams.

For example, an SSH server may listen on:

```text
0.0.0.0:22
```

This means it is listening on all IPv4 interfaces.

An application listening on:

```text
127.0.0.1:8080
```

is listening only on the IPv4 loopback interface.

This distinction is important for security.

A service bound to a loopback address is generally not directly reachable from other network devices.

### 11. Inspect Listening Ports with ss

The `ss` utility displays socket information.

Run:

```bash
ss -tuln
```

Understand the options:

| Option | Meaning |
|---|---|
| `-t` | TCP sockets |
| `-u` | UDP sockets |
| `-l` | Listening sockets |
| `-n` | Display numerical addresses and ports |

Possible output:

```text
Netid State  Local Address:Port
tcp   LISTEN 0.0.0.0:22
udp   UNCONN 0.0.0.0:53
```

Your output will depend on which services are running.

If no ports appear, that is not necessarily an error.

### 12. Inspect TCP Listening Ports

Run:

```bash
ss -tl
```

For numerical output:

```bash
ss -tln
```

This displays TCP listening sockets.

Look for:

- Local address.
- Local port.
- Listening state.

### 13. Inspect UDP Sockets

Run:

```bash
ss -uln
```

UDP does not use TCP's LISTEN state.

You may see a state such as:

```text
UNCONN
```

This does not mean that the UDP socket is broken.

It reflects UDP's connectionless communication model.

### 14. Inspect Established TCP Connections

Run:

```bash
ss -tn state established
```

This displays established TCP connections.

If there are no established connections, that may simply mean no active TCP application connections exist at that moment.

To display more information:

```bash
ss -tna
```

### 15. Understand TCP Connection States

Common TCP states include:

| State | Meaning |
|---|---|
| LISTEN | Waiting for incoming connections |
| ESTAB | Connection established |
| SYN-SENT | Connection request sent |
| SYN-RECV | Connection request received |
| FIN-WAIT-1 | Connection closing process started |
| FIN-WAIT-2 | Waiting for remote endpoint to close |
| TIME-WAIT | Waiting before final connection cleanup |
| CLOSE-WAIT | Remote endpoint has closed its side |
| CLOSED | No active connection |

These states help administrators troubleshoot connection behavior.

### 16. Identify Processes Using Network Sockets

Check whether `lsof` is installed:

```bash
command -v lsof
```

If available, inspect network-related files and sockets:

```bash
sudo lsof -i -n -P
```

Explanation:

- `sudo`: Runs with elevated privileges.
- `lsof`: Lists open files.
- `-i`: Selects network-related entries.
- `-n`: Avoids hostname resolution.
- `-P`: Displays numerical port numbers.

Use this command only to inspect your own VM.

If `lsof` is unavailable, continue using `ss`.

### 17. Identify Processes with ss

Try:

```bash
sudo ss -tulpn
```

The `-p` option displays process information where permissions and system support allow it.

Example:

```text
Netid State  Local Address:Port Process
tcp   LISTEN 0.0.0.0:22       users:(("sshd",pid=1234,fd=3))
```

This is an illustrative example.

You may not see any listening TCP services.

### 18. Inspect a Specific Port

You can filter socket output by port.

For example, inspect TCP port 22:

```bash
ss -tln 'sport = :22'
```

Inspect TCP port 443:

```bash
ss -tln 'sport = :443'
```

Inspect UDP port 53:

```bash
ss -uln 'sport = :53'
```

These commands inspect your local system's socket table.

An empty result means no matching socket was displayed at that time.

### 19. Understand Localhost and Network Binding

Applications can bind to different addresses.

| Address | Meaning |
|---|---|
| `127.0.0.1` | IPv4 loopback only |
| `0.0.0.0` | All local IPv4 interfaces |
| `::1` | IPv6 loopback only |
| `::` | IPv6 wildcard address |

A service listening on all interfaces may be accessible through multiple network interfaces, depending on firewall rules and routing.

This is why administrators should review service binding configurations.

### 20. Security Importance of Open Ports

An unnecessarily exposed service can increase the attack surface of a system.

Security administrators should:

1. Identify listening services.
2. Confirm whether each service is necessary.
3. Verify service configurations.
4. Restrict access using appropriate firewall policies.
5. Remove or disable unnecessary services through approved administration procedures.
6. Monitor unexpected connections.
7. Review service logs.

Do not disable services blindly. Some are required for system operation.

### 21. Practical Exercise 1 – Inspect Listening Ports

Run:

```bash
ss -tuln
```

Record:

- TCP listening sockets.
- UDP sockets.
- Local addresses.
- Port numbers.

### 22. Practical Exercise 2 – Inspect TCP Connections

Run:

```bash
ss -tn state established
```

Then:

```bash
ss -tna
```

Record any established connections and their states.

### 23. Practical Exercise 3 – Inspect Specific Ports

Run:

```bash
ss -tln 'sport = :22'
```

```bash
ss -tln 'sport = :80'
```

```bash
ss -tln 'sport = :443'
```

Record which ports have matching listening sockets.

Do not assume a port must be open just because it is commonly used by a service.

### 24. Practical Exercise 4 – Identify Processes

Run:

```bash
sudo ss -tulpn
```

If `lsof` is installed, run:

```bash
sudo lsof -i -n -P
```

Identify any processes associated with listening sockets.

If no process information is available, record that limitation.

### 25. Practical Exercise 5 – Inspect Local Binding

Run:

```bash
ss -tuln
```

Look for addresses such as:

```text
127.0.0.1
0.0.0.0
::1
::
```

Record what each address means.

Consider why a service bound to `127.0.0.1` has a different exposure profile from one bound to `0.0.0.0`.

### 26. Practical Exercise 6 – Examine Network Service Names

Linux maintains a services database.

Inspect it:

```bash
cat /etc/services
```

Search for common services:

```bash
grep -E '^(ssh|http|https|domain|smtp)[[:space:]]' /etc/services
```

This file provides conventional service-name and port mappings.

It does not prove that a particular service is currently running.

### 27. Generate a Port and Connection Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab07
```

Create the script:

```bash
nano ~/practical-labs/module05/lab07/socket-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab07/socket-report.txt"

{
    echo "LINUX PORT AND SOCKET DIAGNOSTIC REPORT"
    echo "======================================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "TCP AND UDP LISTENING SOCKETS"
    ss -tuln
    echo

    echo "TCP LISTENING SOCKETS"
    ss -tln
    echo

    echo "UDP SOCKETS"
    ss -uln
    echo

    echo "ESTABLISHED TCP CONNECTIONS"
    ss -tn state established
    echo

    echo "ALL TCP SOCKET STATES"
    ss -tna
    echo

    echo "PROCESS INFORMATION"
    sudo ss -tulpn
    echo

    echo "COMMON SERVICE PORT MAPPINGS"
    grep -E '^(ssh|http|https|domain|smtp)[[:space:]]' /etc/services

    echo
    echo "NETWORK INTERFACE SUMMARY"
    ip -br addr

} > "$REPORT" 2>&1

echo "Socket report generated: $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab07/socket-report.sh
```

Execute:

```bash
sudo ~/practical-labs/module05/lab07/socket-report.sh
```

Inspect the report:

```bash
cat ~/practical-labs/module05/lab07/socket-report.txt
```

### 28. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab07/lab07-report.txt
```

Record:

```text
MODULE 05 – LAB 07
PORTS, SOCKETS AND CONNECTIONS

1. What is a network port?
2. What is a socket?
3. What is TCP?
4. What is UDP?
5. What is the difference between TCP and UDP?
6. What is a listening port?
7. What does ss -tuln display?
8. Number of TCP listening sockets:
9. Number of UDP sockets:
10. Any established TCP connections:
11. Which common ports did I identify?
12. What is the meaning of 127.0.0.1?
13. What is the meaning of 0.0.0.0?
14. Which processes were associated with sockets?
15. What is the security risk of unnecessary listening services?
16. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 29. Knowledge Check

1. What is a network port?
2. What is the port-number range?
3. What is the difference between TCP and UDP?
4. What is a socket?
5. What is a listening port?
6. What service commonly uses port 22?
7. What service commonly uses port 443?
8. What does `ss -tuln` do?
9. What does `ss -tn state established` show?
10. What is the meaning of the TCP LISTEN state?
11. What is the meaning of ESTAB?
12. What is the difference between `127.0.0.1` and `0.0.0.0`?
13. What does `lsof -i` help identify?
14. Why should unnecessary listening services be reviewed?
15. Why is port inspection important in cybersecurity?

### 30. Lab Completion Checklist

- [ ] Understood network ports.
- [ ] Studied TCP and UDP.
- [ ] Understood sockets.
- [ ] Learned port-number categories.
- [ ] Studied common network ports.
- [ ] Inspected listening TCP sockets.
- [ ] Inspected UDP sockets.
- [ ] Examined established connections.
- [ ] Studied TCP connection states.
- [ ] Identified processes using sockets.
- [ ] Inspected specific local ports.
- [ ] Studied network binding addresses.
- [ ] Inspected `/etc/services`.
- [ ] Generated the socket diagnostic report.
- [ ] Completed lab documentation.

**LAB 07 STATUS: READY FOR PRACTICAL COMPLETION**
