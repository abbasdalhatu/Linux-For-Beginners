# MODULE 15 – NETWORK SECURITY
## LESSON 03: NETWORK ATTACK SURFACES

---

## 1. Introduction to Attack Surfaces

An attack surface is the complete collection of points where an unauthorized person could potentially interact with, access, or compromise a system.

Every network-connected device, application, service, user account, and communication channel can introduce potential exposure.

### Simple example

Imagine a university network containing:

- A student portal.
- A staff portal.
- Wireless access points.
- Database servers.
- Email services.
- Remote administration systems.

Each component introduces possible security weaknesses.

The collection of these potential entry points forms the university's attack surface.

**Important:** An attack surface is not necessarily an indication that a system has been compromised. It describes the possible areas of exposure.

---

## 2. Why Attack Surface Management Matters

Attack surface management is the process of identifying, understanding, monitoring, and reducing unnecessary exposure.

Its objectives include:

1. Identify exposed systems.
2. Discover unnecessary services.
3. Reduce unauthorized access opportunities.
4. Identify vulnerable software.
5. Improve network visibility.
6. Prioritize security improvements.
7. Reduce the likelihood of successful attacks.

Organizations cannot protect systems effectively if they do not know what systems they own and expose.

---

## 3. Types of Attack Surfaces

Attack surfaces can be divided into several categories.

### 3.1 Network Attack Surface

This includes network-connected devices and services.

Examples:

- Routers.
- Switches.
- Firewalls.
- Servers.
- Open network ports.
- Wireless access points.
- Remote access services.

Potential weaknesses:

- Unnecessary open ports.
- Weak network configurations.
- Exposed administration interfaces.
- Outdated firmware.
- Incorrect firewall rules.

### 3.2 Application Attack Surface

This includes software applications and their interfaces.

Examples:

- Websites.
- APIs.
- Mobile applications.
- Student portals.
- Web-based administration systems.

Potential weaknesses:

- Insecure authentication.
- Input validation problems.
- Broken access controls.
- Outdated dependencies.
- Exposed application interfaces.

### 3.3 Human Attack Surface

This involves people who interact with systems.

Examples:

- Students.
- Employees.
- Administrators.
- IT support personnel.
- Contractors.

Potential weaknesses:

- Weak passwords.
- Phishing susceptibility.
- Accidental data disclosure.
- Excessive privileges.
- Poor security awareness.

### 3.4 Physical Attack Surface

This includes physical locations and equipment.

Examples:

- Server rooms.
- Network cabinets.
- Ethernet ports.
- Workstations.
- USB interfaces.
- Unsecured networking equipment.

Potential weaknesses:

- Unlocked server rooms.
- Unattended computers.
- Unsecured network ports.
- Stolen devices.

### 3.5 Cloud Attack Surface

This includes cloud-hosted infrastructure and services.

Examples:

- Cloud servers.
- Storage buckets.
- Cloud databases.
- Identity management systems.
- Cloud APIs.

Potential weaknesses:

- Publicly exposed storage.
- Incorrect permissions.
- Exposed credentials.
- Insecure cloud configurations.

### 3.6 Wireless Attack Surface

This includes wireless communication systems.

Examples:

- Wi-Fi access points.
- Wireless client devices.
- Bluetooth interfaces.
- Guest wireless networks.

Potential weaknesses:

- Weak wireless passwords.
- Outdated encryption.
- Rogue access points.
- Poor guest network isolation.

---

## 4. Attack Surface vs. Attack Vector

These concepts are related but different.

### Attack Surface

Describes the possible points of exposure.

### Attack Vector

Describes the method used to exploit a weakness or gain access.

Example:

A university's remote access portal is part of its attack surface.

An attacker using stolen credentials to access that portal is using an attack vector.

| Attack surface | Attack vector |
|---|---|
| Where exposure exists | How an attack is carried out |
| Describes potential entry points | Describes the attack method |
| Example: Remote login service | Example: Stolen credentials |

---

## 5. Common Network Entry Points

A network entry point is a location or service through which a device or user can connect to a network.

### 5.1 Open Ports

A network port identifies a communication endpoint associated with a service.

Examples:

| Port | Common service |
|---:|---|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP |

A listening port is not automatically dangerous.

However, unnecessary or insecure services can increase exposure.

### 5.2 Remote Access Services

Remote access allows users or administrators to connect to systems from another location.

Examples:

- SSH.
- RDP.
- VPN services.

Security concerns include:

- Weak authentication.
- Excessive privileges.
- Exposed management interfaces.
- Missing access restrictions.

### 5.3 Wireless Networks

Wireless networks allow devices to communicate without physical cables.

Security concerns include:

- Weak encryption.
- Shared passwords.
- Rogue access points.
- Poor separation between guest and internal networks.

### 5.4 Web Applications

Web applications provide services through browsers or APIs.

Security concerns include:

- Weak login controls.
- Insecure session handling.
- Misconfigured servers.
- Vulnerable software components.

---

## 6. The Principle of Attack Surface Reduction

Attack surface reduction means removing unnecessary exposure and strengthening the remaining entry points.

Common techniques include:

### 6.1 Disable Unnecessary Services

If a service is not required, it should not be running unnecessarily.

### 6.2 Close Unnecessary Ports

Firewall rules should allow only required communication.

### 6.3 Remove Unused Accounts

Inactive or unnecessary accounts should be reviewed and disabled according to organizational policy.

### 6.4 Apply Security Updates

Patch operating systems, applications, and firmware.

### 6.5 Enforce Strong Authentication

Use strong passwords, MFA, and secure account policies.

### 6.6 Restrict Administrative Access

Management interfaces should be accessible only to authorized administrators.

### 6.7 Segment Networks

Separate sensitive systems from less-trusted devices.

### 6.8 Monitor Network Activity

Maintain logs and investigate unexpected connections.

---

## 7. Attack Surface Management Lifecycle

Organizations can use the following process to manage attack surfaces.

### Step 1: Asset Discovery

Identify all relevant systems and devices.

### Step 2: Exposure Identification

Identify accessible services, applications, interfaces, and accounts.

### Step 3: Security Assessment

Review configurations and identify weaknesses.

### Step 4: Risk Prioritization

Evaluate the likelihood and impact of each issue.

### Step 5: Remediation

Fix vulnerabilities and remove unnecessary exposure.

### Step 6: Continuous Monitoring

Regularly review systems for new assets, configuration changes, and emerging weaknesses.

### Lifecycle summary

```text
Asset Discovery
      |
      v
Exposure Identification
      |
      v
Security Assessment
      |
      v
Risk Prioritization
      |
      v
Remediation
      |
      v
Continuous Monitoring
      |
      +------> Repeat
```

---

## 8. Attack Surface Mapping

Attack surface mapping is the process of documenting the components and entry points of a system.

Example: University network.

| Asset | Exposure | Possible weakness | Security control |
|---|---|---|---|
| Student portal | HTTPS service | Weak login protection | MFA |
| Database server | Internal network | Excessive access | Access controls |
| Wi-Fi | Wireless access | Weak encryption | WPA3 |
| SSH server | Remote administration | Weak credentials | Key authentication |
| Email system | Internet-facing | Phishing | Email filtering |

The table helps security teams understand where to focus their attention.

---

## 9. Attack Surface vs. Vulnerability Management

These activities are related but serve different purposes.

### Attack Surface Management

Focuses on discovering and reducing exposed systems and entry points.

### Vulnerability Management

Focuses on identifying, assessing, prioritizing, and fixing weaknesses in systems and applications.

| Attack surface management | Vulnerability management |
|---|---|
| Identifies exposure | Identifies weaknesses |
| Focuses on assets and entry points | Focuses on security flaws |
| Reduces unnecessary access points | Remediates vulnerabilities |
| Example: Identify an exposed service | Example: Patch a vulnerable service |

Both processes are important parts of a security program.

---

## 10. Common Attack Surface Management Tools

### Nmap

Used for authorized network discovery and service auditing.

### Wireshark

Used to inspect and analyze network packets.

### tcpdump

A command-line packet capture tool.

### OpenVAS / Greenbone

A vulnerability assessment platform.

### Nessus

A vulnerability scanning platform.

### Lynis

A Linux security auditing tool.

### Microsoft Defender

Provides endpoint security capabilities on Windows systems.

Tools should be used only on systems and networks you own or have explicit permission to assess.

---

# 11. PRACTICAL LAB: INSPECT YOUR KALI VM ATTACK SURFACE

## Lab objective

Identify the network interfaces, listening services, routes, and installed network-facing software on your own Kali virtual machine.

This is a local defensive audit. Do not scan external networks.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson03
cd ~/module15-lab/lesson03
```

### Step 2: Identify network interfaces

```bash
ip addr
```

Record:
- Interface names.
- IP addresses.
- MAC addresses.
- Interface status.

### Step 3: Identify listening services

```bash
ss -tulpen
```

Observe:
- Listening TCP ports.
- Listening UDP sockets.
- Local addresses.
- Associated process names where available.

A service listening on `127.0.0.1` is generally limited to local connections, while a service listening on a broader address may accept connections from other reachable devices, depending on firewall and network configuration.

### Step 4: Identify running services

```bash
systemctl --type=service --state=running
```

This displays active system services.

Look for services that provide network functionality.

Do not disable anything simply because you do not recognize its name.

### Step 5: Inspect installed network tools

```bash
command -v ssh
command -v curl
command -v wget
command -v tcpdump
command -v wireshark
```

A missing command simply means the executable was not found in the current PATH.

### Step 6: Inspect the firewall status

```bash
sudo ufw status verbose
```

If UFW is not installed or is inactive, record that fact. Do not enable or modify firewall rules during this inspection exercise.

### Step 7: Save your results

```bash
ip addr > interfaces.txt
ss -tulpen > listening-services.txt
systemctl --type=service --state=running > running-services.txt
sudo ufw status verbose > firewall-status.txt 2>&1
```

Check the generated files:

```bash
ls -lh
```

---

# 12. PRACTICAL PROJECT: BUILD AN ATTACK SURFACE INVENTORY

Create a file:

```bash
nano attack-surface-inventory.txt
```

Use this template:

```text
ATTACK SURFACE INVENTORY

ASSET 1:
Name: Kali Linux VM
Category: Network
IP Address: Record actual address
Exposure: VMware virtual network
Services: Record listening services
Risk: Depends on actual configuration
Security Recommendation: Disable unnecessary services

ASSET 2:
Name: Windows Host
Category: Physical Endpoint
Operating System: Windows 11 Pro
Exposure: Local and network interfaces
Services: To be assessed separately
Risk: Depends on configuration
Security Recommendation: Maintain updates and endpoint protection

ASSET 3:
Name: VMware Virtual Network
Category: Virtual Infrastructure
Exposure: Virtual network connections
Services: Virtual networking components
Risk: Depends on network configuration
Security Recommendation: Maintain isolation between lab and host
```

Save the file.

Do not include passwords, private keys, or other secrets in your inventory.

---

# 13. PRACTICAL PROJECT: ATTACK SURFACE RISK ASSESSMENT

Create:

```bash
nano attack-surface-risk.txt
```

Enter:

```text
ATTACK SURFACE RISK ASSESSMENT

Risk 1:
Entry Point: Unnecessary listening service
Possible Impact: Unauthorized access
Recommended Action: Review and disable if unnecessary

Risk 2:
Entry Point: Remote administration service
Possible Impact: Unauthorized remote access
Recommended Action: Restrict access and enforce strong authentication

Risk 3:
Entry Point: Wireless network
Possible Impact: Unauthorized network access
Recommended Action: Use modern encryption and strong authentication

Risk 4:
Entry Point: Outdated software
Possible Impact: Exploitation of known vulnerabilities
Recommended Action: Apply security updates

Risk 5:
Entry Point: Excessive user privileges
Possible Impact: Unauthorized modification of resources
Recommended Action: Apply least privilege
```

These are example risks, not confirmed vulnerabilities in your VM.

---

# 14. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is an attack surface?
2. Why is attack surface management important?
3. Mention six categories of attack surfaces.
4. Differentiate between an attack surface and an attack vector.
5. Why can unnecessary open ports increase security exposure?
6. What is a remote access service?
7. Explain attack surface reduction.
8. What is attack surface mapping?
9. Differentiate between attack surface management and vulnerability management.
10. What is asset discovery?
11. Why should organizations disable unnecessary services?
12. Explain the purpose of network segmentation.
13. What is the purpose of `ss -tulpen`?
14. Why should security professionals monitor listening services?
15. Explain the attack surface management lifecycle.

---

# 15. LESSON SUMMARY

In this lesson, you learned:

- The meaning of attack surfaces.
- The importance of attack surface management.
- Network, application, human, physical, cloud, and wireless attack surfaces.
- The difference between attack surfaces and attack vectors.
- Common network entry points.
- Attack surface reduction techniques.
- Attack surface mapping.
- Vulnerability management.
- Common attack surface management tools.
- How to inspect your own Kali VM.
- How to build an attack surface inventory and risk assessment.

**KEY TAKEAWAY:**

Every exposed service, device, application, and account can introduce potential risk. A strong security professional first understands what exists, identifies unnecessary exposure, assesses weaknesses, and reduces the attack surface.

**END OF LESSON 03**