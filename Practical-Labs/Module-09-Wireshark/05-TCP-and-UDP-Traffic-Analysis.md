
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 05: TCP AND UDP TRAFFIC ANALYSIS

## 1. Introduction

TCP and UDP are two major transport-layer protocols used in computer networks.

They operate at Layer 4 of the OSI model, known as the Transport Layer.

Understanding these protocols is essential for:

- Network troubleshooting.
- Packet analysis.
- Application communication.
- Firewall monitoring.
- Security investigations.
- Network performance analysis.

In this lesson, you will learn how TCP and UDP work and how to analyze their packets using Wireshark.

## 2. What Is the Transport Layer?

The Transport Layer provides communication between applications running on different devices.

Its responsibilities include:

- Identifying applications through port numbers.
- Transporting application data.
- Managing communication.
- Supporting reliable delivery where required.
- Detecting certain transmission errors.

The two major transport protocols are:

1. TCP – Transmission Control Protocol.
2. UDP – User Datagram Protocol.

## 3. What Is TCP?

TCP stands for Transmission Control Protocol.

It is a connection-oriented transport protocol.

TCP provides:

- Reliable data delivery.
- Ordered data delivery.
- Error detection.
- Retransmission of lost data.
- Flow control.
- Congestion control.

TCP establishes a connection before normal application data transfer.

Examples of applications that commonly use TCP include:

- HTTPS.
- SSH.
- Email protocols.
- File transfers.

## 4. Understanding TCP Ports

A port identifies a transport-layer endpoint used by an application.

Common TCP ports include:

| Port | Common service |
|---|---|
| 20/21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3306 | MySQL |
| 3389 | RDP |

A port number alone does not prove which application is communicating. Analysts should inspect protocol details and service behaviour.

## 5. TCP Three-Way Handshake

Before a normal TCP connection is established, the client and server perform a three-way handshake.

The three stages are:

1. SYN.
2. SYN-ACK.
3. ACK.

### Handshake Diagram

```text
CLIENT                         SERVER
  |                               |
  |----------- SYN -------------->|
  |                               |
  |<--------- SYN-ACK -------------|
  |                               |
  |----------- ACK -------------->|
  |                               |
  |       CONNECTION READY        |
```

### Step 1: SYN

The client requests a TCP connection.

### Step 2: SYN-ACK

The server acknowledges the request and sends its own SYN.

### Step 3: ACK

The client acknowledges the server's response.

The connection is now established.

## 6. Important TCP Flags

| Flag | Meaning |
|---|---|
| SYN | Initiates connection |
| ACK | Acknowledges received data |
| FIN | Requests orderly connection termination |
| RST | Resets a connection |
| PSH | Requests prompt delivery to the receiving application |
| URG | Indicates urgent data |

These flags help analysts understand TCP communication.

## 7. Understanding TCP Sequence Numbers

TCP uses sequence numbers to track data bytes.

Sequence numbers help:

- Maintain correct data order.
- Detect missing data.
- Support retransmission.
- Track communication progress.

TCP also uses acknowledgment numbers to indicate the next byte expected.

Wireshark can display both relative and absolute sequence numbers.

For beginners, relative sequence numbers are often easier to understand.

## 8. TCP Connection Termination

A normal TCP connection may be closed using FIN and ACK messages.

Simplified example:

```text
CLIENT                         SERVER
  |                               |
  |----------- FIN -------------->|
  |<---------- ACK ---------------|
  |<---------- FIN ---------------|
  |----------- ACK -------------->|
  |                               |
  |       CONNECTION CLOSED       |
```

A connection may also be reset using an RST flag.

A reset is not automatically malicious. It can occur for legitimate networking reasons.

## 9. TCP Retransmissions

TCP may retransmit data when delivery is not acknowledged as expected.

Possible causes include:

- Packet loss.
- Network congestion.
- Unstable connections.
- Receiver delays.
- Network equipment problems.

Wireshark can identify some retransmission events.

A common display filter is:

```text
tcp.analysis.retransmission
```

Other useful analysis filters include:

```text
tcp.analysis.duplicate_ack
```

```text
tcp.analysis.lost_segment
```

These observations help diagnose network problems, but they require context.

## 10. What Is UDP?

UDP stands for User Datagram Protocol.

Unlike TCP, UDP is connectionless.

UDP does not establish a TCP-style connection before sending data.

UDP provides:

- Low protocol overhead.
- Datagram-based communication.
- No built-in retransmission.
- No built-in delivery guarantee.
- No built-in ordering guarantee.

Applications may implement their own reliability mechanisms when necessary.

Examples of UDP use include:

- DNS.
- DHCP.
- Voice and video applications.
- Some streaming applications.
- Online gaming.

## 11. Understanding UDP Ports

Common UDP ports include:

| Port | Common service |
|---|---|
| 53 | DNS |
| 67 | DHCP server |
| 68 | DHCP client |
| 69 | TFTP |
| 123 | NTP |
| 161 | SNMP |
| 500 | IKE |
| 514 | Syslog |

Some services use both TCP and UDP under different circumstances.

## 12. TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Built-in | Not guaranteed |
| Ordering | Maintained | Not guaranteed |
| Retransmission | Supported | Not built in |
| Header size | Minimum 20 bytes | 8 bytes |
| Speed overhead | Generally higher | Generally lower |
| Typical use | Web, SSH, file transfer | DNS, DHCP, streaming |

Remember: UDP is not automatically faster in every application, and TCP is not automatically more secure.

## 13. Understanding TCP and UDP Headers

### TCP Header

Important TCP header fields include:

- Source port.
- Destination port.
- Sequence number.
- Acknowledgment number.
- Header length.
- Flags.
- Window size.
- Checksum.
- Urgent pointer.

### UDP Header

UDP contains four main fields:

- Source port.
- Destination port.
- Length.
- Checksum.

In Wireshark, expand the relevant protocol section to inspect these fields.

## 14. Preparing Your Lab Environment

Create your lesson directory:

```bash
mkdir -p ~/practical-labs/module09/lab05/captures
mkdir -p ~/practical-labs/module09/lab05/reports
```

Your directory should look like:

```text
lab05/
├── captures/
└── reports/
```

## 15. Practical Exercise 1: Inspect TCP and UDP Listening Ports

Open a Kali terminal.

Inspect listening TCP and UDP sockets:

```bash
sudo ss -tuln
```

Meaning:

- `t`: TCP.
- `u`: UDP.
- `l`: Listening.
- `n`: Numeric addresses and ports.

Inspect TCP connections:

```bash
sudo ss -tn
```

Inspect UDP sockets:

```bash
sudo ss -un
```

Inspect listening ports with process information:

```bash
sudo ss -tulnp
```

Record several entries.

| Protocol | Local Address | Port | State |
|---|---|---|---|
| TCP | | | |
| UDP | | | |

Your actual results depend on your running services.

## 16. Practical Exercise 2: Capture TCP Traffic

### Objective

Capture and examine TCP packets generated by your Kali VM.

Step 1: Open Wireshark.

```bash
wireshark
```

Step 2: Select your active network interface.

Step 3: Start capturing.

Step 4: Generate normal TCP activity using the local system:

```bash
curl -I http://example.com
```

This sends an HTTP request to a public example domain. Only use it if your network permits the connection.

If internet access is unavailable, skip this request and use any existing TCP traffic in your capture.

Step 5: Stop the capture.

Step 6: Apply:

```text
tcp
```

Step 7: Select a TCP packet.

Expand:

```text
Transmission Control Protocol
```

Inspect:

- Source port.
- Destination port.
- Sequence number.
- Acknowledgment number.
- TCP flags.
- Window size.

Step 8: Save your capture:

```text
~/practical-labs/module09/lab05/captures/tcp-analysis.pcapng
```

## 17. Practical Exercise 3: Identify the TCP Handshake

In Wireshark, open your TCP capture.

Apply:

```text
tcp.flags.syn == 1
```

This displays TCP packets with the SYN flag set.

To display SYN packets without ACK:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

To display SYN-ACK packets:

```text
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

To display ACK packets:

```text
tcp.flags.ack == 1
```

Look for a matching sequence of:

1. SYN.
2. SYN-ACK.
3. ACK.

The packets must belong to the same connection. Do not assume unrelated SYN and ACK packets form one handshake.

## 18. Practical Exercise 4: Capture UDP Traffic

### Objective

Generate and inspect UDP packets.

Create a local UDP listener using netcat if it is installed:

```bash
nc -u -l 127.0.0.1 9999
```

Leave that terminal open.

In another terminal, send a message:

```bash
echo "Wireshark UDP laboratory" | nc -u -w 1 127.0.0.1 9999
```

This sends a UDP datagram between processes on your own Kali VM.

If the netcat listener does not exit automatically, press CTRL + C after the test.

Capture on the loopback interface (`lo`) in Wireshark.

Apply:

```text
udp
```

Look for:

- Source IP.
- Destination IP.
- Source port.
- Destination port.
- UDP length.
- UDP checksum.

Save the capture as:

```text
~/practical-labs/module09/lab05/captures/udp-analysis.pcapng
```

## 19. Understanding TCP and UDP Filters

### TCP Filters

Show all TCP traffic:

```text
tcp
```

Show TCP port 443:

```text
tcp.port == 443
```

Show TCP destination port 22:

```text
tcp.dstport == 22
```

Show TCP source port 80:

```text
tcp.srcport == 80
```

Show TCP SYN packets:

```text
tcp.flags.syn == 1
```

Show TCP reset packets:

```text
tcp.flags.reset == 1
```

Show retransmissions:

```text
tcp.analysis.retransmission
```

### UDP Filters

Show all UDP traffic:

```text
udp
```

Show UDP port 53:

```text
udp.port == 53
```

Show UDP destination port 123:

```text
udp.dstport == 123
```

Show UDP source port 68:

```text
udp.srcport == 68
```

### Combined Filters

Show TCP or UDP traffic:

```text
tcp || udp
```

Show TCP traffic on port 443 or UDP traffic on port 53:

```text
tcp.port == 443 || udp.port == 53
```

## 20. Understanding TCP Stream Analysis

Wireshark can reconstruct supported TCP communication.

To follow a stream:

1. Open your TCP capture.
2. Select a TCP packet.
3. Right-click.
4. Select Follow.
5. Select TCP Stream.

Wireshark may display the communication between the endpoints.

This is useful for investigating application behaviour.

Important:

- HTTPS content is encrypted under normal circumstances.
- Not all TCP traffic contains readable application data.
- A stream may contain sensitive information.
- Do not collect or inspect other people's private communications.

## 21. Practical Exercise 5: Analyze TCP and UDP with TShark

Read the TCP capture:

```bash
tshark -r ~/practical-labs/module09/lab05/captures/tcp-analysis.pcapng
```

Display TCP packets:

```bash
tshark -r ~/practical-labs/module09/lab05/captures/tcp-analysis.pcapng -Y tcp
```

Extract TCP ports:

```bash
tshark -r ~/practical-labs/module09/lab05/captures/tcp-analysis.pcapng -T fields -e tcp.srcport -e tcp.dstport
```

Read the UDP capture:

```bash
tshark -r ~/practical-labs/module09/lab05/captures/udp-analysis.pcapng
```

Display UDP packets:

```bash
tshark -r ~/practical-labs/module09/lab05/captures/udp-analysis.pcapng -Y udp
```

Extract UDP ports:

```bash
tshark -r ~/practical-labs/module09/lab05/captures/udp-analysis.pcapng -T fields -e udp.srcport -e udp.dstport
```

## 22. Defensive Security Applications

TCP and UDP analysis can help identify unusual network behaviour.

### A. Unexpected Listening Ports

An unexpected listening port may indicate:

- A newly installed service.
- A misconfigured application.
- An unauthorized service.
- A legitimate development tool.

Investigate the owning process and service before drawing conclusions.

### B. Repeated TCP Connection Attempts

Repeated connection attempts may indicate:

- Application failure.
- Network problems.
- Incorrect configuration.
- Automated connection activity.
- Unauthorized probing.

### C. Unusual UDP Traffic

Unusual UDP traffic may require investigation, especially when it involves unexpected destinations or services.

However, UDP is widely used by legitimate applications.

### D. TCP Resets

Frequent TCP resets may indicate:

- Application errors.
- Firewall behaviour.
- Service interruptions.
- Connection failures.

They are not automatically proof of an attack.

## 23. Practical Investigation Scenario

Imagine a workstation reports that an internal application is unavailable.

You receive a packet capture showing:

```text
10:00:01 Client sends SYN
10:00:02 Client retransmits SYN
10:00:04 Client retransmits SYN
10:00:07 No SYN-ACK observed
```

Answer these questions:

1. Which device initiated the connection?
2. What does SYN represent?
3. What does repeated SYN retransmission suggest?
4. Was a successful TCP handshake observed?
5. What additional evidence would you collect?
6. Could a firewall or unavailable service explain the behaviour?

Do not immediately classify this as an attack.

Possible causes include an unavailable server, filtering, routing problems, or packet loss.

## 24. Evidence Preservation

Calculate hashes for both captures:

```bash
sha256sum ~/practical-labs/module09/lab05/captures/tcp-analysis.pcapng
```

```bash
sha256sum ~/practical-labs/module09/lab05/captures/udp-analysis.pcapng
```

Save checksum records:

```bash
sha256sum ~/practical-labs/module09/lab05/captures/tcp-analysis.pcapng ~/practical-labs/module09/lab05/captures/udp-analysis.pcapng > ~/practical-labs/module09/lab05/captures/checksums.sha256
```

Verify:

```bash
sha256sum -c ~/practical-labs/module09/lab05/captures/checksums.sha256
```

## 25. Practical Investigation Report

Create:

```bash
nano ~/practical-labs/module09/lab05/reports/tcp-udp-analysis-report.txt
```

Use this template:

```text
==========================================
TCP AND UDP TRAFFIC ANALYSIS REPORT
==========================================

1. INVESTIGATION INFORMATION

Date:
Operating System:
Network Interface:

2. TCP ANALYSIS

Capture File:
Source Address:
Destination Address:
Source Port:
Destination Port:
TCP Flags:
Handshake Observed:
Retransmissions:

3. UDP ANALYSIS

Capture File:
Source Address:
Destination Address:
Source Port:
Destination Port:
UDP Length:
UDP Checksum:

4. SOCKET INSPECTION

Listening TCP Ports:
Listening UDP Ports:
Associated Processes:

5. WIRESHARK FILTERS

TCP Filters:
UDP Filters:
Combined Filters:

6. SECURITY OBSERVATIONS

Unusual Connections:
Unexpected Ports:
Possible Risks:
Evidence Limitations:

7. EVIDENCE INTEGRITY

TCP Capture SHA-256:
UDP Capture SHA-256:

8. CONCLUSION

Summary:
Confirmed Findings:
Unverified Hypotheses:
Recommendations:

==========================================
END OF REPORT
==========================================
```

## 26. Knowledge Check

1. What is the Transport Layer?
2. What does TCP stand for?
3. What does UDP stand for?
4. What is the difference between TCP and UDP?
5. What is a port number?
6. Explain the TCP three-way handshake.
7. What does the SYN flag do?
8. What does the ACK flag do?
9. What does the RST flag do?
10. What is a TCP sequence number?
11. What is a TCP retransmission?
12. Why does UDP not guarantee delivery?
13. What is the minimum TCP header size?
14. What is the UDP header size?
15. How can Wireshark help troubleshoot failed TCP connections?

## 27. Lesson Completion Checklist

- [ ] Understood the Transport Layer.
- [ ] Learned TCP fundamentals.
- [ ] Learned UDP fundamentals.
- [ ] Studied common TCP and UDP ports.
- [ ] Understood the TCP three-way handshake.
- [ ] Studied TCP flags and sequence numbers.
- [ ] Learned TCP retransmissions.
- [ ] Inspected listening sockets.
- [ ] Captured TCP traffic.
- [ ] Captured local UDP traffic.
- [ ] Applied TCP display filters.
- [ ] Applied UDP display filters.
- [ ] Used TShark for analysis.
- [ ] Preserved packet evidence.
- [ ] Created a TCP and UDP investigation report.

## 28. Lesson Summary

You have learned the differences between TCP and UDP and how these protocols transport application data.

You studied TCP connection establishment, flags, sequence numbers, retransmissions, UDP datagrams, and port numbers.

You also practiced capturing local TCP and UDP traffic, applying Wireshark filters, inspecting sockets, and extracting packet information with TShark.

**Next Lesson: Lesson 06 – DNS and DHCP Traffic Analysis.**
