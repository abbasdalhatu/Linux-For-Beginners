
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 08: FIREWALL TESTING AND TROUBLESHOOTING

## 1. Introduction

Firewall testing is the process of checking whether firewall rules behave as expected.

A firewall may be correctly installed but incorrectly configured.

For example:

- A web server may be unreachable because port 443 is blocked.
- SSH access may fail because the firewall does not permit TCP port 22.
- A legitimate DNS request may fail because outbound DNS traffic is restricted.
- An unnecessary service may remain accessible because the firewall rule is too permissive.

Firewall troubleshooting helps administrators identify and resolve these problems.

In this lab, you will learn how to diagnose firewall-related connectivity problems without changing your active Kali firewall configuration.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain firewall testing.
2. Understand common connectivity problems.
3. Inspect network interfaces and routes.
4. Test network connectivity.
5. Inspect listening services.
6. Identify possible firewall-related failures.
7. Understand firewall troubleshooting tools.
8. Create a structured troubleshooting report.

## 3. Why Firewall Testing Is Important

Firewall testing helps confirm that:

- Required services are accessible.
- Unauthorized traffic is blocked.
- Firewall rules match security requirements.
- Legitimate applications can communicate.
- Network services are not unnecessarily exposed.

A firewall should be tested after configuration changes and periodically during security reviews.

Testing should be performed only on systems and networks you own or have permission to assess.

## 4. Common Firewall Problems

### A. Required Port Is Blocked

A service is running, but a firewall rule prevents clients from connecting.

Example:

A web server listens on TCP port 443, but the firewall blocks incoming connections.

### B. Incorrect Source Address

A rule permits the wrong source IP address.

The intended administrator cannot connect.

### C. Incorrect Protocol

A rule allows TCP traffic when the application requires UDP, or vice versa.

### D. Incorrect Default Policy

A restrictive default policy blocks legitimate communication.

### E. Service Is Not Running

A firewall is not always responsible for a failed connection.

The actual service may not be listening.

### F. Routing Problems

Traffic may be unable to reach its destination because of an incorrect route or gateway.

This is a networking problem rather than necessarily a firewall problem.

## 5. The Firewall Troubleshooting Method

Use this sequence when investigating a connectivity problem.

**Step 1: Confirm the problem**

Identify the affected service, destination address, port, and protocol.

**Step 2: Inspect the network interface**

Confirm that the machine has an active network interface and valid IP configuration.

**Step 3: Inspect routing**

Check whether the machine has an appropriate route to the destination.

**Step 4: Inspect the service**

Determine whether the expected application is running and listening.

**Step 5: Inspect firewall rules**

Check whether a firewall rule may block the required traffic.

**Step 6: Inspect logs**

Look for relevant blocked or rejected traffic.

**Step 7: Document findings**

Record the evidence, probable cause, and recommended corrective action.

## 6. Important Troubleshooting Tools

| Tool | Purpose |
|---|---|
| ip | Inspect interfaces and routing |
| ping | Test basic IP reachability |
| ss | Inspect sockets and listening ports |
| curl | Test application-level HTTP communication |
| nc | Test TCP or UDP connectivity |
| traceroute | Inspect network paths |
| tcpdump | Inspect packets |
| journalctl | Inspect system logs |
| nft | Inspect nftables configuration |
| ufw | Inspect UFW configuration |

Some tools may not be installed. Do not install additional software just for this inspection-only lab.

## 7. Practical Exercise 1: Inspect Network Configuration

Open your Kali terminal.

Run:

```bash
ip -br addr
```

This displays network interfaces and assigned addresses.

Next:

```bash
ip route
```

Inspect the default route.

Then:

```bash
ip link
```

Inspect interface states.

Record:

- Active interface.
- IP address.
- Interface state.
- Default gateway.

A network interface can be UP without having a working connection to its destination. Consider the full network configuration.

## 8. Practical Exercise 2: Test Basic Connectivity

First, check whether your default gateway is reachable.

Identify the gateway:

```bash
ip route
```

Then, if an address is displayed, test it:

```bash
ping -c 4 GATEWAY_IP
```

Replace `GATEWAY_IP` with the actual gateway address.

Next, test an approved external connectivity destination if your network policy permits it:

```bash
ping -c 4 1.1.1.1
```

A failed ping does not automatically mean the firewall is blocking traffic. ICMP may be filtered by the destination or network.

Stop here if your environment does not permit external connectivity tests.

## 9. Practical Exercise 3: Inspect DNS Resolution

Run:

```bash
getent hosts example.com
```

This checks whether the system can resolve the domain name.

You can also inspect DNS configuration:

```bash
cat /etc/resolv.conf
```

Record:

- Whether the hostname resolves.
- The returned IP address, if any.
- The configured DNS information.

If name resolution fails, possible causes include:

- DNS server unavailability.
- Incorrect DNS configuration.
- Network connectivity problems.
- DNS filtering.

Do not conclude that the firewall is responsible without additional evidence.

## 10. Practical Exercise 4: Inspect Listening Services

Run:

```bash
sudo ss -tulpn
```

Identify any listening TCP and UDP services.

For each service, record:

- Local address.
- Port.
- Protocol.
- Process name, if available.

A service listening only on `127.0.0.1` is normally bound to the local loopback interface.

A service listening on `0.0.0.0` is bound to all IPv4 interfaces.

A service listening on `::` may be bound to IPv6 interfaces and, depending on system configuration, may also accept IPv4-mapped connections.

Do not assume that listening on all interfaces means the service is publicly reachable.

## 11. Practical Exercise 5: Inspect Firewall Configuration

Inspect UFW:

```bash
sudo ufw status verbose
```

Inspect nftables:

```bash
sudo nft list ruleset
```

Inspect iptables:

```bash
sudo iptables -L -n -v
```

Look for:

- Default policies.
- Accept rules.
- Drop rules.
- Reject rules.
- Interface-specific rules.
- Source or destination restrictions.

Record the findings.

Do not modify existing rules.

## 12. Practical Exercise 6: Inspect Logs for Possible Problems

Inspect recent system logs:

```bash
sudo journalctl -n 50 --no-pager
```

Search for firewall-related messages:

```bash
sudo journalctl --no-pager | grep -iE 'ufw|firewall|drop|reject'
```

If no matching messages are found, record that result.

Remember that the absence of firewall log messages does not prove that no traffic was blocked.

Logging may be disabled, incomplete, or handled by another component.

## 13. Practical Exercise 7: Simulated Troubleshooting Scenarios

Analyze these situations.

### Scenario A: SSH Connection Failure

A system administrator cannot connect to a Linux server using SSH.

Evidence:

- The server has a valid IP address.
- The SSH service is listening on TCP port 22.
- A firewall rule blocks incoming TCP port 22.

Questions:

1. What is the likely cause?
2. Which firewall component should be inspected?
3. What corrective action could an administrator consider?
4. How should the change be tested?

### Scenario B: Website Unavailable

A web server is listening on TCP port 443, but clients cannot access it.

Evidence:

- The server interface is active.
- The service is listening.
- The firewall has no matching allow rule.
- The default incoming policy is deny.

Questions:

1. What is the likely cause?
2. What evidence supports your conclusion?
3. What rule might be required?
4. How would you validate the correction?

### Scenario C: DNS Resolution Failure

A computer can reach its network gateway but cannot resolve domain names.

Possible causes:

- Incorrect DNS server configuration.
- DNS server unavailable.
- Outbound DNS traffic blocked.
- DNS service failure.

Questions:

1. Which command can test name resolution?
2. Which file may contain DNS configuration?
3. How can you distinguish DNS failure from general connectivity failure?
4. What additional evidence would you collect?

### Scenario D: Unexpected Open Port

A server has a listening service on TCP port 3306.

The organization does not expect a database service on this machine.

Questions:

1. Which command can identify the process?
2. Why could this be a security concern?
3. What information should be collected before changing the service?
4. Why should you avoid stopping it immediately?

## 14. Practical Exercise 8: Create a Troubleshooting Report

Create your lab directory:

```bash
mkdir -p ~/practical-labs/module07/lab08
```

Enter it:

```bash
cd ~/practical-labs/module07/lab08
```

Create the report:

```bash
nano firewall-troubleshooting-report.txt
```

Use this template:

```text
MODULE 07 - LAB 08
FIREWALL TESTING AND TROUBLESHOOTING

1. NETWORK CONFIGURATION

Interface:
IP Address:
Default Gateway:
Interface State:

2. CONNECTIVITY TESTS

Gateway Test:
External Connectivity Test:

3. DNS TEST

Hostname Resolution:
DNS Configuration:

4. LISTENING SERVICES

Service 1:
Port:
Protocol:

Service 2:
Port:
Protocol:

5. FIREWALL INSPECTION

UFW:
NFTABLES:
IPTABLES:

6. LOG INSPECTION

Relevant Events:
Observations:

7. SCENARIO ANALYSIS

SSH FAILURE:
WEB SERVER FAILURE:
DNS FAILURE:
UNEXPECTED OPEN PORT:

8. FINAL FINDINGS

Possible Problems:
Supporting Evidence:
Recommended Actions:
```

Save using:

- CTRL + O
- ENTER
- CTRL + X

## 15. Troubleshooting Best Practices

1. Identify the actual problem before changing anything.
2. Check the service before blaming the firewall.
3. Verify routing and DNS.
4. Inspect firewall rules.
5. Review relevant logs.
6. Make one controlled change at a time.
7. Maintain a rollback plan.
8. Validate connectivity after changes.
9. Document evidence.
10. Avoid disabling the firewall as a troubleshooting shortcut.

## 16. Knowledge Check

1. What is firewall testing?
2. Why might a listening service still be unreachable?
3. What is the purpose of `ss -tulpn`?
4. What does `ip route` display?
5. What does `getent hosts` help investigate?
6. Why does a failed ping not prove firewall interference?
7. What is the purpose of journalctl?
8. Why should administrators inspect logs before changing rules?
9. What is a rollback plan?
10. Why should firewall changes be tested one at a time?

## 17. Lab Completion Checklist

- [ ] Inspected network interfaces.
- [ ] Inspected routing information.
- [ ] Tested permitted basic connectivity.
- [ ] Inspected DNS resolution.
- [ ] Inspected listening services.
- [ ] Inspected UFW, nftables, and iptables.
- [ ] Reviewed relevant system logs.
- [ ] Analyzed four troubleshooting scenarios.
- [ ] Created `firewall-troubleshooting-report.txt`.
- [ ] Completed the knowledge check.

## 18. Expected Outcome

You should now understand how to investigate firewall-related connectivity problems using a structured troubleshooting process.

You should also understand why a failed connection can have several possible causes, including routing, DNS, service configuration, and firewall policies.

**Safety reminder:** This lab is inspection-only. Do not modify firewall settings, stop services, or disable network protections.
