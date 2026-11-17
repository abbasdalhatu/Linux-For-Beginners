---
---


# Module 13 – Lesson 12: Intrusion Detection and Monitoring

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define intrusion detection.
2. Explain the purpose of security monitoring.
3. Differentiate IDS and IPS.
4. Understand host-based and network-based intrusion detection.
5. Explain signature-based and anomaly-based detection.
6. Understand Security Information and Event Management (SIEM).
7. Identify suspicious Linux activities.
8. Monitor system logs and network connections.
9. Understand tools such as Snort, Suricata, and OSSEC.
10. Perform a basic Linux intrusion-detection investigation.

---

## 2. What Is Intrusion Detection?

Intrusion Detection is the process of monitoring computer systems or networks to identify suspicious activities, policy violations, and potential security attacks.

An intrusion detection system analyses activity and alerts administrators when it identifies behaviour that may indicate a security threat.

### Simple Analogy

Imagine a university security team monitoring CCTV cameras.

The security team watches for:

- Unauthorised entry.
- Suspicious movement.
- People accessing restricted rooms.
- Unusual activities.

An Intrusion Detection System (IDS) performs a similar function in computer systems.

Instead of monitoring people physically, it monitors digital activities.

### Examples of Activities an IDS May Detect

- Repeated failed login attempts.
- Port scanning.
- Suspicious network traffic.
- Unusual account activity.
- Unexpected file modifications.
- Attempts to exploit vulnerabilities.
- Suspicious connections to remote systems.

---

## 3. What Is Security Monitoring?

Security monitoring is the continuous observation and analysis of system activities to identify security problems.

It helps administrators understand what is happening inside their systems.

### Important Areas to Monitor

| Area | What to Monitor |
|---|---|
| User accounts | Login and account activity |
| Network | Connections and traffic |
| Processes | Running applications |
| Files | Changes to important files |
| Authentication | Successful and failed logins |
| Services | Unexpected service activity |
| System logs | Errors and suspicious events |
| Applications | Abnormal application behaviour |

### Why Monitoring Matters

Monitoring helps organisations:

1. Detect attacks early.
2. Investigate suspicious activity.
3. Identify compromised accounts.
4. Detect unexpected system changes.
5. Reduce incident response time.
6. Maintain security visibility.
7. Support forensic investigations.

---

## 4. Understanding IDS and IPS

Two important security technologies are:

- IDS – Intrusion Detection System.
- IPS – Intrusion Prevention System.

They are related, but they have different responsibilities.

### Intrusion Detection System (IDS)

An IDS monitors activities and generates alerts when suspicious behaviour is detected.

Its main purpose is detection and notification.

### Intrusion Prevention System (IPS)

An IPS monitors activities and can automatically take action to block or prevent detected threats.

Its main purpose is detection and prevention.

### Difference Between IDS and IPS

| Feature | IDS | IPS |
|---|---|---|
| Full name | Intrusion Detection System | Intrusion Prevention System |
| Main purpose | Detect suspicious activity | Detect and prevent threats |
| Monitoring | Yes | Yes |
| Generates alerts | Yes | Yes |
| Automatically blocks traffic | Not normally | Can |
| Typical deployment | Monitoring point | Inline or prevention-capable system |

### Simple Example

Imagine someone attempting to access a restricted university server.

An IDS may:

1. Detect suspicious login activity.
2. Generate an alert.
3. Notify the security administrator.

An IPS may:

1. Detect suspicious traffic.
2. Apply a configured prevention rule.
3. Block the connection.
4. Record the event.

An IPS must be configured carefully because incorrect prevention rules can block legitimate traffic.

---

## 5. Types of Intrusion Detection Systems

### 5.1 Host-Based Intrusion Detection System (HIDS)

A HIDS monitors activities on an individual computer or server.

It may examine:

- System logs.
- File integrity.
- User activity.
- Processes.
- Configuration changes.
- Security events.

Examples include:

- Wazuh agent.
- OSSEC.
- AIDE for file-integrity monitoring.

### 5.2 Network-Based Intrusion Detection System (NIDS)

A NIDS monitors network traffic to identify suspicious communications.

It may detect:

- Port scanning.
- Malicious traffic patterns.
- Suspicious protocols.
- Exploit attempts.
- Unusual network behaviour.

Examples include:

- Snort.
- Suricata.
- Zeek.

### Comparison

| HIDS | NIDS |
|---|---|
| Monitors an individual host | Monitors network traffic |
| Examines system events | Examines network communications |
| Can monitor file changes | Can detect network attack patterns |
| Useful for endpoint security | Useful for network visibility |

### Important Point

HIDS and NIDS are complementary. Organisations often use both for better visibility.

---

## 6. IDS Detection Methods

Intrusion detection systems use different methods to identify suspicious activities.

Two major methods are signature-based detection and anomaly-based detection.

### 6.1 Signature-Based Detection

Signature-based detection compares observed activity against known patterns of malicious behaviour.

These patterns are called signatures.

### Example

A security tool may have a rule designed to recognise a known attack pattern.

When matching activity appears, the tool generates an alert.

Advantages:

- Effective against known threats.
- Can produce precise alerts when signatures are well designed.
- Rules can be updated.

Limitations:

- May miss unknown attacks.
- Requires updated signatures.
- Modified attacks may evade known patterns.

### 6.2 Anomaly-Based Detection

Anomaly-based detection identifies behaviour that differs significantly from an established baseline.

### Example

A server normally receives a small number of login attempts.

Suddenly, hundreds of login attempts appear within a short period.

The system may classify this as unusual behaviour.

Advantages:

- Can identify previously unknown suspicious patterns.
- Helps detect unusual activity.
- Useful for behavioural monitoring.

Limitations:

- May generate false positives.
- Requires a reliable baseline.
- Normal changes in system activity may appear suspicious.

### Comparison

| Signature-Based | Anomaly-Based |
|---|---|
| Uses known attack patterns | Uses behavioural deviations |
| Good for known threats | Can identify unusual unknown behaviour |
| Requires signature updates | Requires baseline and tuning |
| May miss new attacks | May generate false positives |

Some modern security platforms combine both approaches.

---

## 7. Understanding Security Alerts

A security alert is a notification generated when a security tool identifies an event requiring attention.

Not every alert means an attack has occurred.

### Common Alert Categories

1. Critical.
2. High.
3. Medium.
4. Low.
5. Informational.

### Example Alerts

| Alert | Possible Meaning |
|---|---|
| Multiple failed logins | Possible brute-force attempt |
| Unexpected listening port | Possible unauthorised service |
| Important file changed | Possible configuration modification |
| Suspicious network traffic | Possible malicious communication |
| Unknown process | Requires investigation |

### False Positives

A false positive occurs when a legitimate activity is incorrectly identified as suspicious.

Example:

A legitimate administrator repeatedly attempts to log in after forgetting a password.

The IDS may interpret the activity as a possible brute-force attack.

### False Negatives

A false negative occurs when malicious activity happens but the security system fails to detect it.

Both problems affect security monitoring.

---

## 8. Understanding SIEM

SIEM stands for Security Information and Event Management.

A SIEM platform collects, centralises, correlates, and analyses security events from multiple systems.

### Information a SIEM Can Collect

- Linux system logs.
- Windows event logs.
- Firewall events.
- Network security alerts.
- Authentication events.
- Application logs.
- Endpoint security alerts.

### How SIEM Works

```text
Linux Server ----+
                 |
Windows PC ------+
                 |
Firewall --------+----> SIEM PLATFORM
                 |           |
Network IDS -----+           v
                 |      Event Analysis
Applications ----+           |
                             v
                       Security Alerts
                             |
                             v
                      Security Analyst
```

### SIEM Benefits

- Centralised log collection.
- Security event correlation.
- Threat detection.
- Incident investigation.
- Reporting.
- Security visibility.

### Examples of SIEM Platforms

- Splunk Enterprise Security.
- Microsoft Sentinel.
- IBM QRadar.
- Elastic Security.
- Wazuh.

---

## 9. Linux Security Monitoring Commands

Linux provides built-in commands that help administrators inspect system activity.

### 9.1 Monitor Running Processes

```bash
ps aux
```

Displays running processes.

### 9.2 Monitor Processes in Real Time

```bash
top
```

Provides live information about:

- CPU usage.
- Memory usage.
- Running processes.
- System load.

Press `q` to exit.

### 9.3 Monitor Network Connections

```bash
sudo ss -tunap
```

Displays TCP and UDP socket information.

### 9.4 Monitor Listening Ports

```bash
sudo ss -tulpen
```

Displays listening network sockets.

### 9.5 Monitor System Logs

```bash
sudo journalctl -f
```

Displays new system journal entries as they arrive.

Press `Ctrl + C` to stop.

### 9.6 Inspect Recent Authentication Events

```bash
sudo journalctl -n 50 --no-pager
```

On some Debian-based systems, authentication events may also be available in:

```bash
sudo tail -n 50 /var/log/auth.log
```

The authentication log file may not exist on every installation.

---

## 10. Introduction to Snort

Snort is an open-source network intrusion detection and prevention system.

It analyses network packets and uses rules to identify suspicious traffic.

### Important Snort Capabilities

- Packet inspection.
- Network intrusion detection.
- Signature-based detection.
- Alert generation.
- Rule-based traffic analysis.
- Packet logging.

### How Snort Works

```text
NETWORK TRAFFIC
       |
       v
PACKET CAPTURE
       |
       v
PACKET ANALYSIS
       |
       v
RULE EVALUATION
       |
       v
SUSPICIOUS MATCH?
       |
   +---+---+
   |       |
  YES      NO
   |       |
   v       v
 ALERT   CONTINUE
```

Snort can operate in different modes depending on its configuration and deployment.

### Common Snort Uses

- Monitoring network traffic.
- Detecting suspicious connections.
- Identifying known attack signatures.
- Supporting incident investigations.

---

## 11. Introduction to Suricata

Suricata is an open-source network threat detection engine.

It supports:

- Intrusion detection.
- Intrusion prevention.
- Network security monitoring.
- Protocol identification.
- Rule-based detection.
- Network traffic analysis.

### Snort vs Suricata

| Feature | Snort | Suricata |
|---|---|---|
| Network IDS | Yes | Yes |
| IPS capability | Yes | Yes |
| Rule-based detection | Yes | Yes |
| Protocol analysis | Yes | Yes |
| Multithreading | Depends on version and configuration | Supports multithreading |
| Network monitoring | Yes | Yes |

Both tools are widely used in cybersecurity.

---

## 12. Introduction to OSSEC and Wazuh

OSSEC is an open-source host-based intrusion detection system.

It monitors systems for suspicious activities.

Wazuh is an open-source security platform that evolved from the OSSEC project and provides additional capabilities.

### Wazuh Capabilities

- Security monitoring.
- File integrity monitoring.
- Log analysis.
- Vulnerability detection.
- Configuration assessment.
- Security event analysis.

### Example

Imagine a Linux server containing important configuration files.

Wazuh can monitor selected files and generate alerts when unexpected changes occur.

This helps administrators investigate potential unauthorised modifications.

---

## 13. Understanding File Integrity Monitoring

File Integrity Monitoring (FIM) checks whether important files have changed.

It is useful because attackers may attempt to modify:

- System configuration files.
- Authentication settings.
- Application files.
- Security scripts.
- Important binaries.

### How File Integrity Monitoring Works

1. A baseline is created.
2. The system records file properties or cryptographic hashes.
3. Files are checked again later.
4. Changes are compared against the baseline.
5. Unexpected changes generate alerts.

### Example Using SHA-256

Create a test file in your lab:

```bash
mkdir -p ~/module13-lab/lesson12
cd ~/module13-lab/lesson12
echo "Original security configuration" > test-config.txt
```

Generate its hash:

```bash
sha256sum test-config.txt
```

Save the original hash:

```bash
sha256sum test-config.txt > original-hash.txt
```

Now make a controlled modification:

```bash
echo "A test modification" >> test-config.txt
```

Generate a new hash:

```bash
sha256sum test-config.txt
```

Compare the hashes:

```bash
diff original-hash.txt <(sha256sum test-config.txt)
```

If the file changes, its SHA-256 hash will normally change.

This demonstrates the basic principle behind file integrity monitoring.

---

## 14. Security Monitoring Best Practices

Effective monitoring requires more than installing security software.

Recommended practices include:

1. Monitor important system logs.
2. Review failed authentication attempts.
3. Monitor unusual network connections.
4. Monitor important file changes.
5. Keep detection rules updated.
6. Investigate alerts promptly.
7. Protect collected logs.
8. Synchronise system time.
9. Reduce unnecessary false positives.
10. Document security incidents.

### Why Time Synchronisation Matters

Accurate timestamps help investigators reconstruct the sequence of events during an incident.

If different systems have incorrect clocks, correlating their logs becomes difficult.

---

# 15. Practical Lab: Linux Intrusion Detection and Monitoring

## Lab Objective

Perform basic monitoring activities on your Kali Linux VM.

You will:

- Inspect processes.
- Inspect network connections.
- Monitor system logs.
- Examine authentication events.
- Demonstrate file integrity monitoring.
- Generate a monitoring report.

This lab is inspection-only apart from modifying a test file inside your own lab directory.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson12
cd ~/module13-lab/lesson12
```

### Step 2: Inspect Running Processes

```bash
ps aux
```

Record a few processes you recognise.

### Step 3: Inspect System Resources

```bash
top
```

Press `q` to exit.

### Step 4: Inspect Network Connections

```bash
sudo ss -tunap
```

### Step 5: Inspect Listening Ports

```bash
sudo ss -tulpen
```

### Step 6: Inspect Recent System Logs

```bash
sudo journalctl -n 50 --no-pager
```

### Step 7: Monitor Live Logs

```bash
sudo journalctl -f
```

Allow it to run briefly.

Press `Ctrl + C` to stop.

### Step 8: Inspect Authentication Logs

```bash
sudo journalctl -n 50 --no-pager
```

If available:

```bash
sudo tail -n 50 /var/log/auth.log
```

### Step 9: Perform a File Integrity Test

Create a test file:

```bash
echo "Original security configuration" > test-config.txt
```

Generate the baseline:

```bash
sha256sum test-config.txt > original-hash.txt
```

Modify the test file:

```bash
echo "Controlled modification" >> test-config.txt
```

Compare the hashes:

```bash
sha256sum test-config.txt
```

```bash
cat original-hash.txt
```

Observe the difference.

### Step 10: Generate Your Monitoring Report

Run:

```bash
{
    echo "LINUX INTRUSION DETECTION AND MONITORING REPORT"
    echo "==============================================="
    echo
    echo "DATE"
    date
    echo
    echo "HOSTNAME"
    hostname
    echo
    echo "RUNNING PROCESSES"
    ps aux
    echo
    echo "NETWORK CONNECTIONS"
    sudo ss -tunap
    echo
    echo "LISTENING PORTS"
    sudo ss -tulpen
    echo
    echo "RECENT SYSTEM LOGS"
    sudo journalctl -n 30 --no-pager
    echo
    echo "AUTHENTICATION LOG FILE"
    if [ -f /var/log/auth.log ]; then
        sudo tail -n 30 /var/log/auth.log
    else
        echo "Authentication log file unavailable."
    fi
    echo
    echo "FILE INTEGRITY BASELINE"
    cat original-hash.txt
    echo
    echo "CURRENT FILE HASH"
    sha256sum test-config.txt
} > intrusion-monitoring-report.txt 2>&1
```

### Step 11: Inspect the Report

```bash
less intrusion-monitoring-report.txt
```

Press `q` to exit.

### Expected Files

```text
lesson12/
├── test-config.txt
├── original-hash.txt
└── intrusion-monitoring-report.txt
```

---

## 16. Real-World Scenario

Imagine you are monitoring a university Linux server.

Your monitoring system generates these events:

1. Several failed SSH login attempts.
2. An unexpected listening port appears.
3. A sensitive configuration file changes.
4. An unfamiliar process starts running.

As a security analyst, you should:

- Investigate the source of the login attempts.
- Identify the application listening on the unexpected port.
- Compare the changed file with its approved baseline.
- Investigate the unfamiliar process.
- Preserve relevant logs and evidence.
- Assess the incident's severity.
- Recommend appropriate containment and remediation.

Do not automatically assume every unusual event is malicious.

---

## 17. Knowledge Check

Answer these questions in your notebook.

1. What is intrusion detection?
2. What is security monitoring?
3. Differentiate between IDS and IPS.
4. Explain HIDS and NIDS.
5. What is signature-based detection?
6. What is anomaly-based detection?
7. What is a false positive?
8. What is a false negative?
9. What does SIEM stand for?
10. Explain how a SIEM platform works.
11. What is Snort?
12. What is Suricata?
13. What is Wazuh?
14. What is file integrity monitoring?
15. Why are SHA-256 hashes useful for integrity checking?
16. Why is time synchronisation important?
17. Identify five activities that Linux administrators should monitor.
18. What steps should a security analyst take after detecting suspicious activity?

---

## 18. Practical Completion Checklist

- [ ] I understand intrusion detection.
- [ ] I understand security monitoring.
- [ ] I can differentiate IDS and IPS.
- [ ] I understand HIDS and NIDS.
- [ ] I understand signature-based detection.
- [ ] I understand anomaly-based detection.
- [ ] I understand false positives and false negatives.
- [ ] I understand SIEM.
- [ ] I understand Snort and Suricata.
- [ ] I understand OSSEC and Wazuh.
- [ ] I understand file integrity monitoring.
- [ ] I performed a SHA-256 integrity test.
- [ ] I inspected Linux processes and network connections.
- [ ] I monitored system logs.
- [ ] I generated my intrusion-monitoring report.

**Lesson 12 Complete: Intrusion Detection and Monitoring.**
