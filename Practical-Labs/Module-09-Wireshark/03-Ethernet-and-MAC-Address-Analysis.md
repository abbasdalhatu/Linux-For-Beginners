
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 03: ETHERNET AND MAC ADDRESS ANALYSIS

## 1. Introduction

In this lesson, you will learn how devices communicate inside a local area network (LAN).

We will focus on:

- Ethernet.
- MAC addresses.
- Ethernet frames.
- ARP.
- Broadcast and unicast communication.
- Ethernet packet inspection in Wireshark.
- Identifying unusual local network activity.

These concepts are important for network administration, troubleshooting, and defensive cybersecurity.

## 2. What Is Ethernet?

Ethernet is a widely used technology for communication within wired local area networks.

It defines how devices transmit data over an Ethernet network.

Ethernet operates primarily at Layer 2 of the OSI model, known as the Data Link Layer.

Examples of Ethernet technologies include:

- 100BASE-TX.
- 1000BASE-T.
- 10GBASE-T.

Ethernet frames carry network-layer packets between devices.

## 3. What Is a MAC Address?

A MAC address is a link-layer address associated with a network interface.

MAC stands for Media Access Control.

A MAC address is commonly represented using 48 bits, written as six hexadecimal pairs.

Example:

```text
00:1A:2B:3C:4D:5E
```

Other representations include:

```text
00-1A-2B-3C-4D-5E
```

```text
001A.2B3C.4D5E
```

A MAC address is used for communication on a local network segment.

Important: MAC addresses can be changed or spoofed. They should not be treated as reliable proof of a device's identity.

## 4. MAC Address Structure

A traditional 48-bit MAC address contains:

- First 24 bits: Organizationally Unique Identifier (OUI).
- Last 24 bits: Interface-specific portion assigned under the organization's allocation.

Example:

```text
00:1A:2B:3C:4D:5E
```

Conceptually:

```text
00:1A:2B | 3C:4D:5E
   OUI   | Interface portion
```

The OUI can help identify the organization associated with an address allocation.

However, locally administered addresses and virtual interfaces may not identify a hardware manufacturer.

## 5. Types of MAC Address Communication

### A. Unicast

Unicast communication sends a frame to one destination interface.

Example:

```text
Source MAC:      AA:AA:AA:AA:AA:AA
Destination MAC: BB:BB:BB:BB:BB:BB
```

### B. Broadcast

Broadcast communication sends a frame to all devices on the local broadcast domain.

The standard Ethernet broadcast address is:

```text
FF:FF:FF:FF:FF:FF
```

ARP requests commonly use Ethernet broadcasts.

### C. Multicast

Multicast communication sends traffic to a group of interested receivers.

It is commonly used by network discovery and streaming protocols.

## 6. What Is an Ethernet Frame?

An Ethernet frame is the Layer 2 unit of transmission.

A standard Ethernet II frame contains:

| Field | Purpose |
|---|---|
| Destination MAC | Receiving interface |
| Source MAC | Sending interface |
| EtherType | Identifies the encapsulated protocol |
| Payload | Carries upper-layer data |
| Frame Check Sequence | Error detection information |

A simplified view:

```text
+--------------------+
| Destination MAC    |
+--------------------+
| Source MAC         |
+--------------------+
| EtherType          |
+--------------------+
| Payload            |
+--------------------+
| Frame Check Seq.   |
+--------------------+
```

Wireshark may show the Ethernet frame fields under:

```text
Ethernet II
```

The frame check sequence is not necessarily included in every packet capture.

## 7. Understanding EtherType

EtherType identifies the protocol carried inside an Ethernet frame.

Common examples:

| EtherType | Protocol |
|---|---|
| 0x0800 | IPv4 |
| 0x0806 | ARP |
| 0x86DD | IPv6 |
| 0x8100 | VLAN-tagged Ethernet |

In Wireshark, you can expand the Ethernet II section to inspect the EtherType.

## 8. What Is ARP?

ARP stands for Address Resolution Protocol.

It is used in IPv4 local networks to discover the MAC address associated with an IPv4 address.

For example, suppose your Kali machine wants to communicate with:

```text
192.168.100.1
```

But it does not know the destination MAC address.

It may send an ARP request asking:

```text
Who has 192.168.100.1?
```

The device using that address may respond with its MAC address.

### ARP Request

```text
Who has 192.168.100.1?
Tell 192.168.100.20
```

### ARP Reply

```text
192.168.100.1 is at AA:BB:CC:DD:EE:FF
```

ARP helps devices communicate on an IPv4 local network.

## 9. ARP Request and Reply Process

```text
Kali Linux                         Gateway
192.168.100.20                     192.168.100.1
     |                                  |
     |--------- ARP Request ---------->|
     |                                  |
     |<---------- ARP Reply ------------|
     |                                  |
     |       MAC Address Known         |
     |                                  |
     |--------- Ethernet Data -------->|
```

ARP requests are normally broadcast on the local network.

ARP replies are commonly unicast to the requesting device.

## 10. Inspecting MAC Addresses in Kali Linux

Display network interfaces:

```bash
ip link
```

Display IP and MAC address information:

```bash
ip addr
```

Show a particular interface:

```bash
ip link show eth0
```

Replace `eth0` with your actual interface.

Look for a line similar to:

```text
link/ether 00:11:22:33:44:55
```

That is the interface's current MAC address.

Display the ARP or neighbour table:

```bash
ip neigh
```

The neighbour table contains discovered link-layer information for network neighbours.

Example:

```text
192.168.100.1 dev eth0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
```

This indicates that the system has a neighbour entry associating an IPv4 address with a MAC address.

The exact entries depend on your network activity.

## 11. Practical Exercise 1: Inspect Your MAC Address

### Objective

Identify the MAC address of your Kali network interface.

Run:

```bash
ip link
```

Identify the active network interface.

Then run:

```bash
ip addr
```

Record:

```text
Interface:
MAC Address:
IPv4 Address:
Interface State:
```

Do not change the MAC address.

## 12. Practical Exercise 2: Inspect the Neighbour Table

Run:

```bash
ip neigh
```

If the table is empty, generate normal local network activity.

For example, inspect your default gateway:

```bash
ip route
```

Then ping the gateway shown in your routing table:

```bash
ping -c 3 GATEWAY_IP
```

Replace `GATEWAY_IP` with your actual gateway address.

Inspect the neighbour table again:

```bash
ip neigh
```

Record any entries.

An empty neighbour table is not automatically a problem.

## 13. Practical Exercise 3: Capture Ethernet Frames in Wireshark

### Objective

Inspect Ethernet frames generated by your own Kali VM.

Step 1: Open Wireshark.

```bash
wireshark
```

Step 2: Select your active network interface.

Step 3: Start capturing.

Step 4: Generate local network activity:

```bash
ping -c 5 GATEWAY_IP
```

Use your actual gateway address.

Step 5: Stop the capture.

Step 6: Apply this display filter:

```text
eth
```

Step 7: Select an Ethernet packet.

Step 8: Expand:

```text
Ethernet II
```

Step 9: Inspect:

- Destination MAC.
- Source MAC.
- Type.

Step 10: Save your capture:

```text
ethernet-analysis.pcapng
```

Store it in:

```text
~/practical-labs/module09/lab03/
```

## 14. Practical Exercise 4: Analyze ARP Traffic

Start another capture on your active network interface.

Generate local network activity by pinging your gateway.

Apply this Wireshark display filter:

```text
arp
```

You may see:

```text
Who has 192.168.100.1? Tell 192.168.100.20
```

If an ARP response is captured, inspect it.

Look for:

- Sender IP address.
- Sender MAC address.
- Target IP address.
- Target MAC address.
- Operation type.

ARP packets may not appear if the relevant neighbour entry is already cached.

If you do not see ARP packets, do not clear the neighbour table or force network changes. Record that the capture contained no ARP exchanges.

## 15. ARP Security Risks

ARP is useful, but it has security limitations.

### A. ARP Spoofing

ARP spoofing occurs when an attacker sends misleading ARP messages to associate an IP address with an incorrect MAC address.

Potential consequences include:

- Traffic interception.
- Network disruption.
- Man-in-the-middle positioning.

### B. ARP Cache Poisoning

ARP cache poisoning involves corrupting or manipulating ARP neighbour information.

This may cause devices to send local traffic to an unintended destination.

### C. Defensive Detection

Security analysts can investigate:

- Unexpected changes in IP-to-MAC mappings.
- Duplicate IP address warnings.
- Unusual ARP reply patterns.
- Conflicting gateway MAC addresses.

A single unfamiliar MAC address is not enough to prove an attack.

Virtual machines, gateways, and legitimate network changes can produce unfamiliar entries.

## 16. Wireshark Filters for Ethernet and ARP

Use these filters:

Show ARP packets:

```text
arp
```

Show Ethernet packets:

```text
eth
```

Show packets from a specific MAC address:

```text
eth.src == aa:bb:cc:dd:ee:ff
```

Show packets going to a specific MAC address:

```text
eth.dst == aa:bb:cc:dd:ee:ff
```

Show broadcast Ethernet frames:

```text
eth.dst == ff:ff:ff:ff:ff:ff
```

Show ARP requests:

```text
arp.opcode == 1
```

Show ARP replies:

```text
arp.opcode == 2
```

Replace example MAC addresses with values from your capture.

## 17. Understanding Broadcast Domains

A broadcast domain is a network area in which Layer 2 broadcast frames can be received by participating devices.

Routers normally separate broadcast domains.

Switches forward broadcast frames within the relevant VLAN or broadcast domain.

This is important because ARP broadcasts are normally limited to the local broadcast domain.

## 18. Defensive Security Investigation Scenario

Imagine you are monitoring a company's local network.

A workstation reports that its gateway MAC address has unexpectedly changed.

Your investigation should include:

1. Record the workstation's IP address.
2. Inspect its neighbour table.
3. Record the current gateway MAC address.
4. Examine authorized packet captures.
5. Compare the gateway MAC with trusted network records.
6. Check for conflicting ARP messages.
7. Record the timestamps and evidence.
8. Escalate confirmed suspicious findings.

Do not immediately conclude that an attack occurred.

Possible explanations include:

- Router replacement.
- Network failover.
- Virtual networking changes.
- Legitimate MAC address changes.
- ARP spoofing.

The investigator must distinguish evidence from assumptions.

## 19. Evidence Preservation

Create a lab directory:

```bash
mkdir -p ~/practical-labs/module09/lab03/captures
mkdir -p ~/practical-labs/module09/lab03/reports
```

Save your capture in the captures directory.

Calculate its hash:

```bash
sha256sum ~/practical-labs/module09/lab03/captures/ethernet-analysis.pcapng
```

Save the checksum:

```bash
sha256sum ~/practical-labs/module09/lab03/captures/ethernet-analysis.pcapng > ~/practical-labs/module09/lab03/captures/ethernet-analysis.sha256
```

Verify it:

```bash
sha256sum -c ~/practical-labs/module09/lab03/captures/ethernet-analysis.sha256
```

## 20. Practical Investigation Report

Create:

```bash
nano ~/practical-labs/module09/lab03/reports/ethernet-analysis-report.txt
```

Use this template:

```text
==========================================
ETHERNET AND MAC ADDRESS ANALYSIS REPORT
==========================================

1. INVESTIGATION INFORMATION

Date:
Operating System:
Network Interface:

2. INTERFACE INFORMATION

Interface Name:
MAC Address:
IPv4 Address:
Interface State:

3. NEIGHBOUR TABLE

IP Addresses:
MAC Addresses:
Neighbour States:

4. ETHERNET ANALYSIS

Source MAC:
Destination MAC:
EtherType:
Packet Length:

5. ARP ANALYSIS

ARP Requests:
ARP Replies:
Sender IP:
Sender MAC:
Target IP:
Target MAC:

6. SECURITY OBSERVATIONS

Unexpected MAC Addresses:
Conflicting Mappings:
Suspicious ARP Activity:
Evidence Limitations:

7. EVIDENCE INTEGRITY

Capture File:
SHA-256:

8. CONCLUSION

Summary:
Confirmed Findings:
Unverified Hypotheses:
Recommendations:

==========================================
END OF REPORT
==========================================
```

## 21. Knowledge Check

1. What is Ethernet?
2. At which OSI layer does Ethernet primarily operate?
3. What is a MAC address?
4. How many bits are in a traditional MAC address?
5. What is an OUI?
6. What is an Ethernet frame?
7. What does EtherType identify?
8. What is ARP?
9. What is the difference between an ARP request and an ARP reply?
10. What is the Ethernet broadcast address?
11. What is ARP spoofing?
12. What is the purpose of the `ip neigh` command?
13. Why should MAC addresses not be treated as reliable proof of device identity?
14. How can Wireshark help detect suspicious ARP activity?

## 22. Lesson Completion Checklist

- [ ] Understood Ethernet fundamentals.
- [ ] Learned MAC address structure.
- [ ] Understood unicast, broadcast, and multicast.
- [ ] Studied Ethernet frame structure.
- [ ] Learned EtherType values.
- [ ] Understood ARP requests and replies.
- [ ] Inspected MAC addresses in Kali.
- [ ] Examined the neighbour table.
- [ ] Captured Ethernet traffic.
- [ ] Applied ARP and Ethernet filters.
- [ ] Studied ARP security risks.
- [ ] Preserved a packet capture.
- [ ] Calculated a SHA-256 hash.
- [ ] Created an investigation report.

## 23. Lesson Summary

In this lesson, you learned how devices communicate within local networks using Ethernet frames and MAC addresses.

You studied ARP, neighbour tables, broadcast communication, Ethernet packet inspection, and defensive detection of suspicious ARP behaviour.

You also practiced inspecting your Kali VM's network interface and analyzing Ethernet and ARP traffic in Wireshark.

**Next Lesson: Lesson 04 – IP and ICMP Traffic Analysis.**
