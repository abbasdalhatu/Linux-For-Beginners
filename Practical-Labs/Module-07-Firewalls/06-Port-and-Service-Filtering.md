
# MODULE 07 – FIREWALL CONFIGURATION AND TESTING

# LAB 06: PORT AND SERVICE FILTERING

## 1. Introduction

Port and service filtering is a firewall technique used to control access to network services.

A computer can run multiple network services simultaneously. Each service may listen on a particular port.

For example:

- SSH commonly uses TCP port 22.
- HTTP commonly uses TCP port 80.
- HTTPS commonly uses TCP port 443.
- DNS commonly uses UDP and TCP port 53.

Firewalls use port numbers, protocols, IP addresses, and other information to determine which connections should be allowed.

Port filtering helps reduce unnecessary network exposure.

## 2. Learning Objectives

By the end of this lab, you should be able to:

1. Explain network ports.
2. Differentiate TCP and UDP.
3. Identify common service ports.
4. Understand port filtering.
5. Inspect listening services in Kali Linux.
6. Identify processes associated with listening ports.
7. Understand service exposure risks.
8. Design basic port-filtering policies.

## 3. Understanding Network Ports

A network port is a logical communication endpoint used by applications and services.

IP addresses identify network interfaces or hosts, while port numbers help identify the communication endpoint.

TCP and UDP use port numbers from 0 to 65535.

Port ranges are commonly classified as follows:

| Range | Classification |
|---|---|
| 0–1023 | Well-known ports |
| 1024–49151 | Registered ports |
| 49152–65535 | Dynamic/private ports |

These classifications are commonly used for both TCP and UDP.

## 4. Understanding TCP and UDP

### A. TCP

TCP stands for Transmission Control Protocol.

It provides connection-oriented communication.

Important characteristics include:

- Connection establishment.
- Reliable delivery.
- Ordered data transmission.
- Error detection.
- Retransmission of lost data.

Common TCP services include:

| Port | Service |
|---|---|
| 22 | SSH |
| 25 | SMTP |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |

### B. UDP

UDP stands for User Datagram Protocol.

It provides connectionless communication.

Important characteristics include:

- Lower protocol overhead.
- No built-in delivery guarantee.
- No built-in packet ordering.
- No connection establishment like TCP.

Common UDP uses include:

| Port | Service |
|---|---|
| 53 | DNS |
| 67 | DHCP Server |
| 68 | DHCP Client |
| 123 | NTP |
| 161 | SNMP |

Some services use both TCP and UDP depending on their operation.

## 5. Common Network Services and Ports

| Port | Protocol | Service | Purpose |
|---|---|---|---|
| 20 | TCP | FTP Data | Traditional FTP data |
| 21 | TCP | FTP Control | FTP control |
| 22 | TCP | SSH | Secure remote administration |
| 23 | TCP | Telnet | Remote terminal access |
| 25 | TCP | SMTP | Email transfer |
| 53 | TCP/UDP | DNS | Name resolution |
| 67 | UDP | DHCP Server | Address assignment |
| 68 | UDP | DHCP Client | Address assignment |
| 80 | TCP | HTTP | Web communication |
| 110 | TCP | POP3 | Email retrieval |
| 123 | UDP | NTP | Time synchronization |
| 143 | TCP | IMAP | Email retrieval |
| 443 | TCP | HTTPS | Encrypted web communication |
| 445 | TCP | SMB | File and printer sharing |
| 3306 | TCP | MySQL | Database service |
| 3389 | TCP/UDP | RDP | Remote desktop |

A port number alone does not prove which application is using it. Services can be configured to use non-standard ports.

## 6. What Is Port Filtering?

Port filtering means controlling traffic based on source or destination port numbers.

For example, a web server may allow:

- TCP 80 for HTTP.
- TCP 443 for HTTPS.

It may block unnecessary services such as Telnet on TCP 23.

A firewall rule can also restrict a service to particular source IP addresses.

Port filtering helps administrators reduce the number of services exposed to other systems.

## 7. Understanding Listening Ports

A listening port is a network endpoint waiting for incoming connections or datagrams.

For example, if an SSH server is listening on TCP port 22, it may accept incoming SSH connection attempts.

However, a listening socket does not necessarily mean that the service is reachable from another machine. Network interfaces, routing, and firewall rules also affect accessibility.

## 8. Practical Exercise 1: Inspect Listening Ports

Open Kali Linux.

Run:

```bash
ss -tuln
```

This displays listening TCP and UDP sockets.

Now run:

```bash
sudo ss -tulpn
```

This provides additional information, including process details where available.

Understand these options:

| Option | Meaning |
|---|---|
| -t | TCP sockets |
| -u | UDP sockets |
| -l | Listening sockets |
| -n | Numeric addresses and ports |
| -p | Process information |

Record the listening ports you find.

Do not assume every listening port is a security problem.

## 9. Practical Exercise 2: Identify Services and Processes

Run:

```bash
sudo lsof -i -P -n
```

This displays open network files and associated processes, if `lsof` is installed.

Check availability first:

```bash
command -v lsof
```

If it is unavailable, continue using `ss`.

Study the output and identify:

1. The process name.
2. The process ID, if displayed.
3. The protocol.
4. The local address.
5. The port number.

Record any services you recognize.

## 10. Practical Exercise 3: Inspect Service Definitions

Linux maintains a service-name database that maps common port numbers to service names.

Inspect it using:

```bash
less /etc/services
```

Search for SSH:

```bash
grep -E '^[[:space:]]*ssh[[:space:]]' /etc/services
```

Search for HTTP:

```bash
grep -E '^[[:space:]]*http[[:space:]]' /etc/services
```

Search for HTTPS:

```bash
grep -E '^[[:space:]]*https[[:space:]]' /etc/services
```

Search for DNS:

```bash
grep -E '^[[:space:]]*domain[[:space:]]' /etc/services
```

This file provides conventional service-to-port mappings. It does not prove that a service is currently running.

## 11. Practical Exercise 4: Understand Port-Filtering Rules

Study the following examples.

Do not execute them in this lab.

### Example A: Allow HTTPS

```bash
sudo ufw allow 443/tcp
```

Meaning:

- Action: Allow.
- Port: 443.
- Protocol: TCP.
- Purpose: Permit matching incoming HTTPS traffic.

### Example B: Block Telnet

```bash
sudo ufw deny 23/tcp
```

Meaning:

- Action: Deny.
- Port: 23.
- Protocol: TCP.
- Purpose: Block matching incoming Telnet traffic.

### Example C: Allow DNS

```bash
sudo ufw allow 53/udp
```

Meaning:

- Action: Allow.
- Port: 53.
- Protocol: UDP.
- Purpose: Permit matching incoming DNS traffic.

Remember that DNS may also require TCP port 53.

### Example D: Restrict SSH to a Trusted Address

Illustrative rule:

```bash
sudo ufw allow from 192.168.56.10 to any port 22 proto tcp
```

Meaning:

Only the specified source address is permitted to connect to the matching SSH destination port.

The address is an example, not a value to copy into your actual firewall.

## 12. Practical Exercise 5: Design a Port-Filtering Policy

Imagine you are protecting a Linux web server.

Design a policy using this table:

| Service | Port | Protocol | Proposed Action |
|---|---:|---|---|
| SSH | 22 | TCP | Allow trusted administrators |
| HTTP | 80 | TCP | Allow if required |
| HTTPS | 443 | TCP | Allow |
| Telnet | 23 | TCP | Block |
| MySQL | 3306 | TCP | Restrict to approved application hosts |
| DNS | 53 | UDP/TCP | Allow required DNS communication |

Answer these questions:

1. Which services must be publicly accessible?
2. Which services should be restricted to trusted addresses?
3. Which services should not be exposed unnecessarily?
4. How would you identify the process using a listening port?
5. Why is a port number not sufficient to identify a running service?

Do not apply this policy to your current VM.

## 13. Practical Exercise 6: Create Your Lab Notes

Create your working directory:

```bash
mkdir -p ~/practical-labs/module07/lab06
```

Enter it:

```bash
cd ~/practical-labs/module07/lab06
```

Create the notes file:

```bash
nano port-filtering-observations.txt
```

Use this template:

```text
MODULE 07 - LAB 06
PORT AND SERVICE FILTERING

1. LISTENING PORTS

Port 1:
Protocol:
Process:

Port 2:
Protocol:
Process:

Port 3:
Protocol:
Process:

2. SERVICE DEFINITIONS

SSH:
HTTP:
HTTPS:
DNS:

3. SERVICE SECURITY

Unnecessary Services:
Services Requiring Restriction:
Potential Risks:

4. PORT-FILTERING POLICY

SSH:
HTTP:
HTTPS:
TELNET:
MYSQL:
DNS:

5. SECURITY OBSERVATIONS
```

Save using:

- CTRL + O
- ENTER
- CTRL + X

## 14. Security Best Practices

When managing ports and services:

1. Expose only necessary services.
2. Disable unnecessary services through approved administrative procedures.
3. Restrict administrative ports to trusted networks.
4. Use encrypted protocols.
5. Monitor unexpected listening ports.
6. Review firewall rules regularly.
7. Avoid exposing database services publicly.
8. Document legitimate service requirements.
9. Investigate unexpected processes.
10. Test firewall changes in an isolated environment.

## 15. Knowledge Check

1. What is a network port?
2. What is the difference between TCP and UDP?
3. What is the conventional SSH port?
4. What is the conventional HTTPS port?
5. What is a listening port?
6. What does `ss -tuln` display?
7. What does the `-p` option do in `ss`?
8. Why should MySQL generally not be exposed publicly without a strong operational requirement?
9. What is port filtering?
10. Why does a listening port not automatically mean a service is reachable externally?

## 16. Lab Completion Checklist

- [ ] Understood network ports.
- [ ] Studied TCP and UDP.
- [ ] Reviewed common service ports.
- [ ] Inspected listening sockets.
- [ ] Identified available process information.
- [ ] Inspected `/etc/services`.
- [ ] Studied port-filtering rule syntax.
- [ ] Designed a port-filtering policy.
- [ ] Created `port-filtering-observations.txt`.
- [ ] Completed the knowledge check.

## 17. Expected Outcome

You should now understand how firewalls use ports and protocols to control access to network services.

You should also be able to inspect listening sockets and identify potentially unnecessary network exposure.

**Safety reminder:** This lab is inspection and policy-design focused. Do not add or remove firewall rules or stop services during this exercise.
