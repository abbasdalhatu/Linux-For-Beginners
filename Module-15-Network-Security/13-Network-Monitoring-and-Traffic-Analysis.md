---
---

# MODULE 15 – NETWORK SECURITY
## LESSON 13: NETWORK MONITORING AND TRAFFIC ANALYSIS

---

## 1. Introduction

Network monitoring is the continuous observation of network devices, connections, traffic, and performance.

Organizations monitor networks to identify:

- Unauthorized connections.
- Suspicious traffic.
- Unusual bandwidth usage.
- Network failures.
- Malware communication.
- Service disruptions.
- Security incidents.

Traffic analysis involves examining network communication to understand what is happening across a network.

In this lesson, you will learn how network administrators and cybersecurity professionals monitor and analyze network activity.

---

## 2. What Is Network Monitoring?

Network monitoring is the process of collecting and analyzing information about network infrastructure and communication.

It helps administrators understand the health, performance, availability, and security of a network.

### Example

Imagine a university network suddenly becomes slow.

A network administrator investigates:

1. Which devices are connected?
2. How much bandwidth is being consumed?
3. Are there unusual connections?
4. Is a server responding slowly?
5. Are there signs of malicious activity?

Network monitoring helps answer these questions.

### Main objectives

- Detect network problems.
- Monitor network availability.
- Identify suspicious behavior.
- Measure performance.
- Investigate security incidents.
- Maintain reliable services.

---

## 3. What Is Network Traffic?

Network traffic is the data transmitted between devices over a network.

Examples include:

- Web browsing.
- Email communication.
- File transfers.
- Video streaming.
- DNS requests.
- Remote administration.
- Database communication.

### Types of network traffic

**Inbound traffic:** Data entering a network or device.

**Outbound traffic:** Data leaving a network or device.

**Internal traffic:** Communication between devices within an organization's network.

**External traffic:** Communication between an internal device and an external network.

### Example

```text
       INTERNET
           |
           |
      NETWORK ROUTER
           |
     +-----+-----+
     |           |
     v           v
   Laptop      Server
```

Traffic can flow between the laptop, server, router, and external systems.

---

## 4. Important Network Monitoring Metrics

Administrators monitor different metrics to understand network performance.

### 4.1 Bandwidth

Bandwidth is the maximum data-transfer capacity of a network connection.

It is commonly measured in bits per second.

Examples:

- Mbps: Megabits per second.
- Gbps: Gigabits per second.

### 4.2 Throughput

Throughput is the actual amount of data successfully transferred over a period.

Throughput is often lower than the theoretical bandwidth.

### 4.3 Latency

Latency is the time taken for data to travel from one point to another.

It is commonly measured in milliseconds (ms).

### 4.4 Packet Loss

Packet loss occurs when transmitted packets fail to reach their destination.

High packet loss can cause:

- Slow connections.
- Video interruptions.
- Poor voice communication.
- Application failures.

### 4.5 Jitter

Jitter is variation in packet arrival time.

It is particularly important for:

- Voice over IP.
- Video conferencing.
- Real-time communication.

### 4.6 Availability

Availability measures how often a network service or device is operational.

A highly available system experiences minimal downtime.

---

## 5. Network Monitoring Methods

### 5.1 Active Monitoring

Active monitoring involves sending test traffic to evaluate a network or service.

Examples:

- Ping tests.
- Traceroute.
- Synthetic HTTP requests.
- Service availability checks.

### 5.2 Passive Monitoring

Passive monitoring observes existing network communication without generating test traffic.

Examples:

- Packet capture.
- Network traffic analysis.
- Flow monitoring.
- Interface statistics.

### Difference

| Active Monitoring | Passive Monitoring |
|---|---|
| Generates test traffic | Observes existing traffic |
| Tests reachability | Examines actual communication |
| Measures response time | Analyzes traffic patterns |
| Examples: ping, traceroute | Examples: Wireshark, tcpdump |

Both methods are useful in network operations and cybersecurity.

---

## 6. What Is Packet Analysis?

Packet analysis is the examination of individual network packets to understand network communication.

A packet may contain:

- Source IP address.
- Destination IP address.
- Source port.
- Destination port.
- Protocol.
- Packet length.
- TCP flags.
- Payload information.

### Simplified packet structure

```text
+----------------------------+
| Ethernet Header            |
+----------------------------+
| IP Header                  |
+----------------------------+
| TCP / UDP Header           |
+----------------------------+
| Application Data           |
+----------------------------+
```

Different protocols use different packet structures.

### Why packet analysis matters

It can help detect:

- Suspicious connections.
- DNS failures.
- Unusual traffic patterns.
- Protocol misconfigurations.
- Unexpected retransmissions.
- Potential data exposure.

---

## 7. Common Network Monitoring Tools

### 7.1 Wireshark

Wireshark is a graphical packet analysis tool.

It allows analysts to inspect network packets and filter communication.

Common uses:

- Protocol analysis.
- DNS troubleshooting.
- TCP troubleshooting.
- Network incident investigation.

### 7.2 tcpdump

tcpdump is a command-line packet capture and analysis tool.

Example:

```bash
tcpdump -D
```

Lists available capture interfaces.

For a passive capture on your own VM interface:

```bash
sudo tcpdump -i eth0 -c 20
```

Replace `eth0` with your actual interface name.

### 7.3 Nmap

Nmap is a network discovery and security auditing tool.

It can identify hosts and services within an authorized assessment scope.

### 7.4 Netstat and ss

These tools display network connections and socket information.

Example:

```bash
ss -tulpen
```

### 7.5 ip

The `ip` command displays and manages network configuration.

Example:

```bash
ip addr
ip route
```

### 7.6 Nagios

Nagios is a monitoring platform used to monitor infrastructure and services.

### 7.7 Zabbix

Zabbix is an infrastructure monitoring platform.

It supports monitoring devices, servers, services, and performance metrics.

### 7.8 Prometheus and Grafana

Prometheus collects and stores time-series monitoring data.

Grafana provides dashboards for visualizing monitoring information.

---

## 8. Understanding TCP Traffic

TCP is a connection-oriented transport protocol.

It uses a handshake to establish a connection.

### TCP three-way handshake

```text
Client                    Server
  |                          |
  |-------- SYN ------------>|
  |                          |
  |<----- SYN-ACK -----------|
  |                          |
  |-------- ACK ------------>|
  |                          |
  |                    Connection
  |                     Established
```

The three steps are:

1. SYN: Client requests a connection.
2. SYN-ACK: Server acknowledges and responds.
3. ACK: Client confirms the connection.

### Important TCP flags

| Flag | Meaning |
|---|---|
| SYN | Synchronize |
| ACK | Acknowledgment |
| FIN | Finish |
| RST | Reset |
| PSH | Push |
| URG | Urgent |

Monitoring TCP behavior can help diagnose connection problems and suspicious activity.

---

## 9. Understanding DNS Traffic

DNS translates domain names into IP addresses.

Example:

```text
example.com
     |
     v
DNS Resolver
     |
     v
IP Address
```

DNS monitoring can help identify:

- Failed name resolution.
- Unexpected DNS servers.
- Unusual DNS request volumes.
- Suspicious domain lookups.
- Possible DNS tunneling indicators.

DNS traffic commonly uses UDP port 53, with TCP port 53 also used in specific situations.

Encrypted DNS protocols include DNS over HTTPS (DoH) and DNS over TLS (DoT).

---

## 10. Network Traffic Indicators

Security analysts investigate unusual network behavior.

Examples include:

### 10.1 Unusual Traffic Volume

A device suddenly sends or receives much more traffic than normal.

### 10.2 Unexpected Connections

A device communicates with unfamiliar external destinations.

### 10.3 Repeated Connection Failures

Repeated unsuccessful connection attempts may indicate misconfiguration or suspicious activity.

### 10.4 Unusual DNS Queries

A device makes repeated requests for unusual or suspicious domains.

### 10.5 Unexpected Listening Services

A device exposes a network service that should not be running.

### 10.6 Unusual Outbound Traffic

A workstation communicates with external systems despite having no expected reason to do so.

These indicators require investigation. One unusual event does not automatically prove an attack.

---

## 11. Understanding Network Flow Monitoring

Flow monitoring summarizes communication between network endpoints.

Instead of storing every packet, a flow record commonly contains information such as:

- Source IP.
- Destination IP.
- Source port.
- Destination port.
- Protocol.
- Packet count.
- Byte count.
- Start and end times.

### Common technologies

- NetFlow.
- IPFIX.
- sFlow.

### Example flow record

```text
Source IP:       192.168.1.10
Destination IP:  93.184.216.34
Source Port:     51520
Destination Port: 443
Protocol:        TCP
Packets:         120
Bytes:           85000
```

This is an illustrative record, not a real network observation.

Flow monitoring helps identify communication patterns and unusual traffic volumes.

---

## 12. Introduction to Network Baselines

A network baseline describes normal network behavior.

Security teams use baselines to identify deviations.

### Example

An organization normally observes:

- Office computers accessing approved web services.
- DNS requests to authorized resolvers.
- Regular backups at scheduled times.
- Expected server-to-server communication.

If a workstation suddenly sends unusually large amounts of data to an unknown external destination, analysts may investigate.

### Why baselines matter

Without knowing what is normal, it is difficult to identify what is unusual.

---

## 13. Network Monitoring and SIEM

SIEM means Security Information and Event Management.

A SIEM collects and analyzes security information from different sources.

Examples include:

- Firewalls.
- Servers.
- Endpoints.
- IDS/IPS systems.
- Authentication systems.
- Network monitoring systems.

### Simplified SIEM architecture

```text
 Firewall Logs
      |
      |
 Server Logs
      |
      |
 IDS/IPS Alerts
      |
      v
  SIEM Platform
      |
      v
 Correlation and Analysis
      |
      v
 Security Alerts
      |
      v
 Security Analyst
```

### SIEM functions

- Log collection.
- Event correlation.
- Alert generation.
- Threat investigation.
- Incident reporting.
- Security dashboards.

---

## 14. PRACTICAL LAB 1: INSPECT NETWORK CONNECTIONS

### Objective

Inspect the network connections and listening services on your own Kali VM.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson13
cd ~/module15-lab/lesson13
```

### Step 2: Display network interfaces

```bash
ip -br addr
```

### Step 3: Display routing information

```bash
ip route
```

### Step 4: Display listening sockets

```bash
ss -tulpen
```

### Step 5: Display established TCP connections

```bash
ss -tn state established
```

### Step 6: Display interface statistics

```bash
ip -s link
```

### Step 7: Save the results

```bash
ip -br addr > interfaces.txt
ip route > routes.txt
ss -tulpen > listening-services.txt
ss -tn state established > established-connections.txt
ip -s link > interface-statistics.txt
```

### Step 8: Analyze

Identify:

- Active network interfaces.
- Listening TCP ports.
- Listening UDP ports.
- Established TCP connections.
- Interface packet statistics.

Do not assume that every listening service is malicious. Determine whether it is expected.

---

## 15. PRACTICAL LAB 2: PASSIVE PACKET CAPTURE

### Objective

Capture a small number of packets from your own Kali VM interface.

This is a passive, local inspection exercise.

### Step 1: Identify your interface

```bash
ip -br link
```

### Step 2: Check whether tcpdump is installed

```bash
command -v tcpdump
```

### Step 3: Capture a limited number of packets

Replace `eth0` with your actual interface:

```bash
sudo tcpdump -i eth0 -c 20
```

### Step 4: Save a capture file

```bash
sudo tcpdump -i eth0 -c 20 -w lesson13-capture.pcap
```

### Step 5: Read the saved capture

```bash
tcpdump -r lesson13-capture.pcap
```

### Step 6: Display a summary

```bash
tcpdump -r lesson13-capture.pcap -nn
```

### Step 7: Record your observation

Create:

```bash
nano packet-analysis.txt
```

Use this template:

```text
NETWORK PACKET ANALYSIS

Capture Interface:

Number of Packets:

Protocols Observed:

Source Addresses:

Destination Addresses:

Interesting Observations:

Unexpected Activity:

Conclusion:
```

Packet captures can contain sensitive information. Keep your captures private and use only traffic you are authorized to inspect.

---

## 16. PRACTICAL LAB 3: DNS TRAFFIC ANALYSIS

### Objective

Understand DNS resolution using a normal DNS lookup.

### Step 1: Check whether dig is installed

```bash
command -v dig
```

### Step 2: Query a domain

```bash
dig example.com
```

### Step 3: Display only the answer

```bash
dig example.com +short
```

### Step 4: Inspect the DNS response details

```bash
dig example.com +noall +answer
```

### Step 5: Save the result

```bash
dig example.com > dns-analysis.txt
```

### Step 6: Analyze

Identify:

- The queried domain.
- The DNS server used.
- The record type.
- The response status.
- The returned answer, if available.

DNS results may differ depending on resolver configuration and time.

---

## 17. PRACTICAL PROJECT: MINI NETWORK MONITORING REPORT

### Objective

Create a simple report describing the network state of your Kali VM.

Create:

```bash
nano network-monitoring-report.txt
```

Use this template:

```text
NETWORK MONITORING REPORT

1. SYSTEM INFORMATION

Hostname:
Operating System:
Date:

2. NETWORK INTERFACES

Interface Names:
Interface States:
Assigned IP Addresses:

3. ROUTING

Default Gateway:
Network Routes:

4. LISTENING SERVICES

TCP Listening Services:
UDP Listening Services:

5. ACTIVE CONNECTIONS

Established Connections:
Unexpected Connections:

6. INTERFACE STATISTICS

Received Packets:
Transmitted Packets:
Errors:
Dropped Packets:

7. DNS ANALYSIS

DNS Resolver:
Domain Tested:
DNS Result:

8. SECURITY OBSERVATIONS

Unexpected Services:
Unusual Traffic:
Potential Risks:

9. RECOMMENDATIONS

10. CONCLUSION
```

Use actual command output. If information is unavailable, state that rather than inventing it.

---

## 18. SCENARIO-BASED EXERCISE

### Scenario A

A company notices unusually high outbound traffic from one employee computer.

What information should the security analyst investigate?

### Scenario B

A server has an unexpected listening port.

What steps should an administrator take?

### Scenario C

A network experiences high packet loss and latency.

Which monitoring metrics should be examined?

### Scenario D

A workstation repeatedly queries suspicious domains.

Which network monitoring tools could help investigate?

### Scenario E

A security analyst notices repeated connections to an unfamiliar external IP address.

Why should the analyst investigate the connection before deciding whether it is malicious?

---

## 19. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is network monitoring?
2. What is network traffic?
3. Differentiate between inbound and outbound traffic.
4. What is bandwidth?
5. What is throughput?
6. Define latency.
7. What is packet loss?
8. What is jitter?
9. Differentiate between active and passive monitoring.
10. What is packet analysis?
11. What is Wireshark used for?
12. What is tcpdump?
13. What does the TCP SYN flag mean?
14. Explain the TCP three-way handshake.
15. What is DNS monitoring?
16. What is flow monitoring?
17. What is NetFlow?
18. What is a network baseline?
19. What is SIEM?
20. Why is continuous network monitoring important?

---

## 20. LESSON SUMMARY

In this lesson, you learned:

- Network monitoring fundamentals.
- Network traffic and traffic direction.
- Bandwidth, throughput, latency, jitter, and packet loss.
- Active and passive monitoring.
- Packet analysis.
- Wireshark and tcpdump.
- TCP and DNS traffic.
- Network flow monitoring.
- Network baselines.
- Network traffic indicators.
- SIEM fundamentals.
- How to inspect Kali network connections.
- How to capture and inspect packets safely.
- How to investigate DNS resolution.
- How to write a network monitoring report.

**KEY TAKEAWAY:**

Effective network monitoring helps organizations understand normal network behavior, detect unusual activity, investigate security incidents, and maintain reliable network services.

**END OF LESSON 13**