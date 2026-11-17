---
---

# Module 01: Linux Fundamentals

# Lesson 06 – Linux vs Windows

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 06 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

Linux and Windows are two widely used operating systems. Both allow users to interact with computer hardware, run applications, manage files, connect to networks, and perform computing tasks.

However, they have different designs, philosophies, architectures, and methods of managing computer systems.

Understanding the differences between Linux and Windows is important for computer science students, software engineers, system administrators, and cybersecurity professionals.

In ethical hacking, Linux is widely used because it provides powerful command-line tools, networking utilities, scripting capabilities, and specialised security distributions such as Kali Linux.

Windows is also important in cybersecurity because many organisations use Windows computers and Windows Server systems.

**The key idea:** Linux and Windows are both operating systems, but they provide different environments and approaches to managing computers.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what Linux and Windows are.
2. Identify the major differences between Linux and Windows.
3. Compare their file systems and directory structures.
4. Understand their command-line interfaces.
5. Explain their software installation methods.
6. Compare their security and permission systems.
7. Identify situations where Linux or Windows is more suitable.
8. Explain why cybersecurity professionals learn both operating systems.

## 3. What Is Linux?

Linux is an open-source, Unix-like operating-system kernel.

In everyday usage, the term Linux also refers to complete operating systems built around the Linux kernel.

Linux distributions combine the kernel with system utilities, libraries, package managers, applications, and other components.

Examples include:

- Ubuntu
- Debian
- Kali Linux
- Fedora
- Linux Mint
- Arch Linux

**Important characteristics of Linux:**

- Open-source software ecosystem.
- Highly configurable.
- Supports command-line and graphical interfaces.
- Commonly used on servers and cloud infrastructure.
- Strong networking and scripting capabilities.
- Available in specialised distributions for cybersecurity.

## 4. What Is Windows?

Windows is a family of proprietary operating systems developed by Microsoft.

Windows provides a graphical environment for running applications, managing files, connecting devices, and performing everyday computing tasks.

Examples include:

- Windows 10
- Windows 11
- Windows Server 2022
- Windows Server 2025

**Important characteristics of Windows:**

- Developed and maintained by Microsoft.
- Proprietary operating system.
- User-friendly graphical interface.
- Broad support for commercial desktop applications.
- Extensive hardware and software compatibility.
- Commonly used in homes, schools, businesses, and organisations.

## 5. Major Differences Between Linux and Windows

| Feature | Linux | Windows |
|---|---|---|
| Developer | Open-source community and organisations | Microsoft |
| Source code | Generally available under open-source licences | Proprietary |
| Kernel | Linux kernel | Windows NT kernel |
| Interface | CLI and GUI | GUI and CLI |
| Customisation | Highly customisable | Customisable within Microsoft's design |
| Software installation | Package managers and application stores | Installers, Microsoft Store and package managers |
| File systems | Commonly ext4, XFS, Btrfs | Commonly NTFS, FAT32, exFAT |
| Security permissions | Users, groups, ownership and permissions | Users, groups, ACLs and security policies |
| Common use | Servers, cloud, development and security | Desktop computing, business and gaming |
| Terminal | Bash, Zsh and other shells | PowerShell and Command Prompt |

**Remember:** Neither operating system is automatically secure in every situation. Security depends on configuration, updates, permissions, software, and user behaviour.

## 6. Differences in Directory Structure

Linux and Windows organise files differently.

### Linux directory structure

Linux uses a single directory hierarchy beginning at the root directory, represented by `/`.

Important Linux directories include:

| Directory | Purpose |
|---|---|
| `/` | Root of the entire filesystem hierarchy |
| `/home` | Personal directories of ordinary users |
| `/root` | Home directory of the root administrator |
| `/etc` | System configuration files |
| `/bin` | Essential user command binaries on systems using this layout |
| `/var` | Variable data, including logs |
| `/tmp` | Temporary files |
| `/usr` | Many user applications, libraries and shared resources |
| `/dev` | Device files |
| `/proc` | Virtual filesystem exposing process and kernel information |

Example:

```bash
cd /home
ls
```

This changes the current directory to `/home` and lists its contents.

### Windows directory structure

Windows commonly organises storage using drive letters.

Examples:

- `C:\`
- `D:\`
- `E:\`

Important Windows directories include:

| Directory | Purpose |
|---|---|
| `C:\Windows` | Windows operating-system files |
| `C:\Users` | User profiles and personal files |
| `C:\Program Files` | Installed 64-bit applications on typical 64-bit Windows installations |
| `C:\Program Files (x86)` | Common location for 32-bit applications |
| `C:\ProgramData` | Application data shared across users |

Example in Command Prompt:

```cmd
cd C:\Users
dir
```

This changes the directory to `C:\Users` and displays its contents.

### Key difference

Linux uses one unified directory hierarchy, while Windows commonly uses drive letters to identify storage volumes.

## 7. Differences in Command-Line Interfaces

Both operating systems provide command-line interfaces (CLI), but their default tools and commands differ.

### Linux terminal

Linux commonly uses shells such as:

- Bash
- Zsh
- Fish

Examples:

```bash
pwd
```

Displays the current working directory.

```bash
ls
```

Lists files and directories.

```bash
whoami
```

Displays the current username.

```bash
ip addr
```

Displays network interface information.

### Windows Command Prompt

Command Prompt is the traditional Windows command-line interpreter.

Examples:

```cmd
cd
```

Displays the current directory.

```cmd
dir
```

Lists files and directories.

```cmd
whoami
```

Displays the current username.

```cmd
ipconfig
```

Displays network configuration.

### Windows PowerShell

PowerShell is a more advanced command-line shell and scripting environment.

Examples:

```powershell
Get-Location
```

Displays the current location.

```powershell
Get-ChildItem
```

Lists files and directories.

```powershell
Get-Process
```

Displays running processes.

```powershell
Get-NetIPConfiguration
```

Displays network configuration.

**Important:** Some commands have similar names or purposes across operating systems, but their syntax and behaviour may differ.

## 8. Software Installation and Package Management

Linux and Windows use different approaches to installing and managing software.

### Linux package management

Linux distributions commonly use package managers.

For example, Kali Linux uses APT.

Update package information:

```bash
sudo apt update
```

Search for a package:

```bash
apt search nmap
```

Display package information:

```bash
apt show nmap
```

Install Nmap:

```bash
sudo apt install nmap
```

APT downloads and manages software packages from configured repositories.

### Windows software installation

Windows commonly uses:

- Official application installers.
- Microsoft Store.
- Windows Package Manager (`winget`).
- Organisation-managed software deployment tools.

Example using PowerShell or Command Prompt:

```powershell
winget search 7zip
```

Install 7-Zip:

```powershell
winget install 7zip.7zip
```

Package availability and installation requirements depend on the configured package sources and system permissions.

## 9. Security Differences

Security is one of the most important areas to understand when comparing Linux and Windows.

### Linux security

Linux uses several security mechanisms:

1. User accounts and groups.
2. File ownership and permissions.
3. Administrative privileges through `sudo`.
4. Firewall tools such as `nftables` and UFW.
5. Security frameworks such as SELinux and AppArmor.
6. Software updates and trusted package repositories.

Example:

```bash
ls -l
```

Displays file permissions and ownership information.

Example output:

```text
-rw-r--r-- 1 user user 1200 notes.txt
```

The permission section indicates that the owner can read and write, while the group and other users can read.

### Windows security

Windows also provides several security mechanisms:

1. User accounts and groups.
2. User Account Control (UAC).
3. NTFS permissions and Access Control Lists (ACLs).
4. Microsoft Defender Antivirus.
5. Windows Firewall.
6. BitLocker drive encryption.
7. Windows security policies and updates.

Example in PowerShell:

```powershell
Get-MpComputerStatus
```

Displays Microsoft Defender Antivirus status information when the relevant Defender features are available.

### Important security lesson

Linux is not automatically immune to malware, and Windows is not automatically insecure.

Both systems can be attacked when they are outdated, misconfigured, exposed to unnecessary services, or used carelessly.

Cybersecurity professionals must understand how to secure and investigate both.

## 10. Linux and Windows in Ethical Hacking

Ethical hackers need to understand both operating systems because real organisations use a mixture of Linux and Windows systems.

### Why ethical hackers use Linux

Linux is popular in cybersecurity because it provides:

- Powerful networking utilities.
- Flexible command-line environments.
- Shell scripting.
- Access to many open-source security tools.
- Distributions designed for security testing.

Examples of security tools:

| Tool | Purpose |
|---|---|
| Nmap | Network discovery and port scanning |
| Wireshark | Network traffic analysis |
| Metasploit Framework | Security testing and exploitation framework |
| Burp Suite | Web application security testing |
| tcpdump | Command-line packet capture |
| John the Ripper | Password security auditing |

These tools must only be used on systems you own or have explicit permission to test.

### Why ethical hackers use Windows

Windows is important because organisations frequently operate Windows-based environments.

Cybersecurity professionals use Windows knowledge to understand:

- Windows user accounts and privileges.
- Windows services and processes.
- Windows Defender and Firewall.
- Event logs and security monitoring.
- PowerShell administration.
- Active Directory environments.
- Windows Server security.

### Practical example

A security professional may use Kali Linux to perform an authorised network assessment while examining Windows event logs to understand how the Windows computer responded.

This is why learning both operating systems is valuable.

## 11. Advantages and Disadvantages

### Linux advantages

- Open-source ecosystem.
- High customisation.
- Powerful command-line tools.
- Excellent server and cloud support.
- Strong scripting capabilities.
- Many distributions are available without a licence fee.

### Linux disadvantages

- Some commercial applications are unavailable or have limited support.
- Certain hardware may require extra configuration.
- Beginners may need time to learn the command line.
- Software compatibility can vary between distributions.

### Windows advantages

- Familiar graphical interface.
- Broad commercial software compatibility.
- Strong support for many hardware devices.
- Widely used in businesses and educational institutions.
- Good support for gaming and productivity applications.

### Windows disadvantages

- Proprietary source code.
- Some editions require a paid licence.
- Common target for malware because of its widespread use.
- Some advanced administration tasks require learning PowerShell and Windows-specific tools.

## 12. Practical Lab: Comparing Linux and Windows

**Lab objective:** Practise identifying operating-system information, users, processes, directories, and network configuration on both systems.

### Part A: Linux practical (Kali Linux VM)

Open your Kali Linux terminal.

**Step 1 – Identify the operating system**

```bash
cat /etc/os-release
```

**Step 2 – Identify the kernel**

```bash
uname -r
```

**Step 3 – Identify the current user**

```bash
whoami
```

**Step 4 – Display the current directory**

```bash
pwd
```

**Step 5 – List files**

```bash
ls -la
```

**Step 6 – Display running processes**

```bash
ps aux | head
```

**Step 7 – Display network configuration**

```bash
ip addr
```

### Part B: Windows practical

Open Windows PowerShell.

**Step 1 – Identify Windows version**

```powershell
Get-ComputerInfo | Select-Object WindowsProductName, WindowsVersion, OsBuildNumber
```

**Step 2 – Identify the current user**

```powershell
whoami
```

**Step 3 – Display the current directory**

```powershell
Get-Location
```

**Step 4 – List files**

```powershell
Get-ChildItem
```

**Step 5 – Display running processes**

```powershell
Get-Process | Select-Object -First 10
```

**Step 6 – Display network configuration**

```powershell
ipconfig
```

**Step 7 – Display system information**

```powershell
systeminfo
```

### Part C: Comparison Exercise

Record your observations:

| Task | Linux command | Windows command |
|---|---|---|
| Current user | `whoami` | `whoami` |
| Current directory | `pwd` | `Get-Location` |
| List files | `ls -la` | `Get-ChildItem` |
| Running processes | `ps aux` | `Get-Process` |
| Network details | `ip addr` | `ipconfig` |
| System information | `uname -a` | `systeminfo` |

**Lab safety:** Run these commands in your own Kali virtual machine and Windows computer. They are intended for system inspection and do not require attacking another device.

## 13. Important Terms

| Term | Meaning |
|---|---|
| Operating System | Software that manages computer hardware and software resources |
| Linux | An open-source, Unix-like operating-system kernel and, commonly, systems built around it |
| Windows | A family of proprietary operating systems developed by Microsoft |
| Kernel | The core component that manages hardware and system resources |
| CLI | Command-Line Interface |
| GUI | Graphical User Interface |
| Shell | A program that provides an interface for interacting with an operating system |
| Package Manager | Software used to install and manage packages |
| NTFS | A common Windows filesystem |
| ext4 | A commonly used Linux filesystem |
| PowerShell | Microsoft's command-line shell and scripting environment |
| sudo | A command used to execute authorised commands with elevated privileges |

## 14. Knowledge Check

Answer these questions without looking at the lesson.

1. What is Linux?
2. What is Windows?
3. Who develops Windows?
4. What does open-source mean?
5. State four differences between Linux and Windows.
6. What is the main difference between their directory structures?
7. Name two Linux shells.
8. What is PowerShell?
9. What is APT used for?
10. Why should ethical hackers understand both Linux and Windows?

## 15. Practical Assignment

Complete the following tasks:

1. Open your Kali Linux virtual machine.
2. Run `uname -a` and record the output.
3. Run `cat /etc/os-release` and identify your Linux distribution.
4. Run `whoami` and identify the current user.
5. Open Windows PowerShell.
6. Run `systeminfo` and identify your Windows version.
7. Run `Get-Process` and observe running processes.
8. Write five differences between your Kali Linux environment and Windows 11.
9. Explain which operating system you would choose for cybersecurity practice and why.

**Submission:** Save your observations in a Markdown file named:

`Linux-vs-Windows-Practical-Assignment.md`

## 16. Exam-Ready Summary

Linux and Windows are two widely used operating systems with different designs and management approaches. Linux is an open-source, Unix-like operating-system kernel, while Windows is a proprietary operating-system family developed by Microsoft. Linux commonly uses a unified directory hierarchy beginning at `/`, whereas Windows commonly uses drive letters such as `C:\`. Linux uses shells such as Bash and package managers such as APT, while Windows provides Command Prompt, PowerShell, and software installation methods such as Windows Package Manager. Both operating systems provide security features, but their security models and administration tools differ. Ethical hackers study both because organisations use Linux and Windows systems in their networks.

## 17. Lesson Conclusion

Linux and Windows are both important operating systems in modern computing.

Linux provides a flexible environment for system administration, software development, servers, and cybersecurity testing. Windows provides a widely used environment for desktop computing, business applications, and enterprise infrastructure.

As an ethical hacker, you should not limit your knowledge to one operating system. Understanding both helps you assess, protect, troubleshoot, and manage different computing environments.

**Next Lesson:** Lesson 07 – Linux Kernel and Shell.