
# MODULE 10 – NETWORK ENUMERATION

# LESSON 02: HOST DISCOVERY AND NETWORK MAPPING

## 1. Introduction

Before investigating services and open ports, a security professional needs to understand which devices exist within the authorized network.

This process is called **host discovery**.

Host discovery helps identify active devices, while network mapping organizes those devices into a visual or documented representation of the network.

For example, a small lab network might contain:

```text
             LAB NETWORK
                  |
          Virtual Network
                  |
          +-------+-------+
          |               |
       Kali VM         Target VM
      192.168.56.10    192.168.56.20
```

These addresses are examples only.

## 2. What Is Host Discovery?

Host discovery is the process of identifying devices that are reachable within a specified network.

A host may be:

- A laptop.
- A desktop computer.
- A server.
- A virtual machine.
- A printer.
- A network appliance.

Host discovery does not necessarily identify every device. Firewalls, routing rules, sleeping devices, and network isolation can affect the results.

## 3. What Is Network Mapping?

Network mapping is the process of documenting discovered devices and their relationships.

A network map may contain:

1. Hostnames.
2. IP addresses.
3. Network interfaces.
4. Network ranges.
5. Default gateways.
6. Discovered services.
7. Device relationships.

### Example Network Inventory

| Host | IP Address | Status | Notes |
|---|---|---|---|
| Kali VM | 192.168.56.10 | Active | Analysis machine |
| Target VM | 192.168.56.20 | Active | Authorized lab target |
| Test Server | 192.168.56.30 | Unknown | Not yet investigated |

These are illustrative entries, not actual discoveries.

## 4. Methods of Host Discovery

### A. ICMP Discovery

ICMP is used by tools such as `ping` to test network reachability.

Example:

```bash
ping -c 4 127.0.0.1
```

This tests your own loopback interface.

Important: A failed ping does not prove a host is offline. A firewall may block ICMP.

### B. ARP Discovery

ARP helps IPv4 devices discover the MAC address associated with an IP address on a local network.

The neighbour table can be inspected using:

```bash
ip neigh
```

An ARP entry may provide information about a device recently contacted on the local network.

### C. TCP-Based Discovery

TCP connection attempts can help establish whether a particular service is reachable.

For example, a connection test to a local service may show whether it accepts connections.

### D. Passive Discovery

Passive discovery observes traffic that already exists instead of generating probes.

Tools include:

- Wireshark.
- TShark.
- Network monitoring systems.

Passive discovery is useful for observing authorized network activity.

## 5. Understanding IP Network Ranges

Before discovering hosts, you must understand the network range.

Example:

```text
IP Address: 192.168.56.10
Subnet Mask: 255.255.255.0
CIDR: /24
```

The `/24` network commonly represents:

```text
192.168.56.0/24
```

This network has:

- Network address: 192.168.56.0
- Usable host addresses: 192.168.56.1–192.168.56.254
- Broadcast address: 192.168.56.255

These values assume a conventional IPv4 `/24` subnet.

Inspect your own network configuration:

```bash
ip -4 address
```

Inspect routes:

```bash
ip route
```

Never assume your network uses the example subnet. Use your actual configuration.

## 6. Practical Lab 1: Identify Your Own Host

Create the workspace:

```bash
mkdir -p ~/practical-labs/module10/lab02
cd ~/practical-labs/module10/lab02
```

Identify your hostname:

```bash
hostname
```

Display detailed hostname information:

```bash
hostnamectl
```

Inspect your IP addresses:

```bash
ip address
```

Inspect the routing table:

```bash
ip route
```

Inspect known neighbours:

```bash
ip neigh
```

Save the outputs:

```bash
hostnamectl > hostname-information.txt
ip address > ip-addresses.txt
ip route > routing-information.txt
ip neigh > neighbour-information.txt
```

## 7. Practical Lab 2: Test Loopback Connectivity

The loopback address allows a computer to communicate with itself.

IPv4 loopback:

```text
127.0.0.1
```

IPv6 loopback:

```text
::1
```

Test IPv4:

```bash
ping -c 4 127.0.0.1
```

Test IPv6:

```bash
ping -6 -c 4 ::1
```

Expected observations may include:

- Four transmitted packets.
- Four received packets.
- Packet loss percentage.
- Round-trip time.

Results depend on whether the relevant protocol is enabled and functioning.

### Questions

1. What is a loopback address?
2. Why is loopback useful for testing?
3. Why might a remote host fail to respond to ping?
4. Does ping identify all services running on a host?

## 8. Practical Lab 3: Observe Local Network Neighbours

Display the neighbour table:

```bash
ip neigh show
```

If entries are missing, you may generate harmless traffic to your configured local gateway, but only within your own authorized network.

First inspect the default route:

```bash
ip route
```

If a default gateway is listed, note its address.

You may then test reachability to that gateway:

```bash
ping -c 2 GATEWAY_IP
```

Replace `GATEWAY_IP` with the actual gateway address.

Inspect the neighbour table again:

```bash
ip neigh show
```

Record:

- Neighbour IP.
- MAC address, if learned.
- Neighbour state.
- Interface.

Possible neighbour states include:

| State | Meaning |
|---|---|
| REACHABLE | Recently confirmed reachable |
| STALE | Entry exists but has not recently been confirmed |
| DELAY | Reachability confirmation is delayed |
| PROBE | Reachability is being checked |
| FAILED | Resolution or reachability failed |

Not every state will appear in your environment.

## 9. Practical Lab 4: Passive Host Discovery with Wireshark

Open Wireshark:

```bash
wireshark
```

Select an authorized interface that carries traffic.

Start a short capture and observe normal network activity.

Useful display filters:

```text
arp
```

```text
icmp
```

```text
ip
```

```text
ipv6
```

```text
dhcp
```

Observe:

- Source IP addresses.
- Destination IP addresses.
- Source MAC addresses where available.
- ARP requests and responses.
- DHCP activity where present.

Do not capture other people's traffic or inspect private communications without authorization.

Save the capture as:

```text
host-discovery-observation.pcapng
```

## 10. Practical Lab 5: Understand Nmap Host Discovery

Nmap is a network discovery and security auditing tool.

It can help identify hosts and services within an authorized assessment scope.

Check whether Nmap is installed:

```bash
nmap --version
```

If it is unavailable, use the official Kali package manager:

```bash
sudo apt update
sudo apt install nmap
```

### Test 1: Discover Your Own Local Host

```bash
nmap -sn 127.0.0.1
```

The `-sn` option performs host discovery without a port scan.

This is a test against your own loopback address.

### Test 2: Inspect a Host You Own

For an additional test, use the IP address of a second lab VM that you control and have explicitly authorized for assessment.

```bash
nmap -sn TARGET_IP
```

Replace `TARGET_IP` with the actual authorized target address.

Do not run this against an unknown network or a public IP address.

### Important Notes

- A host may be active even if Nmap reports it as down.
- Firewalls can block discovery probes.
- NAT can affect host visibility.
- Host-only networks provide useful isolation for VM-to-VM exercises.

## 11. Understanding NAT and Host-Only Networking

In VMware, different network modes affect host discovery.

### NAT

NAT allows the VM to access external networks through the host's network connection.

It is useful for:
- Package updates.
- Downloading authorized tools.
- General internet access.

### Host-Only

Host-only networking creates a private virtual network between the host and connected virtual machines, depending on configuration.

It is useful for:
- Isolated lab exercises.
- Kali-to-target communication.
- Controlled network enumeration.

For this course, prefer host-only networking for exercises involving deliberately vulnerable target machines.

Do not change your VMware network settings unless the lab requires it.

## 12. Practical Lab 6: Create a Network Map

Create a file:

```bash
nano network-map.txt
```

Use this structure:

```text
========================================
NETWORK MAP – MODULE 10
========================================

ASSESSMENT NAME:
Host Discovery and Network Mapping

DATE:

1. ANALYSIS MACHINE

Hostname:
Operating System:
IP Address:
Network Interface:

2. NETWORK INFORMATION

Network CIDR:
Subnet Mask:
Default Gateway:
Network Mode:

3. DISCOVERED HOSTS

Host 1:
IP Address:
Discovery Method:
Observed Status:

Host 2:
IP Address:
Discovery Method:
Observed Status:

4. OBSERVED PROTOCOLS

ARP:
ICMP:
IPv4:
IPv6:
DHCP:

5. NETWORK RELATIONSHIPS

Describe how the devices connect.

6. SECURITY OBSERVATIONS

Unexpected Devices:
Unknown Addresses:
Discovery Limitations:

7. CONCLUSION

Summary:
Recommendations:

========================================
END OF NETWORK MAP
========================================
```

Use actual observations. If you have only discovered your Kali VM, record only that device.

## 13. Common Host Discovery Problems

### Problem 1: Ping Fails

Possible explanations:
- The host is offline.
- ICMP is blocked.
- The route is unavailable.
- The target address is incorrect.

### Problem 2: No Neighbour Entries

Possible explanations:
- No local communication has occurred.
- The neighbour cache has expired.
- The destination is not on the local subnet.

### Problem 3: Nmap Reports Host Down

Possible explanations:
- Discovery probes are filtered.
- The target is offline.
- NAT or routing prevents communication.
- The target does not respond to the selected probes.

### Problem 4: Wireshark Shows No Traffic

Check:
- Correct interface selection.
- Whether the interface is active.
- Whether traffic is being generated.
- Whether the capture has started.

## 14. Knowledge Check

Answer these questions:

1. What is host discovery?
2. What is network mapping?
3. What is ICMP discovery?
4. What is ARP used for?
5. What is passive discovery?
6. What does the Nmap `-sn` option do?
7. What is the difference between NAT and host-only networking?
8. Why does a failed ping not necessarily mean a host is offline?
9. What is a network CIDR?
10. Why should host discovery be performed only within an authorized scope?

## 15. Lesson Completion Checklist

- [ ] Understand host discovery.
- [ ] Understand network mapping.
- [ ] Understand ICMP discovery.
- [ ] Understand ARP discovery.
- [ ] Understand passive discovery.
- [ ] Inspect your own host information.
- [ ] Test loopback connectivity.
- [ ] Inspect neighbour entries.
- [ ] Observe network traffic in Wireshark.
- [ ] Test Nmap against your own loopback address.
- [ ] Understand NAT and host-only networking.
- [ ] Create a network map.

## 16. Lesson Summary

Host discovery is an important stage of network enumeration.

It helps security professionals identify devices and understand the structure of an authorized network.

Remember:

- Host discovery identifies reachable devices.
- Network mapping documents network structure.
- ICMP helps test connectivity.
- ARP resolves local IPv4 addresses to MAC addresses.
- Passive discovery observes existing traffic.
- Nmap can perform host discovery.
- VMware host-only networking is useful for isolated cybersecurity labs.

**NEXT LESSON: NMAP FUNDAMENTALS.**
