# Module 10 – Final Assessment: Linux Networking

## 1. Assessment Information

**Module:** 10 – Linux Networking

**Assessment Type:** Theory and Practical

**Total Marks:** 100

**Recommended Pass Mark:** 50%

**Instructions:**

- Answer all sections.
- Use your own words.
- Show calculations where required.
- Perform practical exercises only on your own Kali VM.
- Do not modify active network settings unnecessarily.
- Document your observations.

---

# SECTION A: Multiple-Choice Questions

**20 Questions – 2 Marks Each = 40 Marks**

Choose the correct answer.

### 1. What is the main purpose of an IP address?

A. Encrypting files

B. Identifying a device on an IP network

C. Managing Linux packages

D. Creating user accounts

### 2. Which command displays Linux network interfaces?

A. `ls`

B. `ps`

C. `ip addr`

D. `chmod`

### 3. Which IPv4 address is the loopback address?

A. `192.168.1.1`

B. `8.8.8.8`

C. `127.0.0.1`

D. `10.0.0.1`

### 4. What does CIDR `/24` represent?

A. 24 network bits

B. 24 host bits

C. 24 IP addresses

D. 24 network interfaces

### 5. What is the main purpose of a default gateway?

A. Store files

B. Forward traffic to other networks

C. Encrypt passwords

D. Manage processes

### 6. Which protocol automatically assigns IP configuration?

A. FTP

B. SSH

C. DHCP

D. HTTP

### 7. Which command displays the Linux routing table?

A. `ip route`

B. `ip user`

C. `ls route`

D. `netstat file`

### 8. Which command tests basic IP connectivity?

A. `mkdir`

B. `ping`

C. `touch`

D. `passwd`

### 9. What is the purpose of DNS?

A. Assign file permissions

B. Resolve domain names

C. Monitor CPU usage

D. Create network interfaces

### 10. Which port is commonly used by SSH?

A. 21

B. 22

C. 80

D. 443

### 11. Which transport protocol provides reliable, ordered delivery?

A. UDP

B. ICMP

C. TCP

D. ARP

### 12. Which protocol is commonly used for HTTPS?

A. HTTP over TLS

B. FTP over UDP

C. DNS over ICMP

D. ARP over TCP

### 13. What does `ss -tuln` display?

A. User accounts

B. Listening TCP and UDP sockets

C. Disk partitions

D. Installed packages

### 14. Which command displays network interface statistics?

A. `ip -s link`

B. `pwd`

C. `whoami`

D. `df -h`

### 15. Which tool is commonly used for graphical packet analysis?

A. Wireshark

B. Nano

C. Bash

D. APT

### 16. Which command captures network packets from the terminal?

A. `tcpdump`

B. `chmod`

C. `grep`

D. `useradd`

### 17. What is the main purpose of a firewall?

A. Increase RAM

B. Control network traffic

C. Install applications

D. Create directories

### 18. What does UFW stand for?

A. Universal File Workspace

B. Uncomplicated Firewall

C. Unified Web Framework

D. User Firewall Window

### 19. What is a Man-in-the-Middle attack?

A. An attack that intercepts or manipulates communication

B. A method of installing Linux

C. A disk-cleaning technique

D. A package-management method

### 20. What is the main purpose of network monitoring?

A. Delete unused files

B. Observe network activity and identify problems

C. Change desktop themes

D. Create Linux users

---

# SECTION B: Short-Answer Questions

**10 Questions – 2 Marks Each = 20 Marks**

Answer in clear sentences.

1. Define computer networking.
2. Explain the difference between IPv4 and IPv6.
3. What is a subnet mask?
4. Explain the difference between a static IP and a dynamic IP.
5. What is DHCP, and how does DORA work?
6. Explain the difference between TCP and UDP.
7. What is DNS, and why is it important?
8. Explain the difference between HTTP and HTTPS.
9. What is packet capture, and why is it useful?
10. Explain why firewalls are important in Linux security.

---

# SECTION C: Practical Linux Networking

**5 Tasks – 4 Marks Each = 20 Marks**

Perform the following exercises in your Kali Linux VM.

### Task 1: Network Interface Inspection

Execute:

```bash
ip -br addr
```

Answer:

- What is your active interface?
- What IPv4 address is assigned?
- Is the loopback interface available?

### Task 2: Routing Inspection

Execute:

```bash
ip route
```

Answer:

- What is your default gateway?
- Which interface is used for the default route?
- What does the default route represent?

### Task 3: DNS Investigation

Execute:

```bash
dig example.com
```

Answer:

- What IP address was returned?
- Which DNS server handled the query?
- What does the ANSWER section represent?

### Task 4: Socket Inspection

Execute:

```bash
sudo ss -tulnp
```

Answer:

- Identify one listening TCP service, if present.
- Identify one listening UDP service, if present.
- Which application owns one of the listening sockets?

### Task 5: Network Connectivity

Execute:

```bash
ping -c 4 1.1.1.1
```

Then:

```bash
getent hosts example.com
```

Answer:

- Did the IP connectivity test succeed?
- Did hostname resolution succeed?
- What could cause one test to succeed while the other fails?

---

# SECTION D: Problem-Solving and Analysis

**4 Questions – 5 Marks Each = 20 Marks**

Show your working.

### Question 1: Subnetting

Given:

```text
IP Address: 192.168.10.70/26
```

Calculate:

1. Subnet mask.
2. Network address.
3. Broadcast address.
4. First usable host address.
5. Last usable host address.

### Question 2: Network Troubleshooting

A Linux computer can ping `1.1.1.1`, but it cannot open `example.com`.

Explain:

1. What this tells you about basic IP connectivity.
2. Which network service you should investigate.
3. Two Linux commands you can use.
4. Two possible causes.

### Question 3: Security Analysis

You execute:

```bash
sudo ss -tulnp
```

You discover an unfamiliar application listening on TCP port 8080.

Explain:

1. What a listening port means.
2. How you can identify the application.
3. Why you should investigate it.
4. Two defensive actions you could consider after confirming it is unnecessary or unauthorized.

### Question 4: Packet Monitoring

A cybersecurity analyst captures network traffic using:

```bash
sudo tcpdump -i eth0 -nn -c 20
```

Explain:

1. What tcpdump does.
2. What `-i` means.
3. What `-nn` means.
4. What `-c 20` means.
5. Why packet capture is useful in incident investigation.

---

# SECTION E: Practical Security Audit

**Optional Extended Exercise**

Perform a basic security inspection of your Kali VM.

Use:

```bash
ip -br addr
```

```bash
ip route
```

```bash
sudo ss -tulnp
```

```bash
sudo ufw status verbose
```

```bash
sudo nft list ruleset
```

```bash
ip neigh
```

Write a short report containing:

1. Network interfaces identified.
2. Default gateway.
3. Listening services.
4. Firewall status.
5. Existing nftables rules.
6. Neighbor information.
7. Potential security improvements.

Do not change firewall or network configuration during this assessment.

---

# 2. Grading Guide

| Section | Marks |
|---|---:|
| A – Multiple Choice | 40 |
| B – Short Answers | 20 |
| C – Practical Exercises | 20 |
| D – Problem Solving | 20 |
| **Total** | **100** |

### Performance Interpretation

| Score | Performance |
|---|---|
| 80–100 | Excellent |
| 70–79 | Very Good |
| 60–69 | Good |
| 50–59 | Pass |
| Below 50 | Needs Improvement |

---

# 3. Final Competency Checklist

After completing Module 10, you should be able to:

- [ ] Explain Linux networking fundamentals.
- [ ] Identify network interfaces.
- [ ] Understand IPv4 and IPv6.
- [ ] Calculate basic subnets.
- [ ] Understand gateways and routing.
- [ ] Use Linux `ip` commands.
- [ ] Test network connectivity.
- [ ] Investigate DNS.
- [ ] Understand ports and sockets.
- [ ] Troubleshoot network problems.
- [ ] Understand static and dynamic network configuration.
- [ ] Explain major network protocols.
- [ ] Monitor network activity.
- [ ] Capture and analyze packets.
- [ ] Inspect basic Linux network security.
- [ ] Explain firewall fundamentals.

---

# 4. Module Completion

**Module 10: Linux Networking**

Completion status: Pending assessment.

Once you have completed this assessment, mark the module as completed in your course roadmap.

**Congratulations on reaching the end of Module 10!**

Your next module is:

**Module 11 – Bash Scripting**

This module will introduce you to automating Linux tasks using shell scripts, variables, conditions, loops, functions, and practical administration scripts.