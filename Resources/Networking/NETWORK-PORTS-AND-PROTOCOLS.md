
# NETWORK PORTS AND PROTOCOLS

## 1. What Is a Network Protocol?

A network protocol is a set of rules that allows devices to communicate over a network.

Examples:
- TCP
- UDP
- HTTP
- HTTPS
- DNS
- SSH

## 2. What Is a Network Port?

A port is a logical communication endpoint used by applications and services.

Ports help a computer direct network traffic to the correct application.

Port numbers range from 0 to 65535.

## 3. Port Number Categories

| Range | Category |
|---|---|
| 0–1023 | Well-known ports |
| 1024–49151 | Registered ports |
| 49152–65535 | Dynamic/private ports |

## 4. Common Network Ports

| Port | Protocol/Service | Purpose |
|---:|---|---|
| 20 | FTP | File transfer data |
| 21 | FTP | File transfer control |
| 22 | SSH | Secure remote access |
| 23 | Telnet | Unencrypted remote access |
| 25 | SMTP | Email transmission |
| 53 | DNS | Domain name resolution |
| 67 | DHCP | IPv4 address assignment, server |
| 68 | DHCP | IPv4 address assignment, client |
| 69 | TFTP | Simple file transfer |
| 80 | HTTP | Web traffic |
| 110 | POP3 | Email retrieval |
| 123 | NTP | Time synchronization |
| 143 | IMAP | Email access |
| 161 | SNMP | Network management |
| 389 | LDAP | Directory services |
| 443 | HTTPS | Secure web traffic |
| 445 | SMB | Windows file sharing |
| 465 | SMTPS | Email submission over TLS |
| 514 | Syslog | System logging |
| 587 | SMTP Submission | Email submission |
| 636 | LDAPS | LDAP over TLS |
| 993 | IMAPS | Secure IMAP |
| 995 | POP3S | Secure POP3 |
| 1433 | Microsoft SQL Server | Database access |
| 3306 | MySQL | Database access |
| 3389 | RDP | Remote Desktop |
| 5432 | PostgreSQL | Database access |
| 5900 | VNC | Remote desktop access |
| 8080 | HTTP Alternative | Alternative web service port |

Note: A service can use a different port depending on its configuration.

## 5. TCP and UDP

### TCP

Transmission Control Protocol.

Features:
- Connection-oriented.
- Reliable data delivery.
- Uses acknowledgements.
- Maintains packet order.
- Provides retransmission when needed.

Examples:
- HTTPS
- SSH
- FTP

### UDP

User Datagram Protocol.

Features:
- Connectionless.
- Does not guarantee delivery.
- Does not guarantee packet ordering.
- Has lower protocol overhead.

Examples:
- DNS queries commonly use UDP.
- DHCP.
- Voice and video applications.

## 6. HTTP and HTTPS

HTTP stands for Hypertext Transfer Protocol.

HTTPS is HTTP protected using TLS.

- HTTP commonly uses port 80.
- HTTPS commonly uses port 443.

HTTPS helps protect data in transit through encryption and authentication.

## 7. DNS

DNS stands for Domain Name System.

It translates domain names into IP addresses.

Example:

example.com → IP address

Common DNS port:
- UDP 53
- TCP 53

## 8. SSH

SSH stands for Secure Shell.

It provides encrypted remote access to another computer.

Default port: 22.

Example:

```bash
ssh username@192.168.1.10
```

## 9. Inspect Listening Ports in Linux

Display listening TCP and UDP ports:

```bash
ss -tuln
```

Display listening ports with process information:

```bash
sudo ss -tulpn
```

Display TCP connections:

```bash
ss -t
```

Display UDP sockets:

```bash
ss -u
```

## 10. Test a Specific Port

Using netcat:

```bash
nc -zv 192.168.1.10 22
```

This checks whether a TCP connection can be established to port 22.

Only test systems you own or are authorized to assess.

## 11. Cybersecurity Applications

Understanding ports and protocols helps security professionals:

- Identify exposed services.
- Investigate suspicious connections.
- Configure firewall rules.
- Understand network traffic.
- Recognize insecure services.
- Perform authorized security assessments.

## 12. Security Best Practices

- Avoid exposing unnecessary ports.
- Replace insecure Telnet with SSH.
- Secure remote desktop services.
- Restrict database access.
- Monitor unexpected listening services.
- Keep network services updated.
- Use firewalls to control access.

## 13. Practice Exercises

1. What is a network port?
2. What is the difference between TCP and UDP?
3. Which port does SSH commonly use?
4. Which port does HTTPS commonly use?
5. What is the purpose of DNS?
6. What is the security difference between HTTP and HTTPS?
7. Which Linux command displays listening ports?

## Summary

Network ports identify communication endpoints, while protocols define how data is exchanged. Understanding both is essential for networking, troubleshooting, and cybersecurity.
