
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 10: FINAL PRACTICAL ASSESSMENT

## 1. Assessment Overview

Congratulations on reaching the final lesson of Module 09.

Throughout this module, you have studied Wireshark and network traffic analysis.

You have learned how to:

- Capture network packets.
- Analyze Ethernet frames.
- Inspect MAC addresses.
- Analyze IPv4 and IPv6.
- Understand ICMP.
- Investigate TCP and UDP.
- Analyze DNS and DHCP.
- Inspect HTTP and HTTPS.
- Apply advanced Wireshark filters.
- Investigate suspicious network activity.
- Preserve packet evidence.
- Write professional investigation reports.

This assessment evaluates your ability to apply these skills in practical situations.

## 2. Assessment Information

| Item | Details |
|---|---|
| Module | 09 – Wireshark and Network Traffic Analysis |
| Assessment Type | Practical and theoretical |
| Total Marks | 100 |
| Passing Mark | 50 |
| Recommended Time | 3–4 hours |
| Environment | Kali Linux in VMware |
| Main Tool | Wireshark |
| Additional Tool | TShark |

### Assessment Rules

1. Use your own Kali Linux VM.
2. Capture only traffic you are authorized to inspect.
3. Use local loopback services for generated traffic.
4. Do not scan public or unauthorized networks.
5. Preserve original packet captures.
6. Document your observations accurately.
7. Do not invent results when traffic is unavailable.

## 3. Assessment Workspace

Create the final assessment directory:

```bash
mkdir -p ~/practical-labs/module09/assessment/captures
mkdir -p ~/practical-labs/module09/assessment/reports
mkdir -p ~/practical-labs/module09/assessment/evidence
mkdir -p ~/practical-labs/module09/assessment/screenshots
```

Expected structure:

```text
assessment/
├── captures/
├── reports/
├── evidence/
└── screenshots/
```

## 4. Task 1: Network Environment Inspection

**Marks: 10**

Inspect your Kali network environment.

Run:

```bash
ip address
```

```bash
ip route
```

```bash
ip neigh
```

```bash
ss -tuln
```

```bash
sudo ss -tulnp
```

### Questions

1. Identify your active network interface.
2. Identify your IPv4 address, if assigned.
3. Identify your IPv6 address, if assigned.
4. Identify your default gateway, if configured.
5. Identify at least two listening services, if present.
6. Explain the purpose of the neighbour table.

### Evidence

Save the command outputs:

```bash
ip address > ~/practical-labs/module09/assessment/evidence/ip-address.txt
```

```bash
ip route > ~/practical-labs/module09/assessment/evidence/routing-table.txt
```

```bash
ss -tuln > ~/practical-labs/module09/assessment/evidence/listening-sockets.txt
```

## 5. Task 2: Packet Capture and Ethernet Analysis

**Marks: 10**

Capture traffic on your Kali loopback interface or another authorized interface.

Generate local traffic where possible.

Inspect:

- Ethernet information where available.
- Source and destination MAC addresses.
- EtherType.
- Packet length.
- Frame information.

Important: Loopback traffic may not contain ordinary Ethernet headers or MAC addresses. If you use `lo`, use an appropriate Ethernet capture from an earlier lesson for the Ethernet-specific part.

### Questions

1. What is a MAC address?
2. What is the purpose of EtherType?
3. What is a broadcast MAC address?
4. What is the difference between a frame and a packet?

### Evidence

Save your capture as:

```text
assessment/captures/ethernet-analysis.pcapng
```

If you use a previous authorized Ethernet capture, copy it into the assessment directory without modifying the original.

## 6. Task 3: IP and ICMP Analysis

**Marks: 10**

Generate harmless ICMP traffic to the loopback address:

```bash
ping -c 4 127.0.0.1
```

Capture the traffic using Wireshark on the loopback interface.

Apply:

```text
icmp
```

Inspect:

- Source IP.
- Destination IP.
- ICMP type.
- ICMP code.
- TTL where applicable.
- Packet length.

For IPv6, use:

```bash
ping -6 -c 4 ::1
```

If your system supports the command and IPv6 is enabled, capture and inspect the corresponding ICMPv6 traffic.

### Questions

1. What is the purpose of ICMP?
2. What is an ICMP Echo Request?
3. What is an ICMP Echo Reply?
4. What is TTL?
5. What is the difference between IPv4 and IPv6?

### Evidence

Save your capture as:

```text
assessment/captures/ip-icmp-analysis.pcapng
```

## 7. Task 4: TCP and UDP Analysis

**Marks: 15**

### Part A: TCP

Create a local HTTP server:

```bash
mkdir -p ~/practical-labs/module09/assessment/www
echo "Final Wireshark TCP Assessment" > ~/practical-labs/module09/assessment/www/index.html
cd ~/practical-labs/module09/assessment/www
python3 -m http.server 8000 --bind 127.0.0.1
```

Keep the server running.

In another terminal:

```bash
curl http://127.0.0.1:8000
```

Capture the traffic on `lo`.

Apply:

```text
tcp.port == 8000
```

Identify:

- TCP SYN.
- TCP SYN-ACK.
- TCP ACK.
- Application data.
- Connection termination.

### Part B: UDP

Generate a local UDP datagram using netcat if available.

Terminal 1:

```bash
nc -u -l 127.0.0.1 9999
```

Terminal 2:

```bash
echo "Final UDP Assessment" | nc -u -w 1 127.0.0.1 9999
```

Capture the traffic on `lo`.

Apply:

```text
udp.port == 9999
```

Identify:

- Source port.
- Destination port.
- UDP length.
- UDP checksum.

### Questions

1. What is the TCP three-way handshake?
2. What does SYN mean?
3. What does ACK mean?
4. What does RST mean?
5. What is the difference between TCP and UDP?
6. Why does UDP have lower header overhead?

### Evidence

Save:

```text
assessment/captures/tcp-analysis.pcapng
assessment/captures/udp-analysis.pcapng
```

Stop the HTTP and UDP servers after completing the exercises.

## 8. Task 5: DNS and DHCP Analysis

**Marks: 10**

### Part A: DNS

If internet access is available, generate a DNS query:

```bash
dig example.com
```

Capture the traffic and apply:

```text
dns
```

Inspect:

- Query name.
- Query type.
- Transaction ID.
- Response code.
- Answer records.

### Part B: DHCP

Inspect an existing authorized DHCP capture or wait for a natural DHCP exchange.

Apply:

```text
dhcp
```

If unsupported, try:

```text
bootp
```

Identify the DORA messages if available:

- Discover.
- Offer.
- Request.
- Acknowledge.

Do not restart networking or change VM settings simply to force DHCP traffic.

### Questions

1. What is DNS?
2. What is an A record?
3. What is an AAAA record?
4. What is DHCP?
5. What does DORA mean?
6. Which UDP ports are commonly used by DHCP for IPv4?

### Evidence

Save:

```text
assessment/captures/dns-analysis.pcapng
assessment/captures/dhcp-analysis.pcapng
```

If no DHCP exchange is available, document that limitation and use a previously saved authorized DHCP capture if available.

## 9. Task 6: HTTP and HTTPS Analysis

**Marks: 10**

Use your local HTTP server from Task 4.

Generate:

```bash
curl http://127.0.0.1:8000
```

Generate response headers:

```bash
curl -I http://127.0.0.1:8000
```

Generate a missing-page response:

```bash
curl -I http://127.0.0.1:8000/missing.html
```

Apply:

```text
http
```

Identify:

- HTTP request method.
- Request URI.
- Response status.
- Host header.
- Content type.

If internet access is available, generate HTTPS traffic:

```bash
curl -I https://example.com
```

Inspect the TLS handshake where visible.

Apply:

```text
tls
```

Explain why HTTPS application data is encrypted.

### Questions

1. What is HTTP?
2. What is HTTPS?
3. What is TLS?
4. What does HTTP 200 mean?
5. What does HTTP 404 mean?
6. Why can ordinary Wireshark captures not normally display HTTPS application content?

### Evidence

Save:

```text
assessment/captures/http-analysis.pcapng
assessment/captures/https-analysis.pcapng
```

If HTTPS is unavailable, document the limitation.

## 10. Task 7: Advanced Wireshark Analysis

**Marks: 10**

Open one of your assessment captures.

Complete the following:

### A. Protocol Hierarchy

Open:

```text
Statistics → Protocol Hierarchy
```

Record the protocols observed.

### B. Conversations

Open:

```text
Statistics → Conversations
```

Record at least one conversation if available.

### C. Endpoints

Open:

```text
Statistics → Endpoints
```

Record the endpoints observed.

### D. I/O Graphs

Open:

```text
Statistics → I/O Graphs
```

Describe the traffic pattern.

### E. Expert Information

Open:

```text
Analyze → Expert Information
```

Record any relevant messages.

### F. Filters

Test these filters:

```text
tcp
```

```text
udp
```

```text
dns
```

```text
icmp
```

```text
http
```

```text
tcp.flags.syn == 1
```

```text
frame.len > 100
```

Record which filters returned results.

A filter that returns no packets is not necessarily incorrect. The traffic may simply be absent.

## 11. Task 8: Network Security Investigation

**Marks: 10**

Use one or more of your saved captures.

Investigate:

- Unexpected connections.
- Repeated TCP SYN packets.
- TCP retransmissions.
- Connection resets.
- Unusual DNS queries.
- Unexpected protocols.
- Unusual traffic patterns.

Use these filters where relevant:

```text
tcp.analysis.retransmission
```

```text
tcp.flags.reset == 1
```

```text
dns.flags.rcode != 0
```

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

### Investigation Questions

1. Which observations deserve further investigation?
2. Which observations are confirmed?
3. Which observations are only hypotheses?
4. What legitimate explanations might exist?
5. What additional evidence would you collect?

Do not claim an attack occurred unless the available evidence supports that conclusion.

## 12. Task 9: Evidence Integrity

**Marks: 5**

Calculate SHA-256 hashes for your final captures.

Example:

```bash
sha256sum ~/practical-labs/module09/assessment/captures/*.pcapng
```

Save the checksums:

```bash
sha256sum ~/practical-labs/module09/assessment/captures/*.pcapng > ~/practical-labs/module09/assessment/evidence/checksums.sha256
```

Verify:

```bash
sha256sum -c ~/practical-labs/module09/assessment/evidence/checksums.sha256
```

Document:

- Capture filename.
- SHA-256 hash.
- Date of capture.
- Capture interface.

## 13. Task 10: Final Investigation Report

**Marks: 10**

Create your final report:

```bash
nano ~/practical-labs/module09/assessment/reports/module09-final-report.txt
```

Use the following structure:

```text
==================================================
MODULE 09 – FINAL WIRESHARK ASSESSMENT REPORT
==================================================

STUDENT INFORMATION

Student:
Course:
Module:
Assessment Date:

1. NETWORK ENVIRONMENT

Operating System:
Network Interface:
IPv4 Address:
IPv6 Address:
Default Gateway:

2. PACKET CAPTURE

Capture Files:
Capture Interfaces:
Packet Counts:
Capture Duration:

3. ETHERNET ANALYSIS

Source MAC:
Destination MAC:
EtherType:
Observations:

4. IP AND ICMP ANALYSIS

Source IP:
Destination IP:
ICMP Type:
ICMP Code:
TTL:
Observations:

5. TCP AND UDP ANALYSIS

TCP Ports:
UDP Ports:
TCP Handshake:
TCP Termination:
UDP Observations:

6. DNS AND DHCP ANALYSIS

DNS Queries:
DNS Responses:
DNS Record Types:
DHCP Messages:
DORA Stages:

7. HTTP AND HTTPS ANALYSIS

HTTP Methods:
HTTP Status Codes:
HTTP Headers:
TLS Observations:
Encryption Findings:

8. ADVANCED WIRESHARK ANALYSIS

Protocol Hierarchy:
Conversations:
Endpoints:
I/O Graphs:
Expert Information:

9. SECURITY INVESTIGATION

Suspicious Indicators:
Confirmed Findings:
Unverified Hypotheses:
False Positive Considerations:

10. EVIDENCE INTEGRITY

Capture Files:
SHA-256 Hashes:
Verification Results:

11. CONCLUSION

Technical Summary:
Key Findings:
Limitations:
Recommendations:

==================================================
END OF FINAL REPORT
==================================================
```

## 14. Assessment Marking Scheme

| Task | Marks |
|---|---:|
| 1. Network Environment Inspection | 10 |
| 2. Packet Capture and Ethernet Analysis | 10 |
| 3. IP and ICMP Analysis | 10 |
| 4. TCP and UDP Analysis | 15 |
| 5. DNS and DHCP Analysis | 10 |
| 6. HTTP and HTTPS Analysis | 10 |
| 7. Advanced Wireshark Analysis | 10 |
| 8. Network Security Investigation | 10 |
| 9. Evidence Integrity | 5 |
| 10. Final Investigation Report | 10 |
| **TOTAL** | **100** |

### Grading Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Needs Improvement |

**Passing mark: 50/100.**

## 15. Final Module Completion Checklist

- [ ] Completed Lesson 01 – Wireshark Fundamentals.
- [ ] Completed Lesson 02 – Packet Capture and Analysis.
- [ ] Completed Lesson 03 – Ethernet and MAC Address Analysis.
- [ ] Completed Lesson 04 – IP and ICMP Traffic Analysis.
- [ ] Completed Lesson 05 – TCP and UDP Traffic Analysis.
- [ ] Completed Lesson 06 – DNS and DHCP Traffic Analysis.
- [ ] Completed Lesson 07 – HTTP and HTTPS Traffic Analysis.
- [ ] Completed Lesson 08 – Wireshark Filters and Advanced Analysis.
- [ ] Completed Lesson 09 – Network Security Investigation.
- [ ] Completed Lesson 10 – Final Practical Assessment.
- [ ] Preserved packet captures.
- [ ] Generated evidence hashes.
- [ ] Prepared the final report.

## 16. Module 09 Achievement

By completing this module, you have developed practical skills in network traffic analysis.

You can now:

1. Capture network traffic.
2. Identify network protocols.
3. Inspect Ethernet and IP headers.
4. Analyze TCP and UDP communication.
5. Investigate DNS and DHCP.
6. Inspect HTTP and HTTPS traffic.
7. Apply advanced Wireshark filters.
8. Use TShark for command-line analysis.
9. Identify suspicious network patterns.
10. Preserve and document packet evidence.

These skills provide an important foundation for network security monitoring, digital forensics, incident response, and penetration testing.

**MODULE 09 STATUS: READY FOR COMPLETION.**

**Next Module: Module 10 – Network Enumeration.**
