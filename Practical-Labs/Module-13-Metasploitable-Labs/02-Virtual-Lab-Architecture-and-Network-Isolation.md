
# MODULE 13 – METASPLOITABLE LABS

# LESSON 02: VIRTUAL LAB ARCHITECTURE AND NETWORK ISOLATION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain virtual network architecture.
2. Understand VMware networking modes.
3. Explain Host-only networking.
4. Configure a dedicated isolated virtual network.
5. Understand IP communication between virtual machines.
6. Identify the risks of exposing vulnerable systems.
7. Verify network isolation before beginning penetration testing.

---

## 2. What Is Virtual Lab Architecture?

Virtual lab architecture describes how virtual machines, networks, and other resources are organized to create a controlled testing environment.

In our cybersecurity lab, we have three main components:

**1. Windows 11 Host**

Your physical laptop running VMware Workstation.

**2. Kali Linux VM**

Your security testing workstation.

**3. Metasploitable 2 VM**

Your intentionally vulnerable target.

The virtual network connects Kali and Metasploitable without requiring either machine to be exposed to your normal physical network.

### Our Intended Architecture

```text
                  INTERNET
                     |
              WINDOWS 11 HOST
                     |
              VMware Workstation
                     |
             VMnet Host-only Network
                 (Private Lab)
                     |
             +-------+-------+
             |               |
          KALI VM       METASPLOITABLE
        TEST MACHINE     VULNERABLE VM
             |               |
             +-------+-------+
                     |
                ISOLATED LAB
```

The vulnerable target should not have a connection to the public internet.

---

## 3. Understanding VMware Network Modes

VMware provides several network modes.

### A. NAT Networking

NAT stands for Network Address Translation.

A virtual machine uses the host's network connection to access external networks.

Example:

```text
Kali VM
   |
VMware NAT
   |
Windows Host
   |
Internet
```

Common uses:

- Downloading software.
- Updating Kali Linux.
- Accessing external websites.

NAT is useful for normal virtual machine connectivity, but it is not our chosen configuration for Metasploitable.

### B. Bridged Networking

Bridged networking connects a virtual machine to the physical network through the host's network adapter.

The virtual machine can appear as another device on the physical network.

Example:

```text
Physical Router
      |
      +------ Windows Laptop
      |
      +------ Kali VM
      |
      +------ Metasploitable VM
```

This configuration is inappropriate for our intentionally vulnerable target.

Do not use Bridged networking for Metasploitable.

### C. Host-only Networking

Host-only networking creates a private virtual network.

Connected virtual machines can communicate with each other and, depending on VMware configuration, the host.

Example:

```text
Windows Host
      |
   VMnet1
      |
      +------ Kali Linux
      |
      +------ Metasploitable
```

This is the preferred network mode for our lab.

### D. Custom Networking

Custom networking allows a virtual machine to use a specific VMware virtual network.

For example, you might configure:

```text
VMnet2 = Dedicated Cybersecurity Lab
```

We will use the VMware virtual network assigned to our Host-only environment.

---

## 4. Understanding VMware Virtual Networks

VMware creates virtual network interfaces known as VMnets.

Common default configurations include:

| Virtual Network | Common Purpose |
|---|---|
| VMnet0 | Bridged networking |
| VMnet1 | Host-only networking |
| VMnet8 | NAT networking |

These are common defaults, not guaranteed configurations. Your VMware installation may differ.

We will identify the actual network settings rather than assume them.

### Important Network Components

**Virtual Network Adapter**

Connects a virtual machine to a selected VMware network.

**DHCP**

Automatically assigns IP addresses to devices.

**Subnet**

Defines the range of IP addresses available within a network.

**Gateway**

Provides a route to another network.

**Virtual Network Editor**

Allows administrators to configure VMware virtual networks.

---

## 5. Understanding IP Addressing in Our Lab

Each virtual machine needs an IP address on the same subnet to communicate directly.

Example:

| Machine | Example IP |
|---|---|
| Kali Linux | 192.168.100.10 |
| Metasploitable | 192.168.100.20 |
| Network | 192.168.100.0/24 |

These addresses are illustrative. Use the subnet configured in your VMware environment.

Both machines should have addresses within the same subnet.

For example:

```text
Kali:
192.168.100.10/24

Metasploitable:
192.168.100.20/24
```

They can communicate directly because they belong to the same IPv4 subnet.

### What Does /24 Mean?

The `/24` prefix represents a subnet mask of:

```text
255.255.255.0
```

It provides 256 total IPv4 addresses, with 254 traditionally usable host addresses in an ordinary IPv4 subnet.

---

## 6. Practical Lab 1 – Inspect VMware Network Configuration

On Windows 11:

1. Open VMware Workstation.
2. Locate the Edit menu.
3. Open Virtual Network Editor.
4. Review the available VMnet networks.
5. Identify the Host-only network.
6. Record its subnet address and subnet mask.

Do not change unrelated network settings.

If Virtual Network Editor is unavailable, check whether VMware requires administrator permission to open it.

### Record Your Findings

Create or edit:

```text
lab-architecture.txt
```

Record:

```text
VMware Host-only Network:

VMnet Name:
Subnet Address:
Subnet Mask:
DHCP Enabled:
Host Virtual Adapter:
```

Use the actual values displayed by VMware.

---

## 7. Practical Lab 2 – Verify Kali Network Configuration

Start your Kali VM.

Run:

```bash
ip addr
```

Identify the active network interface.

Example interface names:

```text
eth0
ens33
ens160
```

Your interface name may differ.

Check the routing table:

```bash
ip route
```

Check the IP configuration:

```bash
hostname -I
```

Record:

- Interface name.
- IPv4 address.
- Subnet prefix.
- Default gateway, if present.

### Important

For this lesson, we are preparing the network configuration. Do not attempt to access or scan any external system.

---

## 8. Practical Lab 3 – Configure Kali for Host-only Networking

Shut down Kali Linux completely.

Do not suspend the VM.

In VMware Workstation:

1. Select your Kali virtual machine.
2. Open Edit virtual machine settings.
3. Select Network Adapter.
4. Choose Host-only.
5. Confirm that the selected network is your intended Host-only VMnet.
6. Save the settings.
7. Start Kali again.

After booting, run:

```bash
ip addr
```

Then:

```bash
ip route
```

Confirm that Kali has an address on the intended Host-only subnet.

If no address is assigned, do not immediately change random network settings. Record the output so we can troubleshoot it.

---

## 9. Practical Lab 4 – Prepare the Metasploitable Network Adapter

This step is performed when you have imported the Metasploitable virtual machine.

Before powering it on:

1. Open VMware Workstation.
2. Select Metasploitable.
3. Open Virtual Machine Settings.
4. Select Network Adapter.
5. Choose Host-only.
6. Select the same Host-only network used by Kali.
7. Confirm the adapter is connected.
8. Save the configuration.

### Security Verification

Metasploitable must not use:

- Bridged networking.
- An additional NAT adapter.
- An additional adapter connected to your normal physical network.

For this training environment, use one Host-only adapter.

If you have not imported Metasploitable yet, simply record the intended configuration. We will configure the machine during Lesson 03.

---

## 10. Practical Lab 5 – Understand Network Isolation

Complete the following conceptual checks.

### Check 1: Can Kali and Metasploitable communicate?

Yes, provided that:

- Both adapters are connected.
- Both machines use the same Host-only VMnet.
- Their IP addresses belong to the same subnet.
- Their local firewall settings allow the required traffic.

### Check 2: Can Metasploitable access the public internet?

With only the isolated Host-only adapter and no separate routing mechanism, it should not have normal internet access.

### Check 3: Can Windows communicate with the virtual machines?

Depending on the VMware Host-only configuration, the Windows host may communicate with connected virtual machines.

### Check 4: Why is this important?

A vulnerable machine should remain within a controlled environment so its exposed services cannot be reached by unauthorized devices.

---

## 11. Practical Lab 6 – Create a Network Architecture Record

Open:

```bash
nano lab-architecture.txt
```

Complete the following template:

```text
MODULE 13 – LAB NETWORK ARCHITECTURE

HOST SYSTEM:
Windows 11

VIRTUALIZATION:
VMware Workstation

TESTING MACHINE:
Kali Linux

TARGET MACHINE:
Metasploitable 2

NETWORK MODE:
Host-only

VIRTUAL NETWORK:
Record your actual VMnet

KALI IP:
Record when configured

METASPLOITABLE IP:
To be recorded in Lesson 03

SUBNET:
Record the actual VMware subnet

SECURITY RESTRICTIONS:
- No Bridged networking for target.
- No second NAT adapter on target.
- No public network scanning.
- No unauthorized testing.
- All testing stays inside the lab.
```

Save the file.

---

## 12. Troubleshooting Common Network Problems

### Problem 1: Kali Has No IPv4 Address

Possible causes:

- Host-only DHCP is disabled.
- Virtual adapter is disconnected.
- Incorrect VMnet selected.
- Network configuration issue.

Useful commands:

```bash
ip addr
```

```bash
ip route
```

```bash
nmcli device status
```

Record the output before making changes.

### Problem 2: Two Machines Cannot Communicate

Possible causes:

- Different virtual networks.
- Different subnets.
- One virtual adapter disconnected.
- Local firewall restrictions.

Verify both machines use the same Host-only VMnet.

### Problem 3: Metasploitable Has Internet Access

Stop the virtual machine.

Check its adapter configuration.

Confirm that it uses only the intended Host-only adapter and that no unintended routing or additional adapters are configured.

Do not continue penetration-testing activities until isolation is verified.

---

## 13. Knowledge Check

Answer these questions:

1. What is virtual lab architecture?
2. What is the purpose of VMware Host-only networking?
3. Explain the difference between NAT and Bridged networking.
4. Why should Metasploitable not use Bridged networking?
5. What is a VMnet?
6. What is DHCP?
7. What is an IPv4 subnet?
8. Why should Kali and Metasploitable use the same Host-only network?
9. What can happen if an intentionally vulnerable VM is exposed to a normal network?
10. Why should network isolation be verified before penetration testing?

---

## 14. Lesson Summary

You have learned:

- The architecture of an isolated penetration-testing lab.
- VMware NAT, Bridged, Host-only, and Custom networking.
- The purpose of VMware virtual networks.
- Basic IPv4 addressing and subnet concepts.
- How to configure Kali for Host-only networking.
- How to prepare Metasploitable for isolation.
- How to document your lab architecture.
- How to recognize and troubleshoot common network problems.

## 15. Professional References

1. VMware Workstation Documentation: https://docs.vmware.com/
2. Kali Linux Documentation: https://www.kali.org/docs/
3. NIST Computer Security Resource Center: https://csrc.nist.gov/
4. RFC 1918 – Private IPv4 Address Allocation: https://www.rfc-editor.org/rfc/rfc1918

---

# END OF LESSON 02
