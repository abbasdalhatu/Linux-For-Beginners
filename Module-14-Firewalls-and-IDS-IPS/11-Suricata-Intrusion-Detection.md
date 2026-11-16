# Module 14 – Lesson 11: Suricata Intrusion Detection

## 1. Introduction to Suricata

Suricata is an open-source network security engine developed by the Open Information Security Foundation (OISF).

It can perform:

- Network Intrusion Detection (NIDS).
- Network Intrusion Prevention (NIPS).
- Network Security Monitoring (NSM).
- Protocol analysis.
- Network traffic logging.

Suricata examines network traffic and identifies suspicious patterns using detection rules and protocol analysis.

### Simple Explanation

Imagine a security officer monitoring all vehicles entering a university.

The officer checks vehicles, identifies suspicious activity, and records important events.

Suricata performs a similar role in a computer network.

It inspects network traffic, detects suspicious patterns, and generates security alerts.

### What Can Suricata Detect?

- Port scanning.
- Suspicious HTTP requests.
- Malware-related traffic.
- Exploitation attempts.
- Suspicious DNS activity.
- Unusual network connections.
- Known attack signatures.
- Protocol anomalies.

---

## 2. History and Development

Suricata is developed by the Open Information Security Foundation.

It was designed as a modern, high-performance network security monitoring engine.

It supports multi-threaded processing, allowing it to use multiple CPU cores.

This makes it suitable for environments that need to analyse large amounts of network traffic.

### Important Characteristics

- Open-source technology.
- Multi-threaded architecture.
- Supports IDS and IPS operation.
- Supports application-layer protocol detection.
- Supports Snort-compatible rule syntax for many common rules.
- Provides detailed event logging.
- Supports network security monitoring.

---

## 3. Understanding Suricata Architecture

Suricata contains several components that work together.

### A. Packet Capture

Receives network packets from a network interface or another supported capture source.

### B. Packet Decoder

Identifies packet structures and protocols.

### C. Stream Engine

Tracks and reassembles network flows.

This is important because network communication may be divided into multiple packets.

### D. Detection Engine

Compares network traffic against configured detection rules.

### E. Logging and Output System

Records alerts and network events.

### Basic Architecture

```text
Network Traffic
      |
      v
Packet Capture
      |
      v
Packet Decoder
      |
      v
Stream Engine
      |
      v
Detection Engine
      |
      v
Suricata Rules
      |
      v
Alerts and Logs
      |
      v
Security Analyst
```

---

## 4. Understanding Suricata Operating Modes

Suricata supports several operating modes.

### A. IDS Mode

Suricata monitors network traffic and generates alerts when suspicious activity is detected.

It does not automatically block matching traffic in ordinary passive IDS operation.

### B. IPS Mode

Suricata can operate inline to inspect and prevent matching traffic from passing through.

This requires an appropriate inline deployment and configuration.

### C. Network Security Monitoring Mode

Suricata can collect detailed information about network activity.

This can help administrators understand:

- Which systems communicate.
- Which protocols are used.
- When connections occur.
- What application-layer activity is visible.

### Comparison

| Mode | Main Purpose |
|---|---|
| IDS | Detect suspicious activity |
| IPS | Detect and prevent matching traffic |
| NSM | Collect and analyse network activity |

---

## 5. Installing Suricata on Kali Linux

Kali is Debian-based and uses APT.

First, check whether Suricata is already installed:

```bash
suricata --build-info
```

Check package availability:

```bash
apt-cache policy suricata
```

If the package is available and installation is required:

```bash
sudo apt update
sudo apt install suricata
```

Installation may require network access.

Use official Kali/Debian package repositories rather than unknown installation scripts.

### Verify Installation

```bash
suricata --build-info
```

You can also inspect the version:

```bash
suricata -V
```

---

## 6. Understanding Suricata Network Interfaces

Suricata needs access to network traffic to perform network inspection.

Examples of Linux interfaces:

- `eth0`
- `ens33`
- `wlan0`
- `lo`

### Identify Interfaces

```bash
ip addr
```

### Display Network Links

```bash
ip link show
```

### Inspect Routing Information

```bash
ip route
```

### Important

Suricata can only inspect traffic visible to its sensor.

On a switched network, a monitoring sensor may require a TAP or switch mirror port to see traffic between other devices.

For our learning environment, use only the network traffic available inside your authorised Kali VM lab.

---

## 7. Understanding Suricata Configuration

Suricata commonly uses a YAML configuration file.

The usual configuration path on Debian-based installations is:

```text
/etc/suricata/suricata.yaml
```

The configuration file may define:

- Network interfaces.
- Home network variables.
- External network variables.
- Detection rules.
- Logging options.
- Protocol inspection.
- Capture settings.
- Performance settings.

### Inspect Configuration

```bash
sudo less /etc/suricata/suricata.yaml
```

Press `q` to exit.

### Find the Configuration File

```bash
sudo find /etc/suricata -maxdepth 2 -type f
```

### Inspect the Service

```bash
systemctl status suricata
```

The service may be inactive or unavailable if Suricata has not been installed or configured.

---

## 8. Understanding Suricata Rules

Suricata uses a rule language that is compatible with many common Snort rule structures.

A rule defines the traffic conditions that should generate an action.

### General Rule Structure

```text
action protocol source_ip source_port direction destination_ip destination_port (options)
```

### Example Rule

```suricata
alert tcp any any -> 192.168.1.20 22 (msg:"SSH Traffic Detected"; sid:1000001; rev:1;)
```

This illustrative rule generates an alert when matching TCP traffic is observed toward the specified destination and port.

### Rule Components

| Component | Meaning |
|---|---|
| alert | Generate an alert |
| tcp | TCP protocol |
| any | Any matching address or port |
| -> | Traffic direction |
| 192.168.1.20 | Destination IP |
| 22 | Destination port |
| msg | Alert description |
| sid | Rule identifier |
| rev | Rule revision |

---

## 9. Understanding Suricata Rule Actions

Suricata supports different rule actions.

| Action | Purpose |
|---|---|
| alert | Generate an alert |
| pass | Pass matching traffic |
| drop | Drop matching traffic in supported inline modes |
| reject | Reject matching traffic in supported inline modes |
| rejectsrc | Reject traffic from the source |
| rejectdst | Reject traffic toward the destination |
| rejectboth | Reject traffic in both directions |

The actual effect depends on the deployment mode and configuration.

An alert rule is commonly used in passive IDS monitoring.

---

## 10. Understanding Suricata Rule Options

Rule options describe additional conditions and metadata.

### Common Options

**msg**

Defines the alert message.

**sid**

Identifies the rule.

**rev**

Defines the rule revision.

**content**

Searches packet payload data for a specified pattern.

**flow**

Specifies flow direction or connection state.

**flags**

Matches TCP flags.

**classtype**

Classifies the type of activity.

**reference**

Provides a reference to relevant security information.

### Example

```suricata
alert tcp any any -> any 80 (msg:"HTTP GET Pattern"; content:"GET"; sid:1000002; rev:1;)
```

This illustrative rule searches matching TCP payload data for the text `GET`.

It is a basic educational example, not a complete HTTP threat detection rule.

---

## 11. Understanding Suricata Rule Management

Suricata can load detection rules from configured rule files.

A common local rules file is:

```text
/etc/suricata/rules/local.rules
```

The actual location depends on configuration.

### Inspect Available Rule Files

```bash
sudo find /etc/suricata -type f -name "*.rules"
```

### Inspect a Local Rule File

```bash
sudo less /etc/suricata/rules/local.rules
```

The file may not exist until the rules directory has been configured.

### Rule Management Tools

Suricata installations may use `suricata-update` to manage supported rule sources.

Check whether it is available:

```bash
suricata-update --help
```

Only use trusted rule sources.

---

## 12. Understanding Suricata EVE JSON Logs

One of Suricata's important features is its structured logging.

Suricata can generate EVE JSON logs.

A common location is:

```text
/var/log/suricata/eve.json
```

EVE JSON records can contain different event types.

Examples include:

- Alert events.
- DNS events.
- HTTP events.
- TLS events.
- Flow events.
- File information.

### Example Alert Record

```json
{
  "timestamp": "2026-11-15T10:30:25.000000+0000",
  "event_type": "alert",
  "src_ip": "192.168.1.50",
  "dest_ip": "192.168.1.20",
  "dest_port": 22,
  "proto": "TCP",
  "alert": {
    "signature": "SSH Traffic Detected",
    "severity": 2
  }
}
```

This is a fictional example.

### Understanding JSON Fields

| Field | Meaning |
|---|---|
| timestamp | Time of the event |
| event_type | Type of event |
| src_ip | Source IP address |
| dest_ip | Destination IP address |
| dest_port | Destination port |
| proto | Protocol |
| signature | Detection description |
| severity | Alert severity value |

Structured logging makes it easier for other security tools to process Suricata events.

---

## 13. Understanding Suricata Logging

Suricata may produce several types of logs.

| Log | Purpose |
|---|---|
| eve.json | Structured event records |
| fast.log | Human-readable alerts |
| stats.log | Performance statistics |
| suricata.log | Operational messages |

Not every log is enabled in every configuration.

### Inspect Log Directory

```bash
sudo ls -lah /var/log/suricata/
```

### View Recent Alerts

If `fast.log` exists:

```bash
sudo tail -n 20 /var/log/suricata/fast.log
```

### Inspect EVE JSON

If `eve.json` exists:

```bash
sudo tail -n 20 /var/log/suricata/eve.json
```

---

## 14. Understanding Suricata Performance

Suricata is designed for efficient network inspection.

Important performance factors include:

- CPU availability.
- Memory.
- Network traffic volume.
- Packet capture method.
- Rule count.
- Protocol inspection settings.
- Logging volume.

### Why Performance Matters

A network sensor must process packets quickly enough to avoid missing relevant traffic.

If the sensor cannot keep up with traffic, packets may be dropped before inspection.

Administrators should monitor sensor performance and investigate packet drops.

### Useful Inspection

```bash
top
```

This displays system resource usage.

You can also inspect memory:

```bash
free -h
```

---

## 15. Snort vs Suricata

Both are widely used network security engines.

| Feature | Snort | Suricata |
|---|---|---|
| Main purpose | Network threat detection | Network threat detection |
| IDS | Yes | Yes |
| IPS | Yes | Yes |
| Rule language | Snort rules | Snort-compatible rules |
| Multi-threading | Version-dependent | Multi-threaded architecture |
| Structured EVE JSON | Not its standard primary output format | Yes |
| Protocol inspection | Yes | Yes |
| Open source | Yes | Yes |

### Which Should You Learn?

Both are valuable.

Understanding both helps you work with different security environments and monitoring architectures.

---

## 16. Suricata Security Best Practices

1. Keep Suricata updated.
2. Use trusted detection rules.
3. Validate configuration before deployment.
4. Monitor sensor performance.
5. Protect log files.
6. Review alerts regularly.
7. Reduce false positives.
8. Ensure the sensor can see relevant traffic.
9. Monitor packet drops.
10. Synchronise system clocks.
11. Document rule changes.
12. Use isolated test environments.

---

# 17. Practical Lab: Suricata Fundamentals

## Objective

Inspect Suricata availability, understand its configuration, examine sample rules, and analyse simulated EVE JSON alerts.

This lab is inspection-focused and does not require you to deploy Suricata inline or modify firewall rules.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson11
cd ~/module14-lab/lesson11
```

### Step 2: Check Whether Suricata Is Installed

```bash
suricata -V
```

Record the version.

If the command is unavailable, record that Suricata is not installed.

### Step 3: Check Package Availability

```bash
apt-cache policy suricata
```

### Step 4: Identify Network Interfaces

```bash
ip addr
```

Record:

- Interface names.
- IPv4 addresses.
- IPv6 addresses.
- Interface states.

### Step 5: Inspect Suricata Configuration

If Suricata is installed:

```bash
sudo find /etc/suricata -maxdepth 2 -type f
```

Inspect the YAML configuration:

```bash
sudo less /etc/suricata/suricata.yaml
```

Do not edit the configuration.

### Step 6: Create Sample Suricata Rules

```bash
nano sample-suricata.rules
```

Enter:

```suricata
alert tcp any any -> 192.168.1.20 22 (msg:"SSH Traffic Detected"; sid:1000001; rev:1;)

alert tcp any any -> 192.168.1.20 443 (msg:"HTTPS Traffic Detected"; sid:1000002; rev:1;)

alert tcp any any -> 192.168.1.20 80 (msg:"HTTP Traffic Detected"; sid:1000003; rev:1;)
```

Save the file.

These rules are for educational analysis only.

### Step 7: Create a Sample EVE JSON File

```bash
nano sample-eve.json
```

Enter:

```json
{"timestamp":"2026-11-15T10:30:25.000000+0000","event_type":"alert","src_ip":"192.168.1.50","dest_ip":"192.168.1.20","dest_port":22,"proto":"TCP","alert":{"signature":"SSH Traffic Detected","severity":2}}
{"timestamp":"2026-11-15T10:31:25.000000+0000","event_type":"alert","src_ip":"192.168.1.60","dest_ip":"192.168.1.20","dest_port":443,"proto":"TCP","alert":{"signature":"HTTPS Traffic Detected","severity":3}}
{"timestamp":"2026-11-15T10:32:25.000000+0000","event_type":"alert","src_ip":"192.168.1.70","dest_ip":"192.168.1.20","dest_port":80,"proto":"TCP","alert":{"signature":"HTTP Traffic Detected","severity":3}}
```

Save the file.

### Step 8: Validate the JSON File

Use Python's built-in JSON module:

```bash
python3 -c 'import json; [json.loads(line) for line in open("sample-eve.json")]; print("JSON validation successful")'
```

This checks that each line contains valid JSON.

### Step 9: Display the Sample Events

```bash
cat sample-eve.json
```

Answer:

1. How many alert events are present?
2. Which source address triggered the SSH alert?
3. Which event targets port 443?
4. Which event targets port 80?
5. What does `event_type` represent?
6. Why is JSON useful for security monitoring?

### Step 10: Create Your Investigation Report

```bash
nano suricata-investigation-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 11
SURICATA INTRUSION DETECTION

1. Suricata Installation Status:

2. Suricata Version:

3. Network Interfaces:

4. Suricata Architecture:

5. Operating Modes:

6. Configuration File:

7. Rule Structure:

8. Rule Actions:

9. EVE JSON Explanation:

10. Sample Alert Analysis:

11. Snort vs Suricata:

12. Security Recommendations:

13. Conclusion:
```

Save your report.

---

## 18. Knowledge Check

Answer these questions:

1. What is Suricata?
2. Which organisation develops Suricata?
3. What are the three major security capabilities of Suricata?
4. What is the purpose of the packet decoder?
5. What does the stream engine do?
6. Explain the difference between IDS and IPS modes.
7. What is the purpose of `suricata.yaml`?
8. What is a Suricata rule?
9. Explain the purpose of `sid` and `rev`.
10. What is EVE JSON?
11. Name four types of events that Suricata can record.
12. What is the purpose of `fast.log`?
13. Why is sensor performance important?
14. Give three differences between Snort and Suricata.
15. Why must Suricata be able to observe the traffic it is expected to analyse?

---

## 19. Lesson Summary

In this lesson, you learned:

- Suricata fundamentals.
- Suricata architecture.
- IDS, IPS, and NSM modes.
- Installation and verification.
- Network interface inspection.
- Suricata configuration.
- Rule structure and actions.
- Rule management.
- EVE JSON logging.
- Fast and operational logs.
- Performance monitoring.
- Snort vs Suricata.
- Suricata security best practices.
- Practical alert analysis.

**Next Lesson: Module 14 – Lesson 12: Intrusion Prevention Systems (IPS).**