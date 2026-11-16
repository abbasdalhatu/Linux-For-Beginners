# Module 10 – Lesson 13: Network Monitoring and Analysis

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain network monitoring.
2. Understand network traffic analysis.
3. Identify active network connections.
4. Monitor network interfaces.
5. Understand bandwidth and packet statistics.
6. Use Linux network monitoring tools.
7. Understand packet capture.
8. Identify suspicious network activity.
9. Understand how network monitoring supports cybersecurity.

---

## 2. What Is Network Monitoring?

Network monitoring is the process of observing network activity to understand how devices communicate and to identify problems.

A network administrator or cybersecurity professional monitors network activity to determine:

- Which devices are communicating.
- Which services are active.
- How much data is being transmitted.
- Whether connections are successful.
- Whether network performance is normal.
- Whether suspicious activity is occurring.

### Real-World Example

Imagine managing a university network.

You may want to know:

- How many devices are connected?
- Which servers are receiving traffic?
- Is the internet connection slow?
- Is an unknown device communicating with a server?
- Are there unusual connection attempts?

Network monitoring helps answer these questions.

---

## 3. Understanding Network Traffic

Network traffic refers to data moving across a network.

Examples include:

- Opening a website.
- Sending an email.
- Downloading a file.
- Streaming a video.
- Connecting to a remote server.
- Communicating with a DNS server.

Network traffic generally travels in packets.

Each packet contains information needed to deliver data to its destination.

### Basic Packet Information

A packet may contain:

- Source IP address.
- Destination IP address.
- Protocol.
- Source port.
- Destination port.
- Payload.
- Other control information.

The exact fields depend on the protocol.

---

## 4. Understanding Network Monitoring Metrics

Network monitoring tools measure different aspects of network activity.

### 4.1 Bandwidth

Bandwidth is the maximum data-transfer capacity of a network connection.

It is commonly measured in:

- Kbps
- Mbps
- Gbps

### 4.2 Throughput

Throughput is the actual amount of data successfully transferred over a period.

A connection may have a capacity of 100 Mbps but achieve a lower actual throughput.

### 4.3 Latency

Latency is the time required for data to travel between two points.

It is commonly measured in milliseconds (ms).

### 4.4 Packet Loss

Packet loss occurs when transmitted packets fail to reach their intended destination.

Possible causes include:

- Network congestion.
- Hardware problems.
- Wireless interference.
- Configuration errors.

### 4.5 Jitter

Jitter is variation in packet delay.

High jitter can affect:

- Voice calls.
- Video conferences.
- Online gaming.
- Real-time communication.

### 4.6 Network Availability

Availability measures whether a network or service is accessible when needed.

---

## 5. Monitoring Network Interfaces

Linux provides commands for monitoring network interfaces.

### 5.1 Display Network Interfaces

```bash
ip link
```

### 5.2 Display Interface Addresses

```bash
ip addr
```

### 5.3 Display Interface Statistics

```bash
ip -s link
```

The statistics may include:

- Received packets.
- Transmitted packets.
- Received bytes.
- Transmitted bytes.
- Errors.
- Dropped packets.

### 5.4 Display Interfaces in Brief Format

```bash
ip -br link
```

### 5.5 Display Address Information Briefly

```bash
ip -br addr
```

These commands help administrators inspect the state and activity of network interfaces.

---

## 6. Monitoring Network Connections with `ss`

The `ss` command displays socket information.

It is useful for identifying active and listening network connections.

### 6.1 Display All TCP Connections

```bash
ss -ta
```

### 6.2 Display All UDP Sockets

```bash
ss -ua
```

### 6.3 Display Listening TCP and UDP Sockets

```bash
ss -tuln
```

### 6.4 Display Processes Associated with Sockets

```bash
sudo ss -tulnp
```

### 6.5 Display Established TCP Connections

```bash
ss -t state established
```

### Understanding Common TCP States

| State | Meaning |
|---|---|
| LISTEN | Waiting for incoming connections |
| ESTAB | Connection established |
| SYN-SENT | Connection request sent |
| SYN-RECV | Connection request received |
| TIME-WAIT | Waiting before connection cleanup |
| CLOSE-WAIT | Waiting for the local application to close |

A listening port is not automatically a security vulnerability. Its purpose and exposure must be investigated.

---

## 7. Monitoring Traffic with `nload`

`nload` is a Linux command-line tool for monitoring network traffic.

It displays incoming and outgoing traffic statistics.

### Check Whether It Is Installed

```bash
command -v nload
```

If it is not installed, you may install it through Kali's official APT repositories:

```bash
sudo apt update
sudo apt install nload
```

### Start Monitoring

```bash
nload
```

### Select a Specific Interface

```bash
nload eth0
```

Replace `eth0` with your actual interface name.

### What You Can Observe

- Incoming traffic.
- Outgoing traffic.
- Current transfer rate.
- Average transfer rate.
- Maximum transfer rate.
- Total transferred data.

Use `Ctrl + C` to stop the monitoring session.

---

## 8. Monitoring Traffic with `iftop`

`iftop` displays network traffic by connection.

It can help identify which network conversations are consuming bandwidth.

### Check Installation

```bash
command -v iftop
```

Install if necessary:

```bash
sudo apt update
sudo apt install iftop
```

### Start Monitoring

```bash
sudo iftop
```

### Monitor a Specific Interface

```bash
sudo iftop -i eth0
```

### Important Information

`iftop` can show:

- Source and destination hosts.
- Network conversations.
- Transfer rates.
- Traffic direction.

It is useful for investigating unexpectedly high network usage.

Use `Ctrl + C` to exit.

Only monitor networks and traffic you are authorized to inspect.

---

## 9. Monitoring Traffic with `vnstat`

`vnstat` is a network traffic monitoring utility.

Unlike tools focused mainly on live traffic, it can maintain traffic statistics over time.

### Check Installation

```bash
command -v vnstat
```

Install if necessary:

```bash
sudo apt update
sudo apt install vnstat
```

### Display Traffic Statistics

```bash
vnstat
```

### Display Hourly Statistics

```bash
vnstat -h
```

### Display Daily Statistics

```bash
vnstat -d
```

### Display Monthly Statistics

```bash
vnstat -m
```

The amount of historical data available depends on how long monitoring has been running and whether its service has been configured.

---

## 10. Understanding Packet Capture

Packet capture is the process of collecting network packets for inspection.

It is useful for:

- Troubleshooting network problems.
- Investigating communication failures.
- Understanding protocols.
- Detecting suspicious traffic.
- Performing authorized security analysis.

Packet capture is an important skill in network security.

However, captured traffic may contain sensitive information. Only capture traffic you own or have explicit permission to analyze.

---

## 11. Understanding Wireshark

Wireshark is a graphical network protocol analyzer.

It allows users to inspect packets and understand network communication.

### Common Uses

- Inspect DNS requests.
- Investigate TCP handshakes.
- Analyze HTTP communication.
- Troubleshoot network failures.
- Identify unexpected connections.

<Insert screenshot of Wireshark showing a packet list and packet details here.>

### Main Wireshark Areas

1. Packet List: Displays captured packets.
2. Packet Details: Displays the selected packet's protocol information.
3. Packet Bytes: Displays the packet's raw bytes.

### Common Display Filters

DNS traffic:

```text
dns
```

TCP traffic:

```text
tcp
```

UDP traffic:

```text
udp
```

HTTP traffic:

```text
http
```

HTTPS-related TLS traffic:

```text
tls
```

Traffic involving an IP address:

```text
ip.addr == 192.168.1.10
```

Traffic using a specific TCP port:

```text
tcp.port == 443
```

These are display filters. They help you inspect packets already captured; they do not generate network traffic.

---

## 12. Understanding tcpdump

`tcpdump` is a command-line packet capture utility.

It allows administrators to capture and inspect packets directly from a terminal.

### 12.1 Check Installation

```bash
command -v tcpdump
```

If necessary:

```bash
sudo apt update
sudo apt install tcpdump
```

### 12.2 List Available Interfaces

```bash
sudo tcpdump -D
```

### 12.3 Capture a Small Number of Packets

```bash
sudo tcpdump -i eth0 -c 10
```

This captures up to ten packets on the selected interface.

### 12.4 Capture DNS Traffic

```bash
sudo tcpdump -i eth0 -nn 'port 53' -c 20
```

### 12.5 Capture TCP Traffic

```bash
sudo tcpdump -i eth0 -nn tcp -c 20
```

### 12.6 Save Captured Packets to a File

```bash
sudo tcpdump -i eth0 -c 20 -w network-capture.pcap
```

This saves the capture in PCAP format.

### 12.7 Read a Saved Capture

```bash
tcpdump -r network-capture.pcap
```

### Important Options

| Option | Meaning |
|---|---|
| `-i` | Select interface |
| `-c` | Stop after a specified number of packets |
| `-nn` | Disable hostname and service-name resolution |
| `-w` | Write capture to a file |
| `-r` | Read a saved capture |
| `-D` | List capture interfaces |

---

## 13. Understanding Network Monitoring Security Indicators

Monitoring tools can help identify suspicious behaviour.

### 13.1 Unexpected Listening Ports

A service may be listening on a port that you do not recognize.

Investigation:

```bash
sudo ss -tulnp
```

Determine which application owns the socket and whether it is required.

### 13.2 Unusual Outbound Connections

A system may communicate with an unfamiliar external address.

Investigation:

```bash
ss -tun
```

Do not assume that every unfamiliar address is malicious. Investigate the application, destination, and context.

### 13.3 Unexpected Traffic Spikes

A sudden increase in network traffic may result from:

- Software updates.
- Large downloads.
- Cloud synchronization.
- Backups.
- Malware activity.

Monitoring helps establish what is happening.

### 13.4 High Packet Loss

High packet loss may indicate:

- Network congestion.
- Wireless interference.
- Faulty hardware.
- Routing problems.
- Firewall behaviour.

### 13.5 Unusual DNS Activity

Unexpected DNS requests or changes to DNS servers may indicate misconfiguration or malicious activity.

---

## 14. Network Monitoring and Cybersecurity

Network monitoring supports several cybersecurity activities.

### Network Defense

Security professionals monitor traffic to identify abnormal communication.

### Incident Response

Captured packets and connection records can help investigate a security incident.

### Threat Detection

Unexpected connections, traffic patterns, and protocol behaviour can provide useful warning signs.

### Performance Monitoring

Administrators investigate slow connections and network failures.

### Digital Forensics

Packet captures and network logs can help reconstruct communication events.

Network monitoring is especially important in Security Operations Centers (SOCs).

---

## 15. Practical Lab: Monitoring Your Kali VM

### Lab Objective

Use Linux monitoring tools to observe network interfaces, connections, and authorized traffic generated by your own VM.

### Step 1: Identify Your Interface

```bash
ip -br link
```

Record your active interface.

### Step 2: Inspect Interface Statistics

```bash
ip -s link
```

Record received and transmitted packet counts.

### Step 3: Inspect Listening Services

```bash
sudo ss -tulnp
```

Identify the listening services.

### Step 4: Inspect Established TCP Connections

```bash
ss -t state established
```

Record any established connections.

### Step 5: Check for Monitoring Tools

```bash
command -v tcpdump
command -v nload
command -v iftop
command -v vnstat
```

Record which tools are installed.

### Step 6: Identify Capture Interfaces

```bash
sudo tcpdump -D
```

Find the name of your active interface.

### Step 7: Capture a Small Amount of Your VM's Own Traffic

Replace `eth0` with your actual interface:

```bash
sudo tcpdump -i eth0 -nn -c 10
```

Generate a small amount of ordinary traffic in another terminal:

```bash
ping -c 4 1.1.1.1
```

Observe the captured packets.

If no packets appear, verify the interface name and whether the ping succeeded.

### Step 8: Capture DNS Traffic

```bash
sudo tcpdump -i eth0 -nn 'port 53' -c 20
```

In another terminal, run:

```bash
dig example.com
```

Observe any DNS packets visible in the capture.

Note: DNS may be cached or handled by a local resolver, so the expected packets may not appear on the physical virtual interface every time.

### Step 9: Save a Small Packet Capture

```bash
sudo tcpdump -i eth0 -nn -c 20 -w ~/network-monitoring-lab.pcap
```

If fewer than 20 packets are generated, press `Ctrl + C` to stop.

### Step 10: Read the Capture

```bash
tcpdump -r ~/network-monitoring-lab.pcap
```

Record what you observe.

### Step 11: Clean Up

After recording your findings:

```bash
rm ~/network-monitoring-lab.pcap
```

This removes only the capture file created in this lab.

---

## 16. Practical Lab Report

Complete the following table:

| Item | Your Observation |
|---|---|
| Active network interface | |
| Received packets | |
| Transmitted packets | |
| Listening TCP services | |
| Listening UDP services | |
| Established connections | |
| Installed monitoring tools | |
| Captured packet count | |
| DNS traffic observed | |
| Main findings | |

### Analysis Questions

1. Did you observe any listening ports?
2. Which applications were associated with them?
3. Did your interface show received and transmitted packets?
4. What happened when you generated ICMP traffic?
5. What information could you identify in the packet capture?
6. Why is packet capture useful for troubleshooting?

---

## 17. Important Commands to Remember

```bash
# Interface information
ip link
ip addr
ip -s link

# Connection monitoring
ss -ta
ss -ua
ss -tuln
ss -tun
ss -t state established

# Live traffic monitoring
nload
sudo iftop
vnstat

# Packet capture
sudo tcpdump -D
sudo tcpdump -i eth0 -c 10
sudo tcpdump -i eth0 -nn 'port 53' -c 20

# Save and read captures
sudo tcpdump -i eth0 -c 20 -w capture.pcap
tcpdump -r capture.pcap
```

Replace `eth0` with your actual interface.

---

## 18. Assessment Questions

1. What is network monitoring?
2. What is network traffic?
3. Explain bandwidth and throughput.
4. What is latency?
5. What is packet loss?
6. What is jitter?
7. What does `ip -s link` display?
8. What is the purpose of the `ss` command?
9. What is Wireshark?
10. What is packet capture?
11. What is `tcpdump` used for?
12. What is the purpose of the `-w` option in tcpdump?
13. What does the `-r` option do?
14. Why should packet captures be protected?
15. How can network monitoring help identify suspicious activity?

---

## 19. Lesson Summary

In this lesson, you learned:

- Network monitoring observes network activity.
- Network traffic is data moving across a network.
- Bandwidth describes capacity.
- Throughput describes actual data transfer.
- Latency measures delay.
- Packet loss means packets fail to reach their destination.
- Jitter measures variation in packet delay.
- `ip -s link` displays interface statistics.
- `ss` displays socket and connection information.
- `nload`, `iftop`, and `vnstat` support traffic monitoring.
- Wireshark provides graphical packet analysis.
- `tcpdump` provides command-line packet capture.
- PCAP files store captured packet data.
- Network monitoring supports troubleshooting, incident response, and threat detection.

### Key Definitions

**Network Monitoring:** Observing network activity to understand performance and detect problems.

**Network Traffic:** Data moving between devices over a network.

**Bandwidth:** Maximum data-transfer capacity.

**Throughput:** Actual data successfully transferred over time.

**Latency:** Time delay in communication.

**Packet Loss:** Failure of packets to reach their destination.

**Jitter:** Variation in packet delay.

**Packet Capture:** Collecting packets for inspection.

**Wireshark:** A graphical network protocol analyzer.

**tcpdump:** A command-line packet capture utility.

**PCAP:** A file format used to store packet capture data.

---

## 20. Knowledge Check

1. Which command displays network interface statistics?
2. Which command displays listening TCP and UDP sockets?
3. Which tool is used for graphical packet analysis?
4. Which tool captures packets from the command line?
5. What does packet loss indicate?
6. What is the difference between bandwidth and throughput?
7. Why might an administrator use `iftop`?
8. What does the `-w` option do in tcpdump?
9. What can an unexpected outbound connection indicate?
10. Why should cybersecurity professionals monitor network traffic?

**End of Lesson 13 – Network Monitoring and Analysis**