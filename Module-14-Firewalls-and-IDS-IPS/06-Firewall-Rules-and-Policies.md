# Module 14 – Lesson 06: Firewall Rules and Policies

## 1. Introduction to Firewall Rules

A firewall rule is an instruction that tells a firewall how to handle network traffic that matches specified conditions.

Firewall rules help administrators decide:

- Which traffic is allowed.
- Which traffic is blocked.
- Which IP addresses can connect.
- Which ports can be accessed.
- Which protocols are permitted.
- Which traffic should be logged.

### Real-World Example

Imagine a university network containing:

- Student computers.
- Staff computers.
- An administration server.
- A database server.
- A public university website.

The university's security team may create firewall rules that:

1. Allow students to access the public website.
2. Allow authorised staff to access administration systems.
3. Restrict database access to approved application servers.
4. Block unnecessary external connections to internal systems.
5. Allow administrators to manage servers securely.

This is how firewall rules help enforce network security policies.

---

## 2. Understanding Firewall Policies

A firewall policy is a set of security decisions that defines how network traffic should be handled.

A policy establishes rules for controlling network communication.

### Common Firewall Policies

**Default Allow Policy**

Allows traffic unless a rule specifically blocks it.

**Default Deny Policy**

Blocks traffic unless a rule specifically permits it.

### Which Is More Secure?

A default-deny approach is commonly preferred for inbound traffic.

Why?

Because only explicitly authorised connections are permitted.

This follows the cybersecurity principle of least privilege.

However, a firewall policy must still allow all legitimate services required for the system to function.

---

## 3. Components of a Firewall Rule

A firewall rule may contain several elements.

| Component | Meaning |
|---|---|
| Source IP | Where traffic originates |
| Destination IP | Where traffic is going |
| Source port | Port used by the sending application |
| Destination port | Port of the receiving service |
| Protocol | TCP, UDP, ICMP, etc. |
| Direction | Incoming, outgoing, or forwarded |
| Action | Allow, drop, reject, or log |
| Interface | Network interface associated with traffic |
| Connection state | NEW, ESTABLISHED, RELATED, INVALID |

### Example

Consider this conceptual rule:

```text
Source: 192.168.1.10
Destination: Linux Server
Protocol: TCP
Destination Port: 22
Action: ALLOW
```

Meaning:

The firewall allows matching SSH traffic from the specified source address to the Linux server.

---

## 4. Understanding Rule Order

Firewall rule order is important because many firewall systems evaluate rules sequentially.

When a packet matches a terminating rule, processing may stop.

### Example

Suppose a firewall has these rules:

```text
Rule 1: Allow SSH from trusted IP addresses.
Rule 2: Allow HTTPS traffic.
Rule 3: Block all other incoming traffic.
```

A matching trusted SSH connection is allowed by Rule 1.

A matching HTTPS connection is allowed by Rule 2.

Other incoming traffic is blocked by Rule 3.

### What Happens If the Order Is Wrong?

Consider:

```text
Rule 1: Block all incoming traffic.
Rule 2: Allow HTTPS traffic.
```

Depending on the firewall and chain behaviour, the first rule may prevent the HTTPS rule from being reached.

### Security Lesson

Always review rule order before applying firewall changes.

---

## 5. Understanding Allow Rules

An allow rule permits traffic that matches specified conditions.

Example using UFW:

```bash
sudo ufw allow 443/tcp
```

Meaning:

Allow incoming TCP traffic to port 443.

Example using iptables:

```bash
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

Example using nftables:

```bash
sudo nft add rule inet filter input tcp dport 443 accept
```

These commands illustrate equivalent filtering intentions using different interfaces.

They are examples only. Do not execute them during this lesson.

---

## 6. Understanding Block Rules

A block rule prevents matching traffic from reaching its intended destination.

Two common actions are DROP and REJECT.

### DROP

Silently discards matching packets.

### REJECT

Blocks matching traffic and returns a rejection response where supported.

Example using UFW:

```bash
sudo ufw deny 23/tcp
```

Example using iptables:

```bash
sudo iptables -A INPUT -p tcp --dport 23 -j DROP
```

Example using nftables:

```bash
sudo nft add rule inet filter input tcp dport 23 drop
```

These examples illustrate how different firewall tools express similar filtering policies.

---

## 7. Understanding Source IP Restrictions

Administrators may restrict access to specific trusted IP addresses.

Example policy:

```text
Allow SSH from:
192.168.1.10

Block unauthorised SSH access from other sources.
```

Why is this useful?

It reduces the number of devices that can attempt to access an administrative service.

However, trusted IP addresses must be managed carefully, especially where networks use DHCP or dynamic addressing.

---

## 8. Understanding Destination Port Restrictions

A destination port identifies the service that receives a network connection.

Examples:

| Port | Service |
|---|---|
| 22 | SSH |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |

### Example

A database server may need to accept connections from an application server.

The database administrator can restrict database access to the application server's trusted IP address.

This is safer than exposing the database service to every network.

---

## 9. Understanding Protocol-Based Rules

Firewall rules can filter traffic by protocol.

### TCP

TCP provides reliable, connection-oriented communication.

Examples:

- SSH.
- HTTP.
- HTTPS.

### UDP

UDP provides connectionless communication.

Examples:

- DNS queries.
- DHCP.
- Some streaming and real-time applications.

### ICMP

ICMP supports network diagnostics and error reporting.

Example:

The `ping` command commonly uses ICMP Echo Request and Echo Reply messages.

A firewall should not automatically block all ICMP traffic because some ICMP messages are important for normal network operation and troubleshooting.

---

## 10. Understanding Connection-State Rules

Stateful firewall rules can consider the state of a network connection.

Common connection states:

### NEW

A packet is attempting to establish a new connection.

### ESTABLISHED

The packet belongs to an existing tracked connection.

### RELATED

The packet belongs to a connection related to an existing connection.

### INVALID

The packet does not belong to a valid tracked connection.

### Example

A firewall may permit new outbound connections and allow the returning traffic belonging to established connections.

This helps provide secure communication without unnecessarily opening broad inbound access.

---

## 11. Understanding Firewall Interfaces

A Linux machine may have multiple network interfaces.

Examples:

- `eth0`
- `ens33`
- `wlan0`
- `lo`

A firewall can apply different rules depending on the network interface.

### Example

A server may have:

- One interface connected to the public network.
- Another interface connected to an internal management network.

The administrator can apply different access policies to each interface.

### Important

Always identify the actual interface names before designing interface-specific firewall rules.

Do not assume every Linux machine uses `eth0`.

---

## 12. Understanding Firewall Rule Counters

Firewall systems may maintain counters showing how many packets and bytes match particular rules.

Example iptables command:

```bash
sudo iptables -L -n -v
```

Output may include:

```text
pkts bytes target
120  9600  ACCEPT
35   2100  DROP
```

Meaning:

- `pkts`: Number of matching packets.
- `bytes`: Total matching data size.
- `target`: Configured action.

Counters help administrators understand how rules are being used.

A zero counter does not automatically mean that a rule is unnecessary. It may simply not have matched traffic during the observation period.

---

## 13. Understanding Firewall Rule Documentation

Professional firewall administrators document important rules.

A useful documentation table:

| Rule ID | Source | Destination | Protocol | Port | Action | Purpose |
|---|---|---|---|---|---|---|
| FW01 | Trusted admin network | Linux server | TCP | 22 | Allow | SSH administration |
| FW02 | Any | Web server | TCP | 443 | Allow | HTTPS access |
| FW03 | Untrusted networks | Database server | TCP | 3306 | Block | Restrict database access |

Documentation should also record:

- Rule owner.
- Approval.
- Date created.
- Reason for the rule.
- Expiry or review date.
- Testing results.

This makes firewall administration more accountable.

---

## 14. Firewall Security Best Practices

1. Apply least privilege.
2. Prefer default-deny for inbound traffic where appropriate.
3. Allow only required ports.
4. Restrict administrative access.
5. Review rule ordering.
6. Remove obsolete rules after approval.
7. Monitor rule counters and logs.
8. Document every important rule.
9. Consider both IPv4 and IPv6.
10. Test changes before applying them to production.
11. Maintain a recovery plan.
12. Review firewall policies regularly.

### Common Firewall Mistakes

- Allowing all traffic unnecessarily.
- Exposing database ports publicly.
- Forgetting IPv6 filtering.
- Applying rules in the wrong order.
- Deleting required SSH access.
- Failing to monitor firewall logs.
- Keeping outdated rules indefinitely.

---

# 15. Practical Lab: Firewall Rule Analysis

## Objective

Learn to analyse firewall rules and document security policies without changing your active firewall configuration.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson06
cd ~/module14-lab/lesson06
```

### Step 2: Inspect UFW Rules

```bash
sudo ufw status numbered
```

Record the available rules.

### Step 3: Inspect iptables Rules

```bash
sudo iptables -L -n -v --line-numbers
```

Observe:

- Rule order.
- Actions.
- Protocols.
- Ports.
- Packet counters.
- Default policies.

### Step 4: Inspect nftables Rules

```bash
sudo nft -a list ruleset
```

Observe:

- Tables.
- Chains.
- Rule order.
- Actions.
- Handles.

### Step 5: Analyse a Sample Firewall Policy

Study this hypothetical policy:

```text
RULE 1:
Source: 192.168.1.10
Destination: Linux Server
Protocol: TCP
Port: 22
Action: ALLOW

RULE 2:
Source: Any
Destination: Web Server
Protocol: TCP
Port: 443
Action: ALLOW

RULE 3:
Source: Any
Destination: Database Server
Protocol: TCP
Port: 3306
Action: BLOCK

RULE 4:
Source: Any
Destination: Linux Server
Action: BLOCK
```

Answer:

1. Which rule permits SSH access?
2. Which rule permits HTTPS access?
3. Which rule blocks database access?
4. Why is the trusted SSH source restricted?
5. What is the purpose of the final rule?
6. What might happen if the final rule appeared before the allow rules?

Assume the rules are processed sequentially and matching terminal actions stop evaluation.

### Step 6: Create a Firewall Policy Document

```bash
nano firewall-policy-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 06
FIREWALL RULES AND POLICIES

1. Existing UFW Rules:

2. Existing iptables Rules:

3. Existing nftables Rules:

4. Rule Ordering Observations:

5. Allow Rules:

6. Block Rules:

7. Source IP Restrictions:

8. Destination Port Restrictions:

9. Connection-State Observations:

10. Firewall Rule Counters:

11. Sample Policy Analysis:

12. Security Recommendations:

13. Conclusion:
```

Save your report.

---

## 16. Knowledge Check

Answer these questions:

1. What is a firewall rule?
2. What is a firewall policy?
3. Differentiate between default-allow and default-deny.
4. Why is default-deny commonly preferred for inbound traffic?
5. Identify six possible components of a firewall rule.
6. Why does firewall rule order matter?
7. Differentiate between ACCEPT, DROP, and REJECT.
8. Why might an administrator restrict SSH access by source IP?
9. Why should database ports be restricted?
10. Explain the purpose of protocol-based filtering.
11. What are NEW, ESTABLISHED, RELATED, and INVALID?
12. Why are network interfaces important when configuring firewall rules?
13. What are firewall rule counters?
14. Why should firewall rules be documented?
15. Identify five common firewall configuration mistakes.

---

## 17. Lesson Summary

In this lesson, you learned:

- Firewall rules and policies.
- Default-allow and default-deny.
- Components of firewall rules.
- Rule ordering.
- Allow and block rules.
- Source IP and destination port restrictions.
- TCP, UDP, and ICMP filtering.
- Connection-state rules.
- Network interface filtering.
- Firewall counters.
- Firewall documentation.
- Firewall security best practices.
- How to analyse firewall configurations.

**Next Lesson: Module 14 – Lesson 07: Inbound and Outbound Traffic Control.**
