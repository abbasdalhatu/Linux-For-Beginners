
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 06: NETWORK CONNECTIVITY TESTING

### 1. Lab Overview

Network connectivity testing is the process of checking whether devices, services, and networks can communicate successfully.

In system administration and cybersecurity, connectivity testing helps identify:

- Network failures.
- Unreachable gateways.
- DNS problems.
- Packet loss.
- High latency.
- Service availability issues.
- Firewall filtering.
- Routing problems.

In this lab, you will use Linux diagnostic tools to test connectivity, examine packet loss, measure response times, and investigate network failures.

**Lab environment:** Kali Linux VM.

**Safety:** Use your own VM, local gateway, and ordinary public diagnostic destinations. Do not scan networks or test services without authorization.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain network connectivity testing.
2. Use `ping` to test reachability.
3. Understand latency and packet loss.
4. Use `traceroute` to examine network paths.
5. Use `tracepath` as an alternative.
6. Understand the purpose of `curl`.
7. Test DNS and IP connectivity separately.
8. Inspect network errors.
9. Troubleshoot common connectivity problems.
10. Generate a network connectivity report.

### 3. What Is Network Connectivity?

Network connectivity describes the ability of two or more devices to exchange data.

For example:

```text
Kali Linux VM
     |
     v
VMware Network
     |
     v
Gateway
     |
     v
Internet
```

If any required connection fails, communication may be interrupted.

Connectivity can be tested at different levels:

| Level | Example |
|---|---|
| Local system | Loopback test |
| Local network | Gateway test |
| IP connectivity | Test an IP destination |
| DNS | Resolve a domain |
| Application | Connect to a website |
| Network path | Trace intermediate hops |

### 4. Understand ICMP

ICMP stands for Internet Control Message Protocol.

It is used by network devices to exchange diagnostic and control information.

Common ICMP messages include:

- Echo Request.
- Echo Reply.
- Destination Unreachable.
- Time Exceeded.
- Redirect.

The `ping` utility uses ICMP Echo Requests and Echo Replies to test reachability.

ICMP is useful for troubleshooting, but it can also be filtered by firewalls.

### 5. Test the Loopback Interface

The loopback address refers to the local system.

IPv4 loopback:

```text
127.0.0.1
```

IPv6 loopback:

```text
::1
```

Run:

```bash
ping -c 4 127.0.0.1
```

Then:

```bash
ping -6 -c 4 ::1
```

Expected behavior:

- Four requests are sent.
- The system normally responds to each request.
- Packet loss should normally be zero.

If loopback fails, investigate the local networking stack before troubleshooting external connectivity.

### 6. Test Your Default Gateway

Display the default route:

```bash
ip route show default
```

Identify the gateway address.

Test it:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Replace the placeholder with your actual gateway address.

Example:

```bash
ping -c 4 192.168.100.1
```

Interpretation:

- Successful replies indicate that the gateway responded.
- Packet loss may indicate connectivity problems.
- No replies may mean ICMP is filtered.

A failed ping alone does not prove that the gateway is offline.

### 7. Test External IP Connectivity

If your VM has internet access, test a known public IP address:

```bash
ping -c 4 1.1.1.1
```

You can also test:

```bash
ping -c 4 8.8.8.8
```

These are public DNS service IP addresses.

You are using them only as ordinary connectivity destinations, not scanning their networks.

Record:

- Number of packets transmitted.
- Number of packets received.
- Packet loss percentage.
- Average response time.

If ICMP is blocked, use the application-level test in Section 12 instead.

### 8. Understand Latency

Latency is the time required for data to travel between two network endpoints.

It is commonly measured in milliseconds (ms).

Example:

```text
time=25.4 ms
```

This means the measured round-trip time was approximately 25.4 milliseconds.

General interpretation:

| Latency | General Interpretation |
|---|---|
| Very low | Fast response |
| Moderate | Normal in many networks |
| High | Possible delay or congestion |
| Extremely high | Possible connectivity problem |

Latency depends on physical distance, network congestion, routing, and other conditions.

### 9. Understand Packet Loss

Packet loss occurs when packets sent across a network fail to reach their intended destination or their responses do not return.

Example:

```text
4 packets transmitted
3 packets received
25% packet loss
```

Possible causes include:

- Network congestion.
- Unstable wireless connections.
- Faulty network equipment.
- Firewall filtering.
- Routing problems.
- Destination-side limitations.

Packet loss should be investigated using multiple observations rather than one test.

### 10. Use Ping Statistics

Run:

```bash
ping -c 10 1.1.1.1
```

At the end, inspect the statistics.

Example:

```text
10 packets transmitted, 10 received, 0% packet loss
rtt min/avg/max/mdev = 20/25/30/3 ms
```

The values are illustrative.

| Statistic | Meaning |
|---|---|
| min | Minimum round-trip time |
| avg | Average round-trip time |
| max | Maximum round-trip time |
| mdev | Mean deviation of round-trip times |

These statistics help identify network instability.

### 11. Understand Traceroute

Traceroute identifies intermediate routing hops between your system and a destination.

Check availability:

```bash
command -v traceroute
```

If installed:

```bash
traceroute 1.1.1.1
```

Alternative:

```bash
command -v tracepath
```

If installed:

```bash
tracepath 1.1.1.1
```

Each hop represents an intermediate point in the path.

Important:

- Some routers do not respond to diagnostic probes.
- Asterisks do not automatically mean the route is broken.
- Different protocols can produce different traceroute results.
- Routing paths can change over time.

### 12. Test Application-Level Connectivity with curl

Sometimes ping fails because ICMP is blocked, but the website or application is still reachable.

`curl` can test HTTP and HTTPS connectivity.

Check whether it is installed:

```bash
command -v curl
```

If available, run:

```bash
curl -I --max-time 10 https://example.com
```

The `-I` option requests response headers.

The `--max-time 10` option limits the operation to ten seconds.

Possible response:

```text
HTTP/2 200
```

This indicates a successful HTTP response.

Other responses such as `301`, `403`, or `404` also demonstrate that an HTTP server responded, although the application outcome differs.

If the request times out, investigate DNS, routing, firewall rules, and application availability.

### 13. Separate DNS Testing from IP Testing

A useful troubleshooting approach is to compare domain resolution with IP connectivity.

Test DNS:

```bash
getent hosts example.com
```

Test IP connectivity:

```bash
ping -c 4 1.1.1.1
```

Test application connectivity:

```bash
curl -I --max-time 10 https://example.com
```

Interpretation:

| DNS | IP connectivity | Possible Explanation |
|---|---|---|
| Works | Works | Basic connectivity is likely functional |
| Fails | Works | Possible DNS problem |
| Works | Fails | Possible routing, filtering, or destination issue |
| Fails | Fails | Possible broader network problem |

These are diagnostic clues, not absolute conclusions.

### 14. Inspect Network Interface Statistics

Run:

```bash
ip -s link
```

You will see statistics for each network interface.

Look for:

- RX packets.
- TX packets.
- RX errors.
- TX errors.
- Dropped packets.

RX means received.

TX means transmitted.

An increasing error or drop count may indicate an interface or network problem.

### 15. Inspect Network Connectivity with ss

The `ss` utility displays socket information.

Run:

```bash
ss -s
```

This displays a summary of socket statistics.

Inspect listening TCP and UDP sockets:

```bash
ss -tuln
```

Inspect established TCP connections:

```bash
ss -tn state established
```

These commands help you understand local network communication.

### 16. Common Connectivity Problems

| Symptom | Possible Cause |
|---|---|
| Loopback fails | Local networking stack issue |
| Gateway unreachable | Local network or VM adapter problem |
| External IP unreachable | Routing or upstream connectivity problem |
| Domain fails but IP works | DNS problem |
| Ping fails but curl works | ICMP filtering is possible |
| High latency | Congestion or routing delay |
| Packet loss | Network instability or filtering |
| Website unavailable | DNS, routing, TLS, or application issue |

### 17. Practical Exercise 1 – Loopback Testing

Run:

```bash
ping -c 4 127.0.0.1
```

Run:

```bash
ping -6 -c 4 ::1
```

Record the results.

### 18. Practical Exercise 2 – Gateway Testing

Run:

```bash
ip route show default
```

Identify your gateway.

Then:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Record the result.

If you have no default gateway, document that observation and continue.

### 19. Practical Exercise 3 – External Connectivity

If internet access is available:

```bash
ping -c 4 1.1.1.1
```

Record:

- Transmitted packets.
- Received packets.
- Packet loss.
- Average latency.

If ICMP is filtered, document the result and continue.

### 20. Practical Exercise 4 – DNS and Application Testing

Run:

```bash
getent hosts example.com
```

Then, if `curl` is installed:

```bash
curl -I --max-time 10 https://example.com
```

Compare the results.

### 21. Practical Exercise 5 – Examine the Network Path

If traceroute or tracepath is installed, run:

```bash
traceroute 1.1.1.1
```

Alternatively:

```bash
tracepath 1.1.1.1
```

Observe the number of visible hops and any non-responsive hops.

### 22. Practical Exercise 6 – Inspect Interface Statistics

Run:

```bash
ip -s link
```

Record the RX and TX packet counts.

Look for errors or dropped packets.

Then run:

```bash
ss -s
```

Record the socket summary.

### 23. Generate a Connectivity Diagnostic Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab06
```

Create the script:

```bash
nano ~/practical-labs/module05/lab06/connectivity-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab06/connectivity-report.txt"

{
    echo "NETWORK CONNECTIVITY DIAGNOSTIC REPORT"
    echo "======================================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "NETWORK INTERFACES"
    ip -br addr
    echo

    echo "ROUTING TABLE"
    ip route
    echo

    echo "LOOPBACK TEST"
    ping -c 4 127.0.0.1
    echo

    echo "EXTERNAL IP CONNECTIVITY TEST"
    ping -c 4 -W 3 1.1.1.1
    echo

    echo "DNS RESOLUTION TEST"
    getent hosts example.com
    echo

    echo "APPLICATION CONNECTIVITY TEST"
    if command -v curl >/dev/null 2>&1; then
        curl -I --max-time 10 https://example.com
    else
        echo "curl is not installed"
    fi
    echo

    echo "INTERFACE STATISTICS"
    ip -s link
    echo

    echo "SOCKET SUMMARY"
    ss -s

} > "$REPORT" 2>&1

echo "Connectivity report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab06/connectivity-report.sh
```

Execute:

```bash
~/practical-labs/module05/lab06/connectivity-report.sh
```

Inspect:

```bash
cat ~/practical-labs/module05/lab06/connectivity-report.txt
```

### 24. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab06/lab06-report.txt
```

Record:

```text
MODULE 05 – LAB 06
NETWORK CONNECTIVITY TESTING

1. What is network connectivity?
2. What does ICMP stand for?
3. Result of IPv4 loopback test:
4. Result of IPv6 loopback test:
5. My default gateway:
6. Result of gateway test:
7. Result of external IP test:
8. Average latency:
9. Packet loss:
10. Result of DNS resolution:
11. Result of curl test:
12. Number of visible traceroute hops, if tested:
13. Any interface errors or dropped packets:
14. What is the purpose of ss?
15. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 25. Knowledge Check

1. What is network connectivity?
2. What does ICMP stand for?
3. What is the purpose of ping?
4. What is latency?
5. What is packet loss?
6. What does traceroute do?
7. Why can ping fail while a website remains accessible?
8. What is the purpose of curl?
9. What is the difference between DNS testing and IP connectivity testing?
10. What do RX and TX mean?
11. What does `ip -s link` display?
12. What does `ss -s` display?
13. What can cause high latency?
14. Why is connectivity testing important in cybersecurity?

### 26. Lab Completion Checklist

- [ ] Understood network connectivity.
- [ ] Learned basic ICMP concepts.
- [ ] Tested IPv4 loopback.
- [ ] Tested IPv6 loopback.
- [ ] Identified and tested the default gateway.
- [ ] Tested external IP connectivity, if available.
- [ ] Examined latency.
- [ ] Examined packet loss.
- [ ] Studied traceroute and tracepath.
- [ ] Tested application connectivity using curl, if available.
- [ ] Compared DNS and IP connectivity.
- [ ] Inspected interface statistics.
- [ ] Inspected socket statistics.
- [ ] Generated the connectivity diagnostic report.
- [ ] Completed lab documentation.

**LAB 06 STATUS: READY FOR PRACTICAL COMPLETION**
