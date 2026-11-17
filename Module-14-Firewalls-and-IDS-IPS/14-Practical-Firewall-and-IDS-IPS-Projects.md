---
---

# Module 14 – Lesson 14: Practical Firewall and IDS/IPS Projects

## 1. Introduction

In previous lessons, we studied:

- Firewall fundamentals.
- Linux firewall architecture.
- UFW.
- iptables.
- nftables.
- Firewall policies.
- Traffic control.
- Firewall logging.
- IDS.
- Snort.
- Suricata.
- IPS.
- SIEM integration.

Now we will combine these concepts into practical cybersecurity projects.

The objective is to understand how security technologies work together in real environments.

### Learning Objectives

By the end of this lesson, you should be able to:

1. Design a basic Linux firewall policy.
2. Inspect firewall configurations.
3. Analyse firewall logs.
4. Understand IDS alert investigation.
5. Interpret Suricata events.
6. Design an IPS prevention policy.
7. Analyse security events using Python.
8. Produce a professional security investigation report.

---

# PROJECT 1: Linux Firewall Security Audit

## 2. Project Overview

A firewall security audit examines firewall configuration and identifies potential weaknesses.

You will inspect your Kali Linux firewall environment without changing its active configuration.

### Objectives

- Identify available firewall technologies.
- Inspect firewall status.
- Examine existing rules.
- Identify potentially exposed services.
- Document findings.

### Step 1: Create the Project Directory

```bash
mkdir -p ~/module14-lab/project01-firewall-audit
cd ~/module14-lab/project01-firewall-audit
```

### Step 2: Identify Firewall Tools

```bash
command -v ufw
command -v iptables
command -v nft
```

This identifies which firewall tools are installed.

### Step 3: Inspect UFW

```bash
sudo ufw status verbose
```

Record:

- Whether UFW is installed.
- Whether it is active.
- Default incoming policy.
- Default outgoing policy.
- Existing rules.

If UFW is unavailable, record that fact.

### Step 4: Inspect iptables

```bash
sudo iptables -L -n -v
```

Inspect the NAT table:

```bash
sudo iptables -t nat -L -n -v
```

These commands display existing rules and counters.

### Step 5: Inspect nftables

```bash
sudo nft list ruleset
```

If no rules are displayed, record that observation rather than assuming the system is unprotected.

### Step 6: Inspect Listening Services

```bash
sudo ss -tulpen
```

Identify:

- Listening ports.
- Protocols.
- Local addresses.
- Associated processes, where visible.

A listening service is not automatically a vulnerability. Determine whether it is expected and necessary.

### Step 7: Create an Audit Report

```bash
nano firewall-audit-report.txt
```

Use this structure:

```text
FIREWALL SECURITY AUDIT

1. Operating System:
2. Available Firewall Tools:
3. UFW Status:
4. iptables Findings:
5. nftables Findings:
6. Listening Services:
7. Potential Security Concerns:
8. Recommended Improvements:
9. Conclusion:
```

Save the report.

---

# PROJECT 2: Firewall Policy Design

## 3. Project Overview

Design a firewall policy for a fictional small organisation.

### Scenario

A company has:

- One web server.
- One administrative workstation.
- One internal database server.
- Several employee computers.

The organisation wants to control network access.

### Security Requirements

1. Permit legitimate web traffic.
2. Restrict administrative access.
3. Prevent unnecessary external database access.
4. Permit established communication.
5. Deny unnecessary inbound connections.
6. Record important security events.

### Step 1: Create the Project Directory

```bash
mkdir -p ~/module14-lab/project02-firewall-policy
cd ~/module14-lab/project02-firewall-policy
```

### Step 2: Create the Policy File

```bash
nano firewall-policy.txt
```

Enter:

```text
ORGANISATION FIREWALL POLICY

INBOUND POLICY:
- Default: Deny unnecessary inbound traffic.
- HTTP: Permit where required.
- HTTPS: Permit where required.
- SSH: Restrict to authorised administrative systems.
- Database: Do not expose directly to the internet.

OUTBOUND POLICY:
- Permit approved business communication.
- Restrict unnecessary outbound services.
- Record important security events.

NETWORK SECURITY:
- Separate public-facing servers from internal systems.
- Permit only required communication between network segments.
- Review firewall rules regularly.

MONITORING:
- Record important firewall events.
- Investigate suspicious activity.
- Review policy violations.
```

Save the file.

### Step 3: Explain Your Decisions

Write answers to these questions:

1. Why should unnecessary inbound traffic be denied?
2. Why should SSH access be restricted?
3. Why should databases not be exposed directly to the internet?
4. Why is network segmentation important?
5. Why should firewall events be recorded?

### Step 4: Create a Network Policy Diagram

```text
              INTERNET
                  |
                  v
             FIREWALL
                  |
          +-------+-------+
          |               |
          v               v
      WEB SERVER      INTERNAL LAN
                          |
                   +------+------+
                   |             |
                   v             v
             ADMIN PC      EMPLOYEE PCs
                   |
                   v
             DATABASE SERVER
```

This is a conceptual diagram. A production design would normally place the database in a protected server segment with carefully controlled access.

---

# PROJECT 3: IDS Alert Investigation

## 4. Project Overview

Security analysts investigate alerts generated by intrusion detection systems.

In this project, you will examine fictional IDS alerts.

### Step 1: Create the Directory

```bash
mkdir -p ~/module14-lab/project03-ids-investigation
cd ~/module14-lab/project03-ids-investigation
```

### Step 2: Create an Alert File

```bash
nano ids-alerts.txt
```

Enter:

```text
ALERT 1
Time: 10:00
Source: 192.168.1.50
Destination: 192.168.1.20
Protocol: TCP
Port: 22
Signature: Suspicious SSH Activity
Severity: High

ALERT 2
Time: 10:05
Source: 192.168.1.60
Destination: 192.168.1.30
Protocol: TCP
Port: 80
Signature: Unusual HTTP Request
Severity: Medium

ALERT 3
Time: 10:10
Source: 192.168.1.70
Destination: 192.168.1.40
Protocol: UDP
Port: 53
Signature: Unusual DNS Activity
Severity: High

ALERT 4
Time: 10:15
Source: 192.168.1.50
Destination: 192.168.1.20
Protocol: TCP
Port: 22
Signature: Repeated SSH Activity
Severity: Critical
```

Save the file.

### Step 3: Display High-Severity Alerts

```bash
grep -E 'Severity: (High|Critical)' ids-alerts.txt
```

### Step 4: Find Repeated Source Addresses

```bash
grep 'Source:' ids-alerts.txt
```

Notice that `192.168.1.50` appears more than once.

### Step 5: Investigate the Repeated Activity

Answer:

1. Which source address generated repeated alerts?
2. Which destination was involved?
3. Which service was being monitored?
4. Did the alert severity increase?
5. What additional evidence would you collect before concluding that an attack occurred?

Remember: an IDS alert is an indication requiring investigation, not proof that an attack succeeded.

---

# PROJECT 4: Suricata Log Analysis

## 5. Project Overview

Suricata produces structured events that can be analysed by security analysts.

In this project, you will create fictional EVE JSON records and investigate them.

### Step 1: Create the Directory

```bash
mkdir -p ~/module14-lab/project04-suricata-analysis
cd ~/module14-lab/project04-suricata-analysis
```

### Step 2: Create a Sample EVE File

```bash
nano eve-sample.json
```

Enter:

```json
{"timestamp":"2026-11-15T10:00:00Z","event_type":"alert","src_ip":"192.168.1.50","dest_ip":"192.168.1.20","dest_port":22,"proto":"TCP","alert":{"signature":"SSH Activity","severity":2}}
{"timestamp":"2026-11-15T10:05:00Z","event_type":"alert","src_ip":"192.168.1.60","dest_ip":"192.168.1.30","dest_port":80,"proto":"TCP","alert":{"signature":"HTTP Activity","severity":3}}
{"timestamp":"2026-11-15T10:10:00Z","event_type":"alert","src_ip":"192.168.1.70","dest_ip":"192.168.1.40","dest_port":53,"proto":"UDP","alert":{"signature":"DNS Activity","severity":1}}
```

Save the file.

### Step 3: Validate the JSON

```bash
python3 -c 'import json; [json.loads(line) for line in open("eve-sample.json")]; print("JSON is valid")'
```

### Step 4: Analyse with Python

Create:

```bash
nano suricata_analyzer.py
```

Enter:

```python
import json
from collections import Counter

events = []

with open("eve-sample.json", "r") as file:
    for line in file:
        events.append(json.loads(line))

print("=== SURICATA LOG ANALYSER ===")

print("\nTotal events:", len(events))

print("\nEvents by source IP:")

sources = Counter(
    event["src_ip"] for event in events
)

for ip, count in sources.items():
    print(ip, count)

print("\nEvents by destination port:")

ports = Counter(
    event["dest_port"] for event in events
)

for port, count in ports.items():
    print(port, count)

print("\nAlert Details:")

for event in events:
    alert = event.get("alert", {})

    print(
        "Source:", event["src_ip"],
        "| Destination:", event["dest_ip"],
        "| Port:", event["dest_port"],
        "| Signature:", alert.get("signature"),
        "| Severity:", alert.get("severity")
    )
```

Save the file.

### Step 5: Execute the Analyzer

```bash
python3 suricata_analyzer.py
```

Study the output.

### Step 6: Write Your Findings

Record:

- Total events.
- Source addresses.
- Destination ports.
- Alert signatures.
- Severity values.
- Recommended investigation steps.

---

# PROJECT 5: Simulated IPS Prevention System

## 6. Project Overview

This project demonstrates how an IPS can make prevention decisions.

You will build a small Python simulation.

It will examine fictional events and decide whether to allow, alert, or block them based on a simple policy.

**Important:** This program does not inspect real network traffic or modify firewall rules. It is an educational simulation.

### Step 1: Create the Directory

```bash
mkdir -p ~/module14-lab/project05-ips-simulation
cd ~/module14-lab/project05-ips-simulation
```

### Step 2: Create the Python Program

```bash
nano ips_simulator.py
```

Enter:

```python
events = [
    {
        "source": "192.168.1.10",
        "service": "HTTPS",
        "risk": "normal"
    },
    {
        "source": "192.168.1.50",
        "service": "SSH",
        "risk": "suspicious"
    },
    {
        "source": "192.168.1.60",
        "service": "HTTP",
        "risk": "malicious"
    },
    {
        "source": "192.168.1.70",
        "service": "DNS",
        "risk": "normal"
    }
]

print("=== IPS SIMULATION ===")

for event in events:

    if event["risk"] == "normal":
        decision = "ALLOW"

    elif event["risk"] == "suspicious":
        decision = "ALERT"

    elif event["risk"] == "malicious":
        decision = "BLOCK"

    else:
        decision = "INVESTIGATE"

    print(
        "Source:", event["source"],
        "| Service:", event["service"],
        "| Risk:", event["risk"],
        "| Decision:", decision
    )
```

Save the file.

### Step 3: Run the Simulation

```bash
python3 ips_simulator.py
```

### Step 4: Understand the Decisions

| Risk | Decision |
|---|---|
| Normal | Allow |
| Suspicious | Alert |
| Malicious | Block |

This demonstrates a basic policy-driven decision process.

Real IPS platforms use much more sophisticated inspection, rule matching, state tracking, and policy enforcement.

---

# PROJECT 6: Mini Security Monitoring Dashboard

## 7. Project Overview

Build a simple command-line security monitoring dashboard using Python.

It will collect fictional security events, count event severity, and display a summary.

### Step 1: Create the Directory

```bash
mkdir -p ~/module14-lab/project06-security-dashboard
cd ~/module14-lab/project06-security-dashboard
```

### Step 2: Create the Event Database

```bash
nano events.json
```

Enter:

```json
{"source":"Firewall","event":"Connection allowed","severity":"low"}
{"source":"Suricata","event":"Suspicious SSH activity","severity":"high"}
{"source":"IPS","event":"Matching traffic blocked","severity":"critical"}
{"source":"Linux","event":"Failed login","severity":"medium"}
{"source":"Firewall","event":"Unusual outbound traffic","severity":"high"}
```

Save the file.

### Step 3: Create the Dashboard

```bash
nano security_dashboard.py
```

Enter:

```python
import json
from collections import Counter

events = []

with open("events.json", "r") as file:
    for line in file:
        events.append(json.loads(line))

severity_counts = Counter(
    event["severity"] for event in events
)

print("=" * 45)
print("       MINI SECURITY DASHBOARD")
print("=" * 45)

print("Total Events:", len(events))

print("\nSEVERITY SUMMARY")

for severity in [
    "critical",
    "high",
    "medium",
    "low"
]:
    print(
        severity.upper(),
        ":",
        severity_counts.get(severity, 0)
    )

print("\nRECENT SECURITY EVENTS")

for event in events:
    print(
        event["source"],
        "-",
        event["event"],
        "-",
        event["severity"].upper()
    )

print("\n" + "=" * 45)
```

Save the file.

### Step 4: Run the Dashboard

```bash
python3 security_dashboard.py
```

### Step 5: Interpret the Results

Answer:

1. How many events were recorded?
2. How many high-severity events occurred?
3. Which event was critical?
4. Which security tools generated events?
5. Why is centralised monitoring useful?

---

# 8. Final Project Documentation

Create a master report for all six projects.

```bash
cd ~/module14-lab
nano module14-project-summary.txt
```

Use this template:

```text
MODULE 14 – PRACTICAL PROJECT SUMMARY

PROJECT 1: LINUX FIREWALL SECURITY AUDIT
- Tools inspected:
- Firewall findings:
- Listening services:
- Recommendations:

PROJECT 2: FIREWALL POLICY DESIGN
- Security requirements:
- Inbound policy:
- Outbound policy:
- Network segmentation:

PROJECT 3: IDS ALERT INVESTIGATION
- Alerts analysed:
- Repeated source addresses:
- High-severity findings:
- Investigation recommendations:

PROJECT 4: SURICATA LOG ANALYSIS
- Events analysed:
- Source addresses:
- Destination ports:
- Findings:

PROJECT 5: IPS SIMULATION
- Events processed:
- Allowed events:
- Alerted events:
- Blocked events:

PROJECT 6: SECURITY MONITORING DASHBOARD
- Total events:
- Severity summary:
- Security sources:
- Findings:

OVERALL CONCLUSION:
```

Save the report.

---

## 9. Professional Security Reporting

A professional security report should include:

1. Title.
2. Executive summary.
3. Scope.
4. Methodology.
5. Tools used.
6. Findings.
7. Evidence.
8. Risk assessment.
9. Recommendations.
10. Conclusion.

### Example Finding

**Finding:** Unnecessary listening service.

**Observation:** A service is listening on a network interface.

**Risk:** If the service is not required or is insufficiently protected, it may increase the attack surface.

**Recommendation:** Verify the service's purpose, restrict unnecessary exposure, and ensure appropriate authentication and firewall controls.

Do not describe a service as vulnerable merely because it is listening.

---

## 10. Knowledge Check

1. What is a firewall security audit?
2. Why should unnecessary inbound traffic be restricted?
3. What is the purpose of network segmentation?
4. Why should IDS alerts be investigated?
5. What is Suricata EVE JSON?
6. What is the difference between an IDS alert and an IPS block?
7. What does a security monitoring dashboard display?
8. Why is Python useful for analysing security logs?
9. What is event correlation?
10. Why should security findings include evidence?
11. What should a security analyst do before recommending that a service be blocked?
12. Why should firewall and IPS policies be tested before production deployment?

---

## 11. Lesson Summary

You have completed six practical project exercises:

1. Linux Firewall Security Audit.
2. Firewall Policy Design.
3. IDS Alert Investigation.
4. Suricata Log Analysis.
5. Simulated IPS Prevention System.
6. Mini Security Monitoring Dashboard.

You have also practised:

- Linux security inspection.
- Firewall policy documentation.
- Log analysis.
- Python automation.
- Security event correlation.
- Threat investigation.
- Professional security reporting.

**Next Lesson: Module 14 – Lesson 15: Module Assessment.**

This will be the final lesson of Module 14.