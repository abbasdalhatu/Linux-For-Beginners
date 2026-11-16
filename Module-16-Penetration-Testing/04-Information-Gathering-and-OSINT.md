# MODULE 16 – PENETRATION TESTING
## LESSON 04: INFORMATION GATHERING AND OSINT

## 1. Introduction

Information gathering is the process of collecting relevant information about a target before conducting a security assessment.

It helps a penetration tester understand:
- What systems exist.
- What technologies are being used.
- Which assets are important.
- What potential security risks may exist.
- Where further authorized assessment may be useful.

**OSINT** stands for Open-Source Intelligence.

OSINT is the collection and analysis of information from publicly available sources.

Examples include:
- Search engines.
- Public websites.
- Official documentation.
- Public DNS records.
- Publicly available technical reports.
- Public code repositories.
- Public company information.

Publicly available information is not automatically permission to test the systems it describes.

---

## 2. Types of Information Gathering

Information gathering is commonly divided into two categories.

### A. Passive Information Gathering

Passive information gathering involves collecting information without directly interacting with the target's systems in a way that probes them.

Examples:
- Reviewing public websites.
- Reading published documentation.
- Searching public DNS information.
- Reviewing public security advisories.
- Examining publicly available company information.

**Example:** Reading an organization's published technology documentation.

### B. Active Information Gathering

Active information gathering involves directly interacting with an authorized target to collect information.

Examples:
- Checking a permitted test server's response.
- Performing approved network discovery.
- Identifying services on a lab machine.
- Reviewing application responses.

Active gathering may create logs or alerts and must remain within the approved scope.

### Comparison

| Passive | Active |
|---|---|
| Uses publicly available information | Directly interacts with the target |
| Usually lower operational impact | May generate network traffic |
| May not alert the target | May be logged or detected |
| Example: Reviewing public documentation | Example: Checking an authorized lab service |

---

## 3. Common OSINT Sources

### 3.1 Search Engines

Search engines can reveal publicly indexed information.

Examples:
- Google.
- Bing.
- DuckDuckGo.

They can help researchers discover:
- Official websites.
- Public documents.
- Technology announcements.
- Security advisories.
- Public support information.

Use search engines responsibly. Do not attempt to access private information or bypass authentication.

### 3.2 WHOIS

WHOIS is a protocol and service used to retrieve domain registration information.

Depending on privacy settings and registry policies, information may include:
- Domain registrar.
- Registration dates.
- Nameservers.
- Domain status.
- Registrant information, when publicly available.

Example:

```bash
whois example.com
```

The command may not be installed by default in Kali.

WHOIS data is not always complete or accurate, and privacy protection may hide registrant details.

### 3.3 DNS Information

DNS stands for Domain Name System.

It translates domain names into information such as IP addresses and mail-server destinations.

Common DNS record types:

| Record | Purpose |
|---|---|
| A | Maps a hostname to an IPv4 address |
| AAAA | Maps a hostname to an IPv6 address |
| MX | Identifies mail servers |
| NS | Identifies authoritative nameservers |
| CNAME | Creates an alias for another hostname |
| TXT | Stores text information, often used for verification and email security |

Example of querying a public DNS resolver:

```bash
dig example.com
```

To query specific record types:

```bash
dig example.com A
dig example.com MX
dig example.com NS
dig example.com TXT
```

These examples use a documentation domain. DNS queries do not authorize further testing of the resulting systems.

### 3.4 Public Code Repositories

Developers sometimes publish code on platforms such as GitHub.

Public repositories may contain:
- Application source code.
- Configuration examples.
- Software documentation.
- Dependency information.
- Security fixes.

Security professionals may review their own organization's repositories for accidentally committed secrets.

Never use exposed credentials or access tokens found in another person's repository.

### 3.5 Security Advisories

Security advisories describe known weaknesses in software and systems.

Useful resources include:
- National Vulnerability Database (NVD).
- CVE records.
- Vendor security advisories.
- CERT security bulletins.

These resources help testers understand whether a software version may be affected by a known vulnerability.

---

## 4. Understanding IP Addresses and Domains

An IP address identifies a network interface within an IP network.

Two major versions exist:

- IPv4: 32-bit addressing.
- IPv6: 128-bit addressing.

Examples:

```text
IPv4: 192.0.2.10
IPv6: 2001:db8::10
Domain: example.com
```

The IP addresses above are documentation examples.

A domain name may resolve to multiple IP addresses, and an IP address may host multiple websites.

Therefore, domain names and IP addresses are related but not interchangeable.

---

## 5. Subdomains

A subdomain is a domain name that exists beneath another domain.

Example:

```text
example.com
mail.example.com
support.example.com
training.example.com
```

Organizations may use subdomains for different services.

During an authorized assessment, the tester should confirm whether each discovered subdomain is included in the engagement scope.

Finding a subdomain does not automatically authorize testing it.

---

## 6. Technology Fingerprinting

Technology fingerprinting is the process of identifying technologies used by a system or application.

Examples include:
- Web server software.
- Operating system information.
- Programming languages.
- Application frameworks.
- Database technologies.
- Content management systems.

This information helps testers understand the target environment.

However, identifying a technology does not prove that it is vulnerable.

Software version information can also be incomplete, hidden, or misleading.

---

## 7. Common OSINT Tools

| Tool | Purpose |
|---|---|
| WHOIS | Domain registration information |
| DIG | DNS queries |
| Nslookup | DNS information lookup |
| TheHarvester | Authorized public information gathering |
| Maltego | Visual relationship analysis |
| Shodan | Search engine for internet-connected devices |
| SpiderFoot | OSINT automation and analysis |
| Recon-ng | Modular reconnaissance framework |

Use these tools only for lawful research and approved assessment objectives. Do not turn information gathering into unauthorized probing.

---

## 8. Understanding the Information-Gathering Process

A structured information-gathering process can follow these steps:

### Step 1: Define the Target

Identify the exact domain, application, or system covered by the assessment.

### Step 2: Confirm Authorization

Check that the target is included in the written scope.

### Step 3: Identify Information Sources

Select relevant public documentation, DNS information, and security advisories.

### Step 4: Collect Information

Gather the required information using permitted methods.

### Step 5: Verify Information

Check whether the information is accurate, current, and relevant.

### Step 6: Organize Findings

Record observations in a structured format.

### Step 7: Identify Next Steps

Determine which observations require further authorized assessment.

---

## 9. Practical Exercise – Basic OSINT and DNS Research

This exercise uses documentation domains and public information only. It does not involve scanning or exploiting a real organization.

### Step 1: Create your workspace

```bash
mkdir -p ~/module16-lab/lesson04
cd ~/module16-lab/lesson04
```

### Step 2: Check whether DNS tools are available

```bash
command -v dig
command -v nslookup
```

If `dig` is unavailable, you can install its package in Kali:

```bash
sudo apt update
sudo apt install dnsutils
```

### Step 3: Query documentation-domain DNS records

```bash
dig example.com A
```

Inspect the output.

Look for:
- QUESTION SECTION.
- ANSWER SECTION.
- Query time.
- Server information.

Next, examine other record types:

```bash
dig example.com MX
dig example.com NS
dig example.com TXT
```

### Step 4: Use nslookup

```bash
nslookup example.com
```

Compare the information returned by `dig` and `nslookup`.

### Step 5: Create an OSINT notes file

```bash
nano osint-notes.txt
```

Enter:

```text
OSINT RESEARCH NOTES

Research Subject:
Documentation Domain - example.com

Research Type:
Passive/Public Information Research

DNS Information:
IPv4 Record:
Mail Exchange Record:
Nameserver Record:
TXT Record:

Information Sources:
DNS lookup tools.

Observations:
Record what the DNS queries returned.

Limitations:
DNS records may change.
Public information may be incomplete.
DNS information does not establish authorization
to test the resulting systems.

Conclusion:
Summarize what was learned.
```

Save with:
- CTRL + O
- ENTER
- CTRL + X

### Step 6: Review your notes

```bash
cat osint-notes.txt
```

Fill in the fields using your actual DNS query results.

---

## 10. Responsible OSINT Practices

OSINT can create privacy and security concerns if handled carelessly.

Follow these principles:

1. Use lawful, publicly available sources.
2. Respect privacy and applicable data protection rules.
3. Do not attempt to access private accounts.
4. Do not use exposed credentials.
5. Do not publish personal information unnecessarily.
6. Verify information before treating it as fact.
7. Record the source and date of important observations.
8. Keep research within the approved assessment purpose.

---

## 11. Knowledge Check

**Question 1:** What does OSINT stand for?

A. Open System Internet Network Technology  
B. Open-Source Intelligence  
C. Operating System Internal Network Testing  
D. Online Security Investigation Network Tool  

**Question 2:** Which is an example of passive information gathering?

A. Reviewing public documentation  
B. Exploiting a server  
C. Modifying a database  
D. Attempting unauthorized access  

**Question 3:** Which DNS record identifies mail servers?

A. A  
B. MX  
C. NS  
D. AAAA  

**Question 4:** What is the purpose of WHOIS?

A. Encrypting files  
B. Retrieving domain registration information  
C. Capturing keyboard input  
D. Managing Linux processes  

**Question 5:** Does discovering a subdomain automatically authorize penetration testing against it?

A. Yes  
B. No  

---

## 12. Practical Thinking Questions

Answer these in your notebook:

1. Define information gathering.
2. Explain the difference between passive and active information gathering.
3. What is OSINT?
4. Explain the purpose of WHOIS.
5. What is DNS, and why is it important during reconnaissance?
6. Explain the difference between A, AAAA, MX, NS, CNAME, and TXT records.
7. What is technology fingerprinting?
8. Why must discovered subdomains be checked against the approved scope?
9. Name five OSINT tools and explain their purposes.
10. Why should OSINT findings be verified before being included in a security report?

---

## 13. Lesson Summary

In this lesson, you learned:

- Information gathering and its purpose.
- Passive and active information gathering.
- Open-Source Intelligence (OSINT).
- WHOIS and DNS research.
- DNS record types.
- Domains and subdomains.
- Technology fingerprinting.
- Common OSINT tools.
- Responsible information gathering.
- How to document basic OSINT findings.

### Key Principle

**Good penetration testing begins with good information gathering. Collect relevant information, verify it, respect privacy, and never confuse discovering a target with having permission to test it.**

**End of Lesson 04 – Information Gathering and OSINT**