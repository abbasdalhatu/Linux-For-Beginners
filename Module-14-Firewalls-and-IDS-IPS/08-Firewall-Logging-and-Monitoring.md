---
---

# Module 14 – Lesson 08: Firewall Logging and Monitoring

## 1. Introduction to Firewall Logging

Firewall logging is the process of recording network traffic events and firewall decisions.

A firewall log can record information such as:

- Source IP address.
- Destination IP address.
- Source and destination ports.
- Network protocol.
- Timestamp.
- Firewall action.
- Network interface.
- Connection information.

Firewall logs help security administrators understand what is happening on a network.

### Real-World Example

Imagine a university server receives hundreds of connection attempts from an unknown IP address.

The firewall blocks many of these attempts.

Without logging, administrators may know that traffic was blocked but have limited information about the activity.

With logging enabled, they can investigate:

- Where the traffic originated.
- Which ports were targeted.
- When the attempts occurred.
- How frequently the attempts happened.
- Whether the activity appears suspicious.

---

## 2. Why Firewall Logging Is Important

Firewall logging supports several security activities.

### A. Threat Detection

Logs can reveal unusual network behaviour.

Example:

Repeated connection attempts against SSH may indicate an attempted brute-force attack.

### B. Incident Investigation

When a security incident occurs, logs help reconstruct events.

### C. Network Troubleshooting

Firewall logs can explain why legitimate connections are being blocked.

### D. Compliance and Auditing

Organisations may need records showing how network access is controlled.

### E. Security Monitoring

Administrators can continuously observe firewall activity and identify unusual patterns.

---

## 3. Understanding Firewall Log Entries

A firewall log entry may contain several fields.

Example:

```text
Nov 15 10:30:25 server kernel: UFW BLOCK IN=eth0 OUT= MAC=... SRC=192.168.1.50 DST=192.168.1.20 PROTO=TCP SPT=51234 DPT=22
```

This is an illustrative example.

Let's understand the fields.

| Field | Meaning |
|---|---|
| Nov 15 | Date |
| 10:30:25 | Time |
| server | Hostname |
| kernel | Logging source |
| UFW BLOCK | Firewall blocked traffic |
| IN=eth0 | Incoming interface |
| SRC | Source IP address |
| DST | Destination IP address |
| PROTO | Network protocol |
| SPT | Source port |
| DPT | Destination port |

### Important Observation

In this example:

- Source IP: `192.168.1.50`
- Destination IP: `192.168.1.20`
- Protocol: TCP
- Destination port: 22
- Action: BLOCK

The event represents blocked traffic targeting SSH.

A single blocked event does not automatically prove an attack. Context and repeated behaviour matter.

---

## 4. Understanding UFW Logging

UFW supports firewall logging.

### Check UFW Status

```bash
sudo ufw status verbose
```

### Inspect UFW Logging Configuration

```bash
sudo ufw status verbose
```

The output may show:

```text
Logging: on (low)
```

Possible logging levels include:

| Level | Meaning |
|---|---|
| off | Logging disabled |
| low | Basic logging |
| medium | More detailed logging |
| high | Detailed logging |
| full | Maximum logging detail |

Higher logging levels can generate large amounts of log data.

### Example Configuration Command

The following is illustrative and changes firewall logging configuration:

```bash
sudo ufw logging medium
```

Do not execute configuration-changing commands during this lesson.

---

## 5. Understanding iptables Logging

iptables supports logging through its LOG target.

Example:

```bash
sudo iptables -A INPUT -j LOG --log-prefix "FIREWALL EVENT: "
```

This is an illustrative rule, not a recommended production configuration.

It logs matching packets but does not itself block them.

An administrator may use logging rules alongside filtering rules.

### Important Security Consideration

Logging every packet can produce enormous amounts of data.

It may also:

- Increase disk usage.
- Make important events harder to identify.
- Affect system performance.
- Create opportunities for log flooding.

Use appropriate logging scope and rate limits.

---

## 6. Understanding nftables Logging

nftables supports logging through the `log` statement.

Illustrative example:

```bash
sudo nft add rule inet filter input log prefix "NFT EVENT: "
```

This example assumes an existing table and chain with those names.

The logging statement records matching packets.

A logging statement does not automatically accept or drop the traffic. The subsequent rules and chain policy determine how it is handled.

### Why Use nftables Logging?

It allows administrators to monitor matching traffic within nftables rules.

Logs can help identify:

- Blocked connections.
- Unusual source addresses.
- Unexpected protocols.
- Suspicious connection patterns.

---

## 7. Linux System Logs

Linux stores system events in log files and, on many distributions, the systemd journal.

Important locations include:

| Log | Purpose |
|---|---|
| `/var/log/syslog` | General system messages on systems using rsyslog |
| `/var/log/messages` | General system messages on some distributions |
| `/var/log/auth.log` | Authentication events on Debian-based systems |
| `/var/log/kern.log` | Kernel messages on systems using this log |
| `/var/log/ufw.log` | UFW events where configured |

Not every distribution uses every file.

Kali Linux is Debian-based, but its logging configuration may differ depending on installed services.

### List Available Log Files

```bash
ls -lah /var/log/
```

### Inspect Kernel Messages

```bash
sudo dmesg
```

### View System Journal

```bash
sudo journalctl
```

---

## 8. Using journalctl for Firewall Monitoring

`journalctl` allows administrators to inspect the systemd journal.

### View Recent Logs

```bash
sudo journalctl -n 50
```

Displays the latest 50 journal entries.

### View Kernel Logs

```bash
sudo journalctl -k
```

Displays kernel-related journal entries.

### View Logs from the Current Boot

```bash
sudo journalctl -b
```

### Follow Logs in Real Time

```bash
sudo journalctl -f
```

This continuously displays new journal entries.

Press `Ctrl + C` to stop following the journal.

### Search for UFW Events

```bash
sudo journalctl | grep -i ufw
```

This searches available journal output for UFW-related text.

---

## 9. Searching Firewall Logs

Linux administrators frequently use tools such as `grep`, `less`, and `tail`.

### Search for Blocked Traffic

```bash
sudo grep -i "BLOCK" /var/log/ufw.log
```

This works if the UFW log file exists and contains matching entries.

### Display the Last 20 Log Entries

```bash
sudo tail -n 20 /var/log/ufw.log
```

### Monitor a Log File Continuously

```bash
sudo tail -f /var/log/ufw.log
```

### Search for a Specific IP Address

```bash
sudo grep "192.168.1.50" /var/log/ufw.log
```

Replace the example address with an address relevant to your authorised investigation.

---

## 10. Understanding Suspicious Firewall Activity

Security administrators look for unusual patterns.

### A. Repeated SSH Attempts

Example:

```text
SRC=192.168.1.50 DPT=22
SRC=192.168.1.50 DPT=22
SRC=192.168.1.50 DPT=22
SRC=192.168.1.50 DPT=22
```

Possible interpretation:

The same source address repeatedly attempts to reach SSH.

This may indicate:

- Automated scanning.
- Misconfigured software.
- Repeated legitimate login attempts.
- A possible brute-force attempt.

More investigation is required before reaching a conclusion.

### B. Port Scanning Activity

An address attempts connections to multiple destination ports.

Example:

```text
SRC=192.168.1.50 DPT=21
SRC=192.168.1.50 DPT=22
SRC=192.168.1.50 DPT=23
SRC=192.168.1.50 DPT=80
SRC=192.168.1.50 DPT=443
```

Possible interpretation:

The source may be discovering available services.

### C. Unusual Outbound Connections

A server unexpectedly communicates with unfamiliar external addresses.

Possible explanations include:

- Legitimate application activity.
- Software updates.
- Misconfiguration.
- Malware communication.
- Unauthorised data transfer.

Investigate the destination, process, timing, and network context.

---

## 11. Understanding Log Rotation

Log files can grow continuously.

Log rotation manages old logs so that storage is not exhausted.

Linux commonly uses `logrotate`.

### Check Logrotate Version

```bash
logrotate --version
```

### Inspect Logrotate Configuration

```bash
cat /etc/logrotate.conf
```

### View Available Rotation Rules

```bash
ls -lah /etc/logrotate.d/
```

Log rotation may:

- Rename old log files.
- Compress archived logs.
- Create fresh log files.
- Remove logs after a retention period.

### Why Is Log Rotation Important?

Without appropriate log management, large log files can consume disk space and make monitoring difficult.

---

## 12. Understanding Centralised Logging

Large organisations often collect logs from many devices in one central location.

Examples include:

- Firewalls.
- Linux servers.
- Windows computers.
- Routers.
- IDS sensors.
- Authentication systems.

Centralised logging helps security teams investigate events across multiple systems.

### Example Architecture

```text
Linux Server --------\
Windows Computer -----\
Firewall --------------> Central Log Server
IDS Sensor -----------/         |
Router --------------/          |
                                |
                         Security Analyst
```

### Benefits

- Centralised visibility.
- Easier investigation.
- Better log correlation.
- Reduced dependence on individual devices.
- Support for security monitoring teams.

---

## 13. Understanding SIEM

SIEM means Security Information and Event Management.

A SIEM platform collects and analyses security information from different systems.

Examples include:

- Wazuh.
- Splunk Enterprise Security.
- IBM QRadar.
- Microsoft Sentinel.

### What Does SIEM Do?

1. Collects security events.
2. Normalises information.
3. Correlates related events.
4. Detects suspicious patterns.
5. Generates alerts.
6. Supports incident investigation.
7. Produces reports.

### Example

Imagine:

- A firewall records repeated blocked SSH attempts.
- A Linux server records failed authentication attempts.
- An IDS detects suspicious scanning activity.

A SIEM may correlate these events to help an analyst identify a possible attack.

---

## 14. Firewall Monitoring Best Practices

1. Enable appropriate logging.
2. Avoid unnecessary logging of every packet.
3. Monitor blocked traffic.
4. Monitor unexpected outbound connections.
5. Protect log files from unauthorised modification.
6. Use log rotation.
7. Synchronise system clocks.
8. Centralise important security logs.
9. Establish suitable retention periods.
10. Investigate unusual activity.
11. Avoid treating every blocked connection as an attack.
12. Document significant security events.

---

# 15. Practical Lab: Firewall Log Investigation

## Objective

Practise inspecting Linux firewall logs and identifying potentially suspicious network activity.

This lesson is inspection-only. Do not change firewall policies or generate attacks against other systems.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson08
cd ~/module14-lab/lesson08
```

### Step 2: Inspect Available Log Files

```bash
ls -lah /var/log/
```

Identify whether the following files exist:

- `ufw.log`
- `syslog`
- `kern.log`
- `auth.log`

### Step 3: Inspect the System Journal

```bash
sudo journalctl -n 50
```

### Step 4: Inspect Kernel Messages

```bash
sudo journalctl -k -n 50
```

### Step 5: Search for Firewall Events

```bash
sudo journalctl | grep -Ei 'ufw|iptables|nftables'
```

If no results appear, that does not necessarily mean the firewall is inactive. Events may be stored elsewhere or may not have been generated.

### Step 6: Inspect UFW Logs If Available

```bash
sudo tail -n 20 /var/log/ufw.log
```

If the file does not exist, record that observation instead of creating or changing firewall logging configuration.

### Step 7: Study a Sample Firewall Log

Create a sample log file:

```bash
nano sample-firewall.log
```

Enter:

```text
Nov 15 10:30:25 server kernel: UFW BLOCK IN=eth0 SRC=192.168.1.50 DST=192.168.1.20 PROTO=TCP SPT=51234 DPT=22
Nov 15 10:31:10 server kernel: UFW BLOCK IN=eth0 SRC=192.168.1.50 DST=192.168.1.20 PROTO=TCP SPT=51235 DPT=22
Nov 15 10:32:15 server kernel: UFW BLOCK IN=eth0 SRC=192.168.1.60 DST=192.168.1.20 PROTO=TCP SPT=51000 DPT=443
Nov 15 10:33:20 server kernel: UFW BLOCK IN=eth0 SRC=192.168.1.70 DST=192.168.1.20 PROTO=TCP SPT=52000 DPT=23
Nov 15 10:34:30 server kernel: UFW BLOCK IN=eth0 SRC=192.168.1.50 DST=192.168.1.20 PROTO=TCP SPT=51236 DPT=22
```

Save and exit.

These are fictional events for analysis.

### Step 8: Search the Sample Logs

Find all blocked events:

```bash
grep -i "BLOCK" sample-firewall.log
```

Find events targeting SSH:

```bash
grep "DPT=22" sample-firewall.log
```

Find events originating from one address:

```bash
grep "SRC=192.168.1.50" sample-firewall.log
```

Count the SSH events:

```bash
grep -c "DPT=22" sample-firewall.log
```

### Step 9: Investigate the Sample

Answer:

1. Which source IP appears most frequently?
2. Which destination port is targeted repeatedly?
3. How many events target port 22?
4. Which event targets port 443?
5. Which event targets port 23?
6. What suspicious behaviour might the repeated SSH attempts indicate?
7. Why should the analyst avoid immediately concluding that an attack occurred?

### Step 10: Create Your Investigation Report

```bash
nano firewall-log-investigation.txt
```

Use this template:

```text
MODULE 14 – LESSON 08
FIREWALL LOGGING AND MONITORING

1. Available System Log Files:

2. Journalctl Observations:

3. Kernel Log Observations:

4. UFW Log Availability:

5. Firewall Log Fields:

6. Repeated Source Addresses:

7. Frequently Targeted Ports:

8. Possible Suspicious Activity:

9. Log Rotation Observations:

10. SIEM Explanation:

11. Security Recommendations:

12. Conclusion:
```

Save your report.

---

## 16. Knowledge Check

Answer these questions:

1. What is firewall logging?
2. Give five reasons firewall logging is important.
3. What information can a firewall log contain?
4. What does UFW BLOCK mean?
5. What is the purpose of the iptables LOG target?
6. What does the nftables `log` statement do?
7. What is the purpose of `journalctl`?
8. Differentiate between `journalctl -n 50` and `journalctl -f`.
9. What is the purpose of `grep` when investigating logs?
10. What might repeated SSH connection attempts indicate?
11. What is port scanning?
12. What is log rotation?
13. Why is centralised logging useful?
14. What does SIEM stand for?
15. How can SIEM help identify a coordinated security incident?

---

## 17. Lesson Summary

In this lesson, you learned:

- Firewall logging and monitoring.
- Importance of firewall logs.
- Understanding log entries.
- UFW logging.
- iptables logging.
- nftables logging.
- Linux system logs.
- Using journalctl.
- Searching logs with grep and tail.
- Identifying suspicious network patterns.
- Log rotation.
- Centralised logging.
- SIEM fundamentals.
- Firewall monitoring best practices.

**Next Lesson: Module 14 – Lesson 09: Intrusion Detection Systems (IDS).**