---
---

# Module 14 – Lesson 03: UFW Firewall Management

## 1. Introduction to UFW

**UFW (Uncomplicated Firewall)** is a command-line firewall management tool designed to make configuring Linux firewall rules easier.

It provides a simpler interface for managing firewall policies without requiring administrators to write complicated low-level rules manually.

UFW is commonly used on Ubuntu and other Debian-based Linux distributions.

### Why Is UFW Important?

UFW helps administrators:

1. Allow or block network connections.
2. Control incoming traffic.
3. Control outgoing traffic.
4. Restrict access to specific ports.
5. Allow connections from trusted IP addresses.
6. Manage firewall rules.
7. Inspect firewall status.

### Real-World Example

Imagine a university Linux web server.

The server provides an HTTPS website to students.

The administrator may configure UFW to:

- Allow HTTPS traffic.
- Restrict SSH access to trusted administrators.
- Block unnecessary incoming connections.

This helps reduce the server's exposure to unauthorised access.

---

## 2. How UFW Works

UFW acts as a simplified management interface for Linux firewall functionality.

It allows administrators to define policies and rules using readable commands.

Example:

```bash
sudo ufw allow 443/tcp
```

This allows incoming TCP connections to port 443 under the applicable UFW configuration.

Another example:

```bash
sudo ufw deny 23/tcp
```

This blocks matching incoming Telnet traffic.

Important: A rule's actual effect depends on the firewall backend, existing rules, rule ordering, and default policies.

---

## 3. Checking Whether UFW Is Installed

Before using UFW, check whether it is installed.

```bash
ufw version
```

If the command is available, it displays the installed version.

If you receive:

```text
command not found
```

UFW may not be installed.

Check package information:

```bash
apt policy ufw
```

Do not install or activate UFW yet. We will inspect your existing firewall configuration first.

---

## 4. Understanding UFW Status

The following command displays the firewall status:

```bash
sudo ufw status
```

For more information:

```bash
sudo ufw status verbose
```

Possible results include:

### Active

```text
Status: active
```

This means UFW is enabled.

### Inactive

```text
Status: inactive
```

This means UFW is not currently enabled.

### Not Installed

If the command is unavailable, UFW may not be installed.

Remember: UFW being inactive does not prove that the system has no firewall protection. Other firewall tools may be managing rules.

---

## 5. Understanding UFW Default Policies

UFW supports default policies.

### Allow Incoming

```bash
sudo ufw default allow incoming
```

This allows incoming traffic by default.

### Deny Incoming

```bash
sudo ufw default deny incoming
```

This blocks incoming traffic by default unless an applicable rule permits it.

### Allow Outgoing

```bash
sudo ufw default allow outgoing
```

This allows outgoing traffic by default.

### Deny Outgoing

```bash
sudo ufw default deny outgoing
```

This blocks outgoing traffic by default unless permitted.

### Important Security Principle

A common server configuration uses:

- Deny incoming by default.
- Allow outgoing by default.

However, firewall policies must match the server's actual requirements.

Do not change your default policies during this lesson.

---

## 6. Allowing Network Connections

The `allow` command permits matching traffic.

### Allow SSH

```bash
sudo ufw allow 22/tcp
```

Allows incoming TCP connections to port 22.

### Allow HTTP

```bash
sudo ufw allow 80/tcp
```

Allows incoming HTTP traffic.

### Allow HTTPS

```bash
sudo ufw allow 443/tcp
```

Allows incoming HTTPS traffic.

### Allow DNS

```bash
sudo ufw allow 53
```

Allows matching traffic to port 53.

For production configurations, specify the appropriate protocol and direction based on the DNS service's requirements.

---

## 7. Blocking Network Connections

The `deny` command blocks matching traffic.

Example:

```bash
sudo ufw deny 23/tcp
```

This blocks incoming TCP traffic to port 23.

Another example:

```bash
sudo ufw deny 21/tcp
```

This blocks incoming TCP traffic to port 21.

These examples illustrate rule syntax. They are not instructions to change your active firewall configuration.

---

## 8. Allowing Connections from Specific IP Addresses

Sometimes administrators want only trusted computers to access an administrative service.

Example:

```bash
sudo ufw allow from 192.168.1.10
```

This permits traffic from the specified source address under the applicable rule configuration.

To restrict access to SSH:

```bash
sudo ufw allow from 192.168.1.10 to any port 22 proto tcp
```

This permits TCP SSH access from the specified IP address.

### Why Is This Useful?

An administrator may restrict SSH access to a trusted management computer instead of exposing SSH to every possible source.

Only use trusted addresses that have been verified.

---

## 9. Deleting UFW Rules

Administrators sometimes need to remove outdated rules.

First, display numbered rules:

```bash
sudo ufw status numbered
```

Example:

```text
[ 1] 22/tcp ALLOW IN Anywhere
[ 2] 443/tcp ALLOW IN Anywhere
```

A numbered rule can be deleted with:

```bash
sudo ufw delete 1
```

Warning: Deleting rules by number changes the rule list. Always inspect the current numbered list immediately before deleting anything.

Do not delete firewall rules during this lesson.

---

## 10. Reloading and Resetting UFW

### Reload

```bash
sudo ufw reload
```

Reloads the firewall configuration.

### Reset

```bash
sudo ufw reset
```

Resets UFW configuration and removes its managed rules.

**Important:** A reset can remove important firewall protections. Never run it on a production server without an approved recovery plan.

We will not perform either operation in this lesson.

---

## 11. Understanding UFW Logging

UFW can record firewall-related activity.

To inspect the current logging setting:

```bash
sudo ufw status verbose
```

Common logging levels include:

- off
- low
- medium
- high
- full

Logging can help administrators investigate:

- Blocked connections.
- Suspicious connection attempts.
- Unexpected traffic.
- Repeated access attempts.

On systems using UFW, firewall logs may appear in:

```text
/var/log/ufw.log
```

Availability depends on the logging configuration and system logging setup.

You can inspect the file if it exists:

```bash
sudo tail -n 30 /var/log/ufw.log
```

You can also inspect the system journal:

```bash
sudo journalctl -n 50 --no-pager
```

---

## 12. Understanding UFW Application Profiles

UFW can use application profiles to simplify firewall configuration.

These profiles describe application-related network rules.

Display available profiles:

```bash
sudo ufw app list
```

Inspect a particular profile:

```bash
sudo ufw app info OpenSSH
```

The available profile names depend on installed applications.

Application profiles are useful because administrators can refer to a service by its profile name instead of manually specifying every port.

---

## 13. UFW Security Best Practices

1. Inspect firewall status before making changes.
2. Use default-deny for inbound traffic when appropriate.
3. Allow only necessary services.
4. Restrict SSH access to trusted sources.
5. Monitor firewall logs.
6. Review rules regularly.
7. Avoid unnecessary open ports.
8. Keep firewall policies documented.
9. Test changes before production deployment.
10. Maintain a recovery method before changing remote firewall settings.

### Important: SSH Lockout Prevention

If you are connected to a remote Linux server through SSH, enabling a firewall without first permitting the required SSH connection can disconnect you.

Always plan and verify administrative access before applying firewall changes.

---

# 14. Practical Lab: UFW Inspection

## Objective

Inspect UFW on your Kali Linux VM and understand its configuration without changing active firewall settings.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module14-lab/lesson03
cd ~/module14-lab/lesson03
```

### Step 2: Check UFW Version

```bash
ufw version
```

Record whether UFW is installed.

### Step 3: Check Firewall Status

```bash
sudo ufw status
```

### Step 4: Display Detailed Status

```bash
sudo ufw status verbose
```

Record:

- Whether UFW is active.
- Default incoming policy, if displayed.
- Default outgoing policy, if displayed.
- Logging status.

### Step 5: Inspect Numbered Rules

```bash
sudo ufw status numbered
```

If there are no rules, record that observation.

### Step 6: Inspect Application Profiles

```bash
sudo ufw app list
```

If UFW is installed, inspect the OpenSSH profile where available:

```bash
sudo ufw app info OpenSSH
```

### Step 7: Inspect UFW Logs

Check whether the log exists:

```bash
ls -l /var/log/ufw.log
```

If it exists:

```bash
sudo tail -n 30 /var/log/ufw.log
```

If it does not exist, record that it was unavailable.

### Step 8: Create Your Report

```bash
nano ufw-inspection-report.txt
```

Use this template:

```text
MODULE 14 – LESSON 03
UFW FIREWALL MANAGEMENT

1. UFW Installation Status:

2. UFW Version:

3. Firewall Active/Inactive Status:

4. Default Incoming Policy:

5. Default Outgoing Policy:

6. Existing Firewall Rules:

7. Numbered Rules:

8. Application Profiles:

9. Logging Configuration:

10. Log Inspection Results:

11. Security Observations:

12. Recommendations:

13. Conclusion:
```

Save your report.

---

# 15. Knowledge Check

Answer these questions:

1. What does UFW stand for?
2. Why was UFW developed?
3. What command displays UFW's status?
4. What is the difference between `allow` and `deny`?
5. What does `sudo ufw allow 443/tcp` do?
6. What is a default firewall policy?
7. Why is default-deny commonly used for incoming traffic?
8. How can an administrator restrict SSH access to a particular IP address?
9. What does `sudo ufw status numbered` display?
10. Why must firewall rules be inspected before deleting them?
11. What is the purpose of UFW logging?
12. What are UFW application profiles?
13. Why can enabling a firewall lock out a remote administrator?
14. What is the danger of resetting UFW?
15. Why should firewall rules be documented?

---

# 16. Lesson Summary

In this lesson, you learned:

- What UFW is.
- How UFW works.
- How to inspect UFW installation and status.
- Default firewall policies.
- Allowing and blocking traffic.
- Restricting access by IP address.
- Viewing and deleting rules.
- Reloading and resetting firewall configurations.
- Firewall logging.
- Application profiles.
- UFW security best practices.

**Next Lesson: Module 14 – Lesson 04: iptables Fundamentals.**
