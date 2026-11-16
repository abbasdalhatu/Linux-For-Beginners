# Module 14 – Lesson 10: Snort Intrusion Detection

## 1. Introduction to Snort

Snort is an open-source network security tool used to analyse network traffic and identify suspicious activities.

It was originally developed by Martin Roesch.

Snort can operate in three major modes:

1. Packet Sniffer.
2. Packet Logger.
3. Network Intrusion Detection System (NIDS).

It can also operate as an Intrusion Prevention System (IPS) when configured appropriately.

### Simple Explanation

Imagine a security officer watching vehicles entering a university.

The officer checks each vehicle against security rules and reports suspicious activity.

Snort performs a similar task for network packets.

It examines network traffic and identifies patterns that may indicate an attack.

### What Can Snort Detect?

- Port scanning.
- Suspicious network connections.
- Known attack signatures.
- Malicious payload patterns.
- Unusual protocol activity.
- Exploitation attempts.
- Policy violations.

---

## 2. Snort Architecture

Snort processes network traffic through several components.

### Major Components

**1. Packet Decoder**

Receives packets and identifies their protocol structure.

**2. Preprocessors and Inspectors**

Prepare and analyse traffic before detection. Their exact architecture differs between Snort versions.

**3. Detection Engine**

Compares network traffic against detection rules.

**4. Logging and Alerting System**

Records detected events and generates alerts.

### Basic Architecture

```text
Network Traffic
      |
      v
Packet Decoder
      |
      v
Traffic Inspection
      |
      v
Detection Engine
      |
      v
Snort Rules
      |
      v
Logging and Alerts
      |
      v
Security Analyst
```

---

## 3. Snort Operating Modes

### A. Packet Sniffer Mode

Snort can capture and display network packets.

Example:

```bash
sudo snort -v
```

This displays packet information.

### B. Packet Logger Mode

Snort can record captured packets for later analysis.

Example:

```bash
sudo snort -dev -l ./logs
```

The command illustrates packet logging. The destination directory must exist and be writable.

### C. Network Intrusion Detection Mode

Snort analyses packets using configured detection rules.

Example:

```bash
sudo snort -c /etc/snort/snort.conf -i eth0
```

The configuration path and interface name depend on the installed version and system.

Do not assume that every installation uses this exact configuration path.

---

## 4. Snort Versions

Two major Snort versions are commonly encountered:

| Feature | Snort 2 | Snort 3 |
|---|---|---|
| Architecture | Older architecture | Redesigned architecture |
| Configuration | Traditionally uses `snort.conf` | Uses Lua configuration |
| Rule support | Snort rules | Snort rules |
| Performance | Established engine | Updated engine and architecture |
| Command options | Version-specific | Version-specific |

Always check the installed version before following configuration instructions.

### Check Version

```bash
snort -V
```

If Snort is not installed, the command may return an error.

---

## 5. Installing Snort on Kali Linux

Kali is Debian-based and provides package management through APT.

First, check whether Snort is already installed:

```bash
snort -V
```

Check package availability:

```bash
apt-cache policy snort
```

If an appropriate package is available, installation can be performed using:

```bash
sudo apt update
sudo apt install snort
```

Installation may request network configuration information depending on the package.

**Important:** Package availability and installed Snort version may vary. Do not install packages from unofficial repositories or run downloaded installation scripts as root.

---

## 6. Understanding Snort Network Interfaces

Snort needs access to network traffic through an appropriate interface.

Examples of Linux interfaces include:

- `eth0`
- `ens33`
- `wlan0`
- `lo`

### Identify Interfaces

```bash
ip addr
```

### Display Available Network Devices

```bash
ip link show
```

### Inspect Network Routes

```bash
ip route
```

### Important

A Snort sensor can only inspect traffic visible to it.

For example, on a switched network, a sensor connected to an ordinary switch port may not automatically see traffic between other devices.

Traffic visibility may require:

- A network TAP.
- A switch mirror port.
- An appropriate monitoring interface.
- A controlled virtual network.

---

## 7. Understanding Snort Rules

Snort rules describe traffic patterns that should trigger an action.

A simplified Snort rule has two main sections:

1. Rule header.
2. Rule options.

### General Structure

```text
action protocol source_ip source_port direction destination_ip destination_port (options)
```

### Example

```snort
alert tcp any any -> 192.168.1.20 22 (msg:"SSH Traffic Detected"; sid:1000001; rev:1;)
```

This illustrative rule generates an alert when matching TCP traffic is observed toward the specified destination and port.

### Understanding the Rule

| Element | Meaning |
|---|---|
| alert | Generate an alert |
| tcp | Match TCP traffic |
| any | Any matching address or port |
| -> | Traffic direction |
| 192.168.1.20 | Destination IP |
| 22 | Destination port |
| msg | Alert description |
| sid | Snort rule identifier |
| rev | Rule revision |

This is a detection example, not a rule to deploy on an uncontrolled network.

---

## 8. Understanding Snort Rule Actions

Common Snort rule actions include:

| Action | Purpose |
|---|---|
| alert | Generate an alert |
| log | Record matching packets |
| pass | Ignore matching traffic |
| drop | Drop matching traffic in supported inline configurations |
| reject | Reject matching traffic in supported configurations |

The available actions and their behaviour depend on the Snort version and operating mode.

An IDS deployment commonly uses alerting rather than inline packet blocking.

---

## 9. Understanding Snort Rule Options

Rule options provide additional matching conditions and information.

### Common Options

**msg**

Defines the alert message.

**sid**

Identifies the Snort rule.

**rev**

Identifies the rule revision.

**content**

Searches packet payloads for specified content.

**flags**

Matches TCP flags.

**flow**

Describes connection direction or state.

### Example

```snort
alert tcp any any -> any 80 (msg:"HTTP Request Pattern"; content:"GET"; sid:1000002; rev:1;)
```

This illustrative rule looks for the text `GET` in matching TCP packet payloads destined for port 80.

Real HTTP inspection requires understanding packet reassembly and application-layer parsing. A simple content match is not a complete HTTP security analysis.

---

## 10. Understanding Snort Rule Identifiers

Snort rules commonly use a Signature ID (SID).

Example:

```text
sid:1000001;
```

The SID identifies a particular rule.

Rules also commonly include a revision number:

```text
rev:1;
```

This indicates the current revision of the rule.

Locally created rules generally use SIDs beginning at 1,000,000 or above to reduce conflicts with published rule sets.

---

## 11. Understanding Snort Configuration

Snort requires configuration information to operate correctly.

Depending on the version, configuration may define:

- Network variables.
- Interface settings.
- Rule files.
- Detection settings.
- Logging options.
- Output formats.
- Preprocessor or inspector configuration.

### Snort 2

Traditionally uses a configuration file such as:

```text
/etc/snort/snort.conf
```

### Snort 3

Uses a Lua-based configuration file, commonly named:

```text
snort.lua
```

The actual file location depends on the installation.

### Configuration Inspection

For Snort 2:

```bash
sudo find /etc/snort -maxdepth 2 -type f
```

For Snort 3, inspect the installation and package documentation to locate its Lua configuration.

Do not overwrite an existing configuration file without creating a backup.

---

## 12. Understanding Snort Alerts

Snort can generate alerts when traffic matches configured detection rules.

An alert may contain:

- Timestamp.
- Signature ID.
- Alert message.
- Source address.
- Destination address.
- Source port.
- Destination port.
- Protocol.

### Example Alert

```text
[**] [1:1000001:1] SSH Traffic Detected [**]
[Priority: 2]
192.168.1.50:51234 -> 192.168.1.20:22
```

Interpretation:

- SID: 1000001.
- Message: SSH Traffic Detected.
- Source: 192.168.1.50.
- Destination: 192.168.1.20.
- Destination port: 22.

This is a fictional example.

### Why Are Alerts Useful?

They help analysts identify:

- Suspicious connections.
- Known attack patterns.
- Potential scanning.
- Unusual network communication.

An alert indicates a match against a detection rule. It does not automatically prove that an attack succeeded.

---

## 13. Understanding Snort Logging

Snort can record information about inspected network traffic.

Logging and output options vary between versions.

Common output information includes:

- Alert records.
- Packet records.
- Protocol details.
- Detection signatures.
- Event timestamps.

### Find Snort Log Directories

```bash
sudo find /var/log -maxdepth 2 -iname '*snort*'
```

### Inspect Existing Snort Files

```bash
sudo find /var/log/snort -maxdepth 2 -type f
```

The second command may report that the directory does not exist if Snort is not installed or configured to use that location.

---

## 14. Understanding Snort Rule Testing

Before deploying detection rules, administrators should validate them.

A rule-testing process may involve:

1. Checking Snort version.
2. Identifying the configuration file.
3. Reviewing the rule syntax.
4. Validating configuration.
5. Testing against controlled traffic.
6. Inspecting generated alerts.
7. Adjusting false positives.
8. Documenting results.

### Configuration Validation

For Snort 2, a common validation command is:

```bash
sudo snort -T -c /etc/snort/snort.conf
```

For Snort 3, the command and configuration path differ depending on the installation.

Use the installed version's help and documentation.

Never assume a Snort 2 command is valid for Snort 3.

---

## 15. Snort Security Best Practices

1. Keep Snort updated.
2. Use trusted rule sources.
3. Validate rules before deployment.
4. Monitor alert quality.
5. Reduce unnecessary false positives.
6. Protect log files.
7. Monitor sensor health.
8. Ensure the sensor can see relevant traffic.
9. Synchronise system clocks.
10. Document configuration changes.
11. Use an isolated test environment.
12. Review alerts regularly.

---

# 16. Practical Lab: Snort Fundamentals

## Objective

Inspect Snort availability, identify network interfaces, understand rule structure, and analyse simulated Snort alerts.

This lab is designed to be safe even if Snort is not installed.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson10
cd ~/module14-lab/lesson10
```

### Step 2: Check Whether Snort Is Installed

```bash
snort -V
```

Record the version if installed.

If the command is unavailable, record that Snort is not currently installed.

### Step 3: Check Package Availability

```bash
apt-cache policy snort
```

Do not install anything unless you have confirmed that installation is necessary and appropriate.

### Step 4: Identify Network Interfaces

```bash
ip addr
```

Record:

- Interface names.
- IP addresses.
- Interface states.

### Step 5: Study a Sample Snort Rule

Create a file:

```bash
nano sample-snort.rules
```

Enter:

```snort
alert tcp any any -> 192.168.1.20 22 (msg:"SSH Traffic Detected"; sid:1000001; rev:1;)

alert tcp any any -> 192.168.1.20 443 (msg:"HTTPS Traffic Detected"; sid:1000002; rev:1;)

alert tcp any any -> 192.168.1.20 80 (msg:"HTTP Traffic Detected"; sid:1000003; rev:1;)
```

Save the file.

These are educational examples. They do not need to be loaded into a running Snort sensor.

### Step 6: Study the Rule Structure

Identify:

- Rule action.
- Protocol.
- Source address.
- Source port.
- Direction operator.
- Destination address.
- Destination port.
- Alert message.
- SID.
- Revision.

### Step 7: Create Sample Snort Alerts

```bash
nano sample-snort-alerts.txt
```

Enter:

```text
[**] [1:1000001:1] SSH Traffic Detected [**]
[Priority: 2]
192.168.1.50:51234 -> 192.168.1.20:22

[**] [1:1000002:1] HTTPS Traffic Detected [**]
[Priority: 3]
192.168.1.60:51000 -> 192.168.1.20:443

[**] [1:1000003:1] HTTP Traffic Detected [**]
[Priority: 3]
192.168.1.70:52000 -> 192.168.1.20:80
```

Save the file.

### Step 8: Analyse the Alerts

```bash
cat sample-snort-alerts.txt
```

Answer:

1. How many alerts were generated?
2. Which alert identifies SSH traffic?
3. Which alert identifies HTTPS traffic?
4. Which destination port is associated with HTTP?
5. What is the purpose of a SID?
6. Why does an alert not automatically prove an attack occurred?

### Step 9: Create Your Investigation Report

```bash
nano snort-investigation-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 10
SNORT INTRUSION DETECTION

1. Snort Installation Status:

2. Snort Version:

3. Network Interfaces:

4. Snort Operating Modes:

5. Snort Architecture:

6. Rule Structure:

7. Rule Actions:

8. Rule Options:

9. Sample Alert Analysis:

10. Snort Logging:

11. Security Recommendations:

12. Conclusion:
```

Save your report.

---

## 17. Knowledge Check

Answer these questions:

1. What is Snort?
2. Who originally developed Snort?
3. Identify the three major Snort operating modes.
4. What is the purpose of the packet decoder?
5. What does the detection engine do?
6. What is a Snort rule?
7. Differentiate between a rule header and rule options.
8. What is the purpose of the `alert` action?
9. What is the purpose of `sid`?
10. What is the purpose of `rev`?
11. Differentiate between Snort 2 and Snort 3 configuration.
12. Why must a Snort sensor have network visibility?
13. What information can a Snort alert contain?
14. Why should detection rules be tested before deployment?
15. Does every Snort alert represent a successful cyberattack? Explain.

---

## 18. Lesson Summary

In this lesson, you learned:

- Snort fundamentals.
- Snort architecture.
- Snort operating modes.
- Snort 2 and Snort 3.
- Installation considerations.
- Network interfaces.
- Snort rule structure.
- Rule actions and options.
- Signature IDs and revisions.
- Configuration files.
- Alert generation.
- Logging.
- Rule validation.
- Snort security best practices.
- Practical alert analysis.

**Next Lesson: Module 14 – Lesson 11: Suricata Intrusion Detection.**