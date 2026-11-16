
# LINUX NETWORK TROUBLESHOOTING

## 1. What Is Network Troubleshooting?

Network troubleshooting is the process of identifying, diagnosing, and fixing problems that prevent devices from communicating.

Common problems include:
- No internet connection.
- Incorrect IP address.
- DNS resolution failure.
- Gateway problems.
- Packet loss.
- Firewall restrictions.

## 2. Step One: Check Network Interfaces

Display network interfaces:

```bash
ip link
```

Display IP addresses:

```bash
ip addr
```

Show a short interface summary:

```bash
ip -br addr
```

Check whether the interface is UP or DOWN.

## 3. Step Two: Check the IP Configuration

Display the routing table:

```bash
ip route
```

Check the default gateway:

```bash
ip route show default
```

A device generally needs a valid IP configuration to communicate with other network devices.

## 4. Step Three: Test Connectivity

Test your local network interface:

```bash
ping -c 4 127.0.0.1
```

Test your default gateway:

```bash
ping -c 4 192.168.1.1
```

Replace the example gateway with your actual gateway address.

Test an external IP address:

```bash
ping -c 4 8.8.8.8
```

Test a domain name:

```bash
ping -c 4 example.com
```

If the external IP works but the domain name fails, investigate DNS resolution.

Note: Some networks block ping, so a failed response is not conclusive evidence of a connection failure.

## 5. Step Four: Check DNS

Use nslookup:

```bash
nslookup example.com
```

Use dig:

```bash
dig example.com
```

Inspect resolver configuration:

```bash
cat /etc/resolv.conf
```

A DNS problem may prevent domain names from resolving even when network connectivity is available.

## 6. Step Five: Inspect Network Connections

Display listening ports:

```bash
ss -tuln
```

Display established TCP connections:

```bash
ss -tn
```

Display network socket information:

```bash
sudo ss -tulpn
```

## 7. Step Six: Trace the Network Route

Use traceroute:

```bash
traceroute example.com
```

Alternative:

```bash
tracepath example.com
```

These tools help identify where packets may be delayed or stopped along a route.

## 8. Step Seven: Check Network Services

Check NetworkManager:

```bash
systemctl status NetworkManager
```

Restart NetworkManager if appropriate:

```bash
sudo systemctl restart NetworkManager
```

Check system logs:

```bash
journalctl -u NetworkManager -n 50
```

Note: Not every Linux distribution uses NetworkManager. Identify the active network manager before restarting services.

## 9. Common Network Problems

| Problem | Possible Cause | Investigation |
|---|---|---|
| No IP address | DHCP failure | Check `ip addr` |
| No internet | Gateway or routing problem | Check `ip route` |
| Domain not resolving | DNS problem | Use `nslookup` |
| Slow connection | Congestion or packet loss | Use `ping` |
| Service unreachable | Firewall or service issue | Check `ss` and firewall rules |
| Interface down | Interface or driver issue | Check `ip link` |

## 10. Basic Troubleshooting Sequence

Follow this order:

1. Check the network interface.
2. Check the assigned IP address.
3. Check the default gateway.
4. Test local connectivity.
5. Test external IP connectivity.
6. Test DNS resolution.
7. Inspect routes and listening ports.
8. Review relevant network service logs.

## 11. Cybersecurity Applications

Network troubleshooting supports cybersecurity by helping professionals:

- Detect unexpected network behavior.
- Investigate connection failures.
- Identify suspicious listening ports.
- Examine routing problems.
- Review network service logs.
- Distinguish connectivity problems from possible security restrictions.

## 12. Practice Exercises

1. Which command displays Linux IP addresses?
2. How do you display the default gateway?
3. What does `ping` test?
4. Which command checks DNS resolution?
5. What is the purpose of traceroute?
6. How can you inspect listening ports?
7. Explain why a domain may fail to resolve while an IP address remains reachable.

## Summary

Network troubleshooting is an essential administration skill. A systematic approach makes it easier to identify connectivity, routing, DNS, and service problems.
