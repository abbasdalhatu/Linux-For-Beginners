
# MODULE 13 – METASPLOITABLE LABS

# LESSON 04: HOST DISCOVERY AND ENUMERATION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain host discovery.
2. Understand network reconnaissance.
3. Identify devices on an isolated virtual network.
4. Use Nmap for basic host discovery.
5. Understand ARP-based discovery.
6. Identify the target's IP address and MAC address.
7. Perform basic service enumeration.
8. Document reconnaissance results professionally.

---

## 2. What Is Host Discovery?

Host discovery is the process of identifying active devices on a network.

Before assessing a system, a security professional needs to establish:

- Which devices are present?
- Which devices are reachable?
- What IP addresses do they use?
- Which device is the intended target?

In our lab, the objective is to identify one machine:

**Metasploitable 2**

Our testing workstation is:

**Kali Linux**

Both machines must remain on the same isolated Host-only network.

### Example

Suppose your virtual network contains:

| Device | IP Address |
|---|---|
| Kali Linux | 192.168.100.10 |
| Metasploitable | 192.168.100.20 |

Kali is the testing machine, and Metasploitable is the authorized target.

These are example addresses. Use the actual addresses displayed by your virtual machines.

---

## 3. What Is Network Reconnaissance?

Reconnaissance is the information-gathering stage of a security assessment.

It helps an assessor understand the target before performing deeper tests.

There are two broad categories.

### A. Passive Reconnaissance

Collecting information without directly interacting with the target in a probing manner.

Examples include:

- Reviewing authorized documentation.
- Studying network diagrams.
- Reviewing asset inventories.
- Examining previously collected evidence.

### B. Active Reconnaissance

Interacting with an authorized target to collect information.

Examples include:

- Host discovery.
- Port scanning.
- Service detection.
- Network connectivity checks.

Our practical exercises use active reconnaissance only against your own isolated Metasploitable VM.

---

## 4. Introduction to Nmap

Nmap stands for Network Mapper.

It is a widely used network discovery and security auditing tool.

Nmap can help identify:

- Active hosts.
- Open ports.
- Running services.
- Service versions.
- Operating system clues.
- Network characteristics.

Check whether Nmap is installed:

```bash
nmap --version
```

If installed, the command displays its version and build information.

### Understanding Nmap Syntax

A basic Nmap command follows this structure:

```bash
nmap [options] TARGET
```

Where:

- `nmap` starts the scanner.
- `options` determine what type of scan is performed.
- `TARGET` identifies the authorized machine.

Never replace your lab target with an unrelated public or university IP address.

---

## 5. Practical Lab 1 – Confirm Your Target Information

Open Kali Linux.

Check its IP address:

```bash
ip addr
```

Check the routing table:

```bash
ip route
```

Check the target's IP address using the information recorded during Lesson 03.

Create a variable containing the actual target address:

```bash
TARGET=192.168.100.20
```

Replace the example with your real Metasploitable IP.

Verify the variable:

```bash
echo "$TARGET"
```

This variable will help you avoid repeatedly typing the address.

**Important:** Do not copy the example IP if your Metasploitable machine uses a different address.

---

## 6. Practical Lab 2 – Test Basic Connectivity

Run:

```bash
ping -c 4 "$TARGET"
```

This sends four ICMP echo requests to your own target.

### Possible Results

**Successful connectivity**

You may see replies indicating that the target responds.

**No response**

Possible reasons include:

- Incorrect target IP.
- Target VM is powered off.
- Virtual adapter is disconnected.
- Different Host-only networks.
- ICMP traffic is filtered.

A failed ping does not prove that the machine is offline. Other discovery techniques may provide additional information.

Record your result.

---

## 7. Practical Lab 3 – Perform Basic Host Discovery

Run an Nmap host-discovery scan against the single target:

```bash
nmap -sn "$TARGET"
```

The `-sn` option performs host discovery without conducting a port scan.

Possible output:

```text
Starting Nmap

Nmap scan report for 192.168.100.20
Host is up

Nmap done
```

Your output may differ.

### Understand the Result

- `Nmap scan report`: identifies the scanned target.
- `Host is up`: indicates that Nmap considers the target reachable.
- `Nmap done`: indicates that the scan has finished.

Record the output in your reconnaissance notes.

---

## 8. Practical Lab 4 – Understand ARP Discovery

ARP means Address Resolution Protocol.

It helps devices on an IPv4 local network discover the MAC address associated with an IPv4 address.

For example:

```text
IP Address: 192.168.100.20
MAC Address: 00:0C:29:XX:XX:XX
```

The MAC address above is illustrative.

Check your ARP neighbor table:

```bash
ip neigh
```

You may see an entry for Metasploitable.

### Understanding the Output

| Field | Meaning |
|---|---|
| IP address | Neighbor's IPv4 address |
| lladdr | MAC address |
| dev | Local network interface |
| REACHABLE | Neighbor recently confirmed reachable |
| STALE | Existing entry that needs refreshing |
| FAILED | Neighbor resolution failed |

An empty neighbor table does not necessarily mean that no machines exist. It may simply mean Kali has not recently communicated with them.

### Optional Tool: arp-scan

Check whether it is installed:

```bash
arp-scan --version
```

If available, you may use it to inspect the local virtual subnet, but restrict the scan to the exact Host-only subnet confirmed in VMware.

Do not guess a subnet range.

If the tool is unavailable, the Nmap and `ip neigh` exercises are sufficient for this lesson.

---

## 9. Practical Lab 5 – Perform Basic Port Enumeration

Host discovery identifies a reachable machine.

Enumeration gathers additional information about the machine.

For this exercise, inspect only the 20 most common TCP ports on your own Metasploitable target:

```bash
nmap --top-ports 20 "$TARGET"
```

This is a limited TCP port scan.

### Understanding Port States

| Port State | Meaning |
|---|---|
| Open | A service is accepting connections |
| Closed | The host responded, but no service is listening |
| Filtered | Nmap cannot determine whether the port is open because filtering prevents a clear result |
| Open\|Filtered | Nmap cannot distinguish between an open port and a filtered port |
| Closed\|Filtered | Nmap cannot distinguish between a closed port and a filtered port |

### Example

```text
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
80/tcp   open  http
```

This is an illustrative example, not a prediction of your actual results.

Record the ports you observe.

Do not assume that every service is vulnerable merely because its port is open.

---

## 10. Practical Lab 6 – Identify Service Information

Perform a version-detection scan against the same target and a small selection of ports you observed:

```bash
nmap -sV -p 21,22,23,80 "$TARGET"
```

The `-sV` option attempts to identify the services and their versions.

Only include ports that are within your authorized lab scope.

### Example

```text
PORT   STATE SERVICE VERSION
21/tcp open  ftp     Example FTP service
22/tcp open  ssh     Example SSH service
80/tcp open  http    Example web service
```

Actual results depend on the target's installed services.

### Why Service Versions Matter

Version information helps security professionals:

- Identify outdated software.
- Research known vulnerabilities.
- Determine whether updates are required.
- Understand the target's technology.

A reported version alone does not prove a vulnerability. Further validation is necessary.

---

## 11. Practical Lab 7 – Save Your Enumeration Results

Create your lesson workspace:

```bash
mkdir -p ~/practical-labs/module13/lab04
cd ~/practical-labs/module13/lab04
```

Create a results file:

```bash
nano reconnaissance.txt
```

Use this structure:

```text
MODULE 13 – HOST DISCOVERY AND ENUMERATION

TARGET:
Metasploitable 2

TARGET IP:
Record actual IP

KALI IP:
Record actual IP

NETWORK:
Host-only

1. CONNECTIVITY TEST
Command:
Result:

2. HOST DISCOVERY
Command:
Result:

3. ARP INFORMATION
Command:
Result:

4. PORT ENUMERATION
Command:
Result:

5. SERVICE IDENTIFICATION
Command:
Result:

OBSERVATIONS:
Record important findings.

LIMITATIONS:
Record anything that could not be verified.
```

You can also save Nmap output directly:

```bash
nmap -sn "$TARGET" -oN host-discovery.txt
```

And save your limited port scan:

```bash
nmap --top-ports 20 "$TARGET" -oN port-scan.txt
```

These commands create text reports in your current directory.

---

## 12. Understanding Enumeration Results

A security assessor must interpret results carefully.

Consider the following example:

```text
Port: 21
Service: FTP
State: Open
```

What can you conclude?

- An FTP service appears to be listening.
- The target accepts connections on TCP port 21.
- Further assessment may be appropriate within the authorized scope.

What can you not conclude?

- That the FTP service is definitely exploitable.
- That its credentials are weak.
- That sensitive information is exposed.
- That the server has a particular vulnerability.

Good security work separates observed facts from assumptions.

---

## 13. Knowledge Check

Answer these questions:

1. What is host discovery?
2. What is network reconnaissance?
3. Explain the difference between passive and active reconnaissance.
4. What does Nmap do?
5. What does the `-sn` option mean?
6. What is the purpose of ARP?
7. What is a MAC address?
8. What does an open port indicate?
9. What does the `-sV` option do?
10. Why does an open port not automatically mean a vulnerability exists?

---

## 14. Lesson Summary

In this lesson, you learned:

- Host discovery.
- Active and passive reconnaissance.
- Nmap fundamentals.
- Basic connectivity testing.
- ARP and MAC address discovery.
- Limited port enumeration.
- Service version identification.
- Nmap output storage.
- Professional interpretation of scan results.

## 15. Professional References

1. Nmap Reference Guide: https://nmap.org/book/man.html
2. Nmap Host Discovery: https://nmap.org/book/man-host-discovery.html
3. Nmap Service and Version Detection: https://nmap.org/book/man-version-detection.html
4. RFC 826 – Address Resolution Protocol: https://www.rfc-editor.org/rfc/rfc826

---

# END OF LESSON 04
