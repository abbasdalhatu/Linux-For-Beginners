---
---

# Module 10 – Lesson 08: DNS and Name Resolution

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand DNS and its importance.
2. Explain how domain names are translated into IP addresses.
3. Understand DNS servers and resolvers.
4. Identify common DNS record types.
5. Understand Linux DNS configuration.
6. Use Linux commands to investigate DNS.
7. Troubleshoot common DNS problems.
8. Understand DNS security risks.

---

## 2. What Is DNS?

DNS stands for Domain Name System.

It is a distributed naming system that translates human-readable domain names into IP addresses.

Computers communicate across IP networks using IP addresses, but humans generally find domain names easier to remember.

For example:

```text
google.com
```

A computer needs an IP address to communicate with the destination.

DNS helps resolve the domain name to an appropriate IP address.

### Simple Analogy

Think of DNS as a telephone directory.

| Telephone System | DNS |
|---|---|
| Person's name | Domain name |
| Phone number | IP address |
| Telephone directory | DNS system |
| Looking up a contact | DNS resolution |

You search for a person's name instead of memorizing their telephone number.

Similarly, DNS allows you to use domain names instead of remembering IP addresses.

---

## 3. Why DNS Is Important

DNS makes the internet easier to use.

Without DNS, users would need to remember IP addresses for websites and services.

DNS supports:

- Website access.
- Email delivery.
- Application communication.
- Network service discovery.
- Internal organizational networks.
- Cloud infrastructure.

For example:

```text
www.example.com
```

DNS helps a computer discover the IP address associated with that hostname.

---

## 4. How DNS Resolution Works

DNS resolution is the process of finding the IP address associated with a domain name.

### Step-by-Step Process

1. A user enters a domain name into a browser.
2. The computer checks relevant local caches and configured name-resolution sources.
3. If necessary, a DNS resolver receives the query.
4. The resolver obtains the required DNS information, potentially consulting other DNS servers.
5. The resolver returns the answer.
6. The computer can use the resulting IP address to communicate with the destination.

### DNS Resolution Diagram

```text
        User enters example.com
                  |
                  v
          Local name lookup
                  |
                  v
           DNS Resolver
                  |
                  v
          DNS Infrastructure
                  |
                  v
          DNS Answer Returned
                  |
                  v
          IP Address Obtained
                  |
                  v
       Computer Connects to Server
```

A recursive resolver may use cached information or query the DNS hierarchy to obtain an answer.

---

## 5. Understanding the DNS Hierarchy

DNS is organized hierarchically.

Its major components include:

### 5.1 Root DNS Servers

Root servers provide information about the authoritative servers responsible for top-level domains.

Examples of top-level domains include:

- `.com`
- `.org`
- `.net`
- `.ng`

### 5.2 Top-Level Domain Servers

TLD servers provide referrals to authoritative DNS servers for domains under their top-level domain.

Example:

```text
example.com
```

The `.com` DNS infrastructure helps direct queries toward the appropriate authoritative servers.

### 5.3 Authoritative DNS Servers

An authoritative DNS server stores DNS records for a domain or zone.

It provides authoritative answers about that domain.

### 5.4 Recursive DNS Resolvers

A recursive resolver receives DNS requests from client devices and obtains answers on their behalf.

Examples of public DNS resolver services include:

| Provider | IPv4 Address |
|---|---|
| Google Public DNS | 8.8.8.8 |
| Google Public DNS | 8.8.4.4 |
| Cloudflare DNS | 1.1.1.1 |
| Cloudflare DNS | 1.0.0.1 |

These are examples of public DNS resolver addresses, not authoritative servers for every domain.

---

## 6. Understanding DNS Record Types

DNS records contain information about domains and services.

Important record types include:

| Record | Purpose |
|---|---|
| A | Maps a hostname to an IPv4 address |
| AAAA | Maps a hostname to an IPv6 address |
| CNAME | Creates an alias for another domain name |
| MX | Identifies mail servers for a domain |
| NS | Identifies authoritative name servers |
| TXT | Stores text information, often used for verification and email security |
| PTR | Supports reverse DNS lookups |
| SOA | Contains administrative information about a DNS zone |
| SRV | Identifies servers providing particular services |

### Example: A Record

```text
example.com → 93.184.216.34
```

This is an illustrative example of an IPv4 mapping.

### Example: AAAA Record

```text
example.com → 2001:db8::10
```

This illustrates an IPv6 mapping. The `2001:db8::/32` range is reserved for documentation and is not a normal public destination.

### Example: MX Record

```text
example.com → mail.example.com
```

An MX record identifies mail servers responsible for receiving email for a domain.

---

## 7. Understanding DNS Caching

DNS caching temporarily stores DNS information so that repeated queries can be answered more efficiently.

Caching may occur in:

- Applications and browsers.
- The operating system.
- Local DNS services.
- Recursive DNS resolvers.

### Benefits of DNS Caching

1. Reduces repeated DNS queries.
2. Improves response times.
3. Reduces DNS traffic.
4. Improves efficiency.

### Understanding TTL in DNS

DNS records have a TTL value that determines how long a cached record may be retained.

This is different from the IPv4 packet TTL studied in Lesson 07.

**Important distinction:**

| IPv4 Packet TTL | DNS Record TTL |
|---|---|
| Limits packet forwarding hops | Controls DNS cache lifetime |
| Part of the IP header | Part of DNS record information |
| Reduced by routers | Counts down in DNS caches |

---

## 8. Linux DNS Configuration

Linux uses several configuration files and system services for name resolution.

### 8.1 Understanding `/etc/resolv.conf`

This file contains DNS resolver configuration.

Inspect it using:

```bash
cat /etc/resolv.conf
```

Example:

```text
nameserver 192.168.100.1
```

This indicates that the system is configured to use that DNS server.

Your actual configuration may differ.

### 8.2 Understanding `/etc/hosts`

The hosts file allows local hostname-to-IP mappings.

Inspect it using:

```bash
cat /etc/hosts
```

Example:

```text
127.0.0.1 localhost
127.0.1.1 kali
```

A manually configured entry can provide a local name mapping without querying an external DNS server.

### 8.3 Understanding `/etc/nsswitch.conf`

This file controls the order in which certain name-resolution sources are consulted.

Inspect it using:

```bash
grep '^hosts:' /etc/nsswitch.conf
```

An example entry may look like:

```text
hosts: files dns
```

This indicates that local files are consulted before DNS in that example configuration.

---

## 9. Linux DNS Investigation Commands

### 9.1 Using `getent hosts`

```bash
getent hosts example.com
```

This performs hostname lookup through the system's configured name-service mechanisms.

### 9.2 Using `nslookup`

```bash
nslookup example.com
```

Displays DNS lookup information.

If it is not installed:

```bash
sudo apt update
sudo apt install dnsutils
```

### 9.3 Using `dig`

`dig` stands for Domain Information Groper.

It is a powerful DNS investigation utility.

Basic lookup:

```bash
dig example.com
```

Display only the answer section:

```bash
dig +short example.com
```

Query an A record:

```bash
dig example.com A
```

Query an AAAA record:

```bash
dig example.com AAAA
```

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

Query a particular DNS resolver:

```bash
dig @1.1.1.1 example.com
```

This sends the DNS query to the specified resolver.

### 9.4 Using `host`

```bash
host example.com
```

This provides a simple DNS lookup.

---

## 10. Understanding Reverse DNS

Normal DNS resolution maps a hostname to an IP address.

Reverse DNS attempts to find a hostname associated with an IP address.

For IPv4, reverse DNS uses the `in-addr.arpa` domain.

For IPv6, it uses the `ip6.arpa` domain.

Example:

```bash
dig -x 8.8.8.8
```

This requests the PTR record associated with the address.

Reverse DNS information may be missing or may not correspond to a useful hostname.

---

## 11. Common DNS Problems

### 11.1 DNS Server Unavailable

The configured resolver may be unreachable.

Possible symptoms:

- Websites fail to resolve.
- `dig` times out.
- `getent hosts` returns no useful result.

Inspect:

```bash
cat /etc/resolv.conf
```

Test the resolver:

```bash
dig @1.1.1.1 example.com
```

### 11.2 Incorrect DNS Configuration

A system may use an incorrect or unavailable DNS server.

Inspect:

```bash
cat /etc/resolv.conf
```

Do not edit DNS configuration blindly. Network managers or system services may manage this file automatically.

### 11.3 DNS Resolution Failure but IP Connectivity Works

Suppose this succeeds:

```bash
ping -c 4 1.1.1.1
```

But this fails:

```bash
getent hosts example.com
```

Possible explanation: IP connectivity exists, but name resolution is failing.

### 11.4 Domain Resolves but Service Is Unavailable

DNS may successfully resolve a domain even when its web server is unavailable.

DNS resolution and application availability are separate stages.

---

## 12. DNS Security Risks

DNS is an important part of cybersecurity.

Attackers may target DNS infrastructure or manipulate name resolution.

### 12.1 DNS Spoofing

An attacker attempts to provide false DNS information.

This may redirect a user to an unintended IP address.

### 12.2 DNS Cache Poisoning

An attacker attempts to insert false DNS data into a resolver's cache.

### 12.3 DNS Hijacking

DNS settings or traffic may be manipulated to redirect users.

### 12.4 DNS Tunneling

An attacker may encode information inside DNS queries and responses to create a covert communication channel.

### 12.5 DNS Amplification

An attacker abuses DNS servers to generate large responses toward a victim, potentially contributing to a distributed denial-of-service attack.

### Defensive Measures

- Use trusted DNS resolvers.
- Keep DNS software updated.
- Restrict unauthorized DNS server access.
- Monitor unusual DNS traffic.
- Use DNSSEC validation where supported.
- Consider encrypted DNS protocols such as DNS over HTTPS or DNS over TLS.
- Investigate unexpected DNS configuration changes.

---

## 13. Practical Lab: DNS Investigation in Kali Linux

### Lab Objective

Investigate DNS resolution using Linux commands.

Use public domains for ordinary DNS lookups.

### Step 1: Inspect Resolver Configuration

```bash
cat /etc/resolv.conf
```

Record the configured nameserver entries.

### Step 2: Inspect the Hosts File

```bash
cat /etc/hosts
```

Identify the localhost entries.

### Step 3: Inspect Name-Service Configuration

```bash
grep '^hosts:' /etc/nsswitch.conf
```

Record the lookup order.

### Step 4: Perform a System DNS Lookup

```bash
getent hosts example.com
```

Observe the returned address.

### Step 5: Use the `host` Command

```bash
host example.com
```

### Step 6: Perform a Basic DNS Query

```bash
dig example.com
```

Identify:

- The question section.
- The answer section.
- The responding server.
- The query time.

### Step 7: Display a Short DNS Answer

```bash
dig +short example.com
```

### Step 8: Query an A Record

```bash
dig example.com A
```

### Step 9: Query an MX Record

```bash
dig example.com MX
```

### Step 10: Query a Public DNS Resolver

```bash
dig @1.1.1.1 example.com
```

Compare the answer with your normal DNS query.

### Step 11: Perform a Reverse Lookup

```bash
dig -x 8.8.8.8
```

Observe whether a PTR record is returned.

### Step 12: Compare DNS and IP Connectivity

```bash
ping -c 4 1.1.1.1
```

Then:

```bash
getent hosts example.com
```

Explain whether both tests succeeded.

---

## 14. Practical Analysis Table

Record your findings:

| Investigation | Observation |
|---|---|
| `/etc/resolv.conf` | |
| `/etc/hosts` | |
| Name-service lookup order | |
| `getent hosts` | |
| `host` result | |
| `dig` answer | |
| A record | |
| MX record | |
| Public resolver result | |
| Reverse DNS result | |

---

## 15. Assessment Questions

Answer the following:

1. What does DNS stand for?
2. Why is DNS important?
3. What is the difference between a recursive resolver and an authoritative DNS server?
4. What is the purpose of an A record?
5. What is the difference between A and AAAA records?
6. What is an MX record used for?
7. What is DNS caching?
8. What is the difference between IPv4 packet TTL and DNS record TTL?
9. Which Linux command can perform detailed DNS investigations?
10. What is DNS spoofing?
11. What is the purpose of `/etc/hosts`?
12. Why can a computer successfully ping an IP address but fail to resolve a domain name?

---

## 16. Important Commands to Remember

```bash
cat /etc/resolv.conf
cat /etc/hosts
grep '^hosts:' /etc/nsswitch.conf

getent hosts example.com

host example.com
nslookup example.com

dig example.com
dig +short example.com
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com NS
dig example.com TXT

dig @1.1.1.1 example.com
dig -x 8.8.8.8
```

---

## 17. Lesson Summary

In this lesson, you learned:

- DNS translates domain names into IP addresses.
- DNS uses a hierarchical structure.
- Recursive resolvers obtain DNS answers for clients.
- Authoritative servers provide authoritative information about DNS zones.
- DNS records include A, AAAA, CNAME, MX, NS, TXT, PTR, SOA, and SRV.
- DNS caching improves lookup efficiency.
- Linux uses resolver configuration and name-service mechanisms.
- `dig`, `host`, `nslookup`, and `getent` help investigate name resolution.
- DNS infrastructure can be targeted through spoofing, poisoning, hijacking, and tunneling.

### Key Definitions

**DNS:** Domain Name System, a distributed naming system that translates domain names into network information.

**Resolver:** A component that processes DNS queries and obtains answers.

**Authoritative DNS Server:** A server that provides authoritative DNS information for a zone.

**DNS Record:** An entry containing information about a domain or service.

**DNS Cache:** Temporary storage of DNS information to reduce repeated lookups.

**DNS Spoofing:** An attack involving false DNS responses or information.

**DNSSEC:** Domain Name System Security Extensions, which help authenticate DNS data using cryptographic signatures.

---

## 18. Knowledge Check

1. Which DNS record maps a hostname to an IPv4 address?
2. Which DNS record identifies mail servers?
3. What is the purpose of `dig`?
4. Which file contains local hostname mappings?
5. What does `dig -x` perform?
6. What is DNS cache poisoning?
7. What is the purpose of DNSSEC?
8. Why is DNS important in cybersecurity?

**End of Lesson 08 – DNS and Name Resolution**