---
---

# Module 10 – Lesson 06: Working with Linux `ip` Commands

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand the Linux `ip` command.
2. Display network interfaces.
3. Inspect IPv4 and IPv6 addresses.
4. Examine routing tables.
5. Inspect neighbour information.
6. Understand interface states.
7. Interpret basic network statistics.
8. Use the `ip` command for network troubleshooting.

---

## 2. What Is the Linux `ip` Command?

The `ip` command is a Linux networking utility used to inspect and manage network interfaces, IP addresses, routes, and neighbour information.

It is part of the `iproute2` package.

It is one of the most important tools for Linux network administration.

The general syntax is:

```bash
ip [ OPTIONS ] OBJECT COMMAND
```

Common objects include:

| Object | Purpose |
|---|---|
| `link` | Network interfaces |
| `addr` | IP addresses |
| `route` | Routing information |
| `neigh` | Neighbour information |
| `rule` | Routing policy rules |
| `tunnel` | Network tunnels |

---

## 3. Understanding Network Interface Commands

### 3.1 Display All Interfaces

```bash
ip link
```

Displays all network interfaces.

Example:

```text
1: lo: <LOOPBACK,UP,LOWER_UP>
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

Your interface names may be different.

### 3.2 Display a Specific Interface

```bash
ip link show eth0
```

Replace `eth0` with your actual interface name.

### 3.3 Display Only Administratively Enabled Interfaces

```bash
ip link show up
```

### 3.4 Display Interface Statistics

```bash
ip -s link
```

Displays transmitted and received packet statistics.

### 3.5 Display Detailed Interface Information

```bash
ip -details link show
```

Provides additional interface information.

---

## 4. Understanding IP Address Commands

The `addr` object is used to inspect or manage IP addresses.

### 4.1 Display All IP Addresses

```bash
ip addr
```

### 4.2 Display IPv4 Addresses

```bash
ip -4 addr
```

### 4.3 Display IPv6 Addresses

```bash
ip -6 addr
```

### 4.4 Display a Specific Interface's Addresses

```bash
ip addr show eth0
```

### 4.5 Display Only One Interface's IPv4 Address

```bash
ip -4 addr show dev eth0
```

### 4.6 Display Brief Address Information

```bash
ip -br addr
```

This provides a compact summary of interface names, states, and addresses.

Example:

```text
lo      UNKNOWN  127.0.0.1/8
eth0    UP       192.168.100.20/24
```

The actual output depends on your system.

---

## 5. Understanding Routing Commands

The `route` object displays and manages routing information.

### 5.1 Display IPv4 Routes

```bash
ip route
```

### 5.2 Display IPv6 Routes

```bash
ip -6 route
```

### 5.3 Display the Default Route

```bash
ip route show default
```

### 5.4 Inspect a Destination Route

```bash
ip route get 8.8.8.8
```

This asks Linux which route it would use for the destination.

### 5.5 Display All Routing Tables

```bash
ip route show table all
```

---

## 6. Understanding Neighbour Commands

The `neigh` object displays information about neighbouring network devices.

In IPv4 Ethernet networks, this commonly includes information learned through ARP.

### 6.1 Display Neighbour Entries

```bash
ip neigh
```

### 6.2 Display Neighbours for a Specific Interface

```bash
ip neigh show dev eth0
```

### 6.3 Display IPv4 Neighbour Information

```bash
ip -4 neigh
```

### 6.4 Display IPv6 Neighbour Information

```bash
ip -6 neigh
```

### Understanding Neighbour States

| State | Meaning |
|---|---|
| `REACHABLE` | Neighbour reachability has been recently confirmed |
| `STALE` | Entry exists, but reachability has not been recently confirmed |
| `DELAY` | The system is waiting before probing |
| `PROBE` | The system is actively checking reachability |
| `INCOMPLETE` | Address resolution is in progress |
| `FAILED` | Address resolution failed |
| `PERMANENT` | Static neighbour entry |

---

## 7. Understanding Network Interface States

A network interface can be administratively enabled or disabled.

### UP

The interface is administratively enabled.

### DOWN

The interface is administratively disabled.

### LOWER_UP

The lower-level connection is detected as available.

### UNKNOWN

The system cannot provide a definitive operational state.

This is not automatically an error.

### Example

```bash
ip link show
```

Possible output:

```text
eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

This indicates that the interface is enabled and its lower-level connection is available.

---

## 8. Managing Network Interfaces

The `ip` command can also modify network configuration.

These commands require administrator privileges in normal configurations.

**Important:** In this lesson, understand these commands but do not run configuration-changing commands on your main VMware network interface. Changing it could disconnect your Kali VM.

### 8.1 Enable an Interface

```bash
sudo ip link set eth0 up
```

### 8.2 Disable an Interface

```bash
sudo ip link set eth0 down
```

### 8.3 Change an Interface's MTU

```bash
sudo ip link set eth0 mtu 1400
```

MTU means Maximum Transmission Unit.

It represents the maximum size of a network-layer packet that can be carried by an interface without fragmentation at that layer.

### 8.4 Add an IPv4 Address

```bash
sudo ip addr add 192.168.1.50/24 dev eth0
```

### 8.5 Remove an IPv4 Address

```bash
sudo ip addr del 192.168.1.50/24 dev eth0
```

These are examples only. Do not execute them on your active interface during this lesson.

---

## 9. Understanding Routing Policy Rules

Linux supports policy-based routing.

This means routing decisions can depend on more than just the destination address.

For example, rules may consider:

- Source IP address.
- Routing table.
- Packet marks.
- Other configured selectors.

Display routing policy rules:

```bash
ip rule
```

Display all IPv4 routing tables:

```bash
ip route show table all
```

This functionality is useful in advanced network administration, VPN configurations, and complex server environments.

---

## 10. Practical Lab: Linux Network Inspection

### Lab Objective

Use the `ip` command to inspect your Kali Linux network configuration.

This lab is observational. Do not change the configuration of your active interface.

### Step 1: Display Network Interfaces

```bash
ip link
```

Record all interface names.

### Step 2: Display Brief Interface Information

```bash
ip -br link
```

Observe the state of each interface.

### Step 3: Display All IP Addresses

```bash
ip addr
```

Identify your main interface.

### Step 4: Display IPv4 Information

```bash
ip -4 addr
```

Record your main interface's IPv4 address and prefix.

### Step 5: Display IPv6 Information

```bash
ip -6 addr
```

Observe whether IPv6 addresses are configured.

### Step 6: Display Interface Statistics

```bash
ip -s link
```

Record the RX and TX packet counts.

### Step 7: Display Routing Information

```bash
ip route
```

Identify the default route if one exists.

### Step 8: Check a Destination Route

```bash
ip route get 1.1.1.1
```

Identify the selected output interface and next hop.

### Step 9: Display Neighbour Information

```bash
ip neigh
```

Observe the entries and their states.

### Step 10: Display Routing Policy Rules

```bash
ip rule
```

Record the output.

---

## 11. Practical Assessment Questions

Answer the following:

1. What is the purpose of the Linux `ip` command?
2. Which package provides the `ip` utility?
3. What does `ip link` display?
4. What is the difference between `ip addr` and `ip route`?
5. What does `ip -br addr` display?
6. What is the purpose of `ip neigh`?
7. What does the `STALE` neighbour state mean?
8. What is the purpose of `ip route get 8.8.8.8`?
9. What does MTU stand for?
10. Why should you be careful when running `ip link set eth0 down`?

---

## 12. Important Commands to Remember

```bash
ip link
ip link show up
ip -s link
ip -br link

ip addr
ip -4 addr
ip -6 addr
ip -br addr

ip route
ip -6 route
ip route show default
ip route get 8.8.8.8

ip neigh
ip -4 neigh
ip -6 neigh

ip rule
```

---

## 13. Lesson Summary

In this lesson, you learned:

- The `ip` command is a central Linux networking utility.
- `ip link` displays network interface information.
- `ip addr` displays IP address configuration.
- `ip route` displays routing information.
- `ip neigh` displays neighbour information.
- `ip rule` displays routing policy rules.
- `ip -s link` displays interface statistics.
- The `ip` command can inspect and modify network configuration.
- Configuration changes should be performed carefully because they can interrupt connectivity.

### Key Definitions

**iproute2:** A collection of Linux utilities for network configuration and management.

**Interface State:** The administrative or operational condition of a network interface.

**Neighbour Table:** A cache containing information used to resolve local network-layer addresses to link-layer addresses.

**MTU:** Maximum Transmission Unit, the maximum network-layer packet size supported by a link without fragmentation at that layer.

**Routing Policy:** Rules that influence how Linux selects routes for network traffic.

---

## 14. Knowledge Check

1. What is the difference between `ip link` and `ip addr`?
2. Which command displays only IPv4 addresses?
3. Which command displays the default route?
4. What is the purpose of the neighbour table?
5. What does `LOWER_UP` indicate?
6. Why can disabling a network interface interrupt your connection?
7. Which command displays network statistics?
8. Which command displays Linux routing policy rules?

**End of Lesson 06 – Working with Linux `ip` Commands**