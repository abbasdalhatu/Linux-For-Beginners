
# MODULE 10 – NETWORK ENUMERATION

# LESSON 06: DNS AND NETWORK ENUMERATION

## 1. Introduction

DNS stands for Domain Name System.

DNS translates human-readable domain names into IP addresses and provides other information about domain services.

For example:

```text
example.com → IP Address
```

Computers use IP addresses to communicate across networks, while humans generally find domain names easier to remember.

DNS enumeration is the process of collecting and analyzing DNS information within an authorized assessment.

It can help identify:

- Domain names.
- IP addresses.
- Mail servers.
- Name servers.
- DNS record types.
- DNS configuration information.
- Potential DNS misconfigurations.

## 2. What Is DNS?

DNS is a distributed naming system used to locate resources on computer networks.

Imagine a phone contact list:

- A person's name is easy to remember.
- A phone number is used to reach that person.

DNS works in a similar way:

- Domain name: `example.com`
- IP address: the address associated with that domain.

DNS makes internet communication easier.

## 3. How DNS Resolution Works

A simplified DNS lookup works as follows:

```text
User enters a domain name
          |
          v
Computer checks local cache
          |
          v
DNS resolver receives query
          |
          v
Resolver obtains DNS information
          |
          v
IP address is returned
          |
          v
Computer connects to destination
```

A resolver may answer from its cache or perform additional DNS lookups.

## 4. Common DNS Record Types

DNS contains different record types.

| Record | Purpose |
|---|---|
| A | Maps a name to an IPv4 address |
| AAAA | Maps a name to an IPv6 address |
| MX | Identifies mail servers |
| NS | Identifies authoritative name servers |
| CNAME | Defines a canonical name alias |
| TXT | Stores text information |
| PTR | Supports reverse DNS lookup |
| SOA | Contains zone authority information |

### Example

Illustrative DNS records:

```text
example.com       A       192.0.2.10
example.com       AAAA    2001:db8::10
example.com       MX      mail.example.com
example.com       NS      ns1.example.com
```

The addresses shown are documentation examples, not real discovered records.

## 5. What Is DNS Enumeration?

DNS enumeration gathers DNS information to understand how a domain or authorized DNS environment is configured.

Examples include:

1. Identifying A records.
2. Identifying AAAA records.
3. Finding mail servers.
4. Identifying name servers.
5. Examining TXT records.
6. Performing reverse DNS lookups.
7. Investigating DNS responses and errors.

DNS enumeration can help security teams identify configuration mistakes and unnecessary information exposure.

## 6. DNS Tools

| Tool | Purpose |
|---|---|
| dig | Detailed DNS queries |
| nslookup | DNS lookup and troubleshooting |
| host | Simple DNS queries |
| whois | Domain registration information where available |
| Wireshark | DNS packet analysis |
| TShark | Command-line DNS analysis |

We will primarily use `dig`, `nslookup`, and `host`.

## 7. Practical Lab 1: Create Your Workspace

Run:

```bash
mkdir -p ~/practical-labs/module10/lab06
cd ~/practical-labs/module10/lab06
```

Check whether DNS utilities are available:

```bash
dig -v
```

```bash
nslookup -version
```

```bash
host -V
```

If `dig` or `host` is missing, install the DNS utilities package from Kali's official repositories:

```bash
sudo apt update
sudo apt install dnsutils
```

## 8. Practical Lab 2: Perform Your First DNS Lookup

Use the reserved example domain:

```bash
dig example.com
```

Inspect the output.

Important fields:

- QUESTION SECTION
- ANSWER SECTION
- AUTHORITY SECTION
- ADDITIONAL SECTION
- Query time
- SERVER
- Query status

### Understanding the Status

| Status | Meaning |
|---|---|
| NOERROR | Query completed without a DNS error |
| NXDOMAIN | Requested domain name does not exist |
| SERVFAIL | DNS server could not complete the query |
| REFUSED | DNS server refused the request |

An empty answer section does not always mean a domain is invalid. The record type and response status matter.

## 9. Practical Lab 3: Identify A and AAAA Records

Query the IPv4 record:

```bash
dig example.com A
```

Query the IPv6 record:

```bash
dig example.com AAAA
```

Compare the responses.

Questions:

1. What is an A record?
2. What is an AAAA record?
3. Did both queries return answers?
4. Why might a domain have an A record but no AAAA record?

Save the results:

```bash
dig example.com A > dns-a-record.txt
```

```bash
dig example.com AAAA > dns-aaaa-record.txt
```

## 10. Practical Lab 4: Identify Mail and Name Servers

Query MX records:

```bash
dig example.com MX
```

Query NS records:

```bash
dig example.com NS
```

Query TXT records:

```bash
dig example.com TXT
```

Observe:

- Mail server names.
- Authoritative name server names.
- TXT information.
- TTL values where displayed.

Save the results:

```bash
dig example.com MX > dns-mx-records.txt
```

```bash
dig example.com NS > dns-ns-records.txt
```

```bash
dig example.com TXT > dns-txt-records.txt
```

## 11. Practical Lab 5: Use nslookup

`nslookup` is another tool for querying DNS servers.

Run:

```bash
nslookup example.com
```

Query a particular record:

```bash
nslookup -type=MX example.com
```

Query a name server record:

```bash
nslookup -type=NS example.com
```

Compare the output with `dig`.

### Key Difference

- `dig` provides detailed DNS response information.
- `nslookup` provides a simpler interface for DNS queries.

## 12. Practical Lab 6: Use the host Command

The `host` command performs DNS lookups.

Run:

```bash
host example.com
```

Check mail records:

```bash
host -t MX example.com
```

Check name servers:

```bash
host -t NS example.com
```

Perform reverse DNS lookup using an IP address obtained from your own authorized DNS query:

```bash
host IP_ADDRESS
```

Replace `IP_ADDRESS` with the actual address you want to investigate.

A reverse lookup may not exist or may return a different hostname.

## 13. Practical Lab 7: Understand DNS Responses with Wireshark

Open Wireshark:

```bash
wireshark
```

Select your active authorized network interface.

Start capturing traffic.

In another terminal, generate a DNS query:

```bash
dig example.com
```

Return to Wireshark and apply:

```text
dns
```

Inspect:

- Source IP.
- Destination IP.
- Query name.
- Query type.
- Transaction ID.
- Response code.
- Answer records.

Useful filters:

Display DNS queries:

```text
dns.flags.response == 0
```

Display DNS responses:

```text
dns.flags.response == 1
```

Display A-record queries:

```text
dns.qry.type == 1
```

Display AAAA-record queries:

```text
dns.qry.type == 28
```

Display unsuccessful DNS responses:

```text
dns.flags.rcode != 0
```

Save your capture as:

```text
dns-enumeration.pcapng
```

Note: DNS over HTTPS and DNS over TLS can hide ordinary DNS query details from a normal DNS packet display. The capture interface and resolver configuration also affect what you see.

## 14. Practical Lab 8: DNS Troubleshooting

### Problem A: DNS Lookup Fails

Run:

```bash
cat /etc/resolv.conf
```

Inspect the configured resolver information.

Test local name resolution:

```bash
getent hosts example.com
```

Compare it with:

```bash
dig example.com
```

### Problem B: NXDOMAIN

Run a query for a deliberately nonexistent subdomain under the reserved example domain:

```bash
dig definitely-not-a-real-hostname.example.com
```

Observe the response status.

This demonstrates how DNS can report that a queried name does not exist.

### Problem C: SERVFAIL

A SERVFAIL response indicates that the DNS server could not complete the query.

Possible causes include:

- Resolver problems.
- Upstream DNS failures.
- DNSSEC validation problems.
- Authoritative server issues.

Do not assume every SERVFAIL response indicates an attack.

## 15. Practical Lab 9: Understand DNS Security

DNS is important to cybersecurity because attackers may abuse DNS for different purposes.

Examples include:

### A. DNS Spoofing

An attacker attempts to provide false DNS information.

### B. DNS Cache Poisoning

Incorrect DNS records are introduced into a resolver's cache.

### C. DNS Tunneling

DNS queries and responses are abused to carry data.

### D. DNS Amplification

DNS infrastructure may be abused in reflection and amplification attacks.

### E. Unauthorized Zone Transfers

A DNS server may incorrectly allow unauthorized clients to obtain zone information.

### Defensive Measures

- Restrict DNS zone transfers.
- Keep DNS software updated.
- Monitor unusual DNS activity.
- Use trusted resolvers.
- Apply DNSSEC validation where appropriate.
- Review DNS server access controls.

This lesson focuses on detection and defensive understanding, not attacking real DNS infrastructure.

## 16. Practical Lab 10: Create a DNS Enumeration Report

Create your report:

```bash
nano dns-enumeration-report.txt
```

Use this template:

```text
========================================
DNS ENUMERATION REPORT
========================================

1. ASSESSMENT INFORMATION

Date:
Domain:
Scope:

2. A RECORD

Record:
IPv4 Address:
Observations:

3. AAAA RECORD

Record:
IPv6 Address:
Observations:

4. MX RECORD

Mail Servers:
Priority:
Observations:

5. NS RECORD

Name Servers:
Observations:

6. TXT RECORD

TXT Information:
Observations:

7. REVERSE DNS

IP Address:
Returned Hostname:
Result:

8. DNS PACKET ANALYSIS

Query Name:
Query Type:
Transaction ID:
Response Code:
Source IP:
Destination IP:

9. SECURITY OBSERVATIONS

Configuration Issues:
Unusual Responses:
Potential Risks:

10. RECOMMENDATIONS

Recommendation 1:
Recommendation 2:
Recommendation 3:

11. CONCLUSION

Summary:

========================================
END OF REPORT
========================================
```

Use actual results. Do not invent records or claim a security problem without evidence.

## 17. Knowledge Check

Answer these questions:

1. What does DNS stand for?
2. What is DNS enumeration?
3. What is an A record?
4. What is an AAAA record?
5. What is an MX record?
6. What is an NS record?
7. What is a TXT record?
8. What is a PTR record?
9. What does NXDOMAIN mean?
10. What is the difference between `dig` and `nslookup`?
11. What is DNS cache poisoning?
12. What is DNS tunneling?
13. Why is DNS monitoring important?
14. What is the purpose of DNSSEC?

## 18. Lesson Completion Checklist

- [ ] Understand DNS fundamentals.
- [ ] Understand DNS resolution.
- [ ] Understand common DNS record types.
- [ ] Install and verify DNS tools.
- [ ] Perform A-record queries.
- [ ] Perform AAAA-record queries.
- [ ] Identify MX records.
- [ ] Identify NS records.
- [ ] Inspect TXT records.
- [ ] Use `dig`.
- [ ] Use `nslookup`.
- [ ] Use `host`.
- [ ] Analyze DNS traffic in Wireshark.
- [ ] Understand common DNS security threats.
- [ ] Create a DNS enumeration report.

## 19. Lesson Summary

DNS enumeration helps security professionals understand how domain names and network services are configured.

Remember:

- DNS translates domain names into network information.
- A records identify IPv4 addresses.
- AAAA records identify IPv6 addresses.
- MX records identify mail servers.
- NS records identify name servers.
- TXT records contain text information.
- PTR records support reverse lookups.
- `dig`, `nslookup`, and `host` are useful DNS tools.
- Wireshark helps investigate DNS traffic.

**NEXT LESSON: SMB AND WINDOWS SERVICE ENUMERATION.**
