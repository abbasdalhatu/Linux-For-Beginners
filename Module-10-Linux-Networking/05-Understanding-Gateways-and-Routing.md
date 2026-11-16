# Module 10 – Lesson 05: Understanding Gateways and Routing

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define a network gateway.
2. Explain the purpose of a default gateway.
3. Understand routing.
4. Differentiate between local and remote network communication.
5. Understand routing tables.
6. Identify default routes.
7. Inspect routing information using Linux commands.
8. Explain the importance of routing in cybersecurity.

---

## 2. What Is a Network Gateway?

A network gateway is a device or system that provides a path from one network to another.

In ordinary IP networking, a router commonly acts as the default gateway for devices on a local network.

### Simple Analogy

Imagine your university has several departments.

To leave the university and reach another location, you must pass through an exit gate.

The gateway performs a similar role in networking.

It provides a path for traffic leaving the local network.

### Example

Suppose your computer has:

```text
IP Address:     192.168.1.10
Subnet Mask:    255.255.255.0
Default Gateway: 192.168.1.1
```

Your computer belongs to:

```text
192.168.1.0/24
```

The gateway is:

```text
192.168.1.1
```

When your computer communicates with an external network, it can send the traffic to this gateway.

---

## 3. What Is a Default Gateway?

A default gateway is the next-hop router used when a destination does not match a more specific route in the routing table.

For example:

Your Kali machine wants to communicate with:

```text
8.8.8.8
```

If there is no more specific route for that destination, the machine may forward the packet to its default gateway.

The gateway then determines how to forward the packet.

### Communication Diagram

```text
+-----------------------+
| Kali Linux VM         |
| IP: 192.168.1.10      |
+-----------+-----------+
            |
            | Local Network
            |
            v
+-----------------------+
| Default Gateway       |
| 192.168.1.1           |
+-----------+-----------+
            |
            | Other Networks
            |
            v
+-----------------------+
| Internet Destination |
| 8.8.8.8               |
+-----------------------+
```

---

## 4. What Is Routing?

Routing is the process of selecting a path through which network packets travel from a source to a destination.

Routers and computers use routing tables to determine where packets should be sent.

Routing can happen:

- Within a local network.
- Between different private networks.
- Across the Internet.

### Example

Suppose a computer wants to communicate with:

```text
192.168.1.20
```

Its own address is:

```text
192.168.1.10/24
```

Both addresses belong to:

```text
192.168.1.0/24
```

The destination is on the same subnet.

The computer can communicate directly over the local network.

Now suppose it wants to reach:

```text
10.10.10.20
```

That destination belongs to a different network.

The computer needs a route to reach it. This may be a specific route or the default gateway.

---

## 5. Understanding Routing Tables

A routing table is a collection of rules that tells a system where to forward network traffic.

A Linux routing table may contain:

- Destination network.
- Gateway or next hop.
- Output interface.
- Route metric.
- Route scope.

### Example Routing Table

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link
```

Explanation:

**Line 1:**

```text
default via 192.168.1.1 dev eth0
```

This means:

- `default`: Use this route when no more specific route matches.
- `via 192.168.1.1`: Send packets to the next-hop gateway.
- `dev eth0`: Use the `eth0` interface.

**Line 2:**

```text
192.168.1.0/24 dev eth0 proto kernel scope link
```

This represents a directly connected network.

The system can reach destinations on that network through `eth0`.

Your actual routing table may look different.

---

## 6. Understanding the Default Route

The default route is commonly represented as:

```text
0.0.0.0/0
```

This IPv4 prefix matches every IPv4 destination.

It is used when a more specific matching route is not available.

An IPv6 default route is represented as:

```text
::/0
```

### Example

```text
default via 192.168.1.1 dev eth0
```

This tells Linux to send otherwise-unmatched IPv4 traffic to `192.168.1.1`.

---

## 7. How Does a Computer Decide Where to Send Packets?

Linux uses the routing table to select a route.

A simplified process is:

1. An application sends data to a destination IP address.
2. Linux examines its routing table.
3. It identifies matching routes.
4. It selects the most specific matching route.
5. It determines the next hop and output interface.
6. It forwards the packet.

### Longest Prefix Match

When multiple routes match a destination, the most specific prefix is generally preferred.

Example:

```text
10.0.0.0/8
10.10.0.0/16
10.10.10.0/24
```

A packet destined for:

```text
10.10.10.50
```

matches all three networks.

The `/24` route is the most specific, so it is preferred, assuming no other routing rules change the decision.

---

## 8. Direct Communication vs Gateway Communication

### Same Subnet

Computer A:

```text
192.168.1.10/24
```

Computer B:

```text
192.168.1.20/24
```

Both belong to:

```text
192.168.1.0/24
```

They can communicate directly at the local network layer.

The sender uses ARP in IPv4 Ethernet networks to discover the destination's MAC address if it is not already known.

### Different Subnets

Computer A:

```text
192.168.1.10/24
```

Computer B:

```text
192.168.2.20/24
```

These devices belong to different subnets.

Communication requires an appropriate router or another routing mechanism.

Computer A sends the packet to its selected next-hop router.

---

## 9. Understanding ARP and the Gateway

ARP stands for Address Resolution Protocol.

In IPv4 Ethernet networks, ARP helps a device discover the MAC address associated with a local IPv4 address.

Example:

Your Kali machine wants to send traffic through:

```text
192.168.1.1
```

If it does not already know the gateway's MAC address, it may send an ARP request.

The gateway replies with its MAC address.

Your computer can then send Ethernet frames to the gateway.

Important:

When sending traffic to a remote destination, your computer normally uses the gateway's MAC address as the local Ethernet destination, not the remote server's MAC address.

---

## 10. Linux Commands for Inspecting Routing

### Command 1: Display the IPv4 Routing Table

```bash
ip route
```

Displays IPv4 routing information.

### Command 2: Display IPv6 Routing Information

```bash
ip -6 route
```

### Command 3: Display All Routing Information

```bash
ip route show table all
```

### Command 4: Display the Default Route

```bash
ip route show default
```

### Command 5: Ask Linux Which Route It Will Use

```bash
ip route get 8.8.8.8
```

This asks the kernel to perform a route lookup for the destination.

The output may identify:

- Destination.
- Next-hop gateway.
- Output interface.
- Source IP address.

### Command 6: Display Neighbour Information

```bash
ip neigh
```

Displays the IPv4 and IPv6 neighbour cache.

For IPv4 Ethernet networks, this commonly includes information learned through ARP.

---

## 11. Understanding Route Metrics

A route metric is a value used to compare routes of similar specificity.

For example:

```text
default via 192.168.1.1 dev eth0 metric 100
default via 192.168.2.1 dev eth1 metric 200
```

When routes have equal prefix specificity, a lower metric is generally preferred.

Other routing rules and policy-routing configuration can also affect route selection.

---

## 12. Practical Lab: Inspecting Gateways and Routes in Kali

### Lab Objective

Examine your Kali VM's routing table and identify its default gateway.

All commands are observational. Do not change your routing configuration.

### Step 1: Display the IPv4 Routing Table

```bash
ip route
```

Record the output.

### Step 2: Display the Default Route

```bash
ip route show default
```

Look for:

```text
default via
```

Record the gateway address if one exists.

### Step 3: Inspect IPv6 Routes

```bash
ip -6 route
```

Observe whether your system has an IPv6 default route.

### Step 4: Check the Route to a Public IP

```bash
ip route get 1.1.1.1
```

Identify the output interface and next hop, if present.

### Step 5: Check the Route to Your Loopback Address

```bash
ip route get 127.0.0.1
```

Observe which interface Linux selects.

### Step 6: Inspect Neighbour Information

```bash
ip neigh
```

Observe whether any neighbour entries are present.

### Step 7: Display All Routing Tables

```bash
ip route show table all
```

Notice that Linux may maintain more than one routing table.

### Step 8: Test Connectivity

```bash
ping -c 4 1.1.1.1
```

Compare the result with your route lookup.

A valid route does not guarantee that the destination will respond to ping.

---

## 13. Practical Assessment Questions

Answer the following:

1. What is a network gateway?
2. What is the purpose of a default gateway?
3. Define routing.
4. What is a routing table?
5. What does `0.0.0.0/0` represent?
6. What does `::/0` represent?
7. What is the difference between local and remote network communication?
8. What does ARP do?
9. What is a route metric?
10. Which Linux command displays the default route?

---

## 14. Important Commands to Remember

```bash
ip route
ip route show default
ip -6 route
ip route show table all
ip route get 8.8.8.8
ip neigh
ping -c 4 1.1.1.1
```

---

## 15. Lesson Summary

In this lesson, you learned:

- A gateway provides a path between networks.
- A default gateway is commonly used for destinations without a more specific route.
- Routing determines where packets should be forwarded.
- Routing tables contain destination and forwarding information.
- Linux generally prefers the most specific matching route.
- ARP resolves local IPv4 addresses to MAC addresses on Ethernet networks.
- Route metrics help compare routes of similar specificity.
- Linux uses the `ip route` command to inspect routing information.

### Key Definitions

**Gateway:** A device or system that provides a path between networks.

**Default Gateway:** The next-hop router used for destinations without a more specific route.

**Routing:** The process of selecting a path for network packets.

**Routing Table:** A collection of routes used to determine packet forwarding.

**ARP:** Address Resolution Protocol, used to discover link-layer addresses for local IPv4 communication.

**Route Metric:** A value used to compare the preference of routes.

---

## 16. Knowledge Check

1. Why does a computer need a default gateway?
2. What happens when a destination belongs to a different subnet?
3. What does the Linux command `ip route` display?
4. Why is `0.0.0.0/0` called the default route?
5. What is the purpose of ARP?
6. What does longest prefix match mean?
7. What is the purpose of a route metric?
8. Which command can show the route Linux would use to reach `1.1.1.1`?

**End of Lesson 05 – Understanding Gateways and Routing**