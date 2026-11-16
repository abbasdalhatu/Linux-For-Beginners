
# RECONNAISSANCE AND OSINT FUNDAMENTALS

## 1. What Is Reconnaissance?

Reconnaissance is the process of gathering information about a target before a cybersecurity assessment.

Security professionals use reconnaissance to understand:
- The systems an organization operates.
- Its publicly available digital assets.
- Its domain names and network infrastructure.
- Potential security weaknesses.

Reconnaissance is an early stage of ethical hacking.

## 2. What Is OSINT?

OSINT means Open-Source Intelligence.

It is the collection and analysis of information from publicly available sources.

Examples include:
- Public websites.
- Search engines.
- Public DNS records.
- Domain registration information.
- Public technical documentation.
- Published security reports.

OSINT does not mean breaking into systems or accessing private information.

## 3. Types of Reconnaissance

### Passive Reconnaissance

Passive reconnaissance collects information without directly interacting with the target's systems.

Examples:
- Reading public websites.
- Searching public documents.
- Reviewing published DNS information.
- Examining publicly available domain records.

### Active Reconnaissance

Active reconnaissance involves directly interacting with a target system.

Examples:
- Network scanning.
- Checking whether a server responds.
- Identifying open ports.
- Running authorized service discovery.

Active reconnaissance requires explicit permission because it can generate traffic and affect systems.

## 4. Common OSINT Sources

| Source | Information |
|---|---|
| Search engines | Publicly indexed information |
| Official websites | Organization information |
| WHOIS/RDAP | Domain registration details |
| DNS records | Domain and mail infrastructure |
| Certificate Transparency | Publicly logged TLS certificates |
| Public repositories | Publicly shared code and documentation |
| Security advisories | Known vulnerabilities and patches |

Always respect privacy, website terms, and applicable laws.

## 5. Important Domain and DNS Concepts

A domain name is a human-readable name used to access an internet resource.

Examples:
- example.com
- university.edu

Common DNS record types:

- A: Maps a domain to an IPv4 address.
- AAAA: Maps a domain to an IPv6 address.
- MX: Identifies mail servers.
- NS: Identifies authoritative name servers.
- TXT: Stores text information, often used for verification and email security.
- CNAME: Creates an alias for another domain name.

## 6. Useful Windows CMD Commands

Display DNS information:

```cmd
nslookup example.com
```

Display mail server records:

```cmd
nslookup -type=MX example.com
```

Display name server records:

```cmd
nslookup -type=NS example.com
```

Display your computer's network configuration:

```cmd
ipconfig /all
```

Display your computer's DNS cache:

```cmd
ipconfig /displaydns
```

These commands are useful for learning DNS and troubleshooting your own network.

## 7. Common Reconnaissance Tools

### WHOIS and RDAP
Used to examine publicly available domain registration information.

### Nslookup
Used to query DNS records.

### Dig
A command-line tool for detailed DNS queries, commonly available on Linux.

### Nmap
Used for authorized network discovery and service identification.

### Wireshark
Used to capture and analyze network traffic.

### theHarvester
A tool for collecting publicly available information about domains and organizations. Use it only within an authorized assessment.

## 8. Responsible Reconnaissance Rules

Before conducting reconnaissance:

1. Obtain written authorization when assessing an organization.
2. Define the permitted domains, IP addresses, and systems.
3. Respect the assessment scope.
4. Avoid collecting unnecessary personal information.
5. Do not attempt unauthorized access.
6. Keep collected information secure.
7. Report findings responsibly.

Public availability does not automatically mean information can be used for any purpose.

## 9. Practical Learning Exercises

### Exercise 1: DNS Lookup

Run:

```cmd
nslookup example.com
```

Observe the DNS server and the returned address.

### Exercise 2: Mail Server Lookup

Run:

```cmd
nslookup -type=MX example.com
```

Observe the mail exchange records.

### Exercise 3: Network Configuration

Run:

```cmd
ipconfig /all
```

Identify:
- IPv4 address.
- Default gateway.
- DNS servers.
- Network adapter name.

Only share network information when necessary, and avoid publishing private details.

## 10. Summary

Reconnaissance is the information-gathering stage of ethical hacking.

OSINT uses publicly available information to understand digital assets and potential security risks.

The two main types are passive and active reconnaissance.

Security professionals must obtain authorization, respect privacy, and stay within the defined assessment scope.
