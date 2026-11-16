
# LINUX FIREWALL FUNDAMENTALS

## 1. What Is a Firewall?

A firewall is a security mechanism that monitors and controls network traffic according to predefined rules.

It helps protect computers and networks from unauthorized access.

## 2. Types of Firewall Rules

- Incoming rules: Control traffic entering a system.
- Outgoing rules: Control traffic leaving a system.
- Allow rules: Permit specified traffic.
- Deny rules: Block specified traffic.

## 3. What Is UFW?

UFW means Uncomplicated Firewall.

It provides a simple command-line interface for managing firewall rules on many Linux distributions.

Check its status:

```bash
sudo ufw status
```

Display detailed status:

```bash
sudo ufw status verbose
```

## 4. Enable and Disable UFW

Enable the firewall:

```bash
sudo ufw enable
```

Disable the firewall:

```bash
sudo ufw disable
```

Reload firewall rules:

```bash
sudo ufw reload
```

Important: Before enabling a firewall on a remote machine, make sure your remote administration connection will not be blocked.

## 5. Allow Network Traffic

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

Allow a specific port:

```bash
sudo ufw allow 8080/tcp
```

## 6. Deny Network Traffic

Block a specific port:

```bash
sudo ufw deny 23/tcp
```

Deny traffic from an IP address:

```bash
sudo ufw deny from 192.168.1.50
```

## 7. Delete Firewall Rules

Display numbered rules:

```bash
sudo ufw status numbered
```

Delete a rule by its number:

```bash
sudo ufw delete 2
```

Replace 2 with the correct rule number displayed on your system.

## 8. Default Firewall Policies

Deny incoming connections:

```bash
sudo ufw default deny incoming
```

Allow outgoing connections:

```bash
sudo ufw default allow outgoing
```

These are common workstation policies, but firewall policies should be chosen according to the system's role and requirements.

## 9. Other Linux Firewall Technologies

### nftables

A modern Linux framework for packet filtering and network address translation.

### iptables

A traditional command-line interface for configuring Linux packet-filtering rules.

### firewalld

A firewall management service commonly used on some Linux distributions.

## 10. Inspect Firewall Configuration

Check whether UFW is installed:

```bash
ufw --version
```

Check firewall rules using nftables:

```bash
sudo nft list ruleset
```

Inspect iptables rules:

```bash
sudo iptables -L -n -v
```

Note: These commands inspect different firewall frameworks. Their availability depends on your Linux distribution and configuration.

## 11. Cybersecurity Applications

Firewalls help security professionals:

- Restrict unauthorized network access.
- Protect exposed services.
- Control network communication.
- Reduce attack surfaces.
- Implement network security policies.
- Investigate suspicious traffic.

## 12. Security Best Practices

- Allow only necessary traffic.
- Avoid exposing unnecessary services.
- Review firewall rules regularly.
- Protect remote administration access.
- Remove outdated rules.
- Monitor firewall logs.
- Test rule changes carefully.

## 13. Practice Exercises

1. Explain what a firewall does.
2. What does UFW stand for?
3. Display the current UFW status.
4. Explain the difference between allow and deny rules.
5. Which command allows SSH traffic?
6. What is nftables?
7. Why should unnecessary ports be blocked?

## Summary

A Linux firewall controls network traffic and helps prevent unauthorized access. Understanding UFW and other firewall technologies is an important cybersecurity skill.
