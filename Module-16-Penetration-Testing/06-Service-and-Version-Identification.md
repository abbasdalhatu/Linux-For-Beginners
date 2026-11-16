
# MODULE 16 – LESSON 06
# SERVICE AND VERSION IDENTIFICATION

## 1. Introduction

After discovering open ports during network scanning, the next step in penetration testing is to identify the services running on those ports.

This process is called **Service and Version Identification**.

It helps a security professional understand:
- Which services are running.
- Which software provides those services.
- Which software versions are installed.
- Whether outdated or potentially vulnerable software is present.

### Simple Explanation

Imagine entering a building and discovering three doors.

You identify:
- Door 1: Reception.
- Door 2: Security office.
- Door 3: Server room.

Service identification is similar. It tells us what services are available, while version identification helps determine which software versions provide them.

## 2. Understanding Network Ports

A port is a logical communication endpoint used by network applications.

Ports help computers direct network traffic to the correct service.

Port numbers range from 0 to 65535.

### Common Ports and Services

| Port | Protocol | Common Service |
|---|---|---|
| 20 | TCP | FTP data |
| 21 | TCP | FTP control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 143 | TCP | IMAP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |
| 5432 | TCP | PostgreSQL |
| 8080 | TCP | Alternative HTTP |

**Important:** A port number suggests a common service, but it does not prove which application is actually running there.

## 3. Understanding Services

A service is a program that performs a particular function and may communicate with other computers.

Examples:

### A. SSH

SSH stands for Secure Shell.

It provides encrypted remote access to computers.

Common port: 22.

### B. HTTP

HTTP stands for Hypertext Transfer Protocol.

It enables communication between web browsers and web servers.

Common port: 80.

### C. HTTPS

HTTPS is HTTP protected by TLS encryption.

Common port: 443.

### D. FTP

FTP stands for File Transfer Protocol.

It is used to transfer files between computers.

Common ports: 20 and 21.

### E. DNS

DNS stands for Domain Name System.

It translates domain names into IP addresses and supports other domain-related records.

Common port: 53.

## 4. What Is Version Identification?

Version identification determines the software version associated with a running service.

For example:

```text
Service: OpenSSH
Version: 9.x
Port: 22
Protocol: TCP
```

The exact version depends on the installed software.

### Why Is Version Identification Important?

1. It helps identify outdated software.
2. It supports vulnerability research.
3. It helps administrators plan updates.
4. It helps security professionals document system configurations.
5. It helps identify software that may require additional security review.

**Important:** An old-looking version number does not automatically mean a system is vulnerable. Linux distributions sometimes backport security fixes without changing the upstream version number.

## 5. Banner Grabbing

Banner grabbing is a technique used to collect information that a network service reveals when contacted.

A banner may reveal:
- Software name.
- Software version.
- Protocol information.
- Server details.

Example of a hypothetical banner:

```text
SSH-2.0-OpenSSH_9.2
```

This suggests the service uses SSH protocol version 2.0 and identifies itself as OpenSSH 9.2.

Banners can be inaccurate, incomplete, or deliberately modified. Always verify findings through trusted package records and configuration information where possible.

## 6. Common Service Identification Tools

| Tool | Purpose |
|---|---|
| Nmap | Network discovery and service detection |
| Netcat | Network connection testing |
| curl | HTTP requests and response information |
| ss | Local socket and listening-port inspection |
| systemctl | Linux service management and status |
| dpkg | Debian package information |
| apt | Package management |
| ssh | SSH client and version information |

Use these tools only within systems you own or are authorized to assess.

## 7. Understanding Important Commands

### A. Display Listening Ports

```bash
sudo ss -tulpn
```

Explanation:

- `sudo`: Runs the command with elevated privileges.
- `ss`: Displays socket information.
- `-t`: Shows TCP sockets.
- `-u`: Shows UDP sockets.
- `-l`: Shows listening sockets.
- `-p`: Shows associated processes when permitted.
- `-n`: Displays numeric addresses and port numbers.

### B. Display SSH Version

```bash
ssh -V
```

This displays the installed SSH client version.

### C. Check Installed SSH Packages

```bash
dpkg -l | grep openssh
```

This searches installed package records for OpenSSH packages.

### D. Check SSH Server Package Information

```bash
dpkg -l openssh-server
```

This checks whether the OpenSSH server package is installed.

### E. Check SSH Service Status

```bash
systemctl status ssh
```

This displays the status of the SSH service if the service is available on your system.

### F. Inspect Operating System Information

```bash
cat /etc/os-release
```

This displays information about your Linux distribution.

## 8. Practical Lab: Local Service Identification

In this exercise, we will inspect the services and software versions on your own Kali Linux virtual machine.

We will not scan external systems or other machines.

### Step 1: Create a Lab Directory

```bash
mkdir -p ~/module16-lab/lesson06
cd ~/module16-lab/lesson06
```

### Step 2: Identify Your Operating System

```bash
cat /etc/os-release
```

Record:
- Distribution name.
- Version.
- Operating system ID.

### Step 3: Identify Installed SSH Software

```bash
dpkg -l | grep openssh
```

Record the installed OpenSSH packages and their versions.

### Step 4: Identify the SSH Client Version

```bash
ssh -V
```

Record the version displayed.

### Step 5: Check the SSH Server

```bash
dpkg -l openssh-server
```

Determine whether the SSH server package is installed.

### Step 6: Inspect SSH Service Status

```bash
systemctl status ssh
```

If the service is unavailable, record that result. Do not install or enable it just to make the exercise pass.

Press `q` to leave the status screen.

### Step 7: Identify Listening Services

```bash
sudo ss -tulpn
```

Record the listening ports and protocols.

A listening port is not necessarily accessible from another computer. Its exposure depends on the listening address and network/firewall configuration.

## 9. Understanding Listening Addresses

When examining network services, pay attention to the address on which a service listens.

| Address | Meaning |
|---|---|
| 127.0.0.1 | IPv4 loopback; local machine only |
| 0.0.0.0 | All local IPv4 interfaces |
| ::1 | IPv6 loopback |
| :: | IPv6 wildcard address |

For example:

```text
127.0.0.1:5432
```

This indicates a service listening on the local IPv4 loopback address at port 5432.

Compare that with:

```text
0.0.0.0:5432
```

This indicates a service listening on all local IPv4 interfaces.

The second configuration may expose the service to other network devices, depending on firewall and routing rules.

## 10. Service Inventory Report

Create your report:

```bash
nano service-inventory.txt
```

Use this structure:

```text
MODULE 16 - LESSON 06
SERVICE AND VERSION IDENTIFICATION REPORT

1. SYSTEM INFORMATION

Operating System:
Distribution Version:
Hostname:

2. SSH INFORMATION

SSH Client Version:
OpenSSH Server Installed:
SSH Service Status:

3. LISTENING SERVICES

Service 1:
Port:
Protocol:
Listening Address:

Service 2:
Port:
Protocol:
Listening Address:

4. SECURITY OBSERVATIONS

Observation 1:
Observation 2:
Observation 3:

5. CONCLUSION

What did I discover?
Which services need further review?
What security improvements may be necessary?
```

Save the file.

## 11. Security Risks Associated with Services

Running unnecessary or poorly configured services can increase the attack surface of a computer.

Common risks include:

### A. Outdated Software

Older software may contain known security vulnerabilities.

### B. Unnecessary Services

Services that are not needed may create additional opportunities for unauthorized access.

### C. Weak Authentication

Poor authentication settings can make unauthorized access easier.

### D. Information Disclosure

Excessive service banners may reveal information about software and configurations.

### E. Insecure Protocols

Some older protocols do not provide adequate encryption.

## 12. Defensive Security Recommendations

To improve service security:

1. Remove unnecessary services where appropriate.
2. Keep software updated.
3. Use secure communication protocols.
4. Restrict access to administrative services.
5. Use strong authentication.
6. Review listening ports regularly.
7. Monitor service logs.
8. Apply firewall rules according to operational requirements.
9. Avoid exposing unnecessary software version information.
10. Verify security configurations after changes.

Never disable a service without first understanding whether the system depends on it.

## 13. Knowledge Check

Answer these questions in your notebook:

1. What is a network port?
2. What is a network service?
3. What is service identification?
4. What is version identification?
5. What is banner grabbing?
6. Which port is commonly associated with SSH?
7. What is the purpose of the `ss` command?
8. What does `ssh -V` display?
9. Why is an outdated service potentially dangerous?
10. What is the difference between `127.0.0.1` and `0.0.0.0`?

## 14. Practical Thinking Questions

1. Why should administrators identify all running services?
2. Can a service use a different port from its commonly assigned port?
3. Why should a service banner not be treated as definitive proof of a software version?
4. What risks can arise from exposing SSH to an untrusted network?
5. Why is it important to understand a service before disabling it?

## 15. Lesson Summary

In this lesson, you learned:

- What network ports are.
- How network services operate.
- Common ports and protocols.
- The meaning of version identification.
- How banner grabbing works.
- Tools used for service identification.
- How to inspect local services in Kali Linux.
- How to create a service inventory.
- Security risks associated with exposed services.

**Key takeaway:** Knowing which services are running is not enough. A security professional must understand their software versions, configurations, listening addresses, and potential security risks.

**Next lesson:** Module 16 – Lesson 07: Enumeration Fundamentals.
