
# MODULE 14 – DIGITAL FORENSICS

# LESSON 08: MEMORY FORENSICS FUNDAMENTALS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain memory forensics.
2. Understand RAM and volatile evidence.
3. Explain memory acquisition.
4. Identify suspicious information that may exist in RAM.
5. Understand processes, network connections, and loaded modules.
6. Identify common memory forensic tools.
7. Understand the Volatility Framework.
8. Perform a safe memory-forensics preparation exercise.

---

## 2. What Is Memory Forensics?

Memory forensics is the process of collecting and examining data stored in a computer's volatile memory, particularly Random Access Memory (RAM).

RAM temporarily stores information that the operating system and running applications need.

Unlike a normal hard drive, RAM is generally volatile.

This means its contents are usually lost when power is removed.

### Simple Example

Imagine your computer is a classroom.

- The hard drive is the library where information is stored.
- RAM is the desk where students place the books they are currently using.
- The CPU is the student actively working with those books.

When the computer is running, RAM may contain useful information about its current activities.

---

## 3. What Is RAM?

RAM stands for Random Access Memory.

It temporarily holds information required by active processes.

Examples include:

- Running applications.
- Operating system processes.
- Active network-related information.
- Loaded libraries and modules.
- Portions of documents being edited.
- Temporary application data.

### Important Characteristics

1. RAM is volatile.
2. RAM provides fast access to active data.
3. Its contents change continuously.
4. It can contain evidence unavailable in ordinary disk files.

---

## 4. What Is Volatile Evidence?

Volatile evidence is information that may disappear when a system shuts down or changes state.

Examples:

| Evidence | Description |
|---|---|
| Running processes | Applications currently executing |
| Network connections | Active communication sessions |
| Logged-in users | Active user sessions |
| Loaded modules | Components currently loaded into processes |
| Temporary data | Data held in memory |
| Open files | Files currently accessed by applications |

### Why Is Volatile Evidence Important?

Suppose an investigator suspects that a computer is communicating with a malicious server.

A disk examination may reveal installed applications and saved files.

A memory examination may provide additional evidence about active processes and network connections at the time of acquisition.

Memory analysis can therefore complement disk forensics.

---

## 5. Memory Acquisition

Memory acquisition is the process of collecting a computer's memory for forensic examination.

A memory image is a file containing captured memory data.

### General Acquisition Process

1. Identify the system under investigation.
2. Obtain authorization.
3. Document the system's current condition.
4. Select an approved acquisition tool.
5. Capture memory using a suitable procedure.
6. Record the acquisition details.
7. Calculate a cryptographic hash.
8. Preserve the original image.
9. Analyze a working copy.

### Important Considerations

Memory acquisition can change the system's state because acquisition software itself executes and uses resources.

A forensic investigator should document the tool, version, time, system condition, and any known limitations.

Never claim that an acquisition is completely non-invasive.

---

## 6. Memory Forensic Evidence

Memory can provide information about several areas.

### 6.1 Process Analysis

A process is a running instance of a program.

Examples:

- Browser processes.
- Command-line applications.
- System services.
- Background applications.

Investigators examine process information to identify unusual or suspicious activity.

### 6.2 Network Connection Analysis

Memory examination may reveal information about active or recently represented network activity.

Examples include:

- Local IP addresses.
- Remote IP addresses.
- Network ports.
- Connection states.
- Process associations.

### 6.3 Loaded Modules

Modules are components loaded into processes or the operating system.

Investigators may examine loaded modules for unexpected or suspicious components.

### 6.4 Command History and Artifacts

Some command-related information or application artifacts may exist in memory.

However, not every command or activity will necessarily be recoverable.

---

## 7. Memory Forensics Tools

| Tool | Purpose |
|---|---|
| Volatility 3 | Memory image analysis |
| WinPmem | Windows memory acquisition |
| DumpIt | Memory acquisition |
| Magnet RAM Capture | Windows memory acquisition |
| LiME | Linux memory acquisition |
| AVML | Linux memory acquisition |

Tool availability, supported operating systems, and compatibility vary.

Only use acquisition tools from trusted official sources and follow their documentation.

---

## 8. Introduction to Volatility Framework

Volatility is an open-source framework used to analyze memory images.

Volatility 3 supports memory analysis across several operating-system platforms.

It uses plugins to examine different types of information.

### Common Volatility 3 Plugins

| Plugin | Purpose |
|---|---|
| windows.info | Windows memory and kernel information |
| windows.pslist | List processes |
| windows.pstree | Display process relationships |
| windows.psscan | Scan for process structures |
| windows.netscan | Examine network-related structures |
| windows.dlllist | Examine loaded DLLs |
| linux.pslist | List Linux processes |
| linux.bash | Examine available Bash-related artifacts |

Plugin availability depends on the operating system and memory image.

### Why Use Volatility?

It helps investigators examine memory images without relying only on information visible through the normal operating system interface.

---

## 9. Volatility 3 Installation

First, check your Python version:

```bash
python3 --version
```

Create a dedicated virtual environment:

```bash
python3 -m venv ~/volatility-env
```

Activate it:

```bash
source ~/volatility-env/bin/activate
```

Install Volatility 3 using its official Python package:

```bash
python -m pip install volatility3
```

Check the installation:

```bash
vol -h
```

If the `vol` command is unavailable, try:

```bash
python -m volatility3 -h
```

You can leave the virtual environment using:

```bash
deactivate
```

---

## 10. Memory Image Analysis: General Workflow

Assume an authorized investigator has received a memory image named:

```text
memory.raw
```

The investigation should follow a controlled process.

### Step 1: Record the Image

```bash
ls -lh memory.raw
```

### Step 2: Calculate SHA-256

```bash
sha256sum memory.raw
```

### Step 3: Record the Image Type

```bash
file memory.raw
```

### Step 4: Examine Available Volatility Options

```bash
vol -h
```

### Step 5: Identify the Image Information

For a supported Windows memory image:

```bash
vol -f memory.raw windows.info
```

### Step 6: Examine Processes

```bash
vol -f memory.raw windows.pslist
```

### Step 7: Examine Process Relationships

```bash
vol -f memory.raw windows.pstree
```

### Step 8: Examine Network Artifacts

```bash
vol -f memory.raw windows.netscan
```

These commands are examples for an actual supported memory image. They will not produce meaningful forensic findings when run against an ordinary text file.

---

## 11. Process Analysis: Example

Consider this fictional process list:

```text
PID    PPID    IMAGE

500    400     System
820    500     smss.exe
900    820     csrss.exe
1100   1000    explorer.exe
1250   1100    chrome.exe
1300   1100    notepad.exe
```

PID means Process Identifier.

PPID means Parent Process Identifier.

The parent process is the process that created another process.

Investigators may examine:

- Unusual process names.
- Unexpected parent-child relationships.
- Processes running from unusual locations.
- Unexpected privileges.
- Processes that do not match normal system activity.

A suspicious-looking process is not automatically malicious. Findings require validation.

---

## 12. Network Artifact Analysis

Consider this fictional example:

```text
Process: browser.exe
Local Address: 192.168.1.20
Local Port: 52340
Remote Address: 203.0.113.25
Remote Port: 443
State: ESTABLISHED
```

This example represents a process with an established connection to a remote address.

Investigators may ask:

1. Which process owns the connection?
2. What service is associated with the remote port?
3. Is the remote address expected?
4. Does the connection match normal application activity?
5. Is there supporting evidence in firewall or network logs?

The address `203.0.113.25` belongs to a documentation-only IPv4 range. It is not presented as a real malicious server.

---

## 13. Memory Forensics Limitations

Memory analysis has important limitations:

1. Memory changes continuously.
2. Acquisition can alter system state.
3. Some information may be overwritten.
4. Memory images can be incomplete.
5. Different operating-system versions require compatible analysis support.
6. Encryption may limit access to useful information.
7. Not every suspicious artifact indicates malicious activity.
8. A memory image may contain sensitive information such as credentials or private data.

Handle memory images as confidential evidence.

---

## 14. Practical Lab: Memory Forensics Preparation

### Objective

Prepare a safe memory-forensics workspace, install Volatility 3, and learn how to preserve and document a memory image.

This exercise does not require capturing your Windows host's RAM or acquiring a physical machine.

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/practical-labs/module14/lab08/evidence
mkdir -p ~/practical-labs/module14/lab08/reports
cd ~/practical-labs/module14/lab08
```

### Step 2: Create a Training Evidence File

```bash
echo "Memory forensics training evidence" > evidence/training-note.txt
```

### Step 3: Calculate Its Hash

```bash
sha256sum evidence/training-note.txt
```

### Step 4: Record Its File Information

```bash
file evidence/training-note.txt
stat evidence/training-note.txt
```

### Step 5: Install Volatility 3 in a Virtual Environment

```bash
python3 -m venv ~/volatility-env
source ~/volatility-env/bin/activate
python -m pip install volatility3
```

### Step 6: Verify Volatility

```bash
vol -h
```

If unavailable:

```bash
python -m volatility3 -h
```

### Step 7: Create a Memory Analysis Preparation Report

```bash
cat > reports/memory-analysis-preparation.txt <<'EOF'
MEMORY FORENSICS PREPARATION REPORT

Investigation ID: MEM-001

Investigation Type:
Memory Forensics Training

Evidence:
Self-created training note.

Evidence Location:
evidence/training-note.txt

Tool:
Volatility 3

Purpose:
Preparation for memory image analysis.

Integrity:
SHA-256 hash calculated.

Acquisition:
No physical memory acquisition performed.

Analysis:
No actual memory image analyzed.

Limitations:
The training file is not a memory image.
Volatility cannot extract meaningful memory artifacts from it.

Scope:
Authorized training environment only.
EOF
```

### Step 8: Review Your Workspace

```bash
find . -maxdepth 3 -type f
```

Expected:

```text
./evidence/training-note.txt
./reports/memory-analysis-preparation.txt
```

Your Volatility environment is stored separately in:

```text
~/volatility-env
```

---

## 15. Practical Investigation Challenge

Answer these questions:

1. What is memory forensics?
2. Why is RAM considered volatile?
3. Explain memory acquisition.
4. Name four types of evidence that may be found in RAM.
5. What is the purpose of Volatility 3?
6. What is the difference between `pslist` and `pstree`?
7. Why is memory acquisition not completely non-invasive?
8. Why should memory images be protected?
9. Why must investigators validate suspicious process activity?
10. What is the difference between disk forensics and memory forensics?

---

## 16. Knowledge Check

**Question 1:** What does RAM stand for?

A. Random Access Memory  
B. Read Access Module  
C. Remote Application Manager  
D. Random Application Method

**Question 2:** Why is RAM important in digital investigations?

A. It contains only permanent files  
B. It may contain evidence of active system activity  
C. It replaces the hard drive  
D. It stores only operating-system updates

**Question 3:** Which tool is used for memory analysis?

A. Volatility 3  
B. UFW  
C. Nano  
D. Traceroute

**Question 4:** Which Volatility plugin lists Windows processes?

A. windows.pslist  
B. windows.info  
C. windows.netscan  
D. windows.dlllist

**Question 5:** What should an investigator do after acquiring a memory image?

A. Modify the original image  
B. Record and verify its integrity  
C. Delete the acquisition logs  
D. Upload it publicly

---

## 17. Lesson Completion Checklist

- [ ] I understand memory forensics.
- [ ] I understand RAM and volatile evidence.
- [ ] I understand memory acquisition.
- [ ] I can identify common memory artifacts.
- [ ] I understand the purpose of Volatility 3.
- [ ] I know common Volatility plugins.
- [ ] I created my Module 14 Lesson 08 workspace.
- [ ] I installed or checked Volatility 3.
- [ ] I created a preparation report.
- [ ] I completed the knowledge check.

---

## 18. Key Takeaway

Memory forensics helps investigators understand what a computer was doing while it was running.

Disk forensics examines persistent storage, while memory forensics examines volatile information that may disappear when a system shuts down.

**Remember: In digital forensics, time matters. Volatile evidence should be considered early in an investigation and preserved using a documented, authorized procedure.**
