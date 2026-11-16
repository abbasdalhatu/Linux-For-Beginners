
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 02: NETWORK INTERFACES AND IP CONFIGURATION

### 1. Lab Overview

A network interface is a hardware or virtual component that allows a computer to communicate with other devices over a network.

Linux uses network interfaces to send and receive data.

Examples include:

- Ethernet interfaces.
- Wireless interfaces.
- Loopback interfaces.
- Virtual network interfaces.
- VPN interfaces.

In this lab, you will learn how to inspect network interfaces, understand their states, identify IP configurations, and investigate basic interface problems.

**Lab environment:** Kali Linux running inside VMware.

**Safety:** This lab focuses on inspection and controlled configuration of your own VM. Do not change Windows host networking or modify interfaces you do not understand.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Identify Linux network interfaces.
2. Understand interface names.
3. Inspect interface states.
4. Identify MAC addresses.
5. Inspect IPv4 and IPv6 configuration.
6. Understand DHCP and static IP configuration.
7. Inspect network interface statistics.
8. Understand basic interface troubleshooting.

### 3. Understanding Network Interfaces

A network interface provides a connection between a Linux system and a network.

Consider these examples:

| Interface | Description |
|---|---|
| `lo` | Loopback interface |
| `eth0` | Common Ethernet interface name |
| `ens33` | Predictable Ethernet interface name |
| `wlan0` | Common wireless interface name |
| `tun0` | Common tunnel interface name |
| `docker0` | Docker virtual bridge, when present |

Your Kali VM may use a different name.

Do not assume that every system has all these interfaces.

### 4. Inspect All Network Interfaces

Run:

```bash
ip link
```

This displays network interfaces and their link-layer information.

A simplified example:

```text
1: lo: <LOOPBACK,UP,LOWER_UP>
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP>
```

Your output may be different.

Understand the important terms:

- `LOOPBACK`: Local internal communication.
- `BROADCAST`: Supports broadcast communication.
- `MULTICAST`: Supports multicast communication.
- `UP`: Interface is administratively enabled.
- `LOWER_UP`: The lower-level connection reports an active link.

### 5. Display Interfaces in a Short Format

Run:

```bash
ip -br link
```

The `-br` option displays a brief summary.

Example:

```text
lo      UNKNOWN
eth0    UP
```

This is useful when a system has many interfaces.

### 6. Inspect a Specific Interface

Run:

```bash
ip link show lo
```

Inspect your main interface by replacing `eth0` with its actual name:

```bash
ip link show eth0
```

Identify:

- Interface name.
- Interface index.
- Interface state.
- MAC address.
- Supported link-layer information.

### 7. Understand Interface States

Linux network interfaces can have different states.

**UP**

The interface is administratively enabled.

**DOWN**

The interface is administratively disabled.

**LOWER_UP**

The underlying link is available.

**UNKNOWN**

The interface driver or virtual interface does not provide a conventional operational state.

Important distinction:

An interface showing `UP` does not automatically mean that internet connectivity is working.

You must also inspect IP addressing, routes, DNS, and connectivity.

### 8. Inspect MAC Addresses

Run:

```bash
ip link
```

Look for the `link/ether` field.

Example:

```text
link/ether 00:0c:29:12:34:56
```

This is the interface's Ethernet MAC address.

Inspect a specific interface:

```bash
ip link show eth0
```

Replace `eth0` with your interface name.

MAC addresses are useful for understanding local-link communication and identifying interfaces.

However, they can be changed or randomized, so they should not be treated as permanent device identities.

### 9. Inspect IPv4 and IPv6 Addresses

Run:

```bash
ip addr
```

Short format:

```bash
ip -br addr
```

Display only IPv4 addresses:

```bash
ip -4 addr
```

Display only IPv6 addresses:

```bash
ip -6 addr
```

Important address examples:

| Address | Meaning |
|---|---|
| `127.0.0.1` | IPv4 loopback |
| `::1` | IPv6 loopback |
| `169.254.x.x` | IPv4 link-local address |
| `192.168.x.x` | Common private IPv4 range |
| `10.x.x.x` | Private IPv4 range |
| `172.16.x.x` to `172.31.x.x` | Private IPv4 range |
| `fe80::/10` | IPv6 link-local range |

A missing IPv4 address does not necessarily mean the interface is broken. The VM might be configured for IPv6 only or might not have received an IPv4 lease.

### 10. Understand DHCP

DHCP stands for Dynamic Host Configuration Protocol.

It allows a network to automatically provide IP configuration to a device.

A DHCP server may provide:

- IPv4 address.
- Subnet mask or prefix information.
- Default gateway.
- DNS server addresses.
- Lease duration.

The basic DHCP process is commonly described using DORA:

1. Discover.
2. Offer.
3. Request.
4. Acknowledge.

Example:

Your Kali VM connects to a VMware NAT network.

The virtual DHCP service may automatically assign it an IP address.

Inspect your current address configuration:

```bash
ip addr
```

Inspect network routes:

```bash
ip route
```

Do not manually release or renew your DHCP lease during this initial inspection exercise.

### 11. Understand Static IP Configuration

A static IP address is configured manually or through a persistent network configuration system.

Example:

```text
IP address: 192.168.50.10
Prefix:     /24
Gateway:    192.168.50.1
```

Static addressing is useful when a device needs a predictable address.

Examples:

- Servers.
- Network appliances.
- Laboratory machines.
- Infrastructure services.

However, incorrect static configuration can create IP conflicts or connectivity problems.

For now, understand the concept without changing your VMware configuration.

### 12. Inspect Network Interface Statistics

Run:

```bash
ip -s link
```

You will see receive and transmit statistics.

Important fields include:

- RX: Received traffic.
- TX: Transmitted traffic.
- Packets: Number of packets.
- Bytes: Total data transferred.
- Errors: Reported transmission or reception errors.
- Dropped: Packets discarded by the interface or networking stack.

These statistics help identify possible interface problems.

For a more focused inspection:

```bash
ip -s link show eth0
```

Replace `eth0` with your actual interface.

### 13. Inspect Network Configuration Using NetworkManager

Kali commonly uses NetworkManager.

Check whether it is active:

```bash
systemctl status NetworkManager --no-pager
```

Display available connections:

```bash
nmcli connection show
```

Display device status:

```bash
nmcli device status
```

Inspect general network status:

```bash
nmcli general status
```

Inspect a specific device:

```bash
nmcli device show
```

NetworkManager helps manage:

- Ethernet connections.
- Wireless connections.
- IP configuration.
- DNS configuration.
- Network connection profiles.

If `nmcli` is unavailable, continue using the `ip` commands.

### 14. Understand Interface Troubleshooting

Consider these situations:

**Problem A: Interface is DOWN**

Possible causes:

- Interface administratively disabled.
- Virtual adapter disconnected.
- Driver issue.

**Problem B: Interface has no IPv4 address**

Possible causes:

- DHCP server unavailable.
- DHCP configuration problem.
- Interface not connected.
- Static configuration not applied.

**Problem C: Interface is UP but internet does not work**

Possible causes:

- Missing default gateway.
- DNS resolution failure.
- VMware NAT problem.
- Upstream connectivity problem.

**Problem D: High packet drops**

Possible causes:

- Network congestion.
- Driver issues.
- Resource limitations.
- Network interface problems.

A good administrator investigates the cause before changing configuration.

### 15. Practical Exercise 1 – Identify Your Interfaces

Run:

```bash
ip link
```

Then:

```bash
ip -br link
```

Record:

- Number of interfaces.
- Main interface name.
- Loopback interface.
- Main interface state.

### 16. Practical Exercise 2 – Identify IP Addresses

Run:

```bash
ip addr
```

Then:

```bash
ip -br addr
```

Record:

- Main interface IPv4 address, if present.
- Main interface IPv6 address, if present.
- Loopback address.

### 17. Practical Exercise 3 – Inspect MAC Addresses

Run:

```bash
ip link
```

Identify the MAC address associated with your main network interface.

Record it in your report, but avoid sharing it publicly.

### 18. Practical Exercise 4 – Inspect Interface Statistics

Run:

```bash
ip -s link
```

Identify:

- Received packets.
- Transmitted packets.
- Received bytes.
- Transmitted bytes.
- Errors.
- Dropped packets.

Repeat the command after generating a small amount of normal network activity, such as opening an approved website, if your VM has internet access.

Compare the counters.

### 19. Practical Exercise 5 – Inspect NetworkManager

Run:

```bash
nmcli general status
```

```bash
nmcli device status
```

```bash
nmcli connection show
```

Identify the connection profile associated with your main interface.

### 20. Practical Exercise 6 – Inspect the Loopback Interface

Run:

```bash
ip addr show lo
```

Identify:

- Loopback IPv4 address.
- Loopback IPv6 address, if present.
- Interface state.

The loopback interface allows a computer to communicate with itself.

It is commonly used for local testing and internal application communication.

### 21. Generate a Network Interface Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab02
```

Create the script:

```bash
nano ~/practical-labs/module05/lab02/interface-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab02/interface-report.txt"

{
    echo "NETWORK INTERFACE REPORT"
    echo "========================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "NETWORK INTERFACES"
    ip -br link
    echo

    echo "IP CONFIGURATION"
    ip -br addr
    echo

    echo "MAC AND LINK INFORMATION"
    ip link
    echo

    echo "INTERFACE STATISTICS"
    ip -s link
    echo

    echo "NETWORK DEVICE STATUS"
    nmcli device status 2>&1
    echo

    echo "NETWORK CONNECTIONS"
    nmcli connection show 2>&1

} > "$REPORT" 2>&1

echo "Report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab02/interface-report.sh
```

Run it:

```bash
~/practical-labs/module05/lab02/interface-report.sh
```

Inspect the report:

```bash
cat ~/practical-labs/module05/lab02/interface-report.txt
```

### 22. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab02/lab02-report.txt
```

Record:

```text
MODULE 05 – LAB 02
NETWORK INTERFACES AND IP CONFIGURATION

1. Number of network interfaces:
2. Main network interface:
3. Main interface state:
4. MAC address:
5. IPv4 address, if present:
6. IPv6 address, if present:
7. Loopback IPv4 address:
8. What does UP mean?
9. What does LOWER_UP mean?
10. What is DHCP?
11. What is a static IP address?
12. What does nmcli do?
13. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 23. Knowledge Check

1. What is a network interface?
2. What is the purpose of the loopback interface?
3. What does `ip link` display?
4. What is the difference between `UP` and `LOWER_UP`?
5. What is a MAC address?
6. What is DHCP?
7. What is a static IP address?
8. What does `ip -br addr` display?
9. What is the purpose of `ip -s link`?
10. What is NetworkManager?

### 24. Lab Completion Checklist

- [ ] Identified Linux network interfaces.
- [ ] Inspected interface names.
- [ ] Examined interface states.
- [ ] Identified MAC addresses.
- [ ] Inspected IPv4 configuration.
- [ ] Inspected IPv6 configuration.
- [ ] Learned DHCP fundamentals.
- [ ] Learned static IP fundamentals.
- [ ] Inspected interface statistics.
- [ ] Examined NetworkManager.
- [ ] Inspected the loopback interface.
- [ ] Generated the interface report.
- [ ] Completed lab documentation.

**LAB 02 STATUS: READY FOR PRACTICAL COMPLETION**
