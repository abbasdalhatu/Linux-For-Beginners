
# MODULE 13 – METASPLOITABLE LABS

# LESSON 07: METASPLOIT FRAMEWORK FUNDAMENTALS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what the Metasploit Framework is.
2. Understand the architecture of Metasploit.
3. Identify its major components.
4. Start and navigate the Metasploit console.
5. Search for modules.
6. Understand exploits, payloads, auxiliary modules, and encoders.
7. Inspect module information without executing exploits.
8. Understand safe and responsible use of Metasploit.

---

## 2. What Is Metasploit?

The Metasploit Framework is an open-source penetration testing platform used by security professionals to research, test, and validate security weaknesses.

It provides tools for:

- Security testing.
- Exploit research.
- Vulnerability validation.
- Payload testing.
- Security assessment.
- Penetration testing in authorized environments.

Metasploit is widely used in cybersecurity training and professional penetration testing.

Official website:

https://www.metasploit.com/

---

## 3. Understanding the Metasploit Architecture

Metasploit consists of different components that work together.

### A. MSFconsole

MSFconsole is the primary command-line interface for interacting with Metasploit.

It allows users to:

- Search modules.
- Inspect module information.
- Configure module options.
- Manage authorized testing activities.

### B. Modules

Modules are components that perform specific tasks.

Common module categories include:

| Module | Purpose |
|---|---|
| Exploit | Attempts to use a known vulnerability |
| Auxiliary | Performs supporting security tasks |
| Payload | Defines an action or code executed after successful exploitation |
| Post | Performs authorized post-exploitation activities |
| Encoder | Transforms payload representations |
| NOP | Provides no-operation instructions used in certain exploit contexts |

### C. Exploit

An exploit is code or a technique that attempts to take advantage of a vulnerability.

### D. Payload

A payload is the code or action associated with an exploit.

For example, a payload may establish a controlled connection after an exploit succeeds.

### E. Session

A session represents an established interaction with a target following a successful operation.

For this lesson, you will learn these concepts without creating sessions or executing exploits.

---

## 4. Metasploit Module Naming

Metasploit modules are organized by category and purpose.

Example module path:

```text
exploit/multi/handler
```

The path contains:

- `exploit`: module category.
- `multi`: platform or environment classification.
- `handler`: module name.

Another example:

```text
auxiliary/scanner/ssh/ssh_version
```

This represents an auxiliary scanner module associated with SSH version detection.

Module availability may differ between Metasploit versions.

---

## 5. Practical Lab 1 – Verify Metasploit Installation

Open your Kali Linux terminal.

Check whether Metasploit is installed:

```bash
msfconsole --version
```

If installed, you should see version information.

If the command is unavailable, check:

```bash
command -v msfconsole
```

Do not install software from unofficial websites or run unknown installation scripts.

Use the official Kali package repository if installation is necessary.

---

## 6. Practical Lab 2 – Start MSFconsole

Start the Metasploit console:

```bash
msfconsole
```

The first startup may take some time.

You should eventually see a prompt similar to:

```text
msf6 >
```

This indicates that you are inside the Metasploit console.

Exit safely:

```text
exit
```

You can reopen it whenever needed.

---

## 7. Practical Lab 3 – Learn Basic Console Commands

Start Metasploit again:

```bash
msfconsole
```

Try the following commands individually.

### Display Help

```text
help
```

Displays available commands and descriptions.

### Display Version

```text
version
```

Displays Metasploit version information.

### Display Current Context

```text
get
```

Shows available configuration information in the current context where supported.

### Display Module Search Help

```text
search
```

Displays search usage information.

### Clear the Console

```text
clear
```

Clears the visible console output.

### Exit

```text
exit
```

Closes the Metasploit console.

---

## 8. Practical Lab 4 – Search for Modules

Start Metasploit:

```bash
msfconsole
```

Search for an SSH version scanner:

```text
search ssh_version
```

You may see a module similar to:

```text
auxiliary/scanner/ssh/ssh_version
```

This module is designed to gather SSH version information.

Search for FTP-related modules:

```text
search type:auxiliary ftp
```

Search for HTTP-related modules:

```text
search type:auxiliary http
```

Search results depend on the installed Metasploit version.

**Important:** Searching for a module does not execute it.

---

## 9. Practical Lab 5 – Inspect a Module

Select the SSH version scanner:

```text
use auxiliary/scanner/ssh/ssh_version
```

Inspect its information:

```text
info
```

You should see information such as:

- Module name.
- Description.
- Author.
- References.
- Available options.
- Module category.

Return to the general console:

```text
back
```

This clears the selected module context.

### Important

Selecting a module does not necessarily execute it.

For this lesson, do not configure a target or run the module.

---

## 10. Practical Lab 6 – Understand Module Options

A Metasploit module may contain configurable options.

Examples include:

| Option | Meaning |
|---|---|
| RHOSTS | Remote target address or addresses |
| RPORT | Remote target port |
| LHOST | Local address used by certain payloads |
| LPORT | Local port used by certain payloads |
| THREADS | Number of concurrent workers, where supported |

The options available depend on the selected module.

You can inspect options with:

```text
show options
```

You can inspect compatible payloads, where relevant, with:

```text
show payloads
```

You do not need to configure or execute a module to understand its options.

---

## 11. Practical Lab 7 – Understand Auxiliary Modules

Auxiliary modules perform supporting security assessment tasks.

Examples include:

- Service discovery.
- Version detection.
- Protocol information gathering.
- Configuration checks.
- Certain vulnerability checks.

Search for an HTTP-related auxiliary module:

```text
search type:auxiliary http
```

Inspect a suitable information-gathering module using:

```text
use MODULE_PATH
```

Then:

```text
info
```

Replace `MODULE_PATH` with an actual module path returned by your search.

Do not run the module.

Return to the main prompt:

```text
back
```

---

## 12. Practical Lab 8 – Understand Payload Types

Payloads describe what happens after an exploit succeeds.

Common categories include:

### A. Singles

A self-contained payload that performs a specific action.

### B. Stagers

Small initial components that establish communication or prepare a larger payload.

### C. Stages

Larger components delivered after a stager has established communication.

### D. Meterpreter

An advanced Metasploit payload environment that can provide an interactive session.

Meterpreter is commonly used in authorized penetration testing labs to demonstrate post-exploitation concepts.

For this lesson, understand the architecture only. Do not generate payloads, establish sessions, or perform post-exploitation actions.

---

## 13. Practical Lab 9 – Create a Metasploit Learning Record

Create your workspace:

```bash
mkdir -p ~/practical-labs/module13/lab07
cd ~/practical-labs/module13/lab07
```

Create:

```bash
nano metasploit-notes.md
```

Use this template:

```markdown
# METASPLOIT FRAMEWORK FUNDAMENTALS

## Installation

Metasploit Version:

## Framework Components

### MSFconsole

Purpose:

### Exploit

Purpose:

### Payload

Purpose:

### Auxiliary

Purpose:

### Post

Purpose:

## Module Research

Module Name:
Module Category:
Description:
Available Options:

## Important Commands

- msfconsole:
- help:
- version:
- search:
- use:
- info:
- show options:
- back:
- exit:

## Safety Notes

Record the importance of authorization and isolated testing.

## Conclusion

Summarize what you learned.
```

Save the file.

---

## 14. Practical Lab 10 – Review Your Understanding

Start Metasploit and perform the following sequence:

```text
help
version
search ssh_version
use auxiliary/scanner/ssh/ssh_version
info
show options
back
exit
```

Observe the output of each command.

Do not execute any module.

Record any important observations in your learning notes.

---

## 15. Knowledge Check

Answer these questions:

1. What is the Metasploit Framework?
2. What is MSFconsole?
3. What is a Metasploit module?
4. What is an exploit?
5. What is a payload?
6. What is an auxiliary module?
7. What is Meterpreter?
8. What does the `search` command do?
9. What is the purpose of the `info` command?
10. Why should penetration testing only be conducted with authorization?

---

## 16. Lesson Summary

You have learned:

- Metasploit fundamentals.
- Metasploit architecture.
- MSFconsole.
- Module categories.
- Exploits and payloads.
- Auxiliary modules.
- Module searching.
- Module inspection.
- Basic console navigation.
- Safe framework familiarization.

## 17. Professional References

1. Metasploit Official Website: https://www.metasploit.com/
2. Metasploit Documentation: https://docs.metasploit.com/
3. Rapid7 Metasploit Framework: https://github.com/rapid7/metasploit-framework
4. Kali Linux Documentation: https://www.kali.org/docs/

---

# END OF LESSON 07
