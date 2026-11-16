
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 04: ROUTING AND GATEWAY DIAGNOSTICS

### 1. Lab Overview

Routing is the process of selecting a path for network packets to travel from one network to another.

A router forwards packets between different networks using routing information.

In Linux, routing information determines how your system sends traffic toward local devices, remote networks, and the internet.

For example, when your Kali VM accesses a website, it may send traffic through its default gateway.

Understanding routing is essential for:

- Network administration.
- Troubleshooting connectivity.
- Network segmentation.
- Firewall configuration.
- Cybersecurity investigations.
- Understanding how packets travel across networks.

In this lab, you will inspect routing tables, identify gateways, examine routes, and troubleshoot routing problems safely.

**Lab environment:** Kali Linux VM.

**Safety:** This lab focuses on inspecting your existing routing configuration. Do not modify routes on your Windows host or any network you do not administer.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain network routing.
2. Understand the role of routers.
3. Explain default gateways.
4. Inspect Linux routing tables.
5. Understand static and dynamic routing.
6. Understand default and connected routes.
7. Inspect IPv4 and IPv6 routes.
8. Use route diagnostic commands.
9. Understand common routing problems.
10. Generate a routing diagnostic report.

### 3. What Is Routing?

Routing is the process of determining where packets should be forwarded.

Imagine that your computer wants to communicate with a server on another network.

Your computer checks its routing table to determine where the packet should go.

A simplified example:

```text
Kali Linux VM
      |
      v
VMware Virtual Network
      |
      v
Default Gateway
      |
      v
Router
      |
      v
Destination Network
```

The routing table contains information that helps the system select an appropriate path.

### 4. What Is a Router?

A router is a networking device that connects different IP networks.

Its responsibilities include:

- Forwarding packets.
- Selecting routes.
- Connecting networks.
- Applying routing policies.
- Supporting network segmentation.

A router can connect:

- Home networks to the internet.
- Office networks to remote branches.
- Virtual networks to external networks.
- Different organizational network segments.

### 5. What Is a Default Gateway?

A default gateway is the next-hop device used when a destination does not match a more specific route in the routing table.

Example:

```text
Kali IP:       192.168.100.20
Gateway:       192.168.100.1
Destination:   8.8.8.8
```

If the routing table has no more specific route for the destination, the system may send the packet to:

```text
192.168.100.1
```

The gateway then handles forwarding toward the destination.

Important: The gateway is usually a router, but the exact network architecture can vary.

### 6. Inspect the Linux Routing Table

Run:

```bash
ip route
```

You may see output similar to:

```text
default via 192.168.100.1 dev eth0
192.168.100.0/24 dev eth0 proto kernel scope link
```

Understand the fields:

| Field | Meaning |
|---|---|
| `default` | Default route |
| `via` | Next-hop gateway |
| `dev` | Interface used |
| `proto kernel` | Route created by the kernel |
| `scope link` | Route applies directly to the connected link |

Your actual output will depend on your VMware configuration.

### 7. Understand the Default Route

A default route is commonly represented as:

```text
default via 192.168.100.1 dev eth0
```

It can also be represented using:

```text
0.0.0.0/0
```

The `/0` prefix means that the route matches any IPv4 destination not matched by a more specific route.

Inspect the default route:

```bash
ip route show default
```

To inspect the IPv6 default route:

```bash
ip -6 route show default
```

If no default route appears, your system may not have a configured route for general external destinations.

### 8. Understand Connected Routes

A connected route identifies a network directly reachable through an interface.

Example:

```text
192.168.100.0/24 dev eth0
```

This means the system has a route to the `192.168.100.0/24` network through `eth0`.

The system can communicate with destinations on that connected network without sending packets to its default gateway.

Inspect connected routes:

```bash
ip route show
```

Look for entries associated with your active network interface.

### 9. Inspect Routing Information in Detail

Run:

```bash
ip -details route
```

Inspect a specific network:

```bash
ip route get 192.168.100.1
```

Replace the address with your actual gateway.

Inspect a remote destination:

```bash
ip route get 8.8.8.8
```

This command helps identify:

- The selected route.
- The outgoing interface.
- The source IP address.
- The next-hop gateway, if applicable.

It does not send a packet to the destination; it performs a local routing lookup.

### 10. Understand Static Routing

A static route is manually configured by an administrator.

Example:

```text
Destination: 10.20.0.0/16
Gateway:     192.168.100.1
```

A static route tells the system where to send traffic destined for that network.

Static routes are commonly used in:

- Small office networks.
- Laboratory environments.
- Fixed network topologies.
- Specific routing requirements.

Advantages:

- Predictable.
- Simple in small networks.
- Does not require a routing protocol.

Disadvantages:

- Requires manual maintenance.
- Can become difficult to manage in large networks.
- May not automatically adapt to network failures.

We will inspect routing concepts without adding routes to your actual VM.

### 11. Understand Dynamic Routing

Dynamic routing allows routers to learn and exchange routing information through routing protocols.

Examples include:

| Protocol | Purpose |
|---|---|
| RIP | Distance-vector routing |
| OSPF | Link-state routing |
| BGP | Inter-domain routing |
| IS-IS | Link-state routing |

Dynamic routing is useful in larger networks where routes may change.

You do not need to configure these protocols in this lab.

### 12. Inspect IPv4 Routing

Run:

```bash
ip -4 route
```

Display only the default IPv4 route:

```bash
ip -4 route show default
```

Inspect a destination:

```bash
ip -4 route get 8.8.8.8
```

Inspect your local gateway using its actual IP address:

```bash
ip -4 route get YOUR_GATEWAY_IP
```

Replace `YOUR_GATEWAY_IP` with the real gateway address.

### 13. Inspect IPv6 Routing

IPv6 has its own routing table.

Run:

```bash
ip -6 route
```

Display the IPv6 default route:

```bash
ip -6 route show default
```

Inspect the IPv6 loopback route:

```bash
ip -6 route get ::1
```

IPv6 uses different address formats and routing rules from IPv4.

### 14. Understand the Difference Between Gateway and DNS

These two concepts are often confused.

**Gateway**

A gateway helps forward traffic toward other networks.

**DNS server**

A DNS server resolves domain names into IP addresses.

Example:

You open:

```text
example.com
```

DNS helps resolve the domain name.

The routing table determines how your system sends packets toward the resulting IP address.

A working gateway does not automatically guarantee that DNS works.

Similarly, working DNS does not guarantee that the gateway is reachable.

### 15. Test Gateway Connectivity

The `ping` command uses ICMP Echo Requests and Echo Replies.

Identify your gateway:

```bash
ip route show default
```

Then test it:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Replace the placeholder with your actual gateway address.

Example:

```bash
ping -c 4 192.168.100.1
```

Interpret the output:

- Replies indicate that the destination responded.
- Packet loss may indicate connectivity problems or filtering.
- No replies do not conclusively prove the gateway is down because ICMP may be filtered.

### 16. Test the Loopback Address

Run:

```bash
ping -c 4 127.0.0.1
```

Test IPv6 loopback:

```bash
ping -6 -c 4 ::1
```

Loopback tests help confirm local IP stack functionality.

They do not test external network connectivity.

### 17. Understand traceroute

Traceroute helps identify intermediate routing hops between your computer and a destination.

Check whether it is installed:

```bash
command -v traceroute
```

If available, you can inspect a destination:

```bash
traceroute 8.8.8.8
```

Alternatively, check whether `tracepath` is available:

```bash
command -v tracepath
```

If available:

```bash
tracepath 8.8.8.8
```

Some routers do not respond to traceroute probes. Asterisks do not necessarily indicate a broken route.

Use only an approved destination.

### 18. Common Routing Problems

| Problem | Possible Cause |
|---|---|
| No default route | Missing gateway configuration |
| Gateway unreachable | Virtual adapter or local network problem |
| Wrong outgoing interface | Incorrect routing configuration |
| Destination unreachable | No suitable route or network rejection |
| High packet loss | Congestion or connectivity issue |
| Internet unavailable but gateway responds | Upstream routing, firewall, or DNS issue |
| IPv4 works but IPv6 fails | IPv6 configuration or routing issue |

A reliable troubleshooting process investigates one layer at a time.

### 19. Practical Exercise 1 – Inspect the Routing Table

Run:

```bash
ip route
```

Record:

- Default route.
- Gateway.
- Network prefix.
- Interface name.

### 20. Practical Exercise 2 – Inspect the Default Gateway

Run:

```bash
ip route show default
```

Identify the next-hop gateway.

If no default route exists, record that observation.

### 21. Practical Exercise 3 – Perform a Routing Lookup

Run:

```bash
ip route get 8.8.8.8
```

Record:

- Destination.
- Source address.
- Outgoing interface.
- Next-hop gateway, if present.

### 22. Practical Exercise 4 – Inspect IPv6 Routes

Run:

```bash
ip -6 route
```

Identify:

- IPv6 connected routes.
- Default IPv6 route, if present.
- Loopback route.

### 23. Practical Exercise 5 – Test Local Connectivity

Run:

```bash
ping -c 4 127.0.0.1
```

If you have a gateway:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Record the results and any packet loss.

### 24. Practical Exercise 6 – Trace an Approved Destination

If traceroute or tracepath is available, use:

```bash
traceroute 8.8.8.8
```

or:

```bash
tracepath 8.8.8.8
```

Observe the route and identify any non-responsive hops.

If these tools are unavailable, skip this exercise and record that limitation.

### 25. Generate a Routing Diagnostic Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab04
```

Create the script:

```bash
nano ~/practical-labs/module05/lab04/routing-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab04/routing-report.txt"

{
    echo "LINUX ROUTING DIAGNOSTIC REPORT"
    echo "==============================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "IPV4 ROUTING TABLE"
    ip -4 route
    echo

    echo "IPV4 DEFAULT ROUTE"
    ip -4 route show default
    echo

    echo "IPV6 ROUTING TABLE"
    ip -6 route
    echo

    echo "INTERFACE SUMMARY"
    ip -br addr
    echo

    echo "ROUTE TO 8.8.8.8"
    ip route get 8.8.8.8
    echo

    echo "LOCAL LOOPBACK TEST"
    ping -c 4 127.0.0.1

} > "$REPORT" 2>&1

echo "Routing report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab04/routing-report.sh
```

Run it:

```bash
~/practical-labs/module05/lab04/routing-report.sh
```

Inspect the report:

```bash
cat ~/practical-labs/module05/lab04/routing-report.txt
```

### 26. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab04/lab04-report.txt
```

Record:

```text
MODULE 05 – LAB 04
ROUTING AND GATEWAY DIAGNOSTICS

1. Default gateway:
2. Default route:
3. Connected network:
4. Main network interface:
5. What is routing?
6. What is a router?
7. What is a default gateway?
8. What is a connected route?
9. What is the difference between static and dynamic routing?
10. What does ip route get do?
11. What is the purpose of traceroute?
12. Result of gateway connectivity test:
13. Result of loopback test:
14. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 27. Knowledge Check

1. What is routing?
2. What is the purpose of a router?
3. What is a default gateway?
4. What is a default route?
5. What is a connected route?
6. What does `ip route` display?
7. What is the difference between static and dynamic routing?
8. What does `ip route get` do?
9. What is the purpose of traceroute?
10. What is the difference between a gateway and a DNS server?
11. Why might ping fail even when a destination is available?
12. Why is routing knowledge important in cybersecurity?

### 28. Lab Completion Checklist

- [ ] Understood routing fundamentals.
- [ ] Learned the role of routers.
- [ ] Understood default gateways.
- [ ] Inspected the IPv4 routing table.
- [ ] Inspected the default route.
- [ ] Inspected connected routes.
- [ ] Performed routing lookups.
- [ ] Inspected IPv6 routing.
- [ ] Tested loopback connectivity.
- [ ] Tested gateway connectivity, if available.
- [ ] Studied traceroute.
- [ ] Reviewed common routing problems.
- [ ] Generated the routing diagnostic report.
- [ ] Completed lab documentation.

**LAB 04 STATUS: READY FOR PRACTICAL COMPLETION**
