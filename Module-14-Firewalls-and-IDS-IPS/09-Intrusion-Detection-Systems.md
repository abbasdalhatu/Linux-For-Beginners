---
---

# Module 14 – Lesson 09: Intrusion Detection Systems (IDS)

## 1. Introduction to Intrusion Detection Systems

An Intrusion Detection System (IDS) is a security technology that monitors computers or network traffic to identify suspicious activities, security violations, and possible attacks.

An IDS analyses activity and generates alerts when it detects behaviour that matches known attack patterns or unusual activity.

### Simple Explanation

Imagine a security guard monitoring a university building.

The guard watches people entering and leaving, looks for suspicious behaviour, and reports possible security problems.

An IDS performs a similar function in a computer network.

It monitors digital activity and alerts security administrators when something suspicious happens.

### What Can an IDS Detect?

- Port scanning.
- Brute-force attempts.
- Suspicious network connections.
- Malware-related communication.
- Unusual traffic patterns.
- Exploitation attempts.
- Policy violations.
- Unauthorised access attempts.

**Important:** An IDS detects and reports suspicious activity. A traditional IDS does not automatically block the traffic it detects.

---

## 2. Why IDS Is Important

Firewalls control network traffic based on security rules.

However, allowing a connection does not guarantee that the communication is safe.

For example:

A firewall permits HTTPS traffic through port 443.

An attacker may attempt to exploit a vulnerable web application using that permitted connection.

The firewall may allow the traffic because it matches the configured network policy.

An IDS can inspect the traffic and identify suspicious patterns.

### Major Benefits

1. Detects suspicious activities.
2. Provides security alerts.
3. Helps identify possible attacks.
4. Supports incident investigation.
5. Monitors network behaviour.
6. Helps enforce security policies.
7. Provides visibility into network activity.

---

## 3. Understanding How an IDS Works

An IDS generally follows these steps:

1. Collects data.
2. Monitors activity.
3. Analyses the collected information.
4. Compares activity against detection rules or behavioural models.
5. Identifies suspicious events.
6. Generates alerts.
7. Stores information for investigation.

### Example

Suppose an attacker attempts to scan a Linux server.

The IDS observes connections to several different ports.

It compares the activity against its detection rules.

If the pattern matches a port-scanning signature, the IDS generates an alert.

### Basic Architecture

```text
Network Traffic
      |
      v
IDS Sensor
      |
      v
Traffic Analysis
      |
      v
Detection Engine
      |
      v
Security Alert
      |
      v
Security Analyst
```

---

## 4. Types of Intrusion Detection Systems

There are several types of IDS technologies.

The two major categories are:

1. Network-based IDS (NIDS).
2. Host-based IDS (HIDS).

Other approaches include wireless IDS and application-based IDS.

### A. Network-Based IDS (NIDS)

A Network-Based Intrusion Detection System monitors network traffic.

It is commonly deployed at strategic points within a network.

Examples of monitoring locations:

- Network gateways.
- Server network segments.
- Data centre network connections.
- Internal network boundaries.

**Example:**

An IDS sensor monitors traffic entering a university network and identifies suspicious connection patterns.

### B. Host-Based IDS (HIDS)

A Host-Based Intrusion Detection System monitors activity on an individual computer or server.

It may examine:

- System logs.
- File integrity.
- Running processes.
- User activity.
- Configuration changes.
- Authentication events.

**Example:**

A HIDS monitors important Linux system files and alerts administrators when unexpected modifications occur.

### C. Wireless IDS (WIDS)

A Wireless Intrusion Detection System monitors wireless networks.

It can help identify:

- Unauthorised wireless access points.
- Suspicious wireless devices.
- Rogue access points.
- Unusual wireless activity.

### D. Application-Based IDS

An Application-Based Intrusion Detection System monitors activity within a particular application.

For example, it may identify suspicious activity involving a web application or database system.

---

## 5. NIDS vs HIDS

| Feature | NIDS | HIDS |
|---|---|---|
| Full name | Network IDS | Host IDS |
| Monitoring | Network traffic | Individual host |
| Location | Network segment | Computer or server |
| Main focus | Network-based threats | Host-based threats |
| Example | Suricata | OSSEC |
| Visibility | Traffic visible to sensor | Activity visible on monitored host |

### Which Is Better?

Neither is universally better.

They provide different types of visibility.

A strong security architecture may combine both approaches.

For example:

- NIDS monitors network communications.
- HIDS monitors server activity.
- SIEM correlates their alerts.

---

## 6. Understanding IDS Detection Methods

IDS technologies use different detection techniques.

Three important approaches are:

1. Signature-based detection.
2. Anomaly-based detection.
3. Stateful protocol analysis.

### A. Signature-Based Detection

Signature-based detection identifies activity that matches known attack patterns.

A signature is a detection pattern associated with a particular threat or behaviour.

Example:

An IDS has a rule that identifies a known malicious payload pattern.

When matching traffic appears, the IDS generates an alert.

**Advantages:**

- Effective against known threats.
- Can produce specific alerts.
- Easier to investigate when signatures are accurate.

**Limitations:**

- May not detect completely new attacks.
- Requires updated signatures.
- Attackers may modify known attack patterns.

### B. Anomaly-Based Detection

Anomaly-based detection identifies activity that differs from established normal behaviour.

Example:

A server normally receives 100 connection requests per minute.

Suddenly, it receives 10,000 connection requests per minute.

The unusual activity may trigger an alert.

**Advantages:**

- Can identify previously unseen suspicious behaviour.
- Useful for detecting unusual activity.
- Can reveal deviations from normal network patterns.

**Limitations:**

- Normal behaviour can change.
- May generate false positives.
- Requires suitable baselines and tuning.

### C. Stateful Protocol Analysis

This technique examines whether network communications follow expected protocol behaviour.

For example, an IDS may analyse whether a TCP connection follows a valid sequence of protocol operations.

Unexpected or malformed communication may trigger an alert.

### Comparison

| Method | How It Detects Threats |
|---|---|
| Signature-based | Matches known patterns |
| Anomaly-based | Identifies unusual behaviour |
| Stateful protocol analysis | Checks protocol behaviour |

Modern detection systems may combine multiple techniques.

---

## 7. Understanding IDS Alerts

An IDS alert is a notification generated when the system identifies suspicious activity.

An alert may contain:

- Alert ID.
- Timestamp.
- Source IP address.
- Destination IP address.
- Protocol.
- Source port.
- Destination port.
- Detection signature.
- Severity.
- Description.

### Example IDS Alert

```text
ALERT ID: 1001
TIME: 10:30:25
SOURCE IP: 192.168.1.50
DESTINATION IP: 192.168.1.20
PROTOCOL: TCP
DESTINATION PORT: 22
SIGNATURE: Possible SSH Brute Force
SEVERITY: HIGH
```

This is a fictional example.

### Understanding Severity

Organisations may classify alerts using levels such as:

| Severity | Meaning |
|---|---|
| Critical | Potentially severe security incident |
| High | Serious suspicious activity |
| Medium | Activity requiring investigation |
| Low | Minor or informational event |
| Informational | Useful monitoring information |

Severity labels and scoring systems vary between IDS products.

---

## 8. Understanding False Positives and False Negatives

IDS detection is not perfect.

Two important concepts are false positives and false negatives.

### A. False Positive

A false positive occurs when legitimate activity is incorrectly identified as suspicious.

Example:

A legitimate administrator performs many SSH connection attempts during maintenance.

The IDS incorrectly flags the activity as a possible attack.

### B. False Negative

A false negative occurs when malicious activity is not detected.

Example:

An attacker uses a modified attack technique that does not match the IDS detection rules.

The IDS fails to generate an alert.

### Comparison

| Concept | Meaning |
|---|---|
| True Positive | Actual threat correctly detected |
| False Positive | Legitimate activity incorrectly flagged |
| True Negative | Legitimate activity correctly recognised |
| False Negative | Actual threat missed |

### Why Is This Important?

Too many false positives can cause alert fatigue.

Security analysts may become overwhelmed by unnecessary alerts and overlook important incidents.

False negatives are also dangerous because real threats may go unnoticed.

---

## 9. Understanding IDS Deployment

An IDS must be placed where it can observe relevant activity.

### Network IDS Deployment

A NIDS sensor may be connected to:

- A network tap.
- A switch mirror port.
- A monitoring interface.
- A suitable network gateway.

### Host IDS Deployment

A HIDS agent may be installed directly on:

- Linux servers.
- Windows computers.
- Database servers.
- Application servers.

### Example Network Architecture

```text
              INTERNET
                  |
                  |
               FIREWALL
                  |
                  |
             NETWORK SWITCH
                  |
          +-------+-------+
          |               |
          |               |
       SERVERS         WORKSTATIONS
          |
          |
      NIDS SENSOR
          |
          |
     SECURITY TEAM
```

A real deployment needs appropriate network visibility and sensor placement. A sensor cannot analyse traffic it cannot observe.

---

## 10. IDS vs Firewall

A firewall and IDS perform different security functions.

| Feature | Firewall | IDS |
|---|---|---|
| Primary purpose | Traffic control | Threat detection |
| Main function | Allows or blocks traffic | Analyses activity |
| Rule-based operation | Yes | Yes |
| Generates alerts | Can | Yes |
| Inspects suspicious patterns | Depending on capability | Yes |
| Automatically blocks attacks | Yes, according to policy | Usually no |

### Example

A firewall allows HTTPS traffic.

An IDS analyses the permitted traffic and detects a suspicious request.

The firewall controls network access while the IDS provides additional security monitoring.

---

## 11. Popular IDS Technologies

Several technologies are commonly used for intrusion detection.

### A. Snort

Snort is an open-source network intrusion detection and prevention technology.

It supports rule-based traffic analysis.

### B. Suricata

Suricata is an open-source network threat detection engine.

It supports intrusion detection, intrusion prevention, and network security monitoring.

### C. OSSEC

OSSEC is an open-source host-based intrusion detection system.

It supports log analysis, file integrity monitoring, and host security monitoring.

### D. Wazuh

Wazuh is an open-source security platform that provides capabilities including:

- Security event analysis.
- File integrity monitoring.
- Vulnerability detection.
- Security configuration assessment.
- Log analysis.

Wazuh can be integrated into broader security monitoring environments.

---

## 12. IDS and Cybersecurity Careers

Understanding IDS is useful in several cybersecurity roles.

### Security Operations Centre (SOC) Analyst

Monitors alerts and investigates suspicious activity.

### Network Security Engineer

Designs and manages network security monitoring systems.

### Incident Responder

Uses security evidence to investigate and respond to incidents.

### Threat Hunter

Searches for suspicious activity that may not have triggered ordinary alerts.

### Security Engineer

Deploys, configures, and maintains detection technologies.

---

# 13. Practical Lab: Understanding IDS Concepts

## Objective

Learn how IDS alerts work and practise analysing simulated security events.

This lab uses fictional events. You will not generate attacks or modify firewall configurations.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson09
cd ~/module14-lab/lesson09
```

### Step 2: Create a Sample IDS Alert File

```bash
nano sample-ids-alerts.txt
```

Enter:

```text
ALERT 001
TIME: 09:10:00
SOURCE: 192.168.1.50
DESTINATION: 192.168.1.20
PORT: 22
PROTOCOL: TCP
SIGNATURE: Possible SSH Brute Force
SEVERITY: HIGH

ALERT 002
TIME: 09:12:00
SOURCE: 192.168.1.60
DESTINATION: 192.168.1.20
PORT: 443
PROTOCOL: TCP
SIGNATURE: Suspicious HTTP Request Pattern
SEVERITY: MEDIUM

ALERT 003
TIME: 09:15:00
SOURCE: 192.168.1.70
DESTINATION: 192.168.1.20
PORT: MULTIPLE
PROTOCOL: TCP
SIGNATURE: Possible Port Scan
SEVERITY: HIGH

ALERT 004
TIME: 09:20:00
SOURCE: 192.168.1.10
DESTINATION: 192.168.1.20
PORT: 22
PROTOCOL: TCP
SIGNATURE: Repeated SSH Login Attempts
SEVERITY: LOW
```

Save the file.

### Step 3: Read the Sample Alerts

```bash
cat sample-ids-alerts.txt
```

Identify:

- Number of alerts.
- Source addresses.
- Destination address.
- Targeted ports.
- Detection signatures.
- Severity levels.

### Step 4: Search for High-Severity Alerts

```bash
grep -B 7 -A 1 "SEVERITY: HIGH" sample-ids-alerts.txt
```

This displays context around matching high-severity entries.

### Step 5: Search for SSH-Related Alerts

```bash
grep -i -B 7 -A 1 "SSH" sample-ids-alerts.txt
```

### Step 6: Identify Potential Threats

Study the four alerts.

Answer:

1. Which alert indicates possible brute-force activity?
2. Which alert indicates possible port scanning?
3. Which alert involves HTTPS?
4. Which alert has the lowest severity?
5. Why should a security analyst investigate an alert before declaring an incident?
6. Which alert might be a false positive?

### Step 7: Create an IDS Investigation Report

```bash
nano ids-analysis-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 09
INTRUSION DETECTION SYSTEMS

1. Definition of IDS:

2. Types of IDS:

3. Detection Methods:

4. Number of Sample Alerts:

5. High-Severity Alerts:

6. SSH-Related Alerts:

7. Possible Port Scanning:

8. Potential False Positives:

9. Firewall vs IDS:

10. Security Recommendations:

11. Conclusion:
```

Save your report.

---

## 14. Knowledge Check

Answer these questions:

1. What is an Intrusion Detection System?
2. Why is IDS important in cybersecurity?
3. Explain how an IDS works.
4. What is a Network-Based IDS?
5. What is a Host-Based IDS?
6. Differentiate between NIDS and HIDS.
7. What is signature-based detection?
8. What is anomaly-based detection?
9. Explain stateful protocol analysis.
10. What is an IDS alert?
11. Differentiate between false positives and false negatives.
12. What is alert fatigue?
13. Where can a NIDS sensor be deployed?
14. Explain the difference between a firewall and IDS.
15. Name four popular IDS technologies.

---

## 15. Lesson Summary

In this lesson, you learned:

- Definition of IDS.
- Importance of intrusion detection.
- How IDS works.
- Network-based IDS.
- Host-based IDS.
- Wireless and application-based IDS.
- Signature-based detection.
- Anomaly-based detection.
- Stateful protocol analysis.
- IDS alerts and severity.
- False positives and false negatives.
- IDS deployment.
- Firewall vs IDS.
- Popular IDS technologies.
- Cybersecurity careers involving IDS.

**Next Lesson: Module 14 – Lesson 10: Snort Intrusion Detection.**