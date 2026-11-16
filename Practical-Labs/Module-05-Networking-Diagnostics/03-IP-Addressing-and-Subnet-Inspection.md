
# PRACTICAL LABS
# MODULE 05 – LINUX NETWORKING DIAGNOSTICS

## LAB 03: IP ADDRESSING AND SUBNET INSPECTION

### 1. Lab Overview

IP addressing is one of the most important foundations of computer networking.

Every device communicating over an IP network uses IP addressing to identify network interfaces and deliver packets toward their destinations.

Subnetting divides an IP network into smaller logical networks called subnets.

Network administrators use subnetting to:

- Organize devices into networks.
- Reduce unnecessary broadcast traffic.
- Separate departments.
- Improve network management.
- Support network security segmentation.
- Control how traffic moves between networks.

In cybersecurity, understanding IP addressing helps you identify network boundaries, understand routing, investigate traffic, and assess network exposure.

**Lab environment:** Kali Linux VM.

**Safety:** All calculations and inspections in this lab use private, example, or locally assigned addresses. Do not change your actual network configuration.

### 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain IPv4 addressing.
2. Identify network and host portions.
3. Understand subnet masks.
4. Understand CIDR notation.
5. Identify private and public IPv4 addresses.
6. Calculate basic subnet information.
7. Identify network and broadcast addresses.
8. Inspect IP configuration using Linux commands.
9. Use `ipcalc` when available.
10. Generate an IP addressing report.

### 3. Understanding IPv4 Addresses

IPv4 stands for Internet Protocol version 4.

An IPv4 address contains 32 bits.

It is commonly represented using four decimal numbers separated by dots.

Example:

```text
192.168.1.10
```

Each number represents eight bits, known as an octet.

Therefore:

```text
192       .168       .1         .10
8 bits    8 bits     8 bits     8 bits
```

Total:

\[
8+8+8+8=32\text{ bits}
\]

Each octet ranges from 0 to 255.

### 4. Binary Representation of IPv4

Computers process binary values.

Each IPv4 octet can be represented using eight binary bits.

Example:

```text
Decimal: 192
Binary:  11000000
```

Another example:

```text
Decimal: 168
Binary:  10101000
```

Consider:

```text
192.168.1.10
```

Its binary representation is:

```text
11000000.10101000.00000001.00001010
```

You do not need to memorize every possible binary value. Understanding the relationship between binary and decimal is what matters.

### 5. Understanding Subnet Masks

A subnet mask identifies which bits of an IPv4 address represent the network portion and which represent the host portion.

Example:

```text
IP Address:  192.168.1.10
Subnet Mask: 255.255.255.0
```

The subnet mask in binary is:

```text
11111111.11111111.11111111.00000000
```

The first 24 bits identify the network portion.

The remaining 8 bits identify the host portion.

This subnet is also written as:

```text
192.168.1.10/24
```

### 6. Understanding CIDR Notation

CIDR stands for Classless Inter-Domain Routing.

CIDR represents the number of network bits in an IP address.

Examples:

| CIDR | Subnet Mask | Total IPv4 Addresses |
|---|---|---:|
| /8 | 255.0.0.0 | 16,777,216 |
| /16 | 255.255.0.0 | 65,536 |
| /24 | 255.255.255.0 | 256 |
| /25 | 255.255.255.128 | 128 |
| /26 | 255.255.255.192 | 64 |
| /27 | 255.255.255.224 | 32 |
| /28 | 255.255.255.240 | 16 |
| /29 | 255.255.255.248 | 8 |
| /30 | 255.255.255.252 | 4 |

For a conventional IPv4 subnet with \(h\) host bits, the number of addresses is:

\[
2^h
\]

For most ordinary subnets, the usable host count is:

\[
2^h-2
\]

The two excluded addresses are traditionally the network address and broadcast address.

There are special cases, including `/31` point-to-point networks and `/32` host routes.

### 7. Understanding Network and Host Portions

Consider:

```text
192.168.1.10/24
```

The network address is:

```text
192.168.1.0
```

The first usable host address is:

```text
192.168.1.1
```

The last usable host address is:

```text
192.168.1.254
```

The broadcast address is:

```text
192.168.1.255
```

The network contains 256 total addresses, of which 254 are conventionally usable host addresses.

### 8. Understanding Private and Public IPv4 Addresses

Private IPv4 addresses are reserved for internal networks.

The three major private IPv4 ranges are:

| Private Range | CIDR |
|---|---|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

Examples:

```text
10.10.1.5
172.16.5.20
192.168.1.100
```

Public IPv4 addresses are globally routable addresses assigned through internet address allocation systems.

Important: Not every address outside the private ranges is necessarily publicly reachable. Some other ranges are reserved for special purposes.

### 9. Understanding Special IPv4 Addresses

| Address or Range | Purpose |
|---|---|
| 127.0.0.0/8 | Loopback |
| 169.254.0.0/16 | IPv4 link-local |
| 0.0.0.0 | Unspecified address; also used in certain routing and listening contexts |
| 255.255.255.255 | Limited broadcast |
| 192.0.2.0/24 | Documentation and examples |
| 198.51.100.0/24 | Documentation and examples |
| 203.0.113.0/24 | Documentation and examples |

Documentation ranges are useful when writing networking examples without exposing real public addresses.

### 10. Understanding Subnet Calculations

Let's solve a subnetting example step by step.

**Question:**

Given:

```text
192.168.10.25/24
```

Find:

1. Subnet mask.
2. Network address.
3. Broadcast address.
4. First usable host.
5. Last usable host.
6. Total addresses.
7. Usable host addresses.

**Step 1 – Identify the prefix**

```text
/24
```

This means 24 network bits and 8 host bits.

**Step 2 – Find the subnet mask**

```text
255.255.255.0
```

**Step 3 – Find the network address**

The host portion becomes zero:

```text
192.168.10.0
```

**Step 4 – Find the broadcast address**

The host portion becomes all ones:

```text
192.168.10.255
```

**Step 5 – Find usable addresses**

First usable:

```text
192.168.10.1
```

Last usable:

```text
192.168.10.254
```

**Step 6 – Calculate total addresses**

\[
2^8=256
\]

**Step 7 – Calculate usable hosts**

\[
256-2=254
\]

Final answer:

| Property | Result |
|---|---|
| IP address | 192.168.10.25 |
| Prefix | /24 |
| Subnet mask | 255.255.255.0 |
| Network | 192.168.10.0 |
| Broadcast | 192.168.10.255 |
| First host | 192.168.10.1 |
| Last host | 192.168.10.254 |
| Total addresses | 256 |
| Usable hosts | 254 |

### 11. Understanding a /26 Subnet

Consider:

```text
192.168.1.70/26
```

A `/26` subnet has:

- 26 network bits.
- 6 host bits.
- 64 total addresses.
- 62 conventionally usable host addresses.

The subnet mask is:

```text
255.255.255.192
```

The block size is:

\[
256-192=64
\]

The subnet ranges in the last octet are:

| Network | Address Range | Broadcast |
|---|---|---|
| 192.168.1.0/26 | .0 – .63 | .63 |
| 192.168.1.64/26 | .64 – .127 | .127 |
| 192.168.1.128/26 | .128 – .191 | .191 |
| 192.168.1.192/26 | .192 – .255 | .255 |

The address `192.168.1.70` belongs to:

```text
192.168.1.64/26
```

Therefore:

- Network: `192.168.1.64`
- First host: `192.168.1.65`
- Last host: `192.168.1.126`
- Broadcast: `192.168.1.127`

### 12. Inspect IP Configuration in Linux

Run:

```bash
ip addr
```

Display only IPv4 addresses:

```bash
ip -4 addr
```

Display a short summary:

```bash
ip -br -4 addr
```

Inspect a particular interface:

```bash
ip -4 addr show eth0
```

Replace `eth0` with your actual interface name.

Look for the prefix after the address.

Example:

```text
inet 192.168.100.20/24
```

The `/24` tells you the network prefix length.

### 13. Inspect Routing Information

Run:

```bash
ip route
```

Example:

```text
default via 192.168.100.1 dev eth0
192.168.100.0/24 dev eth0
```

This indicates:

- The default gateway is `192.168.100.1`.
- The connected network is `192.168.100.0/24`.
- Traffic for other destinations may use the default route.

Your actual addresses will depend on VMware configuration.

### 14. Use ipcalc for Subnet Inspection

`ipcalc` is a utility that calculates subnet information.

Check whether it is installed:

```bash
command -v ipcalc
```

If it is available, run:

```bash
ipcalc 192.168.10.25/24
```

Try:

```bash
ipcalc 192.168.1.70/26
```

You may see:

- Network address.
- Broadcast address.
- Subnet mask.
- Host range.
- Number of addresses.

If it is not installed, inspect its availability:

```bash
apt-cache policy ipcalc
```

You may install it later if necessary, but this lab does not require package installation.

### 15. Understanding IPv6 Addressing

IPv6 addresses use 128 bits.

Example:

```text
2001:db8:abcd:1::10/64
```

Important IPv6 concepts:

- IPv6 uses hexadecimal notation.
- Addresses are separated using colons.
- Consecutive zero groups can be shortened using `::`.
- `/64` is a commonly used IPv6 subnet size.
- IPv6 does not use IPv4-style broadcast addresses.

Inspect IPv6 configuration:

```bash
ip -6 addr
```

Inspect IPv6 routes:

```bash
ip -6 route
```

### 16. Practical Exercise 1 – Inspect Your Actual IPv4 Address

Run:

```bash
ip -br -4 addr
```

Record:

- Interface name.
- IPv4 address.
- Prefix length.

Do not assume that your VM must use a particular private address.

### 17. Practical Exercise 2 – Identify the Subnet Mask

If your interface displays:

```text
192.168.100.20/24
```

Identify:

- IP address.
- Prefix length.
- Subnet mask.
- Network address.

Use the examples above or `ipcalc` if available.

### 18. Practical Exercise 3 – Inspect Private Address Ranges

Study these addresses:

```text
10.20.30.40
172.20.10.5
192.168.50.100
8.8.8.8
```

Classify each as private or non-private IPv4.

Remember that non-private does not automatically mean globally reachable.

### 19. Practical Exercise 4 – Calculate a /27 Subnet

Given:

```text
192.168.5.45/27
```

Calculate:

1. Subnet mask.
2. Total addresses.
3. Usable host addresses.
4. Network address.
5. First usable host.
6. Last usable host.
7. Broadcast address.

Show your working.

### 20. Practical Exercise 5 – Calculate a /28 Subnet

Given:

```text
10.10.10.77/28
```

Calculate:

1. Subnet mask.
2. Total addresses.
3. Usable host addresses.
4. Network address.
5. First usable host.
6. Last usable host.
7. Broadcast address.

Show your working.

### 21. Practical Exercise 6 – Inspect Your Routing Table

Run:

```bash
ip route
```

Identify:

- Default gateway.
- Connected network.
- Interface used by the default route.

### 22. Generate an IP Addressing Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module05/lab03
```

Create the script:

```bash
nano ~/practical-labs/module05/lab03/ip-address-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/practical-labs/module05/lab03/ip-address-report.txt"

{
    echo "IP ADDRESSING AND SUBNET REPORT"
    echo "==============================="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo

    echo "IPV4 CONFIGURATION"
    ip -4 addr
    echo

    echo "IPV6 CONFIGURATION"
    ip -6 addr
    echo

    echo "ROUTING TABLE"
    ip route
    echo

    echo "IPV6 ROUTING TABLE"
    ip -6 route
    echo

    echo "NETWORK INTERFACE SUMMARY"
    ip -br addr
    echo

    echo "SUBNET CALCULATION TOOL"
    if command -v ipcalc >/dev/null 2>&1; then
        ipcalc 192.168.10.25/24
    else
        echo "ipcalc is not installed."
    fi

} > "$REPORT" 2>&1

echo "Report generated: $REPORT"
```

Save the script.

Make it executable:

```bash
chmod +x ~/practical-labs/module05/lab03/ip-address-report.sh
```

Execute:

```bash
~/practical-labs/module05/lab03/ip-address-report.sh
```

Inspect:

```bash
cat ~/practical-labs/module05/lab03/ip-address-report.txt
```

### 23. Lab Documentation

Create:

```bash
nano ~/practical-labs/module05/lab03/lab03-report.txt
```

Record:

```text
MODULE 05 – LAB 03
IP ADDRESSING AND SUBNET INSPECTION

1. Actual IPv4 address:
2. Network prefix:
3. Subnet mask:
4. Network address:
5. Default gateway:
6. Difference between private and public IPv4:
7. What does CIDR mean?
8. What is the purpose of a subnet mask?
9. What is a broadcast address?
10. How many usable hosts are in a /24 subnet?
11. Result of the /27 calculation:
12. Result of the /28 calculation:
13. What did I learn?
```

Save using `CTRL + O`, press `ENTER`, then exit using `CTRL + X`.

### 24. Knowledge Check

1. How many bits are in an IPv4 address?
2. What is an octet?
3. What is a subnet mask?
4. What does `/24` mean?
5. What is CIDR?
6. What is the difference between a network address and a broadcast address?
7. Name the three private IPv4 ranges.
8. How many total addresses are in a `/26` subnet?
9. How many conventionally usable host addresses are in a `/27` subnet?
10. What does `ipcalc` do?
11. What is the purpose of `ip route`?
12. Why is subnetting important in network security?

### 25. Lab Completion Checklist

- [ ] Understood IPv4 addressing.
- [ ] Reviewed binary representation.
- [ ] Understood subnet masks.
- [ ] Learned CIDR notation.
- [ ] Identified network and host portions.
- [ ] Studied private and public IPv4 addressing.
- [ ] Studied special IPv4 ranges.
- [ ] Completed the /24 example.
- [ ] Completed the /26 example.
- [ ] Inspected actual Linux IP configuration.
- [ ] Inspected routing information.
- [ ] Practiced using ipcalc or manual calculations.
- [ ] Reviewed IPv6 addressing.
- [ ] Completed /27 and /28 exercises.
- [ ] Generated the IP addressing report.
- [ ] Completed lab documentation.

**LAB 03 STATUS: READY FOR PRACTICAL COMPLETION**
