
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 05: DNS RESOLUTION AND TROUBLESHOOTING

### 1. Lab Overview

DNS stands for **Domain Name System**.

DNS is responsible for translating human-readable domain names into IP addresses that computers use to communicate.

For example, when you enter:

```text
google.com
```

Your computer needs to discover the IP address associated with that domain.

DNS helps perform this translation.

Without DNS, users would generally need to remember IP addresses instead of convenient domain names.

DNS is essential in:

- Web browsing.
- Email delivery.
- Cloud computing.
- Network administration.
- Cybersecurity investigations.
- Domain infrastructure.
- Enterprise network management.

In this lab, you will inspect DNS configuration, perform domain lookups, examine DNS records, and troubleshoot common resolution problems.

**Lab environment:** Kali Linux VM.

**Safety:** We will use normal DNS queries against public domains. No DNS attacks or unauthorized infrastructure testing will be performed.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain DNS.
2. Understand how DNS resolution works.
3. Identify DNS servers.
4. Inspect Linux DNS configuration.
5. Use `getent` to resolve domain names.
6. Use `nslookup` and `dig` when available.
7. Understand common DNS record types.
8. Inspect DNS resolution failures.
9. Understand DNS caching.
10. Generate a DNS diagnostic report.

### 3. What Is DNS?

DNS is a distributed naming system that connects domain names to network information.

Example:

```text
Domain Name: example.com
IP Address: 93.184.216.34
```

The IP address above is an illustrative example. Actual DNS answers can vary.

Think of DNS as the internet's phonebook.

| Human-readable name | Computer-readable destination |
|---|---|
| example.com | IP address |
| google.com | IP address |
| wikipedia.org | IP address |

### 4. How DNS Resolution Works

A simplified DNS lookup follows this process:

```text
User enters a domain name
          |
          v
Operating system checks
local DNS information
          |
          v
DNS resolver receives query
          |
          v
Resolver obtains DNS answer
          |
          v
IP address is returned
          |
          v
Computer connects to destination
```

The actual process may involve caching, recursive resolvers, root servers, top-level domain servers, and authoritative DNS servers.

### 5. Important DNS Components

**1. DNS Resolver**

A service that receives DNS queries and obtains answers for clients.

**2. Recursive Resolver**

A resolver that performs the necessary lookup process on behalf of a client.

**3. Root DNS Servers**

The top level of the DNS hierarchy. They direct queries toward the appropriate top-level domain servers.

**4. TLD Servers**

TLD means Top-Level Domain.

Examples:

- `.com`
- `.org`
- `.net`
- `.ng`

**5. Authoritative DNS Servers**

Servers that provide authoritative DNS information for a domain.

### 6. Common DNS Record Types

| Record | Purpose |
|---|---|
| A | Maps a domain to an IPv4 address |
| AAAA | Maps a domain to an IPv6 address |
| CNAME | Creates an alias for another domain name |
| MX | Identifies mail servers |
| NS | Identifies authoritative name servers |
| TXT | Stores text information, often used for verification and email security |
| PTR | Supports reverse DNS lookups |
| SOA | Contains administrative information about a DNS zone |
| SRV | Identifies services and their connection details |

Examples:

```text
example.com     A       IPv4 address
example.com     AAAA    IPv6 address
example.com     MX      Mail server
example.com     NS      Name server
```

### 7. Inspect Linux DNS Configuration

Start by inspecting:

```bash
cat /etc/resolv.conf
```

You may see something similar to:

```text
nameserver 192.168.100.1
```

This identifies a DNS server configured for the system.

Your actual output may be different.

Other possible entries include:

```text
search example.local
options timeout:2
```

Important:

On systems using NetworkManager or `systemd-resolved`, `/etc/resolv.conf` may be generated automatically or point to a local resolver.

Do not manually edit it during this lab.

### 8. Inspect systemd-resolved

Check whether the service exists:

```bash
systemctl status systemd-resolved
```

If it is unavailable, record that and continue.

If available, inspect DNS information:

```bash
resolvectl status
```

You can also run:

```bash
resolvectl dns
```

These commands can show DNS configuration associated with network interfaces.

### 9. Resolve Domain Names Using getent

`getent` is useful because it uses the system's configured name-resolution mechanisms.

Run:

```bash
getent hosts example.com
```

Try:

```bash
getent hosts google.com
```

Try:

```bash
getent hosts wikipedia.org
```

Possible output:

```text
93.184.216.34 example.com
```

The exact addresses may vary.

If the command produces no result, DNS resolution may have failed or the system may be unable to reach its resolver.

### 10. Understand nslookup

`nslookup` is a DNS query utility.

Check whether it is installed:

```bash
command -v nslookup
```

If available, run:

```bash
nslookup example.com
```

You may see:

```text
Server:  192.168.100.1
Address: 192.168.100.1#53

Name:    example.com
Address: 93.184.216.34
```

Interpretation:

- Server: DNS server used.
- Address: DNS server address.
- Name: queried domain.
- Address under the answer: returned IP address.

The actual response can contain different addresses or additional information.

### 11. Understand dig

`dig` means Domain Information Groper.

It provides detailed DNS query information.

Check whether it is available:

```bash
command -v dig
```

If available:

```bash
dig example.com
```

Inspect the answer section.

Example:

```text
;; ANSWER SECTION:
example.com.  3600  IN  A  93.184.216.34
```

Important fields:

| Field | Meaning |
|---|---|
| example.com | Queried domain |
| 3600 | TTL in seconds |
| IN | Internet class |
| A | Record type |
| 93.184.216.34 | Example IPv4 answer |

### 12. Query Specific DNS Record Types

If `dig` is available, inspect different records.

**A record**

```bash
dig example.com A
```

**AAAA record**

```bash
dig example.com AAAA
```

**MX record**

```bash
dig example.com MX
```

**NS record**

```bash
dig example.com NS
```

**TXT record**

```bash
dig example.com TXT
```

**SOA record**

```bash
dig example.com SOA
```

You can also use `nslookup` for specific record types:

```bash
nslookup -type=MX example.com
```

The available records depend on the domain.

### 13. Understand DNS Ports

DNS commonly uses:

| Protocol | Port | Purpose |
|---|---:|---|
| UDP | 53 | Standard DNS queries and responses |
| TCP | 53 | DNS responses and other operations requiring TCP |

DNS over TLS commonly uses TCP port 853.

DNS over HTTPS commonly uses HTTPS over TCP port 443, although HTTP/3 can use UDP.

DNS is not always encrypted. Traditional DNS queries can reveal the domain names a client is requesting to network observers.

### 14. Understand DNS Caching

DNS caching stores previously obtained DNS answers for a period of time.

Caching improves performance by reducing repeated DNS lookups.

The TTL (Time To Live) determines how long a DNS record may be cached.

Example:

```text
example.com.  3600  IN  A  93.184.216.34
```

The TTL is 3600 seconds, or one hour.

DNS caching can exist in:

- Operating systems.
- Browsers.
- Local DNS services.
- Recursive DNS resolvers.

Not every Linux distribution uses the same local caching service.

### 15. Inspect the Hosts File

Linux has a local hosts file.

Inspect it:

```bash
cat /etc/hosts
```

Example:

```text
127.0.0.1 localhost
::1       localhost
```

The hosts file can provide local name-to-address mappings.

For example:

```text
192.168.100.50 lab-server.local
```

This would associate `lab-server.local` with the specified address for applications using the system's normal name-resolution mechanisms.

For this exercise, inspect the file only. Do not modify it.

### 16. Understand DNS Troubleshooting

DNS problems can occur for several reasons.

| Problem | Possible Cause |
|---|---|
| Domain does not resolve | DNS server unavailable |
| Internet works by IP but not domain | DNS resolution problem |
| Wrong DNS answer | Incorrect or outdated DNS information |
| DNS timeout | Resolver unreachable or filtering |
| Some domains resolve, others fail | Domain-specific or record-specific issue |
| Slow DNS response | Resolver delay or network latency |

### 17. Troubleshooting Workflow

When a domain fails to resolve, follow this order.

**Step 1: Check interface configuration**

```bash
ip -br addr
```

**Step 2: Check routing**

```bash
ip route
```

**Step 3: Inspect DNS configuration**

```bash
cat /etc/resolv.conf
```

**Step 4: Test system name resolution**

```bash
getent hosts example.com
```

**Step 5: Test DNS using dig, if installed**

```bash
dig example.com
```

**Step 6: Compare results**

If `dig` returns an answer but `getent` fails, the issue may involve the operating system's name-service configuration rather than basic DNS query capability.

If both fail, investigate connectivity, resolver availability, and filtering.

### 18. Understand the Difference Between DNS and IP Connectivity

Consider two situations.

**Situation A:**

```text
ping 1.1.1.1
```

Works, but:

```text
getent hosts example.com
```

Fails.

This suggests that basic IP connectivity may work while DNS resolution is failing.

**Situation B:**

Both commands fail.

This may indicate a broader network connectivity problem, although ICMP filtering and other factors must be considered.

Do not assume that one failed ping proves a server is offline.

### 19. Practical Exercise 1 – Inspect DNS Configuration

Run:

```bash
cat /etc/resolv.conf
```

Record:

- Configured DNS server addresses.
- Search domains, if present.
- Other resolver options.

### 20. Practical Exercise 2 – Resolve Three Domains

Run:

```bash
getent hosts example.com
```

```bash
getent hosts google.com
```

```bash
getent hosts wikipedia.org
```

Record whether each domain resolves.

### 21. Practical Exercise 3 – Inspect DNS Utilities

Run:

```bash
command -v dig
command -v nslookup
command -v host
```

Record which tools are installed.

Do not install anything just to complete this exercise.

### 22. Practical Exercise 4 – Query DNS Records

If `dig` is available, run:

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com NS
dig example.com TXT
```

Observe the answer sections.

Record any record types that return no answer.

A missing record is not necessarily a DNS failure.

### 23. Practical Exercise 5 – Inspect the Hosts File

Run:

```bash
cat /etc/hosts
```

Identify:

- The IPv4 loopback entry.
- The IPv6 loopback entry.
- Any other local name mappings.

### 24. Practical Exercise 6 – Compare Resolution Methods

Run:

```bash
getent hosts example.com
```

If available:

```bash
dig example.com
```

Compare the results.

Consider:

- Do both commands return addresses?
- Are the returned addresses identical?
- Which DNS server was queried?
- Could caching or different DNS resolution paths explain differences?

### 25. Generate a DNS Diagnostic Report

Create the lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab05
```

Create the script:

```bash
nano ~/practical-labs/module05/lab05/dns-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab05/dns-report.txt"

{
    echo "LINUX DNS DIAGNOSTIC REPORT"
    echo "==========================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "DNS CONFIGURATION"
    cat /etc/resolv.conf
    echo

    echo "HOSTS FILE"
    cat /etc/hosts
    echo

    echo "DOMAIN RESOLUTION"
    getent hosts example.com
    echo

    echo "DNS TOOL AVAILABILITY"
    command -v dig || echo "dig not installed"
    command -v nslookup || echo "nslookup not installed"
    command -v host || echo "host not installed"
    echo

    if command -v dig >/dev/null 2>&1; then
        echo "DIG A RECORD QUERY"
        dig example.com A
    fi

    echo
    echo "NETWORK INTERFACE SUMMARY"
    ip -br addr

    echo
    echo "ROUTING TABLE"
    ip route

} > "$REPORT" 2>&1

echo "DNS report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab05/dns-report.sh
```

Execute it:

```bash
~/practical-labs/module05/lab05/dns-report.sh
```

Read the report:

```bash
cat ~/practical-labs/module05/lab05/dns-report.txt
```

### 26. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab05/lab05-report.txt
```

Record:

```text
MODULE 05 – LAB 05
DNS RESOLUTION AND TROUBLESHOOTING

1. What does DNS stand for?
2. What is the purpose of DNS?
3. DNS server configured on my system:
4. Result of example.com lookup:
5. Result of google.com lookup:
6. Result of wikipedia.org lookup:
7. Is dig installed?
8. Is nslookup installed?
9. What is an A record?
10. What is an AAAA record?
11. What is an MX record?
12. What is a DNS TTL?
13. What is the purpose of /etc/hosts?
14. What are common DNS problems?
15. What did I learn?
```

Save with `CTRL + O`, press `ENTER`, then exit with `CTRL + X`.

### 27. Knowledge Check

1. What does DNS stand for?
2. Why is DNS important?
3. What is a recursive resolver?
4. What is an authoritative DNS server?
5. What is the difference between A and AAAA records?
6. What is the purpose of an MX record?
7. Which port does traditional DNS commonly use?
8. What is DNS caching?
9. What does TTL mean?
10. What is the purpose of `/etc/resolv.conf`?
11. What is the purpose of `/etc/hosts`?
12. What is the difference between `dig` and `getent`?
13. Why might an IP address work while a domain name fails?
14. Why is DNS security important?

### 28. Lab Completion Checklist

- [ ] Understood DNS fundamentals.
- [ ] Learned the DNS resolution process.
- [ ] Identified major DNS components.
- [ ] Studied common DNS record types.
- [ ] Inspected `/etc/resolv.conf`.
- [ ] Inspected systemd-resolved, if available.
- [ ] Practised domain resolution using `getent`.
- [ ] Checked availability of DNS utilities.
- [ ] Queried DNS records, if `dig` was available.
- [ ] Inspected `/etc/hosts`.
- [ ] Studied DNS caching and TTL.
- [ ] Learned DNS troubleshooting techniques.
- [ ] Generated the DNS diagnostic report.
- [ ] Completed lab documentation.

**LAB 05 STATUS: READY FOR PRACTICAL COMPLETION**
