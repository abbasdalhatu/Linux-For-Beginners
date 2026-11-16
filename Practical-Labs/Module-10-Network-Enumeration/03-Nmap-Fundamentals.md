
# MODULE 10 – NETWORK ENUMERATION

# LESSON 03: NMAP FUNDAMENTALS

## 1. Introduction

Nmap stands for **Network Mapper**.

It is an open-source tool used for network discovery, port scanning, service identification, and security auditing.

Security professionals use Nmap to understand which services are exposed on systems they are authorized to assess.

Nmap is one of the most important tools in ethical hacking and penetration testing.

## 2. What Is Nmap?

Nmap helps answer questions such as:

- Is a host reachable?
- Which TCP ports are open?
- Which ports are closed?
- Which ports are filtered?
- Which services are listening?
- What software may be running?
- What operating system might the host use?

Example:

```text
Host: 192.168.56.20

PORT     STATE    SERVICE
22/tcp   open     ssh
80/tcp   open     http
443/tcp  closed   https
```

This is an illustrative result, not an actual scan.

## 3. Why Nmap Is Important

### A. Network Discovery

Identifies reachable devices.

### B. Port Scanning

Identifies accessible TCP and UDP ports.

### C. Service Detection

Attempts to identify services running on discovered ports.

### D. Operating System Detection

Uses network-response characteristics to estimate the target operating system.

### E. Security Auditing

Helps administrators identify unexpectedly exposed services.

## 4. Installing and Verifying Nmap

Check whether Nmap is installed:

```bash
nmap --version
```

If it is not installed:

```bash
sudo apt update
sudo apt install nmap
```

Verify installation:

```bash
which nmap
```

Display basic help:

```bash
nmap --help
```

Read the manual:

```bash
man nmap
```

## 5. Understanding Nmap Scan Results

Nmap commonly reports these port states:

| State | Meaning |
|---|---|
| open | An application is accepting connections |
| closed | The port is reachable but no application is listening |
| filtered | Nmap cannot determine the state because filtering prevents a clear response |
| unfiltered | The port is reachable, but its open/closed state is not determined by that scan |
| open\|filtered | Nmap cannot distinguish between open and filtered |
| closed\|filtered | Nmap cannot distinguish between closed and filtered |

Important: An open port does not automatically mean a vulnerability exists.

## 6. Understanding TCP and UDP Scanning

### TCP

TCP is connection-oriented.

It uses a handshake to establish connections.

Common TCP services include:

- SSH.
- HTTP.
- HTTPS.
- SMTP.
- SMB.

### UDP

UDP is connectionless.

It is commonly used by:

- DNS.
- DHCP.
- NTP.
- Some streaming and real-time applications.

UDP scanning can be less conclusive because open UDP services may not respond to every probe.

## 7. Practical Lab 1: Scan Your Own Loopback Address

Create your workspace:

```bash
mkdir -p ~/practical-labs/module10/lab03
cd ~/practical-labs/module10/lab03
```

Run a basic Nmap scan against your own machine:

```bash
nmap 127.0.0.1
```

This checks common TCP ports on your own loopback address.

Observe:

- Host status.
- Number of ports scanned.
- Open ports.
- Closed ports.
- Service names.

Save the result:

```bash
nmap 127.0.0.1 -oN loopback-basic-scan.txt
```

### Questions

1. Was your host reported as up?
2. How many ports were scanned?
3. Were any ports open?
4. What services were identified?

Your results may differ depending on the services running on Kali.

## 8. Practical Lab 2: Host Discovery

The `-sn` option performs host discovery without a port scan.

Run:

```bash
nmap -sn 127.0.0.1
```

Understand the command:

- `nmap` launches the scanner.
- `-sn` disables port scanning.
- `127.0.0.1` identifies your own loopback address.

Save the result:

```bash
nmap -sn 127.0.0.1 -oN loopback-host-discovery.txt
```

## 9. Practical Lab 3: Scan Specific Ports

The `-p` option specifies ports.

Check port 22 on your own system:

```bash
nmap -p 22 127.0.0.1
```

Check several selected ports:

```bash
nmap -p 22,53,80,443 127.0.0.1
```

Check a small port range:

```bash
nmap -p 1-100 127.0.0.1
```

The range `1-100` means ports 1 through 100.

Save your results:

```bash
nmap -p 1-100 127.0.0.1 -oN selected-port-scan.txt
```

## 10. Practical Lab 4: Understand Scan Types

Nmap supports several scanning techniques.

### A. TCP Connect Scan

Command:

```bash
nmap -sT 127.0.0.1
```

This uses the operating system's TCP connection mechanism to establish connections.

It is useful when raw-packet privileges are unavailable.

### B. SYN Scan

Command:

```bash
sudo nmap -sS 127.0.0.1
```

This uses TCP SYN probes.

It is commonly called a half-open scan because it does not complete the normal TCP connection in the same way as a connect scan.

### C. UDP Scan

Command:

```bash
sudo nmap -sU -p 53,123,161 127.0.0.1
```

This checks selected UDP ports on your own machine.

UDP results may be ambiguous because some services do not respond to empty or unexpected probes.

### Important

These exercises are restricted to your own loopback system. Do not substitute public or unknown addresses.

## 11. Practical Lab 5: Service Version Detection

The `-sV` option attempts to identify service versions.

Test your own system:

```bash
nmap -sV 127.0.0.1
```

Test selected ports:

```bash
nmap -sV -p 22,80,443 127.0.0.1
```

A result might look like:

```text
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH ...
```

The exact version depends on your installed software.

Version detection is useful because outdated software may require security updates.

However, a detected version does not prove that a vulnerability exists.

## 12. Practical Lab 6: Operating System Detection

Nmap provides operating system detection through:

```bash
sudo nmap -O 127.0.0.1
```

The `-O` option attempts to estimate the operating system.

Possible limitations:

- Insufficient response information.
- Firewall filtering.
- Virtualization.
- Similar network-stack characteristics.
- Insufficient privileges.

You may receive an inconclusive result. That is normal.

## 13. Practical Lab 7: Save Scan Results

Nmap supports multiple output formats.

### Normal Output

```bash
nmap 127.0.0.1 -oN normal-scan.txt
```

### XML Output

```bash
nmap 127.0.0.1 -oX scan-results.xml
```

### Grepable Output

```bash
nmap 127.0.0.1 -oG grepable-scan.txt
```

### All Major Output Formats

```bash
nmap 127.0.0.1 -oA loopback-results
```

This produces files with different output extensions.

Inspect your workspace:

```bash
ls -lh
```

## 14. Practical Lab 8: Create an Nmap Summary

Create a report:

```bash
nano nmap-summary.txt
```

Use this structure:

```text
========================================
NMAP FUNDAMENTALS REPORT
========================================

1. TARGET INFORMATION

Target:
Assessment Scope:
Date:

2. HOST DISCOVERY

Command:
Result:
Host Status:

3. PORT SCANNING

Command:
Ports Scanned:
Open Ports:
Closed Ports:
Filtered Ports:

4. SERVICE DETECTION

Service:
Port:
Detected Version:
Confidence:

5. OPERATING SYSTEM DETECTION

Command:
Result:
Limitations:

6. OUTPUT FILES

Normal Output:
XML Output:
Grepable Output:

7. SECURITY OBSERVATIONS

Unexpected Services:
Potential Exposure:
Recommendations:

8. CONCLUSION

Summary:

========================================
END OF REPORT
========================================
```

Fill it using your actual results.

## 15. Common Nmap Errors

### Error 1: Command Not Found

Possible solution:

```bash
sudo apt install nmap
```

### Error 2: Permission Denied

Some scanning methods require elevated privileges.

For your own loopback exercises, use `sudo` only where necessary.

### Error 3: Host Seems Down

Possible causes:

- Incorrect target address.
- Firewall restrictions.
- Network isolation.
- Host unavailable.

### Error 4: No Open Ports

This does not necessarily mean Nmap failed.

Your system may simply have no services listening on the tested ports.

## 16. Knowledge Check

Answer these questions:

1. What does Nmap stand for?
2. What is the primary purpose of Nmap?
3. What does the `-sn` option do?
4. What does `-p` specify?
5. What is the difference between TCP and UDP scanning?
6. What does `-sT` do?
7. What does `-sS` do?
8. What does `-sV` do?
9. What does `-O` attempt to identify?
10. What is the difference between open, closed, and filtered ports?
11. Why should Nmap results be interpreted carefully?
12. Why must scanning stay within an authorized scope?

## 17. Lesson Completion Checklist

- [ ] Understand Nmap fundamentals.
- [ ] Verify Nmap installation.
- [ ] Understand port states.
- [ ] Perform host discovery.
- [ ] Scan selected ports on loopback.
- [ ] Understand TCP connect scanning.
- [ ] Understand SYN scanning.
- [ ] Understand UDP scanning.
- [ ] Perform service version detection.
- [ ] Understand OS detection.
- [ ] Save scan results in multiple formats.
- [ ] Create an Nmap summary report.

## 18. Lesson Summary

Nmap is an essential network enumeration tool.

It helps security professionals discover hosts, identify open ports, examine services, and document network exposure.

Remember:

- `-sn` performs host discovery.
- `-p` specifies ports.
- `-sT` performs a TCP connect scan.
- `-sS` performs a TCP SYN scan.
- `-sU` performs a UDP scan.
- `-sV` performs service version detection.
- `-O` attempts operating system detection.
- `-oN` saves normal output.
- `-oX` saves XML output.
- `-oA` saves multiple output formats.

**NEXT LESSON: PORT SCANNING AND SERVICE DISCOVERY.**
