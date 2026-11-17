---
---

# Module 14 – Lesson 05: nftables Fundamentals

## 1. Introduction to nftables

**nftables** is a modern Linux framework used to configure packet filtering, firewall rules, and Network Address Translation (NAT).

It was designed to replace much of the older iptables infrastructure.

nftables works with the Linux kernel's Netfilter framework.

### Why Was nftables Developed?

Traditional iptables has separate command-line tools and rule-management approaches for IPv4, IPv6, ARP, and other protocol families.

nftables provides a more unified approach.

Its advantages include:

1. Unified rule management.
2. Support for IPv4 and IPv6.
3. Improved rule organisation.
4. More flexible rule expressions.
5. Support for sets and maps.
6. Efficient packet processing.
7. A modern replacement for much of the iptables functionality.

### Real-World Application

Imagine a company operating several Linux servers.

The security administrator needs to:

- Allow HTTPS connections.
- Restrict SSH access.
- Block unnecessary traffic.
- Manage IPv4 and IPv6 rules.
- Monitor firewall activity.

nftables can provide a central framework for implementing these firewall policies.

---

## 2. Understanding nftables Architecture

nftables uses three major components:

### A. Tables

Tables organise firewall rules.

### B. Chains

Chains contain rules and can be attached to specific Netfilter hooks.

### C. Rules

Rules define conditions and actions for processing network packets.

### Simplified Architecture

```text
              NETFILTER
                  |
                  v
               NFTABLES
                  |
             +----+----+
             |         |
             v         v
          TABLES     TABLES
             |
             v
           CHAINS
             |
             v
            RULES
             |
       +-----+-----+
       |     |     |
       v     v     v
     ACCEPT DROP REJECT
```

---

## 3. nftables Families

A family determines the type of network traffic a table handles.

Common families include:

| Family | Purpose |
|---|---|
| ip | IPv4 traffic |
| ip6 | IPv6 traffic |
| inet | IPv4 and IPv6 traffic |
| arp | ARP traffic |
| bridge | Bridged network traffic |
| netdev | Network-device-level filtering |

### Understanding the inet Family

The `inet` family is particularly useful because it allows administrators to manage IPv4 and IPv6 filtering within one table.

Example table name:

```text
inet filter
```

This represents a table called `filter` belonging to the `inet` family.

---

## 4. Understanding nftables Tables

A table is a container for chains, rules, sets, and other objects.

For example:

```text
Table: inet filter
```

This creates an organisation structure for firewall rules.

Unlike the predefined iptables tables, nftables allows administrators to create tables with meaningful names.

Examples:

- `inet filter`
- `ip nat`
- `ip6 security`

A table does not automatically filter traffic. Filtering happens through appropriately configured base chains and rules.

---

## 5. Understanding nftables Chains

A chain is an ordered collection of firewall rules.

There are two important chain types.

### A. Base Chain

A base chain is attached to a Netfilter hook.

It participates in packet processing.

Example hooks include:

- input
- output
- forward
- prerouting
- postrouting

### B. Regular Chain

A regular chain is a reusable collection of rules.

It is not directly attached to a Netfilter hook.

Another chain can jump to it to process packets.

### Example Structure

```text
inet filter
     |
     +-- input (base chain)
     |
     +-- output (base chain)
     |
     +-- forward (base chain)
     |
     +-- ssh_rules (regular chain)
```

---

## 6. Understanding nftables Chain Types

Base chains can use different types depending on their purpose.

### Filter

Used for packet filtering.

### NAT

Used for network address translation.

### Route

Used for routing-related processing of locally generated packets in supported configurations.

Example conceptual chain:

```text
type filter hook input priority 0;
```

Explanation:

- `type filter`: The chain performs filtering.
- `hook input`: It is attached to the INPUT processing point.
- `priority 0`: It specifies the chain's processing priority.

---

## 7. Understanding nftables Commands

The main command-line utility is:

```bash
nft
```

### Display nftables Version

```bash
nft --version
```

### Display All Tables

```bash
sudo nft list tables
```

### Display Complete Ruleset

```bash
sudo nft list ruleset
```

### Display a Specific Table

```bash
sudo nft list table inet filter
```

This displays the table if it exists.

### Display Help

```bash
nft --help
```

These commands are useful for inspecting existing firewall configurations.

---

## 8. Creating nftables Tables

The following is an illustrative configuration command:

```bash
sudo nft add table inet filter
```

It creates a table named `filter` in the `inet` family.

Before creating a table, check whether it already exists.

If a table with the same name exists, the command may fail.

**Do not execute this command during the inspection lab.**

---

## 9. Creating nftables Chains

Example:

```bash
sudo nft add chain inet filter input { type filter hook input priority 0 \; }
```

This creates an input base chain.

Explanation:

- `add chain`: Creates a chain.
- `inet`: Selects the IPv4/IPv6 family.
- `filter`: Selects the table.
- `input`: Names the chain.
- `type filter`: Sets the chain type.
- `hook input`: Attaches the chain to incoming local traffic.
- `priority 0`: Defines its priority.

The escaped semicolon terminates the chain configuration within the shell command.

This is an example for understanding syntax, not an instruction to modify your current firewall.

---

## 10. Adding nftables Rules

A rule defines how matching packets should be handled.

### Example: Allow HTTPS

```bash
sudo nft add rule inet filter input tcp dport 443 accept
```

This allows matching incoming TCP traffic destined for port 443.

### Example: Drop Telnet Traffic

```bash
sudo nft add rule inet filter input tcp dport 23 drop
```

This drops matching incoming TCP traffic destined for port 23.

### Example: Allow SSH from a Trusted IP

```bash
sudo nft add rule inet filter input ip saddr 192.168.1.10 tcp dport 22 accept
```

This allows matching IPv4 TCP SSH traffic from the specified source address.

These are illustrative examples. Do not apply them to your existing firewall configuration.

---

## 11. Understanding nftables Sets

A set allows administrators to group multiple values together.

For example, an administrator may maintain a list of trusted IP addresses.

Conceptual example:

```text
set trusted_hosts {
    type ipv4_addr
    elements = { 192.168.1.10, 192.168.1.20 }
}
```

Sets are useful because they reduce the need to create separate rules for every individual address.

They can also simplify firewall maintenance.

---

## 12. Understanding nftables Maps

A map associates one value with another.

Maps can be used to organise related network information and create more flexible rule logic.

For example, a firewall administrator could associate different network addresses with specific actions or values.

Sets store collections of values, while maps store key-value relationships.

---

## 13. Understanding nftables NAT

nftables supports Network Address Translation.

Common NAT operations include:

### SNAT

Changes the source IP address.

### DNAT

Changes the destination IP address.

### Masquerade

Performs source NAT using the address associated with an outgoing interface.

Example conceptual rule:

```bash
sudo nft add rule ip nat postrouting oifname "eth0" masquerade
```

This is an illustrative NAT rule. It is not suitable for every system and must be used only in an appropriately designed routing configuration.

NAT is not a substitute for firewall filtering.

---

## 14. nftables Rule Management

Administrators need to understand how to inspect and manage rules.

### List Rules

```bash
sudo nft list ruleset
```

### List Rules with Handles

```bash
sudo nft -a list ruleset
```

Handles identify individual rules.

### Delete a Rule

An illustrative deletion command uses a rule handle:

```bash
sudo nft delete rule inet filter input handle 5
```

This removes the rule with handle 5 from the specified chain.

Always inspect the current ruleset and handle values before deleting anything.

### Flush a Table

```bash
sudo nft flush table inet filter
```

This removes the rules and other objects contained in the specified table.

**Warning:** Flushing a table can remove important protections. Do not use this command on your working firewall.

---

## 15. Understanding nftables Logging

nftables can log matching packets.

Example:

```bash
sudo nft add rule inet filter input tcp dport 22 log prefix "SSH ATTEMPT: "
```

This logs matching incoming SSH packets.

Logging rules need careful placement because a logging statement alone does not necessarily block the traffic.

A later rule or chain policy determines what happens.

Firewall logs can help investigate:

- Repeated connection attempts.
- Unexpected access.
- Suspicious traffic.
- Unauthorised service requests.

---

## 16. iptables vs nftables

| Feature | iptables | nftables |
|---|---|---|
| Architecture | Traditional interface | Modern framework |
| IPv4 and IPv6 | Historically separate tools | Unified `inet` family |
| Organisation | Tables and chains | Tables, chains, sets and maps |
| Rule syntax | Traditional syntax | Modern expression-based syntax |
| Management | Multiple utilities historically | Unified `nft` utility |
| Modern Linux | Legacy or compatibility environments | Preferred modern framework |

### Important

Some Linux distributions use the iptables command through an nftables compatibility backend.

That does not mean the two command interfaces are identical.

Avoid managing the same firewall rules independently through multiple tools without understanding how the system integrates them.

---

# 17. Practical Lab: Inspecting nftables

## Objective

Inspect nftables on your Kali Linux VM without changing existing firewall configurations.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson05
cd ~/module14-lab/lesson05
```

### Step 2: Check nftables Version

```bash
nft --version
```

Record the version.

### Step 3: Display Existing Tables

```bash
sudo nft list tables
```

Record the tables you find.

### Step 4: Display the Complete Ruleset

```bash
sudo nft list ruleset
```

Observe:

- Table names.
- Families.
- Chain names.
- Chain types.
- Hooks.
- Rules.
- Default policies, where configured.

### Step 5: Display Rules with Handles

```bash
sudo nft -a list ruleset
```

Observe the rule handles if any are displayed.

### Step 6: Inspect the inet Family

If an `inet` table exists, inspect it using its actual name.

For example:

```bash
sudo nft list table inet filter
```

If the table does not exist, record that it was not found.

### Step 7: Inspect the nftables Service

```bash
systemctl status nftables --no-pager
```

Record whether the service is active, inactive, or unavailable.

Remember: The service status alone does not prove whether firewall rules are present.

### Step 8: Create Your Report

```bash
nano nftables-inspection-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 05
NFTABLES FUNDAMENTALS

1. nftables Version:

2. Existing Tables:

3. Ruleset Overview:

4. Table Families:

5. Chain Names:

6. Chain Types:

7. Netfilter Hooks:

8. Rule Handles:

9. inet Family Inspection:

10. nftables Service Status:

11. Security Observations:

12. Recommendations:

13. Conclusion:
```

Save your report.

---

# 18. Knowledge Check

Answer these questions:

1. What is nftables?
2. Why was nftables developed?
3. What is the relationship between nftables and Netfilter?
4. What are the three main nftables components?
5. What is an nftables family?
6. What is the purpose of the inet family?
7. Differentiate between a base chain and a regular chain.
8. What is a Netfilter hook?
9. Explain the purpose of chain priority.
10. What does `nft list ruleset` display?
11. What is the purpose of an nftables set?
12. Differentiate between sets and maps.
13. What is the purpose of SNAT and DNAT?
14. Why is NAT not a complete firewall solution?
15. What is a rule handle?
16. Why should administrators avoid flushing active firewall tables?
17. What is the difference between iptables and nftables?

---

# 19. Lesson Summary

In this lesson, you learned:

- Introduction to nftables.
- Its relationship with Netfilter.
- nftables architecture.
- Families, tables, chains, and rules.
- Base chains and regular chains.
- Netfilter hooks and priorities.
- nftables commands.
- Adding tables, chains, and rules.
- Sets and maps.
- NAT fundamentals.
- Rule handles and management.
- Firewall logging.
- Differences between iptables and nftables.
- How to inspect nftables safely.

**Next Lesson: Module 14 – Lesson 06: Firewall Rules and Policies.**
