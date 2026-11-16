
# MODULE 10 – NETWORK ENUMERATION

# LESSON 10: FINAL PRACTICAL ASSESSMENT

## 1. Assessment Overview

Welcome to the final practical assessment for Module 10 – Network Enumeration.

This assessment evaluates your ability to:

- Understand network enumeration.
- Inspect network interfaces.
- Identify network services.
- Perform authorized Nmap scans.
- Identify service versions.
- Analyze DNS records.
- Understand SMB enumeration.
- Inspect web services.
- Analyze network traffic.
- Produce a professional security report.

### Assessment Information

| Item | Details |
|---|---|
| Module | 10 – Network Enumeration |
| Assessment Type | Practical and Theoretical |
| Total Marks | 100 |
| Pass Mark | 50 |
| Environment | Kali Linux VM |
| Main Tools | Nmap, dig, curl, Wireshark |
| Target | Own Kali VM / loopback |

## 2. Safety and Scope

All practical activities must remain within your own Kali VM.

Approved target:

```text
127.0.0.1
```

You may also inspect services on another VM if it belongs to you and is intentionally configured as an isolated laboratory target.

Do not scan public IP addresses, university networks, or third-party systems.

Do not attempt unauthorized authentication, exploitation, or vulnerability attacks.

## 3. Assessment Workspace

Create your assessment directory:

```bash
mkdir -p ~/practical-labs/module10/final-assessment/evidence
mkdir -p ~/practical-labs/module10/final-assessment/reports
cd ~/practical-labs/module10/final-assessment
```

Verify your working directory:

```bash
pwd
```

## 4. Task 1: Network Information Collection

**Marks: 10**

### Objective

Inspect your Kali VM's network configuration.

Run:

```bash
ip addr
```

```bash
ip route
```

```bash
ip neigh
```

```bash
hostname
```

```bash
hostname -I
```

### Questions

1. What is your network interface name?
2. What is your Kali VM's IP address?
3. What is your default gateway, if configured?
4. What does `ip neigh` display?
5. What is the purpose of a loopback address?

Save your results:

```bash
ip addr > evidence/network-interfaces.txt
ip route > evidence/routing-table.txt
ip neigh > evidence/neighbor-table.txt
```

## 5. Task 2: Host Discovery

**Marks: 10**

### Objective

Understand host discovery using Nmap.

Run:

```bash
nmap -sn 127.0.0.1
```

Save the result:

```bash
nmap -sn 127.0.0.1 -oN evidence/host-discovery.txt
```

### Questions

1. What does `-sn` mean?
2. What is the purpose of host discovery?
3. What does a loopback address represent?
4. Why might host discovery behave differently across network configurations?

## 6. Task 3: Port Scanning

**Marks: 15**

### Objective

Inspect selected TCP ports on your own Kali VM.

Run:

```bash
nmap -sT -p 22,80,443,8080,8085,9090 127.0.0.1
```

Save the output:

```bash
nmap -sT -p 22,80,443,8080,8085,9090 127.0.0.1 -oN evidence/port-scan.txt
```

### Questions

1. What is a TCP port?
2. What is the difference between open and closed?
3. What does filtered mean?
4. Why is port 22 commonly associated with SSH?
5. Does an open port automatically indicate a vulnerability?

Record your observed port states.

## 7. Task 4: Service Version Detection

**Marks: 10**

### Objective

Identify services and available version information.

Run:

```bash
nmap -sV -p 22,80,443,8080,8085,9090 127.0.0.1
```

Save the result:

```bash
nmap -sV -p 22,80,443,8080,8085,9090 127.0.0.1 -oN evidence/service-detection.txt
```

### Questions

1. What does `-sV` do?
2. What is service banner information?
3. Why should detected versions be verified?
4. Can a service banner be inaccurate?

## 8. Task 5: DNS Enumeration

**Marks: 15**

### Objective

Perform basic DNS record queries.

Run:

```bash
dig example.com A
```

```bash
dig example.com AAAA
```

```bash
dig example.com MX
```

```bash
dig example.com NS
```

```bash
dig example.com TXT
```

Save the results:

```bash
dig example.com A > evidence/dns-a.txt
dig example.com AAAA > evidence/dns-aaaa.txt
dig example.com MX > evidence/dns-mx.txt
dig example.com NS > evidence/dns-ns.txt
dig example.com TXT > evidence/dns-txt.txt
```

### Questions

1. What does an A record represent?
2. What does an AAAA record represent?
3. What does an MX record identify?
4. What is an NS record?
5. What does NXDOMAIN mean?

## 9. Task 6: SMB and Windows Service Understanding

**Marks: 10**

### Objective

Inspect SMB-related information without enabling additional services.

On Kali, run:

```bash
smbclient --version
```

```bash
sudo ss -tulpn | grep -E ':139|:445'
```

If you have access to your Windows 11 host, open PowerShell and run:

```powershell
Get-SmbClientConfiguration
```

```powershell
Get-SmbShare
```

### Questions

1. What does SMB stand for?
2. What is the main modern SMB port?
3. What is Samba?
4. Why should SMBv1 generally be avoided?
5. Why should shared-folder permissions be restricted?

If a command or service is unavailable, document that limitation. Do not enable SMB merely to produce a result.

## 10. Task 7: Web Service Enumeration

**Marks: 15**

### Objective

Create a temporary local HTTP server.

Navigate to your assessment directory:

```bash
cd ~/practical-labs/module10/final-assessment
```

Create a web page:

```bash
echo "<h1>Module 10 Assessment</h1>" > index.html
```

Start the local server:

```bash
python3 -m http.server 8080 --bind 127.0.0.1
```

Open another terminal.

Inspect the HTTP response:

```bash
curl -I http://127.0.0.1:8080
```

Retrieve the page:

```bash
curl http://127.0.0.1:8080
```

Inspect the service:

```bash
nmap -sV -p 8080 127.0.0.1
```

Inspect the page title:

```bash
nmap -p 8080 --script http-title 127.0.0.1
```

Save your results:

```bash
curl -I http://127.0.0.1:8080 > evidence/http-headers.txt
curl http://127.0.0.1:8080 > evidence/http-response.txt
```

Stop the server after the exercise:

```text
CTRL + C
```

### Questions

1. What does HTTP stand for?
2. What is the default HTTP port?
3. What does status code 200 mean?
4. What does status code 404 mean?
5. What is the purpose of HTTPS?
6. What information can HTTP headers reveal?

## 11. Task 8: Network Traffic Analysis

**Marks: 5**

### Objective

Observe DNS traffic with Wireshark.

Start Wireshark:

```bash
wireshark
```

Select the active network interface.

Start capturing.

Generate a DNS query:

```bash
dig example.com
```

Apply this display filter:

```text
dns
```

Observe:

- DNS query name.
- Query type.
- Source address.
- Destination address.
- Response code.

Save the capture:

```text
evidence/module10-dns-capture.pcapng
```

### Questions

1. What does Wireshark do?
2. What is a packet capture?
3. What does the `dns` display filter show?

If no DNS packet appears, check your selected interface and resolver behavior. Record the limitation rather than fabricating a capture.

## 12. Task 9: Evidence Integrity

**Marks: 5**

### Objective

Generate SHA-256 hashes for your evidence files.

Run:

```bash
find evidence -type f -exec sha256sum {} \; > evidence-hashes.txt
```

Inspect the hash list:

```bash
cat evidence-hashes.txt
```

Verify the recorded hashes:

```bash
sha256sum -c evidence-hashes.txt
```

### Questions

1. What is SHA-256?
2. Why is evidence integrity important?
3. What does a successful hash verification indicate?

## 13. Task 10: Final Professional Report

**Marks: 5**

Create:

```bash
nano reports/final-network-enumeration-report.md
```

Use this structure:

```markdown
# FINAL NETWORK ENUMERATION REPORT

## 1. Assessment Information

Assessment:
Date:
Assessor:
Target:
Authorized Scope:

## 2. Executive Summary

Summarize the assessment.

## 3. Network Configuration

Interface:
IP Address:
Gateway:
Hostname:

## 4. Host Discovery

Tool:
Command:
Result:

## 5. Port Scanning

| Port | State | Service |
|------|-------|---------|

## 6. Service Detection

Detected Services:
Version Information:
Limitations:

## 7. DNS Enumeration

A Records:
AAAA Records:
MX Records:
NS Records:
TXT Records:

## 8. SMB Inspection

SMB Tools:
Port Observations:
Windows Inspection:
Limitations:

## 9. Web Service Enumeration

HTTP Status:
Headers:
Service:
Page Content:

## 10. Traffic Analysis

Capture File:
DNS Observations:

## 11. Evidence Integrity

Hash File:
Verification Result:

## 12. Security Observations

Document verified observations only.

## 13. Recommendations

List practical security improvements.

## 14. Limitations

Document unavailable services and incomplete tests.

## 15. Conclusion

Summarize the assessment.
```

## 14. Final Theoretical Examination

Answer these questions in your own words.

1. Define network enumeration.
2. Explain the difference between host discovery and port scanning.
3. What is the purpose of Nmap?
4. Explain the three common port states: open, closed, and filtered.
5. What does service version detection accomplish?
6. Explain DNS resolution.
7. Differentiate A, AAAA, MX, NS, and TXT records.
8. Explain SMB and its purpose.
9. What is the difference between HTTP and HTTPS?
10. Explain the importance of evidence verification.
11. What is a false positive?
12. What is a false negative?
13. Why is network enumeration important in penetration testing?
14. Why must security assessments have an authorized scope?
15. What are the main components of a professional enumeration report?

## 15. Marking Scheme

| Task | Marks |
|---|---:|
| Network Information Collection | 10 |
| Host Discovery | 10 |
| Port Scanning | 15 |
| Service Version Detection | 10 |
| DNS Enumeration | 15 |
| SMB Understanding | 10 |
| Web Service Enumeration | 15 |
| Traffic Analysis | 5 |
| Evidence Integrity | 5 |
| Professional Report | 5 |
| **Total** | **100** |

### Grading Guide

| Score | Grade |
|---|---|
| 80–100 | Excellent |
| 70–79 | Very Good |
| 60–69 | Good |
| 50–59 | Pass |
| 0–49 | Needs Improvement |

## 16. Final Assessment Completion Checklist

- [ ] Network configuration inspected.
- [ ] Host discovery completed.
- [ ] Port scanning completed.
- [ ] Service detection completed.
- [ ] DNS records inspected.
- [ ] SMB tools and configuration understood.
- [ ] Local web service inspected.
- [ ] Wireshark DNS traffic analyzed.
- [ ] Evidence hashes generated.
- [ ] Final report prepared.
- [ ] Theoretical examination completed.

## 17. Module 10 Completion

Congratulations on reaching the end of Module 10.

You have studied the fundamentals of network enumeration and practiced using important tools employed by network administrators and cybersecurity professionals.

You should now understand how to:

- Discover hosts.
- Identify open network ports.
- Detect services.
- Examine service versions.
- Inspect DNS records.
- Understand SMB services.
- Enumerate local web services.
- Analyze network traffic.
- Organize evidence.
- Write professional technical reports.

**NEXT MODULE: MODULE 11 – VULNERABILITY ASSESSMENT.**

In Module 11, you will learn how security professionals identify, validate, prioritize, and document vulnerabilities in authorized systems.
