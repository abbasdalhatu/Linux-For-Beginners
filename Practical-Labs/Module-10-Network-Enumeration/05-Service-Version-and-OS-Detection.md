
# MODULE 10 – NETWORK ENUMERATION

# LESSON 05: SERVICE VERSION AND OPERATING SYSTEM DETECTION

## 1. Introduction

After identifying open ports, the next step is understanding what software and operating systems are running on the discovered systems.

This process involves:

1. Service identification.
2. Version detection.
3. Operating system fingerprinting.
4. Banner inspection.
5. Result validation.

For example, discovering TCP port 22 tells us that a service may be listening on the SSH port.

Version detection may help identify the SSH software and its reported version.

This information helps administrators identify outdated software and investigate potential security risks.

## 2. What Is Service Version Detection?

Service version detection is the process of identifying the software running behind a network service.

Examples:

| Port | Service | Possible Software |
|---|---|---|
| 22 | SSH | OpenSSH |
| 80 | HTTP | Apache or Nginx |
| 443 | HTTPS | Apache or Nginx |
| 3306 | MySQL | MySQL Server |
| 5432 | PostgreSQL | PostgreSQL Server |

These are common examples, not guaranteed port-to-service mappings.

Nmap uses service probes and response analysis to identify services.

## 3. Why Version Detection Matters

### A. Software Inventory

Helps administrators understand which software is installed and exposed.

### B. Patch Management

Helps identify software that may require updates.

### C. Vulnerability Assessment

Allows security professionals to investigate whether a particular software version is affected by known vulnerabilities.

### D. Configuration Review

Helps identify unexpected services and potentially insecure configurations.

### E. Security Reporting

Provides technical information for professional assessment reports.

**Important:** A reported version does not automatically prove a vulnerability. Vendors may backport security fixes without changing the version string in the way you expect.

## 4. Nmap Service Detection

The main option for service version detection is:

```bash
nmap -sV TARGET
```

For this lesson, use only your own loopback address:

```bash
nmap -sV 127.0.0.1
```

Scan a selected port:

```bash
nmap -sV -p 22 127.0.0.1
```

Scan several ports:

```bash
nmap -sV -p 22,80,443 127.0.0.1
```

### Understanding the Output

Illustrative output:

```text
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH ...
```

Explanation:

- PORT: Network port.
- STATE: Port state.
- SERVICE: Identified service.
- VERSION: Software information, if detected.

## 5. Understanding Service Detection Intensity

Nmap supports different service detection intensity levels.

The intensity range is 0–9.

A higher intensity generally performs more probing.

Examples:

```bash
nmap -sV --version-intensity 0 127.0.0.1
```

```bash
nmap -sV --version-intensity 5 127.0.0.1
```

```bash
nmap -sV --version-intensity 9 127.0.0.1
```

For your controlled local lab, the default intensity is sufficient.

A higher intensity does not guarantee a more accurate result and may generate additional probes.

## 6. What Is Operating System Detection?

Operating system detection attempts to estimate the operating system running on a remote host.

Nmap uses network-response characteristics to create an OS fingerprint.

Examples of operating systems include:

- Linux.
- Windows.
- FreeBSD.
- macOS.
- Other Unix-like systems.

The primary Nmap option is:

```bash
sudo nmap -O 127.0.0.1
```

Operating system detection may require elevated privileges.

### Limitations

OS detection can be affected by:

- Firewalls.
- NAT.
- Network filtering.
- Virtualization.
- Similar TCP/IP stack characteristics.
- Insufficient response information.

An inconclusive result is a valid outcome.

## 7. Service Banners

A service banner is information returned by a network service when a connection is established or an appropriate request is sent.

Example:

```text
SSH-2.0-OpenSSH_...
```

This is an illustrative banner format.

Banners may reveal:

- Service name.
- Software version.
- Protocol version.
- Server information.

However, banners can be hidden, modified, or misleading.

## 8. Practical Lab 1: Create Your Workspace

Run:

```bash
mkdir -p ~/practical-labs/module10/lab05
cd ~/practical-labs/module10/lab05
```

Verify Nmap:

```bash
nmap --version
```

Inspect your local system:

```bash
hostnamectl
```

Inspect listening services:

```bash
ss -tuln
```

Save the outputs:

```bash
hostnamectl > system-information.txt
ss -tuln > local-services.txt
```

## 9. Practical Lab 2: Create a Local HTTP Service

Create a web directory:

```bash
mkdir -p ~/practical-labs/module10/lab05/www
```

Create a test page:

```bash
echo "Service Version Detection Laboratory" > ~/practical-labs/module10/lab05/www/index.html
```

Start a Python HTTP server:

```bash
cd ~/practical-labs/module10/lab05/www
python3 -m http.server 8085 --bind 127.0.0.1
```

The server listens only on your own loopback interface.

Open another terminal.

Test it:

```bash
curl http://127.0.0.1:8085
```

Expected response:

```text
Service Version Detection Laboratory
```

## 10. Practical Lab 3: Identify the HTTP Service

Run:

```bash
nmap -sV -p 8085 127.0.0.1
```

Observe:

- Port state.
- Service identification.
- Version information.
- Any additional service details.

Save the result:

```bash
nmap -sV -p 8085 127.0.0.1 -oN http-version-detection.txt
```

The exact identification depends on the Nmap version database and service responses.

## 11. Practical Lab 4: Inspect an HTTP Response

In another terminal, run:

```bash
curl -I http://127.0.0.1:8085
```

The `-I` option requests HTTP response headers.

You may see information such as:

```text
HTTP/1.0 200 OK
Server: SimpleHTTP/...
Date: ...
Content-type: text/html
```

The exact response depends on your Python installation.

Record:

1. HTTP status.
2. Server header.
3. Content type.
4. Any other returned headers.

### Security Observation

Service headers can disclose software information.

Administrators should review whether unnecessary version information is being exposed.

Removing a banner alone is not a substitute for patching and securing the underlying service.

## 12. Practical Lab 5: Inspect an SSH Banner

First check whether an SSH service is listening locally:

```bash
ss -ltn
```

Look for TCP port 22.

If an SSH service is already listening on loopback, inspect its banner:

```bash
nc -nv 127.0.0.1 22
```

If a banner is returned, record it.

If the connection fails or no banner appears, document that result.

Do not install or enable SSH merely to force a result. We are investigating the services already available on your machine.

## 13. Practical Lab 6: Operating System Detection

Run:

```bash
sudo nmap -O 127.0.0.1
```

Observe:

- OS detection result.
- Confidence information.
- Any uncertainty.
- Whether Nmap reports that additional information is required.

Save the result:

```bash
sudo nmap -O 127.0.0.1 -oN os-detection.txt
```

### Important

Because you are scanning your own loopback interface, the result may be limited or uninformative.

This does not mean the operating system is unknown to you. It means the network fingerprinting technique may not provide enough independent information.

You can verify your local operating system directly:

```bash
cat /etc/os-release
```

Compare direct system information with the network-based detection result.

## 14. Practical Lab 7: Compare Direct and Network-Based Information

Create a comparison table:

| Information | Direct Inspection | Nmap |
|---|---|---|
| Operating System | `/etc/os-release` | `-O` |
| Listening Ports | `ss -tuln` | Port scan |
| Service Software | Local package/service information | `-sV` |
| Service Banner | Local configuration or connection | Network response |

Explain why the results may differ.

## 15. Practical Lab 8: Save Your Findings

Create:

```bash
nano service-os-detection-report.txt
```

Use this template:

```text
========================================
SERVICE VERSION AND OS DETECTION REPORT
========================================

1. ASSESSMENT INFORMATION

Date:
Target:
Scope:

2. SERVICE VERSION DETECTION

Port:
Service:
Detected Software:
Reported Version:
Detection Confidence:

3. HTTP BANNER ANALYSIS

HTTP Status:
Server Header:
Content Type:
Other Headers:

4. SSH BANNER ANALYSIS

SSH Service Available:
Reported Banner:
Observations:

5. OPERATING SYSTEM DETECTION

Nmap Command:
Nmap Result:
Confidence:
Limitations:

6. DIRECT OS VERIFICATION

Operating System:
Distribution:
Kernel Information:

7. COMPARISON

Direct Information:
Network-Based Information:
Differences:

8. SECURITY OBSERVATIONS

Outdated Software:
Unexpected Services:
Information Disclosure:
Recommendations:

9. CONCLUSION

Summary:

========================================
END OF REPORT
========================================
```

Complete the report using your actual results.

## 16. Important Security Concepts

### A. Version Information Is Not Proof of Vulnerability

A software version may appear outdated but contain security fixes from a vendor.

Always validate findings against reliable vendor advisories and security documentation.

### B. OS Fingerprinting Is an Estimate

Network responses can suggest an operating system, but they are not always conclusive.

### C. Banners Can Be Misleading

Services may hide or modify their banners.

### D. Evidence Must Be Preserved

Save scan results and record when and how they were obtained.

### E. Authorization Is Mandatory

Only identify services on systems within your approved assessment scope.

## 17. Knowledge Check

Answer these questions:

1. What is service version detection?
2. Why is software version information useful?
3. What does Nmap's `-sV` option do?
4. What is service detection intensity?
5. What does the `-O` option attempt to identify?
6. What is an OS fingerprint?
7. What is a service banner?
8. Why can a service banner be misleading?
9. Why does an old-looking version not automatically prove a vulnerability?
10. What is the difference between direct OS inspection and network-based OS detection?
11. Why is evidence preservation important?
12. What should a security professional do after discovering an outdated service?

## 18. Lesson Completion Checklist

- [ ] Understand service version detection.
- [ ] Understand Nmap `-sV`.
- [ ] Understand version detection intensity.
- [ ] Understand operating system fingerprinting.
- [ ] Understand service banners.
- [ ] Create a local HTTP service.
- [ ] Identify the local HTTP service with Nmap.
- [ ] Inspect HTTP response headers.
- [ ] Check for a local SSH banner if available.
- [ ] Perform local OS detection.
- [ ] Compare direct and network-based information.
- [ ] Save the final report.

## 19. Lesson Summary

Service version and OS detection provide deeper information about network systems.

Remember:

- `-sV` identifies services and attempts version detection.
- `-O` attempts operating system identification.
- Service banners can reveal software information.
- Detection results may be incomplete or inaccurate.
- A detected version does not automatically prove a vulnerability.
- Direct verification and evidence preservation improve assessment quality.

**NEXT LESSON: DNS AND NETWORK ENUMERATION.**
