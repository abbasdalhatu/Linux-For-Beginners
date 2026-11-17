---
---

# Module 14 – Lesson 13: IDS/IPS Integration and SIEM

## 1. Introduction

Modern organisations rarely depend on one security tool to protect their entire network.

They combine different security technologies to monitor systems, detect threats, prevent attacks, and investigate incidents.

Three important technologies are:

1. Intrusion Detection Systems (IDS).
2. Intrusion Prevention Systems (IPS).
3. Security Information and Event Management (SIEM).

Together, these technologies provide stronger security monitoring and incident response.

### Simple Explanation

Imagine a university with several security departments:

- CCTV cameras observe activities.
- Security officers identify suspicious behaviour.
- Guards prevent unauthorised access.
- A central control room collects reports and coordinates responses.

In cybersecurity:

- IDS acts like the monitoring cameras.
- IPS acts like security guards who can stop suspicious activity.
- SIEM acts like the central control room.

---

## 2. What Is Security Information and Event Management?

SIEM stands for Security Information and Event Management.

A SIEM system collects, stores, analyses, and correlates security information from different systems.

It helps security teams identify suspicious activity and investigate security incidents.

### Main Functions of SIEM

- Collect security logs.
- Centralise event information.
- Analyse security events.
- Correlate related events.
- Generate security alerts.
- Support incident investigation.
- Provide dashboards and reports.
- Help with compliance and auditing.

### Examples of SIEM Platforms

| SIEM Platform | Description |
|---|---|
| Splunk Enterprise Security | Security analytics and monitoring |
| IBM QRadar | Enterprise security information and event management |
| Microsoft Sentinel | Cloud-based SIEM and security analytics |
| Elastic Security | Security monitoring using the Elastic ecosystem |
| Wazuh | Open-source security monitoring and analysis platform |

---

## 3. Understanding Security Logs

A security log is a record of an event that occurs within a computer system or network.

Examples include:

- Successful login.
- Failed login.
- Firewall rule match.
- Network connection.
- Malware detection.
- IDS alert.
- IPS prevention event.
- File modification.

### Example Security Log

```text
2026-11-15 10:30:25
Source IP: 192.168.1.50
Destination IP: 192.168.1.20
Protocol: TCP
Destination Port: 22
Event: Suspicious SSH Activity
Severity: Medium
```

This fictional record provides information that a security analyst can investigate.

### Important Log Fields

| Field | Meaning |
|---|---|
| Timestamp | When the event occurred |
| Source IP | Origin of the activity |
| Destination IP | Target of the activity |
| Protocol | Communication protocol |
| Port | Network service port |
| Event type | Type of security event |
| Severity | Importance of the event |
| Description | Explanation of the event |

---

## 4. Understanding Log Collection

A SIEM system receives information from different sources.

Common sources include:

1. Firewalls.
2. IDS platforms.
3. IPS platforms.
4. Linux servers.
5. Windows computers.
6. Endpoint Detection and Response (EDR) tools.
7. Authentication systems.
8. DNS servers.
9. Cloud services.
10. Applications.

### Log Collection Architecture

```text
Firewall Logs -----------+
                         |
IDS Alerts --------------+
                         |
IPS Events --------------+
                         |
Linux System Logs -------+----> SIEM
                         |
Windows Event Logs ------+
                         |
Application Logs --------+
                                  |
                                  v
                           Event Analysis
                                  |
                                  v
                         Correlation Rules
                                  |
                                  v
                            Security Alert
                                  |
                                  v
                          Security Analyst
```

The SIEM combines information from these different sources.

---

## 5. Understanding Event Correlation

Event correlation is the process of identifying relationships between multiple security events.

A single event may not indicate a serious threat.

However, several related events may reveal suspicious activity.

### Example

Imagine the following events:

| Time | Event |
|---|---|
| 10:00 | Multiple failed login attempts |
| 10:02 | Suspicious network connection |
| 10:03 | Firewall records unusual traffic |
| 10:04 | IDS generates an alert |

Individually, these events may require investigation.

When combined, they may indicate a coordinated attack attempt.

### Correlation Workflow

```text
Failed Login Attempts
         |
         v
Suspicious Connection
         |
         v
Firewall Event
         |
         v
IDS Alert
         |
         v
SIEM Correlation
         |
         v
Security Investigation
```

### Why Correlation Matters

- Identifies patterns.
- Reduces isolated-event analysis.
- Improves threat visibility.
- Helps detect coordinated attacks.
- Supports faster incident investigation.

---

## 6. Understanding Security Event Severity

Security teams classify events based on their potential impact.

A common classification is:

| Severity | Meaning |
|---|---|
| Informational | General system information |
| Low | Minor suspicious activity |
| Medium | Activity requiring investigation |
| High | Serious potential security threat |
| Critical | Severe security incident requiring urgent attention |

Severity levels differ between products.

An alert marked critical does not automatically prove that a successful attack occurred.

Analysts must examine the surrounding evidence.

---

## 7. Understanding IDS/IPS and SIEM Integration

IDS and IPS platforms can provide security events to a SIEM.

The SIEM collects those events and combines them with information from other systems.

### Example Architecture

```text
              Network Traffic
                     |
                     v
                Firewall
                     |
              +------+------+
              |             |
              v             v
             IDS            IPS
              |             |
              +------+------+
                     |
                     v
                Security Logs
                     |
                     v
                    SIEM
                     |
              +------+------+
              |             |
              v             v
          Correlation    Dashboard
              |
              v
       Security Investigation
```

### Integration Benefits

1. Centralised monitoring.
2. Better threat visibility.
3. Faster incident investigation.
4. Correlation of multiple alerts.
5. Improved security reporting.
6. Historical event analysis.

---

## 8. Suricata Integration with SIEM

Suricata can produce structured EVE JSON logs.

These logs contain information about network events.

A SIEM or log-management platform can ingest supported JSON records.

### Example Suricata Event

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

### Integration Process

1. Suricata observes network traffic.
2. Its detection engine identifies matching activity.
3. Suricata records an event.
4. A log collector reads the event.
5. The SIEM receives the event.
6. The SIEM analyses and correlates it.
7. The security team investigates the result.

### Common Integration Components

- Suricata.
- EVE JSON.
- Log collectors.
- Data pipelines.
- SIEM platforms.

The exact integration process depends on the chosen SIEM and its supported input methods.

---

## 9. Understanding Wazuh

Wazuh is an open-source security platform used for security monitoring and threat detection.

It provides capabilities including:

- Security event analysis.
- Log analysis.
- File integrity monitoring.
- Vulnerability detection.
- Security configuration assessment.
- Endpoint monitoring.
- Threat detection.

Wazuh can receive and analyse supported security events from other tools.

### Important Wazuh Components

**Wazuh Agent**

Installed on monitored endpoints to collect security information.

**Wazuh Server**

Receives and analyses information from agents and other supported sources.

**Wazuh Indexer**

Stores and indexes security information.

**Wazuh Dashboard**

Provides a visual interface for analysing security events.

### Basic Architecture

```text
Linux Endpoint ----+
                   |
Windows Endpoint --+----> Wazuh Server
                   |            |
Other Sources -----+            v
                              Indexer
                                 |
                                 v
                              Dashboard
                                 |
                                 v
                          Security Analyst
```

Wazuh is commonly described as an extended detection and response platform with SIEM capabilities.

---

## 10. Understanding Log Normalisation

Different security products may record similar information in different formats.

For example:

**Firewall log:**

```text
SRC=192.168.1.50 DST=192.168.1.20 DPT=22
```

**Suricata JSON:**

```json
{
  "src_ip": "192.168.1.50",
  "dest_ip": "192.168.1.20",
  "dest_port": 22
}
```

Both records describe a source address, destination address, and destination port.

Log normalisation converts different formats into a consistent structure.

### Benefits

- Easier event correlation.
- Consistent reporting.
- Improved searching.
- Better analysis across security tools.
- Simplified dashboard creation.

---

## 11. Understanding Security Dashboards

A SIEM dashboard presents security information visually.

Common dashboard elements include:

- Total security events.
- Critical alerts.
- Failed login attempts.
- Top source IP addresses.
- Network activity.
- IDS/IPS alerts.
- Alert severity distribution.
- Incident timelines.

### Example Dashboard Structure

```text
+--------------------------------------+
|       SECURITY MONITORING            |
+--------------------------------------+
| Total Events       |  1,250          |
| Critical Alerts    |  3              |
| High Alerts        |  12             |
| Medium Alerts      |  35             |
+--------------------------------------+
| Top Source IPs                      |
| 192.168.1.50                         |
| 192.168.1.60                         |
+--------------------------------------+
| Recent Security Events               |
| Failed login                         |
| Suspicious SSH activity              |
| Firewall policy match                |
+--------------------------------------+
```

The values above are fictional.

Dashboards help analysts understand security activity quickly.

---

## 12. Understanding Incident Response

A security incident is an event that threatens the confidentiality, integrity, or availability of information systems.

A typical incident response process includes six stages.

### Stage 1: Preparation

Prepare tools, policies, people, and procedures.

### Stage 2: Identification

Determine whether a security incident has occurred.

### Stage 3: Containment

Limit the impact of the incident.

### Stage 4: Eradication

Remove the cause of the incident.

### Stage 5: Recovery

Restore affected systems to normal operation.

### Stage 6: Lessons Learned

Document the incident and improve security controls.

### Incident Response Workflow

```text
Preparation
     |
     v
Identification
     |
     v
Containment
     |
     v
Eradication
     |
     v
Recovery
     |
     v
Lessons Learned
```

SIEM platforms help security teams identify and investigate potential incidents.

They do not replace the complete incident response process.

---

## 13. Common SIEM Challenges

SIEM systems are useful, but they also have limitations.

### A. Alert Fatigue

Security analysts may receive too many alerts.

### B. False Positives

Legitimate activity may be incorrectly flagged.

### C. Data Volume

Large organisations generate enormous amounts of logs.

### D. Poor Configuration

Incorrect rules can reduce detection quality.

### E. Incomplete Visibility

Missing log sources can create gaps in monitoring.

### F. Storage Costs

Retaining large quantities of logs can be expensive.

### Solutions

1. Tune detection rules.
2. Prioritise critical alerts.
3. Use appropriate log retention policies.
4. Monitor data collection.
5. Improve event correlation.
6. Regularly review configurations.

---

## 14. Security Monitoring Best Practices

1. Centralise important security logs.
2. Synchronise system clocks.
3. Protect log integrity.
4. Restrict access to monitoring systems.
5. Monitor log collection failures.
6. Review high-severity alerts.
7. Maintain appropriate retention periods.
8. Test detection rules.
9. Document security incidents.
10. Regularly review monitoring coverage.

---

# 15. Practical Lab: Simulated IDS/IPS and SIEM Integration

## Objective

Create a small simulated SIEM environment using fictional security events.

You will practise collecting, filtering, and correlating security logs.

This lab does not require installing a SIEM server or connecting to a live network.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson13
cd ~/module14-lab/lesson13
```

### Step 2: Create a Security Events File

```bash
nano security-events.json
```

Enter:

```json
{"timestamp":"2026-11-15T10:00:00Z","source":"Linux","event":"Failed login","src_ip":"192.168.1.50","severity":"medium"}
{"timestamp":"2026-11-15T10:01:00Z","source":"Firewall","event":"Suspicious connection","src_ip":"192.168.1.50","severity":"high"}
{"timestamp":"2026-11-15T10:02:00Z","source":"Suricata","event":"SSH traffic alert","src_ip":"192.168.1.50","severity":"high"}
{"timestamp":"2026-11-15T10:03:00Z","source":"IPS","event":"Matching traffic blocked","src_ip":"192.168.1.50","severity":"critical"}
{"timestamp":"2026-11-15T10:04:00Z","source":"Linux","event":"Successful login","src_ip":"192.168.1.10","severity":"low"}
```

Save the file.

### Step 3: Validate the JSON Records

```bash
python3 -c 'import json; [json.loads(line) for line in open("security-events.json")]; print("All records are valid JSON")'
```

### Step 4: Display All Events

```bash
cat security-events.json
```

### Step 5: Identify Critical Events

```bash
grep '"severity":"critical"' security-events.json
```

### Step 6: Identify Suricata Events

```bash
grep '"source":"Suricata"' security-events.json
```

### Step 7: Correlate Events by Source IP

Inspect all events involving the same fictional source address:

```bash
grep '192.168.1.50' security-events.json
```

Observe how the same address appears in several different security events.

### Step 8: Create a Simple Python SIEM Analyzer

Create a Python file:

```bash
nano siem_analyzer.py
```

Enter:

```python
import json
from collections import Counter

events = []

with open("security-events.json", "r") as file:
    for line in file:
        events.append(json.loads(line))

print("=== MINI SIEM ANALYZER ===")

print("\nTotal Events:", len(events))

print("\nEvents by Severity:")
severity_counts = Counter(
    event["severity"] for event in events
)

for severity, count in severity_counts.items():
    print(f"{severity}: {count}")

print("\nEvents by Source:")
source_counts = Counter(
    event["source"] for event in events
)

for source, count in source_counts.items():
    print(f"{source}: {count}")

print("\nCritical Events:")

for event in events:
    if event["severity"] == "critical":
        print(
            event["timestamp"],
            event["source"],
            event["event"],
            event["src_ip"]
        )

print("\nRepeated Source IPs:")

ip_counts = Counter(
    event["src_ip"] for event in events
)

for ip, count in ip_counts.items():
    if count > 1:
        print(ip, "appears", count, "times")
```

Save the file.

### Step 9: Run the Analyzer

```bash
python3 siem_analyzer.py
```

Observe:

- Total events.
- Events grouped by severity.
- Events grouped by source.
- Critical security events.
- Repeated source addresses.

This is a small educational log analyser, not a production SIEM.

### Step 10: Interpret the Results

Answer:

1. How many security events were collected?
2. Which source generated the Suricata event?
3. Which event was classified as critical?
4. Which IP address appeared repeatedly?
5. Why is event correlation useful?
6. Why should an analyst investigate related events together?

### Step 11: Create Your Investigation Report

```bash
nano siem-investigation-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 13
IDS/IPS INTEGRATION AND SIEM

1. SIEM Definition:

2. Main SIEM Functions:

3. Security Log Sources:

4. Event Correlation:

5. Security Event Severity:

6. IDS/IPS Integration:

7. Suricata EVE JSON:

8. Wazuh Architecture:

9. Log Normalisation:

10. Security Dashboards:

11. Incident Response Stages:

12. SIEM Challenges:

13. Python SIEM Analyzer Results:

14. Security Recommendations:

15. Conclusion:
```

Save your report.

---

## 16. Knowledge Check

Answer these questions:

1. What does SIEM stand for?
2. What is the purpose of a SIEM platform?
3. Name five sources of security logs.
4. What is event correlation?
5. Why is event correlation important?
6. What is log normalisation?
7. What is the purpose of a security dashboard?
8. How can Suricata integrate with a SIEM?
9. What is Wazuh?
10. Name four major Wazuh components.
11. What is alert fatigue?
12. Explain the six stages of incident response.
13. Why should system clocks be synchronised?
14. What happens when a SIEM receives incomplete security logs?
15. Explain how IDS, IPS, and SIEM work together.

---

## 17. Lesson Summary

In this lesson, you learned:

- Security Information and Event Management.
- Security log collection.
- Event correlation.
- Security event severity.
- IDS/IPS and SIEM integration.
- Suricata EVE JSON integration.
- Wazuh architecture.
- Log normalisation.
- Security dashboards.
- Incident response.
- SIEM challenges.
- Security monitoring best practices.
- Practical SIEM log analysis using Python.

**Next Lesson: Module 14 – Lesson 14: Practical Firewall and IDS/IPS Projects.**