# MODULE 15 – NETWORK SECURITY
## LESSON 07: DNS SECURITY AND ATTACKS

---

## 1. Introduction

DNS stands for Domain Name System.

DNS is one of the most important services on the internet because it translates human-readable domain names into IP addresses.

For example, when you enter:

```text
google.com
```

Your computer needs to discover the IP address associated with that domain before establishing a connection.

DNS makes this possible.

However, attackers may target DNS services to redirect users, interrupt communication, or steal sensitive information.

In this lesson, we will study:

- How DNS works.
- DNS records and servers.
- DNS queries and responses.
- Common DNS attacks.
- DNS security mechanisms.
- DNS monitoring and troubleshooting.
- Practical defensive DNS inspection.

---

## 2. What Is DNS?

DNS means Domain Name System.

It is a distributed naming system that connects domain names to information such as IP addresses.

### Example

A domain name:

```text
example.com
```

May resolve to an IPv4 address such as:

```text
93.184.216.34
```

The exact IP address can change depending on the domain's configuration and DNS response.

### Why DNS is important

Without DNS, users would need to remember IP addresses for websites and other internet services.

DNS makes internet communication easier by allowing users to use names instead of numeric addresses.

---

## 3. How DNS Works

When you visit a website, DNS resolution generally follows these steps.

### Step 1: User enters a domain

```text
www.example.com
```

### Step 2: Computer checks local information

The operating system may check its DNS cache and local hosts file.

### Step 3: DNS resolver receives the query

If the answer is not already cached, the configured DNS resolver may need to find it.

### Step 4: DNS resolution takes place

The resolver may contact other DNS servers to obtain the required answer.

### Step 5: DNS response is returned

The resolver returns the requested information.

### Step 6: Browser connects to the destination

The browser uses the resolved IP address to establish communication with the destination server.

### DNS resolution diagram

```text
User
 |
 | Requests www.example.com
 |
 v
Operating System
 |
 | Checks local cache
 |
 v
DNS Resolver
 |
 | Resolves domain
 |
 v
DNS Server Hierarchy
 |
 | Returns DNS information
 |
 v
DNS Resolver
 |
 | Returns answer
 |
 v
User's Computer
 |
 | Connects to destination
 |
 v
Website Server
```

---

## 4. Understanding DNS Servers

DNS uses different types of servers.

### 4.1 DNS Resolver

A resolver receives DNS queries from client devices and obtains answers.

It may use cached information or contact other DNS servers.

### 4.2 Root DNS Server

Root servers direct resolvers toward the appropriate top-level domain servers.

### 4.3 TLD DNS Server

TLD means Top-Level Domain.

Examples:

- `.com`
- `.org`
- `.net`
- `.ng`
- `.edu`

TLD servers help direct resolvers toward authoritative DNS servers.

### 4.4 Authoritative DNS Server

An authoritative DNS server provides DNS records for the domains it manages.

It is an important source of official DNS information for a domain.

---

## 5. DNS Records

DNS records contain different types of information.

| Record | Purpose |
|---|---|
| A | Maps a domain to an IPv4 address |
| AAAA | Maps a domain to an IPv6 address |
| CNAME | Creates an alias for another domain name |
| MX | Identifies mail servers |
| NS | Identifies authoritative name servers |
| TXT | Stores text information |
| PTR | Supports reverse DNS lookup |
| SOA | Contains administrative information about a DNS zone |
| SRV | Identifies services and their connection details |

### Example DNS records

```text
example.com       A       192.0.2.10
example.com       AAAA    2001:db8::10
mail.example.com  A       192.0.2.20
example.com       MX      mail.example.com
```

The IP addresses above are documentation examples, not real service recommendations.

---

## 6. DNS Protocol and Ports

DNS commonly uses:

| Protocol | Port | Purpose |
|---|---:|---|
| UDP | 53 | Common DNS queries and responses |
| TCP | 53 | DNS transfers and other DNS communication requiring TCP |

DNS is not limited to UDP.

TCP is important for some DNS operations and may also be used when responses are too large for the relevant UDP exchange.

### DNS over TLS

DNS over TLS (DoT) encrypts DNS communication using TLS.

It commonly uses TCP port 853.

### DNS over HTTPS

DNS over HTTPS (DoH) sends DNS queries through HTTPS.

It commonly uses TCP port 443.

### Why encrypted DNS matters

Traditional DNS traffic can be observed by parties able to monitor the network path.

Encrypted DNS helps protect DNS queries from passive observation and some forms of interference.

However, encrypted DNS does not make all browsing activity anonymous.

---

## 7. Common DNS Attacks

### 7.1 DNS Spoofing

DNS spoofing occurs when false DNS information is supplied to influence the destination a user reaches.

For example, a user requests a legitimate website but receives an incorrect IP address.

### 7.2 DNS Cache Poisoning

DNS cache poisoning occurs when false DNS information is inserted into a resolver's cache.

Other users may receive the incorrect answer while that information remains cached.

### 7.3 DNS Hijacking

DNS hijacking involves unauthorized changes or manipulation of DNS resolution.

Possible targets include:

- Router DNS settings.
- Device DNS configuration.
- Registrar accounts.
- DNS hosting accounts.

### 7.4 DNS Tunneling

DNS tunneling uses DNS queries and responses to carry information that is not ordinary DNS data.

It may be used for unauthorized communication or data exfiltration.

Security teams monitor unusual DNS patterns to identify possible misuse.

### 7.5 DNS Amplification

DNS amplification is a technique used in distributed denial-of-service attacks.

Attackers may exploit publicly accessible DNS servers to generate large responses toward a victim.

### 7.6 DNS-Based Phishing

Attackers may register misleading domains that resemble legitimate websites.

Users may be tricked into entering credentials on fraudulent websites.

---

## 8. DNS Cache Poisoning Explained

Consider a user who wants to visit:

```text
bank.example
```

The DNS resolver should return the correct IP address.

However, if the resolver's cache is poisoned, it may return an attacker-controlled address.

### Normal resolution

```text
User
 |
 v
DNS Resolver
 |
 v
Correct IP Address
 |
 v
Legitimate Website
```

### Poisoned resolution

```text
User
 |
 v
DNS Resolver
 |
 v
Incorrect IP Address
 |
 v
Fraudulent Destination
```

### Possible consequences

- Phishing.
- Credential theft.
- Website impersonation.
- Malware delivery.
- Loss of user trust.

Modern DNS implementations use measures such as transaction ID randomization, source-port randomization, and DNSSEC validation to reduce poisoning risks.

---

## 9. DNSSEC

DNSSEC stands for Domain Name System Security Extensions.

It adds cryptographic authentication to DNS data.

Its purpose is to help resolvers verify that DNS records are authentic and have not been modified during the relevant DNS resolution process.

### Important DNSSEC records

| Record | Purpose |
|---|---|
| DNSKEY | Contains public key information |
| RRSIG | Contains a digital signature for a DNS record set |
| DS | Links a child zone to its parent zone |
| NSEC / NSEC3 | Helps authenticate the nonexistence of DNS names |

### How DNSSEC helps

DNSSEC helps protect against certain forms of DNS data manipulation.

### Important limitation

DNSSEC does not encrypt DNS queries.

It provides authenticity and integrity validation for signed DNS data, not confidentiality.

---

## 10. DNS Security Best Practices

Organizations should apply the following controls:

1. Use trusted DNS resolvers.
2. Keep DNS software updated.
3. Protect DNS administration accounts.
4. Enable MFA for DNS management platforms.
5. Use DNSSEC where appropriate.
6. Restrict unauthorized DNS zone transfers.
7. Monitor unusual DNS queries.
8. Use secure DNS configurations.
9. Maintain reliable DNS backups.
10. Investigate unexpected DNS configuration changes.

### Preventing unauthorized zone transfers

DNS zone transfers should only be allowed to authorized secondary DNS servers.

Unrestricted zone transfers can expose domain information.

---

## 11. DNS Monitoring and Investigation Tools

Cybersecurity professionals use several tools to investigate DNS behavior.

| Tool | Purpose |
|---|---|
| `dig` | Query DNS records |
| `nslookup` | Perform DNS lookups |
| `host` | Perform simple DNS lookups |
| `tcpdump` | Capture network packets |
| Wireshark | Analyze DNS traffic |
| `resolvectl` | Inspect systemd-resolved DNS configuration where available |

### Important commands

Check DNS resolution:

```bash
dig example.com
```

Request an A record:

```bash
dig example.com A
```

Request an AAAA record:

```bash
dig example.com AAAA
```

Request MX records:

```bash
dig example.com MX
```

Request NS records:

```bash
dig example.com NS
```

Request TXT records:

```bash
dig example.com TXT
```

Use nslookup:

```bash
nslookup example.com
```

Use host:

```bash
host example.com
```

Inspect resolver configuration:

```bash
cat /etc/resolv.conf
```

On systems using systemd-resolved:

```bash
resolvectl status
```

Some commands may not be installed or available depending on your Linux configuration.

---

## 12. Detecting Suspicious DNS Activity

Security analysts may investigate:

### Unusual Query Volume

A device repeatedly generating large numbers of DNS queries may require investigation.

### Suspicious Domains

Unexpected or newly registered domains may be associated with malicious activity.

### Unusual DNS Record Types

Unexpected TXT or other record usage may require investigation.

### Large DNS Queries

Unusually large DNS requests or responses may indicate suspicious activity.

### Unexpected Resolver Changes

A device suddenly using an unfamiliar DNS resolver may indicate unauthorized configuration changes.

### Repeated Failed Queries

Large volumes of failed DNS lookups may indicate misconfiguration, scanning, or malicious activity.

**Important:** These indicators are not proof of an attack. They require investigation and supporting evidence.

---

# 13. PRACTICAL LAB 1: DNS INFORMATION GATHERING

## Objective

Learn how to inspect DNS information using your Kali Linux VM.

All queries in this exercise use the public example.com domain.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson07
cd ~/module15-lab/lesson07
```

### Step 2: Check whether dig is available

```bash
command -v dig
```

If it is unavailable, you may use `nslookup` if installed.

### Step 3: Query an A record

```bash
dig example.com A
```

Identify:

- Query name.
- Query type.
- Answer section.
- Returned IP address.
- Query time.

### Step 4: Query an AAAA record

```bash
dig example.com AAAA
```

### Step 5: Query MX records

```bash
dig example.com MX
```

### Step 6: Query NS records

```bash
dig example.com NS
```

### Step 7: Query TXT records

```bash
dig example.com TXT
```

### Step 8: Inspect the DNS resolver

```bash
cat /etc/resolv.conf
```

If available:

```bash
resolvectl status
```

### Step 9: Save your results

```bash
dig example.com A > dns-a-record.txt
dig example.com AAAA > dns-aaaa-record.txt
dig example.com MX > dns-mx-record.txt
dig example.com NS > dns-ns-record.txt
dig example.com TXT > dns-txt-record.txt
```

List your files:

```bash
ls -lh
```

---

# 14. PRACTICAL LAB 2: DNS PACKET MONITORING

## Objective

Observe DNS packet metadata from your own Kali VM.

Do not capture or inspect other people's private traffic.

### Step 1: Check tcpdump availability

```bash
command -v tcpdump
```

### Step 2: Identify your active interface

```bash
ip addr
```

### Step 3: Capture a small number of DNS packets

Replace `eth0` with your actual interface.

```bash
sudo tcpdump -i eth0 -nn -c 10 port 53
```

This captures packets using DNS's traditional port 53.

### Step 4: Generate a DNS query

Open another terminal and run:

```bash
dig example.com
```

### Step 5: Save a short DNS capture

```bash
sudo tcpdump -i eth0 -nn -c 20 -w dns-capture.pcap port 53
```

Generate DNS queries in another terminal while the capture is running.

If no packets are captured, your DNS may be using encrypted DNS or another interface. That does not necessarily indicate a problem.

### Step 6: Inspect the capture

```bash
tcpdump -nn -r dns-capture.pcap
```

Record:

- Source IP.
- Destination IP.
- Source port.
- Destination port.
- DNS query name, if visible.

---

# 15. PRACTICAL LAB 3: DNS SECURITY REPORT

Create:

```bash
nano dns-security-report.txt
```

Use the following template:

```text
DNS SECURITY INSPECTION REPORT

1. LAB ENVIRONMENT
Operating System:
Network Interface:
Date:

2. DNS RESOLUTION
Test Domain:
A Record:
AAAA Record:
MX Record:
NS Record:
TXT Record:

3. DNS CONFIGURATION
Configured Resolver:
Resolver Configuration File:

4. TRAFFIC OBSERVATION
DNS Packets Observed:
Source IP:
Destination IP:
Query Information:

5. SECURITY OBSERVATIONS
Unexpected Resolver:
Unusual DNS Activity:
Configuration Concerns:

6. RECOMMENDATIONS
Use Trusted DNS:
Protect DNS Administration:
Monitor DNS Traffic:
Enable DNSSEC Where Appropriate:
Review DNS Configuration:

7. CONCLUSION
```

Save the report.

---

## 16. Important Distinctions

| Technology | Main purpose |
|---|---|
| DNS | Resolves domain names |
| DNSSEC | Authenticates DNS data |
| DoH | Encrypts DNS queries through HTTPS |
| DoT | Encrypts DNS queries through TLS |
| DHCP | Automatically provides network configuration |
| ARP | Resolves IPv4 addresses to local MAC addresses |

Remember:

- DNSSEC provides authentication and integrity, not encryption.
- DoH and DoT provide encryption for DNS communication.
- Traditional DNS commonly uses port 53.
- DNS attacks can affect both individuals and organizations.

---

# 17. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What does DNS stand for?
2. Why is DNS important?
3. Explain how DNS resolution works.
4. What is a DNS resolver?
5. What is an authoritative DNS server?
6. Differentiate between A and AAAA records.
7. What is the purpose of an MX record?
8. Which ports are commonly used by DNS?
9. Explain DNS spoofing.
10. What is DNS cache poisoning?
11. What is DNS hijacking?
12. Explain DNS tunneling.
13. What is DNS amplification?
14. What does DNSSEC do?
15. Does DNSSEC encrypt DNS queries?
16. Differentiate between DoH and DoT.
17. Mention five DNS security best practices.
18. What is the purpose of `dig`?
19. How can an analyst identify suspicious DNS activity?
20. Why should DNS administration accounts use MFA?

---

# 18. LESSON SUMMARY

In this lesson, you learned:

- The meaning and importance of DNS.
- How DNS resolution works.
- The different types of DNS servers.
- Common DNS records.
- DNS ports and protocols.
- DNS spoofing and cache poisoning.
- DNS hijacking and tunneling.
- DNS amplification.
- DNSSEC and encrypted DNS.
- DNS monitoring and investigation tools.
- How to perform defensive DNS inspection in Kali Linux.
- How to prepare a DNS security report.

**KEY TAKEAWAY:**

DNS is essential for internet communication, but attackers may manipulate DNS information to redirect users or disrupt services. Understanding DNS security, DNSSEC, encrypted DNS, and traffic monitoring helps cybersecurity professionals protect users and network infrastructure.

**END OF LESSON 07**