
# MODULE 15 – CAPSTONE PROJECT

# LESSON 03: SYSTEM DISCOVERY AND ENUMERATION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain system discovery and enumeration.
2. Identify devices in an authorized network.
3. Understand IP and MAC addresses.
4. Perform controlled host discovery.
5. Identify open TCP ports.
6. Examine running services and versions.
7. Save scan results.
8. Prepare a network discovery report.

---

## 2. What Is System Discovery?

System discovery is the process of identifying computers and other devices within a network.

Before assessing a network's security, an investigator must understand which systems are present.

For example, an organization may have:

- Linux servers.
- Windows workstations.
- Network printers.
- Routers.
- Database servers.
- Application servers.

In our capstone, we will identify the systems inside our isolated virtual laboratory.

## 3. What Is Network Enumeration?

Network enumeration involves collecting additional information about discovered systems.

It may reveal:

- IP addresses.
- MAC addresses.
- Open ports.
- Running services.
- Service versions.
- Operating-system information.
- Network configurations.

### Discovery vs Enumeration

| Discovery | Enumeration |
|---|---|
| Identifies available hosts | Collects detailed host information |
| Answers which systems exist | Answers what services they provide |
| Often an initial step | Usually follows discovery |

---

## 4. Understanding IP Addresses

An IP address identifies a network interface on an IP network.

Example:

```text
192.168.56.20
```

An IPv4 address contains four octets separated by periods.

Each octet ranges from 0 to 255.

### Private IPv4 Address Ranges

| Range | CIDR |
|---|---|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

Our virtual laboratory uses a private address range.

Always inspect the actual subnet before performing discovery.

---

## 5. Understanding MAC Addresses

MAC stands for Media Access Control.

A MAC address identifies a network interface at the data-link layer.

Example:

```text
00:0C:29:12:34:56
```

MAC addresses are commonly represented as six hexadecimal pairs.

### IP vs MAC

| IP Address | MAC Address |
|---|---|
| Logical network address | Link-layer interface address |
| Used for IP communication | Used on local network links |
| Can change | Usually assigned to an interface |
| IPv4 or IPv6 | Commonly 48-bit Ethernet address |

MAC addresses can be changed or spoofed, so they should not be treated as permanent proof of device identity.

---

## 6. Introduction to Nmap

Nmap stands for Network Mapper.

It is a widely used network discovery and security auditing tool.

It can help identify:

- Active hosts.
- Open ports.
- Services.
- Service versions.
- Operating-system characteristics.

We will use Nmap only against our authorized Metasploitable 2 VM and the isolated laboratory network.

### Check Nmap Installation

```bash
nmap --version
```

If unavailable:

```bash
sudo apt update
sudo apt install nmap
```

---

## 7. Practical Lab A: Identify Your Network

Open Kali Linux.

### Step 1: Display IP Addresses

```bash
ip -br addr
```

Identify your Host-only interface.

### Step 2: Display Network Routes

```bash
ip route
```

### Step 3: Record Your Host-only Subnet

Use the actual IP address and prefix shown by your system.

For example, if your Host-only address is:

```text
192.168.56.10/24
```

The corresponding subnet is:

```text
192.168.56.0/24
```

This is only an example. Do not assume your subnet matches it.

### Step 4: Record the Target IP

On Metasploitable 2:

```bash
ifconfig
```

Record its Host-only IPv4 address.

Create a note of your actual network details.

---

## 8. Practical Lab B: Host Discovery

Host discovery helps identify devices that respond within a specified network.

### Step 1: Test the Target

From Kali:

```bash
ping -c 4 TARGET_IP
```

Replace `TARGET_IP` with the actual Metasploitable 2 IP.

### Step 2: Perform Nmap Host Discovery

```bash
sudo nmap -sn TARGET_IP
```

This performs host discovery without a port scan.

### Step 3: Examine the Output

Look for:

- Target IP address.
- Host status.
- MAC address, if available.
- Network interface vendor information, if available.

### Step 4: Save the Results

```bash
mkdir -p ~/practical-labs/module15/discovery
```

```bash
sudo nmap -sn TARGET_IP -oN ~/practical-labs/module15/discovery/host-discovery.txt
```

Replace `TARGET_IP` before running.

---

## 9. Practical Lab C: Port Scanning

A network port identifies a communication endpoint associated with a service.

Common TCP ports include:

| Port | Common Service |
|---|---|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 139 | NetBIOS |
| 443 | HTTPS |
| 445 | SMB |

### Step 1: Scan the 20 Most Common TCP Ports

Only scan your authorized Metasploitable 2 target:

```bash
sudo nmap -sS --top-ports 20 TARGET_IP
```

### Step 2: Examine Results

Look for:

- Port number.
- Protocol.
- Port state.
- Service name.

### Understanding Port States

| State | Meaning |
|---|---|
| Open | A service is accepting connections |
| Closed | The host responded, but no service is listening |
| Filtered | Nmap cannot determine the state because filtering interferes |
| Open\|Filtered | Nmap cannot distinguish between open and filtered |

An open port is not automatically a vulnerability.

---

## 10. Practical Lab D: Service Version Detection

Service version detection attempts to identify the service and software version associated with open ports.

Run a limited version scan:

```bash
sudo nmap -sV --top-ports 20 TARGET_IP
```

Examine:

- Port.
- Service.
- Product.
- Version information.
- Additional service details.

### Why Is Version Detection Important?

Software versions help security analysts determine whether a service may be outdated or affected by known vulnerabilities.

However, version detection does not prove that a vulnerability exists.

Further validation is required.

---

## 11. Practical Lab E: Save Your Scan Results

Create an output directory:

```bash
mkdir -p ~/practical-labs/module15/discovery/results
```

Run:

```bash
sudo nmap -sV --top-ports 20 TARGET_IP -oA ~/practical-labs/module15/discovery/results/target-assessment
```

Nmap produces several output formats:

- `.nmap` – Normal readable output.
- `.xml` – Structured XML output.
- `.gnmap` – Grepable output.

Review the normal output:

```bash
cat ~/practical-labs/module15/discovery/results/target-assessment.nmap
```

Keep scan results as evidence of what was observed during the assessment.

---

## 12. Practical Lab F: Examine Local Network Information

Use the following commands:

### View the ARP/Neighbor Table

```bash
ip neigh
```

### Display Interface Details

```bash
ip link show
```

### Display IP Configuration

```bash
ip addr show
```

### Display Routing Information

```bash
ip route show
```

### What Does the Neighbor Table Show?

It displays IP-to-link-layer address information known to the local system.

Entries may be incomplete, stale, or unavailable if communication has not occurred recently.

---

## 13. Create a Network Discovery Report

Create a report:

```bash
cat > ~/practical-labs/module15/discovery/discovery-report.md <<'EOF'
# NETWORK DISCOVERY REPORT

## 1. Project Information

Project:
Isolated Cybersecurity Assessment and Defense Lab

Assessment Type:
Authorized Virtual Network Discovery

## 2. Assessment Scope

Target:
Metasploitable 2 virtual machine

Network:
VMware Host-only network

Authorization:
Self-owned isolated training environment

## 3. Discovery Methods

- IP configuration inspection
- ICMP connectivity testing
- Nmap host discovery
- Limited TCP port scanning
- Service version detection

## 4. Target Information

Target IP:
[Enter actual target IP]

Host Status:
[Enter observed result]

MAC Address:
[Enter if available]

## 5. Port and Service Findings

[Enter actual scan results]

## 6. Observations

[Describe the services discovered.]

## 7. Limitations

The assessment was restricted to the authorized
isolated virtual laboratory.

A discovered open port does not automatically
represent a confirmed vulnerability.

## 8. Conclusion

Network discovery and service enumeration were
performed against the authorized training target.

Further security assessment will be performed
in the next project phase.
EOF
```

Edit the report using:

```bash
nano ~/practical-labs/module15/discovery/discovery-report.md
```

Replace the placeholders with your actual findings.

---

## 14. Knowledge Check

1. What is system discovery?
2. What is network enumeration?
3. What is the difference between an IP address and a MAC address?
4. What does Nmap do?
5. What is the purpose of `nmap -sn`?
6. What is an open port?
7. What does service version detection reveal?
8. Why does an open port not automatically indicate a vulnerability?
9. What is the purpose of Nmap's `-oA` option?
10. Why should scan results be documented?

---

## 15. Practical Completion Checklist

- [ ] I inspected Kali network interfaces.
- [ ] I identified the Host-only subnet.
- [ ] I recorded the actual Metasploitable IP.
- [ ] I performed host discovery.
- [ ] I performed a limited TCP port scan.
- [ ] I performed service version detection.
- [ ] I examined the local neighbor table.
- [ ] I saved Nmap results.
- [ ] I created a network discovery report.
- [ ] I used only my authorized isolated target.

---

## 16. Key Takeaway

System discovery and enumeration provide the foundation for a security assessment.

Before identifying vulnerabilities, you must first understand which systems exist, which ports are open, and which services are running.

**Remember: Discovery tells you what is present. Enumeration helps you understand what those systems expose.**
