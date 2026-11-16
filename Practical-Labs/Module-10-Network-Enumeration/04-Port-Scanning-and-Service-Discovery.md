
# MODULE 10 – NETWORK ENUMERATION

# LESSON 04: PORT SCANNING AND SERVICE DISCOVERY

## 1. Introduction

Port scanning is the process of checking network ports to determine their accessibility and possible services.

Every network service that communicates through TCP or UDP uses a port.

For example:

- SSH commonly uses TCP port 22.
- HTTP commonly uses TCP port 80.
- HTTPS commonly uses TCP port 443.
- DNS commonly uses port 53.
- SMB commonly uses TCP port 445.

Port scanning helps administrators and security professionals understand which services are exposed on authorized systems.

## 2. What Is a Network Port?

A network port is a logical communication endpoint used by applications and services.

IP addresses identify network hosts, while ports help identify communication endpoints on those hosts.

Think of an IP address as a building address and a port as a particular entrance or department.

Example:

```text
IP Address: 192.168.56.20
Port:       22
Protocol:   TCP
Service:    SSH
```

This is an illustrative example.

## 3. Port Number Categories

TCP and UDP use port numbers from 0 to 65535.

They are commonly divided into three ranges.

| Range | Category | Description |
|---|---|---|
| 0–1023 | Well-known ports | Common system and application services |
| 1024–49151 | Registered ports | Registered application services |
| 49152–65535 | Dynamic/private ports | Commonly used for temporary client connections |

These ranges are conventional classifications.

## 4. Common Network Ports

| Port | Protocol | Common Service |
|---|---|---|
| 20 | TCP | FTP Data |
| 21 | TCP | FTP Control |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67 | UDP | DHCP Server |
| 68 | UDP | DHCP Client |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP |
| 443 | TCP | HTTPS |
| 445 | TCP | SMB |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |
| 5432 | TCP | PostgreSQL |
| 8080 | TCP | Alternative HTTP |

A port number does not guarantee the identity of a service. Always validate the actual service.

## 5. What Is Port Scanning?

Port scanning identifies the state of selected ports on a target system.

The main states include:

### Open

An application is accepting connections on the port.

### Closed

The host is reachable, but no application is listening on that port.

### Filtered

A firewall or network filter prevents Nmap from determining whether the port is open or closed.

### Open|Filtered

Nmap cannot distinguish between an open port and a filtered port.

## 6. What Is Service Discovery?

Service discovery is the process of identifying applications or network services associated with accessible ports.

Examples:

| Port | Possible Service |
|---|---|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 445 | SMB |

Service discovery can provide information about:

- Application type.
- Service name.
- Service version.
- Available network functionality.

## 7. Understanding Nmap Port Scanning Options

### A. Scan Common Ports

```bash
nmap 127.0.0.1
```

Scans Nmap's default set of common TCP ports.

### B. Scan Specific Ports

```bash
nmap -p 22,80,443 127.0.0.1
```

Checks TCP ports 22, 80, and 443.

### C. Scan a Port Range

```bash
nmap -p 1-100 127.0.0.1
```

Checks TCP ports from 1 through 100.

### D. Scan All TCP Ports

```bash
nmap -p- 127.0.0.1
```

Checks all TCP port numbers from 1 through 65535.

For this lesson, use only your own loopback system.

### E. Identify Services

```bash
nmap -sV 127.0.0.1
```

Attempts to identify services and versions.

## 8. Practical Lab 1: Inspect Local Listening Ports

Create your workspace:

```bash
mkdir -p ~/practical-labs/module10/lab04
cd ~/practical-labs/module10/lab04
```

Display listening TCP and UDP sockets:

```bash
ss -tuln
```

Display process information where available:

```bash
sudo ss -tulnp
```

Save the results:

```bash
ss -tuln > local-listening-ports.txt
```

Identify:

1. TCP listening ports.
2. UDP listening ports.
3. Local listening addresses.
4. Ports associated with services.

## 9. Practical Lab 2: Create a Controlled Local Service

We will create a simple HTTP service on your own Kali VM.

Create a directory:

```bash
mkdir -p ~/practical-labs/module10/lab04/www
```

Create a webpage:

```bash
echo "Module 10 Port Scanning Laboratory" > ~/practical-labs/module10/lab04/www/index.html
```

Start the server:

```bash
cd ~/practical-labs/module10/lab04/www
python3 -m http.server 8080 --bind 127.0.0.1
```

Keep this terminal open.

The server is bound to `127.0.0.1`, so it listens only on the local loopback interface.

Open another terminal.

Test the service:

```bash
curl http://127.0.0.1:8080
```

Expected content:

```text
Module 10 Port Scanning Laboratory
```

This gives us a predictable service to investigate.

## 10. Practical Lab 3: Discover the Local Service

Run:

```bash
nmap -p 8080 127.0.0.1
```

You should normally see port 8080 reported as open while the server is running.

Now perform service detection:

```bash
nmap -sV -p 8080 127.0.0.1
```

Observe:

- Port number.
- Port state.
- Service identification.
- Version information, if detected.

Save the results:

```bash
nmap -sV -p 8080 127.0.0.1 -oN local-http-service.txt
```

### Important Observation

Nmap may identify the service as HTTP and may provide information about the server software.

The exact result depends on your Python version and Nmap service-detection database.

## 11. Practical Lab 4: Compare Port States

While your local HTTP server is running, check two ports:

```bash
nmap -p 8080,8081 127.0.0.1
```

Expected general behaviour:

- Port 8080: Open while the Python server is running.
- Port 8081: Usually closed if no application is listening.

Your actual result may differ if another application uses port 8081.

Record the result.

Now stop the Python HTTP server using:

```text
CTRL + C
```

Run the scan again:

```bash
nmap -p 8080,8081 127.0.0.1
```

Compare the results.

Port 8080 should normally become closed because the server is no longer listening.

This demonstrates how service availability affects port states.

## 12. Practical Lab 5: Understand UDP Ports

UDP scanning works differently from TCP scanning.

A UDP service may not respond to an unexpected probe.

Therefore, Nmap may report:

- Open.
- Closed.
- Open|Filtered.

For your own loopback machine, inspect UDP listeners:

```bash
ss -uln
```

You may test selected local UDP ports:

```bash
sudo nmap -sU -p 53,123,161 127.0.0.1
```

This may take longer than a TCP scan.

Do not interpret `open|filtered` as confirmation that a service is running.

Record the actual output.

## 13. Practical Lab 6: Service Discovery with Netcat

Netcat is a networking utility that can establish TCP and UDP connections.

Check availability:

```bash
nc -h
```

Start a local listener:

```bash
nc -l 127.0.0.1 9090
```

In another terminal, connect:

```bash
nc 127.0.0.1 9090
```

Type:

```text
Hello from the local service discovery lab.
```

You should see the text appear in the listener terminal.

Now inspect the listening port:

```bash
ss -tuln
```

You may also check the port using:

```bash
nmap -p 9090 127.0.0.1
```

End the Netcat session using `CTRL + C` in the relevant terminals.

This is a simple demonstration of a service listening on a TCP port.

## 14. Practical Lab 7: Analyze Results

Create a report:

```bash
nano port-scanning-report.txt
```

Use this template:

```text
========================================
PORT SCANNING AND SERVICE DISCOVERY
========================================

1. ASSESSMENT INFORMATION

Date:
Target:
Assessment Scope:

2. TCP PORT ANALYSIS

Port:
State:
Service:

3. UDP PORT ANALYSIS

Port:
State:
Service:

4. SERVICE DISCOVERY

Service Name:
Port Number:
Version Information:
Detection Confidence:

5. CONTROLLED HTTP SERVICE

Listening Address:
Listening Port:
HTTP Test Result:
Nmap Result:

6. PORT STATE COMPARISON

Port 8080 Before Shutdown:
Port 8080 After Shutdown:
Port 8081 Result:

7. SECURITY OBSERVATIONS

Unexpected Services:
Potential Exposure:
Recommendations:

8. CONCLUSION

Summary:

========================================
END OF REPORT
========================================
```

Fill in the information using your actual observations.

## 15. Common Port Scanning Problems

### Problem 1: Port Is Closed

Possible explanation:

- No service is listening.
- The application has stopped.
- The service is bound to another address.

### Problem 2: Port Is Filtered

Possible explanation:

- Firewall rules are blocking probes.
- Network filtering prevents a clear response.

### Problem 3: Service Is Not Identified

Possible explanation:

- The service uses a non-standard port.
- The service does not reveal identifying information.
- The detection probe was inconclusive.

### Problem 4: Nmap and ss Show Different Results

Remember:

- `ss` inspects sockets on your local system.
- Nmap examines network responses from the target.

They use different methods and may report different information.

## 16. Knowledge Check

Answer these questions:

1. What is a network port?
2. What are the three common port number categories?
3. What is the difference between an open and closed port?
4. What does a filtered port mean?
5. What is service discovery?
6. Which command scans ports 22, 80, and 443?
7. What does `-p-` mean?
8. What does Nmap's `-sV` option do?
9. Why can UDP scanning be inconclusive?
10. What is the difference between `ss` and Nmap?
11. Why should every discovered service be documented?
12. Why is an open port not automatically a vulnerability?

## 17. Lesson Completion Checklist

- [ ] Understand network ports.
- [ ] Understand port number categories.
- [ ] Understand open, closed, and filtered states.
- [ ] Understand service discovery.
- [ ] Inspect local listening ports.
- [ ] Create a local HTTP service.
- [ ] Discover the local HTTP port.
- [ ] Compare open and closed ports.
- [ ] Understand UDP scanning.
- [ ] Use Netcat for a local connection.
- [ ] Save Nmap scan results.
- [ ] Create a port scanning report.

## 18. Lesson Summary

Port scanning and service discovery help security professionals understand which services are exposed on authorized systems.

Remember:

- Ports identify communication endpoints.
- TCP and UDP use separate port spaces.
- Open ports indicate accepting applications.
- Closed ports have no listening application.
- Filtered ports cannot be conclusively classified.
- Nmap identifies ports and can investigate services.
- Netcat can help test local network connections.
- `ss` provides local socket information.

**NEXT LESSON: SERVICE VERSION AND OPERATING SYSTEM DETECTION.**
