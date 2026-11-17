---
---

# MODULE 16 – PENETRATION TESTING
## LESSON 05: NETWORK SCANNING FUNDAMENTALS

## 1. Introduction

Network scanning is the process of examining an authorized network or system to identify devices, network services, and potential security weaknesses.

During penetration testing, scanning helps security professionals understand which systems are reachable and what services they expose.

For example, an organization may have:
- Web servers.
- Database servers.
- File servers.
- Workstations.
- Network printers.
- Firewalls.

A network scan can help identify devices and services that require further security assessment.

**Important:** Only scan systems and networks that you own or have explicit permission to assess.

---

## 2. What Is Network Scanning?

Network scanning involves sending or analyzing network traffic to obtain information about devices and services.

Common objectives include:

1. Discovering active hosts.
2. Identifying open ports.
3. Identifying network services.
4. Understanding network exposure.
5. Supporting vulnerability assessments.
6. Verifying firewall configurations.

Network scanning is one stage of a larger penetration testing methodology. It does not, by itself, prove that a system is vulnerable.

## 3. Types of Network Scanning

### A. Host Discovery

Host discovery determines which devices appear reachable on a network.

For example, a network may contain:
- A Linux server.
- A Windows workstation.
- A network printer.
- A virtual machine.

Host discovery helps build an inventory of reachable devices.

### B. Port Scanning

Port scanning examines TCP or UDP ports to determine their observed states.

A port represents a communication endpoint associated with a network service.

Common examples:

| Port | Common Service |
|---|---|
| 20 | FTP data |
| 21 | FTP control |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3306 | MySQL |
| 3389 | RDP |

A service does not necessarily run on its conventional port, and the presence of an open port does not automatically indicate a vulnerability.

### C. Service Discovery

Service discovery attempts to identify the application or protocol associated with an accessible port.

Examples:
- SSH.
- HTTP.
- HTTPS.
- DNS.
- SMTP.

### D. Operating System Detection

Operating system detection attempts to estimate which operating system is running on a target.

Possible results include:
- Linux.
- Windows.
- FreeBSD.
- Other operating systems.

Detection is not always exact. Firewalls, network devices, and configuration differences can affect results.

---

## 4. Understanding Network Ports

A port is a logical communication endpoint used by network applications.

TCP and UDP use port numbers from 0 to 65535.

### Port Categories

| Range | Category |
|---|---|
| 0–1023 | Well-known ports |
| 1024–49151 | Registered ports |
| 49152–65535 | Dynamic/private ports |

### Example

When you visit an HTTPS website, your browser normally connects to the server using TCP port 443.

The connection can be represented as:

```text
YOUR COMPUTER
     |
     | TCP connection
     |
     v
REMOTE WEB SERVER
     |
     +---- PORT 443
             |
             v
           HTTPS
```

---

## 5. Understanding Port States

A scanning tool may report several port states.

| State | Meaning |
|---|---|
| Open | An application is accepting connections |
| Closed | The port is reachable, but no application is listening |
| Filtered | A firewall or network filter prevents a clear determination |
| Unfiltered | The port is reachable, but its open/closed state is not established |
| Open\|Filtered | The tool cannot distinguish between open and filtered |
| Closed\|Filtered | The tool cannot distinguish between closed and filtered |

### Important Difference

An open port does not necessarily mean the system is insecure.

For example:
- SSH may be open and securely configured.
- HTTPS may be open and properly protected.
- An unnecessary Telnet service may create a security risk.

Security assessment requires understanding the service, configuration, exposure, and business purpose.

---

## 6. Introduction to Nmap

Nmap stands for Network Mapper.

It is a widely used network discovery and security auditing tool.

Security professionals use Nmap to:
- Discover hosts.
- Examine ports.
- Identify services.
- Estimate operating systems.
- Review network exposure.

Nmap should be used only within an approved testing scope.

### Check Whether Nmap Is Installed

In Kali:

```bash
nmap --version
```

If it is not installed:

```bash
sudo apt update
sudo apt install nmap
```

### Display Help

```bash
nmap --help
```

### Display the Manual

```bash
man nmap
```

Press `q` to exit the manual.

---

## 7. Basic Nmap Concepts

The following commands are examples for an isolated lab. Do not replace the documentation address with an unrelated or public target.

### A. Scan a Local Lab Target

After confirming the target's address and isolation, a basic scan can be performed:

```bash
nmap <AUTHORIZED-LAB-IP>
```

Replace the placeholder only with the IP address of your authorized lab target.

This performs a basic scan using Nmap's default scanning behavior.

### B. Examine Specific Ports

```bash
nmap -p 22,80,443 <AUTHORIZED-LAB-IP>
```

This checks only the listed ports.

### C. Examine a Port Range

```bash
nmap -p 1-1000 <AUTHORIZED-LAB-IP>
```

This checks ports 1 through 1000.

### D. Identify Services and Versions

```bash
nmap -sV <AUTHORIZED-LAB-IP>
```

This attempts to identify services and their versions.

Service detection sends additional probes, so use it only when the testing agreement permits it.

### E. Save Scan Results

```bash
nmap -oN scan-results.txt <AUTHORIZED-LAB-IP>
```

This saves output in normal text format.

**Do not run these commands until you have verified the lab target's IP address and network isolation.**

---

## 8. TCP and UDP Scanning

### TCP Scanning

TCP is a connection-oriented transport protocol.

It is commonly used by:
- HTTP.
- HTTPS.
- SSH.
- FTP.
- SMTP.

### UDP Scanning

UDP is a connectionless transport protocol.

It is commonly used by:
- DNS.
- DHCP.
- NTP.
- Some streaming and voice applications.

TCP and UDP scans may produce different results because the protocols behave differently.

UDP scanning can also be slower or harder to interpret because many UDP services do not respond to every probe.

---

## 9. What Is a Network Scan Result?

A scan result is information collected by a scanning tool.

A basic result may contain:

| Field | Meaning |
|---|---|
| Host | Identified target |
| State | Whether the host appears reachable |
| Port | Network communication endpoint |
| Protocol | TCP or UDP |
| Service | Application or protocol |
| Version | Estimated software version |

### Example of a Hypothetical Result

```text
PORT     STATE  SERVICE
22/tcp   open   ssh
80/tcp   open   http
443/tcp  closed https
```

Interpretation:

- TCP port 22 appears open.
- TCP port 80 appears open.
- TCP port 443 appears closed.

This example does not describe a real target.

The tester should investigate whether exposed services are necessary and securely configured.

---

## 10. Factors That Affect Scanning

Scan results may be affected by:

1. Firewalls.
2. Network routing.
3. Packet filtering.
4. Host availability.
5. Virtual machine network configuration.
6. Service configuration.
7. Network latency.
8. Scanning permissions and restrictions.

A failed scan does not always mean a machine is offline.

For example, a firewall may block discovery traffic while allowing a specific application connection.

---

## 11. Practical Exercise – Inspect Your Own Kali Network

This exercise uses local system inspection only. It does not scan other devices.

### Step 1: Create your lesson workspace

```bash
mkdir -p ~/module16-lab/lesson05
cd ~/module16-lab/lesson05
```

### Step 2: Check your IP configuration

```bash
ip addr
```

Look for:
- Network interface name.
- IPv4 address.
- IPv6 address.
- Interface state.

### Step 3: Check your routing table

```bash
ip route
```

Look for:
- Default route.
- Gateway address.
- Network interface.

### Step 4: Check your listening ports

```bash
ss -tuln
```

Explanation:

- `ss` displays socket information.
- `-t` displays TCP sockets.
- `-u` displays UDP sockets.
- `-l` displays listening sockets.
- `-n` displays numeric addresses and ports.

This command inspects your own Kali machine rather than scanning another device.

### Step 5: Record your findings

```bash
nano network-scan-notes.txt
```

Enter:

```text
NETWORK SCANNING FUNDAMENTALS

Machine:
Kali Linux VM

IPv4 Address:
Record your actual address.

IPv6 Address:
Record your actual address if available.

Network Interface:
Record the interface name.

Default Gateway:
Record the gateway shown by ip route.

Listening TCP Ports:
Record relevant output from ss -tuln.

Listening UDP Ports:
Record relevant output from ss -tuln.

Observations:
Which services appear to be listening?

Security Notes:
Are all listening services necessary?
Do you understand what each service does?

Conclusion:
Summarize what you learned.
```

Save the document and review it:

```bash
cat network-scan-notes.txt
```

### Step 6: Understand the Results

Answer these questions:
- What is your Kali VM's IP address?
- What is its network interface name?
- What is its default gateway?
- Which TCP ports are listening?
- Which UDP ports are listening?

Do not assume every listening port is dangerous. Identify the associated application before drawing conclusions.

---

## 12. Knowledge Check

**Question 1:** What is the main purpose of network scanning?

A. Editing documents  
B. Discovering hosts, ports, and services  
C. Creating passwords  
D. Installing applications  

**Question 2:** Which tool is commonly used for network discovery and port scanning?

A. Nmap  
B. Nano  
C. LibreOffice  
D. GIMP  

**Question 3:** What does an open port indicate?

A. The computer is automatically compromised  
B. An application is accepting connections on that port  
C. The firewall is disabled  
D. The operating system is missing  

**Question 4:** Which command displays listening TCP and UDP sockets on your Kali machine?

A. `ip route`  
B. `ss -tuln`  
C. `pwd`  
D. `whoami`  

**Question 5:** What does service detection attempt to identify?

A. The physical location of a computer  
B. The application or service associated with a port  
C. The user's password  
D. The monitor resolution  

---

## 13. Practical Thinking Questions

Answer these in your notebook:

1. Define network scanning.
2. Explain host discovery.
3. What is a port?
4. Differentiate TCP and UDP.
5. Explain open, closed, and filtered port states.
6. What is Nmap used for?
7. Why does an open port not automatically mean a vulnerability exists?
8. List five factors that can affect scan results.
9. What information does the `ss -tuln` command display?
10. Why must a penetration tester confirm authorization before scanning?

---

## 14. Lesson Summary

In this lesson, you learned:

- The meaning and purpose of network scanning.
- Host discovery, port scanning, and service discovery.
- Common port numbers and services.
- TCP and UDP.
- Port states.
- Introduction to Nmap.
- Basic Nmap command concepts.
- Factors affecting scanning results.
- How to inspect your Kali VM's own network configuration.

### Key Principle

**Network scanning helps identify a system's network exposure. A professional tester interprets the results carefully, confirms authorization, and investigates services without assuming that every open port is a vulnerability.**

**End of Lesson 05 – Network Scanning Fundamentals**