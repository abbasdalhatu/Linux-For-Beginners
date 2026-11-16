
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 09: FIREWALL SECURITY SCENARIOS

## 1. Introduction

In real-world cybersecurity, firewall administrators must make decisions based on security requirements, business needs, and network activity.

A firewall is not simply a tool for blocking traffic. It is a security control that determines which communications should be permitted and which should be denied.

In this lab, you will investigate realistic firewall security situations and recommend appropriate defensive solutions.

All IP addresses and organizations in this lab are fictional.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Analyze firewall security situations.
2. Identify overly permissive firewall policies.
3. Recommend secure firewall rules.
4. Explain source IP restrictions.
5. Identify unnecessary service exposure.
6. Investigate suspicious outbound traffic.
7. Understand rule-ordering problems.
8. Recommend safe firewall changes.
9. Document security findings professionally.

## 3. Important Firewall Security Principles

### Principle 1: Least Privilege

Allow only the network communication required for legitimate operations.

Example:

A database server should not accept connections from every device on the internet.

### Principle 2: Default Deny

A restrictive firewall policy denies traffic unless an explicit rule permits it.

Example:

```text
Default incoming policy: DENY
Default outgoing policy: ALLOW
```

This is one possible policy, not a universal requirement. Outbound restrictions may also be necessary.

### Principle 3: Restrict Source Addresses

Where possible, permit administrative access only from trusted addresses or networks.

### Principle 4: Minimize Exposed Services

Every unnecessary exposed service increases the potential attack surface.

### Principle 5: Monitor and Review

Firewall rules and logs should be reviewed regularly.

### Principle 6: Change Management

Firewall changes should be planned, documented, tested, and reversible.

## 4. Scenario 1: Securing a Public Web Server

### Situation

A fictional organization operates a public website.

Its server has these services:

| Service | Port | Protocol |
|---|---:|---|
| HTTP | 80 | TCP |
| HTTPS | 443 | TCP |
| SSH | 22 | TCP |
| MySQL | 3306 | TCP |

The organization wants visitors to access the website while administrators maintain secure remote access.

### Your Task

Design a firewall policy.

Consider:

- Public website traffic.
- Administrative SSH access.
- Database access.
- Default incoming policy.

### Recommended Security Design

| Traffic | Recommended action | Reason |
|---|---|---|
| Incoming TCP 80 | Allow if HTTP is required | Website access |
| Incoming TCP 443 | Allow | Secure website access |
| Incoming TCP 22 | Restrict to trusted administrator IPs | Secure administration |
| Incoming TCP 3306 | Deny public access | Protect database |
| Other unnecessary incoming traffic | Deny | Reduce exposure |

### Important Observation

A database service should normally communicate only with authorized application servers or trusted administrative systems.

Never expose database administration ports publicly without a specific, justified architecture and appropriate protections.

## 5. Scenario 2: Restricting SSH Access

### Situation

A company has a Linux server with SSH enabled.

The administrator connects from this fictional address:

```text
203.0.113.25
```

The organization wants SSH access restricted to that administrator.

### Security Requirements

- Allow SSH from the trusted administrator address.
- Deny SSH from other untrusted sources.
- Preserve access for authorized administrators.
- Monitor unsuccessful connection attempts.

### Your Task

Answer:

1. Why is restricting SSH access useful?
2. What is the purpose of source IP filtering?
3. What could happen if the administrator's address changes?
4. Why should console or recovery access be available before applying restrictive SSH rules?

### Key Lesson

Source IP restrictions reduce exposure, but they should not replace strong authentication, secure SSH configuration, and monitoring.

## 6. Scenario 3: Preventing Public Database Exposure

### Situation

A company operates an application server and a database server.

The fictional network is:

```text
Application Server: 10.10.10.20
Database Server:    10.10.10.30
Database Port:      3306
```

The database should accept connections only from the application server.

### Recommended Policy

```text
Source: 10.10.10.20
Destination: 10.10.10.30
Protocol: TCP
Destination Port: 3306
Action: ALLOW
```

Other unauthorized sources should not be permitted to reach the database service.

### Your Task

Explain:

1. Why should the database not accept connections from everyone?
2. Why is the application server's IP address specified?
3. What could happen if port 3306 is exposed publicly?
4. What additional security controls should protect the database?

### Additional Defensive Controls

- Strong database authentication.
- Database user privileges.
- Encryption in transit.
- Regular patching.
- Database activity monitoring.
- Backups and recovery procedures.

## 7. Scenario 4: Unexpected Outbound Connections

### Situation

A security administrator notices repeated outbound connections from a Linux server to an unfamiliar external IP address.

The server normally communicates only with approved application services.

### Possible Explanations

- An unauthorized application.
- Malware activity.
- Misconfigured software.
- A legitimate service using a new endpoint.
- A compromised user account.

### Investigation Steps

1. Record the destination IP and destination port.
2. Identify the process responsible for the connection.
3. Inspect the connection's timing and frequency.
4. Review system and application logs.
5. Compare the destination with approved business services.
6. Escalate suspicious activity according to incident-response procedures.

### Useful Commands

Inspect current connections:

```bash
sudo ss -tunap
```

Inspect listening services:

```bash
sudo ss -tulpn
```

Inspect recent system logs:

```bash
sudo journalctl -n 100 --no-pager
```

### Your Task

Explain why administrators should investigate an unfamiliar connection before blocking it.

**Important:** An unfamiliar destination is not automatically malicious. Verify the process and business purpose before making a decision.

## 8. Scenario 5: DNS Failure Caused by Firewall Policy

### Situation

Employees report that they cannot access websites using domain names.

However, some direct IP connectivity tests succeed.

The network administrator discovers that outbound DNS traffic may be restricted.

### DNS Information

| Protocol | Port | Purpose |
|---|---:|---|
| UDP | 53 | Common DNS queries |
| TCP | 53 | DNS responses and operations requiring TCP |

### Your Task

1. Explain why DNS is important.
2. Why can blocking DNS affect website access?
3. Why should both UDP and TCP DNS traffic be considered?
4. What commands can help investigate DNS failures?

### Recommended Investigation

```bash
getent hosts example.com
```

```bash
cat /etc/resolv.conf
```

If available:

```bash
resolvectl status
```

### Key Lesson

Firewall policies must support legitimate infrastructure services such as DNS, DHCP, and required application communications.

## 9. Scenario 6: Firewall Rule-Ordering Problem

### Situation

A firewall contains these conceptual rules:

```text
Rule 1: DENY all incoming traffic
Rule 2: ALLOW incoming TCP port 443
```

The administrator expects HTTPS traffic to be allowed, but the first matching rule blocks it.

### Your Task

Answer:

1. Why might the HTTPS rule never be reached?
2. What is rule ordering?
3. How can administrators prevent conflicting rules?
4. Why should rules be tested after changes?

### Key Lesson

Firewall evaluation depends on the firewall framework and configuration.

Some systems evaluate rules in sequence, while others use chains, priorities, sets, or additional control flow.

Always understand the specific firewall's evaluation model.

## 10. Scenario 7: Excessive Firewall Logging

### Situation

A server generates thousands of firewall log messages every hour.

The administrator discovers that a broad rule logs every rejected packet.

### Possible Problems

- Important security events become difficult to identify.
- Log storage may grow rapidly.
- Monitoring systems may become overloaded.
- Investigations may take longer.

### Your Task

Recommend ways to improve firewall logging.

### Recommended Practices

- Use appropriate logging levels.
- Avoid unnecessarily logging every repeated event.
- Apply rate limiting where supported.
- Retain relevant security events.
- Monitor storage consumption.
- Forward important logs to a protected central logging system.

Never disable security logging completely just to reduce log volume.

## 11. Scenario 8: Firewall Change and Rollback Planning

### Situation

An administrator needs to change a production firewall policy to permit a new application.

The change could affect existing network services.

### Your Task

Create a change plan containing:

1. Business reason for the change.
2. Current firewall configuration backup.
3. Proposed rule.
4. Expected impact.
5. Testing procedure.
6. Approval requirements.
7. Rollback procedure.
8. Final verification.

### Example Change Plan

```text
CHANGE ID: FW-2026-001

PURPOSE:
Permit authorized application traffic.

CURRENT STATE:
Application communication is blocked.

PROPOSED CHANGE:
Add a narrowly scoped allow rule.

EXPECTED IMPACT:
The approved application should communicate successfully.

TESTING:
Verify the application connection and inspect firewall logs.

ROLLBACK:
Restore the previous approved firewall configuration.

VERIFICATION:
Confirm existing services remain functional.
```

### Important Warning

Never make restrictive firewall changes remotely without a reliable recovery method.

A mistaken rule can lock administrators out of the system.

## 12. Practical Exercise: Build a Security Decision Matrix

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module07/lab09
```

Enter the directory:

```bash
cd ~/practical-labs/module07/lab09
```

Create a report:

```bash
nano firewall-security-scenarios.txt
```

Use this template:

```text
MODULE 07 - LAB 09
FIREWALL SECURITY SCENARIOS

SCENARIO 1: PUBLIC WEB SERVER

Required Ports:
Restricted Ports:
Recommended Policy:
Reason:

SCENARIO 2: SSH RESTRICTION

Trusted Source:
Required Action:
Security Benefit:
Potential Risk:

SCENARIO 3: DATABASE EXPOSURE

Database Server:
Application Server:
Required Policy:
Additional Security Controls:

SCENARIO 4: SUSPICIOUS OUTBOUND TRAFFIC

Observed Activity:
Possible Causes:
Evidence to Collect:
Recommended Response:

SCENARIO 5: DNS FAILURE

Possible Cause:
Investigation Commands:
Recommended Solution:

SCENARIO 6: RULE ORDERING

Problem:
Explanation:
Recommended Correction:

SCENARIO 7: EXCESSIVE LOGGING

Problem:
Possible Impact:
Recommended Improvements:

SCENARIO 8: CHANGE MANAGEMENT

Change Purpose:
Testing:
Approval:
Rollback:
Verification:

FINAL SECURITY RECOMMENDATIONS:

1.
2.
3.
4.
5.
```

Save and exit:

- CTRL + O
- ENTER
- CTRL + X

## 13. Knowledge Check

1. What is the principle of least privilege?
2. Why should database ports not normally be exposed publicly?
3. What is source IP filtering?
4. Why is SSH restriction useful?
5. What could an unexpected outbound connection indicate?
6. Why must DNS traffic be considered in firewall policies?
7. What is a firewall rule-ordering problem?
8. Why can excessive logging become a problem?
9. What is a rollback plan?
10. Why should firewall changes be documented?

## 14. Lab Completion Checklist

- [ ] Analyzed a public web server security scenario.
- [ ] Designed an SSH access restriction policy.
- [ ] Analyzed database exposure risks.
- [ ] Investigated suspicious outbound traffic conceptually.
- [ ] Analyzed DNS filtering problems.
- [ ] Explained firewall rule ordering.
- [ ] Recommended logging improvements.
- [ ] Created a firewall change and rollback plan.
- [ ] Completed the security decision matrix.
- [ ] Completed the knowledge check.

## 15. Expected Outcome

You should now be able to analyze practical firewall security problems and recommend defensive solutions.

You have also learned how firewall administration connects to:

- Network security.
- Server hardening.
- Incident response.
- Access control.
- Security monitoring.
- Change management.

**Safety reminder:** This lab uses fictional scenarios and conceptual policies. Do not apply the example rules to your active network or production systems.
