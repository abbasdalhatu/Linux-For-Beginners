
# MODULE 17 – ADVANCED LINUX
## LESSON 09: ADVANCED NETWORK ADMINISTRATION

---

## 1. Introduction to Advanced Network Administration

Linux provides powerful tools for configuring, monitoring, and troubleshooting computer networks.

Network administration involves managing how computers communicate with each other and access network resources.

As a Linux administrator or cybersecurity professional, you need to understand:

- Network interfaces.
- IP addressing.
- Routing tables.
- DNS resolution.
- Network sockets.
- Network ports.
- Firewall fundamentals.
- Network diagnostics.
- Packet inspection.
- Network monitoring.

### Learning objectives

By the end of this lesson, you should be able to:

1. Inspect Linux network interfaces.
2. Understand advanced IP configuration.
3. Examine routing tables.
4. Understand network sockets.
5. Inspect listening ports.
6. Troubleshoot DNS.
7. Examine network connections.
8. Understand packet capture.
9. Inspect network configuration.
10. Build a network information report.

---

## 2. Understanding Network Interfaces

A network interface is a hardware or virtual component that allows a computer to communicate over a network.

Examples include:

| Interface | Description |
|---|---|
| `eth0` | Traditional Ethernet interface name |
| `ens33` | Predictable Ethernet interface name |
| `enp0s3` | PCI-based Ethernet naming |
| `wlan0` | Traditional wireless interface name |
| `lo` | Loopback interface |
| `tun0` | Common tunnel interface |
| `virbr0` | Virtual bridge interface |

Names vary according to the operating system and network environment.

### Display network interfaces

```bash
ip link
```

Display IP addresses:

```bash
ip addr
```

Display detailed interface information:

```bash
ip -s link
```

The `-s` option displays interface statistics.

### Understanding interface states

Common states include:

- UP: Interface is administratively enabled.
- DOWN: Interface is administratively disabled.
- UNKNOWN: The interface state cannot be determined in the usual way.

An interface being administratively UP does not always mean that a working network connection exists.

---

## 3. Understanding Advanced IP Configuration

Linux uses IP addresses to identify network interfaces.

Two major IP versions are:

- IPv4.
- IPv6.

### IPv4 example

```text
192.168.1.10
```

IPv4 addresses use 32 bits.

### IPv6 example

```text
2001:db8::10
```

IPv6 addresses use 128 bits.

### Display IP configuration

```bash
ip addr show
```

Display IPv4 addresses:

```bash
ip -4 addr
```

Display IPv6 addresses:

```bash
ip -6 addr
```

Display the primary routing decision for an address:

```bash
ip route get 8.8.8.8
```

This command reports how the kernel would route traffic toward the destination.

### Inspect interface details

```bash
ip -details link
```

### Why this matters

Network administrators use IP configuration information to identify:

- Incorrect addresses.
- Missing gateways.
- Subnet problems.
- Interface failures.
- IPv4 and IPv6 configuration issues.

---

## 4. Understanding Routing Tables

A routing table tells Linux where to send network traffic.

When a computer wants to communicate with another device, the kernel uses routing information to determine the next destination.

### Display the routing table

```bash
ip route
```

Display IPv6 routes:

```bash
ip -6 route
```

### Example routing table

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link
```

Explanation:

| Component | Meaning |
|---|---|
| `default` | Default route |
| `via` | Next-hop gateway |
| `dev` | Network interface |
| `192.168.1.0/24` | Local network |
| `scope link` | Route applies directly to the link |

### Understanding the default gateway

A default gateway is the router used when no more specific route matches a destination.

For example:

```text
default via 192.168.1.1
```

This indicates that the system uses `192.168.1.1` as its default next hop.

### Inspect a routing decision

```bash
ip route get 1.1.1.1
```

This helps identify the selected interface, source address, and next hop.

---

## 5. Understanding Network Sockets

A network socket is an endpoint used by applications to communicate.

Sockets can use different communication protocols.

Common types include:

| Socket type | Purpose |
|---|---|
| TCP | Reliable, connection-oriented communication |
| UDP | Connectionless datagram communication |
| UNIX socket | Local inter-process communication |
| RAW socket | Direct access to certain network protocols |

### Understanding TCP

TCP provides:

- Connection establishment.
- Reliable delivery.
- Ordered data transmission.
- Flow control.
- Congestion control.

### Understanding UDP

UDP provides:

- Connectionless communication.
- Low protocol overhead.
- No built-in delivery guarantee.
- No built-in ordering guarantee.

### Inspect socket information

```bash
ss -s
```

Display TCP sockets:

```bash
ss -t
```

Display UDP sockets:

```bash
ss -u
```

Display listening sockets:

```bash
ss -l
```

Display processes associated with sockets:

```bash
sudo ss -tulpn
```

### Common options

| Option | Purpose |
|---|---|
| `-t` | TCP sockets |
| `-u` | UDP sockets |
| `-l` | Listening sockets |
| `-p` | Display process information |
| `-n` | Numeric addresses and ports |
| `-a` | All sockets |

---

## 6. Understanding Network Ports

A port identifies a particular application or service endpoint on a host.

TCP and UDP each have a 16-bit port-number space.

Ports range from 0 to 65535.

### Common ports

| Port | Protocol | Common service |
|---|---|---|
| 20/21 | TCP | FTP data/control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 143 | TCP | IMAP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |

These are conventional service ports, not proof that a particular service is running.

### Inspect listening ports

```bash
sudo ss -tulpn
```

Display listening TCP sockets:

```bash
sudo ss -ltnp
```

Display listening UDP sockets:

```bash
sudo ss -lunp
```

### Why port inspection matters

It helps administrators identify:

- Unexpected services.
- Applications listening on network interfaces.
- Unnecessary exposed services.
- Port conflicts.
- Potential security risks.

Only investigate systems and services you own or are authorized to administer.

---

## 7. Understanding DNS Resolution

DNS stands for Domain Name System.

It translates human-readable domain names into IP addresses.

Example:

```text
example.com → IP address
```

Without DNS, users would generally need to remember numerical addresses for network services.

### Inspect DNS configuration

```bash
cat /etc/resolv.conf
```

On systems using systemd-resolved, you can also inspect:

```bash
resolvectl status
```

This command may not be available on every Kali installation.

### Resolve a domain

```bash
getent hosts example.com
```

Use `dig` if installed:

```bash
dig example.com
```

Display only the answer section:

```bash
dig +short example.com
```

Use `nslookup` if installed:

```bash
nslookup example.com
```

### Understand DNS record types

| Record | Purpose |
|---|---|
| A | IPv4 address |
| AAAA | IPv6 address |
| MX | Mail exchange |
| CNAME | Canonical name alias |
| NS | Name server |
| TXT | Text information |
| PTR | Reverse DNS lookup |

### Example DNS queries

```bash
dig example.com A
```

```bash
dig example.com AAAA
```

```bash
dig example.com MX
```

---

## 8. Understanding Network Diagnostics

Network diagnostics help identify communication problems.

### A. ping

`ping` tests whether a destination responds to ICMP echo requests.

```bash
ping -c 4 127.0.0.1
```

Test your configured default gateway:

```bash
ip route
```

Identify the gateway, then test it if it is your own network device.

### B. tracepath

`tracepath` helps examine the network path toward a destination.

Check availability:

```bash
command -v tracepath
```

Example:

```bash
tracepath example.com
```

### C. curl

`curl` can test HTTP and HTTPS connectivity.

```bash
curl -I https://example.com
```

This retrieves response headers.

### D. getent

```bash
getent hosts example.com
```

This tests name resolution through the system's configured name-service mechanisms.

### E. ip route get

```bash
ip route get 8.8.8.8
```

This helps determine which route the kernel would select.

### Diagnostic order

A useful troubleshooting sequence is:

1. Inspect the interface.
2. Inspect the IP address.
3. Inspect the routing table.
4. Test local connectivity.
5. Test gateway connectivity.
6. Test DNS resolution.
7. Test application connectivity.

---

## 9. Understanding Packet Capture

Packet capture is the process of recording network packets for analysis.

It is commonly used for:

- Network troubleshooting.
- Protocol analysis.
- Security monitoring.
- Incident response.
- Performance investigations.

### Common packet capture tools

| Tool | Purpose |
|---|---|
| tcpdump | Command-line packet capture |
| Wireshark | Graphical packet analysis |
| tshark | Command-line Wireshark interface |

### Check tcpdump

```bash
command -v tcpdump
```

Install it if necessary:

```bash
sudo apt update
sudo apt install tcpdump
```

### List capture interfaces

```bash
sudo tcpdump -D
```

### Capture loopback traffic

```bash
sudo tcpdump -i lo -c 10
```

This captures up to ten packets on the loopback interface.

### Capture traffic for a specific protocol

```bash
sudo tcpdump -i lo -c 10 icmp
```

### Save a capture file

```bash
sudo tcpdump -i lo -c 20 -w lesson09-capture.pcap
```

### Read a saved capture

```bash
tcpdump -r lesson09-capture.pcap
```

### Important privacy principle

Packet captures may contain sensitive information.

Only capture traffic on systems and networks you own or have explicit permission to monitor. Avoid collecting other people's private communications.

---

## 10. Understanding Network Configuration Management

Linux network configuration may be managed by different services.

Common examples include:

- NetworkManager.
- systemd-networkd.
- Netplan.
- Traditional interface configuration files.

Kali commonly uses NetworkManager for desktop networking.

### Inspect NetworkManager

```bash
systemctl status NetworkManager
```

### Display connection information

```bash
nmcli connection show
```

### Display active connections

```bash
nmcli connection show --active
```

### Display device information

```bash
nmcli device status
```

### Display detailed device information

```bash
nmcli device show
```

These commands are for inspection.

Do not modify connection profiles during this lesson.

---

## 11. Understanding Firewall Fundamentals

A firewall controls network traffic according to defined rules.

Linux supports multiple firewall technologies.

Examples include:

- nftables.
- iptables.
- UFW.
- firewalld.

### Understanding nftables

`nftables` is the modern Linux packet-filtering framework.

Inspect its availability:

```bash
command -v nft
```

Display the current ruleset:

```bash
sudo nft list ruleset
```

An empty ruleset does not necessarily mean that no other filtering mechanism is active.

### Understanding UFW

UFW is a simplified firewall management tool.

Check whether it is installed:

```bash
command -v ufw
```

If available, inspect its status:

```bash
sudo ufw status verbose
```

### Important warning

This lesson is inspection-focused.

Do not enable, disable, flush, or modify firewall rules during this exercise. Incorrect firewall changes can interrupt networking or expose services.

---

## 12. Practical Lab: Network Inspection

We will inspect the network configuration of your Kali VM.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module17-lab/lesson09
cd ~/module17-lab/lesson09
```

### Step 2: Inspect network interfaces

```bash
ip link
```

### Step 3: Inspect IPv4 and IPv6 addresses

```bash
ip -4 addr
```

```bash
ip -6 addr
```

### Step 4: Inspect routing

```bash
ip route
```

### Step 5: Inspect socket statistics

```bash
ss -s
```

### Step 6: Inspect listening ports

```bash
sudo ss -tulpn
```

### Step 7: Inspect DNS configuration

```bash
cat /etc/resolv.conf
```

### Step 8: Test local connectivity

```bash
ping -c 4 127.0.0.1
```

### Step 9: Test DNS resolution

```bash
getent hosts example.com
```

### Step 10: Inspect NetworkManager

```bash
nmcli device status
```

### Step 11: Inspect firewall availability

```bash
command -v nft
```

If available:

```bash
sudo nft list ruleset
```

Record your observations.

---

## 13. Practical Project: Build a Network Information Report

Create a Bash script that gathers important network information.

### Step 1: Create the script

```bash
nano network-report.sh
```

Enter:

```bash
#!/bin/bash

REPORT="$HOME/module17-lab/lesson09/network-report.txt"

{
    echo "======================================"
    echo "       LINUX NETWORK REPORT"
    echo "======================================"

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "NETWORK INTERFACES:"
    ip link

    echo
    echo "IP ADDRESSES:"
    ip addr

    echo
    echo "ROUTING TABLE:"
    ip route

    echo
    echo "SOCKET STATISTICS:"
    ss -s

    echo
    echo "LISTENING SOCKETS:"
    ss -tulpn

    echo
    echo "DNS CONFIGURATION:"
    cat /etc/resolv.conf

    echo
    echo "NETWORK CONNECTIONS:"
    nmcli connection show --active

    echo
    echo "======================================"
    echo "          REPORT COMPLETE"
    echo "======================================"

} > "$REPORT" 2>&1

echo "Network report saved to: $REPORT"
```

### Step 2: Make it executable

```bash
chmod +x network-report.sh
```

### Step 3: Run the script

```bash
./network-report.sh
```

### Step 4: Read the report

```bash
cat ~/module17-lab/lesson09/network-report.txt
```

### Step 5: Verify the report

```bash
ls -lh ~/module17-lab/lesson09/
```

The report should include interfaces, IP addresses, routes, sockets, DNS configuration, and active NetworkManager connections.

---

## 14. Network Security Best Practices

Advanced network administration must include security.

Important practices include:

### A. Minimize exposed services

Run only the network services that are required.

### B. Monitor listening ports

Regularly inspect listening sockets to identify unexpected services.

### C. Use secure protocols

Prefer SSH, HTTPS, and other properly configured encrypted protocols over insecure alternatives.

### D. Apply firewall policies

Allow only necessary traffic according to your network's requirements.

### E. Monitor DNS configuration

Investigate unexpected DNS resolver changes.

### F. Capture traffic responsibly

Protect packet captures because they may contain sensitive information.

### G. Maintain accurate network documentation

Record network interfaces, IP addresses, gateways, DNS resolvers, and authorized services.

---

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is a network interface?
2. Explain the difference between IPv4 and IPv6.
3. What is the purpose of a routing table?
4. What is a default gateway?
5. Define a network socket.
6. Explain the difference between TCP and UDP.
7. What is a network port?
8. What does the `ss` command do?
9. What is DNS?
10. Explain four DNS record types.
11. What is the purpose of `tcpdump`?
12. What is packet capture?
13. Name three Linux firewall technologies.
14. Why should administrators monitor listening ports?
15. Why must packet captures be handled carefully?

---

## 16. Important Commands Summary

| Command | Purpose |
|---|---|
| `ip link` | Inspect network interfaces |
| `ip addr` | Display IP addresses |
| `ip -4 addr` | Display IPv4 addresses |
| `ip -6 addr` | Display IPv6 addresses |
| `ip route` | Display routing table |
| `ip route get ADDRESS` | Inspect a routing decision |
| `ss -s` | Display socket statistics |
| `ss -tulpn` | Inspect listening sockets and processes |
| `getent hosts DOMAIN` | Resolve a hostname |
| `dig DOMAIN` | Perform DNS queries |
| `curl -I URL` | Inspect HTTP response headers |
| `ping -c 4 ADDRESS` | Test ICMP connectivity |
| `tracepath DOMAIN` | Inspect network path |
| `tcpdump -D` | List capture interfaces |
| `tcpdump -i lo -c 10` | Capture loopback packets |
| `nmcli device status` | Inspect network devices |
| `nmcli connection show` | List connection profiles |
| `nft list ruleset` | Inspect nftables rules |

---

## 17. Lesson Summary

In this lesson, you studied advanced Linux network administration.

You learned:

- Network interfaces.
- IPv4 and IPv6 inspection.
- Routing tables.
- Network sockets.
- Port inspection.
- DNS resolution.
- Network troubleshooting.
- Packet capture.
- NetworkManager.
- Firewall fundamentals.
- Network security practices.

You also created a network information reporting script.

### Completion checklist

- [ ] Inspect network interfaces.
- [ ] Inspect IPv4 and IPv6 configuration.
- [ ] Understand routing tables.
- [ ] Inspect listening ports.
- [ ] Understand TCP and UDP sockets.
- [ ] Perform DNS lookups.
- [ ] Use network diagnostic commands.
- [ ] Understand packet capture.
- [ ] Inspect NetworkManager.
- [ ] Understand Linux firewall technologies.
- [ ] Build the network-report script.
- [ ] Review network security practices.

**END OF LESSON 09**
