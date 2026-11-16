
# MODULE 10 – NETWORK ENUMERATION

# LESSON 07: SMB AND WINDOWS SERVICE ENUMERATION

## 1. Introduction

SMB stands for Server Message Block.

It is a network communication protocol that allows computers to share resources such as:

- Files
- Folders
- Printers
- Network drives
- Other shared resources

SMB is commonly used in Windows networks, but Linux systems can also support SMB through Samba.

In cybersecurity, SMB enumeration involves identifying and examining SMB services and their configurations within an authorized environment.

This lesson focuses on understanding SMB, identifying services, and performing safe enumeration in your own lab.

## 2. What Is SMB?

SMB is a client-server protocol.

A client requests access to a shared resource, while an SMB server provides that resource if the client is authorized.

Example:

```text
Windows Computer
       |
       | SMB Request
       v
SMB Server
       |
       v
Shared Folder
```

SMB allows users and computers to access shared resources across a network.

## 3. SMB Ports

Common SMB-related ports include:

| Port | Protocol | Purpose |
|---|---|---|
| 445 | TCP | Direct-hosted SMB |
| 139 | TCP | SMB over NetBIOS session service |
| 137 | UDP | NetBIOS name service |
| 138 | UDP | NetBIOS datagram service |

Modern Windows environments commonly use TCP port 445 for SMB.

Port 139 is associated with older NetBIOS-based SMB communication.

## 4. What Is Samba?

Samba is an implementation of SMB/CIFS for Unix-like operating systems.

It allows Linux systems to:

- Share folders with Windows.
- Access Windows shared folders.
- Participate in compatible network environments.
- Provide file-sharing services.

Common Samba utilities include:

| Tool | Purpose |
|---|---|
| smbclient | Access SMB shares |
| smbstatus | Display Samba server activity |
| smbtree | Display SMB network resources |
| testparm | Validate Samba configuration |
| nmblookup | Query NetBIOS names |

## 5. What Is SMB Enumeration?

SMB enumeration is the process of collecting information about SMB services.

It may reveal:

- Whether an SMB service is running.
- The SMB protocol version.
- Available shared folders.
- Server information.
- Authentication requirements.
- Security configuration.

SMB enumeration is useful during authorized security assessments because misconfigured file sharing can expose sensitive information.

## 6. Practical Lab 1: Create Your Workspace

Run:

```bash
mkdir -p ~/practical-labs/module10/lab07
cd ~/practical-labs/module10/lab07
```

Check your current network configuration:

```bash
ip addr
```

Check your routing table:

```bash
ip route
```

Check whether SMB-related ports are listening locally:

```bash
sudo ss -tulpn | grep -E ':139|:445'
```

If nothing appears, that is normal. It simply means no matching local service is listening.

## 7. Practical Lab 2: Check SMB Tools

Check whether the main utilities are installed:

```bash
smbclient --version
```

```bash
smbtree --version
```

```bash
testparm --version
```

If they are unavailable, install Samba utilities from Kali's official repositories:

```bash
sudo apt update
sudo apt install smbclient samba-common-bin
```

Verify the installation:

```bash
smbclient --version
```

## 8. Practical Lab 3: Understand SMB Service Detection

Nmap can identify whether SMB-related ports are exposed on an authorized system.

For this exercise, inspect your own Kali VM's loopback interface only:

```bash
nmap -sT -p 139,445 127.0.0.1
```

Understand the results:

| State | Meaning |
|---|---|
| open | A service is accepting connections |
| closed | The port is reachable but no service is listening |
| filtered | A filter prevents Nmap from determining the state |

An open SMB port does not automatically mean the system is vulnerable.

## 9. Practical Lab 4: Inspect SMB Shares with smbclient

`smbclient` is a command-line tool for communicating with SMB servers.

Its general format is:

```bash
smbclient -L //SERVER_ADDRESS -U USERNAME
```

This requests a listing of available shares.

Only use this against your own Samba server or a lab system you have permission to examine.

For example, if you have a local SMB server configured:

```bash
smbclient -L //127.0.0.1 -U your_username
```

Replace `your_username` with an account configured on that server.

If no local SMB server exists, the connection will fail. That is expected and does not mean your Kali installation is broken.

Do not attempt anonymous access against systems outside your authorized lab.

## 10. Practical Lab 5: Understand SMB Share Access

A share is a resource made available through SMB.

Example:

```text
\\SERVER\Documents
\\SERVER\Public
\\SERVER\Backups
```

An SMB share may require authentication.

Typical permissions include:

- Read
- Write
- Modify
- Full control

A share that allows unauthorized users to read sensitive files may expose confidential information.

A share that permits unrestricted writing may create additional security risks.

## 11. Practical Lab 6: Inspect Samba Configuration

If Samba is installed, check whether its configuration file exists:

```bash
ls -l /etc/samba/
```

Inspect the configuration:

```bash
sudo testparm -s
```

This validates and displays the effective Samba configuration.

If the configuration file is missing, you may not have a Samba server configured. That is fine.

Do not enable Samba services or create network shares merely to complete this lesson.

### Important Configuration Concepts

| Setting | Purpose |
|---|---|
| workgroup | Defines the Windows workgroup |
| server string | Describes the server |
| security | Controls authentication behavior |
| path | Defines a shared directory |
| read only | Controls whether writing is allowed |
| valid users | Restricts access to specified users |

Example of a conceptual share:

```ini
[Documents]
   path = /srv/samba/documents
   read only = yes
   valid users = student
```

This is an illustrative configuration, not an instruction to apply it to your system.

## 12. Practical Lab 7: Understand SMB Security

SMB has experienced serious security issues over the years.

Examples include:

### A. SMBv1

SMBv1 is an outdated protocol version with known security weaknesses.

Modern environments should avoid SMBv1 unless a documented compatibility requirement exists.

### B. Weak Share Permissions

Poorly configured shares can expose sensitive files.

### C. Weak Authentication

Weak passwords and poor account management can increase the risk of unauthorized access.

### D. Excessive Network Exposure

Exposing SMB services to untrusted networks increases the attack surface.

### E. Outdated SMB Software

Unpatched SMB implementations may contain known vulnerabilities.

## 13. Practical Lab 8: Windows SMB Security Awareness

On your Windows 11 host, you can inspect SMB configuration using PowerShell.

Open PowerShell and run:

```powershell
Get-SmbClientConfiguration
```

Inspect SMB server configuration:

```powershell
Get-SmbServerConfiguration
```

List locally configured SMB shares:

```powershell
Get-SmbShare
```

Check whether SMB1 is enabled:

```powershell
Get-SmbServerConfiguration | Select-Object EnableSMB1Protocol
```

These are inspection commands. Do not change your Windows SMB configuration as part of this exercise.

If a command returns an error or the SMB server component is unavailable, record the result rather than enabling additional Windows services.

## 14. Practical Lab 9: SMB Defensive Recommendations

Use these recommendations when assessing an SMB environment:

1. Disable SMBv1 where it is not required.
2. Keep Windows and Samba updated.
3. Restrict access to necessary users.
4. Avoid unnecessary shared folders.
5. Apply least-privilege permissions.
6. Use strong authentication.
7. Restrict SMB access to trusted network segments.
8. Monitor unusual SMB connections.
9. Review share permissions regularly.
10. Maintain backups of important shared data.

## 15. Practical Lab 10: Create an SMB Enumeration Report

Create your report:

```bash
nano smb-enumeration-report.txt
```

Use this template:

```text
========================================
SMB ENUMERATION REPORT
========================================

1. ASSESSMENT INFORMATION

Date:
Target:
Authorized Scope:

2. SMB PORT INSPECTION

Port 445:
Port 139:
Observed States:

3. SMB TOOLS

smbclient:
smbtree:
testparm:

4. SMB SERVICE INFORMATION

Service:
Server Information:
Protocol Information:

5. SHARE INFORMATION

Share Name:
Access Requirements:
Permissions:

6. WINDOWS SECURITY INSPECTION

SMB1 Status:
SMB Server Configuration:
Configured Shares:

7. SECURITY OBSERVATIONS

Potential Misconfigurations:
Unnecessary Exposure:
Weak Permissions:

8. RECOMMENDATIONS

Recommendation 1:
Recommendation 2:
Recommendation 3:

9. CONCLUSION

Summary:

========================================
END OF REPORT
========================================
```

Only include information you actually observed.

If no SMB service was available, document that fact. Do not fabricate shares, versions, or vulnerabilities.

## 16. Knowledge Check

1. What does SMB stand for?
2. What is the purpose of SMB?
3. What is the main TCP port used by modern SMB?
4. What is NetBIOS?
5. What is Samba?
6. What is the purpose of `smbclient`?
7. What does `smbtree` do?
8. What is an SMB share?
9. Why is SMBv1 considered outdated?
10. What is the risk of excessive share permissions?
11. Why should SMB access be restricted?
12. What does least privilege mean?

## 17. Lesson Completion Checklist

- [ ] Understand SMB fundamentals.
- [ ] Understand SMB ports.
- [ ] Understand NetBIOS basics.
- [ ] Understand Samba.
- [ ] Install and verify SMB utilities.
- [ ] Inspect local SMB-related ports.
- [ ] Understand SMB share enumeration.
- [ ] Understand Samba configuration.
- [ ] Inspect Windows SMB configuration.
- [ ] Understand SMB security risks.
- [ ] Create an SMB enumeration report.

## 18. Lesson Summary

SMB is an important protocol in Windows networking.

Remember:

- SMB enables network resource sharing.
- TCP 445 is the main modern SMB port.
- TCP 139 is associated with NetBIOS-based SMB.
- Samba provides SMB functionality on Linux.
- `smbclient` helps access SMB shares.
- `testparm` validates Samba configuration.
- Poorly configured SMB shares can expose sensitive information.
- SMBv1 should generally be avoided.

**NEXT LESSON: WEB SERVICE ENUMERATION.**
