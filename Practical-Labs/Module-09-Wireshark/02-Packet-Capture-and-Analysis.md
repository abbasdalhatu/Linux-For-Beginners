
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 02: PACKET CAPTURE AND ANALYSIS

## 1. Introduction

In Lesson 01, you learned what Wireshark is and performed your first packet capture.

In this lesson, we will go deeper into packet capture and learn how to examine network communications systematically.

Packet analysis is the process of examining captured network packets to understand communication between devices.

A security analyst uses packet analysis to answer questions such as:

- Which devices are communicating?
- What protocols are being used?
- Which ports are involved?
- When did communication occur?
- Was a connection successful?
- Are there unusual communication patterns?
- Is there evidence of a network problem?

## 2. Understanding Packet Capture

Packet capture is the process of collecting network packets from a network interface.

A packet capture can be performed in two ways:

### A. Live Capture

Live capture means collecting packets while network communication is happening.

Example:

You start Wireshark, select an interface, and observe packets as your computer communicates with other devices.

### B. Offline Capture

Offline analysis means opening an existing packet capture file and examining previously collected traffic.

Examples of capture files:

```text
first-capture.pcap
first-capture.pcapng
network-investigation.pcapng
```

Offline analysis is especially useful during security investigations because analysts can preserve evidence and examine it repeatedly.

## 3. Understanding Packet Capture Components

When capturing packets, several components are important.

### A. Source Address

The source address identifies where a packet originated.

Example:

```text
192.168.100.20
```

### B. Destination Address

The destination address identifies where the packet is being sent.

Example:

```text
192.168.100.1
```

### C. Protocol

The protocol identifies the communication rules associated with the packet.

Examples:

- TCP
- UDP
- ICMP
- DNS
- HTTP

### D. Source Port

The source port identifies the sending application's transport-layer endpoint.

### E. Destination Port

The destination port identifies the receiving application's transport-layer endpoint.

Example:

```text
Source IP:       192.168.100.20
Destination IP:  192.168.100.1
Protocol:        UDP
Source Port:     53000
Destination Port: 53
```

This could represent a DNS request from a client to a DNS server.

## 4. Preparing Your Packet Analysis Workspace

Open Kali Linux and create a dedicated directory:

```bash
mkdir -p ~/practical-labs/module09/lab02
```

Enter the directory:

```bash
cd ~/practical-labs/module09/lab02
```

Create folders for captures and reports:

```bash
mkdir captures reports
```

Your structure should look like this:

```text
lab02/
├── captures/
└── reports/
```

Keep your original packet captures in the captures directory.

## 5. Identifying Available Network Interfaces

Before capturing packets, identify the network interface.

Run:

```bash
ip addr
```

List network interfaces:

```bash
ip link
```

Inspect routing information:

```bash
ip route
```

Display the interface used to reach an external IP:

```bash
ip route get 1.1.1.1
```

Example:

```text
1.1.1.1 via 192.168.100.1 dev eth0 src 192.168.100.20
```

In this example:

- `eth0` is the interface.
- `192.168.100.1` is the gateway.
- `192.168.100.20` is the local IP address.

Use your actual interface name.

## 6. Capturing Packets with Wireshark

### Practical Exercise 1: Live Packet Capture

Objective: Capture normal network communication.

Step 1: Open Wireshark.

```bash
wireshark
```

Step 2: Select your active Kali network interface.

Step 3: Start capturing packets.

Step 4: Open another terminal.

Step 5: Generate controlled network activity:

```bash
ping -c 5 1.1.1.1
```

If internet access is unavailable, use:

```bash
ping -c 5 127.0.0.1
```

For loopback traffic, capture on the `lo` interface.

Step 6: Stop the capture.

Step 7: Examine the captured packets.

Look for:

- Source address.
- Destination address.
- Protocol.
- Packet length.
- Packet information.

Step 8: Save the capture as:

```text
captures/live-capture.pcapng
```

## 7. Understanding Packet Information

Select a packet in Wireshark.

You will normally see several protocol layers.

For example:

```text
Frame
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol
```

Each layer provides different information.

### A. Frame Layer

Provides capture-related information, including:

- Frame number.
- Frame length.
- Capture timestamp.

### B. Ethernet Layer

Provides local network information, including:

- Source MAC address.
- Destination MAC address.
- Ethernet type.

### C. IP Layer

Provides network-layer information, including:

- Source IP address.
- Destination IP address.
- TTL.
- IP protocol number.

### D. TCP Layer

Provides transport-layer information, including:

- Source port.
- Destination port.
- Sequence number.
- Acknowledgment number.
- TCP flags.

### E. Application Layer

Depending on the traffic, Wireshark may identify protocols such as:

- DNS.
- HTTP.
- TLS.
- DHCP.

Not every packet contains every layer.

## 8. Understanding Packet Length

Packet length refers to the size of a captured packet.

Wireshark displays packet length in bytes.

Example:

```text
Packet Length: 74 bytes
```

Packet sizes can help analysts understand communication patterns.

However, a large packet is not automatically malicious, and a small packet is not automatically safe.

Always interpret packet size alongside other evidence.

## 9. Understanding Timestamps

A timestamp indicates when a packet was captured.

Timestamps are important because they allow analysts to:

- Reconstruct communication sequences.
- Compare events.
- Identify delays.
- Correlate packet activity with system logs.
- Build incident timelines.

For example:

```text
10:00:00.100  DNS Query
10:00:00.250  DNS Response
10:00:00.400  TCP SYN
10:00:00.450  TCP SYN-ACK
10:00:00.500  TCP ACK
```

This example illustrates a possible sequence of DNS resolution followed by TCP connection establishment.

## 10. Applying Display Filters

Display filters help isolate relevant packets in a capture.

Try these filters in Wireshark.

Show all TCP packets:

```text
tcp
```

Show all UDP packets:

```text
udp
```

Show ICMP packets:

```text
icmp
```

Show DNS packets:

```text
dns
```

Show packets from a specific IP:

```text
ip.src == 192.168.100.20
```

Show packets going to a specific IP:

```text
ip.dst == 192.168.100.1
```

Show packets involving an IP:

```text
ip.addr == 192.168.100.20
```

Show packets using TCP port 443:

```text
tcp.port == 443
```

Show packets using UDP port 53:

```text
udp.port == 53
```

Replace example addresses with values from your own capture.

## 11. Understanding TCP Flags

TCP uses flags to manage communication.

Important TCP flags include:

| Flag | Meaning |
|---|---|
| SYN | Initiates a connection |
| ACK | Acknowledges received data |
| FIN | Gracefully closes a connection |
| RST | Resets a connection |
| PSH | Requests immediate delivery to the application |
| URG | Indicates urgent data |

A common TCP connection sequence is:

```text
Client                  Server
  |                        |
  |-------- SYN ---------->|
  |                        |
  |<----- SYN-ACK ----------|
  |                        |
  |-------- ACK ---------->|
  |                        |
  |   Connection Ready     |
```

This is known as the TCP three-way handshake.

We will study TCP in greater detail in Lesson 05.

## 12. Using TShark for Packet Analysis

TShark is Wireshark's command-line packet analysis tool.

Check its version:

```bash
tshark --version
```

List interfaces:

```bash
tshark -D
```

Read a saved capture:

```bash
tshark -r captures/live-capture.pcapng
```

Display only DNS packets:

```bash
tshark -r captures/live-capture.pcapng -Y dns
```

Display only TCP packets:

```bash
tshark -r captures/live-capture.pcapng -Y tcp
```

Display source and destination addresses:

```bash
tshark -r captures/live-capture.pcapng -T fields -e ip.src -e ip.dst
```

Display source and destination ports:

```bash
tshark -r captures/live-capture.pcapng -T fields -e tcp.srcport -e tcp.dstport
```

These commands allow analysts to extract useful information from packet captures.

## 13. Following Network Streams

Wireshark can reconstruct supported communication streams.

For example, with suitable TCP traffic:

1. Select a TCP packet.
2. Right-click the packet.
3. Select Follow.
4. Select TCP Stream.

Wireshark may display the reconstructed conversation.

This is useful for understanding communication between endpoints.

Important: encrypted HTTPS application data is not normally readable as ordinary HTTP text without the appropriate decryption material.

Do not attempt to collect other people's private communications.

## 14. Exporting Packet Captures

Wireshark allows you to save captured packets.

Use:

File → Save As

Choose:

```text
captures/packet-analysis.pcapng
```

You can also export selected packets using the relevant export options.

Preserve original evidence when conducting an investigation.

Do not overwrite the original capture during analysis.

## 15. Practical Exercise 2: Offline Packet Analysis

Objective: Analyze the capture from Lesson 01.

Step 1: Open Wireshark.

Step 2: Select:

```text
~/practical-labs/module09/lab01/first-capture.pcapng
```

Step 3: Examine the packet list.

Step 4: Apply:

```text
icmp
```

Step 5: Examine the available ICMP packets.

Step 6: Clear the filter.

Step 7: Apply:

```text
tcp
```

Step 8: Examine any TCP packets.

Step 9: Apply:

```text
dns
```

Step 10: Record the results.

Some filters may return no packets. This is normal if the original capture did not contain that protocol.

## 16. Practical Exercise 3: Create a New Capture with TShark

Objective: Capture a small amount of authorized lab traffic using TShark.

First, identify your interface:

```bash
tshark -D
```

Then capture a maximum of 30 packets:

```bash
sudo tshark -i eth0 -c 30 -w captures/tshark-capture.pcapng
```

Replace `eth0` with your actual interface.

If prompted for permission, use the authorized administrative method provided by Kali.

Read the saved capture:

```bash
tshark -r captures/tshark-capture.pcapng
```

If no traffic is captured, repeat the exercise while generating normal network activity.

## 17. Evidence Preservation

Packet captures can contain sensitive information.

Treat them as security evidence.

Good evidence-handling practices include:

1. Preserve the original capture.
2. Store captures in a dedicated directory.
3. Record when and where a capture was collected.
4. Record the interface used.
5. Avoid unnecessary modifications.
6. Calculate a SHA-256 hash.

Calculate the hash:

```bash
sha256sum captures/live-capture.pcapng
```

Save the hash:

```bash
sha256sum captures/live-capture.pcapng > captures/live-capture.sha256
```

Verify the file:

```bash
sha256sum -c captures/live-capture.sha256
```

A matching hash helps demonstrate that the file has not changed since the hash was calculated.

## 18. Practical Report

Create a report:

```bash
nano reports/packet-analysis-report.txt
```

Use this template:

```text
=========================================
PACKET CAPTURE AND ANALYSIS REPORT
=========================================

1. INVESTIGATION INFORMATION

Investigation Title:
Date:
Operating System:
Capture Interface:

2. CAPTURE INFORMATION

Capture File:
Capture Method:
Total Packets:
Capture Duration:

3. NETWORK OBSERVATIONS

Source Addresses:
Destination Addresses:
Protocols Identified:
Ports Identified:

4. PACKET ANALYSIS

Packet 1:
Protocol:
Source:
Destination:
Length:

Packet 2:
Protocol:
Source:
Destination:
Length:

5. FILTERS USED

Filter 1:
Result:

Filter 2:
Result:

Filter 3:
Result:

6. SECURITY OBSERVATIONS

Unusual Activity:
Potential Risks:
Evidence:
Limitations:

7. EVIDENCE INTEGRITY

Capture File:
SHA-256:

8. CONCLUSION

Summary:
Findings:
Further Investigation Required:

=========================================
END OF REPORT
=========================================
```

Save and exit using:

- CTRL + O
- ENTER
- CTRL + X

## 19. Knowledge Check

Answer these questions in your own understanding.

1. What is packet capture?
2. What is the difference between live capture and offline analysis?
3. What is a source IP address?
4. What is a destination IP address?
5. What information does the Ethernet layer provide?
6. What information does the TCP layer provide?
7. What is a timestamp?
8. What is the purpose of a Wireshark display filter?
9. What is TShark?
10. What does the TCP SYN flag represent?
11. What is the purpose of following a TCP stream?
12. Why should original packet captures be preserved?
13. Why is SHA-256 useful when handling packet evidence?

## 20. Lesson Completion Checklist

- [ ] Understood live packet capture.
- [ ] Understood offline packet analysis.
- [ ] Identified network interfaces.
- [ ] Captured network traffic.
- [ ] Examined packet layers.
- [ ] Understood packet timestamps and lengths.
- [ ] Applied display filters.
- [ ] Studied basic TCP flags.
- [ ] Used TShark to inspect captures.
- [ ] Saved a packet capture.
- [ ] Calculated a SHA-256 hash.
- [ ] Created a packet analysis report.

## 21. Lesson Summary

In this lesson, you learned how to collect and analyze network packets using Wireshark and TShark.

You explored packet addresses, protocols, ports, timestamps, packet layers, TCP flags, display filters, stream reconstruction, and evidence preservation.

You also practiced analyzing previously captured traffic.

**Next Lesson: Lesson 03 – Ethernet and MAC Address Analysis.**
