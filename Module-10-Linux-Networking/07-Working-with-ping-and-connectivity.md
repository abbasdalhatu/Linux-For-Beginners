# Module 10 – Lesson 07: Working with ping and Connectivity Testing

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand network connectivity testing.
2. Explain how the `ping` command works.
3. Understand ICMP and ICMP Echo messages.
4. Test connectivity between network devices.
5. Interpret packet loss and response times.
6. Understand TTL values.
7. Troubleshoot common connectivity problems.
8. Use additional Linux tools to test network connectivity.

---

## 2. What Is Network Connectivity Testing?

Network connectivity testing is the process of checking whether one device can communicate with another device over a network.

For example, you may want to determine whether:

- Your Kali Linux VM can communicate with its gateway.
- Your computer can reach another computer.
- Your Linux system can reach an internet destination.
- A remote server is responding.
- A network connection is experiencing packet loss.

Network administrators and cybersecurity professionals use connectivity testing to identify network problems.

---

## 3. Understanding the ping Command

`ping` is a Linux command-line utility used to test network reachability.

It sends ICMP Echo Request messages to a destination and waits for ICMP Echo Reply messages.

ICMP stands for Internet Control Message Protocol.

It is used by network devices to exchange control and diagnostic information.

### Basic Syntax

```bash
ping [OPTIONS] DESTINATION
```

The destination can be:

- An IP address.
- A hostname.
- A domain name.

### Example

```bash
ping 8.8.8.8
```

This sends ICMP Echo Requests to Google's public DNS IP address.

A successful response may look like:

```text
64 bytes from 8.8.8.8: icmp_seq=1 ttl=116 time=25.4 ms
```

Understanding this output:

| Field | Meaning |
|---|---|
| `64 bytes` | Size of the received ICMP response |
| `8.8.8.8` | Responding IP address |
| `icmp_seq=1` | Sequence number of the request |
| `ttl=116` | Remaining Time To Live |
| `time=25.4 ms` | Round-trip time |

---

## 4. Understanding ICMP

ICMP is a network-layer protocol used for diagnostics and error reporting.

It is associated with IP networking.

Common ICMP messages include:

| Message | Purpose |
|---|---|
| Echo Request | Requests a response from another device |
| Echo Reply | Responds to an Echo Request |
| Destination Unreachable | Indicates that a destination or network cannot be reached |
| Time Exceeded | Indicates that a packet's TTL expired |

### How ping Works

1. Your computer creates an ICMP Echo Request.
2. The request is placed inside an IP packet.
3. The packet travels through the network.
4. If the destination permits and supports a response, it sends an ICMP Echo Reply.
5. Your computer calculates the round-trip time.

### Simple Communication Diagram

```text
Kali Linux VM                 Destination
     |                             |
     |------ Echo Request -------->|
     |                             |
     |<------- Echo Reply ---------|
     |                             |
     |       Connection tested     |
```

Important: A successful ping demonstrates that ICMP communication worked. It does not prove that every application or service on the destination is available.

---

## 5. Basic ping Commands

### 5.1 Ping an IP Address

```bash
ping 8.8.8.8
```

Tests connectivity to the destination IP address.

### 5.2 Ping a Domain Name

```bash
ping example.com
```

Tests name resolution and attempts ICMP communication with the resolved address.

### 5.3 Ping the Localhost

```bash
ping 127.0.0.1
```

Tests the local IPv4 loopback interface.

### 5.4 Ping Your Default Gateway

First identify your gateway:

```bash
ip route
```

Look for a line similar to:

```text
default via 192.168.100.1 dev eth0
```

Then test it:

```bash
ping 192.168.100.1
```

Replace the example address with your actual gateway.

---

## 6. Controlling the Number of Ping Requests

Linux `ping` normally continues sending requests until you stop it.

Use `Ctrl + C` to stop it.

### 6.1 Send Four Requests

```bash
ping -c 4 8.8.8.8
```

The `-c` option specifies the number of requests.

### 6.2 Send Ten Requests

```bash
ping -c 10 example.com
```

### 6.3 Set the Interval Between Requests

```bash
ping -i 2 -c 5 8.8.8.8
```

This sends five requests with approximately two seconds between them.

### 6.4 Set a Timeout

```bash
ping -W 3 -c 4 8.8.8.8
```

The `-W` option specifies how long to wait for a response to an individual request.

---

## 7. Understanding Packet Loss

Packet loss occurs when some transmitted packets fail to reach their intended destination or their responses fail to return.

Example:

```text
4 packets transmitted, 3 received, 25% packet loss
```

This means:

- Four requests were transmitted.
- Three responses were received.
- One response was not received.
- Packet loss was 25%.

### Packet Loss Formula

\[
\text{Packet Loss} =
\frac{\text{Packets Sent}-\text{Packets Received}}
{\text{Packets Sent}}\times100
\]

### Solved Example

Given:

- Packets sent = 20
- Packets received = 18

\[
\frac{20-18}{20}\times100
\]

\[
=\frac{2}{20}\times100
\]

\[
=10\%
\]

**Final Answer: 10% packet loss.**

Packet loss may result from:

- Network congestion.
- Weak wireless signals.
- Faulty network hardware.
- Routing problems.
- Firewall filtering.
- Destination overload.

A single missing response does not automatically mean a network is broken.

---

## 8. Understanding Latency and Round-Trip Time

Latency is the time required for data to travel through a network.

Ping reports round-trip time (RTT), which measures the time for a request to reach its destination and for the response to return.

It is normally measured in milliseconds (ms).

Example:

```text
time=35.7 ms
```

This means the measured round trip took approximately 35.7 milliseconds.

### Understanding Latency Values

| RTT | General Interpretation |
|---|---|
| Below 10 ms | Very low latency |
| 10–50 ms | Often good latency |
| 50–100 ms | Moderate latency |
| Above 100 ms | Higher latency |

These are general illustrations, not universal quality standards. Acceptable latency depends on the application and network path.

### Why Latency Matters

High latency can affect:

- Online gaming.
- Video calls.
- Remote desktop sessions.
- Cloud applications.
- Real-time communication.

---

## 9. Understanding TTL

TTL stands for Time To Live.

In IPv4, TTL is a field in the IP header.

Each router that forwards a packet normally reduces its TTL by at least one.

When the TTL reaches zero, the packet is discarded and an ICMP Time Exceeded message may be returned.

TTL helps prevent packets from circulating indefinitely.

Example:

```text
64 bytes from 8.8.8.8: ttl=116 time=25.4 ms
```

The displayed TTL is the remaining value in the response packet.

Important: You cannot reliably determine the destination's original TTL from one ping response because the initial TTL and network path are not necessarily known.

---

## 10. Testing IPv4 and IPv6 Connectivity

### 10.1 Test IPv4

```bash
ping -4 -c 4 example.com
```

### 10.2 Test IPv6

```bash
ping -6 -c 4 example.com
```

IPv6 testing requires working IPv6 connectivity and an IPv6 address for the destination.

### 10.3 Test IPv6 Loopback

```bash
ping -6 -c 4 ::1
```

The IPv6 loopback address is `::1`.

---

## 11. Understanding Common ping Errors

### 11.1 Destination Host Unreachable

Example:

```text
Destination Host Unreachable
```

Possible causes:

- No valid route to the destination.
- Gateway problems.
- Local network connectivity issues.
- Neighbour address resolution failure.

### 11.2 Network is Unreachable

Example:

```text
Network is unreachable
```

This commonly indicates that the system has no usable route for the destination.

Check:

```bash
ip route
```

### 11.3 Temporary Failure in Name Resolution

Example:

```text
Temporary failure in name resolution
```

Possible causes:

- DNS configuration problems.
- DNS server unavailable.
- Network connectivity failure.

Inspect DNS configuration:

```bash
cat /etc/resolv.conf
```

### 11.4 100% Packet Loss

Example:

```text
4 packets transmitted, 0 received, 100% packet loss
```

This means no responses were received.

However, it does not prove that the destination is offline. A firewall or network policy may block ICMP messages.

---

## 12. Additional Connectivity Testing Commands

### 12.1 Check the System's Network Interfaces

```bash
ip addr
```

### 12.2 Check the Routing Table

```bash
ip route
```

### 12.3 Check DNS Resolution

```bash
getent hosts example.com
```

### 12.4 Check Whether a TCP Port Is Reachable

If Netcat is installed:

```bash
nc -vz example.com 443
```

This attempts a TCP connection to port 443.

A successful TCP connection indicates that the port accepted the connection attempt. It does not prove that the application is functioning correctly.

### 12.5 Inspect Listening TCP and UDP Ports

```bash
ss -tuln
```

### 12.6 Trace the Network Path

If `traceroute` is installed:

```bash
traceroute example.com
```

If the command is missing, you can install it through Kali's package manager:

```bash
sudo apt update
sudo apt install traceroute
```

Traceroute attempts to identify routers along a network path. Some routers do not respond to its probes, so missing hops do not necessarily indicate a broken route.

---

## 13. Practical Lab: Connectivity Testing in Kali Linux

### Lab Objective

Use `ping` and other network utilities to test your Kali VM's connectivity.

Only test your own VM, your own gateway, and public destinations for ordinary diagnostic purposes.

### Step 1: Test the Loopback Interface

```bash
ping -c 4 127.0.0.1
```

Record whether all four requests received responses.

### Step 2: Identify Your Network Interface

```bash
ip -br addr
```

Record your main network interface.

### Step 3: Identify Your Default Gateway

```bash
ip route
```

Record the default gateway if one exists.

### Step 4: Test Your Gateway

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Replace `YOUR_GATEWAY_IP` with the actual gateway address.

### Step 5: Test an External IP Address

```bash
ping -c 4 1.1.1.1
```

Observe the response time and packet loss.

### Step 6: Test Domain Name Resolution and Connectivity

```bash
ping -c 4 example.com
```

Observe whether the hostname resolves and whether responses arrive.

### Step 7: Test IPv4 Explicitly

```bash
ping -4 -c 4 example.com
```

### Step 8: Test IPv6 Loopback

```bash
ping -6 -c 4 ::1
```

### Step 9: Check DNS Resolution Separately

```bash
getent hosts example.com
```

Compare this result with the domain ping test.

### Step 10: Inspect the Network Path

If available:

```bash
traceroute example.com
```

Observe the hops and any routers that do not respond.

---

## 14. Practical Analysis

Use your lab results to complete this table:

| Test | Result |
|---|---|
| Loopback ping | Successful / Failed |
| Gateway ping | Successful / Failed / Not available |
| External IP ping | Successful / Failed |
| Domain ping | Successful / Failed |
| IPv4 test | Successful / Failed |
| IPv6 loopback | Successful / Failed |
| DNS resolution | Successful / Failed |
| Traceroute | Completed / Incomplete / Not installed |

Do not assume that a failed public ping means your VM has no internet access. Some networks block ICMP while allowing normal web traffic.

---

## 15. Assessment Questions

Answer the following questions:

1. What is the purpose of the `ping` command?
2. What does ICMP stand for?
3. What is the difference between an Echo Request and an Echo Reply?
4. What does `ping -c 4 8.8.8.8` do?
5. What is packet loss?
6. Calculate packet loss when 50 packets are sent and 45 are received.
7. What does RTT mean?
8. What does TTL stand for?
9. Does 100% packet loss always prove that a destination is offline? Explain.
10. Which command can test whether a TCP service on port 443 is reachable?

---

## 16. Important Commands to Remember

```bash
ping 8.8.8.8
ping example.com
ping 127.0.0.1

ping -c 4 8.8.8.8
ping -i 2 -c 5 8.8.8.8
ping -W 3 -c 4 8.8.8.8

ping -4 -c 4 example.com
ping -6 -c 4 ::1

ip addr
ip route
getent hosts example.com
nc -vz example.com 443
ss -tuln
traceroute example.com
```

---

## 17. Lesson Summary

In this lesson, you learned:

- Network connectivity testing checks communication between devices.
- `ping` uses ICMP Echo Requests and Echo Replies.
- Packet loss measures the percentage of requests that did not receive responses.
- Latency measures network delay.
- RTT measures the time for a request and response to complete.
- TTL prevents packets from circulating indefinitely.
- `getent hosts` helps inspect DNS resolution.
- Netcat can test TCP connectivity.
- Traceroute helps investigate network paths.
- ICMP filtering can cause ping failures even when other services remain accessible.

### Key Definitions

**ICMP:** Internet Control Message Protocol, used for network diagnostics and error reporting.

**Ping:** A utility for testing IP reachability using ICMP Echo messages.

**Packet Loss:** The percentage of transmitted packets for which no corresponding response was received.

**Latency:** The time delay associated with communication over a network.

**RTT:** Round-Trip Time, the time taken for a request to travel to a destination and its response to return.

**TTL:** Time To Live, an IPv4 packet-header field that limits how many router hops a packet can traverse.

**Traceroute:** A utility that attempts to identify intermediate routers along a network path.

---

## 18. Knowledge Check

1. Which protocol does ping use?
2. What does `-c` control?
3. How do you test your local IPv4 loopback?
4. What is the formula for calculating packet loss?
5. What does high RTT indicate?
6. Why might a destination fail to respond to ping?
7. Which command checks DNS resolution?
8. What is the purpose of traceroute?

**End of Lesson 07 – Working with ping and Connectivity Testing**   