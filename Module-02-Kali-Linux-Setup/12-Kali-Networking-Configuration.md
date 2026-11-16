# LESSON 12: KALI NETWORKING CONFIGURATION

**Module:** 02 – Kali Linux Setup  
**File:** `12-Kali-Networking-Configuration.md`

## 1. Introduction

Networking allows computers to communicate with one another.

As an ethical hacker, you must understand how Linux connects to networks, obtains IP addresses, communicates with other devices, and manages network interfaces.

In this lesson, we will explore Kali's networking configuration and learn essential networking commands.

## 2. Understanding Network Interfaces

A network interface is a component that allows a computer to communicate over a network.

Common types include:

| Interface | Description |
|---|---|
| Ethernet | Wired network connection |
| Wi-Fi | Wireless network connection |
| Loopback | Internal interface used by the local computer |
| Virtual interface | Software-created network interface |

In a VMware Kali VM, the network interface may appear as:

`eth0`

It may also have another name, depending on the virtual hardware and Linux configuration.

## 3. Display Network Interfaces

Run:

```bash
ip link show
```

This displays available network interfaces.

To display interfaces and their IP addresses, run:

```bash
ip addr show
```

Look for an interface with an IPv4 address.

Example:

```text
eth0:
inet 192.168.100.128/24
```

This is an example only. Your actual IP address will probably be different.

## 4. Understanding IP Addresses

An IP address is a numerical identifier assigned to a network interface.

IPv4 addresses contain four numerical sections separated by dots.

Example:

`192.168.1.10`

Each section ranges from 0 to 255.

An IP address helps identify a device on an IP network.

### Private IPv4 Address Ranges

The three main private IPv4 ranges are:

| Range | Address space |
|---|---|
| Class A private | 10.0.0.0 – 10.255.255.255 |
| Class B private | 172.16.0.0 – 172.31.255.255 |
| Class C private | 192.168.0.0 – 192.168.255.255 |

These addresses are commonly used in private networks.

## 5. Understanding the Loopback Address

The loopback address refers to the local computer.

For IPv4, it is:

`127.0.0.1`

For IPv6, it is:

`::1`

Test the IPv4 loopback interface:

```bash
ping -c 4 127.0.0.1
```

This tests local TCP/IP communication without requiring an external network connection.

## 6. Understanding the Default Gateway

A default gateway is the network device that forwards traffic when the destination is outside the local network.

Display the routing table:

```bash
ip route
```

Example:

```text
default via 192.168.100.2 dev eth0
192.168.100.0/24 dev eth0
```

Explanation:

- `default`: Default route.
- `via`: Gateway address.
- `dev`: Network interface.
- `/24`: Network prefix length.

Your actual gateway and subnet may differ.

## 7. Understanding DNS

DNS stands for Domain Name System.

DNS translates domain names into IP addresses.

For example:

`kali.org`

can be resolved to an IP address.

Test DNS resolution:

```bash
getent hosts kali.org
```

Another useful command is:

```bash
nslookup kali.org
```

If `nslookup` is unavailable, it can be installed using the appropriate DNS utility package.

## 8. Understanding Network Connectivity

Linux provides several commands for testing connectivity.

### Ping

```bash
ping -c 4 1.1.1.1
```

Tests whether your system can communicate with the destination.

### Trace Route

```bash
tracepath kali.org
```

Shows the network path toward a destination, where supported by the network.

### Display Network Connections

```bash
ss -tuln
```

Displays listening TCP and UDP sockets.

### Display Interface Statistics

```bash
ip -s link
```

Displays packet and error statistics for network interfaces.

## 9. Understanding DHCP

DHCP stands for Dynamic Host Configuration Protocol.

It automatically provides network configuration to devices.

A DHCP server may provide:

- IP address.
- Subnet mask or prefix.
- Default gateway.
- DNS server addresses.
- Lease information.

In our VMware NAT setup, VMware's virtual DHCP service can provide an IP address to Kali.

You can inspect the current interface configuration using:

```bash
ip addr
```

Do not manually change network settings unless necessary.

## 10. Understanding VMware NAT Networking

Our Kali VM uses NAT networking.

NAT stands for Network Address Translation.

It allows the virtual machine to access external networks through the Windows host's network connection.

A simplified arrangement looks like this:

```text
Internet
   |
Windows 11 Host
   |
VMware NAT Network
   |
Kali Linux VM
```

NAT is useful for our initial Kali setup because the VM can access the internet without appearing as a separate physical device on the local network.

However, NAT is not a complete security boundary. We will configure a more controlled lab network when introducing vulnerable practice machines.

## 11. Understanding Important Networking Commands

| Command | Purpose |
|---|---|
| `ip addr` | Displays IP addresses |
| `ip link` | Displays network interfaces |
| `ip route` | Displays routing information |
| `ping` | Tests network reachability |
| `ss` | Displays sockets and connections |
| `hostname -I` | Displays assigned IP addresses |
| `getent hosts` | Looks up host information |
| `ip -s link` | Displays interface statistics |
| `tracepath` | Examines network paths |

## 12. Practical Assignment

Complete these exercises on your Kali VM:

- [ ] Display network interfaces using `ip link show`.
- [ ] Display IP addresses using `ip addr show`.
- [ ] Identify your Kali IPv4 address.
- [ ] Test the loopback address.
- [ ] Display your routing table.
- [ ] Identify your default gateway.
- [ ] Test internet connectivity using `ping`.
- [ ] Test DNS resolution using `getent hosts kali.org`.
- [ ] Display listening sockets using `ss -tuln`.
- [ ] Display interface statistics using `ip -s link`.
- [ ] Identify the network mode configured in VMware.

Record your actual IPv4 address, interface name, and gateway in your course notes. Do not publish your network details unnecessarily.

## 13. Knowledge Check

1. What is a network interface?
2. What is an IPv4 address?
3. What is the purpose of the loopback address?
4. What is a default gateway?
5. What does DNS stand for?
6. What is DHCP?
7. What does NAT stand for?
8. What is the difference between `ip addr` and `ip route`?
9. What does `ss -tuln` display?
10. Why do we use NAT for our initial Kali VM setup?

## 14. Lesson Summary

In this lesson, we learned how Kali Linux manages network interfaces, IP addresses, routing, DNS, DHCP, and network connectivity.

We also explored VMware NAT networking and important Linux networking commands.

These concepts form the foundation for network troubleshooting, network security, and ethical hacking.

**Next Lesson:** Lesson 13 – Kali Snapshots and Backup.