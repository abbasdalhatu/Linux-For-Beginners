# Module 01: Linux Fundamentals

# Lesson 08 – Open Source and GNU

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 08 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

One of the most important ideas behind Linux is **open-source software**.

Unlike proprietary software, whose source code is generally controlled by its owner, open-source software makes its source code available under a licence that permits users to use, inspect, modify, and redistribute it according to the licence terms.

Linux became successful partly because of the open-source community and the GNU Project.

The GNU Project helped develop many of the tools and utilities used in Linux-based operating systems today.

To understand Linux properly, you need to understand three important concepts:

1. Open-source software.
2. The GNU Project.
3. The relationship between GNU software and the Linux kernel.

These concepts are also important in cybersecurity because security researchers often inspect open-source code to understand how software works and identify vulnerabilities.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Define open-source software.
2. Explain the meaning of source code.
3. Understand the difference between open-source and proprietary software.
4. Explain the history and purpose of the GNU Project.
5. Identify Richard Stallman's contribution to free software.
6. Explain the GNU General Public License.
7. Understand the relationship between GNU and Linux.
8. Explain why open-source software is important in cybersecurity.

## 3. What Is Source Code?

Source code is the human-readable instructions written by programmers to create software.

Programmers write source code using programming languages such as:

- Python.
- C.
- C++.
- Java.
- JavaScript.
- Go.
- Rust.

Example of Python source code:

```python
print("Hello, Linux!")
```

This instruction tells Python to display the text:

```text
Hello, Linux!
```

Source code can be read, studied, modified, and compiled or interpreted depending on the programming language and software design.

### Why source code matters

Source code helps developers understand how a program works.

For example, a cybersecurity researcher may inspect source code to:

- Identify programming mistakes.
- Find security vulnerabilities.
- Understand how authentication works.
- Examine how data is processed.
- Improve software security.

## 4. What Is Open-Source Software?

Open-source software is software distributed under a licence that allows users to access its source code and exercise specified rights, including modifying and redistributing it.

Open-source software is not simply software that is free to download.

The licence is important because it defines what users are legally permitted to do.

### Main characteristics of open-source software

**1. Source code availability**

Users can access the source code under the software's licence terms.

**2. Modification**

Users may modify the source code when the licence permits it.

**3. Redistribution**

Users may redistribute the original software or modified versions according to the licence.

**4. Collaboration**

Developers from different locations can contribute improvements.

**5. Transparency**

Users and researchers can inspect how the software is designed and implemented.

### Examples of open-source software

| Software | Purpose |
|---|---|
| Linux | Operating-system kernel |
| Firefox | Web browser |
| VLC | Media player |
| LibreOffice | Office productivity suite |
| GIMP | Image editing |
| Wireshark | Network protocol analysis |
| Nmap | Network discovery and security auditing |
| Python | Programming language implementation |

Open-source licences differ, so always check the licence before modifying or redistributing software.

## 5. What Is Proprietary Software?

Proprietary software is software controlled by an individual, company, or organisation under terms that restrict how users may access, modify, or redistribute it.

Its source code is generally not publicly available.

Examples include:

- Microsoft Windows.
- Microsoft Office.
- Adobe Photoshop.
- Some commercial security applications.

### Open-source vs proprietary software

| Feature | Open Source | Proprietary |
|---|---|---|
| Source code | Available under the licence | Generally restricted |
| Modification | Permitted under licence terms | Usually restricted |
| Redistribution | Permitted under licence terms | Usually restricted |
| Development | May involve public collaboration | Usually controlled by the owner |
| Licensing | Open-source licences | Proprietary licences |
| Transparency | Source code can be inspected | Internal source code is generally unavailable |

**Important:** Open-source does not automatically mean free of cost, and proprietary software is not necessarily expensive. These terms describe licensing and access, not simply price.

## 6. Understanding the GNU Project

The GNU Project is a free-software project launched by Richard Stallman in 1983.

Its goal was to develop a complete Unix-like operating system made entirely from free software.

GNU is a recursive acronym meaning:

**GNU's Not Unix.**

The project developed many essential operating-system tools and utilities.

Examples include:

- GNU Compiler Collection (GCC).
- GNU Bash.
- GNU Coreutils.
- GNU Debugger (GDB).
- GNU Make.
- GNU Binutils.

These tools are widely used in software development and Linux-based systems.

### Why was GNU created?

Before the GNU Project, much software was distributed under restrictive licences.

Richard Stallman wanted users to have the freedom to run, study, modify, and share software.

The GNU Project therefore focused on developing software that respected these freedoms.

## 7. Richard Stallman and the Free Software Movement

<text>Richard Stallman</text> is an American programmer and the founder of the GNU Project.

He announced the GNU Project in September 1983 and founded the Free Software Foundation (FSF) in 1985.

His work helped establish the modern free-software movement.

### The four essential software freedoms

The Free Software Foundation describes four essential freedoms:

| Freedom | Explanation |
|---|---|
| Freedom 0 | The freedom to run the program for any purpose |
| Freedom 1 | The freedom to study how the program works and modify it |
| Freedom 2 | The freedom to redistribute copies |
| Freedom 3 | The freedom to distribute modified versions |

Access to the source code is necessary for freedoms 1 and 3.

### Why these freedoms matter

Imagine you are using a program for network monitoring.

If the source code is available under an appropriate free-software licence, you can study how it processes network traffic, modify it for your needs, and share your improvements while respecting the licence.

This encourages collaboration and technical learning.

## 8. GNU General Public License (GPL)

The GNU General Public License, commonly called the GPL, is a free-software licence.

It grants users important freedoms while requiring certain conditions when distributing covered software or modified versions.

The GPL is known as a **copyleft licence**.

### What is copyleft?

Copyleft is a licensing approach that allows people to use, modify, and redistribute software while preserving specified freedoms for downstream recipients.

For example, when distributing a modified version of GPL-covered software, the distributor must comply with the applicable GPL requirements.

These can include providing the corresponding source code under the required conditions.

### Main purposes of the GPL

- Protect users' software freedoms.
- Permit software modification.
- Allow redistribution.
- Encourage sharing of improvements.
- Prevent covered derivatives from being distributed under incompatible restrictions.

### GPL versions

Important versions include:

- GNU GPL version 1 (1989).
- GNU GPL version 2 (1991).
- GNU GPL version 3 (2007).

Different projects use different licence versions. Always check the exact licence attached to a project.

## 9. GNU and Linux: Understanding the Relationship

This is one of the most important concepts in Linux history.

GNU and Linux are related, but they are not the same thing.

### GNU

GNU developed many of the tools, utilities, and components needed for a complete operating-system environment.

Examples include:

- Bash.
- GCC.
- GNU Coreutils.
- GNU Make.
- GNU Debugger.

### Linux

Linux is the operating-system kernel originally created by Linus Torvalds in 1991.

It manages hardware, memory, processes, devices, and other core operating-system resources.

### How they work together

Many Linux distributions combine the Linux kernel with GNU tools and other software.

For example:

```text
Linux-Based Operating System
          |
          v
   Applications
          |
          v
  System Utilities
          |
          v
      GNU Tools
          |
          v
     Linux Kernel
          |
          v
       Hardware
```

This is a simplified conceptual view. Real distributions contain additional libraries, services, drivers, and components, and not every Linux-based system uses GNU utilities.

### Why some people say GNU/Linux

The Free Software Foundation and others use the term GNU/Linux to recognise the contribution of GNU software to systems built around the Linux kernel.

The name highlights the fact that a complete operating-system environment often includes much more than the kernel alone.

**Remember:**

- GNU is a software project.
- Linux is a kernel.
- GNU tools and the Linux kernel are commonly combined in Linux distributions.

## 10. Open Source vs Free Software

Open source and free software are closely related, but they come from different philosophical perspectives.

### Free Software

The free-software movement focuses on users' freedoms.

The word *free* refers primarily to freedom, not necessarily price.

### Open Source

The open-source movement focuses on the practical benefits of making source code available under licences that permit inspection, modification, and redistribution.

### Comparison

| Free Software | Open Source |
|---|---|
| Emphasises user freedom | Emphasises source availability and collaborative development |
| Associated with the GNU Project and FSF | Associated with the Open Source Initiative |
| Focuses strongly on ethical principles | Often emphasises practical development benefits |
| Uses licences that meet the free-software definition | Uses licences that meet the open-source definition |

There is substantial overlap between the two movements. Many licences satisfy both definitions.

## 11. Common Open-Source Licences

Different open-source projects use different licences.

Understanding these licences is important before copying, modifying, or redistributing code.

| Licence | Main Characteristics |
|---|---|
| GNU GPL | Strong copyleft requirements |
| GNU LGPL | More limited copyleft, commonly used for libraries |
| MIT Licence | Permissive licence with relatively few conditions |
| Apache 2.0 | Permissive licence with explicit patent provisions |
| BSD Licences | Permissive licences with conditions that vary by version |

### Permissive licences

Permissive licences generally allow broad reuse, including use in proprietary products, provided their conditions are followed.

Examples include MIT and Apache 2.0.

### Copyleft licences

Copyleft licences require certain freedoms to be preserved when covered software or derivative works are distributed.

The exact requirements depend on the licence.

**Cybersecurity lesson:** Never assume that code found online can be copied and redistributed without checking its licence.

## 12. Why Open Source Is Important in Cybersecurity

Open-source software plays a major role in cybersecurity.

Many security tools are open source, allowing researchers and developers to inspect their implementation and contribute improvements.

### 1. Security auditing

Researchers can inspect source code to identify potential vulnerabilities.

### 2. Transparency

Users can examine how software handles sensitive operations, although source availability alone does not guarantee that anyone has audited it.

### 3. Community collaboration

Developers and security researchers can report bugs, propose fixes, and improve software.

### 4. Customisation

Security professionals can modify tools to support authorised testing and research requirements.

### 5. Learning

Students can study real-world software to understand programming, networking, encryption, and security engineering.

### Examples of open-source cybersecurity tools

| Tool | Purpose |
|---|---|
| Nmap | Network discovery and security auditing |
| Wireshark | Network protocol analysis |
| Metasploit Framework | Security testing framework |
| Snort | Network intrusion detection and prevention |
| Suricata | Network threat detection |
| OpenVAS / Greenbone | Vulnerability assessment |
| tcpdump | Network packet capture |

Open-source tools must still be used responsibly and only within authorised environments.

## 13. Practical Lab: Exploring Open-Source Software

**Lab objective:** Identify open-source components on Kali Linux and inspect package information and software licensing.

Use your Kali Linux virtual machine.

### Part A – Identify your Linux distribution

Run:

```bash
cat /etc/os-release
```

Observe the distribution name and version.

### Part B – Identify the Linux kernel

Run:

```bash
uname -r
```

This displays your running kernel version.

### Part C – Identify GNU Bash

Run:

```bash
bash --version
```

This displays information about your Bash installation.

### Part D – Identify GNU Coreutils

Run:

```bash
ls --version
```

The output should identify the implementation and version of the `ls` utility.

### Part E – Inspect an installed package

Run:

```bash
apt show bash
```

Observe:

- Package name.
- Version.
- Description.
- Maintainer.
- Dependencies.

### Part F – Identify package ownership

Run:

```bash
dpkg -S "$(command -v bash)"
```

This identifies the installed package that owns the Bash executable.

### Part G – Explore copyright and licence information

On Debian-based systems, package copyright information is commonly stored under:

```bash
ls /usr/share/doc/bash/
```

If a copyright file is available, inspect it:

```bash
less /usr/share/doc/bash/copyright
```

Press `q` to exit `less`.

**Important:** The package's copyright file may contain multiple notices and licence conditions. Read the actual licence information instead of assuming every component has the same licence.

## 14. Important Terms

| Term | Meaning |
|---|---|
| Source Code | Human-readable instructions used to create software |
| Open Source | Software distributed under a licence that meets the open-source definition |
| Proprietary Software | Software controlled under restrictive ownership and licensing terms |
| GNU | A free-software project launched in 1983 |
| Richard Stallman | Founder of the GNU Project |
| FSF | Free Software Foundation |
| GPL | GNU General Public License |
| Copyleft | Licensing approach that preserves specified software freedoms |
| Free Software | Software that respects users' essential freedoms |
| Linux Kernel | The core operating-system kernel |
| GCC | GNU Compiler Collection |
| Bash | Bourne Again Shell |
| OSI | Open Source Initiative |

## 15. Knowledge Check

Answer these questions without looking at the lesson.

1. What is source code?
2. Define open-source software.
3. What is the difference between open-source and proprietary software?
4. What is the GNU Project?
5. Who founded the GNU Project?
6. What does GNU stand for?
7. What is the Free Software Foundation?
8. State the four essential software freedoms.
9. What is the GNU General Public License?
10. Explain the meaning of copyleft.
11. What is the difference between GNU and Linux?
12. Why do some people use the term GNU/Linux?
13. Give three examples of open-source licences.
14. Why is open-source software important in cybersecurity?

## 16. Practical Assignment

Complete the following tasks:

1. Explain open-source software in your own words.
2. Write five differences between open-source and proprietary software.
3. Research the history of the GNU Project.
4. Explain the four essential software freedoms.
5. Describe the relationship between GNU and the Linux kernel.
6. Identify five open-source cybersecurity tools.
7. Explain how open-source software helps cybersecurity researchers.
8. Run the practical lab commands and record your observations.
9. Find the licence information for one open-source tool and explain its main conditions.

Save your work as:

`Open-Source-and-GNU-Assignment.md`

## 17. Exam-Ready Summary

Open-source software is software distributed under a licence that allows users to inspect, modify, and redistribute its source code according to specified conditions. The GNU Project was launched by Richard Stallman in 1983 to develop a complete Unix-like operating system based on free software. GNU developed many important tools, including Bash, GCC, and GNU Coreutils. The Linux kernel, created by Linus Torvalds in 1991, manages hardware and system resources. GNU software and the Linux kernel are commonly combined to form complete Linux-based operating systems. The GNU General Public License is a copyleft licence that protects specified software freedoms. Open-source software is important in cybersecurity because it supports transparency, research, collaboration, and security auditing.

## 18. Lesson Conclusion

Open-source software has played a major role in the development of modern computing.

The GNU Project helped establish the free-software movement and developed many tools that are still essential in Linux environments.

The Linux kernel and GNU utilities work together in many Linux distributions, creating powerful operating systems used in servers, software development, education, and cybersecurity.

As a future software engineer and cybersecurity professional, understanding open-source software will help you study existing technologies, contribute to projects, inspect code, and develop your own tools.

**Next Lesson:** Lesson 09 – Linux Advantages and Disadvantages.