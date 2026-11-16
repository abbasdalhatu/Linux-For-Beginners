
# MODULE 09 – WIRESHARK AND NETWORK TRAFFIC ANALYSIS

# LESSON 01: WIRESHARK FUNDAMENTALS

## 1. Introduction

Wireshark is a network protocol analyzer used to capture, inspect, and analyze network traffic.

It allows security analysts and network administrators to examine how devices communicate across a network.

In cybersecurity, understanding network traffic is important because suspicious communication can reveal:

- Unauthorized connections.
- Unexpected network activity.
- DNS resolution problems.
- Suspicious data transfers.
- Communication with unknown servers.
- Network configuration problems.
- Possible indicators of compromise.

Wireshark helps us understand what is happening inside a network.

## 2. What Is Network Traffic?

Network traffic refers to the data transmitted between devices over a network.

For example, when you open a website:

1. Your computer requests the website's IP address.
2. A DNS server may respond with the address.
3. Your computer establishes a connection to the destination.
4. Your browser sends a request.
5. The web server sends a response.
6. Your browser displays the website.

These communications generate network packets.

Wireshark allows us to inspect captured packets.

## 3. What Is a Network Packet?

A packet is a formatted unit of data transmitted across a network.

A packet generally contains:

- Source address.
- Destination address.
- Protocol information.
- Control information.
- Payload or data.

Example:

```text
Source IP:      192.168.1.10
Destination IP: 192.168.1.1
Protocol:       TCP
Source Port:    51520
Destination Port: 443
```

This represents a TCP packet travelling from a local device to a destination using port 443.

Remember: a packet is not necessarily the same thing as a complete application message. Larger communications may be divided into multiple packets.

## 4. What Is Wireshark?

Wireshark is an open-source network protocol analyzer.

Its main functions include:

1. Capturing packets from network interfaces.
2. Displaying packet information.
3. Filtering network traffic.
4. Examining protocol headers.
5. Following supported network streams.
6. Exporting packet captures.
7. Helping investigate network problems and security incidents.

Official website:

https://www.wireshark.org/

## 5. Important Wireshark Concepts

### A. Packet Capture

Packet capture is the process of collecting network packets for later examination or live analysis.

Wireshark can capture traffic from supported network interfaces.

### B. Network Interface

A network interface is a connection point through which a device communicates with a network.

Examples:

- Ethernet.
- Wi-Fi.
- Loopback.
- Virtual network adapters.

### C. Protocol

A protocol is a set of rules governing communication between devices.

Examples:

| Protocol | Purpose |
|---|---|
| Ethernet | Local network communication |
| ARP | IPv4 address-to-MAC address resolution |
| IP | Addressing and routing |
| ICMP | Network control and diagnostic messages |
| TCP | Reliable transport |
| UDP | Connectionless transport |
| DNS | Domain name resolution |
| HTTP | Web communication |
| HTTPS | Encrypted web communication |

### D. Packet Details

Wireshark displays detailed information about each captured packet.

You can inspect:

- Ethernet headers.
- IP headers.
- TCP or UDP headers.
- Application-layer protocol information.
- Available payload data.

### E. Packet Capture Files

Wireshark can save packet captures for later analysis.

Common formats include:

- `.pcap`
- `.pcapng`

PCAPNG is the modern default format used by Wireshark.

## 6. Installing or Checking Wireshark on Kali Linux

First, check whether Wireshark is installed:

```bash
wireshark --version
```

If the command is unavailable, check the package information:

```bash
apt policy wireshark
```

If installation is required, use the official Kali package repository:

```bash
sudo apt update
sudo apt install wireshark
```

Check the version again:

```bash
wireshark --version
```

Do not change system security settings to make packet capture work.

## 7. Launching Wireshark

Launch the graphical interface:

```bash
wireshark
```

Alternatively, open Wireshark from the Kali applications menu.

The main interface contains several important areas.

### A. Interface Selection

This displays available network interfaces.

Examples might include:

- `eth0`
- `ens33`
- `wlan0`
- `lo`

Your interface names may be different.

### B. Packet List Pane

Displays captured packets in rows.

Typical columns include:

| Column | Meaning |
|---|---|
| No. | Packet number |
| Time | Capture timestamp |
| Source | Source address |
| Destination | Destination address |
| Protocol | Protocol identified |
| Length | Packet size |
| Info | Short packet description |

### C. Packet Details Pane

Displays the protocol layers and fields of the selected packet.

### D. Packet Bytes Pane

Displays the packet's raw bytes, usually in hexadecimal and ASCII representations.

## 8. Understanding the Three Main Wireshark Panes

A packet capture can be understood using three main views.

```text
+-------------------------------------------+
|              PACKET LIST                  |
| No.  Time   Source   Destination Protocol |
+-------------------------------------------+
|             PACKET DETAILS                |
| Ethernet II                               |
| Internet Protocol                         |
| Transmission Control Protocol             |
| Application Protocol                      |
+-------------------------------------------+
|              PACKET BYTES                 |
| 45 00 00 3c ...                           |
+-------------------------------------------+
```

The packet list provides a summary.

The packet details pane explains the protocol structure.

The packet bytes pane shows the underlying packet data.

## 9. Understanding Wireshark Capture Filters and Display Filters

Wireshark has two important filtering systems.

### A. Capture Filters

Capture filters determine which packets Wireshark collects.

They are applied before or during packet capture.

Examples:

Capture only TCP traffic:

```text
tcp
```

Capture only traffic involving a particular host:

```text
host 192.168.1.10
```

Capture only DNS traffic:

```text
port 53
```

Capture only traffic from a particular network:

```text
net 192.168.1.0/24
```

Capture filters use the Berkeley Packet Filter (BPF) syntax.

### B. Display Filters

Display filters control which packets are shown after capture.

They do not delete other packets from the capture.

Examples:

Display TCP traffic:

```text
tcp
```

Display DNS traffic:

```text
dns
```

Display HTTP traffic:

```text
http
```

Display traffic involving an IP address:

```text
ip.addr == 192.168.1.10
```

Display packets using destination port 443:

```text
tcp.dstport == 443
```

Display ICMP packets:

```text
icmp
```

Important distinction:

- Capture filter = controls what gets collected.
- Display filter = controls what gets displayed.

## 10. Understanding Network Interfaces in Kali

Open a terminal and inspect your interfaces:

```bash
ip addr
```

Display available network links:

```bash
ip link
```

Display routing information:

```bash
ip route
```

Identify your default network interface:

```bash
ip route get 1.1.1.1
```

Look for the interface name after the `dev` field.

For example:

```text
1.1.1.1 via 192.168.100.1 dev eth0 src 192.168.100.20
```

In this example:

- Interface: `eth0`
- Gateway: `192.168.100.1`
- Local IP: `192.168.100.20`

Your actual values may be different.

## 11. Your First Wireshark Practical Exercise

### Objective

Open Wireshark and become familiar with its interface.

### Step 1: Check Network Interfaces

Run:

```bash
ip addr
```

Identify the interface used by your Kali VM.

### Step 2: Open Wireshark

Run:

```bash
wireshark
```

### Step 3: Select an Interface

Select the interface associated with your Kali VM's active network connection.

Do not select an unrelated interface.

### Step 4: Start a Capture

Start capturing on your selected interface.

Observe the packet list.

### Step 5: Generate Normal Network Activity

In another terminal, run:

```bash
ping -c 4 1.1.1.1
```

If internet connectivity is unavailable, you can skip this test.

You can also generate local loopback traffic:

```bash
ping -c 4 127.0.0.1
```

Note: loopback traffic is normally visible only when capturing on the loopback interface.

### Step 6: Stop the Capture

Click the red stop button in Wireshark.

### Step 7: Inspect a Packet

Select a packet.

Examine:

1. Packet List.
2. Packet Details.
3. Packet Bytes.

Identify the protocol and available source and destination information.

### Step 8: Apply a Display Filter

Enter:

```text
icmp
```

Press Enter.

Observe the ICMP packets.

Clear the filter and try:

```text
tcp
```

Then try:

```text
dns
```

The results depend on the traffic you captured.

### Step 9: Save Your Capture

Create a dedicated directory:

```bash
mkdir -p ~/practical-labs/module09/lab01
```

Save your capture as:

```text
~/practical-labs/module09/lab01/first-capture.pcapng
```

Use File → Save As in Wireshark.

## 12. Basic Command-Line Packet Capture

Wireshark includes command-line tools.

One important tool is `tshark`.

Check whether it is installed:

```bash
tshark --version
```

List available interfaces:

```bash
tshark -D
```

Capture a small number of packets from an interface you own:

```bash
sudo tshark -i eth0 -c 20
```

Replace `eth0` with your actual interface name.

This command captures 20 packets and then stops.

If the interface name is different, use the correct one.

Save a small capture:

```bash
sudo tshark -i eth0 -c 20 -w first-capture.pcapng
```

Use a dedicated lab directory and do not overwrite an existing evidence file.

## 13. Security Applications of Wireshark

Wireshark is useful in defensive cybersecurity.

### Network Troubleshooting

It can help identify:

- Connection failures.
- DNS problems.
- TCP retransmissions.
- Unexpected network delays.

### Security Monitoring

It can help investigate:

- Unusual communication patterns.
- Unexpected services.
- Suspicious DNS queries.
- Unexpected cleartext protocols.
- Unknown connections.

### Incident Response

Security analysts may use packet captures to:

- Reconstruct communication timelines.
- Identify communicating hosts.
- Examine protocol behaviour.
- Preserve network evidence.
- Support incident reports.

### Important Limitation

Wireshark does not automatically identify every attack or malicious packet.

Analysts must interpret packet information in context.

Encrypted HTTPS traffic generally protects application contents from ordinary packet inspection, although metadata such as addresses, ports, timing, and packet sizes may remain observable.

## 14. Important Safety and Privacy Rules

1. Capture only traffic you own or have explicit permission to inspect.
2. Do not capture other people's private network communications.
3. Keep packet captures inside your isolated lab.
4. Avoid collecting passwords or sensitive information.
5. Do not upload private packet captures to public analysis websites.
6. Preserve original evidence when investigating incidents.
7. Use capture filters carefully to reduce unnecessary data collection.

## 15. Knowledge Check

Answer these questions in your own understanding:

1. What is Wireshark?
2. What is network traffic?
3. What is a packet?
4. What is packet capture?
5. What is a network interface?
6. What are the three main Wireshark panes?
7. What is the difference between a capture filter and a display filter?
8. What does the `tcp` display filter show?
9. What is the purpose of `tshark`?
10. Why is Wireshark important in cybersecurity?

## 16. Lesson Completion Checklist

- [ ] Understood network traffic and packets.
- [ ] Understood Wireshark fundamentals.
- [ ] Checked Wireshark installation.
- [ ] Opened the Wireshark graphical interface.
- [ ] Identified the Kali network interface.
- [ ] Started and stopped a packet capture.
- [ ] Inspected packet details.
- [ ] Applied basic display filters.
- [ ] Saved a PCAPNG capture.
- [ ] Understood basic TShark commands.
- [ ] Reviewed cybersecurity applications and safety rules.

## 17. Lesson Summary

In this lesson, you learned that Wireshark is a network protocol analyzer used to capture and inspect network communications.

You explored packet structure, network interfaces, protocol information, capture filters, display filters, and TShark.

You also performed your first controlled packet capture inside your own Kali virtual machine.

**Next Lesson: Lesson 02 – Packet Capture and Analysis.**
