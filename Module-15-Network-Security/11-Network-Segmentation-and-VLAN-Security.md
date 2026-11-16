# MODULE 15 – NETWORK SECURITY
## LESSON 11: NETWORK SEGMENTATION AND VLAN SECURITY

---

## 1. Introduction

A computer network may contain hundreds or thousands of connected devices.

If every device belongs to one large network, a security problem affecting one device may create risks for many others.

Network segmentation helps solve this problem by dividing a large network into smaller, more manageable sections.

Each section can have its own security policies and access restrictions.

In this lesson, you will learn:

- What network segmentation means.
- What VLANs are.
- How VLANs work.
- Access ports and trunk ports.
- VLAN tagging.
- Inter-VLAN routing.
- VLAN security threats.
- How to inspect network interfaces and routes in Kali Linux.

---

## 2. What Is Network Segmentation?

Network segmentation is the process of dividing a network into smaller logical or physical sections.

Each segment can contain devices that share similar functions, security requirements, or access needs.

### Example: University Network

Imagine a university with these departments:

- Administration.
- Computer Science.
- Finance.
- Library.
- Students.
- Security Operations.

Instead of placing every device on one network, the university can create separate network segments.

### Example network structure

```text
                 UNIVERSITY NETWORK
                         |
                    Core Switch
                         |
        +----------------+----------------+
        |                |                |
        v                v                v
   Administration     Students         Finance
     Network          Network          Network
        |                |                |
     Devices          Devices          Devices
```

### Benefits of segmentation

1. Limits unauthorized access.
2. Reduces the spread of certain attacks.
3. Improves network management.
4. Separates sensitive systems.
5. Makes monitoring easier.
6. Helps enforce security policies.
7. Can reduce unnecessary broadcast traffic.

---

## 3. What Is a VLAN?

VLAN stands for Virtual Local Area Network.

A VLAN logically separates devices into different Layer 2 broadcast domains, even when they connect to the same physical switch infrastructure.

This means devices can belong to different logical networks without requiring completely separate physical switches.

### Example

A managed switch has 24 ports.

An administrator creates:

| VLAN | Department | Network |
|---|---|---|
| 10 | Administration | 192.168.10.0/24 |
| 20 | Students | 192.168.20.0/24 |
| 30 | Finance | 192.168.30.0/24 |
| 40 | IT Department | 192.168.40.0/24 |

Devices connected to VLAN 10 belong to the Administration broadcast domain.

Devices connected to VLAN 20 belong to the Students broadcast domain.

The VLANs remain logically separated unless routing or another permitted communication mechanism connects them.

---

## 4. Understanding VLAN IDs

Each VLAN has an identifier called a VLAN ID.

IEEE 802.1Q defines VLAN tagging.

The VLAN ID field is 12 bits long.

The commonly usable VLAN ID range is:

```text
1–4094
```

VLAN IDs 0 and 4095 have special meanings and are not ordinary assignable VLAN IDs.

### Example

```text
VLAN 10 = Administration
VLAN 20 = Students
VLAN 30 = Finance
VLAN 40 = IT
```

Using VLAN IDs helps administrators organize network traffic.

---

## 5. Access Ports

An access port normally connects an end device to a single VLAN.

Examples:

- Desktop computer.
- Printer.
- Ordinary laptop.
- IP phone, depending on switch configuration.

### Example

A computer connected to an access port assigned to VLAN 20 belongs to VLAN 20.

```text
Computer
    |
    |
Access Port
    |
    v
VLAN 20
```

The end device usually sends and receives ordinary untagged Ethernet frames.

The switch associates those frames with the configured access VLAN.

---

## 6. Trunk Ports

A trunk port carries traffic for multiple VLANs between network devices.

Examples:

- Switch-to-switch connection.
- Switch-to-router connection.
- Switch-to-firewall connection.
- Switch-to-virtualization host connection.

### Example

```text
              SWITCH A
             /        \
            /          \
       VLAN 10        VLAN 20
            \          /
             \        /
             TRUNK LINK
                  |
                  |
              SWITCH B
             /        \
            /          \
       VLAN 10        VLAN 20
```

Trunk links commonly use IEEE 802.1Q tags to identify VLAN membership.

---

## 7. Understanding IEEE 802.1Q

IEEE 802.1Q is the standard commonly used for VLAN tagging.

A VLAN tag is inserted into an Ethernet frame.

It identifies the VLAN to which the frame belongs.

### Important components

- Tag Protocol Identifier (TPID).
- Priority Code Point (PCP).
- Drop Eligible Indicator (DEI).
- VLAN Identifier (VID).

The VLAN Identifier identifies the VLAN.

### Why VLAN tagging matters

It allows multiple VLANs to share the same physical trunk connection while keeping their traffic logically separated.

---

## 8. Inter-VLAN Routing

Devices in different VLANs cannot communicate directly at Layer 2.

To communicate between VLANs, traffic generally needs Layer 3 routing.

This process is called inter-VLAN routing.

### Example

A university wants students to access an approved library server in another VLAN.

The network administrator configures routing and firewall policies to allow only the required communication.

### Inter-VLAN routing diagram

```text
         VLAN 10
       Administration
             |
             |
             v
       Layer 3 Router
             |
             |
             v
          VLAN 20
          Students
```

### Common methods

1. Router-on-a-stick.
2. Layer 3 switch.
3. Firewall-based routing.

### Router-on-a-stick

A router uses one physical interface with multiple logical subinterfaces.

Each subinterface handles a different VLAN.

### Layer 3 Switch

A Layer 3 switch can route traffic between VLANs using switched virtual interfaces (SVIs).

---

## 9. VLANs and Security

VLANs are useful for separating network traffic, but they are not complete security boundaries by themselves.

Security also depends on routing policies, firewall rules, switch configuration, and access controls.

### Example security policy

| Source VLAN | Destination VLAN | Policy |
|---|---|---|
| Students | Administration | Deny by default |
| Students | Library Server | Allow approved services |
| Finance | Administration | Allow selected services |
| IT | Managed Devices | Allow authorized administration |

The exact rules depend on organizational requirements.

### Principle of least privilege

Devices should only communicate with the network resources they need.

### Defense in depth

Use VLANs together with:

- Firewalls.
- Access control lists.
- Network monitoring.
- Endpoint security.
- Authentication.
- Secure switch configurations.

---

## 10. Common VLAN Security Threats

### 10.1 VLAN Hopping

VLAN hopping is an attack that attempts to gain access to traffic belonging to another VLAN.

Two commonly discussed techniques are:

- Switch spoofing.
- Double tagging.

### 10.2 Switch Spoofing

An attacker attempts to make a switch port operate as a trunk port.

If the switch is improperly configured, the attacker may gain access to multiple VLANs.

### 10.3 Double Tagging

An attacker crafts Ethernet frames containing multiple VLAN tags in an attempt to bypass VLAN separation under certain switch configurations.

This attack depends on specific network conditions and configuration weaknesses.

### 10.4 Native VLAN Misconfiguration

A native VLAN is associated with untagged traffic on an 802.1Q trunk.

Incorrect native VLAN configurations can create security and interoperability problems.

### 10.5 Unauthorized VLAN Access

An attacker may connect an unauthorized device to a switch port configured for a sensitive VLAN.

### 10.6 VLAN Misconfiguration

Incorrect VLAN assignments or trunk settings may expose sensitive systems or disrupt communication.

---

## 11. Preventing VLAN Attacks

Administrators should apply the following controls.

### 11.1 Disable Unused Switch Ports

Unused switch ports should be administratively disabled where appropriate.

### 11.2 Disable Unnecessary Trunk Negotiation

Configure access ports explicitly rather than allowing automatic trunk negotiation.

### 11.3 Restrict Allowed VLANs

Configure trunk links to carry only the VLANs they require.

### 11.4 Use an Unused Native VLAN

Use an appropriately configured VLAN that is not used for ordinary end-user access as the native VLAN.

Apply consistent settings across trunk endpoints.

### 11.5 Enable Port Security

Where supported, restrict the devices or MAC addresses allowed on access ports.

### 11.6 Apply Access Control Lists

Use ACLs and firewall policies to control communication between VLANs.

### 11.7 Monitor Switch Configuration

Review changes to VLAN membership, trunk settings, and port status.

### 11.8 Use Network Access Control

NAC solutions can help ensure that only authorized devices connect to protected network segments.

---

## 12. VLAN vs Subnet

VLANs and subnets are related but different concepts.

| VLAN | Subnet |
|---|---|
| Layer 2 logical network segmentation | Layer 3 IP network division |
| Defines a broadcast domain | Defines an IP address range |
| Identified by a VLAN ID | Identified by a network prefix |
| Configured on switches | Used in IP addressing and routing |

### Example

```text
VLAN 10
Subnet: 192.168.10.0/24

VLAN 20
Subnet: 192.168.20.0/24
```

A common design uses one IP subnet per VLAN, but network design requirements can vary.

---

## 13. VLAN Security in Virtualized Environments

Virtual machines and virtualization hosts can also participate in VLAN-based networks.

For example, a VMware host may connect virtual machines to different VLAN-backed networks.

### Example

```text
             Physical Switch
                   |
                   |
             VMware Host
                   |
          Virtual Network Switch
             /           \
            /             \
           v               v
       Kali VM       Metasploitable VM
       VLAN 10           VLAN 20
```

This is a conceptual example.

Actual VLAN support depends on the virtual network configuration, physical switch configuration, and VMware networking features.

A host-only VMware network is a separate virtual network arrangement and does not automatically require VLAN tagging.

---

# 14. PRACTICAL LAB 1: INSPECT NETWORK SEGMENTATION INFORMATION

## Objective

Inspect your own Kali VM's network interfaces, addresses, and routes.

This lab is read-only.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson11
cd ~/module15-lab/lesson11
```

### Step 2: Inspect interfaces

```bash
ip -br link
```

### Step 3: Inspect IP addresses

```bash
ip -br addr
```

### Step 4: Inspect routing

```bash
ip route
```

### Step 5: Inspect IPv6 routing

```bash
ip -6 route
```

### Step 6: Inspect interface details

```bash
ip link show
```

### Step 7: Save the results

```bash
ip -br link > interfaces.txt
ip -br addr > ip-addresses.txt
ip route > ipv4-routes.txt
ip -6 route > ipv6-routes.txt
ip link show > interface-details.txt
```

### Step 8: Analyze your results

Identify:

- Available network interfaces.
- Assigned IP addresses.
- Network prefixes.
- Default gateway.
- Interface states.

Remember: a single Kali VM interface does not reveal the complete VLAN configuration of the physical network.

---

# 15. PRACTICAL LAB 2: UNDERSTANDING VLAN TAGS

## Objective

Learn how VLAN tags can appear in packet analysis.

You will inspect an existing capture if you have one. Do not generate or transmit crafted VLAN-tagged traffic.

### Step 1: Check Wireshark or tshark availability

```bash
command -v tshark
```

### Step 2: Check available capture files

```bash
find ~/module15-lab -type f \( -name "*.pcap" -o -name "*.pcapng" \)
```

### Step 3: Inspect an existing authorized capture

If you have an appropriate capture file, replace the example filename:

```bash
tshark -r capture.pcap -Y "vlan"
```

### Step 4: Display VLAN fields

```bash
tshark -r capture.pcap -T fields -e vlan.id
```

If the capture contains no VLAN-tagged frames, the output may be empty.

That is normal for many ordinary VM network captures.

### Step 5: Record your observation

Create:

```bash
nano vlan-analysis.txt
```

Use this template:

```text
VLAN PACKET ANALYSIS

Capture File:
Capture Environment:

VLAN Tags Observed:
VLAN IDs:
Number of Tagged Frames:

Unexpected VLAN Information:
Not Observed / Requires Investigation

Conclusion:
```

If you do not have a suitable capture, record `No VLAN capture available` rather than inventing observations.

---

# 16. PRACTICAL PROJECT: UNIVERSITY VLAN SECURITY DESIGN

## Objective

Design a secure VLAN layout for a fictional university.

Create:

```bash
nano university-vlan-security-design.txt
```

Use the following design.

```text
UNIVERSITY VLAN SECURITY DESIGN

1. NETWORK REQUIREMENTS

University Departments:
- Administration
- Students
- Finance
- Computer Science Laboratory
- IT Department

2. VLAN PLAN

VLAN 10:
Department: Administration
Subnet: 192.168.10.0/24

VLAN 20:
Department: Students
Subnet: 192.168.20.0/24

VLAN 30:
Department: Finance
Subnet: 192.168.30.0/24

VLAN 40:
Department: Computer Science Laboratory
Subnet: 192.168.40.0/24

VLAN 50:
Department: IT Department
Subnet: 192.168.50.0/24

3. ACCESS POLICIES

Students:
- Access approved student services.
- No direct access to Finance systems.

Finance:
- Access authorized financial applications.
- Restricted administrative access.

Administration:
- Access approved administrative resources.

Computer Science Laboratory:
- Access authorized laboratory resources.

IT Department:
- Authorized infrastructure management.

4. SWITCH SECURITY

- Disable unused ports.
- Configure access ports explicitly.
- Restrict allowed trunk VLANs.
- Protect native VLAN configuration.
- Enable port security where appropriate.

5. ROUTING SECURITY

- Use firewall policies.
- Deny unnecessary inter-VLAN communication.
- Permit only required services.
- Monitor routing and access changes.

6. MONITORING

- Monitor switch logs.
- Monitor VLAN configuration changes.
- Investigate unauthorized devices.
- Review firewall logs.

7. CONCLUSION
```

---

## 17. Scenario-Based Exercise

Analyze the following situations.

For each one, identify the possible security problem and recommend an appropriate defense.

### Scenario A

A university uses one large network for students, finance, and administration.

### Scenario B

An unused switch port remains active in a sensitive department.

### Scenario C

A switch access port is incorrectly configured to negotiate trunk mode.

### Scenario D

Students can directly access the finance department's internal servers.

### Scenario E

An administrator discovers that a trunk link allows many VLANs that are not required for its operation.

---

## 18. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is network segmentation?
2. What does VLAN stand for?
3. What is a VLAN ID?
4. What is IEEE 802.1Q?
5. What is an access port?
6. What is a trunk port?
7. What is VLAN tagging?
8. What is inter-VLAN routing?
9. Explain router-on-a-stick.
10. What is a Layer 3 switch?
11. Differentiate between a VLAN and a subnet.
12. What is VLAN hopping?
13. Explain switch spoofing.
14. What is double tagging?
15. Why should unused switch ports be disabled?
16. Why should trunk ports allow only required VLANs?
17. What is a native VLAN?
18. How do firewalls improve VLAN security?
19. Why are VLANs not complete security boundaries by themselves?
20. How does network segmentation reduce security risks?

---

## 19. LESSON SUMMARY

In this lesson, you learned:

- The meaning of network segmentation.
- What VLANs are.
- VLAN IDs and IEEE 802.1Q.
- Access ports and trunk ports.
- VLAN tagging.
- Inter-VLAN routing.
- Router-on-a-stick.
- Layer 3 switching.
- VLAN and subnet differences.
- VLAN hopping and other VLAN threats.
- Switch security controls.
- VLAN security in virtualized environments.
- How to inspect your Kali VM's network configuration.
- How to examine VLAN-tagged packet captures.
- How to design a university VLAN security architecture.

**KEY TAKEAWAY:**

Network segmentation reduces unnecessary communication between devices and helps limit the impact of security incidents. VLANs provide logical Layer 2 separation, but strong security requires properly configured switches, routing policies, firewalls, monitoring, and access controls.

**END OF LESSON 11**