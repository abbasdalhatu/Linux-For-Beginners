
# MODULE 15 – CAPSTONE PROJECT

# LESSON 06: NETWORK MONITORING AND DETECTION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain network monitoring.
2. Understand network traffic and packets.
3. Monitor network interfaces.
4. Inspect active connections.
5. Analyze system and authentication logs.
6. Understand IDS and IPS.
7. Identify suspicious network activity.
8. Document monitoring findings.

---

## 2. What Is Network Monitoring?

Network monitoring is the continuous observation of network devices, connections, traffic, and services to understand their behavior and identify problems.

Organizations monitor networks to detect:

- Unauthorized connections.
- Unusual traffic patterns.
- Failed authentication attempts.
- Unexpected service activity.
- Network failures.
- Potential security incidents.

### Example

Suppose a Linux server normally receives connections from a small number of internal computers.

Suddenly, it begins receiving thousands of unexpected connection attempts.

A monitoring system may identify this unusual activity and alert an administrator.

---

## 3. Understanding Network Traffic

Network traffic is the data transmitted between devices over a network.

Common protocols include:

| Protocol | Purpose |
|---|---|
| TCP | Reliable connection-oriented communication |
| UDP | Connectionless communication |
| ICMP | Network diagnostics and control messages |
| DNS | Domain-name resolution |
| HTTP | Web communication |
| HTTPS | Encrypted web communication |
| SSH | Secure remote administration |

Network monitoring helps administrators understand which protocols are being used and whether their usage is expected.

---

## 4. Understanding Network Packets

A packet is a formatted unit of data transmitted across a network.

A typical IP packet contains:

- Source IP address.
- Destination IP address.
- Protocol information.
- Payload.
- Other control information.

For example:

```text
Source:      192.168.56.10
Destination: 192.168.56.20
Protocol:    TCP
Destination port: 22
```

This could represent a connection attempt to an SSH service.

The addresses above are illustrative only.

---

## 5. Monitoring vs Detection

| Monitoring | Detection |
|---|---|
| Observes system activity | Identifies potentially suspicious behavior |
| Collects information | Analyzes collected information |
| Tracks normal operations | Looks for deviations or known threats |
| Provides visibility | Helps generate security alerts |

Monitoring provides the data needed for effective detection.

---

## 6. Practical Lab A: Inspect Network Interfaces

Open Kali Linux.

Display network interfaces:

```bash
ip -br addr
```

Display interface details:

```bash
ip link show
```

Inspect routing information:

```bash
ip route
```

Display interface statistics:

```bash
ip -s link
```

Look for:

- Interface names.
- IP addresses.
- Interface status.
- Received packets.
- Transmitted packets.
- Errors and dropped packets.

### Task

Record the following:

1. Name of your Host-only interface.
2. Its IP address.
3. Its current state.
4. Received packet count.
5. Transmitted packet count.

---

## 7. Practical Lab B: Monitor Active Network Connections

Linux provides several tools for inspecting network connections.

### Command 1: ss

```bash
ss -tuln
```

Meaning:

- `-t`: TCP sockets.
- `-u`: UDP sockets.
- `-l`: Listening sockets.
- `-n`: Numeric addresses and ports.

Display established TCP connections:

```bash
ss -tn state established
```

Display processes associated with sockets:

```bash
sudo ss -tulpn
```

### Command 2: ip neigh

```bash
ip neigh
```

This displays neighboring devices known to the local network stack.

### Task

Record:

- Listening TCP ports.
- Listening UDP ports.
- Established connections.
- Any unexpected services.

Do not assume that every listening port is malicious.

---

## 8. Practical Lab C: Monitor Network Traffic

We will use tcpdump to inspect traffic in the authorized isolated laboratory.

Check whether it is installed:

```bash
tcpdump --version
```

If unavailable:

```bash
sudo apt update
sudo apt install tcpdump
```

List available interfaces:

```bash
sudo tcpdump -D
```

### Capture a Small Number of Packets

Replace `INTERFACE` with your actual Host-only interface:

```bash
sudo tcpdump -i INTERFACE -c 20
```

This captures up to 20 packets.

### Capture ICMP Traffic

```bash
sudo tcpdump -i INTERFACE icmp -c 10
```

In another terminal, perform a few authorized connectivity tests against your Metasploitable VM:

```bash
ping -c 4 TARGET_IP
```

### Capture TCP Traffic for a Specific Port

For example, inspect SSH traffic:

```bash
sudo tcpdump -i INTERFACE 'tcp port 22' -c 20
```

Only use your isolated lab interface.

### Important

Packet captures may contain sensitive information. Keep them within your private lab and do not publish or share captures containing credentials or personal data.

---

## 9. Practical Lab D: Save Network Traffic

Create a monitoring directory:

```bash
mkdir -p ~/practical-labs/module15/monitoring
```

Capture a limited number of packets:

```bash
sudo tcpdump -i INTERFACE -c 50 -w ~/practical-labs/module15/monitoring/lab-capture.pcap
```

Generate a few test packets from another terminal while the capture runs.

If the capture does not stop automatically, press:

```text
CTRL + C
```

Read the capture:

```bash
tcpdump -r ~/practical-labs/module15/monitoring/lab-capture.pcap
```

The `.pcap` file can also be opened with Wireshark.

---

## 10. Understanding IDS and IPS

### IDS – Intrusion Detection System

An IDS monitors network or system activity and identifies suspicious behavior.

It typically generates alerts.

Example:

A network IDS detects repeated connection attempts against a server and alerts the security administrator.

### IPS – Intrusion Prevention System

An IPS detects suspicious activity and can automatically take action to block or prevent it.

Example:

An IPS identifies traffic matching a malicious pattern and blocks the traffic according to its security policy.

### IDS vs IPS

| IDS | IPS |
|---|---|
| Detects suspicious activity | Detects and can prevent activity |
| Usually alerts administrators | Can block traffic automatically |
| Often operates out of band | Commonly deployed inline |
| Does not normally block traffic itself | Can enforce prevention rules |

---

## 11. Understanding Common Detection Tools

### 11.1 Snort

Snort is an open-source network intrusion detection and prevention system.

It can inspect network traffic using defined rules.

### 11.2 Suricata

Suricata is a network threat detection engine that supports IDS, IPS, and network security monitoring.

### 11.3 Wireshark

Wireshark is a graphical packet analyzer used to inspect network communications.

### 11.4 tcpdump

tcpdump is a command-line packet capture and analysis tool.

### Tool Comparison

| Tool | Main Purpose |
|---|---|
| Snort | Network intrusion detection/prevention |
| Suricata | Network threat detection and prevention |
| Wireshark | Packet analysis |
| tcpdump | Command-line packet capture |

For this lesson, tcpdump is sufficient. We will not deploy additional IDS software.

---

## 12. Practical Lab E: Inspect System Logs

Logs provide records of system activity.

View recent system messages:

```bash
sudo journalctl -n 30
```

View recent errors:

```bash
sudo journalctl -p err -n 20
```

View authentication-related logs where available:

```bash
sudo journalctl -u ssh -n 30
```

Check whether authentication logs exist:

```bash
ls -l /var/log/auth.log
```

If the file exists:

```bash
sudo tail -n 30 /var/log/auth.log
```

Different Linux distributions and configurations store authentication events differently.

### What to Look For

- Failed login attempts.
- Successful authentication events.
- Unexpected service restarts.
- Permission errors.
- Unusual system activity.

Do not assume that a single failed login represents an attack.

---

## 13. Practical Lab F: Identify Suspicious Activity

Consider these examples:

### Scenario 1: Repeated Failed Logins

A server records many failed SSH login attempts.

Possible explanations:
- Incorrect user credentials.
- Misconfigured automated application.
- Unauthorized password guessing.

Investigation:
- Check source addresses.
- Review timestamps.
- Identify affected accounts.
- Look for successful logins following failures.

### Scenario 2: Unexpected Listening Port

A server exposes a service that the administrator does not recognize.

Investigation:
- Identify the process.
- Determine which package owns it.
- Check whether the service is required.
- Review its configuration.

Useful commands:

```bash
sudo ss -tulpn
```

```bash
systemctl list-units --type=service --state=running
```

### Scenario 3: Unusual Network Traffic

A system sends an unexpected amount of traffic.

Investigation:
- Identify the source process.
- Inspect destination addresses.
- Examine protocol usage.
- Compare with normal behavior.

Useful commands:

```bash
ip -s link
```

```bash
ss -tunap
```

---

## 14. Practical Lab G: Create a Monitoring Report

Create the monitoring directory:

```bash
mkdir -p ~/practical-labs/module15/monitoring
```

Create the report:

```bash
cat > ~/practical-labs/module15/monitoring/monitoring-report.md <<'EOF'
# NETWORK MONITORING REPORT

## 1. Project Information

Project:
Isolated Cybersecurity Assessment and Defense Lab

Activity:
Network Monitoring and Detection

## 2. Monitoring Environment

Operating System:
Kali Linux

Network:
VMware Host-only

Target:
Authorized Metasploitable 2 VM

## 3. Tools Used

- ip
- ss
- tcpdump
- journalctl

## 4. Interface Observations

[Record interface name, address and statistics.]

## 5. Connection Observations

[Record listening ports and established connections.]

## 6. Traffic Analysis

[Describe the packets observed.]

## 7. Log Analysis

[Document relevant system events.]

## 8. Suspicious Activity

[Record suspicious activity, or state that none was observed.]

## 9. Limitations

This was a limited observation of an isolated
training environment, not continuous production monitoring.

## 10. Conclusion

[Summarize the monitoring results.]
EOF
```

Edit it:

```bash
nano ~/practical-labs/module15/monitoring/monitoring-report.md
```

---

## 15. Practical Lab H: Understand Evidence Integrity

Monitoring evidence should be preserved carefully.

Calculate the SHA-256 hash of your packet capture:

```bash
sha256sum ~/practical-labs/module15/monitoring/lab-capture.pcap
```

Save the result:

```bash
sha256sum ~/practical-labs/module15/monitoring/lab-capture.pcap > ~/practical-labs/module15/monitoring/capture-hash.txt
```

Verify it:

```bash
sha256sum -c ~/practical-labs/module15/monitoring/capture-hash.txt
```

Expected result:

```text
lab-capture.pcap: OK
```

A matching hash provides evidence that the file has not changed since the recorded hash was calculated. It does not prove who created the file or whether the original capture was accurate.

---

## 16. Knowledge Check

1. What is network monitoring?
2. What is a network packet?
3. What is the difference between monitoring and detection?
4. What does `ss -tuln` display?
5. What is tcpdump used for?
6. What is an IDS?
7. What is an IPS?
8. What is the difference between Snort and Wireshark?
9. Why are authentication logs important?
10. Why should packet captures be hashed?

---

## 17. Practical Completion Checklist

- [ ] I inspected network interfaces.
- [ ] I reviewed interface statistics.
- [ ] I inspected listening sockets.
- [ ] I examined established connections.
- [ ] I captured limited traffic on my Host-only interface.
- [ ] I saved a PCAP file.
- [ ] I reviewed system logs.
- [ ] I examined authentication events where available.
- [ ] I calculated and verified a SHA-256 hash.
- [ ] I created a monitoring report.

---

## 18. Key Takeaway

Network monitoring gives security professionals visibility into system and network activity.

Detection uses that information to identify suspicious behavior and support incident response.

**Remember: You cannot reliably detect abnormal activity if you do not understand what normal activity looks like.**
