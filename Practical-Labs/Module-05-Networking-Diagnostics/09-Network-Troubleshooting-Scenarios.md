
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 09: NETWORK TROUBLESHOOTING SCENARIOS

### 1. Lab Overview

Network troubleshooting is the systematic process of identifying, investigating, and resolving network problems.

In real-world IT environments, network administrators and cybersecurity professionals regularly encounter problems such as:

- No internet connection.
- DNS resolution failures.
- Incorrect IP configuration.
- Unreachable gateways.
- High latency.
- Packet loss.
- Unexpected listening ports.
- Application connection failures.
- Network interface errors.

In this lab, you will investigate realistic network problems using Linux diagnostic tools.

You will learn to identify symptoms, collect evidence, determine possible causes, and recommend appropriate solutions.

**Lab environment:** Kali Linux VM.

**Safety:** These scenarios are simulated through diagnostic exercises and observations. Do not intentionally disrupt your network, modify routing configurations, or attack external systems.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Follow a structured troubleshooting methodology.
2. Diagnose IP configuration problems.
3. Investigate gateway connectivity failures.
4. Diagnose DNS problems.
5. Investigate packet loss and latency.
6. Identify listening services.
7. Investigate application connectivity problems.
8. Interpret network diagnostic evidence.
9. Recommend appropriate corrective actions.
10. Produce a professional troubleshooting report.

### 3. The Network Troubleshooting Methodology

Professional troubleshooting should follow a logical process.

Use this six-step approach:

```text
Identify the problem
        |
        v
Collect information
        |
        v
Develop possible causes
        |
        v
Test the possible causes
        |
        v
Recommend or apply a solution
        |
        v
Verify and document results
```

Do not randomly change configurations.

Always collect evidence before making changes.

### 4. Important Diagnostic Commands

| Command | Purpose |
|---|---|
| `ip -br addr` | Inspect IP configuration |
| `ip route` | Inspect routing table |
| `ip route get` | Perform a routing lookup |
| `ping` | Test IP reachability |
| `getent hosts` | Test system name resolution |
| `dig` | Perform DNS queries |
| `curl` | Test application connectivity |
| `ss -tuln` | Inspect listening sockets |
| `ss -tn` | Inspect TCP connections |
| `ip -s link` | Inspect interface statistics |
| `traceroute` | Examine network paths |
| `journalctl` | Inspect system logs |

### 5. Scenario 1 – No Internet Connectivity

**Situation:**

A user reports:

"My Kali Linux VM cannot access the internet."

Your task is to investigate the possible cause.

#### Step 1: Inspect the Network Interface

Run:

```bash
ip -br addr
```

Check:

- Is the interface present?
- Is it UP?
- Does it have an IP address?

#### Step 2: Inspect the Routing Table

Run:

```bash
ip route
```

Look for a default route.

Example:

```text
default via 192.168.100.1 dev eth0
```

If there is no default route, record that observation.

#### Step 3: Test Loopback

Run:

```bash
ping -c 4 127.0.0.1
```

This checks local networking functionality.

#### Step 4: Test the Gateway

Identify the gateway:

```bash
ip route show default
```

Then:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

#### Step 5: Test External IP Connectivity

If internet connectivity is expected:

```bash
ping -c 4 1.1.1.1
```

#### Step 6: Test DNS

Run:

```bash
getent hosts example.com
```

#### Possible Causes

- VMware network adapter disconnected.
- Missing IP address.
- Missing default route.
- Gateway unreachable.
- DNS configuration problem.
- Upstream network failure.

#### Your Task

Write down the results of each diagnostic step.

Do not change VMware network settings unless you have identified a likely cause and understand the proposed correction.

### 6. Scenario 2 – Domain Names Cannot Be Resolved

**Situation:**

A user reports:

"I can reach an IP address, but websites do not open using their domain names."

This suggests a possible DNS problem.

#### Step 1: Test IP Connectivity

Run:

```bash
ping -c 4 1.1.1.1
```

#### Step 2: Test Domain Resolution

Run:

```bash
getent hosts example.com
```

#### Step 3: Inspect DNS Configuration

Run:

```bash
cat /etc/resolv.conf
```

#### Step 4: Inspect systemd-resolved

If available:

```bash
resolvectl status
```

#### Step 5: Query DNS Directly

If `dig` is installed:

```bash
dig example.com
```

#### Possible Causes

- Incorrect DNS server configuration.
- DNS server unavailable.
- Network filtering.
- Resolver configuration problem.
- DNS-specific connectivity failure.

#### Your Task

Explain why successful IP connectivity combined with failed domain resolution points toward DNS as a possible problem.

### 7. Scenario 3 – Default Gateway Is Unreachable

**Situation:**

A Linux system has an IP address but cannot communicate with its configured gateway.

#### Step 1: Inspect the Interface

```bash
ip -br link
```

#### Step 2: Inspect the IP Address

```bash
ip -br addr
```

#### Step 3: Inspect the Default Route

```bash
ip route show default
```

#### Step 4: Test the Gateway

```bash
ping -c 4 YOUR_GATEWAY_IP
```

#### Step 5: Inspect Neighbor Information

Run:

```bash
ip neigh
```

This displays the system's neighbor table, which is commonly used for IPv4 address-to-MAC resolution.

Possible neighbor states include:

| State | Meaning |
|---|---|
| REACHABLE | Neighbor recently confirmed reachable |
| STALE | Previously confirmed, but confirmation is old |
| DELAY | Waiting before probing |
| PROBE | Actively probing |
| FAILED | Neighbor resolution failed |
| INCOMPLETE | Resolution is still in progress |

#### Possible Causes

- Incorrect gateway address.
- Virtual network adapter problem.
- Gateway unavailable.
- Local network configuration problem.
- Neighbor discovery or filtering issue.

#### Your Task

Record the gateway address, interface, and neighbor state if available.

Do not delete neighbor entries or modify routes.

### 8. Scenario 4 – High Latency

**Situation:**

A user complains that network applications respond slowly.

#### Step 1: Test Local Connectivity

```bash
ping -c 4 127.0.0.1
```

#### Step 2: Test Gateway Latency

```bash
ping -c 10 YOUR_GATEWAY_IP
```

#### Step 3: Test External Latency

```bash
ping -c 10 1.1.1.1
```

#### Step 4: Inspect Interface Statistics

```bash
ip -s link
```

#### Step 5: Examine the Network Path

If available:

```bash
traceroute 1.1.1.1
```

#### Possible Causes

- Network congestion.
- Wireless interference.
- Routing delays.
- High server load.
- Packet loss.
- Virtual machine resource limitations.

#### Your Task

Compare local, gateway, and external response times.

Explain which part of the network path may require further investigation.

### 9. Scenario 5 – Packet Loss

**Situation:**

A user reports that network communication is unreliable.

#### Step 1: Test the Gateway

```bash
ping -c 20 YOUR_GATEWAY_IP
```

#### Step 2: Test External Connectivity

```bash
ping -c 20 1.1.1.1
```

#### Step 3: Inspect Interface Statistics

```bash
ip -s link
```

#### Step 4: Compare Results

Consider:

- Does packet loss occur locally?
- Does it occur when testing the gateway?
- Does it occur only when testing an external destination?
- Are interface errors increasing?

#### Possible Causes

- Congestion.
- Interface errors.
- Unstable virtual networking.
- Wireless interference.
- Filtering or rate limiting.

Remember that ICMP packet loss can be caused by filtering or rate limiting, so it does not always indicate application traffic loss.

### 10. Scenario 6 – Unexpected Listening Port

**Situation:**

A security administrator notices a listening port that requires investigation.

#### Step 1: List Listening Ports

```bash
sudo ss -tulpn
```

#### Step 2: Identify the Process

Inspect the process name and PID shown by the command.

If `lsof` is available:

```bash
sudo lsof -i -n -P
```

#### Step 3: Inspect the Process

For a process ID you have identified:

```bash
ps -p YOUR_PID -f
```

Replace the placeholder with the actual PID.

#### Step 4: Check the Service

If you know the associated systemd service:

```bash
systemctl status SERVICE_NAME
```

Replace `SERVICE_NAME` with the actual service name.

#### Step 5: Document the Finding

Record:

- Port number.
- Transport protocol.
- Listening address.
- Process name.
- PID.
- Whether the service is expected.
- Whether further investigation is required.

Do not terminate processes simply because a port is unfamiliar.

### 11. Scenario 7 – Website Cannot Be Reached

**Situation:**

A user reports that a website does not load.

#### Step 1: Test DNS

```bash
getent hosts example.com
```

#### Step 2: Test IP Connectivity

```bash
ping -c 4 1.1.1.1
```

#### Step 3: Test HTTPS

If `curl` is available:

```bash
curl -I --max-time 10 https://example.com
```

#### Step 4: Inspect the Routing Table

```bash
ip route
```

#### Step 5: Check the System Time

```bash
date
```

An incorrect system clock can cause certificate validation problems.

#### Possible Causes

- DNS resolution failure.
- Routing failure.
- Firewall filtering.
- TLS certificate problem.
- Website server issue.
- Incorrect system time.
- Application-level error.

#### Your Task

Explain why a successful DNS lookup does not guarantee that a website will load successfully.

### 12. Scenario 8 – Network Interface Errors

**Situation:**

A network administrator observes errors and dropped packets on an interface.

#### Step 1: Inspect Interface Statistics

```bash
ip -s link
```

#### Step 2: Inspect Interface State

```bash
ip link
```

#### Step 3: Inspect Kernel Messages

```bash
journalctl -k -n 50 --no-pager
```

This displays recent kernel messages.

#### Step 4: Review the Evidence

Look for:

- Interface state changes.
- Link failures.
- Driver messages.
- Network-related errors.
- Dropped packets.

Possible causes include:

- Virtual network adapter problems.
- Driver problems.
- Network congestion.
- Physical network issues on a physical system.
- Interface configuration problems.

Do not assume that every dropped packet is a hardware failure.

### 13. Understand a Professional Troubleshooting Report

A good troubleshooting report should contain:

1. Problem description.
2. System information.
3. Diagnostic commands used.
4. Observed results.
5. Possible causes.
6. Evidence supporting each finding.
7. Recommended corrective action.
8. Verification steps.
9. Final status.

Avoid writing conclusions without supporting evidence.

### 14. Practical Exercise 1 – Diagnose Your Current Network

Run:

```bash
ip -br addr
ip route
ip neigh
```

Record your interface, IP address, default route, and neighbor information.

### 15. Practical Exercise 2 – Test Connectivity

Run:

```bash
ping -c 4 127.0.0.1
```

If you have a gateway:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

If internet access is available:

```bash
ping -c 4 1.1.1.1
```

Record the results.

### 16. Practical Exercise 3 – Diagnose DNS

Run:

```bash
cat /etc/resolv.conf
getent hosts example.com
```

If available:

```bash
dig example.com
```

Write a short explanation of the results.

### 17. Practical Exercise 4 – Inspect Listening Services

Run:

```bash
sudo ss -tulpn
```

Identify any listening services.

Record their addresses, protocols, ports, and process information where available.

### 18. Practical Exercise 5 – Examine Interface Health

Run:

```bash
ip -s link
```

Then:

```bash
journalctl -k -n 30 --no-pager
```

Record any relevant network-related errors.

### 19. Practical Exercise 6 – Build a Troubleshooting Decision Tree

Create a file:

```bash
nano ~/practical-labs/module05/lab09/troubleshooting-tree.txt
```

Record:

```text
NETWORK TROUBLESHOOTING DECISION TREE

START
  |
  v
Does the interface exist and appear UP?
  |
  +-- NO --> Investigate interface or VM adapter
  |
  +-- YES
       |
       v
Does the interface have an IP address?
       |
       +-- NO --> Investigate IP configuration
       |
       +-- YES
            |
            v
Is a default route configured?
            |
            +-- NO --> Investigate gateway configuration
            |
            +-- YES
                 |
                 v
Can the gateway be reached?
                 |
                 +-- NO --> Investigate local network
                 |
                 +-- YES
                      |
                      v
Can an external IP be reached?
                      |
                      +-- NO --> Investigate upstream routing
                      |
                      +-- YES
                           |
                           v
Can a domain name be resolved?
                           |
                           +-- NO --> Investigate DNS
                           |
                           +-- YES
                                |
                                v
Can the application connect?
                                |
                                +-- NO --> Investigate service,
                                |          firewall, TLS or application
                                |
                                +-- YES --> Basic connectivity works
```

This is a troubleshooting guide, not an automatic diagnosis tool.

### 20. Generate a Troubleshooting Evidence Report

Create the lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab09
```

Create:

```bash
nano ~/practical-labs/module05/lab09/troubleshooting-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab09/troubleshooting-report.txt"

{
    echo "NETWORK TROUBLESHOOTING EVIDENCE REPORT"
    echo "========================================"
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

    echo "SOCKET SUMMARY"
    ss -s
    echo

    echo "RECENT KERNEL MESSAGES"
    journalctl -k -n 30 --no-pager

} > "$REPORT" 2>&1

echo "Troubleshooting report generated: $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab09/troubleshooting-report.sh
```

Run:

```bash
sudo ~/practical-labs/module05/lab09/troubleshooting-report.sh
```

Inspect:

```bash
cat ~/practical-labs/module05/lab09/troubleshooting-report.txt
```

### 21. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab09/lab09-report.txt
```

Record:

```text
MODULE 05 – LAB 09
NETWORK TROUBLESHOOTING SCENARIOS

1. What is network troubleshooting?
2. What are the six troubleshooting steps?
3. What does ip -br addr show?
4. What does ip route show?
5. How do you investigate a DNS failure?
6. How do you investigate an unreachable gateway?
7. How do you investigate high latency?
8. What can cause packet loss?
9. How do you identify listening services?
10. How do you investigate interface errors?
11. Why is evidence important?
12. What is a troubleshooting decision tree?
13. Which diagnostic commands did I use?
14. What problems did I identify?
15. What corrective actions would I recommend?
16. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 22. Knowledge Check

1. What is network troubleshooting?
2. Why should you collect evidence before changing configurations?
3. What command displays network interface addresses?
4. What command displays the routing table?
5. How can you investigate DNS failures?
6. What can cause an unreachable gateway?
7. What is latency?
8. What is packet loss?
9. How can you identify listening ports?
10. Why should unfamiliar processes not be terminated immediately?
11. What is the purpose of `journalctl -k`?
12. What is a troubleshooting decision tree?
13. What should a professional troubleshooting report contain?
14. Why is verification important after a correction?
15. How does network troubleshooting support cybersecurity?

### 23. Lab Completion Checklist

- [ ] Understood the troubleshooting methodology.
- [ ] Reviewed important diagnostic commands.
- [ ] Investigated internet connectivity.
- [ ] Investigated DNS failures.
- [ ] Investigated gateway problems.
- [ ] Studied latency and packet loss.
- [ ] Investigated unexpected listening ports.
- [ ] Studied website connectivity failures.
- [ ] Examined interface errors.
- [ ] Created a troubleshooting decision tree.
- [ ] Generated the troubleshooting evidence report.
- [ ] Completed lab documentation.

**LAB 09 STATUS: READY FOR PRACTICAL COMPLETION**
