
# MODULE 15 – CAPSTONE PROJECT

# LESSON 01: CAPSTONE INTRODUCTION AND PROJECT PLANNING

## 1. Project Overview

Welcome to your final practical project.

Throughout the previous 14 modules, you learned Linux administration, networking, cybersecurity, vulnerability assessment, web security, penetration-testing fundamentals, and digital forensics.

Now, you will combine those skills into one structured cybersecurity project.

### Project Title

**Isolated Cybersecurity Assessment, Defense and Digital Forensics Lab**

### Project Objective

Design, configure, assess, secure, monitor, and document an isolated computer network using your virtual laboratory.

The project will simulate the work of a cybersecurity professional responsible for assessing a system, identifying security weaknesses, applying defensive improvements, monitoring activity, and investigating digital evidence.

---

## 2. Project Background

Organizations depend on computer systems to store information, communicate, and deliver services.

However, systems can experience security problems such as:

- Weak authentication.
- Unnecessary open ports.
- Misconfigured services.
- Outdated software.
- Inadequate firewall rules.
- Suspicious network activity.
- Poor evidence management.
- Insufficient monitoring.

A cybersecurity professional must understand how to identify these weaknesses and recommend appropriate protections.

In this project, you will work in an isolated training environment to simulate these responsibilities.

---

## 3. Project Scope

The project will focus on five major security areas.

### Area 1: System and Network Discovery

You will:
- Identify lab machines.
- Inspect IP configurations.
- Discover authorized lab hosts.
- Identify open ports.
- Examine running services.
- Document the network architecture.

### Area 2: Security Assessment

You will:
- Identify potential vulnerabilities.
- Examine service configurations.
- Review security risks.
- Prioritize weaknesses.
- Produce a vulnerability register.

### Area 3: System Hardening

You will:
- Review system security settings.
- Apply appropriate firewall rules.
- Examine user permissions.
- Review unnecessary services.
- Recommend secure configurations.

### Area 4: Monitoring and Detection

You will:
- Examine system logs.
- Monitor network activity.
- Identify suspicious patterns in controlled training data.
- Document security events.
- Recommend detection improvements.

### Area 5: Digital Forensics and Reporting

You will:
- Create and preserve training evidence.
- Calculate SHA-256 hashes.
- Examine metadata.
- Build an investigation timeline.
- Prepare a professional forensic report.

---

## 4. Project Environment

Your project will use an isolated virtual laboratory.

### Planned Architecture

| Component | Purpose |
|---|---|
| Windows 11 host | Main computer and project documentation |
| Kali Linux VM | Security assessment and analysis workstation |
| Metasploitable 2 VM | Intentionally vulnerable training target |
| VMware Host-only Network | Isolated communication between lab VMs |

### Important Network Rule

Metasploitable 2 must remain isolated from your normal network.

Use VMware Host-only networking for the isolated assessment environment.

Use NAT separately when Kali requires internet access for updates.

Do not use Bridged networking for the vulnerable target.

Do not scan public IP addresses or systems outside your authorized lab.

---

## 5. Project Problem Statement

The project addresses the following simulated problem:

"An organization operates a computer system that may contain unnecessary network services, weak configurations, and insufficient monitoring. A cybersecurity team has been assigned to assess the system, identify security weaknesses, improve its defensive configuration, monitor activity, and document the results."

Your responsibility is to demonstrate a structured security assessment and defensive investigation.

---

## 6. Project Aim

The main aim is to demonstrate the practical application of cybersecurity knowledge through an isolated laboratory assessment.

### Specific Objectives

By the end of the project, you should be able to:

1. Design a documented virtual cybersecurity laboratory.
2. Identify authorized hosts in an isolated network.
3. Perform controlled network and service enumeration.
4. Identify and prioritize security weaknesses.
5. Apply appropriate defensive configurations.
6. Examine system and network logs.
7. Identify suspicious activity in training evidence.
8. Preserve and verify digital evidence.
9. Automate selected security administration tasks.
10. Produce a professional cybersecurity investigation report.

---

## 7. Project Requirements

### Hardware Requirements

Your current laptop:

- Dell Latitude 7320.
- Intel Core i7.
- 16 GB RAM.
- 256 GB SSD.
- Windows 11 Pro.

Because your laptop has limited memory and storage for multiple virtual machines, the project will use modest VM allocations.

Do not run all virtual machines simultaneously unless performance and available memory allow it.

### Software Requirements

| Software | Purpose |
|---|---|
| Windows 11 | Host operating system |
| VMware Workstation | Virtual machine management |
| Kali Linux | Security assessment |
| Metasploitable 2 | Vulnerable training target |
| Nmap | Network discovery |
| Wireshark | Network traffic analysis |
| UFW | Firewall management |
| Volatility 3 | Memory analysis preparation |
| Python/Bash | Security automation |

Only install software when a particular project lesson requires it.

---

## 8. Project Deliverables

At the end of this capstone, you should have the following outputs:

| No. | Deliverable | Description |
|---|---|---|
| 1 | Project plan | Scope, objectives, and requirements |
| 2 | Network architecture | Lab topology and IP addressing |
| 3 | Discovery report | Hosts, ports, and services |
| 4 | Security assessment | Identified weaknesses and risk ratings |
| 5 | Hardening report | Defensive improvements |
| 6 | Monitoring report | Security events and detection findings |
| 7 | Evidence package | Preserved and hashed training evidence |
| 8 | Automation scripts | Selected security administration tasks |
| 9 | Final investigation report | Findings, methods, limitations, recommendations |
| 10 | Final assessment | Demonstration of project knowledge |

---

## 9. Project Phases

The project will be completed in ten lessons.

| Lesson | Project Phase |
|---|---|
| 01 | Introduction and Planning |
| 02 | Lab Environment and Architecture |
| 03 | System Discovery and Enumeration |
| 04 | Security Assessment and Analysis |
| 05 | System Hardening and Remediation |
| 06 | Network Monitoring and Detection |
| 07 | Digital Forensics and Evidence Collection |
| 08 | Security Automation and Scripting |
| 09 | Final Investigation and Reporting |
| 10 | Capstone Final Practical Assessment |

Each lesson builds on the previous one.

---

## 10. Project Constraints and Safety

The following rules apply throughout the capstone:

1. Only test systems you own or have explicit authorization to assess.
2. Keep vulnerable virtual machines isolated.
3. Do not perform destructive exploitation.
4. Do not disable Windows security protections.
5. Do not execute unknown scripts as root.
6. Preserve evidence before analysis.
7. Document commands and important findings.
8. Use snapshots before configuration changes.
9. Separate original evidence from working copies.
10. Clearly identify simulated findings in your reports.

---

## 11. Project Success Criteria

The project will be considered successful when you can demonstrate:

- A correctly documented isolated laboratory.
- Successful discovery of your authorized training machines.
- A structured assessment of services and security weaknesses.
- Appropriate defensive improvements.
- Basic network monitoring and event analysis.
- Proper evidence preservation and integrity verification.
- At least one useful security automation script.
- A clear and professionally structured final report.

The project is about demonstrating a complete security workflow, not simply running tools.

---

## 12. Initial Project Planning Exercise

Before moving to Lesson 02, create a project planning document.

Run:

```bash
mkdir -p ~/practical-labs/module15
cd ~/practical-labs/module15
```

Create the project file:

```bash
cat > project-plan.txt <<'EOF'
CAPSTONE PROJECT PLAN

Project Title:
Isolated Cybersecurity Assessment, Defense and Digital Forensics Lab

Project Type:
Practical Cybersecurity Project

Project Environment:
Windows 11 Host
Kali Linux Virtual Machine
Metasploitable 2 Virtual Machine
VMware Host-only Network

Project Aim:
To assess, secure, monitor, and investigate an isolated
cybersecurity training environment.

Main Activities:
1. Lab architecture and configuration.
2. Network discovery.
3. Security assessment.
4. System hardening.
5. Network monitoring.
6. Digital forensics.
7. Security automation.
8. Investigation and reporting.

Project Scope:
Authorized isolated virtual machines only.

Primary Tools:
Nmap, Wireshark, UFW, Volatility 3, Bash and Python.

Expected Outcome:
A documented cybersecurity assessment and defensive
investigation project.

Project Status:
Planning phase.
EOF
```

Review your document:

```bash
cat project-plan.txt
```

---

## 13. Project Completion Checklist

- [ ] I understand the capstone project.
- [ ] I understand the project objectives.
- [ ] I understand the project scope.
- [ ] I understand the virtual lab architecture.
- [ ] I understand the safety requirements.
- [ ] I understand the ten project phases.
- [ ] I understand the expected deliverables.
- [ ] I created my project workspace.
- [ ] I created my project plan.

---

## 14. Key Takeaway

A professional cybersecurity project requires more than technical tools.

It requires planning, authorization, architecture, assessment, defensive improvements, monitoring, evidence preservation, automation, and reporting.

**Your mission:** Demonstrate that you can approach a cybersecurity problem systematically, investigate it responsibly, improve security, and communicate your findings professionally.
