---
---

# Module 14 – Lesson 12: Intrusion Prevention Systems (IPS)

## 1. Introduction to Intrusion Prevention Systems

An Intrusion Prevention System (IPS) is a security technology designed to monitor network or system activity, identify suspicious behaviour, and automatically prevent detected threats.

Unlike a traditional Intrusion Detection System (IDS), which primarily detects and reports suspicious activity, an IPS can take action to stop matching malicious traffic.

### Simple Explanation

Imagine a university security officer.

- An IDS is like an officer who observes suspicious activity and reports it.
- An IPS is like an officer who observes suspicious activity and intervenes to stop it.

Both help protect the university, but an IPS has an active prevention role.

### Main Functions of an IPS

1. Monitor network traffic.
2. Detect suspicious activity.
3. Compare traffic against security rules.
4. Identify known attack patterns.
5. Block or reject matching traffic when configured.
6. Record security events.
7. Generate alerts.
8. Help reduce security risks.

---

## 2. Difference Between IDS and IPS

An IDS and IPS are related but have different responsibilities.

| Feature | IDS | IPS |
|---|---|---|
| Full meaning | Intrusion Detection System | Intrusion Prevention System |
| Main purpose | Detect threats | Detect and prevent threats |
| Traffic monitoring | Yes | Yes |
| Generates alerts | Yes | Yes |
| Automatically blocks traffic | Not normally | Yes, when configured |
| Typical placement | Passive monitoring point | Inline network path |
| Risk of blocking legitimate traffic | Low | Higher |

### Example

Suppose a computer receives suspicious traffic.

**IDS response:**

```text
Suspicious Traffic Detected
        |
        v
Generate Alert
        |
        v
Security Analyst Investigates
```

**IPS response:**

```text
Suspicious Traffic Detected
        |
        v
Inspect Security Rules
        |
        v
Block Matching Traffic
        |
        v
Record Security Event
```

---

## 3. How an IPS Works

An IPS normally performs the following operations:

### Step 1: Traffic Inspection

The IPS receives network traffic and examines packets.

### Step 2: Protocol Analysis

It identifies protocols and examines communication patterns.

### Step 3: Threat Detection

It compares observed activity against detection rules and other security intelligence.

### Step 4: Decision Making

It determines whether the traffic should be allowed or prevented.

### Step 5: Prevention

When a matching prevention rule applies, it can block, drop, or reject traffic.

### Step 6: Logging

It records relevant security events for investigation.

### IPS Workflow

```text
Incoming Network Traffic
          |
          v
    Traffic Inspection
          |
          v
    Threat Detection
          |
          v
    Security Decision
          |
     +----+----+
     |         |
  Allowed   Suspicious
     |         |
     v         v
  Forward    Prevent
               |
               v
           Record Event
               |
               v
          Generate Alert
```

---

## 4. Types of Intrusion Prevention Systems

There are several categories of IPS technology.

### A. Network-Based IPS (NIPS)

A Network-Based IPS monitors network traffic to detect and prevent malicious activity.

It is commonly deployed at network boundaries or between network segments.

Examples of technologies include:

- Suricata in inline IPS mode.
- Snort in inline prevention mode.
- Commercial network security appliances.

### B. Host-Based IPS (HIPS)

A Host-Based IPS protects an individual computer or server.

It may monitor:

- Running processes.
- File modifications.
- System calls.
- Registry changes on Windows.
- Application behaviour.
- Suspicious system activity.

### C. Wireless IPS (WIPS)

A Wireless IPS monitors wireless networks to identify suspicious wireless activity.

It can help detect:

- Rogue access points.
- Suspicious wireless devices.
- Unauthorised access points.
- Wireless security violations.

### D. Network Behaviour Analysis

Network Behaviour Analysis systems identify suspicious activity by examining network behaviour.

They may detect unusual traffic volumes, communication patterns, or deviations from expected network activity.

---

## 5. IPS Detection Techniques

An IPS uses different methods to identify suspicious activity.

### A. Signature-Based Detection

Compares traffic against known attack signatures.

Example:

A rule may identify a known malicious payload pattern.

Advantages:
- Effective against recognised threats.
- Rules can be specific.
- Alerts can be easier to investigate.

Disadvantages:
- May miss new or unknown threats.
- Requires updated signatures.

### B. Anomaly-Based Detection

Identifies activity that differs from an established normal behaviour baseline.

Example:

A computer normally sends a small amount of network traffic but suddenly begins transmitting an unusually large volume.

Advantages:
- Can identify previously unknown suspicious behaviour.
- Useful for unusual activity.

Disadvantages:
- May generate false positives.
- Requires a reliable baseline.

### C. Stateful Protocol Analysis

Examines whether network communication follows expected protocol behaviour.

For example, an IPS can inspect whether a protocol exchange follows its expected sequence.

Advantages:
- Can identify protocol misuse.
- Provides deeper communication analysis.

Disadvantages:
- Requires processing resources.
- Incorrect assumptions about traffic can create false positives.

---

## 6. Understanding Inline IPS Deployment

An inline IPS sits directly in the communication path.

This means traffic passes through the IPS before reaching its destination.

### Example

```text
Computer A
    |
    v
   IPS
    |
    v
Computer B
```

If the IPS identifies traffic that matches a prevention rule, it can stop that traffic before it reaches Computer B.

### Passive IDS Deployment

```text
Computer A -------- Switch -------- Computer B
                       |
                       v
                    IDS Sensor
```

The IDS receives a copy of traffic through a monitoring mechanism.

### Important Difference

An inline IPS can affect network availability if it fails, is incorrectly configured, or blocks legitimate traffic.

This is why production deployments require careful testing and recovery planning.

---

## 7. Understanding Fail-Open and Fail-Close

These are two important IPS deployment concepts.

### A. Fail-Open

If the IPS fails, traffic is allowed to continue through the network.

Advantages:
- Network availability may be preserved.
- An IPS failure is less likely to interrupt communication.

Disadvantages:
- Traffic may pass without IPS inspection.
- Security protection may be temporarily reduced.

### B. Fail-Close

If the IPS fails, traffic is blocked.

Advantages:
- Prevents uninspected traffic from passing through the protected point.

Disadvantages:
- A device failure can interrupt network communication.
- Availability may be affected.

| Concept | Fail-Open | Fail-Close |
|---|---|---|
| IPS failure | Traffic continues | Traffic is blocked |
| Availability | Generally higher | Can be lower |
| Security enforcement during failure | Reduced | More restrictive |
| Common concern | Uninspected traffic | Service interruption |

The appropriate approach depends on the organisation's security and availability requirements.

---

## 8. IPS Prevention Actions

An IPS can perform different actions depending on its technology and configuration.

| Action | Explanation |
|---|---|
| Alert | Record and report suspicious activity |
| Drop | Discard matching traffic |
| Reject | Discard traffic and send a rejection response where supported |
| Block | Prevent matching communication |
| Rate-limit | Restrict the rate of matching traffic |
| Reset | Terminate a connection where supported |

Not every IPS supports every action.

### Important Concept

A detection rule does not automatically become a prevention rule.

The IPS must be deployed in a supported prevention mode, and the relevant action must be configured correctly.

---

## 9. Suricata as an IPS

Suricata supports inline intrusion prevention.

In an appropriately configured inline deployment, it can inspect traffic and apply prevention rules.

### Suricata Rule Example

```suricata
drop tcp any any -> 192.168.1.20 22 (msg:"Block Matching SSH Traffic"; sid:2000001; rev:1;)
```

This is an illustrative prevention rule.

It describes dropping matching TCP traffic directed toward the specified destination and port.

However, the rule alone does not guarantee blocking.

The deployment must support inline inspection, and the capture and firewall integration must be configured correctly.

### Alert Versus Drop

```suricata
alert tcp any any -> 192.168.1.20 22 (msg:"SSH Traffic Alert"; sid:2000002; rev:1;)
```

The `alert` action generates an alert.

```suricata
drop tcp any any -> 192.168.1.20 22 (msg:"Drop Matching SSH Traffic"; sid:2000003; rev:1;)
```

The `drop` action is intended for an appropriate inline prevention deployment.

---

## 10. Understanding False Positives and False Negatives

### False Positive

A false positive occurs when legitimate activity is incorrectly identified as malicious.

Example:

A legitimate administrator performs an unusual network operation, and the IPS incorrectly blocks it.

### False Negative

A false negative occurs when malicious activity is not detected.

Example:

An attacker uses a technique that does not match the IPS detection rules.

### Comparison

| Type | Meaning |
|---|---|
| False Positive | Legitimate activity identified as malicious |
| False Negative | Malicious activity not detected |

### Reducing False Positives

1. Review detection rules.
2. Tune security policies.
3. Understand normal network behaviour.
4. Test changes before deployment.
5. Monitor alerts.
6. Document exceptions carefully.

### Reducing False Negatives

1. Update detection rules.
2. Monitor threat intelligence.
3. Use multiple detection methods.
4. Inspect relevant network traffic.
5. Review security incidents.
6. Test detection coverage.

---

## 11. IPS and Firewalls

Firewalls and IPS technologies work together but have different responsibilities.

### Firewall

A firewall controls traffic using security policies such as:

- Source IP address.
- Destination IP address.
- Protocol.
- Port number.
- Connection state.

### IPS

An IPS examines traffic for suspicious patterns and can prevent matching activity.

### Example

A firewall may permit HTTPS traffic on TCP port 443.

An IPS can inspect visible HTTPS-related traffic metadata and other available information to identify suspicious activity.

However, encrypted application payloads generally cannot be inspected as plaintext without an authorised decryption arrangement.

### Comparison

| Feature | Firewall | IPS |
|---|---|---|
| Traffic filtering | Yes | Yes, in prevention deployments |
| Port-based policies | Yes | Can use them |
| Attack signature inspection | Not its primary role | Yes |
| Protocol analysis | Varies | Yes |
| Prevents matching threats | Yes, through filtering policies | Yes, through prevention rules |

---

## 12. IPS Deployment Best Practices

Organisations should follow these practices:

1. Identify critical network assets.
2. Understand normal network traffic.
3. Choose suitable deployment locations.
4. Test rules before enforcement.
5. Monitor performance.
6. Keep detection rules updated.
7. Review false positives.
8. Maintain secure logging.
9. Document prevention policies.
10. Prepare a recovery plan.
11. Monitor availability.
12. Review security events regularly.

### Why Testing Matters

Imagine an IPS rule blocks all traffic to a server.

The organisation may lose access to an important service.

Testing helps identify such problems before rules are deployed into production.

---

## 13. Understanding IPS and SIEM Integration

A Security Information and Event Management (SIEM) system collects and analyses security information from different sources.

An IPS can send alerts and security events to a SIEM.

Example:

```text
       Network Traffic
              |
              v
             IPS
              |
              v
       Security Alerts
              |
              v
             SIEM
              |
              v
       Security Analyst
```

A SIEM can correlate IPS events with:

- Firewall logs.
- Authentication events.
- Endpoint security alerts.
- Server logs.
- DNS events.
- Other security information.

This helps analysts understand larger security incidents.

---

# 14. Practical Lab: Understanding IPS Behaviour

## Objective

Learn how IPS prevention decisions work by examining fictional security policies and simulated events.

This lab is deliberately simulation-based. Do not enable inline IPS, create drop rules, or modify your active firewall.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson12
cd ~/module14-lab/lesson12
```

### Step 2: Create an IPS Policy

```bash
nano ips-policy.txt
```

Enter:

```text
IPS POLICY – TRAINING LAB

1. Allow legitimate HTTPS communication.
2. Alert on suspicious SSH connection patterns.
3. Block traffic matching a confirmed malicious signature.
4. Record every prevention decision.
5. Review blocked traffic regularly.
6. Test new rules before enforcement.
7. Maintain a recovery procedure.
```

Save the file.

### Step 3: Create Simulated IPS Events

```bash
nano ips-events.txt
```

Enter:

```text
EVENT 1
Source: 192.168.1.10
Destination: 192.168.1.20
Protocol: TCP
Port: 443
Activity: Normal HTTPS communication
Decision: ALLOW

EVENT 2
Source: 192.168.1.50
Destination: 192.168.1.20
Protocol: TCP
Port: 22
Activity: Suspicious SSH pattern
Decision: ALERT

EVENT 3
Source: 192.168.1.60
Destination: 192.168.1.20
Protocol: TCP
Port: 8080
Activity: Matches a fictional confirmed malicious signature
Decision: DROP
```

Save the file.

### Step 4: Review the Events

```bash
cat ips-events.txt
```

Answer:

1. Which event represents normal communication?
2. Which event generates an alert?
3. Which event represents prevention?
4. Why should the IPS record blocked traffic?
5. Why should legitimate HTTPS communication not be blocked unnecessarily?

### Step 5: Search for Prevention Decisions

```bash
grep -i "decision" ips-events.txt
```

Search for dropped traffic:

```bash
grep -i "DROP" ips-events.txt
```

Search for alerts:

```bash
grep -i "ALERT" ips-events.txt
```

### Step 6: Create an IPS Investigation Report

```bash
nano ips-investigation-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 12
INTRUSION PREVENTION SYSTEMS

1. IPS Definition:

2. Difference Between IDS and IPS:

3. Types of IPS:

4. IPS Detection Techniques:

5. Inline Deployment:

6. Fail-Open:

7. Fail-Close:

8. Prevention Actions:

9. Suricata IPS Support:

10. False Positives:

11. False Negatives:

12. Firewall and IPS Integration:

13. SIEM Integration:

14. Practical Lab Findings:

15. Conclusion:
```

Save your report.

---

## 15. Knowledge Check

Answer these questions:

1. What is an Intrusion Prevention System?
2. Explain the difference between IDS and IPS.
3. What is a Network-Based IPS?
4. What is a Host-Based IPS?
5. Explain signature-based detection.
6. What is anomaly-based detection?
7. What is stateful protocol analysis?
8. What does inline deployment mean?
9. Explain fail-open and fail-close.
10. What does the `drop` action do in an appropriate IPS deployment?
11. What is a false positive?
12. What is a false negative?
13. Why should IPS rules be tested before enforcement?
14. How does an IPS work with a firewall?
15. What is the role of SIEM in IPS monitoring?

---

## 16. Lesson Summary

In this lesson, you learned:

- Intrusion Prevention System fundamentals.
- Differences between IDS and IPS.
- Types of IPS.
- IPS detection techniques.
- Inline deployment.
- Fail-open and fail-close.
- Prevention actions.
- Suricata IPS capabilities.
- False positives and false negatives.
- Firewall and IPS integration.
- SIEM integration.
- IPS deployment best practices.
- Practical IPS event analysis.

**Next Lesson: Module 14 – Lesson 13: IDS/IPS Integration and SIEM.**