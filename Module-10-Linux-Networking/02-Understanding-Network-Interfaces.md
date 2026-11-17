---
---

# Module 10 – Lesson 02: Understanding Network Interfaces

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define a network interface.
2. Differentiate between physical and virtual interfaces.
3. Understand common Linux network interface names.
4. Identify active and inactive interfaces.
5. Understand MAC addresses.
6. Inspect network interfaces using Linux commands.
7. Explain the importance of network interfaces in cybersecurity.

---

## 2. What Is a Network Interface?

A network interface is a connection point that allows a computer to communicate with another device over a network.

Think of it as a communication door through which information enters and leaves your computer.

For example:

- Ethernet allows a computer to connect using a network cable.
- Wi-Fi allows a computer to connect wirelessly.
- A virtual network interface allows a virtual machine to communicate through a software-created network connection.

Without a network interface, a computer cannot communicate through that network connection.

---

## 3. Types of Network Interfaces

Linux supports different types of network interfaces.

### 3.1 Ethernet Interface

An Ethernet interface provides wired network connectivity.

Examples of interface names:

```text
eth0
ens33
enp0s3
```

A physical computer may use an Ethernet cable connected to a router or switch.

### 3.2 Wireless Interface

A wireless interface provides Wi-Fi connectivity.

Common names include:

```text
wlan0
wlp2s0
```

Wireless interfaces allow devices to connect to Wi-Fi networks without Ethernet cables.

### 3.3 Loopback Interface

The loopback interface allows a computer to communicate with itself.

Its standard interface name is:

```text
lo
```

Its commonly used IPv4 address is:

```text
127.0.0.1
```

The loopback interface is useful for testing local networking services.

Example:

```bash
ping -c 4 127.0.0.1
```

This tests communication with your own machine.

### 3.4 Virtual Network Interface

A virtual interface is created through software rather than a separate physical network card.

Examples include:

- VMware virtual network adapters.
- Docker bridge interfaces.
- Linux bridge interfaces.
- Virtual Ethernet pairs.
- VPN tunnel interfaces.

In your VMware Kali environment, the virtual machine normally uses a virtual Ethernet adapter.

---

## 4. Understanding Linux Network Interface Names

Older Linux systems commonly used names such as:

```text
eth0
eth1
wlan0
```

Modern Linux systems may use predictable network interface names.

Examples:

| Interface | Meaning |
|---|---|
| `lo` | Loopback interface |
| `eth0` | Traditional Ethernet name |
| `ens33` | Ethernet interface using a predictable name |
| `enp0s3` | Ethernet interface identified by its bus and slot |
| `wlan0` | Traditional wireless interface |
| `wlp2s0` | Wireless interface using a predictable name |
| `tun0` | Common VPN tunnel interface |
| `docker0` | Common Docker bridge interface |

Your Kali VM may use `eth0`, `ens33`, or another name.

Do not assume every Linux computer uses the same interface names.

---

## 5. Understanding MAC Addresses

A MAC address is a link-layer address associated with a network interface.

MAC means Media Access Control.

Example:

```text
00:0C:29:AB:CD:EF
```

A MAC address is normally represented as six groups of hexadecimal characters.

### What Is It Used For?

MAC addresses help devices identify network interfaces within a local Ethernet network.

For example:

When Computer A sends an Ethernet frame to Computer B on the same local network, the frame uses the destination MAC address of Computer B.

### IP Address vs MAC Address

| IP Address | MAC Address |
|---|---|
| Logical network address | Link-layer address |
| Used for IP communication and routing | Used for local link delivery |
| Can change when network configuration changes | Often assigned to the network interface |
| Example: `192.168.1.10` | Example: `00:0C:29:AB:CD:EF` |

Important: MAC addresses can be changed or spoofed through software. They should not be treated as strong authentication credentials.

---

## 6. Understanding Interface States

Network interfaces can have different operational states.

Common states include:

### UP

The interface is administratively enabled.

### DOWN

The interface is administratively disabled.

### UNKNOWN

The kernel or network driver cannot provide a definitive operational state.

This state is not automatically evidence of a problem. It is common for some virtual interfaces and loopback interfaces.

### LOWER_UP

The interface's lower-level connection is detected as available.

For example, an Ethernet interface may report LOWER_UP when its link is connected.

---

## 7. Linux Commands for Inspecting Network Interfaces

### Command 1: Display All Interfaces

```bash
ip link
```

Displays network interface names and link-layer information.

### Command 2: Display IP Addresses

```bash
ip addr
```

Displays IP addresses assigned to network interfaces.

### Command 3: Display Only Active Interfaces

```bash
ip link show up
```

Displays interfaces that are administratively enabled.

### Command 4: Display Interface Statistics

```bash
ip -s link
```

Displays network interface statistics, including received and transmitted packets.

### Command 5: Display MAC Addresses

```bash
ip link show
```

Look for the `link/ether` field.

Example:

```text
link/ether 00:0c:29:ab:cd:ef
```

### Command 6: Display Interface Information Using sysfs

```bash
ls /sys/class/net/
```

This lists network interfaces known to the Linux kernel.

### Command 7: Inspect a Specific Interface

```bash
ip addr show eth0
```

Replace `eth0` with your actual interface name.

---

## 8. Understanding Network Interface Statistics

Run:

```bash
ip -s link
```

You may see output similar to:

```text
RX: bytes  packets  errors  dropped
TX: bytes  packets  errors  dropped
```

### RX – Receive

Information received by the interface.

### TX – Transmit

Information sent by the interface.

### Errors

Packets that encountered errors.

### Dropped

Packets discarded by the interface or networking subsystem.

High error or drop counts may indicate a problem, but they need context and investigation before drawing conclusions.

---

## 9. VMware Network Interfaces

Your Kali Linux machine runs inside VMware.

VMware provides a virtual network adapter to the guest operating system.

The adapter connects your Kali VM to a virtual network.

Common VMware networking modes include:

### NAT

Network Address Translation.

The virtual machine shares the host's external network connection through VMware's NAT service.

Useful for:

- Downloading Linux updates.
- Accessing websites.
- Downloading legitimate tools.

### Bridged Networking

The VM connects to the network as a separate device through the physical network adapter.

This can expose the VM directly to the local network.

Use it only when necessary and authorized.

### Host-Only Networking

Creates a private network between the host and virtual machines connected to that network.

Useful for isolated practice environments.

For your current Linux lessons, NAT is suitable for ordinary Internet connectivity checks. A host-only network will be useful later for isolated lab communication.

---

## 10. Practical Lab: Inspecting Network Interfaces

### Lab Objective

Identify your Kali VM's network interfaces and examine their properties.

All commands in this lab are observational. You do not need to change any interface settings.

### Step 1: List Network Interfaces

```bash
ip link
```

Record every interface name.

### Step 2: Display IP Addresses

```bash
ip addr
```

Identify which interface has an IPv4 address.

### Step 3: List Interfaces Through sysfs

```bash
ls /sys/class/net/
```

Compare the result with `ip link`.

### Step 4: Inspect Interface Statistics

```bash
ip -s link
```

Record the RX and TX packet counts for your main interface.

### Step 5: Inspect the Loopback Interface

```bash
ip addr show lo
```

Identify its IPv4 address.

### Step 6: Inspect Your Main Network Interface

Replace `eth0` with your actual interface name:

```bash
ip addr show eth0
```

Record:

- Interface name.
- Interface state.
- MAC address.
- IPv4 address, if assigned.

### Step 7: Test the Loopback Interface

```bash
ping -c 4 127.0.0.1
```

Observe whether all four packets receive replies.

### Step 8: Display Active Interfaces

```bash
ip link show up
```

Compare the output with the complete interface list.

---

## 11. Practical Lab Questions

Answer these in your notebook:

1. What is a network interface?
2. What is the name of your main Kali network interface?
3. What is the name of the loopback interface?
4. What is the MAC address of your main interface?
5. Is your main interface UP or DOWN?
6. What is the difference between RX and TX?
7. What is a virtual network interface?
8. What is the difference between NAT and Host-Only networking in VMware?
9. Why should MAC addresses not be treated as strong authentication credentials?
10. What does `ip -s link` display?

---

## 12. Important Commands to Remember

```bash
ip link
ip addr
ip link show up
ip -s link
ip addr show lo
ls /sys/class/net/
ping -c 4 127.0.0.1
```

---

## 13. Lesson Summary

In this lesson, you learned:

- A network interface connects a computer to a network.
- Ethernet and Wi-Fi are common physical network interfaces.
- Loopback provides communication within the same computer.
- Virtual interfaces are created through software.
- MAC addresses identify interfaces at the link layer.
- Linux interfaces can have different operational states.
- The `ip` command provides detailed network interface information.
- VMware provides virtual networking modes such as NAT, Bridged, and Host-Only.

### Key Definitions

**Network Interface:** A connection point that enables network communication.

**MAC Address:** A link-layer address associated with a network interface.

**Loopback:** A virtual interface used for communication with the same computer.

**RX:** Data received by a network interface.

**TX:** Data transmitted by a network interface.

**Virtual Network Adapter:** A software-created network adapter presented to a virtual machine.

---

## 14. Knowledge Check

1. What is a network interface?
2. What does `lo` represent?
3. What is the purpose of a MAC address?
4. What is the difference between `eth0` and `wlan0`?
5. Which command displays network interface statistics?
6. What does UP mean?
7. What is the purpose of VMware NAT?
8. Why is Host-Only networking useful for cybersecurity labs?

**End of Lesson 02 – Understanding Network Interfaces**