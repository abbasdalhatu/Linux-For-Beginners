
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 04: IP AND ICMP TRAFFIC ANALYSIS

## 1. Introduction

In this lesson, you will learn how devices communicate across networks using IP and ICMP.

These protocols are essential to networking and cybersecurity.

You will study:

- IPv4 and IPv6.
- IP addresses.
- IP packet structure.
- Subnet masks and network identification.
- TTL and hop limits.
- ICMP messages.
- Ping analysis.
- Traceroute analysis.
- Wireshark IP and ICMP filters.
- Network troubleshooting and defensive investigation.

## 2. What Is Internet Protocol (IP)?

Internet Protocol (IP) is responsible for addressing and routing packets between networks.

It operates at Layer 3 of the OSI model, known as the Network Layer.

IP helps determine:

- Where a packet originates.
- Where a packet is going.
- How routers forward packets.
- Whether a packet belongs to IPv4 or IPv6.

There are two major versions:

1. IPv4.
2. IPv6.

## 3. Understanding IPv4

IPv4 uses 32-bit addresses.

An IPv4 address is commonly written as four decimal numbers separated by periods.

Example:

```text
192.168.1.10
```

Each section is called an octet.

An octet contains 8 bits.

Therefore:

```text
8 + 8 + 8 + 8 = 32 bits
```

Each octet can have a value between 0 and 255.

### Example IPv4 Addresses

| Address | Typical purpose |
|---|---|
| 192.168.1.10 | Private network |
| 10.0.0.5 | Private network |
| 172.16.1.20 | Private network |
| 127.0.0.1 | IPv4 loopback |
| 8.8.8.8 | Public DNS resolver |

Private IPv4 ranges include:

- 10.0.0.0/8
- 172.16.0.0/12
- 192.168.0.0/16

## 4. Understanding IPv6

IPv6 was developed to provide a much larger address space.

It uses 128-bit addresses.

Example:

```text
2001:db8::1
```

IPv6 addresses are written using hexadecimal groups separated by colons.

Important IPv6 examples:

| Address | Purpose |
|---|---|
| ::1 | Loopback |
| :: | Unspecified address |
| fe80::/10 | Link-local range |
| 2001:db8::/32 | Documentation prefix |

IPv6 is increasingly important in modern networks.

## 5. What Is an IP Packet?

An IP packet contains information required for network communication.

An IPv4 header includes:

| Field | Purpose |
|---|---|
| Version | IP version |
| Header Length | IPv4 header size |
| Total Length | Entire IPv4 packet size |
| Identification | Fragment identification |
| Flags | Fragmentation control |
| Fragment Offset | Fragment position |
| TTL | Limits packet lifetime |
| Protocol | Identifies upper-layer protocol |
| Header Checksum | IPv4 header error check |
| Source IP | Sending address |
| Destination IP | Receiving address |

Wireshark displays these fields under:

```text
Internet Protocol Version 4
```

Not every IP packet contains the same payload.

## 6. Understanding TTL

TTL means Time To Live.

In IPv4, TTL limits how many router forwarding steps a packet can take.

Each router normally decreases the TTL by at least one.

If the TTL reaches zero, the packet is discarded and a router may send an ICMP Time Exceeded message.

Example:

```text
Initial TTL: 64
After router 1: 63
After router 2: 62
After router 3: 61
```

TTL helps prevent packets from circulating indefinitely.

IPv6 uses a corresponding field called Hop Limit.

## 7. Understanding the IP Protocol Field

The IPv4 Protocol field identifies the next protocol carried in the IP payload.

Examples:

| Protocol Number | Protocol |
|---|---|
| 1 | ICMP |
| 6 | TCP |
| 17 | UDP |
| 41 | IPv6 encapsulation |
| 47 | GRE |
| 50 | ESP |

This information helps Wireshark identify packet types.

## 8. What Is ICMP?

ICMP stands for Internet Control Message Protocol.

It is used for network diagnostics and error reporting.

ICMP helps devices and routers communicate information about network conditions.

Common uses include:

- Ping.
- Destination unreachable messages.
- Time exceeded messages.
- Path diagnostics.

ICMP is associated with the Network Layer.

For IPv4, ICMP uses IP protocol number 1.

ICMPv6 is part of IPv6 networking and uses a different protocol number.

## 9. Important ICMP Message Types

| Type | Message | Purpose |
|---|---|---|
| 0 | Echo Reply | Response to an echo request |
| 3 | Destination Unreachable | Destination cannot be reached |
| 5 | Redirect | Suggests an alternative route |
| 8 | Echo Request | Requests a response |
| 11 | Time Exceeded | TTL expired |
| 12 | Parameter Problem | Invalid IP header parameter |

These are common ICMPv4 types.

ICMPv6 has its own message type definitions.

## 10. Understanding Ping

Ping is a network diagnostic utility.

It normally sends ICMP Echo Request messages and waits for Echo Reply messages.

Example:

```bash
ping -c 4 127.0.0.1
```

This sends four requests to the local loopback address.

Another example:

```bash
ping -c 4 1.1.1.1
```

This tests connectivity to an external destination, if permitted by your network.

Example output:

```text
64 bytes from 127.0.0.1:
icmp_seq=1 ttl=64 time=0.050 ms
```

Important information:

- `icmp_seq`: Request sequence number.
- `ttl`: Remaining packet lifetime.
- `time`: Round-trip time.

A failed ping does not necessarily mean a device is offline. Firewalls may block ICMP.

## 11. Practical Exercise 1: Inspect IP Configuration

Open Kali terminal.

Run:

```bash
ip addr
```

Identify:

- Interface name.
- IPv4 address.
- IPv6 address, if available.
- MAC address.

Inspect routing:

```bash
ip route
```

Inspect IPv6 routing:

```bash
ip -6 route
```

Record:

```text
Interface:
IPv4 Address:
IPv6 Address:
Default Gateway:
MAC Address:
```

Do not publish your private network details.

## 12. Practical Exercise 2: Generate ICMP Traffic

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module09/lab04/captures
mkdir -p ~/practical-labs/module09/lab04/reports
```

Open Wireshark:

```bash
wireshark
```

Select your loopback interface:

```text
lo
```

Start capturing.

In another terminal, run:

```bash
ping -c 5 127.0.0.1
```

Stop the capture.

Apply this display filter:

```text
icmp
```

You should see ICMP Echo Request and Echo Reply packets if the capture worked.

Inspect the packets.

Look for:

- Source IP.
- Destination IP.
- ICMP Type.
- ICMP Code.
- Sequence Number.
- TTL.

Save the capture as:

```text
~/practical-labs/module09/lab04/captures/icmp-loopback.pcapng
```

## 13. Understanding ICMP Echo Request and Reply

A normal ping exchange looks like this:

```text
Kali Linux                  Loopback
    |                           |
    |------ Echo Request ------>|
    |                           |
    |<------- Echo Reply -------|
    |                           |
    |------ Echo Request ------>|
    |                           |
    |<------- Echo Reply -------|
```

In Wireshark:

- Echo Request uses ICMP Type 8.
- Echo Reply uses ICMP Type 0.

The request and response sequence numbers help associate each exchange.

## 14. Wireshark IPv4 Display Filters

Show IPv4 packets:

```text
ip
```

Show packets from an IP address:

```text
ip.src == 192.168.1.10
```

Show packets going to an IP address:

```text
ip.dst == 192.168.1.1
```

Show traffic involving an address:

```text
ip.addr == 192.168.1.10
```

Show packets with TTL equal to 64:

```text
ip.ttl == 64
```

Show packets with TTL less than 10:

```text
ip.ttl < 10
```

Replace example addresses with the addresses in your capture.

## 15. Wireshark IPv6 Display Filters

Show IPv6 traffic:

```text
ipv6
```

Show packets from a specific IPv6 address:

```text
ipv6.src == 2001:db8::1
```

Show packets going to a specific IPv6 address:

```text
ipv6.dst == 2001:db8::2
```

Show IPv6 traffic involving an address:

```text
ipv6.addr == 2001:db8::1
```

These examples use documentation addresses.

## 16. Wireshark ICMP Display Filters

Show ICMPv4 traffic:

```text
icmp
```

Show ICMPv6 traffic:

```text
icmpv6
```

Show Echo Requests:

```text
icmp.type == 8
```

Show Echo Replies:

```text
icmp.type == 0
```

Show Destination Unreachable messages:

```text
icmp.type == 3
```

Show Time Exceeded messages:

```text
icmp.type == 11
```

Display filters can be combined.

Example:

```text
ip.addr == 127.0.0.1 && icmp
```

This displays ICMP packets involving the loopback address.

## 17. Understanding Traceroute

Traceroute is a diagnostic tool that helps identify the network path packets take toward a destination.

On Linux, the command is commonly:

```bash
traceroute 1.1.1.1
```

If it is unavailable, check:

```bash
which traceroute
```

Kali also provides:

```bash
tracepath 1.1.1.1
```

Traceroute commonly works by sending packets with increasing TTL values.

Example:

```text
TTL 1  -> Router 1
TTL 2  -> Router 2
TTL 3  -> Router 3
TTL 4  -> Destination
```

When a router receives a packet whose TTL expires, it may return an ICMP Time Exceeded message.

Note:

- Some routers do not respond.
- Firewalls may filter diagnostic messages.
- Asterisks do not necessarily mean a network is broken.
- Different traceroute modes may use different transport protocols.

## 18. Practical Exercise 3: Analyze a Local Traceroute

Use the loopback address for a safe local exercise:

```bash
tracepath 127.0.0.1
```

If available, also run:

```bash
traceroute 127.0.0.1
```

Observe the output.

Explain:

1. What destination was used?
2. How many hops were displayed?
3. Why is a local loopback route different from an external route?

Do not troubleshoot or scan networks you do not own or administer.

## 19. IP and ICMP Security Considerations

IP and ICMP are useful, but they can also appear in security investigations.

Analysts may investigate:

- Unexpected ICMP traffic.
- Repeated destination unreachable messages.
- Unusual TTL patterns.
- Unexpected external destinations.
- Fragmentation anomalies.
- Unusual ICMP communication volumes.

However, one ICMP packet is not evidence of an attack.

Legitimate systems use ICMP for diagnostics and network operation.

### Defensive Investigation Principles

1. Identify the source and destination.
2. Examine the ICMP type and code.
3. Compare packet timestamps.
4. Look for repeated patterns.
5. Correlate with system and firewall logs.
6. Document confirmed observations separately from hypotheses.

## 20. Practical Exercise 4: Inspect a Saved Capture with TShark

Read your ICMP capture:

```bash
tshark -r ~/practical-labs/module09/lab04/captures/icmp-loopback.pcapng
```

Display only ICMP packets:

```bash
tshark -r ~/practical-labs/module09/lab04/captures/icmp-loopback.pcapng -Y icmp
```

Extract source and destination IP addresses:

```bash
tshark -r ~/practical-labs/module09/lab04/captures/icmp-loopback.pcapng -T fields -e ip.src -e ip.dst
```

Extract ICMP types:

```bash
tshark -r ~/practical-labs/module09/lab04/captures/icmp-loopback.pcapng -T fields -e icmp.type
```

If the capture contains no packets, repeat the capture on the loopback interface while pinging `127.0.0.1`.

## 21. Evidence Integrity

Calculate a SHA-256 hash:

```bash
sha256sum ~/practical-labs/module09/lab04/captures/icmp-loopback.pcapng
```

Save it:

```bash
sha256sum ~/practical-labs/module09/lab04/captures/icmp-loopback.pcapng > ~/practical-labs/module09/lab04/captures/icmp-loopback.sha256
```

Verify it:

```bash
sha256sum -c ~/practical-labs/module09/lab04/captures/icmp-loopback.sha256
```

Expected result:

```text
icmp-loopback.pcapng: OK
```

## 22. Practical Investigation Report

Create:

```bash
nano ~/practical-labs/module09/lab04/reports/ip-icmp-analysis-report.txt
```

Use this template:

```text
==========================================
IP AND ICMP TRAFFIC ANALYSIS REPORT
==========================================

1. INVESTIGATION INFORMATION

Date:
Operating System:
Network Interface:

2. IP CONFIGURATION

IPv4 Address:
IPv6 Address:
Default Gateway:

3. PACKET CAPTURE

Capture File:
Capture Interface:
Total Packets:
Protocols Identified:

4. IP ANALYSIS

Source IP:
Destination IP:
IP Version:
TTL or Hop Limit:

5. ICMP ANALYSIS

ICMP Type:
ICMP Code:
Sequence Number:
Echo Requests:
Echo Replies:

6. TRACEROUTE OBSERVATIONS

Destination:
Number of Hops:
Observed Responses:
Limitations:

7. SECURITY OBSERVATIONS

Unexpected Traffic:
Potential Risks:
Supporting Evidence:
Unverified Hypotheses:

8. EVIDENCE INTEGRITY

Capture File:
SHA-256:

9. CONCLUSION

Summary:
Findings:
Recommendations:

==========================================
END OF REPORT
==========================================
```

Save and exit using:

- CTRL + O
- ENTER
- CTRL + X

## 23. Knowledge Check

1. What is Internet Protocol?
2. At which OSI layer does IP operate?
3. How many bits are in IPv4?
4. How many bits are in IPv6?
5. What is the purpose of an IP source address?
6. What does TTL mean?
7. What is the IPv4 protocol number for ICMP?
8. What is ICMP used for?
9. What is the difference between Echo Request and Echo Reply?
10. What is the purpose of ping?
11. What is traceroute?
12. What does an ICMP Destination Unreachable message indicate?
13. Why might a traceroute display asterisks?
14. Why does failed ping not always mean a host is offline?
15. How can Wireshark help investigate suspicious ICMP activity?

## 24. Lesson Completion Checklist

- [ ] Understood IPv4 and IPv6.
- [ ] Studied IP packet structure.
- [ ] Understood TTL and Hop Limit.
- [ ] Learned ICMP fundamentals.
- [ ] Inspected IP configuration.
- [ ] Generated local ICMP traffic.
- [ ] Captured ICMP packets.
- [ ] Applied IPv4 display filters.
- [ ] Applied IPv6 display filters.
- [ ] Applied ICMP display filters.
- [ ] Studied traceroute.
- [ ] Inspected captures using TShark.
- [ ] Calculated a SHA-256 hash.
- [ ] Created an investigation report.

## 25. Lesson Summary

You have learned how IP addresses allow devices to communicate across networks and how ICMP supports network diagnostics.

You practiced inspecting IPv4 and IPv6 traffic, analyzing ICMP Echo Requests and Replies, understanding TTL, and using traceroute.

You also learned how Wireshark and TShark can support network troubleshooting and defensive investigations.

**Next Lesson: Lesson 05 – TCP and UDP Traffic Analysis.**
