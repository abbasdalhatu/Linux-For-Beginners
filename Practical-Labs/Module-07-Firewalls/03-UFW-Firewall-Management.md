
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 03: UFW FIREWALL MANAGEMENT

## 1. Introduction

UFW stands for **Uncomplicated Firewall**.

It is a command-line firewall management tool designed to make Linux firewall configuration easier.

Instead of writing complex low-level firewall rules manually, administrators can use simple commands to manage network access.

UFW works with Linux firewall infrastructure, typically through Netfilter.

For example, an administrator can use UFW to:

- Allow SSH connections.
- Block unwanted incoming connections.
- Control outgoing traffic.
- Manage application access.
- Inspect firewall rules.
- Enable firewall logging.

UFW is especially useful for beginners learning Linux firewall administration.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain what UFW is.
2. Check whether UFW is installed.
3. Inspect the current UFW status.
4. Understand UFW default policies.
5. Understand how allow and deny rules work.
6. Inspect application profiles.
7. Understand how firewall rules are managed.
8. Explain the risks of enabling a firewall remotely.

## 3. Understanding UFW

UFW provides a simplified interface for firewall administration.

Its main command is:

```bash
ufw
```

To display available commands:

```bash
ufw --help
```

To display the installed version:

```bash
ufw version
```

To inspect firewall status:

```bash
sudo ufw status
```

The status command may return:

```text
Status: inactive
```

or:

```text
Status: active
```

These results indicate whether UFW is currently enabled.

An inactive UFW does not automatically mean the machine has no firewall protection. Other firewall mechanisms may still be configured.

## 4. Understanding UFW Default Policies

UFW supports default policies that determine how traffic is handled when no specific rule matches.

Common policies include:

| Policy | Meaning |
|---|---|
| allow | Permit matching traffic |
| deny | Block matching traffic |
| reject | Block traffic and send a rejection response |

A common server configuration uses:

- Deny incoming traffic by default.
- Allow outgoing traffic by default.
- Allow only required incoming services.

This is an example of the principle of least privilege.

However, never apply restrictive default policies blindly. Doing so can interrupt SSH or other essential connections.

## 5. Important UFW Commands

The following commands are important for firewall administration.

### Check status

```bash
sudo ufw status
```

### Check detailed status

```bash
sudo ufw status verbose
```

### Display numbered rules

```bash
sudo ufw status numbered
```

### Display application profiles

```bash
sudo ufw app list
```

### Display a specific application profile

```bash
sudo ufw app info OpenSSH
```

The profile may not exist on every installation.

### Display help

```bash
ufw --help
```

These commands help administrators understand the existing configuration before making changes.

## 6. Understanding UFW Allow Rules

An allow rule permits specified network communication.

For example:

```bash
sudo ufw allow 80/tcp
```

This permits incoming TCP traffic on port 80.

Another example:

```bash
sudo ufw allow 443/tcp
```

This permits incoming HTTPS traffic.

To allow SSH using an application profile:

```bash
sudo ufw allow OpenSSH
```

Important: These are configuration examples only. Do not execute them in this inspection-only lab.

## 7. Understanding UFW Deny Rules

A deny rule blocks specified traffic.

For example:

```bash
sudo ufw deny 23/tcp
```

This blocks incoming TCP traffic on port 23.

Telnet commonly uses port 23 and does not provide the same encryption protections as SSH.

Another example:

```bash
sudo ufw deny 8080/tcp
```

This blocks incoming TCP traffic on port 8080.

Again, these are examples for understanding firewall syntax. Do not execute them yet.

## 8. Understanding UFW Delete and Reset

UFW supports commands for deleting rules and resetting its configuration.

Example:

```bash
sudo ufw delete 1
```

This deletes the rule at position 1 in the numbered rules list.

Another example:

```bash
sudo ufw reset
```

This resets UFW configuration.

**Warning:** These commands modify firewall settings. Never use them casually, especially on systems where network access is important.

We will not use either command in this lab.

## 9. Practical Exercise 1: Check UFW Installation

Open your Kali Linux terminal.

Run:

```bash
command -v ufw
```

Then:

```bash
ufw version
```

Record whether UFW is installed.

If the command is unavailable, record that result. Do not install anything during this exercise.

## 10. Practical Exercise 2: Inspect UFW Status

Run:

```bash
sudo ufw status
```

Then:

```bash
sudo ufw status verbose
```

Finally:

```bash
sudo ufw status numbered
```

Record:

1. Whether UFW is active or inactive.
2. The default incoming policy, if displayed.
3. The default outgoing policy, if displayed.
4. Any existing firewall rules.

If UFW is inactive, do not enable it yet.

## 11. Practical Exercise 3: Inspect Application Profiles

Run:

```bash
sudo ufw app list
```

If an OpenSSH profile is available, inspect it:

```bash
sudo ufw app info OpenSSH
```

Record:

- Available application profiles.
- The profile name.
- The port or ports associated with the profile.

If no profiles are displayed, record that observation.

## 12. Practical Exercise 4: Understand Firewall Rule Syntax

Study these examples without executing them:

**Allow a web server:**

```bash
sudo ufw allow 80/tcp
```

**Allow encrypted web traffic:**

```bash
sudo ufw allow 443/tcp
```

**Allow SSH:**

```bash
sudo ufw allow 22/tcp
```

**Block Telnet:**

```bash
sudo ufw deny 23/tcp
```

**Inspect numbered rules:**

```bash
sudo ufw status numbered
```

For each example, identify:

- The action.
- The port number.
- The protocol.
- The expected effect.

## 13. Practical Exercise 5: Create Your Lab Notes

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module07/lab03
```

Enter it:

```bash
cd ~/practical-labs/module07/lab03
```

Create the notes file:

```bash
nano ufw-observations.txt
```

Use this template:

```text
MODULE 07 - LAB 03
UFW FIREWALL MANAGEMENT

UFW Installed:
UFW Version:

UFW Status:

Default Incoming Policy:
Default Outgoing Policy:

Existing Firewall Rules:

Available Application Profiles:

OpenSSH Profile Information:

RULE SYNTAX ANALYSIS

1. Allow HTTP:
2. Allow HTTPS:
3. Allow SSH:
4. Deny Telnet:

Security Observations:
```

Save using:

- CTRL + O
- ENTER
- CTRL + X

## 14. Important Security Considerations

Before enabling or changing UFW, administrators should:

1. Inspect existing firewall rules.
2. Identify active network connections.
3. Identify essential services.
4. Confirm console access or another recovery method.
5. Prepare a rollback procedure.
6. Test changes in a controlled environment.

For example, enabling a default-deny incoming policy without first allowing the required SSH connection can lock a remote administrator out of the machine.

Never expose unnecessary services to the public internet.

## 15. Knowledge Check

1. What does UFW stand for?
2. What is the purpose of UFW?
3. What command displays the UFW status?
4. What is the difference between allow and deny?
5. What does a default-deny incoming policy mean?
6. What is an application profile?
7. What does `sudo ufw status numbered` display?
8. Why can enabling UFW remotely be risky?
9. What does `sudo ufw reset` do?
10. Why should firewall configurations be inspected before modification?

## 16. Lab Completion Checklist

- [ ] Checked UFW installation.
- [ ] Inspected UFW version.
- [ ] Inspected UFW status.
- [ ] Checked default firewall policies.
- [ ] Inspected numbered rules.
- [ ] Checked available application profiles.
- [ ] Studied allow and deny syntax.
- [ ] Created `ufw-observations.txt`.
- [ ] Completed the knowledge check.

## 17. Expected Outcome

You should now understand how UFW simplifies Linux firewall management and how administrators inspect existing rules before making configuration changes.

**Safety reminder:** This lab is inspection-only. Do not enable, disable, reset, or modify UFW settings. We will use a controlled configuration exercise later.
