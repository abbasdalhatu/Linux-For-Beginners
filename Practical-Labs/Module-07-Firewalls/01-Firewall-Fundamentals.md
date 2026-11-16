
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 01: FIREWALL FUNDAMENTALS

## 1. Introduction

A firewall is a security mechanism that monitors and controls network traffic entering or leaving a computer or network.

It decides which connections should be allowed and which should be blocked based on predefined security rules.

Think of a firewall as a security guard at the entrance of a building:

- It checks who is entering.
- It checks who is leaving.
- It decides whether access should be permitted.
- It blocks traffic that violates its rules.

In cybersecurity, firewalls help protect computers, servers, and networks from unauthorized access.

## 2. Learning Objectives

By the end of this practical, you should be able to:

1. Explain what a firewall is.
2. Understand how firewall rules work.
3. Differentiate inbound and outbound traffic.
4. Identify common firewall types.
5. Understand default firewall policies.
6. Inspect firewall information in Kali Linux.
7. Explain why firewall configuration must be handled carefully.

## 3. How a Firewall Works

A firewall examines network traffic and compares it against configured rules.

For example, imagine a Linux server with these rules:

| Traffic | Action |
|---|---|
| Incoming SSH on port 22 | Allow |
| Incoming HTTP on port 80 | Allow |
| Incoming connection on port 4444 | Block |
| Outgoing DNS traffic | Allow |

When traffic arrives, the firewall checks whether a rule matches it.

If an appropriate rule permits the connection, the traffic is allowed. If a rule blocks it, the traffic is rejected or silently dropped.

## 4. Understanding Network Traffic

There are two important traffic directions.

### A. Inbound Traffic

Inbound traffic is network communication entering your computer.

Examples:

- Someone connecting to your SSH server.
- A browser requesting a web page from your server.
- Another computer attempting to access a service on your machine.

### B. Outbound Traffic

Outbound traffic is network communication leaving your computer.

Examples:

- Your computer requesting a website.
- Kali Linux downloading package updates.
- Your machine sending DNS queries to resolve domain names.

### C. Quick Comparison

| Feature | Inbound | Outbound |
|---|---|---|
| Direction | Entering | Leaving |
| Example | SSH connection to your machine | Downloading a package |
| Security concern | Unauthorized access | Unauthorized data transfer |

## 5. Common Types of Firewalls

### A. Packet-Filtering Firewall

Examines individual network packets using information such as:

- Source IP address.
- Destination IP address.
- Source port.
- Destination port.
- Protocol.

### B. Stateful Firewall

Tracks active network connections.

It understands whether a packet belongs to an existing connection.

For example, when your computer requests a website, a stateful firewall can recognize the response as part of that established connection.

### C. Application-Layer Firewall

Examines application-level traffic.

It can inspect protocols such as HTTP and identify certain types of application activity.

### D. Host-Based Firewall

Installed or configured on an individual computer.

Examples include:

- Windows Defender Firewall.
- Linux UFW.
- Linux Netfilter-based firewall configurations.

### E. Network-Based Firewall

Protects an entire network or network segment.

It is commonly deployed between internal networks and external networks.

## 6. Understanding Firewall Policies

A firewall uses policies to determine what happens to network traffic.

Two important policies are:

**ALLOW:** Permits matching traffic.

**DENY:** Prevents matching traffic.

Common default policies include:

- Default allow: Traffic is generally permitted unless a rule blocks it.
- Default deny: Traffic is generally blocked unless a rule permits it.

A restrictive default-deny policy can improve security, but it must be configured carefully.

Blocking required traffic can interrupt network access or essential services.

## 7. Understanding Ports and Protocols

Firewalls frequently use port numbers and network protocols when filtering traffic.

| Port | Protocol | Common Purpose |
|---|---|---|
| 22 | TCP | SSH |
| 53 | UDP/TCP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 25 | TCP | SMTP |
| 3306 | TCP | MySQL |

Remember:

- IP addresses identify network endpoints.
- Ports identify communication endpoints for services.
- Protocols define communication rules.

A firewall can use these details to control network access.

## 8. Practical Exercise 1: Inspect Network Interfaces

Open your Kali Linux terminal.

Run:

```bash
ip addr
```

This displays network interfaces and their IP addresses.

Next, run:

```bash
ip route
```

This displays your routing table.

Look for your default gateway.

Then run:

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

Record the following:

1. Your network interface name.
2. Your Kali IP address.
3. Your default gateway.
4. Any listening ports you can identify.

Do not assume every listening service is dangerous. Determine what application owns each port before drawing conclusions.

## 9. Practical Exercise 2: Inspect Existing Firewall Tools

Check whether UFW is installed:

```bash
command -v ufw
```

Check whether the nftables command is available:

```bash
command -v nft
```

Check whether iptables is available:

```bash
command -v iptables
```

These commands inspect tool availability. They do not change firewall settings.

Now inspect the current UFW status:

```bash
sudo ufw status verbose
```

Inspect the current nftables ruleset:

```bash
sudo nft list ruleset
```

Inspect the iptables rules:

```bash
sudo iptables -S
```

And:

```bash
sudo iptables -L -n -v
```

Some systems may show empty rulesets or report that a firewall tool is unavailable. That is useful information to record, not a reason to install or change anything during this first lab.

## 10. Practical Exercise 3: Identify Firewall Components

Create a working directory:

```bash
mkdir -p ~/practical-labs/module07/lab01
```

Move into it:

```bash
cd ~/practical-labs/module07/lab01
```

Create a notes file:

```bash
nano firewall-observations.txt
```

Record your observations using this template:

```text
MODULE 07 - LAB 01
FIREWALL FUNDAMENTALS

Network Interface:
Kali IP Address:
Default Gateway:

UFW Installed:
NFTables Available:
IPTables Available:

UFW Status:
NFTables Status:

Listening Services:
1.
2.
3.

Observations:
```

Save the file.

In Nano:

- Press CTRL + O.
- Press ENTER.
- Press CTRL + X.

## 11. Important Firewall Security Principles

### Principle 1: Least Privilege

Allow only the network communication required for legitimate operations.

### Principle 2: Default Deny

Where appropriate, block unnecessary traffic by default and explicitly permit required communication.

### Principle 3: Defense in Depth

A firewall should work alongside:

- Strong authentication.
- System updates.
- Access control.
- Logging and monitoring.
- Endpoint protection.

### Principle 4: Logging

Firewall logs help security analysts investigate suspicious connections and understand network activity.

### Principle 5: Change Management

Before changing firewall rules:

1. Understand the existing configuration.
2. Identify services that depend on network access.
3. Prepare a rollback plan.
4. Test changes in an isolated environment.
5. Verify the resulting configuration.

Never blindly flush firewall rules or disable security protections.

## 12. Knowledge Check

Answer these questions in your own understanding:

1. What is a firewall?
2. What is the difference between inbound and outbound traffic?
3. What is packet filtering?
4. What makes a stateful firewall different?
5. What is the difference between ALLOW and DENY?
6. What is a default-deny policy?
7. What is the purpose of port 443?
8. Why should firewall changes be tested carefully?

## 13. Lab Completion Checklist

- [ ] Understood firewall fundamentals.
- [ ] Learned inbound and outbound traffic.
- [ ] Studied common firewall types.
- [ ] Understood firewall policies.
- [ ] Inspected network interfaces.
- [ ] Inspected listening ports.
- [ ] Checked firewall tool availability.
- [ ] Inspected existing firewall configurations.
- [ ] Created `firewall-observations.txt`.
- [ ] Completed the knowledge check.

## 14. Expected Outcome

After completing this lab, you should understand the purpose of firewalls, how they control network communication, and how to inspect existing firewall configurations in Kali Linux.

**Safety reminder:** This first lab is inspection-only. Do not change firewall rules yet. We will introduce controlled configuration in later labs using an isolated test environment.
