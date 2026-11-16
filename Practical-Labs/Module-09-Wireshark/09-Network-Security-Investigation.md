
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 09: NETWORK SECURITY INVESTIGATION

## 1. Introduction

Network security investigation involves examining network activity to identify, understand, and respond to potential security incidents.

Cybersecurity analysts use packet analysis to investigate:

- Suspicious network connections.
- Unexpected communication.
- Repeated connection attempts.
- Unusual DNS activity.
- Network performance problems.
- Unexpected protocols and services.
- Possible data exposure.
- Communication involving compromised systems.

Wireshark is an important tool for this work because it allows analysts to inspect network packets and reconstruct communication patterns.

In this lesson, you will apply your previous Wireshark knowledge to defensive security investigations.

## 2. What Is Network Security Investigation?

Network security investigation is the process of examining network evidence to determine whether suspicious or unauthorized activity has occurred.

A typical investigation involves:

1. Identifying the problem.
2. Collecting relevant evidence.
3. Examining network traffic.
4. Identifying suspicious patterns.
5. Correlating evidence.
6. Evaluating potential impact.
7. Documenting findings.
8. Recommending corrective actions.

The objective is not simply to find unusual packets.

The objective is to establish what happened using reliable evidence.

## 3. Understanding Normal and Abnormal Network Behaviour

Before investigating suspicious traffic, analysts need to understand normal network behaviour.

For example, a computer may normally communicate with:

- DNS resolvers.
- Web servers.
- Email servers.
- Software update services.
- Authentication servers.
- Internal application servers.

Unexpected communication may deserve investigation.

However, unusual activity is not automatically malicious.

### Example

Suppose a workstation normally communicates with an approved internal DNS resolver.

An analyst notices that it has started using an unfamiliar DNS server.

Possible explanations include:

- A legitimate network configuration change.
- A VPN connection.
- A newly installed application.
- An unauthorized configuration change.
- A potentially malicious activity.

Further investigation is required.

## 4. Important Network Security Indicators

Security analysts may investigate several traffic patterns.

### A. Unexpected Destinations

A device communicates with an unfamiliar external or internal destination.

Possible causes:

- Legitimate software.
- Cloud services.
- Application updates.
- Unauthorized communication.
- Malware-related activity.

### B. Repeated TCP SYN Packets

Repeated SYN packets without successful connections may indicate:

- An unavailable server.
- Network filtering.
- Routing problems.
- Packet loss.
- Automated connection attempts.

### C. Repeated TCP Retransmissions

Retransmissions may indicate:

- Network congestion.
- Packet loss.
- Unstable connectivity.
- Server performance issues.

### D. Unexpected UDP Traffic

Unusual UDP communication may deserve review, especially when the destination or service is unexpected.

### E. DNS Anomalies

Examples include:

- Unexpected DNS resolvers.
- Repeated failed lookups.
- Unusually high query volumes.
- Unexpected domain names.

### F. Unexpected Listening Services

An unexpected service listening on a device may indicate:

- New software.
- Misconfiguration.
- Unauthorized service installation.
- A development server that was left running.

## 5. Investigation Workflow

A professional investigation follows a structured process.

### Step 1: Define the Scope

Determine:

- Which system is being investigated?
- Which network interface is involved?
- What time period is relevant?
- Which capture files are available?
- What is the reported problem?

### Step 2: Preserve Evidence

Preserve original packet captures.

Calculate SHA-256 hashes and avoid modifying original evidence.

### Step 3: Perform Initial Analysis

Inspect:

- Packet count.
- Protocol hierarchy.
- Endpoints.
- Conversations.
- Timestamps.
- Traffic distribution.

### Step 4: Apply Relevant Filters

Use filters to isolate relevant traffic.

Examples:

```text
tcp
```

```text
dns
```

```text
icmp
```

```text
tcp.analysis.retransmission
```

```text
tcp.flags.reset == 1
```

### Step 5: Correlate Findings

Compare packet observations with:

- System logs.
- Firewall logs.
- DNS logs.
- Application logs.
- Endpoint security alerts.

### Step 6: Document Results

Separate confirmed findings from assumptions.

### Step 7: Recommend Actions

Recommend actions based on evidence and the level of risk.

## 6. Preparing Your Investigation Lab

Create the lesson directory:

```bash
mkdir -p ~/practical-labs/module09/lab09/captures
mkdir -p ~/practical-labs/module09/lab09/reports
mkdir -p ~/practical-labs/module09/lab09/evidence
```

Your directory should look like:

```text
lab09/
├── captures/
├── reports/
└── evidence/
```

You will use your own previously generated packet captures and a fictional investigation dataset.

## 7. Practical Exercise 1: Inspect Network Connections

Inspect active TCP connections:

```bash
ss -tn
```

Inspect listening TCP and UDP services:

```bash
ss -tuln
```

Inspect processes associated with listening sockets:

```bash
sudo ss -tulnp
```

Inspect network interfaces:

```bash
ip address
```

Inspect the routing table:

```bash
ip route
```

Record:

- Active connections.
- Listening services.
- Network interface addresses.
- Default gateway.

These commands provide endpoint context for packet analysis.

## 8. Practical Exercise 2: Inspect Your Previous Packet Captures

Locate your earlier captures:

```bash
find ~/practical-labs/module09 -type f -name "*.pcapng"
```

Choose one or more captures from:

- Lesson 05: TCP and UDP.
- Lesson 06: DNS and DHCP.
- Lesson 07: HTTP and HTTPS.
- Lesson 08: Advanced analysis.

Open one capture:

```bash
wireshark
```

Use File → Open to select your saved capture.

Inspect the packet list.

Record:

- Number of packets.
- Capture duration.
- Protocols observed.
- Source and destination addresses.
- Relevant ports.

Do not interpret a small local capture as representative of a complete production network.

## 9. Practical Exercise 3: Investigate Repeated TCP Connections

Apply:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

This displays TCP connection initiation packets.

Next, inspect:

```text
tcp.analysis.retransmission
```

And:

```text
tcp.flags.reset == 1
```

Questions:

1. Are repeated SYN packets visible?
2. Are retransmissions present?
3. Are connection resets present?
4. Do these packets belong to the same connection?
5. Is there evidence of a completed handshake?

Record your observations.

A single SYN packet is normal. Repeated SYN packets can have several explanations.

Do not label the traffic malicious without supporting evidence.

## 10. Practical Exercise 4: Investigate DNS Behaviour

Open your DNS capture from Lesson 06.

Apply:

```text
dns
```

Inspect DNS queries:

```text
dns.flags.response == 0
```

Inspect DNS responses:

```text
dns.flags.response == 1
```

Inspect unsuccessful DNS responses:

```text
dns.flags.rcode != 0
```

Inspect A record queries:

```text
dns.qry.type == 1
```

Record:

- Query names.
- Query types.
- Response codes.
- DNS resolver addresses.
- Repeated queries.

Questions:

1. Are there repeated DNS queries?
2. Are there failed DNS responses?
3. Is the resolver expected?
4. Could application behaviour explain the observations?

## 11. Practical Exercise 5: Investigate Unexpected Protocols

Open a saved capture.

Select:

Statistics → Protocol Hierarchy.

Inspect the protocols present.

Look for protocols you did not expect.

Examples include:

- ARP.
- ICMP.
- TCP.
- UDP.
- DNS.
- HTTP.
- TLS.

Apply:

```text
tcp || udp
```

Then inspect:

```text
icmp
```

And:

```text
arp
```

Record which protocols are present.

Remember that protocol presence alone is not evidence of malicious behaviour.

## 12. Practical Exercise 6: Analyze Network Conversations

Open:

Statistics → Conversations.

Inspect the TCP and UDP tabs.

Record:

- Source endpoint.
- Destination endpoint.
- Source port.
- Destination port.
- Packet count.
- Bytes transferred.
- Conversation duration.

Look for communication that deserves additional investigation.

For example:

- A connection to an unexpected destination.
- A service using an unusual port.
- A large transfer that does not match expected application behaviour.

An unusual port is not automatically malicious. Many legitimate applications use non-standard ports.

## 13. Practical Exercise 7: Investigate Packet Timing

Open:

Statistics → I/O Graphs.

Inspect traffic over time.

Look for:

- Sudden traffic increases.
- Long periods without communication.
- Repeated traffic bursts.
- Differences between TCP and UDP traffic.

If possible, apply a filter such as:

```text
tcp
```

Then compare with:

```text
dns
```

Record the differences.

A traffic spike can result from normal activities such as software updates or file transfers.

## 14. Fictional Security Investigation Dataset

The following dataset is fictional and created for analytical practice.

It does not represent real network traffic.

### Scenario

A security team receives an alert about unusual network activity from a workstation.

The team collects these observations:

```text
09:00:01 TCP connection to approved web server
09:00:02 HTTP 200 response
09:05:10 DNS query for internal.example
09:05:11 DNS response successful
09:10:20 Repeated TCP SYN attempts to an internal server
09:10:24 No SYN-ACK observed
09:10:30 Additional SYN retransmission
09:15:01 DNS query for unknown-domain.example
09:15:02 DNS response NXDOMAIN
09:15:05 Another query for unknown-domain.example
09:15:06 DNS response NXDOMAIN
09:20:00 TCP connection reset
```

### Investigation Questions

1. Which protocols are involved?
2. Which activity appears unusual?
3. What could explain the repeated SYN attempts?
4. What does NXDOMAIN mean?
5. Why might repeated DNS failures deserve investigation?
6. What does a TCP reset indicate?
7. Which observations are confirmed?
8. Which explanations are still hypotheses?
9. What additional evidence would you request?
10. Can you conclude that the workstation is compromised?

### Expected Analytical Approach

Confirmed observations:

- TCP connection attempts occurred.
- Some attempts did not receive a visible SYN-ACK.
- DNS queries received NXDOMAIN responses.
- A TCP reset was observed.

Possible explanations:

- Server unavailability.
- Firewall filtering.
- Application configuration problems.
- Incorrect DNS names.
- Suspicious software behaviour.

The observations alone do not prove compromise.

## 15. Understanding False Positives

A false positive occurs when legitimate activity is incorrectly identified as malicious.

Examples:

- A software update creates many connections.
- A browser repeatedly requests a missing resource.
- A firewall causes a connection reset.
- A DNS resolver returns an error for an invalid domain.

Security analysts should avoid making conclusions based on a single indicator.

### Evidence Confidence

Use these categories:

| Confidence | Meaning |
|---|---|
| Low | Limited supporting evidence |
| Medium | Several observations support the finding |
| High | Strong, consistent, independently supported evidence |

Confidence describes how well the evidence supports a conclusion. It is not the same as severity.

## 16. Evidence Correlation

Evidence correlation means comparing information from different sources.

For example:

| Evidence source | Information |
|---|---|
| Wireshark | Network packets |
| Linux authentication logs | Login events |
| Firewall logs | Allowed and blocked connections |
| DNS logs | Name-resolution activity |
| Process information | Running applications |
| Application logs | Service behaviour |

Combining evidence can help distinguish a genuine security incident from a normal network problem.

## 17. Practical Exercise 8: Create a Security Timeline

Create a timeline from the fictional investigation dataset.

Use this format:

| Time | Observation | Protocol | Interpretation |
|---|---|---|---|
| 09:00:01 | Web connection | TCP | Normal activity |
| 09:05:10 | Internal DNS lookup | DNS | Normal lookup |
| 09:10:20 | Repeated connection attempts | TCP | Requires investigation |
| 09:15:01 | Failed domain lookup | DNS | NXDOMAIN |
| 09:20:00 | Connection reset | TCP | Requires context |

Add your own interpretation.

Clearly identify which entries are facts and which are hypotheses.

## 18. Practical Exercise 9: Use TShark for Investigation

Read an existing capture:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng
```

Display TCP packets:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -Y tcp
```

Display DNS packets:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -Y dns
```

Display TCP retransmissions:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -Y tcp.analysis.retransmission
```

Extract IP addresses:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -T fields -e ip.src -e ip.dst
```

If the Lesson 08 capture does not contain a particular protocol, the corresponding filter may return no results. Use an appropriate capture from an earlier lesson.

## 19. Security Investigation Report

Create your report:

```bash
nano ~/practical-labs/module09/lab09/reports/network-security-investigation.txt
```

Use this structure:

```text
==========================================
NETWORK SECURITY INVESTIGATION REPORT
==========================================

1. CASE INFORMATION

Case Reference:
Investigation Date:
Analyst:
System Under Investigation:
Evidence Source:

2. INVESTIGATION SCOPE

Systems Included:
Time Period:
Authorized Activities:
Limitations:

3. INITIAL OBSERVATIONS

Network Interfaces:
Active Connections:
Listening Services:
Protocols Observed:

4. TRAFFIC ANALYSIS

TCP Observations:
UDP Observations:
DNS Observations:
HTTP/HTTPS Observations:
ICMP Observations:

5. SUSPICIOUS INDICATORS

Unexpected Destinations:
Repeated Connections:
DNS Anomalies:
Retransmissions:
Unexpected Services:

6. TIMELINE

Time:
Observation:
Protocol:
Interpretation:

7. EVIDENCE CORRELATION

Packet Evidence:
System Logs:
Firewall Logs:
Application Logs:
Other Evidence:

8. FINDINGS

Finding 1:
Supporting Evidence:
Confidence:
Severity:

Finding 2:
Supporting Evidence:
Confidence:
Severity:

9. FALSE POSITIVE CONSIDERATIONS

Possible Legitimate Explanations:

10. RECOMMENDATIONS

Immediate Actions:
Further Investigation:
Preventive Measures:

11. CONCLUSION

Confirmed Findings:
Unverified Hypotheses:
Overall Assessment:

==========================================
END OF REPORT
==========================================
```

## 20. Evidence Preservation

Preserve the original packet capture.

Calculate its SHA-256 hash:

```bash
sha256sum ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng
```

Save the checksum:

```bash
sha256sum ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng > ~/practical-labs/module09/lab09/evidence/capture-checksum.sha256
```

Verify it:

```bash
sha256sum -c ~/practical-labs/module09/lab09/evidence/capture-checksum.sha256
```

The original evidence should remain unchanged.

Document when, where, and how evidence was collected.

## 21. Professional Investigation Principles

Always follow these principles:

1. Investigate only systems within your authorization.
2. Preserve original evidence.
3. Record timestamps and relevant context.
4. Protect sensitive packet captures.
5. Distinguish observations from assumptions.
6. Consider legitimate explanations.
7. Correlate evidence from multiple sources.
8. Document limitations.
9. Avoid unnecessary changes to the investigated system.
10. Make recommendations supported by evidence.

## 22. Knowledge Check

1. What is network security investigation?
2. Why is understanding normal network behaviour important?
3. What might repeated TCP SYN packets indicate?
4. What is a false positive?
5. What does NXDOMAIN mean?
6. Why should unusual DNS queries be investigated?
7. What is evidence correlation?
8. Why are network conversations useful?
9. What is the purpose of an investigation timeline?
10. What is the difference between a confirmed finding and a hypothesis?
11. Why should packet captures be hashed?
12. What does a TCP retransmission indicate?
13. Why is an unexpected port not automatically malicious?
14. What is evidence confidence?
15. Why should an analyst document investigation limitations?

## 23. Lesson Completion Checklist

- [ ] Understood network security investigation.
- [ ] Learned to identify unusual network behaviour.
- [ ] Studied the investigation workflow.
- [ ] Inspected active network connections.
- [ ] Reviewed previous packet captures.
- [ ] Investigated TCP connection attempts.
- [ ] Investigated DNS behaviour.
- [ ] Inspected protocol hierarchy.
- [ ] Analyzed network conversations.
- [ ] Examined I/O graphs.
- [ ] Studied false positives.
- [ ] Practiced evidence correlation.
- [ ] Created an investigation timeline.
- [ ] Used TShark for investigation.
- [ ] Preserved evidence.
- [ ] Created a professional security investigation report.

## 24. Lesson Summary

Network security investigation uses packet evidence and supporting information to identify and understand potentially suspicious network activity.

You learned how to investigate unusual TCP connections, DNS anomalies, unexpected protocols, traffic patterns, and network conversations.

You also studied false positives, evidence confidence, timeline construction, evidence correlation, and professional reporting.

**Next Lesson: Lesson 10 – Module 09 Practical Assessment.**
