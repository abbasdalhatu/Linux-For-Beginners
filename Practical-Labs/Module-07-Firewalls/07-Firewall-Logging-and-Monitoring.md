
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 07: FIREWALL LOGGING AND MONITORING

## 1. Introduction

A firewall does more than allow or block network traffic. It can also generate logs that help administrators understand what is happening on a system.

Firewall logging is the process of recording information about network traffic and firewall decisions.

These records help security analysts:

- Identify suspicious connection attempts.
- Investigate blocked traffic.
- Detect unusual network activity.
- Monitor exposed services.
- Troubleshoot connectivity problems.
- Support security incident investigations.

For example, if an unknown computer repeatedly attempts to connect to an SSH service, firewall logs may provide evidence of those connection attempts.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain firewall logging.
2. Understand the importance of monitoring.
3. Identify common firewall log information.
4. Inspect Linux system logs.
5. Understand UFW logging.
6. Inspect available firewall-related log records.
7. Identify suspicious traffic patterns.
8. Create a basic firewall monitoring report.

## 3. What Is Firewall Logging?

Firewall logging records information about traffic processed by firewall rules.

Depending on the configuration, a firewall may record:

- Accepted connections.
- Dropped packets.
- Rejected connections.
- Source IP addresses.
- Destination IP addresses.
- Source ports.
- Destination ports.
- Protocols.
- Timestamps.
- Firewall rule information.

Example of a simplified firewall log:

```text
Nov 15 10:30:15 kali kernel: [UFW BLOCK] IN=eth0 OUT= MAC=... SRC=192.168.56.10 DST=192.168.56.20 PROTO=TCP SPT=51520 DPT=22
```

This is an illustrative example, not an actual record from your machine.

Important fields:

| Field | Meaning |
|---|---|
| SRC | Source IP address |
| DST | Destination IP address |
| PROTO | Network protocol |
| SPT | Source port |
| DPT | Destination port |
| IN | Incoming interface |
| OUT | Outgoing interface |
| UFW BLOCK | Indicates a UFW-related blocked packet |

## 4. Why Firewall Monitoring Matters

### A. Detecting Suspicious Activity

Monitoring can help identify:

- Repeated connection attempts.
- Unexpected access to services.
- Connections from unfamiliar addresses.
- Unusual traffic patterns.

### B. Troubleshooting

A legitimate application may fail to connect because its traffic is blocked.

Firewall logs can help administrators identify the cause.

### C. Incident Investigation

Logs provide evidence that can help reconstruct network events.

### D. Compliance and Accountability

Organizations may retain firewall logs to support internal security policies and auditing requirements.

## 5. Understanding UFW Logging

UFW supports several logging levels.

| Level | Description |
|---|---|
| off | Logging disabled |
| low | Logs blocked packets that do not match the default policy |
| medium | Includes additional allowed and invalid traffic information |
| high | Includes more detailed logging |
| full | Provides very detailed logging |

Higher logging levels can generate large amounts of data.

This may increase storage requirements and make investigations harder if logs are not managed properly.

To inspect UFW logging configuration:

```bash
sudo ufw status verbose
```

A status output may include a line indicating the current logging level.

To configure logging, UFW provides:

```bash
sudo ufw logging LEVEL
```

This is a configuration command. Do not execute it in this lab.

## 6. Understanding Linux Log Locations

Linux stores system logs in several locations.

### A. Systemd Journal

Systems using systemd commonly collect logs through journald.

Inspect recent logs:

```bash
sudo journalctl -n 50
```

Follow live logs:

```bash
sudo journalctl -f
```

Exit live viewing with:

```text
CTRL + C
```

### B. Kernel Logs

Kernel messages can provide information about firewall and network events.

Inspect kernel messages:

```bash
sudo dmesg | tail -n 30
```

### C. Traditional System Logs

Some Linux installations maintain traditional log files under:

```text
/var/log/
```

Common files may include:

- `/var/log/syslog`
- `/var/log/messages`
- `/var/log/kern.log`
- `/var/log/auth.log`

Not every distribution uses all these files.

Kali may use journald as the main source of system log information.

## 7. Practical Exercise 1: Inspect System Logs

Open your Kali terminal.

Run:

```bash
sudo journalctl -n 30
```

Study the output.

Identify:

1. Timestamps.
2. Hostname.
3. Service or process name.
4. Log message.
5. Any network-related events.

Next, inspect kernel messages:

```bash
sudo dmesg | tail -n 30
```

Look for network-related messages.

Do not assume every warning is a security incident.

## 8. Practical Exercise 2: Search for Firewall-Related Events

Search the system journal for UFW messages:

```bash
sudo journalctl --no-pager | grep -i ufw
```

Search kernel messages:

```bash
sudo dmesg | grep -iE 'ufw|drop|reject|filter'
```

If no messages are found, record that no matching events were present in the available logs.

This is a valid result.

Now inspect the UFW status:

```bash
sudo ufw status verbose
```

Record:

- Whether UFW is active.
- Its logging level, if displayed.
- Any visible firewall rules.

Do not enable UFW logging or change its configuration.

## 9. Practical Exercise 3: Inspect Traditional Log Files

Check whether traditional system logs exist:

```bash
ls -lh /var/log/
```

Check for the presence of common files:

```bash
ls -l /var/log/syslog /var/log/kern.log /var/log/auth.log 2>/dev/null
```

If a file exists, inspect its last entries.

For example:

```bash
sudo tail -n 20 /var/log/kern.log
```

If the file does not exist, use journalctl instead.

Do not modify or delete any system log files.

## 10. Practical Exercise 4: Understand Suspicious Traffic Patterns

Study these hypothetical situations.

### Scenario A: Repeated SSH Attempts

A firewall log shows repeated blocked connections to TCP port 22 from one unfamiliar source address.

Possible interpretation:

- Someone or something is repeatedly attempting SSH access.
- The attempts may represent unauthorized access activity.
- Additional investigation is required before drawing conclusions.

### Scenario B: Unexpected Database Access

A server records incoming connection attempts to TCP port 3306 from an unknown address.

Possible interpretation:

- A database service may be exposed unnecessarily.
- An application may be misconfigured.
- Someone may be probing the service.

### Scenario C: Unusual Outbound Connections

A workstation repeatedly connects to an unfamiliar external address.

Possible interpretation:

- A legitimate application may be communicating with a new service.
- An application may be misconfigured.
- Malware or unauthorized software may be involved.

Do not classify traffic as malicious based on one log entry alone.

Correlate timestamps, process information, network context, and other evidence.

## 11. Practical Exercise 5: Analyze a Sample Firewall Log

Study this fictional record:

```text
Nov 15 10:30:15 kali kernel: [UFW BLOCK] IN=eth0 OUT= MAC=... SRC=192.168.56.10 DST=192.168.56.20 PROTO=TCP SPT=51520 DPT=22
```

Answer the following:

1. What is the timestamp?
2. Which firewall-related event is recorded?
3. What is the incoming interface?
4. What is the source IP?
5. What is the destination IP?
6. What protocol is used?
7. What is the source port?
8. What is the destination port?
9. Which service commonly uses destination port 22?
10. What additional evidence would you collect before deciding whether this event is malicious?

Save your answers in your notes.

## 12. Practical Exercise 6: Create a Firewall Monitoring Notes File

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module07/lab07
```

Enter it:

```bash
cd ~/practical-labs/module07/lab07
```

Create your notes file:

```bash
nano firewall-monitoring-report.txt
```

Use this template:

```text
MODULE 07 - LAB 07
FIREWALL LOGGING AND MONITORING

1. SYSTEM LOG INFORMATION

Journalctl Available:
Recent Log Events:
Kernel Log Observations:

2. FIREWALL INFORMATION

UFW Status:
UFW Logging Level:
Firewall-Related Events:

3. LOG FILE INFORMATION

/var/log/syslog:
/var/log/kern.log:
/var/log/auth.log:

4. SAMPLE LOG ANALYSIS

Timestamp:
Event:
Source IP:
Destination IP:
Protocol:
Source Port:
Destination Port:

5. SUSPICIOUS ACTIVITY SCENARIOS

Repeated SSH Attempts:
Unexpected Database Access:
Unusual Outbound Connections:

6. SECURITY OBSERVATIONS

7. RECOMMENDATIONS
```

Save the file:

- CTRL + O
- ENTER
- CTRL + X

## 13. Firewall Monitoring Best Practices

1. Monitor firewall events regularly.
2. Use accurate system time.
3. Protect log files from unauthorized modification.
4. Avoid logging excessive unnecessary traffic.
5. Investigate repeated connection attempts.
6. Correlate firewall logs with authentication logs.
7. Retain logs according to organizational policy.
8. Protect sensitive information in log records.
9. Record investigation findings.
10. Establish alerts for important security events.

## 14. Knowledge Check

1. What is firewall logging?
2. Why do security analysts monitor firewall logs?
3. What does SRC represent in a firewall log?
4. What does DPT represent?
5. What is the purpose of journalctl?
6. What is the purpose of dmesg?
7. What does UFW BLOCK indicate?
8. Why should logs be protected?
9. Why is one suspicious log entry insufficient to prove an attack?
10. What is log correlation?

## 15. Lab Completion Checklist

- [ ] Understood firewall logging.
- [ ] Studied UFW logging levels.
- [ ] Inspected system journal entries.
- [ ] Inspected kernel messages.
- [ ] Searched for firewall-related events.
- [ ] Inspected available log files.
- [ ] Analyzed a sample firewall log.
- [ ] Studied suspicious traffic scenarios.
- [ ] Created `firewall-monitoring-report.txt`.
- [ ] Completed the knowledge check.

## 16. Expected Outcome

You should now understand how firewall logs support security monitoring, troubleshooting, and incident investigations.

You should also be able to locate and inspect available Linux firewall-related logs.

**Safety reminder:** This lab is read-only. Do not enable logging, change firewall rules, delete logs, or generate suspicious traffic.
