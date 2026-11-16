
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 04: FIREWALL RULES AND POLICIES

## 1. Introduction

Firewall rules are instructions that determine how network traffic should be handled.

A firewall administrator creates rules to control which connections are allowed, rejected, or dropped.

For example, a web server may need to accept HTTP and HTTPS traffic while blocking unnecessary services.

A firewall policy defines the general behavior applied to traffic.

In this lab, we will study how firewall rules are structured, how rules are evaluated, and how administrators design secure firewall policies.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain firewall rules and policies.
2. Identify the components of a firewall rule.
3. Understand ACCEPT, DROP, and REJECT.
4. Explain rule ordering and evaluation.
5. Understand default policies.
6. Recognize common firewall configurations.
7. Inspect existing firewall rules safely.
8. Design a basic firewall policy.

## 3. Anatomy of a Firewall Rule

A firewall rule typically contains several components.

| Component | Description |
|---|---|
| Source IP | Where traffic originates |
| Destination IP | Where traffic is going |
| Protocol | TCP, UDP, ICMP, etc. |
| Source port | Port used by the sender |
| Destination port | Port used by the destination service |
| Interface | Network interface involved |
| Action | What the firewall should do |

Example:

```text
Source: 192.168.56.10
Destination: 192.168.56.20
Protocol: TCP
Destination Port: 22
Action: ACCEPT
```

This rule describes allowing an SSH connection from one specific machine to another.

The example is illustrative. Do not apply it to your current firewall.

## 4. Understanding Firewall Actions

Three common firewall actions are ACCEPT, DROP, and REJECT.

### A. ACCEPT

Allows matching traffic to continue through the networking stack.

Example:

```text
TCP destination port 443 → ACCEPT
```

This permits matching HTTPS traffic.

### B. DROP

Silently discards matching packets.

The sender generally receives no explicit rejection response from the firewall.

Example:

```text
TCP destination port 23 → DROP
```

### C. REJECT

Blocks matching traffic and sends an appropriate rejection response.

Example:

```text
TCP destination port 23 → REJECT
```

### Comparison

| Action | Behavior |
|---|---|
| ACCEPT | Permits traffic |
| DROP | Silently discards traffic |
| REJECT | Blocks traffic and responds with rejection |

Security administrators choose actions according to operational and security requirements.

## 5. Understanding Rule Ordering

Firewall rules may be evaluated in sequence.

For a simple ordered ruleset, the first matching rule can determine what happens to a packet.

Consider this illustrative ruleset:

| Order | Rule | Action |
|---|---|---|
| 1 | Allow TCP port 443 | ACCEPT |
| 2 | Allow TCP port 80 | ACCEPT |
| 3 | Block TCP port 23 | DROP |
| 4 | Other incoming traffic | DROP |

If a packet matches rule 1, it is accepted.

If it does not match rule 1, evaluation continues.

**Important:** Actual rule evaluation depends on the firewall framework, chain configuration, jump behavior, and policy. nftables and iptables support more complex control flow than a simple linear list.

## 6. Understanding Default Policies

A default policy determines what happens when traffic reaches the end of a chain without matching a terminating rule.

Two common approaches are:

### Default Allow

Most traffic is permitted unless a rule explicitly blocks it.

### Default Deny

Most traffic is blocked unless a rule explicitly permits it.

For many security-sensitive systems, default deny is a useful approach for inbound traffic.

However, a firewall must also permit legitimate services and required communication.

An incorrectly designed policy can interrupt essential operations.

## 7. Stateful Firewall Rules

A stateful firewall tracks connection information.

It can identify whether packets belong to:

- NEW connections.
- ESTABLISHED connections.
- RELATED connections.
- INVALID connections.

Example:

A computer initiates an HTTPS connection to a website.

The firewall recognizes the outgoing connection and can permit the corresponding response packets.

This is more intelligent than treating every packet as an unrelated event.

## 8. Practical Exercise 1: Inspect Existing Firewall Rules

Open Kali Linux.

Inspect UFW:

```bash
sudo ufw status numbered
```

Inspect nftables:

```bash
sudo nft list ruleset
```

Inspect iptables:

```bash
sudo iptables -L -n -v --line-numbers
```

Record any existing rules.

If no rules are present, record that observation.

Do not add, delete, or modify rules.

## 9. Practical Exercise 2: Study Common Firewall Rules

Study these examples without executing them.

### Allow HTTPS

```bash
sudo ufw allow 443/tcp
```

Meaning:

- Action: Allow.
- Destination port: 443.
- Protocol: TCP.
- Purpose: Permit incoming HTTPS traffic.

### Block Telnet

```bash
sudo ufw deny 23/tcp
```

Meaning:

- Action: Deny.
- Destination port: 23.
- Protocol: TCP.
- Purpose: Block incoming Telnet traffic.

### Allow DNS

```bash
sudo ufw allow 53/udp
```

Meaning:

- Action: Allow.
- Destination port: 53.
- Protocol: UDP.
- Purpose: Permit matching incoming DNS traffic.

Note that DNS can also use TCP port 53. Real firewall rules must account for the actual DNS service architecture and direction of communication.

## 10. Practical Exercise 3: Design a Firewall Policy

Imagine you are responsible for a Linux web server.

The server provides:

- SSH administration.
- HTTP web access.
- HTTPS web access.

Your task is to design a policy.

Use this table:

| Traffic | Proposed Action | Reason |
|---|---|---|
| SSH TCP 22 | Allow from trusted admin addresses | Remote administration |
| HTTP TCP 80 | Allow if required | Web access |
| HTTPS TCP 443 | Allow | Secure web access |
| Telnet TCP 23 | Deny | Unnecessary insecure service |
| Other unnecessary inbound traffic | Deny | Reduce attack surface |

Remember that SSH should not automatically be exposed to every source address.

Also consider whether HTTP is required or whether the server should redirect users to HTTPS.

Do not implement this policy on your current VM.

## 11. Practical Exercise 4: Understand Rule Direction

Classify these examples.

1. A browser requests a website from the internet.
2. A remote administrator connects to your SSH server.
3. A Linux machine sends a DNS query.
4. A remote computer accesses a local web server.
5. A Linux router forwards a packet between two networks.

For each example, identify:

- Traffic direction.
- Relevant firewall chain or hook.
- A possible security consideration.

Remember:

- INPUT handles traffic destined for the local system.
- OUTPUT handles traffic generated by the local system.
- FORWARD handles traffic passing through the system.

## 12. Practical Exercise 5: Create Your Lab Notes

Create your working directory:

```bash
mkdir -p ~/practical-labs/module07/lab04
```

Enter it:

```bash
cd ~/practical-labs/module07/lab04
```

Create your notes file:

```bash
nano firewall-policy-design.txt
```

Use this template:

```text
MODULE 07 - LAB 04
FIREWALL RULES AND POLICIES

1. FIREWALL ACTIONS

ACCEPT:
DROP:
REJECT:

2. DEFAULT POLICIES

DEFAULT ALLOW:
DEFAULT DENY:

3. EXISTING FIREWALL OBSERVATIONS

UFW:
NFTABLES:
IPTABLES:

4. WEB SERVER POLICY

SSH:
HTTP:
HTTPS:
TELNET:
OTHER INBOUND TRAFFIC:

5. TRAFFIC DIRECTION

BROWSER REQUEST:
SSH CONNECTION:
DNS REQUEST:
WEB SERVER REQUEST:
ROUTER FORWARDING:

6. SECURITY OBSERVATIONS
```

Save the file:

- CTRL + O
- ENTER
- CTRL + X

## 13. Security Best Practices

When designing firewall policies:

1. Apply least privilege.
2. Allow only necessary services.
3. Restrict administrative access.
4. Prefer encrypted protocols.
5. Document every rule.
6. Review rules periodically.
7. Monitor firewall logs.
8. Test configurations before deployment.
9. Maintain a rollback plan.
10. Avoid unnecessary public exposure.

## 14. Knowledge Check

1. What is a firewall rule?
2. What are the main components of a firewall rule?
3. Explain ACCEPT, DROP, and REJECT.
4. What is rule ordering?
5. What is a default-deny policy?
6. What is the difference between stateful and stateless filtering?
7. What is the purpose of the INPUT chain?
8. Why should SSH access be restricted?
9. Why should firewall rules be documented?
10. Why is testing important before deploying firewall policies?

## 15. Lab Completion Checklist

- [ ] Understood firewall rules.
- [ ] Learned ACCEPT, DROP, and REJECT.
- [ ] Studied rule ordering.
- [ ] Understood default policies.
- [ ] Learned stateful firewall concepts.
- [ ] Inspected existing firewall configurations.
- [ ] Designed a basic web server firewall policy.
- [ ] Created `firewall-policy-design.txt`.
- [ ] Completed the knowledge check.

## 16. Expected Outcome

You should now understand how firewall rules are structured, how policies control network traffic, and how administrators design rules that balance security and operational requirements.

**Safety reminder:** This lab is focused on inspection and policy design. Do not apply firewall changes to your active Kali VM.
