---
---

# Module 10 – Lesson 10: Network Troubleshooting Commands

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand network troubleshooting.
2. Identify common Linux network problems.
3. Inspect network interfaces and IP configuration.
4. Diagnose gateway and routing problems.
5. Investigate DNS failures.
6. Test connectivity and network services.
7. Trace network paths.
8. Follow a systematic troubleshooting process.

---

## 2. What Is Network Troubleshooting?

Network troubleshooting is the process of identifying, analyzing, and resolving problems that prevent devices from communicating correctly.

Network problems can occur because of:

- Incorrect IP configuration.
- Disabled network interfaces.
- Gateway failures.
- DNS problems.
- Firewall restrictions.
- Routing errors.
- Faulty network hardware.
- Unavailable remote services.

### Real-World Example

Imagine your Kali Linux VM cannot access a website.

Instead of immediately changing random settings, investigate the problem systematically.

Ask:

1. Is the network interface active?
2. Does the VM have an IP address?
3. Is a default gateway configured?
4. Can the VM reach its gateway?
5. Can it reach an external IP address?
6. Can it resolve domain names?
7. Is the destination service available?

This approach helps isolate the actual problem.

---

## 3. The Network Troubleshooting Process

A good troubleshooting process follows a logical sequence.

### Step 1: Identify the Problem

Understand what is failing.

Example:

```text
Kali Linux cannot access websites.
```

### Step 2: Inspect the Network Interface

```bash
ip link
```

Check whether the interface is enabled.

### Step 3: Inspect IP Configuration

```bash
ip addr
```

Check whether the interface has an appropriate IP address.

### Step 4: Inspect the Routing Table

```bash
ip route
```

Check whether a default route exists when external connectivity is expected.

### Step 5: Test Local Connectivity

```bash
ping -c 4 127.0.0.1
```

### Step 6: Test Gateway Connectivity

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Replace the example placeholder with your actual gateway.

### Step 7: Test External IP Connectivity

```bash
ping -c 4 1.1.1.1
```

Remember that some networks block ICMP.

### Step 8: Test DNS Resolution

```bash
getent hosts example.com
```

### Step 9: Inspect Network Services

```bash
ss -tuln
```

### Step 10: Analyze the Results

Use the evidence to identify the likely cause before changing configuration.

---

## 4. Important Linux Network Troubleshooting Commands

### 4.1 `ip addr`

Displays IP address configuration.

```bash
ip addr
```

Useful for checking:

- Interface IP addresses.
- IPv4 addresses.
- IPv6 addresses.
- Interface states.

### 4.2 `ip link`

Displays network interface information.

```bash
ip link
```

Useful for checking:

- Interface names.
- Interface states.
- MAC addresses.

### 4.3 `ip route`

Displays routing information.

```bash
ip route
```

Useful for checking:

- Default gateway.
- Network routes.
- Outgoing interfaces.

### 4.4 `ping`

Tests IP connectivity.

```bash
ping -c 4 1.1.1.1
```

Useful for checking whether an IP destination responds to ICMP.

### 4.5 `traceroute`

Attempts to identify the routers along a network path.

```bash
traceroute example.com
```

Useful for investigating possible routing-path problems.

### 4.6 `ss`

Displays socket and connection information.

```bash
ss -tuln
```

Useful for checking listening TCP and UDP sockets.

### 4.7 `getent`

Tests hostname resolution through the system's configured name-service mechanisms.

```bash
getent hosts example.com
```

### 4.8 `dig`

Performs DNS queries.

```bash
dig example.com
```

### 4.9 `curl`

Transfers data using supported network protocols.

Test an HTTPS endpoint:

```bash
curl -I https://example.com
```

The `-I` option requests response headers.

This can help distinguish a DNS or ICMP issue from an HTTP service problem.

### 4.10 `ip neigh`

Displays neighbour information.

```bash
ip neigh
```

Useful for inspecting local address-resolution information.

---

## 5. Understanding Common Network Problems

### 5.1 No IP Address

Symptoms:

- The interface exists but has no expected IPv4 address.
- Network communication may fail.

Inspect:

```bash
ip addr
```

Possible causes:

- DHCP server unavailable.
- DHCP request failed.
- Interface disconnected.
- Incorrect network configuration.

### 5.2 Interface Is Down

Inspect:

```bash
ip link
```

A disabled interface may appear with a `DOWN` state.

Do not change the state of your active VMware interface during this lab.

### 5.3 No Default Gateway

Inspect:

```bash
ip route
```

If external connectivity is expected but no usable default route exists, the system may not know where to send traffic for remote networks.

### 5.4 Gateway Is Unreachable

Test:

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Possible causes:

- Incorrect gateway address.
- VM network configuration problem.
- Gateway unavailable.
- Local network issue.

ICMP filtering can also cause a failed ping even when the gateway is functioning.

### 5.5 DNS Failure

Test IP connectivity:

```bash
ping -c 4 1.1.1.1
```

Then test DNS:

```bash
getent hosts example.com
```

If IP connectivity works but hostname resolution fails, investigate DNS configuration.

### 5.6 Service Unavailable

A destination may be reachable while a particular service is unavailable.

Test a TCP service:

```bash
nc -vz example.com 443
```

A failed connection may indicate:

- Service unavailable.
- Firewall filtering.
- Incorrect port.
- Network restrictions.

---

## 6. Understanding `traceroute` and `tracepath`

### Traceroute

`traceroute` attempts to discover the routers between your system and a destination.

```bash
traceroute example.com
```

It commonly uses packets with progressively increasing TTL values.

Routers may return ICMP Time Exceeded messages.

### Tracepath

`tracepath` is another Linux utility for investigating network paths.

```bash
tracepath example.com
```

It can also help investigate path MTU information.

### Important

Some routers intentionally do not respond to diagnostic probes.

Therefore, a missing hop does not necessarily mean that traffic cannot pass through that router.

---

## 7. Understanding Network Configuration Files

Linux networking may depend on several configuration files and services.

### 7.1 DNS Resolver Configuration

```bash
cat /etc/resolv.conf
```

Displays resolver configuration.

### 7.2 Hosts File

```bash
cat /etc/hosts
```

Displays local hostname mappings.

### 7.3 Name Service Switch Configuration

```bash
grep '^hosts:' /etc/nsswitch.conf
```

Shows the configured order for hostname lookup sources.

### 7.4 NetworkManager Status

If NetworkManager is installed:

```bash
systemctl status NetworkManager
```

This checks the status of the NetworkManager service.

To inspect its device information:

```bash
nmcli device status
```

To inspect active connections:

```bash
nmcli connection show --active
```

These commands are useful when NetworkManager manages the Kali VM's networking.

---

## 8. VMware Networking Troubleshooting

Your Kali Linux system runs inside VMware, so the virtual network configuration is also important.

### Common VMware Network Modes

| Mode | Description |
|---|---|
| NAT | VM shares the host's external network connection through VMware |
| Bridged | VM connects through the physical network as a separate device |
| Host-only | VM communicates through an isolated virtual network with the host and other connected VMs |

### If Kali Uses NAT

Check:

1. The VM's virtual network adapter is connected.
2. The adapter is configured for NAT.
3. Kali has an appropriate IP address.
4. A default route exists.
5. DNS resolution works.

### If Kali Uses Host-only

Remember:

- Host-only networking normally isolates the VM from the external internet.
- Communication with the host and other VMs may still work.
- Lack of internet access may be expected.

Do not mistake intentional isolation for a fault.

---

## 9. A Systematic Troubleshooting Decision Tree

```text
Start
  |
  v
Check interface with ip link
  |
  v
Check IP address with ip addr
  |
  v
Check route with ip route
  |
  v
Test gateway connectivity
  |
  v
Test external IP connectivity
  |
  v
Test DNS resolution
  |
  v
Test application/service
  |
  v
Identify and document the problem
```

The key principle is to test from the local system outward.

---

## 10. Practical Lab: Diagnosing Your Kali Network

### Lab Objective

Use Linux commands to investigate your Kali VM's network configuration.

This is a read-only investigation. Do not disable interfaces, delete routes, or overwrite network configuration.

### Step 1: Identify Network Interfaces

```bash
ip -br link
```

Record the interface names and states.

### Step 2: Inspect IP Addresses

```bash
ip -br addr
```

Record the IPv4 and IPv6 addresses.

### Step 3: Inspect Routing

```bash
ip route
```

Record the default gateway if present.

### Step 4: Inspect Neighbour Entries

```bash
ip neigh
```

Record any visible neighbour states.

### Step 5: Test Loopback

```bash
ping -c 4 127.0.0.1
```

Record the result.

### Step 6: Test Your Gateway

```bash
ping -c 4 YOUR_GATEWAY_IP
```

Replace the placeholder with the gateway discovered in Step 3.

If no gateway exists, record that fact instead of running the command.

### Step 7: Test an External IP

```bash
ping -c 4 1.1.1.1
```

Record the packet loss and response time, if available.

### Step 8: Test DNS Resolution

```bash
getent hosts example.com
```

Record the result.

### Step 9: Test an HTTPS Service

```bash
curl -I --max-time 10 https://example.com
```

Observe the HTTP response status if one is returned.

### Step 10: Inspect NetworkManager

```bash
nmcli device status
```

If NetworkManager is not installed or running, record that instead of trying to modify services.

### Step 11: Inspect Listening Sockets

```bash
ss -tuln
```

Record any listening ports.

### Step 12: Prepare Your Troubleshooting Report

Summarize what worked, what failed, and what you learned.

---

## 11. Network Troubleshooting Report Template

# Kali Linux Network Troubleshooting Report

**System:** Kali Linux VM

**Environment:** VMware

**Date:** __________________

### 1. Interface Information

- Interface name:
- Interface state:
- MAC address:

### 2. IP Configuration

- IPv4 address:
- IPv6 address:
- Prefix:

### 3. Routing Information

- Default gateway:
- Default route available: Yes / No

### 4. Connectivity Tests

- Loopback test:
- Gateway test:
- External IP test:
- DNS test:
- HTTPS test:

### 5. Identified Problems

Write any problems discovered during the investigation.

### 6. Possible Causes

Explain what may have caused each problem.

### 7. Conclusion

Summarize the overall network condition.

---

## 12. Assessment Questions

Answer the following:

1. What is network troubleshooting?
2. Why should you inspect network interfaces before changing configuration?
3. Which command displays IP addresses?
4. Which command displays the routing table?
5. How can you investigate DNS resolution problems?
6. What is the purpose of traceroute?
7. Why might ping fail while HTTPS works?
8. What is the difference between VMware NAT and host-only networking?
9. Which command checks NetworkManager device status?
10. Why should you troubleshoot systematically instead of changing random network settings?

---

## 13. Important Commands to Remember

```bash
ip link
ip addr
ip -br addr
ip route
ip neigh

ping -c 4 127.0.0.1
ping -c 4 1.1.1.1

traceroute example.com
tracepath example.com

getent hosts example.com
dig example.com

ss -tuln
nc -vz example.com 443
curl -I https://example.com

cat /etc/resolv.conf
cat /etc/hosts

systemctl status NetworkManager
nmcli device status
nmcli connection show --active
```

---

## 14. Lesson Summary

In this lesson, you learned:

- Network troubleshooting identifies and resolves connectivity problems.
- `ip link` and `ip addr` help inspect interfaces and addresses.
- `ip route` helps identify routing problems.
- `ping` tests ICMP reachability.
- `dig` and `getent` help investigate DNS.
- `traceroute` and `tracepath` help examine network paths.
- `curl` tests application-layer connectivity.
- `ss` inspects network sockets.
- NetworkManager tools help inspect managed network connections.
- VMware network mode affects the connectivity available to a VM.
- Systematic troubleshooting reduces unnecessary configuration changes.

### Key Definitions

**Network Troubleshooting:** The systematic process of identifying and resolving network problems.

**Traceroute:** A diagnostic utility that attempts to discover intermediate routers on a network path.

**NetworkManager:** A Linux service that manages network connections and devices.

**DHCP:** Dynamic Host Configuration Protocol, which automatically provides network configuration to clients.

**Connectivity:** The ability of systems or applications to communicate over a network.

**Diagnostic Tool:** A utility used to investigate system or network behaviour.

---

## 15. Knowledge Check

1. Which command displays network interfaces?
2. Which command identifies the default gateway?
3. What should you check if IP connectivity works but DNS fails?
4. What is the purpose of `curl -I`?
5. Why might a host-only VM have no internet access?
6. What is the purpose of `nmcli device status`?
7. What does a missing traceroute hop necessarily prove? 
8. Why is a systematic troubleshooting process important?

**End of Lesson 10 – Network Troubleshooting Commands**