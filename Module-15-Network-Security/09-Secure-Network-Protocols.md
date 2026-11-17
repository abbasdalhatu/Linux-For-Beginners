---
---

# MODULE 15 – NETWORK SECURITY
## LESSON 09: SECURE NETWORK PROTOCOLS

---

## 1. Introduction

Network protocols define the rules that devices follow when communicating.

Some protocols were designed primarily for functionality and convenience, without strong security protections.

As cybersecurity developed, more secure alternatives were introduced.

For example:

- HTTP was supplemented by HTTPS.
- Telnet was largely replaced by SSH.
- FTP can be replaced by SFTP or FTPS.
- Traditional remote login methods can be replaced with encrypted administration protocols.

In this lesson, you will learn how secure network protocols protect information and why organizations should use them.

---

## 2. What Is a Network Protocol?

A network protocol is a set of rules that controls how devices exchange information.

Protocols define:

- How communication begins.
- How information is formatted.
- How devices identify each other.
- How data is transmitted.
- How errors are handled.
- How communication ends.

### Examples

| Protocol | Purpose |
|---|---|
| HTTP | Transfers web content |
| HTTPS | Secure web communication |
| SSH | Secure remote administration |
| FTP | File transfer |
| SFTP | Secure file transfer over SSH |
| SMTP | Sends email |
| DNS | Resolves domain names |
| TLS | Protects communication cryptographically |

---

## 3. Understanding Cleartext and Encrypted Protocols

One of the most important differences between network protocols is how they protect transmitted information.

### 3.1 Cleartext Protocols

A cleartext protocol sends information without application-layer encryption.

If someone can capture the traffic, some transmitted information may be readable.

Examples include traditional:

- HTTP.
- Telnet.
- FTP.
- POP3 without TLS.
- IMAP without TLS.

### 3.2 Encrypted Protocols

Encrypted protocols protect transmitted information against passive observation and certain forms of manipulation.

Examples include:

- HTTPS.
- SSH.
- SFTP.
- FTPS.
- IMAPS.
- POP3S.

### Comparison

| Cleartext protocol | Secure alternative |
|---|---|
| HTTP | HTTPS |
| Telnet | SSH |
| FTP | SFTP or FTPS |
| POP3 | POP3S or POP3 with STARTTLS |
| IMAP | IMAPS or IMAP with STARTTLS |
| SMTP without TLS | SMTP with STARTTLS or implicit TLS |

---

## 4. HTTP vs HTTPS

### HTTP

HTTP stands for Hypertext Transfer Protocol.

It is used for communication between web clients and servers.

Traditional HTTP does not encrypt application data.

Its default port is:

```text
80
```

### HTTPS

HTTPS means HTTP Secure.

It uses TLS to protect HTTP communication.

Its default port is:

```text
443
```

### Benefits of HTTPS

1. Confidentiality – helps prevent unauthorized parties from reading transmitted content.
2. Integrity – helps detect unauthorized modification.
3. Authentication – helps the browser verify the server's identity through certificates.

### Example

HTTP:

```text
http://example.com
```

HTTPS:

```text
https://example.com
```

Always prefer HTTPS when accessing websites, especially when entering sensitive information.

Remember that HTTPS does not prove a website is honest. Phishing websites can also use valid HTTPS certificates.

---

## 5. SSH – Secure Shell

SSH stands for Secure Shell.

It provides encrypted remote access to computers and servers.

System administrators use SSH to:

- Manage Linux servers.
- Execute remote commands.
- Transfer files securely.
- Create secure tunnels.
- Administer remote infrastructure.

### Default port

```text
TCP 22
```

### Example SSH connection

```bash
ssh username@192.168.1.10
```

Replace the username and IP address with an authorized remote system's details.

### SSH authentication methods

#### Password Authentication

The user supplies a password to authenticate.

#### Public-Key Authentication

SSH uses a cryptographic key pair:

- Private key: kept secret by the user.
- Public key: placed on the remote server.

Public-key authentication can provide stronger security when configured properly.

### Generate an SSH key pair

On your own Kali VM:

```bash
ssh-keygen -t ed25519
```

Follow the prompts.

Do not share your private key.

### Inspect SSH configuration

```bash
ss -tulpen | grep ':22'
```

This shows whether a process is listening on port 22, if any.

Check SSH client availability:

```bash
ssh -V
```

---

## 6. FTP vs SFTP vs FTPS

These protocols support file transfer, but their security properties differ.

### FTP

FTP stands for File Transfer Protocol.

Traditional FTP does not encrypt its communication.

It commonly uses TCP port 21 for its control connection.

### SFTP

SFTP stands for SSH File Transfer Protocol.

It transfers files through an SSH connection.

It commonly uses TCP port 22.

### FTPS

FTPS is FTP secured using TLS.

It commonly uses TCP port 21 for explicit TLS mode, although configurations vary.

Implicit FTPS commonly uses TCP port 990.

### Comparison

| Feature | FTP | SFTP | FTPS |
|---|---|---|---|
| Encryption | No, traditionally | Yes, through SSH | Yes, through TLS |
| Security | Weak without extra protection | Strong when properly configured | Strong when properly configured |
| Main technology | FTP | SSH | FTP + TLS |
| Common control port | 21 | 22 | 21 or 990 |

**Important:** SFTP and FTPS are different protocols. SFTP is not simply encrypted FTP.

---

## 7. TLS – Transport Layer Security

TLS stands for Transport Layer Security.

It is a cryptographic protocol used to protect communication over networks.

TLS is used by:

- HTTPS.
- Secure email connections.
- Some VPN implementations.
- Other network applications.

### Main security properties

#### Confidentiality

Encryption helps prevent unauthorized parties from reading transmitted information.

#### Integrity

Cryptographic protection helps detect unauthorized changes.

#### Authentication

Certificates and cryptographic handshakes help authenticate communication endpoints.

### TLS handshake overview

A simplified TLS connection involves:

1. Client sends a ClientHello.
2. Server responds with a ServerHello.
3. The parties negotiate cryptographic parameters.
4. The server presents its certificate when required.
5. The client validates the certificate.
6. Both sides establish session keys.
7. Encrypted communication begins.

Modern TLS versions use more sophisticated handshake mechanisms than this simplified explanation.

### TLS versions

TLS 1.2 and TLS 1.3 are widely used modern versions.

Older SSL protocols and obsolete TLS versions should not be enabled for modern secure services.

---

## 8. Secure Email Protocols

Email systems use several protocols.

| Protocol | Purpose | Common port |
|---|---|---:|
| SMTP | Sends email | 25 |
| SMTP submission with STARTTLS | Sends authenticated email securely | 587 |
| SMTP implicit TLS | Secure email submission | 465 |
| POP3 | Retrieves email | 110 |
| POP3S | Secure email retrieval | 995 |
| IMAP | Accesses and manages email | 143 |
| IMAPS | Secure email access | 993 |

### Important distinction

A secure connection to an email server does not guarantee end-to-end encryption of the email message.

Transport encryption protects communication between the client and server or between mail servers, depending on configuration.

---

## 9. Secure DNS Protocols

Traditional DNS commonly uses UDP or TCP port 53.

Two modern approaches protect DNS communication.

### DNS over HTTPS (DoH)

DoH sends DNS queries through HTTPS.

Common port:

```text
TCP 443
```

### DNS over TLS (DoT)

DoT encrypts DNS queries using TLS.

Common port:

```text
TCP 853
```

### Benefits

- Protects DNS queries against passive observation on the network path.
- Helps reduce certain forms of DNS manipulation.
- Improves privacy when properly configured.

### Limitations

Encrypted DNS does not make a device completely anonymous.

The resolver may still see queries, and other network metadata may remain visible.

---

## 10. VPN Protocols

A VPN creates a protected communication tunnel across another network.

Common VPN technologies include:

- IPsec.
- OpenVPN.
- WireGuard.

### IPsec

Provides network-layer security and is widely used in site-to-site VPNs.

### OpenVPN

Uses TLS-based cryptographic mechanisms to establish protected VPN connections.

### WireGuard

A modern VPN protocol designed with a relatively small codebase and modern cryptographic primitives.

### Why organizations use VPNs

- Secure remote access.
- Site-to-site communication.
- Protection over untrusted networks.
- Controlled access to internal resources.

A VPN does not automatically make an unsafe endpoint secure.

---

## 11. Secure Protocol Configuration

Using a secure protocol is only one part of network security.

Administrators should also:

1. Disable obsolete protocols where possible.
2. Use modern cryptographic settings.
3. Protect private keys.
4. Validate certificates.
5. Apply security updates.
6. Restrict administrative access.
7. Monitor listening services.
8. Use strong authentication.
9. Apply firewall policies.
10. Review protocol configurations regularly.

### Principle of least privilege

Only authorized users and systems should have access to network services.

### Defense in depth

Use multiple security controls rather than relying on one secure protocol.

---

## 12. Common Secure Protocol Ports

| Protocol | Common port | Purpose |
|---|---:|---|
| HTTP | TCP 80 | Web communication |
| HTTPS | TCP 443 | Secure web communication |
| SSH | TCP 22 | Secure remote access |
| FTP | TCP 21 | File transfer control |
| SFTP | TCP 22 | Secure file transfer over SSH |
| DNS | UDP/TCP 53 | Domain resolution |
| DoT | TCP 853 | Encrypted DNS |
| SMTP | TCP 25 | Email transfer |
| SMTP Submission | TCP 587 | Email submission |
| SMTPS | TCP 465 | Implicit TLS email submission |
| POP3 | TCP 110 | Email retrieval |
| POP3S | TCP 995 | Secure email retrieval |
| IMAP | TCP 143 | Email access |
| IMAPS | TCP 993 | Secure email access |

Ports are common defaults, not guarantees. Services can be configured to use different ports.

---

# 13. PRACTICAL LAB 1: IDENTIFY NETWORK SERVICES

## Objective

Inspect services and listening ports on your own Kali VM.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson09
cd ~/module15-lab/lesson09
```

### Step 2: Inspect listening TCP and UDP sockets

```bash
sudo ss -tulpen
```

Explanation:

| Option | Meaning |
|---|---|
| `-t` | TCP sockets |
| `-u` | UDP sockets |
| `-l` | Listening sockets |
| `-p` | Process information |
| `-e` | Extended socket information |
| `-n` | Numeric addresses and ports |

### Step 3: Check SSH availability

```bash
ssh -V
```

### Step 4: Inspect port 22

```bash
sudo ss -tulpen | grep ':22'
```

No output may simply mean that no service is listening on port 22.

### Step 5: Inspect network connections

```bash
ss -tun
```

### Step 6: Save the results

```bash
sudo ss -tulpen > listening-services.txt
ss -tun > network-connections.txt
ssh -V > ssh-version.txt 2>&1
```

---

# 14. PRACTICAL LAB 2: TEST HTTPS CERTIFICATE INFORMATION

## Objective

Inspect publicly presented HTTPS certificate information for example.com.

This is a read-only connection to a public example domain.

### Step 1: Check OpenSSL availability

```bash
openssl version
```

### Step 2: Inspect the TLS connection

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null
```

This establishes a TLS connection and displays certificate and handshake information.

### Step 3: Display certificate details

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

### Step 4: Understand the output

| Field | Meaning |
|---|---|
| Subject | Certificate identity |
| Issuer | Certificate authority |
| Not Before | Certificate validity start |
| Not After | Certificate expiration |

### Step 5: Save the certificate information

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -subject -issuer -dates > certificate-report.txt
```

A successful TLS connection does not, by itself, establish that an application is trustworthy.

---

# 15. PRACTICAL PROJECT: SECURE PROTOCOL AUDIT

Create:

```bash
nano secure-protocol-audit.txt
```

Use this template:

```text
SECURE NETWORK PROTOCOL AUDIT

1. ENVIRONMENT
Operating System:
Date:

2. LISTENING SERVICES
Service:
Port:
Protocol:
Purpose:

3. SECURITY REVIEW
HTTP Usage:
HTTPS Usage:
SSH Availability:
FTP Usage:
Secure File Transfer:
DNS Configuration:

4. TLS REVIEW
Certificate Subject:
Certificate Issuer:
Validity Start:
Validity End:

5. SECURITY RISKS
Cleartext Protocols:
Unnecessary Services:
Weak Authentication:
Expired or Invalid Certificates:

6. RECOMMENDATIONS
Prefer HTTPS:
Use SSH:
Prefer SFTP or FTPS:
Use Modern TLS:
Disable Obsolete Services:
Protect Private Keys:
Monitor Listening Ports:

7. CONCLUSION
```

Save the report.

---

## 16. Scenario-Based Exercise

Analyze the following situations.

For each one, identify the insecure protocol, the risk, and the appropriate secure alternative.

### Scenario A

A company administrator manages a remote Linux server using Telnet.

### Scenario B

An organization transfers confidential files using traditional FTP.

### Scenario C

A website accepts login information over HTTP.

### Scenario D

A company uses an obsolete TLS configuration on its public-facing web server.

### Scenario E

A remote employee needs secure access to internal company resources over an untrusted network.

---

## 17. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is a network protocol?
2. Differentiate between cleartext and encrypted protocols.
3. What is the difference between HTTP and HTTPS?
4. What is SSH?
5. Which port does SSH commonly use?
6. Differentiate between FTP, SFTP, and FTPS.
7. What does TLS stand for?
8. Mention three security properties provided by TLS.
9. Explain the purpose of a TLS certificate.
10. What is DNS over HTTPS?
11. What is DNS over TLS?
12. Differentiate between DoH and DoT.
13. What is IPsec?
14. Mention three common VPN technologies.
15. Why should obsolete protocols be disabled?
16. What is the principle of least privilege?
17. Why is certificate validation important?
18. What is the purpose of `ss -tulpen`?
19. What information can OpenSSL provide about a certificate?
20. Mention five secure network protocol best practices.

---

## 18. LESSON SUMMARY

In this lesson, you learned:

- What network protocols are.
- The difference between cleartext and encrypted communication.
- HTTP and HTTPS.
- SSH and secure remote administration.
- FTP, SFTP, and FTPS.
- TLS and certificate validation.
- Secure email protocols.
- DNS over HTTPS and DNS over TLS.
- IPsec, OpenVPN, and WireGuard.
- Common secure network ports.
- How to inspect listening services.
- How to inspect HTTPS certificate information.
- How to perform a secure protocol audit.

**KEY TAKEAWAY:**

Secure network protocols protect communication through encryption, authentication, and integrity mechanisms. Cybersecurity professionals should identify insecure services, replace obsolete protocols, validate certificates, and enforce secure configurations.

**END OF LESSON 09**