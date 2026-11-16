
# MODULE 13 – METASPLOITABLE LABS

# LESSON 05: SERVICE AND PORT ANALYSIS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain network ports and services.
2. Understand TCP and UDP.
3. Identify common network services.
4. Interpret Nmap port scan results.
5. Perform controlled service version detection.
6. Understand service banners.
7. Identify potentially insecure services.
8. Document service and port analysis findings.

---

## 2. What Is a Network Port?

A network port is a logical communication endpoint used by applications and services.

Think of an IP address as a building address and a port as a particular room inside that building.

For example:

```text
IP Address: 192.168.100.20
Port: 22
Service: SSH
```

The IP address identifies the machine, while port 22 identifies the SSH service endpoint.

### Port Number Ranges

TCP and UDP port numbers range from 0 to 65535.

| Range | Classification |
|---|---|
| 0–1023 | Well-known ports |
| 1024–49151 | Registered ports |
| 49152–65535 | Dynamic/private ports |

These ranges are commonly used for port classification.

---

## 3. Understanding TCP and UDP

### TCP – Transmission Control Protocol

TCP is connection-oriented.

It establishes a connection before transferring data.

Important characteristics:

- Reliable data delivery.
- Ordered communication.
- Error detection.
- Connection management.
- Retransmission when necessary.

Examples of TCP services:

- HTTP.
- HTTPS.
- SSH.
- FTP.
- SMTP.

### UDP – User Datagram Protocol

UDP is connectionless.

It sends datagrams without establishing a TCP-style connection.

Important characteristics:

- Lower protocol overhead.
- No built-in delivery guarantee.
- No built-in ordering guarantee.
- Useful for real-time and lightweight communication.

Examples include:

- DNS queries.
- DHCP.
- NTP.

Some applications use both TCP and UDP under different circumstances.

---

## 4. Common Network Ports

Study the following table.

| Port | Protocol | Service |
|---:|---|---|
| 20 | TCP | FTP Data |
| 21 | TCP | FTP Control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67 | UDP | DHCP Server |
| 68 | UDP | DHCP Client |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 143 | TCP | IMAP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |

These are conventional service port assignments. Actual applications can use different ports.

---

## 5. Understanding Open Ports

An open port indicates that a service is accepting connections on that port.

Example:

```text
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
3306/tcp open  mysql
```

This tells us that:

- TCP port 22 is accepting connections.
- TCP port 80 is accepting connections.
- TCP port 3306 is accepting connections.

However, an open port does not automatically mean the service is vulnerable.

A security assessor must examine its configuration, version, authentication, exposure, and known weaknesses.

---

## 6. Introduction to Service Detection

Service detection helps identify the application or protocol running behind an open port.

For example:

```text
22/tcp open ssh OpenSSH
```

This suggests an SSH service is listening.

Nmap can attempt to identify service versions using:

```bash
nmap -sV TARGET_IP
```

For our lab, use the actual Metasploitable IP only.

Example:

```bash
nmap -sV 192.168.100.20
```

Do not copy the example address if your target uses another address.

---

## 7. Practical Lab 1 – Review Your Previous Scan Results

Navigate to your Lesson 04 workspace:

```bash
cd ~/practical-labs/module13/lab04
```

List your files:

```bash
ls -l
```

Read the previous scan results:

```bash
cat port-scan.txt
```

If you used another filename, read that file instead.

Identify:

1. Open ports.
2. Closed ports.
3. Filtered ports.
4. Reported services.

Record the ports that you will examine further.

---

## 8. Practical Lab 2 – Perform Targeted TCP Service Detection

Set your target variable:

```bash
TARGET=YOUR_METASPLOITABLE_IP
```

Replace the placeholder with your actual target address.

Example:

```bash
TARGET=192.168.100.20
```

Perform a limited service detection scan:

```bash
nmap -sV -p 21,22,23,25,80 "$TARGET"
```

This examines selected TCP ports on your own target.

Save the output:

```bash
nmap -sV -p 21,22,23,25,80 "$TARGET" -oN service-analysis.txt
```

### What to Observe

Record:

- Port number.
- Protocol.
- Port state.
- Service name.
- Version information, if detected.

Do not assume that every selected port will be open.

---

## 9. Practical Lab 3 – Analyze Additional Services

Metasploitable 2 may expose additional services.

Depending on your previous scan results, examine selected ports such as:

```bash
nmap -sV -p 139,445,3306,5432 "$TARGET"
```

These ports are commonly associated with:

| Port | Service |
|---:|---|
| 139 | NetBIOS/SMB |
| 445 | SMB |
| 3306 | MySQL |
| 5432 | PostgreSQL |

Record only the services that your scan actually identifies.

Save the results:

```bash
nmap -sV -p 139,445,3306,5432 "$TARGET" -oN additional-services.txt
```

---

## 10. Practical Lab 4 – Understand Service Banners

A service banner is information returned by a service when a client connects or makes a protocol request.

Banners may reveal:

- Software name.
- Software version.
- Operating system clues.
- Server configuration information.

This information can help security professionals identify outdated software.

However, some services hide or modify their banners.

### Example

```text
Service: SSH
Version: Example SSH version
```

This is illustrative.

A banner alone does not prove that the software contains an exploitable vulnerability.

Always verify findings using reliable evidence.

---

## 11. Practical Lab 5 – Understand Insecure Services

Some network services are considered risky when exposed or poorly configured.

### Telnet – Port 23

Telnet does not provide encryption by default.

Credentials and session data may be transmitted in readable form.

**Recommendation:** Replace Telnet with SSH where appropriate.

### FTP – Port 21

Traditional FTP does not encrypt credentials or transferred files by default.

**Recommendation:** Use secure alternatives such as SFTP or properly configured FTPS.

### SSH – Port 22

SSH provides encrypted remote access.

However, weak passwords, outdated software, and poor configuration can still introduce security risks.

**Recommendation:** Use strong authentication, key-based access where appropriate, updates, and access restrictions.

### SMB – Ports 139 and 445

SMB supports file and printer sharing.

Poor configuration may expose sensitive files or unnecessary services.

**Recommendation:** Restrict access and enforce appropriate authentication and permissions.

### Database Services

MySQL and PostgreSQL provide database access.

Exposing database services unnecessarily increases the attack surface.

**Recommendation:** Restrict database access to authorized application systems and administrators.

---

## 12. Practical Lab 6 – Create a Service Analysis Table

Create a new workspace:

```bash
mkdir -p ~/practical-labs/module13/lab05
cd ~/practical-labs/module13/lab05
```

Create:

```bash
nano service-analysis.md
```

Use this template:

```markdown
# METASPLOITABLE SERVICE ANALYSIS

## Target Information

Target:
Target IP:
Testing Machine:
Network Mode:

## Discovered Services

| Port | Protocol | Service | Version | State |
|------|----------|---------|---------|-------|
|      |          |         |         |       |

## Service Analysis

### Service 1

Service:
Purpose:
Security Concerns:
Recommendation:

### Service 2

Service:
Purpose:
Security Concerns:
Recommendation:

## Important Observations

Record findings supported by scan evidence.

## Limitations

Record services that could not be identified.
```

Populate the table using your actual scan results.

Do not invent versions or service states.

---

## 13. Practical Lab 7 – Save a Final Scan Report

From your Lesson 05 directory, run:

```bash
nmap -sV -p 21,22,23,25,80,139,445,3306,5432 "$TARGET" -oN final-service-scan.txt
```

Review:

```bash
cat final-service-scan.txt
```

Check your files:

```bash
ls -l
```

You should have:

- `service-analysis.md`
- `final-service-scan.txt`

You may also retain the other scan output files.

---

## 14. Knowledge Check

Answer these questions:

1. What is a network port?
2. What is the difference between TCP and UDP?
3. What is the purpose of TCP port 22?
4. Which service commonly uses port 80?
5. What is service version detection?
6. What is a service banner?
7. Why is Telnet considered insecure?
8. Why should database services not be unnecessarily exposed?
9. What does an open port indicate?
10. Why is service identification important in vulnerability assessment?

---

## 15. Lesson Summary

You have learned:

- Network ports and their ranges.
- TCP and UDP.
- Common network services.
- Open, closed, and filtered ports.
- Nmap service detection.
- Service banners.
- Risks associated with insecure services.
- Service analysis documentation.
- Professional reporting of enumeration results.

## 16. Professional References

1. Nmap Service and Version Detection: https://nmap.org/book/man-version-detection.html
2. IANA Service Name and Port Number Registry: https://www.iana.org/assignments/service-names-port-numbers/
3. OpenSSH Manual: https://www.openssh.com/manual.html
4. OWASP Cheat Sheet Series: https://cheatsheetseries.owasp.org/

---

# END OF LESSON 05
