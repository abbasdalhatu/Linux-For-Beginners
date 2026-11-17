---
---

# Module 10 – Lesson 04: Understanding Subnet Masks and CIDR

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define a subnet mask.
2. Explain the purpose of subnetting.
3. Understand network and host portions.
4. Understand CIDR notation.
5. Convert subnet masks into CIDR prefixes.
6. Calculate network and broadcast addresses.
7. Calculate the number of usable IPv4 host addresses.
8. Inspect subnet information in Linux.

---

## 2. What Is a Subnet Mask?

A subnet mask is a 32-bit value used with an IPv4 address to identify the network portion and host portion of the address.

Remember:

An IPv4 address contains 32 bits.

Example:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
```

The subnet mask tells the computer which part of the IP address represents the network.

### Simple Analogy

Imagine a university with different departments.

- The university represents the network.
- Each department represents a subnet.
- Each student represents a host.

Subnetting divides a larger network into smaller networks.

---

## 3. Understanding Network and Host Portions

Consider:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
```

The subnet mask indicates that the first three octets belong to the network portion.

The last octet belongs to the host portion.

| Network Portion | Host Portion |
|---|---|
| 192.168.1 | 10 |

The network address is:

```text
192.168.1.0
```

The host address is represented by the final octet.

---

## 4. What Is Subnetting?

Subnetting is the process of dividing a larger IP network into smaller logical networks.

### Why Is Subnetting Important?

1. Better network organization.
2. Efficient IP address allocation.
3. Reduced broadcast domains.
4. Improved network management.
5. Network isolation and segmentation.
6. Better control of network traffic.

### Real-World Example

A university may have separate networks for:

- Administration.
- Computer laboratories.
- Students.
- Library systems.
- Security infrastructure.

Instead of placing every device in one large network, administrators can create separate subnets.

This makes network management easier and can support stronger security boundaries.

Remember: Subnetting alone does not provide complete security. Firewalls, access controls, and proper routing policies are also necessary.

---

## 5. Understanding Binary Representation

Computers process IP addresses using binary values.

Binary uses only:

- 0
- 1

Each IPv4 octet contains 8 bits.

The decimal values represented by the eight bit positions are:

| Bit Position | Value |
|---|---:|
| 1 | 128 |
| 2 | 64 |
| 3 | 32 |
| 4 | 16 |
| 5 | 8 |
| 6 | 4 |
| 7 | 2 |
| 8 | 1 |

### Example: Convert 192 to Binary

192 can be represented as:

```text
128 + 64 = 192
```

Therefore:

```text
192 = 11000000
```

### Example: Convert 168 to Binary

```text
128 + 32 + 8 = 168
```

Therefore:

```text
168 = 10101000
```

### Complete Example

IP address:

```text
192.168.1.10
```

Binary representation:

```text
11000000.10101000.00000001.00001010
```

You do not need to memorize every possible conversion immediately. Practise the bit values until the process becomes familiar.

---

## 6. Understanding Common Subnet Masks

Some commonly used IPv4 subnet masks are:

| Subnet Mask | CIDR |
|---|---|
| 255.0.0.0 | /8 |
| 255.255.0.0 | /16 |
| 255.255.255.0 | /24 |
| 255.255.255.128 | /25 |
| 255.255.255.192 | /26 |
| 255.255.255.224 | /27 |
| 255.255.255.240 | /28 |
| 255.255.255.248 | /29 |
| 255.255.255.252 | /30 |

These masks contain consecutive binary ones followed by zeros.

For example:

```text
255.255.255.0
```

In binary:

```text
11111111.11111111.11111111.00000000
```

There are 24 binary ones.

Therefore, its CIDR notation is:

```text
/24
```

---

## 7. What Is CIDR?

CIDR stands for Classless Inter-Domain Routing.

CIDR notation represents an IP address and its network prefix length.

Example:

```text
192.168.1.10/24
```

The `/24` means that the first 24 bits are the network portion.

Another example:

```text
192.168.1.10/26
```

The `/26` means that the first 26 bits are the network portion.

The larger the prefix length, the smaller the number of addresses available in that IPv4 subnet.

---

## 8. Important Subnetting Formulas

### Formula 1: Total IPv4 Addresses

For a conventional IPv4 subnet:

\[
\text{Total Addresses} = 2^{32-p}
\]

Where:

- 32 = IPv4 address length.
- p = prefix length.

### Formula 2: Usable Host Addresses

For ordinary IPv4 subnets with a prefix from `/0` through `/30`:

\[
\text{Usable Hosts} = 2^{32-p}-2
\]

The two reserved addresses are normally:

- Network address.
- Broadcast address.

Important exceptions:

- `/31` is used for point-to-point links and can provide two usable endpoint addresses under RFC 3021.
- `/32` identifies one individual IPv4 address and does not describe an ordinary multi-host subnet.

### Formula 3: Number of Borrowed Bits

\[
\text{Borrowed Bits} = \text{New Prefix} - \text{Original Prefix}
\]

### Formula 4: Number of Subnets

\[
\text{Number of Subnets} = 2^b
\]

Where b is the number of borrowed bits.

---

## 9. Solved Example 1: Calculate Hosts in a /24 Network

Given:

```text
192.168.1.0/24
```

Step 1: Calculate host bits.

\[
32-24=8
\]

Step 2: Calculate total addresses.

\[
2^8=256
\]

Step 3: Calculate usable hosts.

\[
256-2=254
\]

Answer:

- Total addresses: 256
- Usable hosts: 254

---

## 10. Solved Example 2: Calculate Hosts in a /26 Network

Given:

```text
192.168.1.0/26
```

Step 1: Calculate host bits.

\[
32-26=6
\]

Step 2: Calculate total addresses.

\[
2^6=64
\]

Step 3: Calculate usable hosts.

\[
64-2=62
\]

Answer:

- Total addresses: 64
- Usable hosts: 62

---

## 11. Solved Example 3: Find the Network and Broadcast Addresses

Given:

```text
IP Address: 192.168.1.70
Subnet Mask: 255.255.255.192
```

Step 1: Convert the subnet mask to CIDR.

```text
255.255.255.192 = /26
```

Step 2: Calculate the block size.

The interesting octet is the fourth octet.

\[
256-192=64
\]

Step 3: Identify the subnet ranges.

```text
0–63
64–127
128–191
192–255
```

Step 4: Identify where 70 belongs.

70 belongs to the range:

```text
64–127
```

Therefore:

- Network address: `192.168.1.64`
- Broadcast address: `192.168.1.127`
- First usable address: `192.168.1.65`
- Last usable address: `192.168.1.126`

---

## 12. Quick Subnet Reference Table

| CIDR | Total Addresses | Usable Hosts |
|---|---:|---:|
| /24 | 256 | 254 |
| /25 | 128 | 126 |
| /26 | 64 | 62 |
| /27 | 32 | 30 |
| /28 | 16 | 14 |
| /29 | 8 | 6 |
| /30 | 4 | 2 |
| /31 | 2 | 2 point-to-point endpoints |
| /32 | 1 | Single address |

This table is useful for CCNA examinations and network administration.

---

## 13. Linux Commands for Inspecting Subnet Information

### Command 1: Display IP Addresses and Prefixes

```bash
ip addr
```

Example output:

```text
inet 192.168.1.10/24
```

The `/24` represents the network prefix.

### Command 2: Display IPv4 Addresses

```bash
ip -4 addr
```

### Command 3: Display Network Routes

```bash
ip route
```

### Command 4: Install or Check ipcalc

First check whether ipcalc is available:

```bash
command -v ipcalc
```

If it is already installed, calculate subnet information:

```bash
ipcalc 192.168.1.70/26
```

If it is not installed, you may install it from your configured Kali repositories:

```bash
sudo apt install ipcalc
```

### Command 5: Inspect a Network Prefix

```bash
ipcalc 10.0.0.0/24
```

Observe the network address, broadcast address, subnet mask, and host range.

---

## 14. Practical Lab: Subnet Calculation

### Lab Objective

Practise calculating IPv4 subnet information and compare your calculations with Linux tools.

### Task 1

Calculate the usable hosts for:

```text
192.168.1.0/24
```

### Task 2

Calculate the usable hosts for:

```text
192.168.1.0/26
```

### Task 3

Calculate the network address and broadcast address for:

```text
IP: 192.168.1.70
Prefix: /26
```

### Task 4

Calculate the total addresses for:

```text
10.0.0.0/27
```

### Task 5

Find the subnet mask represented by:

```text
/28
```

### Task 6

If ipcalc is available, run:

```bash
ipcalc 192.168.1.70/26
```

Compare the result with your manual calculation.

### Task 7

Inspect your Kali VM's actual address:

```bash
ip -4 addr
```

Record the address and prefix of your main interface.

Do not change your VM's network configuration for this exercise.

---

## 15. Practical Assessment Questions

Answer the following:

1. What is a subnet mask?
2. What does CIDR stand for?
3. What does `/24` mean?
4. How many bits are in an IPv4 address?
5. How many usable hosts are available in a conventional `/26` subnet?
6. What is the total number of addresses in a `/27` subnet?
7. What are the network and broadcast addresses of `192.168.1.70/26`?
8. What is the subnet mask for `/28`?
9. Why do organizations divide networks into subnets?
10. Which Linux tool can help calculate subnet information?

---

## 16. Important Formulas to Remember

```text
IPv4 address length = 32 bits

Host bits = 32 - Prefix Length

Total addresses = 2^(Host Bits)

Ordinary usable hosts = Total Addresses - 2

Block size = 256 - Subnet Mask Value
             in the interesting octet

Number of subnets = 2^(Borrowed Bits)
```

---

## 17. Lesson Summary

In this lesson, you learned:

- A subnet mask separates network and host portions.
- Subnetting divides large networks into smaller networks.
- IPv4 uses 32 bits.
- CIDR represents the network prefix length.
- `/24` contains 256 total addresses.
- `/26` contains 64 total addresses.
- `/27` contains 32 total addresses.
- Network and broadcast addresses are normally reserved in conventional IPv4 subnets.
- Linux tools such as `ip addr`, `ip route`, and `ipcalc` help inspect network configuration.

### Key Definitions

**Subnet Mask:** A 32-bit value used to identify the network and host portions of an IPv4 address.

**Subnetting:** Dividing a larger IP network into smaller networks.

**CIDR:** A method of representing IP networks using prefix lengths.

**Network Address:** The address identifying a particular IPv4 subnet.

**Broadcast Address:** The address used to send IPv4 traffic to all hosts on a subnet.

**Host Address:** An IP address assigned to an interface within a network.

---

## 18. Knowledge Check

1. What is the purpose of subnetting?
2. What does the `/26` prefix represent?
3. How many host bits exist in a `/27` network?
4. How many total addresses exist in a `/28` subnet?
5. Why are the network and broadcast addresses normally excluded from usable host calculations?
6. What is the difference between a network address and a host address?
7. What command can you use to inspect an IP address and its prefix in Kali?
8. How does subnetting help with network organization and security?

**End of Lesson 04 – Understanding Subnet Masks and CIDR**