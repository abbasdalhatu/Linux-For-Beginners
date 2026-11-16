
# MODULE 13 – METASPLOITABLE LABS

# LESSON 03: METASPLOITABLE SETUP AND CONFIGURATION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand the Metasploitable 2 virtual machine requirements.
2. Obtain the official training image.
3. Import the virtual machine into VMware Workstation.
4. Configure its virtual hardware.
5. Configure its Host-only network adapter.
6. Boot and inspect Metasploitable.
7. Identify its IP address.
8. Verify basic isolation and connectivity.

---

## 2. Understanding Metasploitable 2

Metasploitable 2 is a deliberately vulnerable Ubuntu-based virtual machine developed by Rapid7 for security training.

It contains intentionally insecure services and applications.

We will use it to study:

- Network enumeration.
- Service identification.
- Vulnerability discovery.
- Controlled exploitation.
- Security validation.
- Evidence collection.
- Remediation.

The virtual machine is intentionally outdated and insecure.

Treat it as an isolated training target, not as a normal computer.

---

## 3. System Requirements

Before importing the VM, check your available laptop resources.

Your computer has:

- Windows 11 Pro.
- Intel Core i7 processor.
- 16 GB RAM.
- 256 GB SSD.

A practical allocation for this lab is:

| Resource | Metasploitable 2 |
|---|---|
| RAM | 512 MB–1 GB |
| CPU | 1 virtual processor |
| Disk | Use the supplied virtual disk |
| Network | Host-only |
| Display | Default |

Metasploitable 2 is lightweight.

Avoid allocating unnecessary RAM or CPU resources.

Keep enough resources available for Windows and Kali Linux.

---

## 4. Obtain the Official Metasploitable 2 Image

Use the official Rapid7 Metasploitable 2 download page:

https://information.rapid7.com/download-metasploitable-2.html

The official project information is also available here:

https://docs.rapid7.com/metasploit/metasploitable-2/

### Download Procedure

1. Open the official Rapid7 download page.
2. Locate Metasploitable 2.
3. Download the training archive.
4. Wait for the download to finish.
5. Do not execute any downloaded scripts.
6. Keep the original archive until the VM is working.

The archive normally contains a virtual machine disk and VMware-related configuration files.

Do not obtain the image from random file-sharing websites.

### Security Note

Metasploitable is deliberately vulnerable. Keep the archive and extracted files in a dedicated lab directory.

---

## 5. Practical Lab 1 – Create a Windows Storage Directory

Open Windows CMD.

Create a dedicated directory:

```cmd
mkdir C:\Users\HP\Desktop\linux\Metasploitable
```

Create a folder for the downloaded archive:

```cmd
mkdir C:\Users\HP\Desktop\linux\Metasploitable\Downloads
```

Create a folder for the extracted virtual machine:

```cmd
mkdir C:\Users\HP\Desktop\linux\Metasploitable\VM
```

Verify:

```cmd
dir C:\Users\HP\Desktop\linux\Metasploitable
```

Save your archive in the Downloads folder.

Extract it into the VM folder using Windows File Explorer.

Do not extract the archive into your Windows system directories.

---

## 6. Practical Lab 2 – Inspect the Extracted Files

Open:

```cmd
C:\Users\HP\Desktop\linux\Metasploitable\VM
```

Look for the virtual machine files.

Common file extensions include:

| Extension | Purpose |
|---|---|
| `.vmdk` | Virtual disk |
| `.vmx` | VMware virtual machine configuration |
| `.nvram` | Virtual machine firmware state |
| `.vmsd` | Snapshot metadata |
| `.vmxf` | Additional VMware configuration |

The exact collection of files may differ.

The `.vmdk` file contains the virtual machine's disk data.

The `.vmx` file contains its VMware hardware and device configuration.

---

## 7. Practical Lab 3 – Import Metasploitable into VMware

Open VMware Workstation.

Follow these steps:

1. Select File.
2. Select Open.
3. Navigate to the extracted Metasploitable VM directory.
4. Locate the `.vmx` configuration file.
5. Select it.
6. Open the virtual machine.

If the extracted image does not contain a `.vmx` file, do not guess at the configuration. Use VMware's documented procedure to create a VM around the supplied virtual disk.

### If VMware Asks Whether the VM Was Moved or Copied

For an imported training VM that you have extracted as a new lab instance, select the option that creates a new virtual machine identity if appropriate.

Record any configuration choices.

---

## 8. Practical Lab 4 – Configure Virtual Hardware

Before starting Metasploitable, open its Virtual Machine Settings.

Review the following:

### Memory

Set:

```text
512 MB to 1024 MB
```

### Processor

Set:

```text
1 virtual processor
```

### Network Adapter

Set:

```text
Host-only
```

Choose the same Host-only VMnet used by Kali.

### Additional Hardware

Do not add unnecessary network adapters.

For this lab, the target should have only one network adapter connected to the isolated Host-only network.

Check that the adapter is connected.

Do not select Bridged or NAT.

---

## 9. Practical Lab 5 – First Boot

Start Metasploitable.

Wait for the login screen.

The standard Metasploitable 2 training image commonly uses:

```text
Username: msfadmin
Password: msfadmin
```

These are intentionally weak credentials for the training machine.

They must never be reused on a real system.

Log in locally through the VMware console.

### Important

Do not enable remote access or expose the machine to external networks.

---

## 10. Practical Lab 6 – Inspect the Target Operating System

After logging in, run:

```bash
whoami
```

Check the operating system:

```bash
cat /etc/os-release
```

If that file is unavailable, try:

```bash
cat /etc/issue
```

Check the kernel:

```bash
uname -a
```

Check the hostname:

```bash
hostname
```

Check system uptime:

```bash
uptime
```

Record your results.

---

## 11. Practical Lab 7 – Identify the Metasploitable IP Address

Run:

```bash
ip addr
```

If the command is unavailable, use:

```bash
ifconfig
```

Identify the active Ethernet interface.

Look for an IPv4 address.

Example:

```text
eth0

inet 192.168.100.20
```

Your address may differ.

Do not copy the example IP as your actual target address.

Record the real address shown by your virtual machine.

### Additional Inspection

Run:

```bash
route -n
```

Record:

- Network interface.
- IPv4 address.
- Subnet mask.
- Default gateway, if present.

---

## 12. Practical Lab 8 – Verify Network Isolation

Return to Kali Linux.

Run:

```bash
ip addr
```

Compare Kali's IPv4 address with Metasploitable's IPv4 address.

Both should belong to the same Host-only subnet.

For example:

```text
Kali:
192.168.100.10

Metasploitable:
192.168.100.20
```

These are illustrative values.

### Test Basic Connectivity

From Kali, use the actual Metasploitable IP:

```bash
ping -c 4 TARGET_IP
```

Replace `TARGET_IP` with the real address.

For example:

```bash
ping -c 4 192.168.100.20
```

A successful response indicates basic IP connectivity.

A failed ping does not automatically mean the network is incorrectly configured. ICMP may be blocked, so check the adapter configuration and addresses before drawing conclusions.

### Isolation Verification

Confirm:

- Kali uses Host-only networking.
- Metasploitable uses the same Host-only network.
- Metasploitable has no NAT adapter.
- Metasploitable has no Bridged adapter.
- No unexpected network adapters are connected.

If Metasploitable appears to have external connectivity, stop and investigate before proceeding.

---

## 13. Practical Lab 9 – Create a Target Information Record

On Kali, create your workspace:

```bash
mkdir -p ~/practical-labs/module13/lab03
cd ~/practical-labs/module13/lab03
```

Create a file:

```bash
nano target-information.txt
```

Use this template:

```text
MODULE 13 – METASPLOITABLE TARGET INFORMATION

TARGET:
Metasploitable 2

VIRTUALIZATION:
VMware Workstation

OPERATING SYSTEM:
Record observed version

KERNEL:
Record observed kernel

HOSTNAME:
Record observed hostname

TARGET IP:
Record actual IPv4 address

NETWORK MODE:
Host-only

KALI IP:
Record actual IPv4 address

CONNECTIVITY:
Record ping result

ISOLATION:
Record adapter verification

NOTES:
Record any configuration issues
```

Save your file.

---

## 14. Troubleshooting

### Problem 1: Metasploitable Does Not Boot

Possible causes:

- Incomplete extraction.
- Missing virtual disk.
- Incorrect VMware configuration.
- Unsupported virtual hardware configuration.

Check that the `.vmdk` file exists and that VMware is opening the correct `.vmx` file.

### Problem 2: No IP Address Appears

Possible causes:

- Host-only DHCP is unavailable.
- Network adapter is disconnected.
- Incorrect VMnet selected.
- Guest network configuration problem.

Inspect:

```bash
ifconfig
```

and:

```bash
route -n
```

Do not change random network settings.

### Problem 3: Kali Cannot Reach Metasploitable

Check:

1. Both virtual machines are running.
2. Both adapters use the same Host-only VMnet.
3. Both machines have addresses in the same subnet.
4. The target adapter is connected.
5. There are no conflicting IP addresses.

### Problem 4: Windows Security Software Raises an Alert

Do not disable Windows Defender or other security controls to make the lab work.

Stop the activity and inspect the alert. Confirm the source and file before taking any action.

---

## 15. Knowledge Check

Answer these questions:

1. What is Metasploitable 2?
2. Why is it considered intentionally vulnerable?
3. What is the purpose of the `.vmdk` file?
4. What is the purpose of the `.vmx` file?
5. Why are we using Host-only networking?
6. What is the default training login for Metasploitable 2?
7. Which command displays IP addresses on Linux?
8. Why should Metasploitable not have a second NAT adapter?
9. What does a successful ping test demonstrate?
10. Why should we record the target's actual IP address rather than assume one?

---

## 16. Lesson Summary

In this lesson, you learned:

- How to obtain Metasploitable 2 from its official source.
- How to organize its files.
- How to import it into VMware.
- How to configure its virtual hardware.
- How to boot and inspect the target.
- How to identify its IP address.
- How to verify communication with Kali.
- How to check that the target remains isolated.
- How to document the target environment.

## 17. Professional References

1. Rapid7 Metasploitable 2 Documentation: https://docs.rapid7.com/metasploit/metasploitable-2/
2. Rapid7 Metasploitable 2 Download: https://information.rapid7.com/download-metasploitable-2.html
3. VMware Workstation Documentation: https://docs.vmware.com/
4. Kali Linux Documentation: https://www.kali.org/docs/

---

# END OF LESSON 03
