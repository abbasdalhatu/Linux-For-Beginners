
# MODULE 15 – CAPSTONE PROJECT

# LESSON 02: LAB ENVIRONMENT AND ARCHITECTURE

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain virtual lab architecture.
2. Understand Host-only and NAT networking.
3. Identify the roles of the lab machines.
4. Plan IP addressing for the laboratory.
5. Configure an isolated virtual network.
6. Verify connectivity between authorized lab machines.
7. Document the network architecture.

---

## 2. What Is a Virtual Cybersecurity Lab?

A virtual cybersecurity lab is an environment where multiple virtual computers operate on a physical computer.

Each virtual machine behaves like an independent computer with its own:

- Operating system.
- Virtual CPU.
- Memory.
- Virtual disk.
- Network adapter.
- Applications and services.

Virtual laboratories allow cybersecurity professionals and students to practice security assessment without exposing real systems to unnecessary risks.

### Why Use a Virtual Lab?

1. Isolation from normal computer activities.
2. Ability to create snapshots.
3. Safe cybersecurity training.
4. Easy restoration after experiments.
5. Ability to simulate multiple computers.
6. Lower hardware cost compared with purchasing multiple physical computers.

---

## 3. Our Capstone Network Architecture

We will use three main systems.

### System 1: Windows 11 Host

Your physical Dell Latitude 7320.

Responsibilities:

- Manage VMware.
- Store project documentation.
- Manage virtual machines.
- Preserve project files.
- Provide the main working environment.

### System 2: Kali Linux

The security assessment workstation.

Responsibilities:

- Network discovery.
- Service enumeration.
- Vulnerability assessment.
- Network traffic analysis.
- Evidence examination.
- Security reporting.

### System 3: Metasploitable 2

An intentionally vulnerable Linux training system.

Responsibilities:

- Act as the authorized assessment target.
- Provide intentionally vulnerable services.
- Support controlled security exercises.
- Demonstrate security weaknesses.

Metasploitable 2 must remain isolated from your normal network.

---

## 4. Understanding VMware Network Modes

VMware provides different virtual networking modes.

### 4.1 NAT Networking

NAT stands for Network Address Translation.

A virtual machine using NAT can access external networks through the host's network connection.

For example:

```text
Kali VM
   |
   v
VMware NAT
   |
   v
Windows Host
   |
   v
Internet
```

Typical uses:

- Downloading updates.
- Installing packages.
- Accessing authorized online resources.

### 4.2 Host-only Networking

Host-only networking creates a private network between the host and connected virtual machines.

Example:

```text
          Windows Host
                |
                |
          VMware Host-only
                |
          +-----+------+
          |            |
          v            v
       Kali VM    Metasploitable 2
```

Typical uses:

- Isolated cybersecurity exercises.
- Private VM-to-VM communication.
- Controlled network assessment.

### 4.3 Bridged Networking

Bridged networking connects a virtual machine to the physical network as though it were another device.

We will not use Bridged networking for Metasploitable 2.

---

## 5. Our Network Design

We will use two separate virtual network connections.

| Network | Purpose |
|---|---|
| NAT | Kali updates and internet access |
| Host-only | Isolated Kali and Metasploitable communication |

### Planned Layout

```text
                 INTERNET
                    |
                    |
              Windows Host
                    |
          +---------+---------+
          |                   |
          v                   v
      VMware NAT         Host-only Network
          |                   |
          v              +----+----+
       Kali VM           |         |
                         v         v
                      Kali VM  Metasploitable 2
```

Kali may have two virtual network adapters:

- Adapter 1: NAT.
- Adapter 2: Host-only.

Metasploitable 2 should have only the isolated Host-only adapter for this project.

This keeps the vulnerable target away from the normal LAN and internet.

---

## 6. IP Addressing Plan

We will use an example private network:

```text
192.168.56.0/24
```

This is a proposed address range, not a claim about your actual VMware network.

| Device | Example IP |
|---|---|
| VMware Host-only adapter | 192.168.56.1 |
| Kali Host-only interface | 192.168.56.10 |
| Metasploitable 2 | 192.168.56.20 |

Subnet mask:

```text
255.255.255.0
```

Network address:

```text
192.168.56.0
```

Broadcast address:

```text
192.168.56.255
```

Important: VMware's existing Host-only subnet may be different. Do not change it simply to match these examples. We will inspect the actual configuration first.

---

## 7. Practical Lab A: Inspect VMware Network Configuration

### Step 1: Open VMware

On Windows:

1. Open VMware Workstation.
2. Locate the Virtual Network Editor.
3. Examine the available virtual networks.
4. Identify the NAT network.
5. Identify the Host-only network.

Depending on your VMware version, you may need administrator privileges to open the Virtual Network Editor.

### Step 2: Record the Host-only Configuration

Look for:

- Virtual network name.
- Subnet IP.
- Subnet mask.
- DHCP enabled or disabled.
- Host virtual adapter status.

Do not change any settings yet.

### Step 3: Record the NAT Configuration

Identify:

- NAT virtual network.
- Subnet IP.
- Subnet mask.
- NAT service status.

You should now understand which virtual network is responsible for internet access and which is responsible for isolated communication.

---

## 8. Practical Lab B: Inspect Your Kali Network Interfaces

Start Kali Linux.

Open a terminal.

### Step 1: Display Network Interfaces

```bash
ip link show
```

### Step 2: Display IP Addresses

```bash
ip -br addr
```

### Step 3: Display Routing Information

```bash
ip route
```

### Step 4: Identify DNS Configuration

```bash
cat /etc/resolv.conf
```

### Step 5: Display Network Interface Details

```bash
ip addr show
```

Record the following:

| Item | Your Result |
|---|---|
| NAT interface | |
| Host-only interface | |
| NAT IP address | |
| Host-only IP address | |
| Default gateway | |
| DNS server | |

Do not assume that interface names will always be `eth0` or `eth1`. Your Kali VM may use names such as `ens33` or `ens37`.

---

## 9. Practical Lab C: Configure Kali Network Adapters

Shut down Kali before modifying its virtual hardware.

In VMware:

1. Select the Kali virtual machine.
2. Open Virtual Machine Settings.
3. Locate Network Adapter 1.
4. Set it to NAT.
5. Add a second network adapter if one does not already exist.
6. Set Network Adapter 2 to Host-only.
7. Save the configuration.
8. Start Kali.

Check the interfaces again:

```bash
ip -br addr
```

You should be able to identify the two network connections.

If VMware assigns different interface names, use the names shown by your system.

### Important

Do not manually assign an IP address until you have inspected the actual Host-only subnet and DHCP configuration.

---

## 10. Practical Lab D: Configure Metasploitable 2

Shut down Metasploitable 2.

In VMware:

1. Select Metasploitable 2.
2. Open Virtual Machine Settings.
3. Select its network adapter.
4. Choose Host-only.
5. Ensure it is not connected using Bridged networking.
6. Save the configuration.
7. Start the virtual machine.

Log in using the credentials supplied with your authorized Metasploitable 2 installation.

On the target, inspect the IP address:

```bash
ifconfig
```

If available, also run:

```bash
ip addr show
```

Record the actual Host-only IP address.

Do not assume it will match the example address in this lesson.

---

## 11. Practical Lab E: Test Isolated Connectivity

From Kali, identify its Host-only address:

```bash
ip -br addr
```

Then test connectivity to your actual Metasploitable 2 IP:

```bash
ping -c 4 TARGET_IP
```

Replace `TARGET_IP` with the actual address discovered on your target.

For example, the command format is:

```bash
ping -c 4 192.168.56.20
```

The example address is illustrative only.

### Expected Result

If the network is correctly configured and ICMP is permitted, you should receive replies.

If there are no replies, check:

1. Both VMs are powered on.
2. Kali has a Host-only adapter.
3. Metasploitable has a Host-only adapter.
4. Both addresses belong to the same subnet.
5. The correct target IP was entered.
6. The virtual network is enabled.

Some systems may block ICMP, so a failed ping alone does not prove that the machines cannot communicate.

Do not proceed to network scanning until the isolation and addressing are understood.

---

## 12. Practical Lab F: Create the Architecture Diagram

Create a project documentation directory:

```bash
mkdir -p ~/practical-labs/module15/architecture
cd ~/practical-labs/module15/architecture
```

Create an architecture record:

```bash
cat > network-architecture.txt <<'EOF'
CAPSTONE NETWORK ARCHITECTURE

Project:
Isolated Cybersecurity Assessment and Defense Lab

Physical Host:
Dell Latitude 7320
Windows 11 Pro

Virtualization:
VMware Workstation

Virtual Machines:

1. Kali Linux
Role: Security assessment workstation
Network: NAT + Host-only

2. Metasploitable 2
Role: Intentionally vulnerable training target
Network: Host-only only

Network Separation:

NAT:
Used for Kali internet access when required.

Host-only:
Used for isolated communication between
Kali and Metasploitable 2.

Security Restrictions:

- Metasploitable 2 must not use Bridged networking.
- No public IP scanning.
- No unauthorized external testing.
- Vulnerable services remain inside the isolated lab.

Actual IP addresses:
To be recorded after inspection.
EOF
```

Review it:

```bash
cat network-architecture.txt
```

---

## 13. Network Architecture Verification Checklist

Before proceeding, confirm:

- [ ] Windows host is operating normally.
- [ ] VMware Workstation is functioning.
- [ ] Kali VM starts successfully.
- [ ] Metasploitable 2 VM starts successfully.
- [ ] Kali has a NAT connection.
- [ ] Kali has a Host-only connection.
- [ ] Metasploitable 2 uses Host-only networking only.
- [ ] Actual IP addresses have been identified.
- [ ] Host-only subnet has been recorded.
- [ ] Isolated connectivity has been tested.
- [ ] Network architecture documentation has been created.

---

## 14. Troubleshooting Guide

### Problem 1: Kali Has No IP Address

Possible causes:

- DHCP is disabled.
- Virtual adapter is disconnected.
- Network service is not running.
- Incorrect virtual network selection.

Inspect:

```bash
ip -br addr
```

Then:

```bash
ip route
```

Check NetworkManager:

```bash
systemctl status NetworkManager
```

### Problem 2: Metasploitable Cannot Communicate With Kali

Check:

- Both VMs use the same Host-only network.
- Both addresses belong to the same subnet.
- The adapters are connected.
- The correct target IP is being used.

### Problem 3: Kali Has No Internet Access

Check:

- NAT adapter is connected.
- Kali has a valid NAT IP.
- A default route exists.
- DNS is configured.

Inspect:

```bash
ip route
```

Test an IP address first:

```bash
ping -c 4 1.1.1.1
```

Then test DNS:

```bash
ping -c 4 example.com
```

These are connectivity checks, not security scans.

---

## 15. Knowledge Check

1. What is a virtual cybersecurity lab?
2. What is the purpose of NAT networking?
3. What is the purpose of Host-only networking?
4. Why should Metasploitable 2 remain isolated?
5. What is the difference between NAT and Bridged networking?
6. What does `/24` mean in an IPv4 network?
7. Which command displays Linux network interfaces and IP addresses?
8. Why should actual IP addresses be recorded rather than assumed?
9. Why might a ping test fail even when a system is running?
10. What is the purpose of a network architecture diagram?

---

## 16. Key Takeaway

A properly designed cybersecurity laboratory is the foundation of a successful security assessment.

Network isolation protects your normal computer environment while allowing you to practice security testing in a controlled setting.

**Remember: Before testing a system, understand its network, confirm your authorization, and verify that the laboratory is isolated.**
