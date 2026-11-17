---
---

# MODULE 15 – NETWORK SECURITY
## LESSON 12: WIRELESS NETWORK SECURITY

---

## 1. Introduction

Wireless networking allows devices to communicate without physical Ethernet cables.

Wi-Fi is one of the most widely used wireless networking technologies.

It is convenient, but wireless communication introduces security risks because radio signals travel through the air and may be received beyond the intended physical location.

Wireless network security focuses on protecting wireless devices, access points, authentication systems, and transmitted data.

In this lesson, you will learn:

- How wireless networks work.
- Wi-Fi standards and frequency bands.
- SSIDs and BSSIDs.
- Wireless authentication and encryption.
- WPA2 and WPA3.
- Common wireless attacks.
- Wireless security controls.
- How to inspect wireless capabilities safely in Kali Linux.

---

## 2. What Is a Wireless Network?

A wireless network connects devices using radio waves rather than physical network cables.

Examples include:

- Home Wi-Fi.
- University Wi-Fi.
- Office wireless networks.
- Public Wi-Fi hotspots.
- Wireless sensor networks.

### Basic wireless network architecture

```text
             INTERNET
                 |
                 |
               ROUTER
                 |
                 |
           WIRELESS ACCESS
                POINT
             /    |    \
            /     |     \
           v      v      v
        Laptop  Phone  Tablet
```

The wireless access point (AP) allows wireless clients to connect to the network.

A router may include an integrated wireless access point.

---

## 3. Understanding Wi-Fi

Wi-Fi is a family of wireless networking technologies based on IEEE 802.11 standards.

Different standards provide different capabilities, speeds, frequency support, and security features.

### Common Wi-Fi generations

| Wi-Fi generation | IEEE standard |
|---|---|
| Wi-Fi 4 | 802.11n |
| Wi-Fi 5 | 802.11ac |
| Wi-Fi 6 | 802.11ax |
| Wi-Fi 6E | 802.11ax with 6 GHz support |
| Wi-Fi 7 | 802.11be |

Actual performance depends on hardware, signal quality, interference, channel width, and network configuration.

---

## 4. Wireless Frequency Bands

Wi-Fi commonly operates in three frequency ranges.

### 4.1 2.4 GHz

Characteristics:

- Longer coverage in many environments.
- Often better wall penetration than higher frequencies.
- More susceptible to interference from other devices.
- Commonly used by older wireless devices.

### 4.2 5 GHz

Characteristics:

- Often provides higher available bandwidth.
- Generally has shorter range than 2.4 GHz under comparable conditions.
- Offers more channel options in many regions.
- Commonly used for modern Wi-Fi networks.

### 4.3 6 GHz

Characteristics:

- Supported by Wi-Fi 6E and Wi-Fi 7 devices.
- Provides additional spectrum where permitted.
- Can reduce congestion.
- Generally requires compatible hardware.
- Availability and channel rules depend on the country.

---

## 5. Important Wireless Network Terms

### 5.1 SSID

SSID means Service Set Identifier.

It is the wireless network name users see when searching for Wi-Fi.

Example:

```text
University-WiFi
Home-Network
MAAUN-Students
```

An SSID identifies a wireless network or network group.

### 5.2 BSSID

BSSID means Basic Service Set Identifier.

It identifies a particular wireless access point radio or basic service set, commonly represented by a MAC address.

### 5.3 Access Point

An access point allows wireless devices to connect to a network.

### 5.4 Wireless Client

A wireless client is a device connecting to an access point.

Examples include phones, laptops, and tablets.

### 5.5 Channel

A Wi-Fi channel is a portion of the frequency spectrum used for wireless communication.

Poor channel planning can increase interference and reduce performance.

---

## 6. Wireless Authentication

Wireless authentication determines whether a device or user is authorized to connect.

Common wireless security approaches include:

- WPA2-Personal.
- WPA2-Enterprise.
- WPA3-Personal.
- WPA3-Enterprise.

### WPA2-Personal

Uses a shared password or passphrase.

All authorized users generally know the same network credential.

### WPA2-Enterprise

Uses enterprise authentication, commonly through 802.1X and an authentication server such as RADIUS.

Users can have individual credentials.

### WPA3-Personal

Uses Simultaneous Authentication of Equals (SAE), designed to improve protection against password-guessing attacks compared with traditional WPA2-Personal authentication.

### WPA3-Enterprise

Provides enterprise-oriented authentication and security capabilities.

---

## 7. Wireless Encryption

Encryption protects wireless data from unauthorized reading.

### WPA2

WPA2 commonly uses AES-based CCMP protection.

### WPA3

WPA3 improves wireless security and uses modern cryptographic mechanisms.

### Why encryption matters

Without appropriate encryption, wireless traffic may be exposed to unauthorized observation.

Encryption helps protect:

- Login sessions.
- Personal information.
- Business communications.
- File transfers.
- Network activity.

However, encryption does not protect a device that has already been compromised.

---

## 8. IEEE 802.1X and RADIUS

Enterprise wireless networks often use centralized authentication.

### IEEE 802.1X

802.1X is a port-based network access control framework that can be used for wired and wireless access.

### RADIUS

RADIUS is a protocol used to support centralized authentication, authorization, and accounting.

### Simplified authentication flow

```text
Wireless Client
       |
       v
Access Point
       |
       v
Authentication Server
     (RADIUS)
       |
       v
Authentication Decision
       |
       v
Allow or Deny Access
```

This approach allows organizations to manage users individually instead of sharing one password with everyone.

---

## 9. Common Wireless Network Attacks

### 9.1 Evil Twin Attack

An evil twin is a malicious access point designed to imitate a legitimate wireless network.

Users may accidentally connect to the malicious network.

Possible consequences include:

- Credential theft.
- Traffic interception.
- Fake login pages.
- Redirection to malicious websites.

### 9.2 Rogue Access Point

A rogue access point is an unauthorized wireless access point connected to an organization's network or operating within its environment.

It may bypass established security controls.

### 9.3 Deauthentication Attack

A deauthentication attack abuses wireless management frames to disconnect clients from an access point.

Protected Management Frames (PMF), defined by IEEE 802.11w and incorporated into modern Wi-Fi security configurations, help defend against certain forged management-frame attacks.

### 9.4 Weak Password Attacks

Attackers may attempt to guess weak wireless passwords.

Common weaknesses include:

- Short passwords.
- Dictionary words.
- Reused passwords.
- Default router credentials.
- Passwords shared publicly.

### 9.5 Packet Sniffing

Wireless packet sniffing involves observing wireless traffic.

Unencrypted traffic may expose sensitive information.

Modern encryption helps protect data, but traffic metadata may still reveal useful information.

### 9.6 WPS Attacks

Wi-Fi Protected Setup (WPS) was designed to simplify device connection.

Some WPS implementations have security weaknesses.

Organizations should disable WPS where it is not required, especially when the implementation cannot be secured adequately.

### 9.7 Jamming

Wireless jamming involves interfering with radio communication.

It can disrupt wireless availability.

Defenses may include interference monitoring, channel planning, and appropriate wireless infrastructure design.

---

## 10. Wireless Security Controls

Strong wireless security requires multiple layers of protection.

### 10.1 Use WPA3 Where Supported

Prefer WPA3 where all required devices support it.

If WPA2 is necessary, use a secure configuration with AES-based CCMP rather than obsolete encryption methods.

### 10.2 Use Strong Passwords

Use long, unique wireless passwords.

Avoid:

- Names.
- Phone numbers.
- Simple dictionary words.
- Common keyboard patterns.

### 10.3 Change Default Administrator Credentials

Change default router and access point management credentials.

### 10.4 Disable Unnecessary WPS

Disable WPS when it is not required.

### 10.5 Update Firmware

Install security updates from the device manufacturer.

### 10.6 Use Guest Networks

Separate visitors from internal organizational systems.

### 10.7 Enable Client Isolation Where Appropriate

Client isolation can prevent wireless clients on the same network from communicating directly.

This is useful for certain guest wireless environments.

### 10.8 Monitor Rogue Access Points

Organizations should regularly monitor for unauthorized wireless devices.

### 10.9 Use Protected Management Frames

Enable PMF where supported and appropriate.

### 10.10 Segment Wireless Networks

Separate employee, guest, IoT, and management devices using suitable VLANs and firewall policies.

---

## 11. Public Wi-Fi Security

Public Wi-Fi networks are commonly found in:

- Airports.
- Hotels.
- Cafés.
- Libraries.
- Shopping centers.

They can be convenient but may be less trustworthy than a properly managed private network.

### Risks

- Evil twin access points.
- Fake captive portals.
- Weak or absent wireless encryption.
- Traffic monitoring.
- Malicious network redirection.
- Insecure device sharing.

### Safe practices

1. Verify the network name with the organization.
2. Avoid entering sensitive information into suspicious login pages.
3. Use HTTPS websites.
4. Keep operating systems and applications updated.
5. Disable unnecessary file sharing.
6. Avoid accepting unexpected certificates.
7. Use a trusted VPN when appropriate.
8. Disable automatic connection to unknown networks.

Remember that a VPN does not make a malicious website safe or protect a compromised device.

---

## 12. Wireless Security Monitoring

Wireless monitoring helps administrators detect suspicious access points, devices, and configuration changes.

Useful monitoring information includes:

- SSID.
- BSSID.
- Signal strength.
- Frequency band.
- Channel.
- Authentication method.
- Encryption method.
- Connected client information.
- Rogue access point alerts.

### Common defensive tools

| Tool | Purpose |
|---|---|
| NetworkManager | Manages network connections |
| nmcli | Command-line network management |
| iw | Wireless device inspection |
| Wireshark | Packet analysis |
| Kismet | Wireless network monitoring |
| WIDS | Wireless intrusion detection |
| WIPS | Wireless intrusion prevention |

Some tools require compatible wireless hardware and special permissions.

---

## 13. PRACTICAL LAB 1: CHECK WIRELESS HARDWARE

### Objective

Determine whether your Kali VM can see a wireless network interface.

A virtual machine may not directly expose the laptop's built-in Wi-Fi adapter. This is normal.

### Step 1: Create the lab directory

```bash
mkdir -p ~/module15-lab/lesson12
cd ~/module15-lab/lesson12
```

### Step 2: List network interfaces

```bash
ip -br link
```

### Step 3: Check wireless interfaces

```bash
iw dev
```

### Step 4: Check wireless hardware

```bash
iw list
```

If no wireless hardware is available, the output may be limited or empty.

### Step 5: Check NetworkManager

```bash
nmcli device status
```

### Step 6: Save your results

```bash
ip -br link > interfaces.txt
iw dev > wireless-interfaces.txt
nmcli device status > network-device-status.txt
```

### Step 7: Record your observation

Answer:

- Does Kali detect a wireless interface?
- What is the interface name?
- Is the interface managed?
- Is the wireless hardware exposed to the VM?

Do not change VMware networking settings for this exercise.

---

## 14. PRACTICAL LAB 2: INSPECT NETWORK CONNECTION INFORMATION

### Objective

Inspect your current network connection using NetworkManager.

### Step 1: Display active connections

```bash
nmcli connection show --active
```

### Step 2: Display device information

```bash
nmcli device show
```

### Step 3: Inspect IP configuration

```bash
ip addr
```

### Step 4: Inspect routing

```bash
ip route
```

### Step 5: Save the results

```bash
nmcli connection show --active > active-connections.txt
nmcli device show > device-information.txt
ip addr > ip-configuration.txt
ip route > routing-information.txt
```

### Step 6: Analyze

Identify:

- Your active network connection.
- Your assigned IP address.
- Your default gateway.
- Your network interface.
- Whether the VM uses an Ethernet-style virtual interface or a wireless interface.

A VMware NAT or host-only virtual adapter may appear as an Ethernet interface even when the physical host connects through Wi-Fi.

---

## 15. PRACTICAL PROJECT: WIRELESS NETWORK SECURITY AUDIT

### Objective

Create a security audit for a fictional university Wi-Fi network.

Create the report:

```bash
nano wireless-security-audit.txt
```

Use this template:

```text
WIRELESS NETWORK SECURITY AUDIT

1. NETWORK INFORMATION

Network Name:
Organization:
Access Point Type:
Frequency Band:

2. AUTHENTICATION

Security Standard:
WPA2 / WPA3:
Personal / Enterprise:
WPS Enabled:
MFA or Individual Authentication:

3. NETWORK CONFIGURATION

Guest Network:
Employee Network:
Management Network:
Client Isolation:
VLAN Segmentation:

4. SECURITY RISKS

Weak Passwords:
Rogue Access Points:
Outdated Firmware:
Unauthorized Devices:
Poor Network Segmentation:

5. RECOMMENDATIONS

- Use WPA3 where supported.
- Use strong authentication.
- Disable unnecessary WPS.
- Update access point firmware.
- Separate guest and employee networks.
- Monitor for rogue access points.
- Enable protected management frames.
- Apply firewall restrictions.

6. CONCLUSION
```

This is a design exercise. Do not attempt to access or audit a real wireless network without authorization.

---

## 16. SCENARIO-BASED EXERCISE

### Scenario A

A university uses the same wireless password for students, lecturers, and administrators.

What security problems could this create?

### Scenario B

A malicious access point uses the same SSID as a university network.

What type of attack is this?

### Scenario C

A public Wi-Fi network has no effective encryption.

What risks may users face?

### Scenario D

A company has no guest wireless network. Visitors connect directly to the employee network.

What security improvements would you recommend?

### Scenario E

An organization uses an outdated wireless access point with known security weaknesses.

What should its IT department do?

---

## 17. KNOWLEDGE CHECK

Answer these questions in your notebook.

1. What is a wireless network?
2. What is Wi-Fi?
3. What does IEEE 802.11 describe?
4. Differentiate between 2.4 GHz, 5 GHz, and 6 GHz.
5. What is an SSID?
6. What is a BSSID?
7. What is a wireless access point?
8. What is WPA2?
9. What is WPA3?
10. Differentiate between WPA2-Personal and WPA2-Enterprise.
11. What is AES-based CCMP?
12. What is IEEE 802.1X?
13. What is RADIUS?
14. Explain an evil twin attack.
15. What is a rogue access point?
16. What is a deauthentication attack?
17. What is WPS?
18. Why should wireless networks use strong passwords?
19. What is Protected Management Frames (PMF)?
20. Why should guest wireless networks be separated from internal networks?

---

## 18. LESSON SUMMARY

In this lesson, you learned:

- Wireless networking fundamentals.
- Wi-Fi standards.
- Wireless frequency bands.
- SSID and BSSID.
- Wireless access points.
- WPA2 and WPA3.
- Wireless authentication and encryption.
- IEEE 802.1X and RADIUS.
- Common wireless attacks.
- Wireless security controls.
- Public Wi-Fi safety.
- Wireless monitoring tools.
- How to inspect wireless capabilities in Kali Linux.
- How to prepare a wireless security audit.

**KEY TAKEAWAY:**

Wireless security is about protecting both the connection and the devices using it. Strong encryption, secure authentication, updated firmware, network segmentation, and continuous monitoring help protect wireless networks.

**END OF LESSON 12**