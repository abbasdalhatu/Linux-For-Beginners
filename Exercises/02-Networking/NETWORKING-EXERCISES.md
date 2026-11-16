
# NETWORKING — PRACTICAL EXERCISES

Welcome to the Networking practical exercises.

These exercises help learners understand computer networking, IP addressing, network protocols, and troubleshooting.

**Difficulty:** Beginner to Intermediate

## Exercise 1: Networking Fundamentals

Answer the following:

1. What is a computer network?
2. What is an IP address?
3. What is the difference between IPv4 and IPv6?
4. What is a MAC address?
5. What is a default gateway?
6. What is DNS?
7. What is DHCP?
8. Explain the difference between LAN and WAN.

## Exercise 2: Network Commands

Practise these commands on your own computer:

```bash
ip addr
ip route
ping -c 4 8.8.8.8
nslookup example.com
ss -tuln
```

Tasks:

1. Identify your network interfaces.
2. Find your IPv4 address.
3. Identify your default gateway.
4. Test connectivity to a public DNS server.
5. Resolve a domain name.
6. Display listening network ports.

Note: These commands are intended for Linux terminals.

## Exercise 3: IP Addressing

Study the following addresses:

- 192.168.1.10
- 10.0.0.5
- 172.16.5.20
- 8.8.8.8

Tasks:

1. Identify which addresses belong to private IPv4 ranges.
2. Identify the public IPv4 address.
3. Explain why private addresses are commonly used in local networks.
4. Explain the purpose of public IP addresses.

## Exercise 4: Subnetting

Solve these problems:

1. What is the subnet mask for /24?
2. How many total addresses are in a /24 subnet?
3. How many usable host addresses are normally available in a /24 subnet?
4. What is the subnet mask for /26?
5. How many total addresses are in a /26 subnet?
6. How many usable host addresses are normally available in a /26 subnet?

Challenge:

Divide 192.168.1.0/24 into four equal-sized subnets.

Identify the network address of each subnet.

## Exercise 5: Network Ports and Protocols

Match each port to its common service:

| Port | Service |
|---|---|
| 20/21 | ? |
| 22 | ? |
| 23 | ? |
| 25 | ? |
| 53 | ? |
| 80 | ? |
| 110 | ? |
| 143 | ? |
| 443 | ? |
| 3389 | ? |

Additional questions:

1. What is TCP?
2. What is UDP?
3. Explain the difference between HTTP and HTTPS.
4. Why should Telnet generally be avoided for sensitive remote administration?

## Exercise 6: DNS Investigation

Run:

```bash
nslookup example.com
nslookup -type=MX example.com
nslookup -type=NS example.com
```

Tasks:

1. Find the IP address returned for the domain.
2. Identify the mail exchange records.
3. Identify the name server records.
4. Explain the purpose of DNS.

## Exercise 7: Network Troubleshooting

Consider these situations:

**Situation A:** Your computer connects to Wi-Fi but cannot access websites.

Questions:
- What could cause this problem?
- How would you check DNS?
- How would you test connectivity?

**Situation B:** You cannot reach another computer on your local network.

Questions:
- What information should you check?
- What is the purpose of ping?
- How could an incorrect IP configuration cause this problem?

**Situation C:** A website works using its IP address but not its domain name.

Question:
- What network service should you investigate?

## Exercise 8: Practical Network Challenge

Using your own computer:

1. Display your network configuration.
2. Identify your IPv4 address.
3. Identify your default gateway.
4. Identify your DNS servers.
5. Test connectivity to a known destination.
6. Perform a DNS lookup.
7. Record your findings in a text file.

Never publish private network details in a public repository.

## Exercise 9: Knowledge Assessment

Answer these questions:

1. What is the purpose of an IP address?
2. What is subnetting?
3. What is the function of a router?
4. What does DNS do?
5. What is DHCP?
6. What is the difference between TCP and UDP?
7. What port does HTTPS commonly use?
8. What port does SSH commonly use?
9. What is a firewall?
10. What is network troubleshooting?

## Completion Checklist

- [ ] Understand networking fundamentals.
- [ ] Identify IPv4 and IPv6 addresses.
- [ ] Understand private and public IP addresses.
- [ ] Perform basic subnetting.
- [ ] Understand common ports and protocols.
- [ ] Perform DNS lookups.
- [ ] Practise network troubleshooting.
- [ ] Complete the practical challenge.

**Learning objective:** Develop the networking knowledge required for Linux administration and cybersecurity.
