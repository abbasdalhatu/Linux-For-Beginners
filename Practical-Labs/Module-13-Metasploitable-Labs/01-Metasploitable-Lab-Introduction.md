
# MODULE 13 – METASPLOITABLE LABS

# LESSON 01: METASPLOITABLE LAB INTRODUCTION

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what Metasploitable is.
2. Understand why intentionally vulnerable systems are used in cybersecurity training.
3. Explain the relationship between Kali Linux and Metasploitable.
4. Understand the basic penetration-testing methodology.
5. Identify the security risks associated with vulnerable systems.
6. Explain why network isolation is important.
7. Prepare for a controlled penetration-testing laboratory.

---

## 2. What Is Metasploitable?

Metasploitable is an intentionally vulnerable virtual machine developed for cybersecurity education and penetration-testing practice.

Unlike normal operating systems, which are designed to protect themselves, Metasploitable contains deliberately introduced security weaknesses.

These weaknesses allow students and security professionals to study:

- Network reconnaissance.
- Open ports and services.
- Service misconfigurations.
- Vulnerability identification.
- Penetration-testing methodology.
- Security validation.
- Evidence collection.
- Remediation recommendations.

Think of Metasploitable as a cybersecurity training building with deliberately weakened doors, windows, and security systems.

You are learning how to identify those weaknesses and explain how to fix them.

### Important

Metasploitable is intentionally insecure.

Never expose it to:
- The public internet.
- Your university network.
- Public Wi-Fi.
- Your normal production network.

Keep it inside an isolated virtual laboratory.

---

## 3. What Is Metasploitable 2?

Metasploitable 2 is a deliberately vulnerable Linux virtual machine commonly used for penetration-testing education.

It includes several vulnerable services and applications that provide opportunities to learn security assessment techniques.

Examples of services commonly found in the training environment include:

| Service | Typical Port | Purpose |
|---|---:|---|
| FTP | 21 | File transfer |
| SSH | 22 | Secure remote access |
| Telnet | 23 | Remote terminal access |
| SMTP | 25 | Email transfer |
| HTTP | 80 | Web applications |
| SMB | 139/445 | File and printer sharing |
| MySQL | 3306 | Database service |
| PostgreSQL | 5432 | Database service |

These are typical ports associated with the lab image. Actual availability must be verified during your own assessment.

---

## 4. Understanding the Laboratory Environment

Our laboratory will use two virtual machines.

### Machine 1: Kali Linux

Kali Linux is your security testing workstation.

It will be used for:

- Network discovery.
- Port scanning.
- Service identification.
- Vulnerability assessment.
- Metasploit Framework.
- Evidence collection.
- Security reporting.

### Machine 2: Metasploitable 2

Metasploitable will act as the intentionally vulnerable target.

It will be used for:

- Service enumeration.
- Vulnerability identification.
- Controlled validation.
- Security investigation.
- Remediation analysis.

### Laboratory Architecture

```text
             WINDOWS 11 HOST
                    |
              VMware Workstation
                    |
              HOST-ONLY NETWORK
                    |
          +---------+---------+
          |                   |
       KALI VM           METASPLOITABLE 2
      TEST SYSTEM          TARGET SYSTEM
          |                   |
          +---------+---------+
                    |
             ISOLATED LAB
```

Both virtual machines communicate through the isolated virtual network.

Your Windows host remains your normal working environment.

---

## 5. Understanding the Penetration-Testing Methodology

Professional penetration testing follows a structured process.

### Stage 1: Planning and Authorization

Determine:

- What system is being tested?
- Who owns the system?
- What activities are permitted?
- What are the testing limitations?

In our laboratory, you own and control both virtual machines.

### Stage 2: Reconnaissance

Gather information about the target.

Examples:

- IP address.
- Host availability.
- Network configuration.
- Available services.

### Stage 3: Enumeration

Identify more detailed information about exposed services.

Examples:

- Service names.
- Service versions.
- Protocols.
- Available resources.

### Stage 4: Vulnerability Identification

Compare discovered services and configurations with known weaknesses.

### Stage 5: Controlled Validation

Determine whether a suspected weakness is genuinely present.

Testing must remain within the isolated environment and avoid unnecessary disruption.

### Stage 6: Evidence Collection

Record:

- Commands executed.
- Results observed.
- Screenshots.
- Service information.
- Vulnerability details.

### Stage 7: Reporting

Produce a professional report containing:

- Findings.
- Severity.
- Evidence.
- Potential impact.
- Recommendations.

---

## 6. What Is the Metasploit Framework?

Metasploit is a penetration-testing framework used by security professionals to research and validate security vulnerabilities.

It provides different types of modules.

| Module Type | Purpose |
|---|---|
| Auxiliary | Information gathering and supporting security tests |
| Exploit | Attempts to trigger a vulnerability |
| Payload | Defines actions performed after successful exploitation |
| Post | Performs authorized post-exploitation activities |
| Encoder | Transforms payload representations |
| NOP | Provides no-operation instructions for certain testing scenarios |

The main Metasploit console is called:

```bash
msfconsole
```

We will study the framework in detail in Lesson 07.

Do not attempt exploitation in this first lesson.

---

## 7. Why Network Isolation Matters

A vulnerable machine can expose services that attackers could abuse.

If you accidentally connect Metasploitable to your normal network, its vulnerable services may become accessible to other devices.

For this reason, our lab uses VMware Host-only networking.

### VMware Network Modes

| Mode | Description |
|---|---|
| NAT | Allows a VM to access external networks through the host |
| Bridged | Connects a VM directly to the physical network |
| Host-only | Creates a private network between the host and connected VMs |
| Custom | Uses a selected virtual network configuration |

### Our Laboratory Configuration

| Machine | Network |
|---|---|
| Kali Linux | Host-only |
| Metasploitable 2 | Host-only |

Both machines must use the same Host-only virtual network.

For this training environment, do not enable a second NAT or Bridged adapter on Metasploitable.

---

## 8. Practical Lab 1 – Inspect Your Kali Environment

Start your Kali Linux VM.

Open the terminal.

Check your username:

```bash
whoami
```

Check the operating system:

```bash
cat /etc/os-release
```

Check the kernel:

```bash
uname -r
```

Check system architecture:

```bash
uname -m
```

Check network interfaces:

```bash
ip addr
```

Check the routing table:

```bash
ip route
```

### Record Your Observations

Write down:

1. Kali Linux version.
2. Current username.
3. Kernel version.
4. Network interface name.
5. Current IP address.
6. Network configuration.

---

## 9. Practical Lab 2 – Create the Module 13 Workspace

Run:

```bash
mkdir -p ~/practical-labs/module13
cd ~/practical-labs/module13
```

Create a directory for this lesson:

```bash
mkdir lesson01
cd lesson01
```

Create the following files:

```bash
touch environment.txt
touch lab-architecture.txt
touch methodology.txt
```

Verify:

```bash
ls -l
```

Record your observations in the appropriate files.

---

## 10. Practical Lab 3 – Check Security Testing Tools

Check whether Nmap is available:

```bash
nmap --version
```

Check whether Metasploit is installed:

```bash
msfconsole --version
```

If Metasploit is not installed, record that observation. Do not install software from unofficial sources.

Check curl:

```bash
curl --version
```

Check your network utilities:

```bash
ip --version
```

Record the available tools.

We will install or troubleshoot missing tools only when necessary.

---

## 11. Practical Lab 4 – Understand the Target and Attacker Roles

Create a short document:

```bash
nano lab-architecture.txt
```

Write:

```text
LAB NAME: MODULE 13 METASPLOITABLE LAB

ATTACKING MACHINE:
Kali Linux

TARGET MACHINE:
Metasploitable 2

VIRTUALIZATION:
VMware Workstation

NETWORK MODE:
Host-only

PURPOSE:
Authorized penetration-testing education

SCOPE:
My own isolated virtual machines

PROHIBITED:
Public network scanning
Unauthorized exploitation
Testing third-party systems
```

Save the document.

---

## 12. Knowledge Check

Answer these questions:

1. What is Metasploitable?
2. Why is Metasploitable intentionally vulnerable?
3. What is the role of Kali Linux?
4. What is the role of Metasploitable?
5. What is the difference between NAT and Host-only networking?
6. Why should Metasploitable not be exposed to the public internet?
7. What is the purpose of Nmap?
8. What is the purpose of Metasploit?
9. What is reconnaissance?
10. Why is evidence collection important in penetration testing?

---

## 13. Lesson Summary

In this lesson, you learned:

- What Metasploitable is.
- Why vulnerable virtual machines are useful for cybersecurity training.
- The roles of Kali Linux and Metasploitable.
- The basic penetration-testing methodology.
- The purpose of the Metasploit Framework.
- Why isolated virtual networks are essential.
- How to inspect your Kali environment.
- How to prepare your Module 13 workspace.

## 14. Professional References

1. Rapid7 Metasploit Documentation: https://docs.metasploit.com/
2. Rapid7 Metasploitable 2: https://github.com/rapid7/metasploitable3
3. Kali Linux Documentation: https://www.kali.org/docs/
4. Nmap Reference Guide: https://nmap.org/book/man.html

---

# END OF LESSON 01
