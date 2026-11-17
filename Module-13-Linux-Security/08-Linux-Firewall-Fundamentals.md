---
---


# Module 13 – Lesson 08: Linux Firewall Fundamentals

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define a firewall.
2. Explain why Linux systems need firewalls.
3. Understand inbound and outbound traffic.
4. Understand packet filtering.
5. Identify common Linux firewall technologies.
6. Understand UFW and nftables.
7. Inspect firewall rules.
8. Explain firewall policies.
9. Understand common firewall misconfigurations.
10. Perform a basic Linux firewall security audit.

---

## 2. What Is a Firewall?

A firewall is a security system that monitors and controls network traffic according to predefined rules.

It helps determine which network connections should be permitted or blocked.

### Simple Analogy

Imagine a university security gate.

- Authorised visitors are allowed inside.
- Unauthorised visitors are denied entry.
- Security personnel monitor access.
- Certain areas may have additional restrictions.

A firewall performs a similar function for network traffic.

### Why Firewalls Are Important

Firewalls help:

- Block unauthorised network connections.
- Restrict access to network services.
- Reduce exposure to attacks.
- Control incoming traffic.
- Control outgoing traffic.
- Enforce network security policies.

A firewall is an important security layer, but it does not replace secure passwords, updates, endpoint protection, or proper system configuration.

---

## 3. Understanding Network Traffic

A firewall examines network traffic moving through a computer or network.

There are three important traffic directions.

### 3.1 Inbound Traffic

Inbound traffic enters a system from another device.

Example:

A remote computer attempts to connect to your Linux SSH server.

### 3.2 Outbound Traffic

Outbound traffic leaves your system.

Example:

Your Kali machine connects to a website to download a package.

### 3.3 Forwarded Traffic

Forwarded traffic passes through a Linux system that is routing traffic between network interfaces.

Example:

A Linux machine operates as a router between two networks.

### Traffic Diagram

```text
             INTERNET
                 |
                 |
          +------+------+
          |   FIREWALL  |
          +------+------+
                 |
                 |
          +------+------+
          |  LINUX HOST |
          +-------------+
```

The firewall applies rules to determine how traffic should be handled.

---

## 4. How Firewalls Work

Firewalls evaluate network packets against configured rules.

A rule may consider:

- Source IP address.
- Destination IP address.
- Source port.
- Destination port.
- Network protocol.
- Connection state.
- Incoming interface.
- Outgoing interface.

### Example Rule

Imagine a server should accept SSH connections only from a trusted administration network.

A firewall rule could permit TCP traffic to port 22 from the authorised network and block other SSH connection attempts.

### Common Firewall Actions

| Action | Meaning |
|---|---|
| ACCEPT | Allow the traffic |
| DROP | Silently discard the traffic |
| REJECT | Deny the traffic, usually with a response |
| LOG | Record matching traffic |

The exact available actions depend on the firewall technology.

---

## 5. Stateful and Stateless Firewalls

### 5.1 Stateless Firewall

A stateless firewall evaluates individual packets using configured rules.

It does not maintain a connection-state table in the same way a stateful firewall does.

### 5.2 Stateful Firewall

A stateful firewall tracks connection information.

It can recognise whether traffic belongs to an established or related connection.

For example:

A Linux machine initiates an outbound connection to a web server.

The firewall can recognise the returning packets as part of that established connection.

### Comparison

| Stateless | Stateful |
|---|---|
| Evaluates packets individually | Tracks connection states |
| Does not maintain connection state in the same way | Maintains connection tracking |
| Uses packet-level rules | Can apply rules based on connection state |
| Useful for simple filtering | Useful for modern connection-aware policies |

---

## 6. Common Linux Firewall Technologies

Linux has several firewall management technologies.

### 6.1 Netfilter

Netfilter is the Linux kernel framework that provides packet filtering, network address translation, and related network packet processing capabilities.

### 6.2 iptables

`iptables` is a traditional user-space tool for configuring firewall rules using Netfilter.

It remains present on many Linux systems, although newer systems often use nftables underneath.

### 6.3 nftables

`nftables` is the modern Linux packet-filtering framework.

Its user-space management command is:

```bash
nft
```

### 6.4 UFW

UFW stands for **Uncomplicated Firewall**.

It provides a simpler interface for managing firewall rules.

Example:

```bash
sudo ufw status
```

### Technology Relationship

```text
          Linux Kernel
               |
            Netfilter
               |
       +-------+-------+
       |               |
    nftables         iptables
       |
    nft command

       UFW
   Simplified
   Management
   Interface
```

UFW and nftables are not identical technologies. UFW is a management interface, while nftables is a firewall framework.

---

## 7. Understanding UFW

UFW is designed to make firewall administration easier.

### Check Whether UFW Is Installed

```bash
command -v ufw
```

### Check UFW Status

```bash
sudo ufw status
```

### Display Detailed Status

```bash
sudo ufw status verbose
```

### Display Numbered Rules

```bash
sudo ufw status numbered
```

These commands inspect the firewall configuration.

### Important Warning

Do not enable, disable, reset, or modify firewall rules during this lesson.

Changing firewall settings can interrupt network access or remote administration.

---

## 8. Understanding Firewall Policies

A firewall policy defines the default treatment of traffic that does not match a more specific rule.

Two common policies are:

### Default Allow

Traffic is allowed unless a rule blocks it.

### Default Deny

Traffic is blocked unless a rule explicitly allows it.

### Example

Imagine a Linux server that provides only SSH and HTTPS services.

A restrictive policy could:

- Permit authorised SSH traffic.
- Permit HTTPS traffic.
- Block unnecessary inbound connections.

This reduces the server's exposed network services.

### Principle of Least Privilege

A firewall should allow only the network traffic necessary for the system's intended purpose.

This is called network least privilege.

---

## 9. Understanding Linux Firewall Rules

Firewall rules define how matching traffic should be handled.

For example, a rule may specify:

```text
Protocol: TCP
Destination Port: 443
Action: ACCEPT
```

This describes a rule that allows matching HTTPS traffic.

Another example:

```text
Protocol: TCP
Destination Port: 23
Action: DROP
```

This describes a rule that blocks matching Telnet traffic.

These are conceptual examples, not commands to apply to your system.

### Why Port Restrictions Matter

An exposed service may become an entry point for attackers if it is vulnerable or improperly configured.

Administrators should:

1. Identify active services.
2. Determine which services are necessary.
3. Restrict unnecessary access.
4. Monitor network activity.
5. Review firewall rules regularly.

---

## 10. Understanding nftables

nftables is a modern Linux firewall framework.

It supports:

- Packet filtering.
- Network address translation.
- Stateful filtering.
- Rule sets.
- Tables and chains.
- IPv4 and IPv6 traffic processing.

### Important nftables Concepts

**Table:** A container for firewall chains and related objects.

**Chain:** A collection of rules that process packets.

**Rule:** A condition and action applied to matching traffic.

**Set:** A collection of values that can be referenced by rules.

### Inspect nftables Rules

```bash
sudo nft list ruleset
```

If nftables is not installed or configured, the output may be empty or the command may be unavailable.

Do not add or delete firewall rules in this lesson.

---

## 11. Understanding iptables

iptables is another tool for managing Linux firewall rules.

It uses tables and chains to process packets.

Important built-in chains include:

| Chain | Purpose |
|---|---|
| INPUT | Traffic destined for the local system |
| OUTPUT | Traffic originating from the local system |
| FORWARD | Traffic routed through the system |

### Inspect Existing Rules

```bash
sudo iptables -L -n -v
```

Inspect NAT rules:

```bash
sudo iptables -t nat -L -n -v
```

These are inspection commands.

Some Linux systems use the iptables compatibility layer backed by nftables. Therefore, iptables output does not always mean the system uses the legacy iptables implementation.

---

## 12. Firewall Security Risks

Incorrect firewall configuration can weaken system security.

### Risk 1: Unnecessary Open Ports

Services may be exposed even when they are not required.

### Risk 2: Overly Permissive Rules

A rule may allow more traffic than necessary.

### Risk 3: Incorrect Default Policy

An overly permissive default policy may increase exposure.

### Risk 4: Missing IPv6 Protection

A firewall may protect IPv4 traffic while leaving IPv6 traffic insufficiently restricted.

### Risk 5: Unmonitored Firewall Changes

Unauthorised modifications may go unnoticed.

### Risk 6: Remote Administration Lockout

A poorly planned firewall change can block the administrator's connection.

### Defensive Recommendations

- Review firewall rules regularly.
- Apply least privilege.
- Protect both IPv4 and IPv6.
- Monitor firewall configuration changes.
- Keep firewall software updated.
- Back up configurations before authorised changes.
- Test changes in a controlled environment.
- Maintain a recovery path.

---

## 13. Firewall Logging and Monitoring

Firewall logs help administrators understand network activity.

Depending on the implementation, logs may include:

- Blocked connection attempts.
- Accepted connections.
- Source and destination addresses.
- Network protocols.
- Port information.
- Firewall rule matches.

### Inspect Kernel Firewall Messages

```bash
sudo journalctl -k -b --no-pager | grep -Ei 'UFW|DROP|REJECT|nft|iptables' | tail -n 30
```

### Inspect UFW Logs

If the UFW logging service is configured:

```bash
sudo grep -i UFW /var/log/ufw.log | tail -n 30
```

The log file may not exist on every system.

### Why Monitoring Matters

Firewall monitoring can help identify:

- Repeated blocked connections.
- Unusual network activity.
- Unexpected service exposure.
- Possible scanning activity.
- Misconfigured network policies.

A blocked packet alone does not prove a successful attack.

---

# 14. Practical Lab: Linux Firewall Security Audit

## Lab Objective

Inspect the firewall technologies and firewall configuration available on your Kali Linux system.

This is an inspection-only exercise.

Do not enable, disable, reset, or modify firewall rules.

### Step 1: Create Your Workspace

```bash
mkdir -p ~/module13-lab/lesson08
cd ~/module13-lab/lesson08
```

### Step 2: Check UFW Availability

```bash
command -v ufw
```

### Step 3: Inspect UFW Status

```bash
sudo ufw status verbose
```

### Step 4: Inspect nftables Availability

```bash
command -v nft
```

### Step 5: Inspect nftables Rules

```bash
sudo nft list ruleset
```

### Step 6: Inspect iptables Availability

```bash
command -v iptables
```

### Step 7: Inspect iptables Rules

```bash
sudo iptables -L -n -v
```

### Step 8: Inspect NAT Rules

```bash
sudo iptables -t nat -L -n -v
```

### Step 9: Inspect Listening Network Services

```bash
sudo ss -tulpen
```

Look for:

- TCP listeners.
- UDP listeners.
- Listening ports.
- Associated processes.
- Local IP addresses.

Do not assume every listening service is exposed to the internet. Binding addresses and network topology matter.

### Step 10: Inspect Firewall-Related Logs

```bash
sudo journalctl -k -b --no-pager | grep -Ei 'UFW|DROP|REJECT|nft|iptables' | tail -n 30
```

### Step 11: Generate Your Firewall Security Report

Run:

```bash
{
    echo "LINUX FIREWALL SECURITY AUDIT"
    echo "=============================="
    echo
    echo "UFW AVAILABILITY"
    command -v ufw
    echo
    echo "UFW STATUS"
    sudo ufw status verbose
    echo
    echo "NFTABLES AVAILABILITY"
    command -v nft
    echo
    echo "NFTABLES RULESET"
    sudo nft list ruleset
    echo
    echo "IPTABLES AVAILABILITY"
    command -v iptables
    echo
    echo "IPTABLES FILTER RULES"
    sudo iptables -L -n -v
    echo
    echo "IPTABLES NAT RULES"
    sudo iptables -t nat -L -n -v
    echo
    echo "LISTENING NETWORK SERVICES"
    sudo ss -tulpen
    echo
    echo "FIREWALL-RELATED KERNEL LOGS"
    sudo journalctl -k -b --no-pager | grep -Ei 'UFW|DROP|REJECT|nft|iptables' | tail -n 30
} > firewall-security-report.txt 2>&1
```

Inspect the report:

```bash
cat firewall-security-report.txt
```

### Expected Files

```text
lesson08/
└── firewall-security-report.txt
```

---

## 15. Real-World Cybersecurity Application

Imagine you are responsible for securing a university Linux server.

The server provides:

- A website.
- SSH administration.
- An internal application.

Your firewall audit discovers that several unnecessary ports are accessible.

Your investigation should:

1. Identify the services using those ports.
2. Determine which services are required.
3. Identify the intended users and networks.
4. Review existing firewall rules.
5. Recommend restrictive access policies.
6. Consider both IPv4 and IPv6.
7. Establish firewall monitoring.
8. Document the changes required.

Never block services without understanding their operational purpose.

---

## 16. Knowledge Check

Answer these questions in your notebook.

1. What is a firewall?
2. Why are firewalls important?
3. Differentiate between inbound and outbound traffic.
4. What is forwarded traffic?
5. Explain how packet filtering works.
6. Differentiate between stateful and stateless firewalls.
7. What is Netfilter?
8. What is UFW?
9. What is nftables?
10. What is iptables?
11. What is the difference between ACCEPT, DROP, and REJECT?
12. What is a default firewall policy?
13. Explain the Principle of Least Privilege in firewall administration.
14. What are INPUT, OUTPUT, and FORWARD chains?
15. Why should IPv6 firewall protection be considered?
16. Why can changing firewall rules remotely be dangerous?
17. How can administrators inspect firewall rules?
18. Identify five common firewall security risks.

---

## 17. Practical Completion Checklist

- [ ] I understand firewalls.
- [ ] I understand inbound, outbound, and forwarded traffic.
- [ ] I understand packet filtering.
- [ ] I understand stateful and stateless firewalls.
- [ ] I understand Netfilter.
- [ ] I understand UFW.
- [ ] I understand nftables.
- [ ] I understand iptables.
- [ ] I can inspect firewall rules.
- [ ] I can identify listening network services.
- [ ] I understand firewall logging.
- [ ] I completed the firewall security audit.
- [ ] I generated my firewall security report.

**Lesson 08 Complete: Linux Firewall Fundamentals.**
