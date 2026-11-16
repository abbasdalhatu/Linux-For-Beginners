
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 08: WIRESHARK FILTERS AND ADVANCED ANALYSIS

## 1. Introduction

Wireshark provides advanced features that help network administrators and cybersecurity analysts investigate network communication.

In previous lessons, you learned how to capture and inspect Ethernet, IP, ICMP, TCP, UDP, DNS, DHCP, HTTP, and HTTPS traffic.

This lesson focuses on advanced packet filtering, network statistics, conversations, endpoints, I/O graphs, expert information, and evidence analysis.

By the end, you should be able to investigate packet captures more efficiently and produce structured technical findings.

## 2. Understanding the Two Main Filter Types

Wireshark has two major filtering mechanisms:

1. Capture filters.
2. Display filters.

They serve different purposes.

### A. Capture Filters

Capture filters determine which packets Wireshark collects.

They are applied before packets are saved.

Example:

```text
tcp
```

This captures TCP packets only.

Another example:

```text
port 53
```

This captures packets involving port 53.

Capture filters use the Berkeley Packet Filter (BPF) syntax.

### B. Display Filters

Display filters determine which packets are shown after they have been captured.

They do not delete packets from the capture file.

Example:

```text
tcp.port == 443
```

This displays TCP packets involving port 443.

Another example:

```text
dns
```

This displays decoded DNS packets.

### Comparison

| Feature | Capture Filter | Display Filter |
|---|---|---|
| Applied | Before capture | After capture |
| Purpose | Controls collection | Controls visibility |
| Syntax | BPF | Wireshark display-filter syntax |
| Changes saved packets | Yes, by excluding packets | No |
| Example | `port 53` | `dns` |

Remember:

Capture filters control what enters the capture.

Display filters control what you see.

## 3. Advanced Display Filter Operators

Wireshark supports logical operators.

### AND

Both conditions must be true.

```text
tcp && ip.src == 192.168.1.10
```

### OR

At least one condition must be true.

```text
dns || dhcp
```

### NOT

Excludes matching packets.

```text
!arp
```

### Equality

```text
tcp.dstport == 443
```

### Inequality

```text
tcp.dstport != 443
```

### Greater Than

```text
frame.len > 1000
```

### Less Than

```text
frame.len < 100
```

### Combining Conditions

```text
tcp && tcp.port == 8000
```

```text
ip && !icmp
```

```text
tcp.port == 80 || tcp.port == 443
```

These filters help narrow large packet captures to relevant traffic.

## 4. Advanced Filtering Examples

### A. Filter by Source IP

```text
ip.src == 192.168.1.10
```

### B. Filter by Destination IP

```text
ip.dst == 192.168.1.20
```

### C. Filter by Both Addresses

```text
ip.addr == 192.168.1.10 && ip.addr == 192.168.1.20
```

### D. Filter by Source Port

```text
tcp.srcport == 443
```

### E. Filter by Destination Port

```text
udp.dstport == 53
```

### F. Filter by Packet Size

```text
frame.len > 1500
```

### G. Filter by Protocol

```text
icmp || dns || tcp
```

Note: The addresses above are examples. Replace them with values from your own captures.

## 5. Understanding Wireshark Statistics

Wireshark includes several statistical tools.

They help analysts understand traffic without manually inspecting every packet.

Important statistics include:

- Protocol Hierarchy.
- Conversations.
- Endpoints.
- I/O Graphs.
- Packet Lengths.
- Expert Information.

These tools are available through Wireshark's Statistics menu, although exact menu placement may vary by version.

## 6. Protocol Hierarchy

Protocol Hierarchy summarizes the protocols present in a capture.

It can show:

- Ethernet.
- IPv4 and IPv6.
- TCP.
- UDP.
- DNS.
- HTTP.
- TLS.
- Other decoded protocols.

### Practical Exercise

1. Open one of your saved packet captures.
2. Select Statistics.
3. Select Protocol Hierarchy.
4. Inspect the protocol distribution.
5. Record the protocols observed.

Example report:

```text
Protocol Hierarchy

Ethernet: Present
IPv4: Present
TCP: Present
HTTP: Present
DNS: Present
```

Your actual results will depend on the capture.

## 7. Conversations Analysis

A conversation represents communication between two network endpoints.

Wireshark can summarize conversations for different protocols.

### Practical Exercise

1. Open your packet capture.
2. Select Statistics.
3. Select Conversations.
4. Choose the relevant tab:
   - Ethernet.
   - IPv4.
   - IPv6.
   - TCP.
   - UDP.
5. Inspect the communication entries.

Important fields may include:

- Address A.
- Address B.
- Port A.
- Port B.
- Packets.
- Bytes.
- Duration.
- Relative start time.

Conversations help identify which endpoints exchanged traffic.

## 8. Endpoints Analysis

An endpoint is an individual network address participating in communication.

Wireshark can list endpoints observed in a capture.

### Practical Exercise

1. Open your capture.
2. Select Statistics.
3. Select Endpoints.
4. Inspect the IPv4 or IPv6 tab.
5. Record the addresses and packet counts.

Example:

| Endpoint | Packets |
|---|---:|
| 127.0.0.1 | Depends on capture |

Endpoint statistics can help identify active hosts.

They do not prove that a host is compromised or malicious.

## 9. I/O Graphs

I/O graphs display packet activity over time.

They help analysts identify:

- Traffic spikes.
- Periods of inactivity.
- Changes in network activity.
- Unusual traffic patterns.
- Possible performance problems.

### Practical Exercise

1. Open a packet capture.
2. Select Statistics.
3. Select I/O Graphs.
4. Observe the graph.
5. Change the interval if needed.
6. Apply a display filter to focus on one protocol.

Example filter:

```text
tcp
```

Another example:

```text
dns
```

Compare how different protocols contribute to the traffic pattern.

A traffic spike is not automatically evidence of an attack.

## 10. Packet Length Analysis

Packet length analysis helps identify the size distribution of captured packets.

In Wireshark:

1. Open a packet capture.
2. Select Statistics.
3. Select Packet Lengths.
4. Inspect the packet size distribution.

You can also use:

```text
frame.len > 1000
```

To display larger frames.

Remember that Ethernet frame size, IP packet size, and transport payload size are different measurements.

## 11. Expert Information

Wireshark's Expert Information feature highlights conditions that may deserve attention.

Examples include:

- Retransmissions.
- Duplicate acknowledgments.
- Malformed packets.
- Connection resets.
- Protocol warnings.
- Unusual communication patterns.

### Practical Exercise

1. Open a packet capture.
2. Select Analyze.
3. Select Expert Information.
4. Review the categories and messages.

Depending on the Wireshark version, categories may include:

- Chat.
- Note.
- Warn.
- Error.

Important:

Expert Information is an analysis aid, not an automatic security verdict.

A warning does not necessarily indicate an attack.

## 12. Following Network Streams

Wireshark can reconstruct supported communication streams.

Common options include:

- Follow TCP Stream.
- Follow UDP Stream.
- Follow HTTP Stream, where supported.

### Practical Exercise

1. Open your local HTTP capture from Lesson 07.
2. Select an HTTP packet.
3. Right-click.
4. Choose Follow.
5. Select TCP Stream.

Observe the request and response.

For a UDP capture, select a UDP packet and inspect Follow UDP Stream if the option is available.

Stream reconstruction is useful for understanding application communication.

Do not use it to inspect private communications without authorization.

## 13. Exporting Objects

Wireshark can extract certain objects from supported application protocols.

For example, HTTP captures may contain files that can be exported.

To inspect this feature:

1. Open your own HTTP capture.
2. Select File.
3. Select Export Objects.
4. Select HTTP if available.
5. Inspect the listed objects.

Only export files from authorized lab captures.

Avoid opening unknown files. Extracted objects can contain malicious content.

## 14. Advanced Capture Filters

Capture filters can reduce the amount of traffic collected.

Examples:

Capture TCP:

```text
tcp
```

Capture UDP:

```text
udp
```

Capture DNS:

```text
port 53
```

Capture HTTP:

```text
tcp port 80
```

Capture HTTPS over TCP:

```text
tcp port 443
```

Capture packets involving a particular host:

```text
host 192.168.1.10
```

Capture packets involving a particular network:

```text
net 192.168.1.0/24
```

Capture ICMP:

```text
icmp
```

Capture TCP traffic excluding port 22:

```text
tcp and not port 22
```

These examples use BPF capture-filter syntax, not display-filter syntax.

## 15. Practical Exercise: Advanced Local Capture

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module09/lab08/captures
mkdir -p ~/practical-labs/module09/lab08/reports
mkdir -p ~/practical-labs/module09/lab08/www
```

Create a local webpage:

```bash
echo "<h1>Advanced Wireshark Analysis</h1>" > ~/practical-labs/module09/lab08/www/index.html
```

Start the local HTTP server:

```bash
cd ~/practical-labs/module09/lab08/www
python3 -m http.server 8000 --bind 127.0.0.1
```

Keep this terminal open.

Open another terminal and generate traffic:

```bash
curl http://127.0.0.1:8000
```

Generate another request:

```bash
curl -I http://127.0.0.1:8000
```

Return to Wireshark.

Select the loopback interface (`lo`) and capture the traffic.

Stop the capture after the requests finish.

Save it as:

```text
~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng
```

Stop the local HTTP server with CTRL + C.

## 16. Practical Exercise: Apply Advanced Filters

Open the saved capture.

Apply the following display filters one at a time.

Show all TCP packets:

```text
tcp
```

Show packets involving port 8000:

```text
tcp.port == 8000
```

Show packets larger than 100 bytes:

```text
frame.len > 100
```

Show packets smaller than 100 bytes:

```text
frame.len < 100
```

Show TCP packets containing the SYN flag:

```text
tcp.flags.syn == 1
```

Show TCP reset packets:

```text
tcp.flags.reset == 1
```

Show HTTP GET requests:

```text
http.request.method == "GET"
```

Show packets that are either TCP or ICMP:

```text
tcp || icmp
```

Record which filters return results.

A filter may return no results if the corresponding traffic is absent from the capture.

## 17. Practical Exercise: Use Wireshark Statistics

Using your saved capture:

### Task A: Protocol Hierarchy

Open Statistics → Protocol Hierarchy.

Record the protocols detected.

### Task B: Conversations

Open Statistics → Conversations.

Inspect TCP communication.

Record:

- Source address.
- Destination address.
- Source port.
- Destination port.
- Number of packets.

### Task C: Endpoints

Open Statistics → Endpoints.

Record the endpoints observed.

### Task D: I/O Graphs

Open Statistics → I/O Graphs.

Observe the traffic pattern.

### Task E: Expert Information

Open Analyze → Expert Information.

Record any warnings or observations.

If no warnings appear, record that as your result.

Do not create artificial security findings just to complete the report.

## 18. Advanced Analysis with TShark

TShark provides command-line packet analysis.

Read your capture:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng
```

Count packets:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -q -z io,stat,1
```

Display TCP packets:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -Y tcp
```

Extract source and destination addresses:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -T fields -e ip.src -e ip.dst
```

Extract TCP ports:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -T fields -e tcp.srcport -e tcp.dstport
```

Count HTTP packets:

```bash
tshark -r ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng -Y http | wc -l
```

The count depends on your captured traffic and protocol decoding.

## 19. Defensive Investigation Applications

Advanced Wireshark analysis supports defensive network monitoring.

### A. Traffic Spikes

I/O graphs can reveal unexpected increases in traffic.

Possible explanations include:

- Software updates.
- File transfers.
- Application activity.
- Automated communication.
- Suspicious network behaviour.

### B. Repeated Retransmissions

Expert Information may highlight repeated retransmissions.

These may suggest:

- Packet loss.
- Congestion.
- Network instability.
- Server performance problems.

### C. Unusual Endpoints

Endpoint statistics may reveal devices or addresses that deserve further investigation.

Compare them with authorized network inventories.

### D. Unusual Conversations

Conversation statistics can help identify communication between unexpected endpoints.

### E. Protocol Distribution

Protocol Hierarchy can help reveal unexpected protocol usage.

Always compare findings with normal network behaviour before assigning a security classification.

## 20. Practical Investigation Scenario

A security analyst reviews a packet capture from a test workstation.

The analysis shows:

```text
Protocol Hierarchy:
Ethernet: Present
IPv4: Present
TCP: Present
HTTP: Present

Conversations:
Client: 127.0.0.1
Server: 127.0.0.1
Server Port: 8000

Expert Information:
No major warnings observed.
```

Answer:

1. Which protocols were identified?
2. What is a conversation?
3. Why is the loopback address important?
4. What does port 8000 represent in this exercise?
5. Does the absence of warnings prove that the network is completely secure?
6. What additional information would improve an investigation?

Expected reasoning:

The capture represents local HTTP communication between processes on the same system. The absence of warnings does not establish that the system is completely secure.

## 21. Evidence Preservation

Calculate the capture hash:

```bash
sha256sum ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng
```

Save the checksum:

```bash
sha256sum ~/practical-labs/module09/lab08/captures/advanced-analysis.pcapng > ~/practical-labs/module09/lab08/captures/checksums.sha256
```

Verify:

```bash
sha256sum -c ~/practical-labs/module09/lab08/captures/checksums.sha256
```

## 22. Practical Investigation Report

Create:

```bash
nano ~/practical-labs/module09/lab08/reports/advanced-wireshark-analysis.txt
```

Use this template:

```text
==========================================
ADVANCED WIRESHARK ANALYSIS REPORT
==========================================

1. INVESTIGATION INFORMATION

Date:
Operating System:
Wireshark Version:
Capture Interface:
Capture File:

2. FILTER ANALYSIS

Capture Filters Used:
Display Filters Used:
Relevant Results:

3. PROTOCOL HIERARCHY

Protocols Identified:
Protocol Observations:

4. CONVERSATION ANALYSIS

Source Address:
Destination Address:
Source Port:
Destination Port:
Packet Count:
Byte Count:

5. ENDPOINT ANALYSIS

Endpoints Identified:
Relevant Observations:

6. I/O GRAPH ANALYSIS

Traffic Pattern:
Traffic Spikes:
Observations:

7. EXPERT INFORMATION

Warnings:
Errors:
Notes:
Interpretation:

8. STREAM ANALYSIS

Stream Type:
Application Information:
Observations:

9. SECURITY ASSESSMENT

Potential Risks:
Confirmed Findings:
Unverified Hypotheses:
Evidence Limitations:

10. EVIDENCE INTEGRITY

Capture SHA-256:

11. CONCLUSION

Summary:
Recommendations:

==========================================
END OF REPORT
==========================================
```

## 23. Knowledge Check

1. What is a capture filter?
2. What is a display filter?
3. What is the main difference between the two?
4. What does the `&&` operator mean?
5. What does the `||` operator mean?
6. What does the `!` operator mean?
7. What is Protocol Hierarchy?
8. What is a network conversation?
9. What are endpoints?
10. What is an I/O graph?
11. What is Expert Information?
12. What does Follow TCP Stream do?
13. What is packet length analysis?
14. Why should exported objects be handled carefully?
15. Does a Wireshark warning automatically prove an attack?

## 24. Lesson Completion Checklist

- [ ] Understood capture filters.
- [ ] Understood display filters.
- [ ] Learned logical filter operators.
- [ ] Practiced advanced display filters.
- [ ] Studied Protocol Hierarchy.
- [ ] Analyzed conversations.
- [ ] Inspected endpoints.
- [ ] Used I/O graphs.
- [ ] Examined packet lengths.
- [ ] Reviewed Expert Information.
- [ ] Followed network streams.
- [ ] Learned about exporting objects.
- [ ] Performed advanced local packet capture.
- [ ] Used TShark for analysis.
- [ ] Preserved capture evidence.
- [ ] Created an advanced analysis report.

## 25. Lesson Summary

You have learned how to use Wireshark's advanced analysis capabilities.

You studied capture and display filters, logical operators, protocol hierarchy, conversations, endpoints, I/O graphs, packet lengths, expert information, and stream reconstruction.

You also practiced advanced local packet capture and command-line analysis with TShark.

**Next Lesson: Lesson 09 – Network Security Investigation.**
