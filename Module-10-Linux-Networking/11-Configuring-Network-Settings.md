---
---

# Module 10 – Lesson 11: Configuring Network Settings in Linux

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand Linux network configuration.
2. Explain static and dynamic IP addressing.
3. Understand DHCP.
4. Configure network settings using NetworkManager.
5. Understand temporary and persistent network configuration.
6. Configure DNS settings safely.
7. Inspect active network connections.
8. Understand common network configuration problems.

---

## 2. What Is Network Configuration?

Network configuration is the process of setting up a computer's network parameters so it can communicate with other devices.

These parameters include:

- IP address.
- Subnet prefix.
- Default gateway.
- DNS servers.
- Network interface.
- Routing information.

For example:

```text
IP Address:      192.168.1.50
Subnet Prefix:   /24
Default Gateway: 192.168.1.1
DNS Server:      1.1.1.1
```

These are illustrative values, not settings you should copy into your Kali VM.

---

## 3. Static vs Dynamic IP Addressing

There are two common methods of assigning IP addresses.

### 3.1 Static IP Addressing

A static IP address is manually configured and intended to remain consistent until changed.

Example:

```text
IP Address: 192.168.1.50
Prefix:     /24
Gateway:    192.168.1.1
DNS:        1.1.1.1
```

Advantages:

- Predictable addressing.
- Useful for servers and network infrastructure.
- Easier to identify specific devices.

Disadvantages:

- Requires manual configuration.
- Incorrect settings can cause connectivity problems.
- Duplicate addresses can create conflicts.

### 3.2 Dynamic IP Addressing

Dynamic IP addressing allows a device to receive network configuration automatically, commonly through DHCP.

Advantages:

- Easier network management.
- Reduces manual configuration.
- Helps avoid address-allocation mistakes.

Disadvantages:

- Addresses may change.
- DHCP service availability is required.
- Configuration depends on the DHCP server.

### Comparison

| Feature | Static IP | Dynamic IP |
|---|---|---|
| Assignment | Manual | Automatic |
| Address consistency | Usually fixed | May change |
| Administration | More manual | Easier for many clients |
| Common use | Servers, infrastructure | Client computers |

---

## 4. Understanding DHCP

DHCP stands for Dynamic Host Configuration Protocol.

It automatically provides network configuration to devices.

A DHCP server can provide:

- IP address.
- Subnet mask or prefix.
- Default gateway.
- DNS servers.
- Lease duration.

### DHCP DORA Process

DHCP commonly uses a four-step process known as DORA.

1. Discover
2. Offer
3. Request
4. Acknowledge

### DHCP Diagram

```text
Client                         DHCP Server
  |                                  |
  |---------- Discover ------------>|
  |                                  |
  |<----------- Offer ---------------|
  |                                  |
  |----------- Request ------------>|
  |                                  |
  |<---------- Acknowledge ----------|
  |                                  |
  |       IP Configuration Received  |
```

### Explanation

**Discover:** The client searches for an available DHCP server.

**Offer:** The server offers an IP configuration.

**Request:** The client requests the offered configuration.

**Acknowledge:** The server confirms the assignment.

DHCP normally uses UDP ports 67 and 68 for IPv4.

---

## 5. Understanding NetworkManager

NetworkManager is a Linux service used to manage network connections.

It can manage:

- Ethernet connections.
- Wi-Fi connections.
- Network interfaces.
- IP addresses.
- DNS settings.
- Connection profiles.

Kali Linux commonly uses NetworkManager for desktop networking.

### Check NetworkManager Status

```bash
systemctl status NetworkManager
```

### Display Network Devices

```bash
nmcli device status
```

### Display Active Connections

```bash
nmcli connection show --active
```

### Display All Connection Profiles

```bash
nmcli connection show
```

### Display Detailed Device Information

```bash
nmcli device show
```

These commands allow you to inspect network configuration without changing anything.

---

## 6. Understanding Temporary and Persistent Configuration

Linux network settings can be temporary or persistent.

### Temporary Configuration

A temporary configuration is applied while the system is running but may disappear after a reboot or connection reset.

Example:

```bash
sudo ip addr add 192.168.1.50/24 dev eth0
```

This adds an IPv4 address to the interface.

It does not automatically create a persistent NetworkManager profile.

### Persistent Configuration

Persistent configuration is saved in a network configuration profile or another system-managed configuration source.

It is intended to survive restarts.

For example, NetworkManager connection profiles can store:

- IP addressing method.
- Static IP address.
- Gateway.
- DNS servers.

### Important Difference

| Temporary | Persistent |
|---|---|
| Runtime configuration | Saved configuration |
| May disappear after restart | Designed to survive restart |
| Often applied directly with `ip` | Often managed through NetworkManager |
| Useful for testing | Useful for long-term configuration |

---

## 7. Configuring Network Settings with `ip`

The `ip` command can make temporary network configuration changes.

**Warning:** These examples are for learning. Do not execute them on your active VMware interface. Incorrect changes can disconnect your Kali VM.

### 7.1 Add an IP Address

```bash
sudo ip addr add 192.168.1.50/24 dev eth0
```

### 7.2 Remove an IP Address

```bash
sudo ip addr del 192.168.1.50/24 dev eth0
```

### 7.3 Enable an Interface

```bash
sudo ip link set eth0 up
```

### 7.4 Disable an Interface

```bash
sudo ip link set eth0 down
```

### 7.5 Inspect the Configuration

```bash
ip addr show
```

### 7.6 Inspect the Routing Table

```bash
ip route
```

Remember that `eth0` is only an example. Your Kali VM may use another interface name.

---

## 8. Managing Connections with nmcli

`nmcli` is the command-line interface for NetworkManager.

It allows administrators to inspect and manage network connections.

### 8.1 List Network Devices

```bash
nmcli device status
```

### 8.2 List Connection Profiles

```bash
nmcli connection show
```

### 8.3 Display Active Connections

```bash
nmcli connection show --active
```

### 8.4 Display a Specific Connection

```bash
nmcli connection show "CONNECTION_NAME"
```

Replace `CONNECTION_NAME` with the actual connection profile name.

### 8.5 Activate a Connection

```bash
sudo nmcli connection up "CONNECTION_NAME"
```

### 8.6 Deactivate a Connection

```bash
sudo nmcli connection down "CONNECTION_NAME"
```

**Important:** Activating or deactivating a connection may interrupt network access. Do not experiment with these two commands on your active VMware connection.

---

## 9. Understanding DHCP Configuration with NetworkManager

A NetworkManager connection profile can be configured to obtain its IPv4 configuration automatically.

Example syntax:

```bash
sudo nmcli connection modify "CONNECTION_NAME" ipv4.method auto
```

This sets the connection's IPv4 method to automatic.

To inspect the configured method:

```bash
nmcli connection show "CONNECTION_NAME"
```

Look for the `ipv4.method` setting.

A profile may use automatic DHCP configuration, while another may use manual addressing.

Do not change your active connection for this lesson.

---

## 10. Understanding Static IP Configuration with NetworkManager

NetworkManager can store static IP settings in a connection profile.

Example:

```bash
sudo nmcli connection modify "CONNECTION_NAME" ipv4.method manual
```

Configure an address:

```bash
sudo nmcli connection modify "CONNECTION_NAME" ipv4.addresses 192.168.1.50/24
```

Configure a gateway:

```bash
sudo nmcli connection modify "CONNECTION_NAME" ipv4.gateway 192.168.1.1
```

Configure DNS:

```bash
sudo nmcli connection modify "CONNECTION_NAME" ipv4.dns "1.1.1.1 8.8.8.8"
```

These commands modify a saved connection profile. They do not necessarily activate the new settings immediately.

**Do not copy these example addresses into your active VMware connection.** A static address must match the actual network and must not conflict with another device.

---

## 11. Understanding DNS Configuration

DNS settings determine which resolver your system uses.

Inspect DNS configuration:

```bash
cat /etc/resolv.conf
```

Inspect DNS information through NetworkManager:

```bash
nmcli device show
```

Look for DNS-related entries.

### Common Public DNS Resolver Addresses

| Provider | IPv4 Address |
|---|---|
| Google Public DNS | 8.8.8.8 |
| Google Public DNS | 8.8.4.4 |
| Cloudflare DNS | 1.1.1.1 |
| Cloudflare DNS | 1.0.0.1 |

These are examples of public DNS resolvers.

NetworkManager or another network service may automatically manage `/etc/resolv.conf`. Avoid editing it directly without understanding how your system manages DNS.

---

## 12. Understanding Network Configuration Problems

### 12.1 Duplicate IP Addresses

Two devices may accidentally receive or use the same IP address.

Possible symptoms:

- Intermittent connectivity.
- Address conflicts.
- Unexpected network behaviour.

### 12.2 Incorrect Subnet Prefix

An incorrect subnet prefix can cause the system to misidentify local and remote networks.

### 12.3 Incorrect Gateway

An incorrect default gateway can prevent communication with external networks.

### 12.4 Incorrect DNS Server

An unavailable or incorrect resolver may cause hostname-resolution failures.

### 12.5 DHCP Failure

A client may fail to obtain a valid address when DHCP is unavailable or blocked.

### 12.6 Interface Disabled

A disabled interface cannot perform normal network communication.

---

## 13. Network Configuration Security

Network configuration affects system security.

Important security practices include:

- Use trusted DHCP servers.
- Avoid unauthorized static address changes.
- Protect network administration privileges.
- Avoid unnecessary network interfaces.
- Use trusted DNS resolvers.
- Restrict management services.
- Monitor unexpected configuration changes.
- Keep network-management software updated.

In cybersecurity, unexpected changes to IP addresses, gateways, DNS servers, or routes may indicate misconfiguration or malicious activity.

---

## 14. Practical Lab: Inspecting Network Configuration

### Lab Objective

Use NetworkManager and Linux commands to investigate your Kali VM's current network configuration.

This lab is read-only. Do not modify your active connection.

### Step 1: Check NetworkManager

```bash
systemctl status NetworkManager
```

Record whether the service is active.

### Step 2: Display Network Devices

```bash
nmcli device status
```

Record your main network interface.

### Step 3: Display All Connections

```bash
nmcli connection show
```

Identify the connection profile associated with your VM.

### Step 4: Display Active Connections

```bash
nmcli connection show --active
```

Record the active connection.

### Step 5: Inspect the Connection Profile

```bash
nmcli connection show "CONNECTION_NAME"
```

Replace the placeholder with the actual profile name.

Observe:

- IPv4 method.
- IPv4 address.
- Gateway.
- DNS configuration.

### Step 6: Inspect IP Addresses

```bash
ip -br addr
```

Record the configured addresses.

### Step 7: Inspect Routes

```bash
ip route
```

Record the default route if one exists.

### Step 8: Inspect DNS Configuration

```bash
cat /etc/resolv.conf
```

Record the configured resolver information.

### Step 9: Inspect DNS Through NetworkManager

```bash
nmcli device show
```

Identify any DNS server entries.

### Step 10: Test Connectivity

```bash
ping -c 4 1.1.1.1
```

Then:

```bash
getent hosts example.com
```

Record both results.

---

## 15. Practical Analysis Table

| Configuration Item | Your Observation |
|---|---|
| NetworkManager status | |
| Main interface | |
| Connection profile | |
| IPv4 method | |
| IPv4 address | |
| Subnet prefix | |
| Default gateway | |
| DNS server | |
| External IP connectivity | |
| DNS resolution | |

---

## 16. Assessment Questions

Answer the following:

1. What is network configuration?
2. What is the difference between static and dynamic IP addressing?
3. What does DHCP stand for?
4. Explain the DHCP DORA process.
5. What is NetworkManager?
6. What does `nmcli device status` display?
7. What is the difference between temporary and persistent configuration?
8. Why can an incorrect gateway prevent internet access?
9. Why should you avoid modifying your active VMware network interface unnecessarily?
10. What is the purpose of DNS configuration?
11. What can happen when two devices use the same IP address?
12. Why is network configuration important to cybersecurity?

---

## 17. Important Commands to Remember

```bash
systemctl status NetworkManager

nmcli device status
nmcli device show
nmcli connection show
nmcli connection show --active

ip addr
ip route
ip link
ip neigh

cat /etc/resolv.conf

getent hosts example.com
ping -c 4 1.1.1.1
```

### Configuration Commands to Understand

```bash
sudo ip addr add 192.168.1.50/24 dev eth0

sudo ip link set eth0 up

sudo nmcli connection modify "CONNECTION_NAME" ipv4.method auto

sudo nmcli connection modify "CONNECTION_NAME" ipv4.method manual

sudo nmcli connection modify "CONNECTION_NAME" ipv4.addresses 192.168.1.50/24
```

These are examples for understanding syntax, not instructions to modify your active VM configuration.

---

## 18. Lesson Summary

In this lesson, you learned:

- Network configuration defines how a system communicates.
- Static addressing uses manually assigned IP settings.
- Dynamic addressing commonly uses DHCP.
- DHCP follows the Discover, Offer, Request, and Acknowledge process.
- NetworkManager manages network connections.
- `nmcli` provides command-line network administration.
- The `ip` command can make temporary network changes.
- NetworkManager profiles can store persistent configuration.
- DNS configuration determines which resolvers are used.
- Incorrect network settings can cause connectivity failures.
- Secure network configuration is important for system protection.

### Key Definitions

**Network Configuration:** The process of setting IP addresses, routes, DNS servers, and other network parameters.

**Static IP:** An IP address configured manually.

**Dynamic IP:** An IP address assigned automatically, commonly through DHCP.

**DHCP:** Dynamic Host Configuration Protocol.

**NetworkManager:** A Linux service used to manage network devices and connections.

**nmcli:** A command-line utility for managing NetworkManager.

**Persistent Configuration:** Network settings stored so they can survive system restarts.

**Temporary Configuration:** Runtime network settings that may disappear after a restart or configuration reset.

---

## 19. Knowledge Check

1. What does DHCP do?
2. What are the four stages of DORA?
3. Which Linux command manages NetworkManager from the terminal?
4. Which command displays active network connections?
5. What is the difference between `ip addr` and `nmcli connection show`?
6. What makes a network configuration persistent?
7. Why must a static IP address match its network?
8. Why should DNS settings be protected from unauthorized modification?

**End of Lesson 11 – Configuring Network Settings**