
# IP ADDRESSING AND SUBNETTING

## 1. What Is an IP Address?

An IP (Internet Protocol) address is a numerical address assigned to a device on a network.

It helps devices identify one another and communicate.

Example:
192.168.1.10

## 2. Types of IP Addresses

### IPv4

IPv4 uses 32 bits and is written as four decimal octets.

Example:
192.168.1.10

Each octet ranges from 0 to 255.

### IPv6

IPv6 uses 128 bits and is written in hexadecimal.

Example:
2001:db8::1

IPv6 provides a much larger address space than IPv4.

## 3. Private and Public IP Addresses

### Private IPv4 ranges

| Range | CIDR |
|---|---|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

Private addresses are commonly used inside local networks.

Public IP addresses are globally routable on the internet.

## 4. What Is a Subnet Mask?

A subnet mask separates the network portion of an IPv4 address from the host portion.

Example:

IP address:
192.168.1.10

Subnet mask:
255.255.255.0

CIDR notation:
192.168.1.10/24

## 5. Understanding CIDR

CIDR means Classless Inter-Domain Routing.

The number after the slash indicates how many bits are used for the network portion.

Examples:

| CIDR | Subnet Mask | Total Addresses |
|---|---|---:|
| /8 | 255.0.0.0 | 16,777,216 |
| /16 | 255.255.0.0 | 65,536 |
| /24 | 255.255.255.0 | 256 |
| /25 | 255.255.255.128 | 128 |
| /26 | 255.255.255.192 | 64 |
| /27 | 255.255.255.224 | 32 |
| /28 | 255.255.255.240 | 16 |
| /30 | 255.255.255.252 | 4 |

For ordinary IPv4 subnets, usable host addresses are generally total addresses minus two, excluding the network and broadcast addresses. Special cases such as /31 and /32 differ.

## 6. Important Subnetting Terms

- Network address: Identifies the subnet.
- Broadcast address: Sends traffic to all hosts in an IPv4 subnet.
- Host address: Identifies an individual device.
- Subnet mask: Separates network and host bits.
- Default gateway: Routes traffic to other networks.

## 7. Basic Subnetting Example

Given:

IP address: 192.168.1.10/24

Subnet mask: 255.255.255.0

Network address: 192.168.1.0

Broadcast address: 192.168.1.255

First usable address: 192.168.1.1

Last usable address: 192.168.1.254

Total addresses: 256

Usable host addresses: 254

## 8. Common Linux Commands

Display IP addresses:

```bash
ip addr
```

Display routing information:

```bash
ip route
```

Display interface details:

```bash
ip link
```

Display network statistics:

```bash
ip -s link
```

## 9. Useful Networking Tools

### ipcalc

Calculates subnet information.

```bash
ipcalc 192.168.1.10/24
```

### sipcalc

Displays detailed IPv4 and IPv6 subnet information.

```bash
sipcalc 192.168.1.10/24
```

These tools may need to be installed first, depending on your Linux distribution.

## 10. Cybersecurity Applications

IP addressing and subnetting help security professionals:

- Understand network layouts.
- Identify devices and network segments.
- Configure firewalls and access rules.
- Investigate suspicious network traffic.
- Design isolated laboratory networks.
- Understand network scanning results.

## 11. Practice Exercises

1. What is an IP address?
2. Explain the difference between IPv4 and IPv6.
3. What is a subnet mask?
4. What does /24 mean?
5. Identify the private IPv4 ranges.
6. Calculate the usable host count for a /26 subnet.
7. Find the network address of 192.168.5.20/24.

## Summary

IP addressing identifies devices on networks, while subnetting divides networks into smaller sections. Both are fundamental networking skills for system administration and cybersecurity.
