---
---

# Module 10 – Lesson 03: Understanding IP Addresses

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define an IP address.
2. Explain why IP addresses are important.
3. Differentiate between IPv4 and IPv6.
4. Understand public and private IP addresses.
5. Identify special IPv4 addresses.
6. Understand static and dynamic IP addressing.
7. Inspect IP addresses using Linux commands.
8. Explain the importance of IP addressing in cybersecurity.

---

## 2. What Is an IP Address?

IP stands for Internet Protocol.

An IP address is a logical address assigned to a network interface so that it can communicate using the Internet Protocol.

Think of an IP address like a house address.

Imagine sending a letter to someone:

- You need the recipient's address.
- The postal system uses that address to deliver the letter.

Computer networks work similarly.

When one device sends information to another device, IP addressing helps identify the source and destination.

### Example

```text
192.168.1.10
```

This is an example of an IPv4 address.

Another device might have:

```text
192.168.1.20
```

Both devices may communicate if they are appropriately configured on the same network.

---

## 3. Versions of IP Addresses

There are two major versions of Internet Protocol addressing:

1. IPv4
2. IPv6

### 3.1 IPv4

IPv4 stands for Internet Protocol version 4.

It uses 32-bit addresses.

An IPv4 address is normally written as four decimal numbers separated by dots.

Example:

```text
192.168.1.10
```

Each number is called an octet.

An octet contains 8 bits.

Therefore:

\[
8 + 8 + 8 + 8 = 32 \text{ bits}
\]

Each octet can have a decimal value from 0 to 255.

Example:

```text
192.168.1.10
```

| Octet | Value |
|---|---:|
| First | 192 |
| Second | 168 |
| Third | 1 |
| Fourth | 10 |

The theoretical IPv4 address space contains:

\[
2^{32} = 4,294,967,296
\]

possible addresses.

### 3.2 IPv6

IPv6 stands for Internet Protocol version 6.

It uses 128-bit addresses.

IPv6 was developed to provide a much larger address space.

Example:

```text
2001:db8:1234:0000:0000:0000:0000:0001
```

IPv6 addresses use hexadecimal characters and colons.

Hexadecimal uses:

- 0–9
- A–F

An IPv6 address can contain eight groups of four hexadecimal digits.

Its address space is:

\[
2^{128}
\]

possible addresses.

### IPv4 vs IPv6

| Feature | IPv4 | IPv6 |
|---|---|---|
| Address length | 32 bits | 128 bits |
| Format | Decimal | Hexadecimal |
| Separator | Dot | Colon |
| Example | `192.168.1.10` | `2001:db8::1` |
| Address space | Smaller | Extremely large |

---

## 4. Public and Private IP Addresses

IP addresses can be classified according to how they are used.

### 4.1 Public IP Address

A public IP address is globally routable on the public Internet.

It is commonly assigned to an Internet connection by an Internet Service Provider.

Example:

```text
8.8.8.8
```

This is one of Google's public DNS addresses.

Public addresses are used for communication across the Internet.

### 4.2 Private IP Address

Private IPv4 addresses are reserved for use inside private networks.

They are not directly routable across the public Internet.

The three main private IPv4 ranges are:

| Private Range | Address Range |
|---|---|
| Class A private | `10.0.0.0 – 10.255.255.255` |
| Class B private | `172.16.0.0 – 172.31.255.255` |
| Class C private | `192.168.0.0 – 192.168.255.255` |

Examples:

```text
10.0.0.5
172.16.10.20
192.168.1.100
```

These addresses are commonly used in homes, schools, offices, and virtual networks.

### Important Note

Private IP addresses are not automatically secure.

A private network can still contain vulnerable systems, weak passwords, and malicious devices.

---

## 5. Important Special IPv4 Addresses

Some IPv4 addresses have special purposes.

### 5.1 Loopback Address

```text
127.0.0.1
```

This address refers to the local computer.

It is commonly used for testing local network communication.

### 5.2 Unspecified Address

```text
0.0.0.0
```

Its meaning depends on context.

It can represent an unspecified address or be used to listen on all IPv4 interfaces when configuring a service.

### 5.3 Broadcast Address

```text
255.255.255.255
```

This is the limited broadcast address used to reach all hosts on the local network segment.

### 5.4 Link-Local Address

IPv4 link-local addresses use this range:

```text
169.254.0.0/16
```

A device may automatically assign itself a link-local address when normal address configuration is unavailable.

For example:

```text
169.254.10.25
```

An address in this range may indicate a DHCP configuration problem, although there are legitimate uses for link-local addressing.

---

## 6. Static and Dynamic IP Addresses

There are two common ways IP addresses are assigned.

### 6.1 Static IP Address

A static IP address is configured manually or reserved so that a device consistently uses the same address.

Example:

```text
192.168.1.50
```

Static addressing is useful for:

- Servers.
- Network printers.
- Network infrastructure.
- Systems requiring predictable addressing.

### 6.2 Dynamic IP Address

A dynamic IP address is assigned automatically, commonly through DHCP.

DHCP stands for Dynamic Host Configuration Protocol.

A DHCP server can provide:

- IP address.
- Subnet mask or prefix.
- Default gateway.
- DNS server information.

Example:

Your laptop connects to university Wi-Fi and automatically receives:

```text
192.168.10.25
```

Another device may receive a different address.

### Static vs Dynamic

| Static | Dynamic |
|---|---|
| Configured manually or reserved | Assigned automatically |
| More predictable | May change over time |
| Useful for servers | Useful for ordinary clients |
| Requires careful configuration | Commonly uses DHCP |

---

## 7. Understanding IPv4 Address Structure

An IPv4 address has two important logical parts:

1. Network portion.
2. Host portion.

The network portion identifies the IP network.

The host portion identifies an interface within that network.

The subnet mask or prefix length tells the system how to distinguish these portions.

Example:

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
```

The equivalent CIDR notation is:

```text
192.168.1.10/24
```

In this example:

- The first 24 bits identify the network.
- The remaining 8 bits identify the host portion.

We will study subnet masks and CIDR carefully in Lesson 04.

---

## 8. Understanding IPv6 Address Types

IPv6 also has different address categories.

### 8.1 Global Unicast

Globally routable IPv6 addresses commonly come from:

```text
2000::/3
```

These are used for communication across IPv6 networks.

### 8.2 Link-Local

IPv6 link-local addresses use:

```text
fe80::/10
```

They are used for communication on the local network link.

### 8.3 Loopback

The IPv6 loopback address is:

```text
::1
```

It serves a similar purpose to IPv4 `127.0.0.1`.

### 8.4 Unspecified

The IPv6 unspecified address is:

```text
::
```

It represents an unspecified address.

---

## 9. Linux Commands for Inspecting IP Addresses

Linux provides commands for examining IP configuration.

### Command 1: Display All IP Addresses

```bash
ip addr
```

Displays network interfaces and their assigned IP addresses.

### Command 2: Display IPv4 Addresses

```bash
ip -4 addr
```

Displays IPv4 addressing information.

### Command 3: Display IPv6 Addresses

```bash
ip -6 addr
```

Displays IPv6 addressing information.

### Command 4: Display a Specific Interface

```bash
ip addr show eth0
```

Replace `eth0` with your actual interface name.

### Command 5: Display the Hostname

```bash
hostname
```

### Command 6: Display the System's Configured IP Addresses

```bash
hostname -I
```

This prints configured IP addresses, but it does not explain which interface or route each address belongs to.

For detailed information, use `ip addr`.

---

## 10. Practical Lab: Investigating IP Addresses in Kali Linux

### Lab Objective

Inspect your Kali VM's IPv4 and IPv6 configuration.

### Step 1: Display All IP Addresses

```bash
ip addr
```

Identify the addresses assigned to each interface.

### Step 2: Display IPv4 Addresses Only

```bash
ip -4 addr
```

Record the IPv4 address of your main network interface.

### Step 3: Display IPv6 Addresses Only

```bash
ip -6 addr
```

Observe whether IPv6 is configured.

### Step 4: Display Configured IP Addresses

```bash
hostname -I
```

Compare the output with `ip addr`.

### Step 5: Inspect the Loopback Address

```bash
ip addr show lo
```

Look for:

```text
127.0.0.1/8
```

You may also see the IPv6 loopback address:

```text
::1/128
```

### Step 6: Test IPv4 Loopback

```bash
ping -4 -c 4 127.0.0.1
```

### Step 7: Test IPv6 Loopback

```bash
ping -6 -c 4 ::1
```

### Step 8: Inspect Your Network Interface

Replace `eth0` with your actual interface name:

```bash
ip addr show eth0
```

Record its IPv4 and IPv6 addresses, if present.

---

## 11. Practical Lab Questions

Answer the following:

1. What does IP stand for?
2. What is the difference between IPv4 and IPv6?
3. How many bits does an IPv4 address contain?
4. How many bits does an IPv6 address contain?
5. Give the three private IPv4 address ranges.
6. What is the purpose of `127.0.0.1`?
7. What does DHCP do?
8. What is the difference between a static and dynamic IP address?
9. What does an address beginning with `169.254` commonly indicate?
10. Which Linux command displays only IPv6 addresses?

---

## 12. Important Commands to Remember

```bash
ip addr
ip -4 addr
ip -6 addr
hostname -I
ip addr show lo
ping -4 -c 4 127.0.0.1
ping -6 -c 4 ::1
```

---

## 13. Lesson Summary

In this lesson, you learned:

- An IP address identifies a network interface for IP communication.
- IPv4 uses 32-bit addresses.
- IPv6 uses 128-bit addresses.
- Public IP addresses are globally routable.
- Private IPv4 addresses are used within private networks.
- Loopback addresses refer to the local machine.
- Static addresses are configured manually or reserved.
- Dynamic addresses are commonly assigned through DHCP.
- Subnet masks and CIDR identify network and host portions.
- Linux provides commands for inspecting IPv4 and IPv6 configuration.

### Key Definitions

**IP Address:** A logical address used to identify a network interface for IP communication.

**IPv4:** A 32-bit Internet Protocol addressing system.

**IPv6:** A 128-bit Internet Protocol addressing system.

**Private IP:** An IPv4 address reserved for private network use.

**Public IP:** An IP address that is globally routable on the public Internet.

**DHCP:** A protocol that automatically provides IP configuration to network clients.

**Loopback:** An address used by a computer to communicate with itself.

---

## 14. Knowledge Check

1. Why do devices need IP addresses?
2. Why was IPv6 introduced?
3. Can two devices on separate private networks use the same private IP address?
4. Is `192.168.1.10` a public or private IPv4 address?
5. Is `8.8.8.8` a private or public IPv4 address?
6. What is the IPv6 loopback address?
7. What happens when DHCP fails and a device self-assigns an IPv4 link-local address?
8. Which command displays IPv4 addresses only?

**End of Lesson 03 – Understanding IP Addresses**