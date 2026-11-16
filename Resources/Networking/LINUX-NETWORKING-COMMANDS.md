
# LINUX NETWORKING COMMANDS

## 1. Introduction

Linux networking commands help administrators inspect network connections, configure interfaces, troubleshoot connectivity, and investigate network activity.

## 2. Display Network Interfaces

Display network interfaces and IP addresses:

```bash
ip addr
```

Display a shorter interface summary:

```bash
ip -br addr
```

Display network interfaces:

```bash
ip link
```

Show interface statistics:

```bash
ip -s link
```

## 3. Display IP Routing Information

Display the routing table:

```bash
ip route
```

Display the default gateway:

```bash
ip route show default
```

Display routing rules:

```bash
ip rule
```

## 4. Test Network Connectivity

Test connectivity to a host:

```bash
ping example.com
```

Send four packets:

```bash
ping -c 4 example.com
```

Test connectivity to an IP address:

```bash
ping 8.8.8.8
```

A successful ping indicates that ICMP communication is working. A failed ping does not always mean the host is offline because firewalls may block ICMP.

## 5. DNS Lookup Commands

Display DNS information:

```bash
nslookup example.com
```

Perform a DNS query:

```bash
dig example.com
```

Display the IP address associated with a hostname:

```bash
getent hosts example.com
```

## 6. Display Active Connections

Display listening TCP and UDP ports:

```bash
ss -tuln
```

Display established connections:

```bash
ss -tn
```

Display processes associated with network sockets:

```bash
sudo ss -tulpn
```

## 7. Trace Network Routes

Trace the route to a destination:

```bash
traceroute example.com
```

Alternative command:

```bash
tracepath example.com
```

These commands help identify the network hops between your machine and a destination.

## 8. Download and Test Network Resources

Download a file:

```bash
wget https://example.com/file.zip
```

Make an HTTP request:

```bash
curl -I https://example.com
```

Display detailed connection information:

```bash
curl -v https://example.com
```

Only use detailed testing against systems you own or have permission to assess.

## 9. Network Configuration Files

Important Linux networking locations include:

| Location | Purpose |
|---|---|
| /etc/hosts | Local hostname-to-IP mappings |
| /etc/resolv.conf | DNS resolver configuration |
| /etc/hostname | System hostname |
| /etc/hosts.allow | Legacy TCP Wrapper access rules |
| /etc/hosts.deny | Legacy TCP Wrapper access rules |

Note: Some distributions manage network and DNS configuration through NetworkManager, systemd-resolved, or other services.

## 10. Network Troubleshooting

A basic troubleshooting sequence:

1. Check whether the network interface is active.
2. Check the IP address.
3. Check the default gateway.
4. Test connectivity using ping.
5. Test DNS resolution.
6. Inspect routing information.
7. Check listening ports and connections.

Useful commands:

```bash
ip -br addr
ip route
ping -c 4 8.8.8.8
nslookup example.com
ss -tuln
```

## 11. Cybersecurity Applications

Networking commands help security professionals:

- Inspect network interfaces.
- Identify unexpected listening ports.
- Investigate active connections.
- Troubleshoot connectivity problems.
- Examine routing information.
- Verify DNS behavior.
- Support authorized network assessments.

## 12. Security Best Practices

- Avoid exposing unnecessary network services.
- Use firewalls to control network traffic.
- Secure remote connections with SSH.
- Monitor unexpected listening ports.
- Keep network software updated.
- Investigate unfamiliar connections.

## 13. Practice Exercises

1. Display your Linux IP addresses.
2. Display the default gateway.
3. Test connectivity to a public IP address.
4. Resolve example.com using nslookup.
5. Display listening TCP and UDP ports.
6. Explain the difference between TCP and UDP.
7. Explain why network administrators inspect listening ports.

## Summary

Linux networking commands provide essential tools for inspecting interfaces, testing connectivity, examining connections, and troubleshooting network problems.
