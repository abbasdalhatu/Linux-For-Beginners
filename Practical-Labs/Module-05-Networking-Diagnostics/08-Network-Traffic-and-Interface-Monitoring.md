
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 08: NETWORK TRAFFIC AND INTERFACE MONITORING

### 1. Lab Overview

Network traffic monitoring is the process of observing data moving through a network.

Network interface monitoring involves examining the activity, performance, and health of network interfaces.

System administrators and cybersecurity professionals use these techniques to understand:

- Network utilization.
- Incoming and outgoing traffic.
- Interface errors.
- Dropped packets.
- Bandwidth consumption.
- Unexpected network activity.
- Communication patterns.
- Possible network incidents.

In this lab, you will inspect network interfaces, examine traffic statistics, monitor live traffic, and generate a network monitoring report.

**Lab environment:** Kali Linux VM.

**Safety:** Monitor only your own VM and controlled lab traffic. Avoid capturing sensitive personal traffic or other users' communications.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain network traffic monitoring.
2. Understand RX and TX statistics.
3. Inspect network interface activity.
4. Monitor interface traffic.
5. Understand bandwidth and throughput.
6. Use Linux network monitoring commands.
7. Understand packet capture.
8. Identify common network monitoring indicators.
9. Recognize suspicious traffic patterns.
10. Generate a network monitoring report.

### 3. What Is Network Traffic?

Network traffic refers to data transmitted and received across a network.

Examples include:

- Web browsing.
- Email communication.
- File transfers.
- DNS queries.
- Video streaming.
- Remote administration.
- Application communication.

Traffic moves between devices using network protocols.

Example:

```text
Kali Linux VM
     |
     | Outgoing traffic
     v
VMware Network
     |
     v
Destination
```

Incoming traffic travels in the opposite direction.

### 4. Understand RX and TX

Linux uses two important terms when reporting interface statistics.

**RX – Receive**

Data received by the network interface.

**TX – Transmit**

Data sent by the network interface.

Example:

```text
RX packets:  1200
TX packets:  900
```

These are illustrative values.

| Metric | Meaning |
|---|---|
| RX packets | Packets received |
| TX packets | Packets transmitted |
| RX bytes | Bytes received |
| TX bytes | Bytes transmitted |
| RX errors | Receive errors |
| TX errors | Transmission errors |
| RX dropped | Received packets dropped |
| TX dropped | Transmitted packets dropped |

### 5. Inspect Network Interfaces

Run:

```bash
ip -br link
```

This displays a brief summary of network interfaces.

Next:

```bash
ip -br addr
```

This displays interface addresses.

For detailed information:

```bash
ip link
```

Identify:

- Interface name.
- Interface state.
- MAC address.
- MTU.
- Operational status.

### 6. Inspect Interface Statistics

Run:

```bash
ip -s link
```

This displays packet and byte statistics.

Example:

```text
RX: bytes packets errors dropped
TX: bytes packets errors dropped
```

Interpret the values.

- High packet counts indicate network activity.
- Increasing byte counts indicate data transfer.
- Increasing errors may indicate interface problems.
- Dropped packets may indicate congestion or other network conditions.

A single nonzero counter does not automatically mean a serious problem. Compare values over time.

### 7. Understand MTU

MTU means Maximum Transmission Unit.

It is the maximum packet size that an interface can transmit at the network layer without fragmentation under the relevant configuration.

A common Ethernet MTU is:

```text
1500 bytes
```

Inspect your interface MTU:

```bash
ip link
```

You may see:

```text
mtu 1500
```

MTU problems can cause:

- Fragmentation.
- Connectivity issues.
- Poor network performance.
- Problems with VPN connections.
- Path MTU discovery failures.

Do not change your MTU during this lab.

### 8. Understand Bandwidth and Throughput

Bandwidth is the maximum theoretical data-carrying capacity of a network link.

Throughput is the actual amount of data successfully transferred over a period of time.

Example:

```text
Available bandwidth: 100 Mbps
Actual throughput:    65 Mbps
```

The values are illustrative.

Throughput may be affected by:

- Network congestion.
- Hardware limitations.
- Wireless interference.
- Packet loss.
- Protocol overhead.
- Server performance.

### 9. Monitor Traffic Using /proc/net/dev

Linux exposes network statistics through the `/proc` filesystem.

Run:

```bash
cat /proc/net/dev
```

You may see:

```text
Inter-| Receive | Transmit
 face |bytes packets errs drop|bytes packets errs drop
 eth0: ...
```

This file provides interface-level traffic statistics.

The values are cumulative counters since the interface was initialized.

### 10. Observe Traffic Changes

Run:

```bash
ip -s link
```

Record the RX and TX byte counts.

Now generate harmless traffic:

```bash
ping -c 5 1.1.1.1
```

If internet connectivity is unavailable, you can use:

```bash
ping -c 5 127.0.0.1
```

Inspect the counters again:

```bash
ip -s link
```

Compare the results.

Note: Loopback traffic appears on the loopback interface rather than necessarily on your Ethernet interface.

### 11. Monitor Interfaces with watch

The `watch` utility repeatedly executes a command.

Check whether it is installed:

```bash
command -v watch
```

If available, run:

```bash
watch -n 2 ip -s link
```

This refreshes the interface statistics every two seconds.

Press:

```text
CTRL + C
```

to stop monitoring.

This is useful when observing changes in network activity.

### 12. Monitor Traffic with nload

`nload` is a terminal-based network traffic monitoring utility.

Check availability:

```bash
command -v nload
```

If installed, run:

```bash
nload
```

It can display:

- Incoming traffic.
- Outgoing traffic.
- Current traffic rate.
- Average traffic rate.
- Maximum traffic rate.

If it is unavailable, skip this optional exercise.

Do not install packages just to complete this lab.

### 13. Understand iftop

`iftop` is a network traffic monitoring utility that displays communication between hosts.

Check availability:

```bash
command -v iftop
```

If installed, identify your interface:

```bash
ip -br link
```

Then monitor that interface:

```bash
sudo iftop -i YOUR_INTERFACE
```

Replace the placeholder with your actual interface name.

For example:

```bash
sudo iftop -i eth0
```

Use it only on your own VM.

Press `q` to exit.

If it is unavailable, skip this optional exercise.

### 14. Understand Packet Capture

Packet capture is the process of collecting network packets for analysis.

A packet capture can help investigators examine:

- Source and destination addresses.
- Protocols.
- Ports.
- Packet timing.
- Communication patterns.
- DNS queries.
- Connection behavior.

Common packet analysis tools include:

- Wireshark.
- TShark.
- tcpdump.

Packet capture must be handled carefully because network traffic can contain sensitive information.

For this lab, capture only controlled traffic generated by your own VM.

### 15. Check tcpdump Availability

Run:

```bash
command -v tcpdump
```

If available, inspect its version:

```bash
tcpdump --version
```

You can inspect available interfaces:

```bash
sudo tcpdump -D
```

If tcpdump is unavailable, continue with the other exercises.

### 16. Capture Controlled ICMP Traffic

Identify your interface:

```bash
ip -br addr
```

Open one terminal and run:

```bash
sudo tcpdump -i lo -n icmp
```

This listens only on the loopback interface for ICMP traffic.

Open another terminal and run:

```bash
ping -c 4 127.0.0.1
```

Return to the first terminal and observe the captured packets.

Stop the capture using:

```text
CTRL + C
```

This exercise generates controlled local traffic.

### 17. Understand tcpdump Output

A packet capture may display:

```text
IP 127.0.0.1 > 127.0.0.1: ICMP echo request
IP 127.0.0.1 > 127.0.0.1: ICMP echo reply
```

Interpretation:

| Field | Meaning |
|---|---|
| IP | IPv4 packet |
| First address | Source |
| Second address | Destination |
| ICMP | Protocol |
| Echo request | Ping request |
| Echo reply | Ping response |

The exact output depends on the system and tcpdump version.

### 18. Save a Controlled Packet Capture

If tcpdump is available, create the lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab08
```

Start a short capture:

```bash
sudo tcpdump -i lo -n -c 10 icmp -w ~/practical-labs/module05/lab08/loopback-capture.pcap
```

While it is running, open another terminal and execute:

```bash
ping -c 4 127.0.0.1
```

The capture stops after collecting ten matching packets or when interrupted.

Inspect the capture:

```bash
sudo tcpdump -nn -r ~/practical-labs/module05/lab08/loopback-capture.pcap
```

This creates a packet capture file that can later be examined using Wireshark.

### 19. Understand Network Monitoring Indicators

During monitoring, pay attention to:

| Indicator | Possible Meaning |
|---|---|
| Sudden traffic increase | Heavy application activity or unexpected transfer |
| High packet loss | Network instability |
| Increasing RX errors | Possible receive-side problem |
| Increasing TX errors | Possible transmission problem |
| Unexpected listening service | Service configuration requiring investigation |
| Unusual outbound connections | Activity requiring verification |
| Repeated failed connections | Application or connectivity problem |

These indicators require investigation and context. They are not automatic proof of malicious activity.

### 20. Practical Exercise 1 – Inspect Network Interfaces

Run:

```bash
ip -br link
```

Then:

```bash
ip -br addr
```

Record your active interface and its state.

### 21. Practical Exercise 2 – Inspect Interface Statistics

Run:

```bash
ip -s link
```

Record:

- RX packets.
- TX packets.
- RX bytes.
- TX bytes.
- RX errors.
- TX errors.
- Dropped packets.

### 22. Practical Exercise 3 – Inspect /proc/net/dev

Run:

```bash
cat /proc/net/dev
```

Identify the interface statistics.

Compare the values with those displayed by `ip -s link`.

### 23. Practical Exercise 4 – Monitor Traffic Changes

Run:

```bash
ip -s link
```

Generate harmless traffic:

```bash
ping -c 5 1.1.1.1
```

If internet connectivity is unavailable:

```bash
ping -c 5 127.0.0.1
```

Inspect statistics again.

Explain which interface counters changed.

### 24. Practical Exercise 5 – Live Interface Monitoring

If `watch` is available, run:

```bash
watch -n 2 ip -s link
```

Observe the statistics.

Stop using `CTRL + C`.

If available, explore:

```bash
nload
```

or:

```bash
sudo iftop -i YOUR_INTERFACE
```

These optional tools may not be installed.

### 25. Practical Exercise 6 – Controlled Packet Capture

If tcpdump is installed, inspect interfaces:

```bash
sudo tcpdump -D
```

Capture local ICMP traffic:

```bash
sudo tcpdump -i lo -n icmp
```

In another terminal:

```bash
ping -c 4 127.0.0.1
```

Observe the captured requests and replies.

Stop using `CTRL + C`.

Do not capture unrelated network traffic.

### 26. Generate a Network Monitoring Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab08
```

Create the script:

```bash
nano ~/practical-labs/module05/lab08/network-monitor-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab08/network-monitor-report.txt"

{
    echo "NETWORK TRAFFIC AND INTERFACE MONITORING REPORT"
    echo "================================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "NETWORK INTERFACES"
    ip -br link
    echo

    echo "IP ADDRESSES"
    ip -br addr
    echo

    echo "INTERFACE STATISTICS"
    ip -s link
    echo

    echo "PROC NET DEV"
    cat /proc/net/dev
    echo

    echo "NETWORK SOCKET SUMMARY"
    ss -s
    echo

    echo "ROUTING TABLE"
    ip route
    echo

    echo "PACKET CAPTURE TOOL"
    if command -v tcpdump >/dev/null 2>&1; then
        tcpdump --version
    else
        echo "tcpdump is not installed"
    fi

    echo
    echo "TRAFFIC MONITORING TOOLS"
    for tool in watch nload iftop; do
        if command -v "$tool" >/dev/null 2>&1; then
            echo "$tool: available"
        else
            echo "$tool: unavailable"
        fi
    done

} > "$REPORT" 2>&1

echo "Monitoring report generated: $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab08/network-monitor-report.sh
```

Execute:

```bash
~/practical-labs/module05/lab08/network-monitor-report.sh
```

Inspect:

```bash
cat ~/practical-labs/module05/lab08/network-monitor-report.txt
```

### 27. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab08/lab08-report.txt
```

Record:

```text
MODULE 05 – LAB 08
NETWORK TRAFFIC AND INTERFACE MONITORING

1. What is network traffic?
2. What does RX mean?
3. What does TX mean?
4. Name my active network interface:
5. What is my interface MTU?
6. Number of received packets:
7. Number of transmitted packets:
8. RX errors:
9. TX errors:
10. What is bandwidth?
11. What is throughput?
12. What is the purpose of /proc/net/dev?
13. Is watch available?
14. Is nload available?
15. Is iftop available?
16. Is tcpdump available?
17. What packets did I capture?
18. What is packet capture?
19. Why must packet capture be handled carefully?
20. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 28. Knowledge Check

1. What is network traffic?
2. What does RX mean?
3. What does TX mean?
4. What is the purpose of `ip -s link`?
5. What does MTU stand for?
6. What is bandwidth?
7. What is throughput?
8. What is the purpose of `/proc/net/dev`?
9. What does `watch` do?
10. What is the purpose of nload?
11. What is the purpose of iftop?
12. What is packet capture?
13. What does tcpdump do?
14. What is the difference between RX errors and dropped packets?
15. Why is network traffic monitoring important in cybersecurity?
16. Why should investigators avoid capturing unrelated private traffic?

### 29. Lab Completion Checklist

- [ ] Understood network traffic.
- [ ] Learned RX and TX.
- [ ] Inspected network interfaces.
- [ ] Examined interface statistics.
- [ ] Studied MTU.
- [ ] Understood bandwidth and throughput.
- [ ] Inspected `/proc/net/dev`.
- [ ] Observed traffic changes.
- [ ] Used watch, if available.
- [ ] Explored nload, if available.
- [ ] Explored iftop, if available.
- [ ] Checked tcpdump availability.
- [ ] Captured controlled loopback traffic, if available.
- [ ] Generated the network monitoring report.
- [ ] Completed lab documentation.

**LAB 08 STATUS: READY FOR PRACTICAL COMPLETION**
