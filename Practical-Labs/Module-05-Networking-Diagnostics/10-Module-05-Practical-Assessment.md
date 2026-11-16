
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 10: MODULE 05 PRACTICAL ASSESSMENT

### 1. Assessment Overview

This is the final practical assessment for Module 05.

You will demonstrate your understanding of Linux networking, IP addressing, routing, DNS, connectivity testing, ports, sockets, traffic monitoring, and troubleshooting.

**Assessment environment:** Kali Linux running inside VMware.

**Assessment type:** Practical and theoretical.

**Important safety rule:** Perform all activities on your own Kali VM and its local environment. Do not scan or probe unauthorized systems.

### 2. Assessment Objectives

By completing this assessment, you should demonstrate the ability to:

1. Inspect Linux network interfaces.
2. Identify IPv4 and IPv6 addresses.
3. Understand subnetting and CIDR.
4. Inspect routing tables and default gateways.
5. Investigate DNS resolution.
6. Test network connectivity.
7. Inspect listening ports and active connections.
8. Monitor network interface statistics.
9. Diagnose common networking problems.
10. Generate a professional network diagnostic report.

---

## PART A – NETWORK INTERFACE INVESTIGATION

### Task 1: Inspect Network Interfaces

Run:

```bash
ip link
ip -br link
ip -br addr
```

Answer in your report:

1. How many network interfaces are visible?
2. Which interface is active?
3. What is its MAC address?
4. Does it have an IPv4 address?
5. Does it have an IPv6 address?

### Task 2: Inspect Interface Statistics

Run:

```bash
ip -s link
```

Record:

- Received packets.
- Transmitted packets.
- Received bytes.
- Transmitted bytes.
- Errors.
- Dropped packets.

Explain the difference between RX and TX.

---

## PART B – IP ADDRESSING AND SUBNETTING

### Task 3: Inspect IP Configuration

Run:

```bash
ip -4 addr
ip -6 addr
```

Record your assigned addresses.

Identify:

- IPv4 address.
- IPv4 prefix length.
- IPv6 address, if assigned.
- Interface associated with each address.

### Task 4: Subnetting Questions

Solve the following exercises manually.

**Question 1**

Given:

```text
IP Address: 192.168.10.25
Subnet Mask: 255.255.255.0
```

Determine:

- CIDR notation.
- Network address.
- Broadcast address.
- Usable host range.
- Number of usable host addresses.

**Question 2**

Given:

```text
IP Address: 192.168.20.70/26
```

Determine:

- Subnet mask.
- Network address.
- Broadcast address.
- Usable host range.
- Number of usable host addresses.

**Question 3**

Explain the difference between IPv4 and IPv6.

---

## PART C – ROUTING AND GATEWAY INVESTIGATION

### Task 5: Inspect Routing

Run:

```bash
ip route
ip route show default
ip -6 route
```

Answer:

1. What is a routing table?
2. What is a default gateway?
3. What does a default route do?
4. Which interface is associated with your default route?

### Task 6: Perform a Routing Lookup

Run:

```bash
ip route get 1.1.1.1
```

Record the output.

Explain how Linux determines which route and interface to use.

---

## PART D – DNS INVESTIGATION

### Task 7: Inspect DNS Configuration

Run:

```bash
cat /etc/resolv.conf
```

Then:

```bash
getent hosts example.com
```

If available:

```bash
dig example.com
```

Answer:

1. What is DNS?
2. What is a DNS resolver?
3. What is an A record?
4. What is an AAAA record?
5. What is a CNAME record?
6. What happens when DNS resolution fails?

---

## PART E – NETWORK CONNECTIVITY TESTING

### Task 8: Test Local Connectivity

Run:

```bash
ping -c 4 127.0.0.1
```

Explain the purpose of the loopback address.

### Task 9: Test Gateway Connectivity

Identify your gateway:

```bash
ip route show default
```

If a gateway is present, test it:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Replace the placeholder with your actual gateway address.

### Task 10: Test External Connectivity

If internet access is available, run:

```bash
ping -c 4 1.1.1.1
```

Then:

```bash
getent hosts example.com
```

Explain how these two tests help distinguish IP connectivity problems from DNS problems.

### Task 11: Inspect an Application Connection

If `curl` is available, run:

```bash
curl -I --max-time 10 https://example.com
```

Explain what this command checks.

---

## PART F – PORTS, SOCKETS AND CONNECTIONS

### Task 12: Inspect Listening Ports

Run:

```bash
ss -tuln
```

Then:

```bash
sudo ss -tulpn
```

Identify:

- TCP listening sockets.
- UDP listening sockets.
- Local addresses.
- Port numbers.
- Associated process information, if available.

### Task 13: Inspect Active TCP Connections

Run:

```bash
ss -tn
```

Then:

```bash
ss -s
```

Explain the difference between listening sockets and established TCP connections.

---

## PART G – NETWORK TRAFFIC MONITORING

### Task 14: Monitor Network Interfaces

Run:

```bash
ip -s link
```

If available:

```bash
watch -n 2 ip -s link
```

Press `CTRL + C` to stop.

Explain what RX and TX counters tell you.

### Task 15: Controlled Packet Capture

Use the loopback interface to generate and capture your own test traffic.

Open Terminal 1:

```bash
sudo tcpdump -i lo -n icmp
```

Open Terminal 2:

```bash
ping -c 4 127.0.0.1
```

Observe the packets in Terminal 1.

Stop the capture using:

```text
CTRL + C
```

If `tcpdump` is unavailable, record that limitation instead of installing anything unnecessarily.

Explain:

1. What is packet capture?
2. What is an ICMP packet?
3. What does the `-n` option do?
4. Why is loopback suitable for this controlled exercise?

---

## PART H – NETWORK TROUBLESHOOTING

### Task 16: Investigate a Simulated Problem

Consider this situation:

A Linux computer has the following symptoms:

- The network interface is active.
- The computer has an IPv4 address.
- The routing table contains a default route.
- The gateway responds to ping.
- External IP connectivity works.
- Domain names cannot be resolved.

Answer:

1. What is the most likely area of failure?
2. Which commands would you use to investigate?
3. What information would you collect?
4. What corrective action might be appropriate?
5. How would you verify the correction?

### Task 17: Investigate an Unexpected Listening Port

A system administrator discovers an unfamiliar listening port.

Explain the investigation process using:

```bash
sudo ss -tulpn
ps -p YOUR_PID -f
systemctl status SERVICE_NAME
```

Explain why identifying the associated process is important before taking action.

---

## PART I – FINAL NETWORK DIAGNOSTIC REPORT

### Task 18: Create the Assessment Directory

Run:

```bash
mkdir -p ~/practical-labs/module05/final-assessment
```

Create the report script:

```bash
nano ~/practical-labs/module05/final-assessment/network-assessment.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/final-assessment/network-assessment-report.txt"

{
    echo "MODULE 05 – FINAL NETWORK ASSESSMENT"
    echo "====================================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "SYSTEM INFORMATION"
    uname -a
    echo

    echo "NETWORK INTERFACES"
    ip -br addr
    echo

    echo "INTERFACE STATISTICS"
    ip -s link
    echo

    echo "IPV4 CONFIGURATION"
    ip -4 addr
    echo

    echo "IPV6 CONFIGURATION"
    ip -6 addr
    echo

    echo "IPV4 ROUTING TABLE"
    ip -4 route
    echo

    echo "IPV6 ROUTING TABLE"
    ip -6 route
    echo

    echo "NEIGHBOR TABLE"
    ip neigh
    echo

    echo "DNS CONFIGURATION"
    cat /etc/resolv.conf
    echo

    echo "DNS RESOLUTION"
    getent hosts example.com
    echo

    echo "LISTENING SOCKETS"
    sudo ss -tulpn
    echo

    echo "TCP CONNECTION SUMMARY"
    ss -s
    echo

    echo "RECENT NETWORK-RELATED KERNEL MESSAGES"
    journalctl -k -n 30 --no-pager

} > "$REPORT" 2>&1

echo "Assessment report generated:"
echo "$REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/final-assessment/network-assessment.sh
```

Run:

```bash
sudo ~/practical-labs/module05/final-assessment/network-assessment.sh
```

Inspect:

```bash
cat ~/practical-labs/module05/final-assessment/network-assessment-report.txt
```

Your report should contain evidence from the major networking diagnostic areas covered in this module.

---

## PART J – THEORETICAL EXAMINATION

Answer these questions in your own study notes.

1. Define computer networking.
2. Explain LAN, WAN, MAN, and PAN.
3. Differentiate between an IP address and a MAC address.
4. Explain the OSI model.
5. Explain the TCP/IP model.
6. Differentiate between TCP and UDP.
7. What is IPv4?
8. What is IPv6?
9. Explain CIDR notation.
10. What is a subnet mask?
11. What is a default gateway?
12. What is routing?
13. Explain the purpose of DNS.
14. Identify five DNS record types.
15. What is ICMP?
16. What is latency?
17. What is packet loss?
18. What is a port?
19. Differentiate between a socket and a port.
20. What is a listening service?
21. Explain network traffic monitoring.
22. What is packet capture?
23. Explain the purpose of `tcpdump`.
24. What is the purpose of `ss`?
25. Explain a systematic network troubleshooting process.

---

## 3. Final Assessment Checklist

- [ ] Inspected network interfaces.
- [ ] Inspected IPv4 and IPv6 configuration.
- [ ] Completed subnetting calculations.
- [ ] Inspected routing tables.
- [ ] Performed a routing lookup.
- [ ] Investigated DNS configuration.
- [ ] Tested local connectivity.
- [ ] Tested gateway connectivity where available.
- [ ] Tested external connectivity where available.
- [ ] Inspected application connectivity.
- [ ] Inspected listening ports.
- [ ] Inspected active TCP connections.
- [ ] Examined interface statistics.
- [ ] Performed controlled packet capture if available.
- [ ] Completed the DNS troubleshooting scenario.
- [ ] Investigated an unfamiliar listening port.
- [ ] Generated the final diagnostic report.
- [ ] Reviewed the theoretical examination.

## 4. Assessment Completion

You have now completed the practical activities for:

**MODULE 05 – LINUX NETWORKING DIAGNOSTICS**

### Skills Acquired

You have practised:

- Linux network interface inspection.
- IP addressing and subnetting.
- Routing and gateway diagnostics.
- DNS investigation.
- Connectivity testing.
- Port and socket inspection.
- Traffic and interface monitoring.
- Network troubleshooting.
- Diagnostic evidence collection and reporting.

**MODULE 05 STATUS: READY FOR COMPLETION**
