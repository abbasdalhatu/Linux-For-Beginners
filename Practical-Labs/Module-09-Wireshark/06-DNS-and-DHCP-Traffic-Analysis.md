
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 06: DNS AND DHCP TRAFFIC ANALYSIS

## 1. Introduction

DNS and DHCP are two essential protocols used in computer networks.

- DNS translates domain names into IP addresses.
- DHCP automatically provides network configuration to devices.

Without DNS, users would have difficulty accessing websites using memorable domain names.

Without DHCP, network administrators would need to configure IP addresses and other network settings manually on many devices.

In this lesson, you will learn how these protocols work and how to investigate their traffic using Wireshark.

## 2. What Is DNS?

DNS stands for Domain Name System.

DNS translates human-readable domain names into IP addresses.

For example:

```text
example.com → IP address
```

Computers use IP addresses to communicate across IP networks.

Humans generally find domain names easier to remember.

### Example

When you enter:

```text
www.example.com
```

Your device may need to perform a DNS lookup to discover the corresponding IP address.

The DNS resolver processes the request and returns an answer when available.

## 3. How DNS Resolution Works

A simplified DNS lookup follows these steps:

1. A user enters a domain name.
2. The computer checks its local DNS cache.
3. If no suitable cached answer exists, the device contacts its configured DNS resolver.
4. The resolver finds the required DNS information.
5. The resolver returns an answer.
6. The computer can use the resulting IP address to communicate with the destination.

### Simplified Diagram

```text
USER
  |
  v
COMPUTER
  |
  v
DNS RESOLVER
  |
  v
DNS LOOKUP
  |
  v
IP ADDRESS RETURNED
  |
  v
APPLICATION CONNECTS
```

Actual DNS resolution can involve caches, recursive resolvers, root servers, and authoritative name servers.

## 4. Common DNS Record Types

DNS contains different record types.

| Record | Purpose |
|---|---|
| A | Maps a name to an IPv4 address |
| AAAA | Maps a name to an IPv6 address |
| CNAME | Provides an alias for another domain name |
| MX | Identifies mail servers |
| NS | Identifies authoritative name servers |
| TXT | Stores text information |
| PTR | Supports reverse DNS lookups |
| SOA | Contains zone authority information |

### Example

An A record:

```text
example.com → 192.0.2.10
```

An AAAA record:

```text
example.com → 2001:db8::10
```

These addresses are documentation examples, not necessarily the real addresses of the domain.

## 5. DNS Ports and Transport Protocols

Traditional DNS commonly uses:

- UDP port 53.
- TCP port 53.

UDP is commonly used for ordinary DNS queries and responses.

TCP may be used for larger responses, zone transfers, and other DNS operations.

DNS encryption technologies include:

- DNS over TLS (DoT).
- DNS over HTTPS (DoH).
- DNS over QUIC (DoQ).

These technologies change how DNS traffic is transported and may make ordinary DNS packet inspection less informative.

## 6. DNS Query and Response

A DNS query is a request for DNS information.

A DNS response contains the answer or information about why the query could not be completed.

Example:

```text
CLIENT                         DNS SERVER
  |                                 |
  |------ DNS QUERY ---------------->|
  |   example.com                   |
  |                                 |
  |<----- DNS RESPONSE --------------|
  |   IP address                    |
  |                                 |
```

Important DNS packet fields include:

- Transaction ID.
- Query name.
- Query type.
- Response code.
- Flags.
- Answer records.
- Time to Live (TTL).

The transaction ID helps match a DNS response to its corresponding query.

## 7. What Is DHCP?

DHCP stands for Dynamic Host Configuration Protocol.

DHCP automatically provides network configuration to clients.

A DHCP server may provide:

- IPv4 address.
- Subnet mask.
- Default gateway.
- DNS server address.
- Lease duration.
- Other network configuration options.

DHCP reduces the need for manual network configuration.

## 8. DHCP DORA Process

A common DHCP IPv4 configuration process is called DORA.

DORA means:

1. Discover.
2. Offer.
3. Request.
4. Acknowledge.

### DHCP DORA Diagram

```text
CLIENT                         DHCP SERVER
  |                                 |
  |--------- DISCOVER -------------->|
  |                                 |
  |<---------- OFFER ----------------|
  |                                 |
  |--------- REQUEST --------------->|
  |                                 |
  |<--------- ACKNOWLEDGE ------------|
  |                                 |
  |       CONFIGURATION RECEIVED     |
```

### Step 1: Discover

The client searches for available DHCP servers.

### Step 2: Offer

The server offers an available IP address and configuration.

### Step 3: Request

The client requests the offered configuration.

### Step 4: Acknowledge

The server confirms the configuration assignment.

Not every DHCP exchange follows this exact simplified sequence. Renewals and other DHCP situations can behave differently.

## 9. DHCP Ports

DHCP for IPv4 commonly uses:

| Port | Role |
|---|---|
| UDP 67 | DHCP server |
| UDP 68 | DHCP client |

DHCPv6 uses UDP ports 546 and 547.

## 10. DNS vs DHCP

| Feature | DNS | DHCP |
|---|---|---|
| Full name | Domain Name System | Dynamic Host Configuration Protocol |
| Main purpose | Name resolution | Network configuration |
| Common IPv4 ports | UDP/TCP 53 | UDP 67/68 |
| Main function | Domain name to IP information | Assigns network settings |
| Typical use | Finding services and hosts | Connecting devices to networks |

Remember:

DNS helps a device find destinations by name.

DHCP helps a device obtain the configuration needed to communicate on a network.

## 11. Preparing Your Practical Lab

Create your directory:

```bash
mkdir -p ~/practical-labs/module09/lab06/captures
mkdir -p ~/practical-labs/module09/lab06/reports
```

Your directory:

```text
lab06/
├── captures/
└── reports/
```

## 12. Practical Exercise 1: Inspect DNS Configuration

Display the system's resolver configuration:

```bash
cat /etc/resolv.conf
```

Depending on how Kali manages networking, this file may contain a local resolver address or generated configuration.

Inspect your network configuration:

```bash
ip address
```

Inspect your routing table:

```bash
ip route
```

Inspect the system's DNS-related resolver information:

```bash
resolvectl status
```

If `resolvectl` is unavailable, skip that command.

Record:

- Your network interface.
- Your configured IP address.
- Your default gateway.
- Your configured DNS resolver, if visible.

Do not publish private network information in public reports.

## 13. Practical Exercise 2: Generate a DNS Query

Use a harmless DNS lookup:

```bash
nslookup example.com
```

Alternative:

```bash
dig example.com
```

Request an IPv4 record:

```bash
dig example.com A
```

Request an IPv6 record:

```bash
dig example.com AAAA
```

Request mail server records:

```bash
dig example.com MX
```

Request name server records:

```bash
dig example.com NS
```

These commands generate DNS queries using your configured resolver.

If internet or DNS access is unavailable, record the error and continue with the analysis of any available capture.

## 14. Practical Exercise 3: Capture DNS Traffic in Wireshark

### Objective

Capture and inspect DNS requests and responses generated by your own Kali VM.

Step 1: Open Wireshark.

```bash
wireshark
```

Step 2: Select the active network interface.

Step 3: Start capturing packets.

Step 4: Generate a DNS lookup in another terminal:

```bash
dig example.com
```

Step 5: Return to Wireshark and stop the capture.

Step 6: Apply this display filter:

```text
dns
```

You should see DNS packets if the lookup generated visible traditional DNS traffic on the selected interface.

Step 7: Select a DNS query.

Expand:

```text
Domain Name System (query)
```

Inspect:

- Transaction ID.
- Query name.
- Query type.
- Query flags.

Step 8: Select the corresponding response.

Expand:

```text
Domain Name System (response)
```

Inspect:

- Response code.
- Answer records.
- Returned addresses.
- TTL values.

Save the capture:

```text
~/practical-labs/module09/lab06/captures/dns-analysis.pcapng
```

## 15. Practical Exercise 4: Analyze DNS Filters

Use these Wireshark display filters.

Show DNS traffic:

```text
dns
```

Show DNS queries:

```text
dns.flags.response == 0
```

Show DNS responses:

```text
dns.flags.response == 1
```

Show A record queries:

```text
dns.qry.type == 1
```

Show AAAA record queries:

```text
dns.qry.type == 28
```

Show MX record queries:

```text
dns.qry.type == 15
```

Show DNS traffic using UDP:

```text
udp.port == 53
```

Show DNS traffic using TCP:

```text
tcp.port == 53
```

Show DNS responses indicating an error:

```text
dns.flags.rcode != 0
```

An error response does not automatically indicate a security problem. It may result from a nonexistent domain, unavailable service, or configuration issue.

## 16. Practical Exercise 5: Inspect DHCP Traffic

### Objective

Learn to recognize DHCP packets in Wireshark.

DHCP traffic may not appear continuously because a device normally requests configuration only when needed or when renewing a lease.

You may need to wait for a natural DHCP exchange or use an existing authorized capture.

Do not repeatedly restart network services or change your VM networking configuration just to force DHCP traffic.

Apply this Wireshark display filter:

```text
dhcp
```

If your Wireshark version does not recognize it, try:

```text
bootp
```

These filters help identify DHCP traffic in different Wireshark versions.

Inspect available packets for:

- Message type.
- Client MAC address.
- Assigned IP address.
- DHCP server identifier.
- Requested IP address.
- Lease time.
- Subnet mask.
- Router option.
- DNS server option.

## 17. Practical Exercise 6: Identify DHCP DORA Messages

If your capture contains a complete DHCP exchange, identify:

| DORA Stage | DHCP Message |
|---|---|
| Discover | DHCP Discover |
| Offer | DHCP Offer |
| Request | DHCP Request |
| Acknowledge | DHCP ACK |

Record the source and destination addresses where visible.

For each message, identify its purpose.

If your capture does not contain a complete exchange, do not invent missing packets. Record which stages were unavailable.

## 18. DHCP Display Filters

Show DHCP traffic:

```text
dhcp
```

Alternative:

```text
bootp
```

Show DHCP traffic involving UDP port 67:

```text
udp.port == 67
```

Show DHCP traffic involving UDP port 68:

```text
udp.port == 68
```

Show DHCP Discover messages:

```text
dhcp.option.dhcp == 1
```

Show DHCP Offer messages:

```text
dhcp.option.dhcp == 2
```

Show DHCP Request messages:

```text
dhcp.option.dhcp == 3
```

Show DHCP ACK messages:

```text
dhcp.option.dhcp == 5
```

If a field name is unsupported in your Wireshark version, select the relevant packet, expand the DHCP protocol tree, and use the field's actual name shown by Wireshark.

## 19. Practical Exercise 7: Analyze DNS Using TShark

Read your DNS capture:

```bash
tshark -r ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng
```

Display DNS packets:

```bash
tshark -r ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng -Y dns
```

Display DNS query packets:

```bash
tshark -r ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng -Y "dns.flags.response == 0"
```

Extract DNS query names:

```bash
tshark -r ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng -T fields -e dns.qry.name
```

Extract DNS response codes:

```bash
tshark -r ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng -T fields -e dns.flags.rcode
```

Extract source and destination addresses:

```bash
tshark -r ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng -T fields -e ip.src -e ip.dst
```

## 20. Defensive DNS Investigation

DNS traffic can provide useful security information.

Security analysts may investigate:

### A. Unusual DNS Query Volume

A device generating an unusually high number of DNS queries may require investigation.

Possible explanations include:

- An application malfunction.
- Automated background activity.
- Misconfigured software.
- Suspicious software behaviour.

### B. Unusual Domain Names

Unexpected or unfamiliar domain names may deserve additional investigation.

However, unfamiliar does not automatically mean malicious.

### C. Repeated Failed DNS Queries

Repeated DNS failures may indicate:

- Incorrect DNS settings.
- Network connectivity problems.
- Typing errors.
- Unavailable domains.
- Suspicious software behaviour.

### D. Unusual DNS Destinations

Unexpected DNS resolver addresses may be worth reviewing against the organization's approved network configuration.

### E. Encrypted DNS

DoH and DoT can limit what ordinary packet inspection reveals.

Analysts may need endpoint logs, resolver logs, approved network telemetry, and other evidence.

## 21. Defensive DHCP Investigation

DHCP analysis can help identify network configuration problems.

Important warning signs include:

- Unexpected DHCP servers.
- Conflicting IP assignments.
- Repeated DHCP requests.
- Unexpected gateway configuration.
- Unusual DNS server assignments.

An unauthorized DHCP server may provide incorrect network settings to clients.

However, confirm the device's identity and network role before treating it as malicious.

## 22. Practical Investigation Scenario

An organization's workstation is experiencing unusual network behaviour.

An analyst observes:

```text
09:15:01 DNS query: internal.example
09:15:02 DNS response: successful
09:15:05 DNS query: unknown-domain.example
09:15:06 DNS response: NXDOMAIN
09:15:08 DNS query: unknown-domain.example
09:15:09 DNS response: NXDOMAIN
```

Answer:

1. Which protocol is being used?
2. What is a DNS query?
3. What does NXDOMAIN mean?
4. Why might repeated failed queries deserve investigation?
5. What additional evidence should an analyst collect?
6. Does this information alone prove malware infection?

Expected reasoning:

NXDOMAIN indicates that the requested domain name does not exist according to the responding DNS system. Repeated requests may be caused by legitimate software or suspicious activity. Additional evidence is necessary.

## 23. Evidence Preservation

Calculate the SHA-256 hash of your DNS capture:

```bash
sha256sum ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng
```

Save the hash:

```bash
sha256sum ~/practical-labs/module09/lab06/captures/dns-analysis.pcapng > ~/practical-labs/module09/lab06/captures/checksums.sha256
```

Verify the capture:

```bash
sha256sum -c ~/practical-labs/module09/lab06/captures/checksums.sha256
```

If you obtain a DHCP capture, preserve and hash it separately as well.

## 24. Practical Investigation Report

Create your report:

```bash
nano ~/practical-labs/module09/lab06/reports/dns-dhcp-analysis-report.txt
```

Use this template:

```text
==========================================
DNS AND DHCP TRAFFIC ANALYSIS REPORT
==========================================

1. INVESTIGATION INFORMATION

Date:
Operating System:
Network Interface:

2. DNS ANALYSIS

Capture File:
DNS Resolver:
Query Names:
Query Types:
Response Codes:
Returned Addresses:

3. DNS SECURITY OBSERVATIONS

Unusual Queries:
Repeated Failures:
Unexpected Resolvers:
Possible Risks:

4. DHCP ANALYSIS

DHCP Capture Available:
Client Address:
DHCP Server:
Assigned Address:
Subnet Mask:
Default Gateway:
DNS Server:
Lease Information:

5. DHCP DORA ANALYSIS

Discover Observed:
Offer Observed:
Request Observed:
ACK Observed:

6. WIRESHARK FILTERS

DNS Filters:
DHCP Filters:

7. EVIDENCE INTEGRITY

DNS Capture SHA-256:
DHCP Capture SHA-256:

8. CONCLUSION

Confirmed Findings:
Unverified Hypotheses:
Recommendations:

==========================================
END OF REPORT
==========================================
```

## 25. Knowledge Check

1. What does DNS stand for?
2. What is the main purpose of DNS?
3. What is the difference between an A and AAAA record?
4. What is a CNAME record?
5. What is an MX record?
6. Which port does traditional DNS commonly use?
7. What is the difference between UDP and TCP DNS?
8. What does DHCP stand for?
9. What are the four stages of DORA?
10. Which UDP ports are commonly used by DHCP for IPv4?
11. What is the purpose of a DHCP lease?
12. What does NXDOMAIN mean?
13. Why might an unexpected DHCP server be suspicious?
14. What is DNS over HTTPS?
15. Why is additional evidence important during DNS investigations?

## 26. Lesson Completion Checklist

- [ ] Understood DNS fundamentals.
- [ ] Learned how DNS resolution works.
- [ ] Studied common DNS record types.
- [ ] Understood DNS query and response packets.
- [ ] Learned DNS ports and transport protocols.
- [ ] Understood DHCP fundamentals.
- [ ] Studied the DHCP DORA process.
- [ ] Inspected DNS configuration.
- [ ] Generated DNS queries.
- [ ] Captured DNS traffic in Wireshark.
- [ ] Applied DNS display filters.
- [ ] Identified DHCP packet fields.
- [ ] Studied DHCP display filters.
- [ ] Used TShark for DNS analysis.
- [ ] Learned defensive DNS and DHCP investigation.
- [ ] Preserved packet evidence.
- [ ] Created an investigation report.

## 27. Lesson Summary

DNS translates domain names into IP address information, while DHCP provides network configuration to devices.

You learned DNS record types, DNS queries and responses, traditional DNS ports, DHCP DORA, and DHCP message types.

You also practiced generating DNS requests, capturing DNS traffic, inspecting DHCP packets, applying Wireshark filters, and using TShark.

**Next Lesson: Lesson 07 – HTTP and HTTPS Traffic Analysis.**
