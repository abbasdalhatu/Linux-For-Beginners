---
---

# Module 09 – Lesson 11: Understanding Package Dependencies

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define package dependencies.
2. Explain why Linux software requires dependencies.
3. Identify different types of package dependencies.
4. Inspect package dependencies using APT and DPKG.
5. Understand dependency conflicts.
6. Troubleshoot common dependency problems.
7. Explain how dependency management affects Linux security.

---

## 2. What Are Package Dependencies?

A package dependency is another software package that a program needs to function correctly.

Most Linux applications do not operate completely independently. They rely on libraries, utilities, frameworks, or other software components.

For example, imagine installing a photo-editing application.

It may require:
- A graphics library.
- An image-processing library.
- A user-interface toolkit.
- Additional system components.

These required components are called dependencies.

### Simple analogy

Imagine building a computer.

You need:
- A motherboard.
- A processor.
- RAM.
- Storage.
- A power supply.

If an essential component is missing, the computer may not work properly.

Software dependencies work similarly.

---

## 3. Why Do Software Packages Need Dependencies?

Dependencies help software developers avoid rebuilding common functionality for every application.

For example, several applications may use the same graphics library.

Instead of including a separate copy of that library in every application, Linux can install one shared library and allow multiple applications to use it.

Benefits include:

1. Code reuse.
2. Reduced duplication.
3. Easier software maintenance.
4. Consistent shared components.
5. More efficient software distribution.

However, dependencies must be managed carefully because an incorrect or missing dependency can prevent software from working.

---

## 4. Types of Package Dependencies

There are several important dependency relationships.

### A. Depends

A package requires another package to function correctly.

Example:

```text
Application A depends on Library B.
```

If Library B is missing, Application A may not work.

### B. Recommends

A recommended package provides useful functionality but is not always strictly required.

APT normally installs recommended packages unless configured otherwise.

### C. Suggests

A suggested package provides optional functionality.

The main application can generally operate without it.

### D. Pre-Depends

A package must have another package installed and configured before its own installation can proceed.

This is a stricter relationship than an ordinary dependency.

### E. Conflicts

Two packages cannot normally be installed together because their software or files are incompatible.

### F. Breaks

A package declares that certain versions of another package are incompatible with it.

### G. Replaces

A package declares that it can replace files or functionality provided by another package.

---

## 5. Understanding Dependency Chains

Sometimes a package depends on another package, which itself depends on additional packages.

This creates a dependency chain.

Example:

```text
Application A
    |
    v
Library B
    |
    v
Library C
    |
    v
System Component D
```

Installing Application A may require several additional packages.

APT examines these relationships and attempts to install the required components.

This is one reason APT is useful for routine software installation.

---

## 6. Inspecting Dependencies with APT

APT provides several commands for investigating dependencies.

### A. Display package dependencies

```bash
apt depends nano
```

This displays dependency information for nano.

### B. Display reverse dependencies

```bash
apt rdepends nano
```

Reverse dependencies are packages that depend on the specified package.

This helps answer:

"What other installed or available software may rely on this package?"

### C. Display package information

```bash
apt show nano
```

Look for the dependency-related fields in the output.

### D. Inspect package policy

```bash
apt policy nano
```

This shows the installed version, candidate version, and available repository information.

---

## 7. Inspecting Dependencies with APT-Cache

`apt-cache` provides another way to investigate package relationships.

### A. Display dependencies

```bash
apt-cache depends nano
```

### B. Display reverse dependencies

```bash
apt-cache rdepends nano
```

### C. Display package information

```bash
apt-cache show nano
```

### D. Search package metadata

```bash
apt-cache search editor
```

These commands are useful when researching packages before installing or removing them.

---

## 8. Inspecting Dependencies with DPKG

DPKG can also display dependency information recorded in the local package database.

### A. Inspect a package

```bash
dpkg -s nano
```

Look for the `Depends` field.

### B. Display installed package information

```bash
dpkg-query -W nano
```

### C. List installed packages

```bash
dpkg -l
```

Remember that DPKG primarily reports information from the local package database, while APT can also inspect repository metadata.

---

## 9. How APT Resolves Dependencies

Suppose you run:

```bash
sudo apt install package-a
```

APT generally performs these operations:

1. Reads the package information from configured repositories.
2. Checks the requested package.
3. Identifies required dependencies.
4. Checks which dependencies are already installed.
5. Calculates which additional packages are required.
6. Presents the proposed changes.
7. Downloads the required packages.
8. Coordinates their installation through DPKG.

You can review the proposed changes before confirming the installation.

For example:

```bash
sudo apt install nano
```

Read the package summary before accepting.

You can also simulate an installation:

```bash
apt-get -s install nano
```

The `-s` option simulates the operation without actually installing the package.

This is useful for investigating the changes APT would make.

---

## 10. Understanding Dependency Conflicts

A dependency conflict occurs when packages have incompatible requirements.

Example:

```text
Package A requires Library B version 1.
Package C requires Library B version 2.
```

If the requirements cannot be satisfied together, package management may fail.

Possible causes include:

- Incompatible package versions.
- Mixing repositories from different distributions.
- Third-party packages.
- Interrupted upgrades.
- Packages being held at older versions.

### Inspecting package versions

```bash
apt policy nano
```

### Inspecting held packages

```bash
apt-mark showhold
```

### Checking package status

```bash
dpkg --audit
```

These commands help identify possible causes before attempting repairs.

---

## 11. Troubleshooting Dependency Problems

### A. Update package information

```bash
sudo apt update
```

This refreshes repository package indexes.

### B. Simulate a proposed installation

```bash
apt-get -s install nano
```

Review the proposed changes.

### C. Check for held packages

```bash
apt-mark showhold
```

A held package may not be upgraded automatically.

### D. Check package database consistency

```bash
sudo dpkg --audit
```

### E. Repair broken dependencies when appropriate

```bash
sudo apt --fix-broken install
```

This asks APT to attempt to correct broken dependencies.

**Important:** Review the proposed changes before confirming. Do not accept unexpected removals or major system changes without understanding them.

### F. Complete interrupted package configuration

```bash
sudo dpkg --configure -a
```

Use this when packages have been unpacked but not properly configured.

Review errors before proceeding with additional package operations.

---

## 12. Dependency Management and Cybersecurity

Dependencies are important to cybersecurity because applications rely on libraries and components that may contain vulnerabilities.

For example:

```text
Application
    |
    v
Third-party Library
    |
    v
Vulnerable Component
```

Even if the main application is secure, a vulnerable dependency may introduce security risks.

### Common dependency-related risks

1. **Outdated dependencies:** Old libraries may contain known vulnerabilities.
2. **Malicious packages:** Untrusted package sources may distribute harmful software.
3. **Dependency confusion:** Attackers may exploit package naming and source-selection mistakes.
4. **Supply-chain attacks:** A trusted application may depend on a compromised component.
5. **Excessive dependencies:** Unnecessary software increases the system's attack surface.

### Security best practices

- Install packages from trusted repositories.
- Keep software updated.
- Avoid unnecessary third-party repositories.
- Review dependency changes before installation.
- Remove software that is no longer needed.
- Investigate unfamiliar packages.
- Use security advisories to monitor vulnerable components.

---

## 13. Practical Lab: Investigating Package Dependencies

We will use nano as our example package.

### Task 1: Inspect nano dependencies

```bash
apt depends nano
```

Record several dependencies shown in the output.

### Task 2: Inspect reverse dependencies

```bash
apt rdepends nano
```

Observe which packages declare a dependency on nano.

### Task 3: Use APT-Cache

```bash
apt-cache depends nano
```

### Task 4: Inspect package metadata

```bash
apt show nano
```

Find the `Depends` field.

### Task 5: Inspect the installed package

```bash
dpkg -s nano
```

Check its installed version and dependency information.

### Task 6: Inspect package policy

```bash
apt policy nano
```

Identify:
- Installed version.
- Candidate version.
- Repository information.

### Task 7: Simulate an installation

```bash
apt-get -s install nano
```

Observe what APT proposes to do.

The simulation does not install the package.

### Task 8: Check held packages

```bash
apt-mark showhold
```

If nothing is displayed, there may be no packages currently held.

### Task 9: Check package health

```bash
sudo dpkg --audit
```

Record any reported issues.

### Task 10: Inspect another package

Repeat the investigation using:

```bash
apt depends curl
```

Compare its dependency information with nano.

---

## 14. Knowledge Check

Answer these questions in your own words.

1. What is a package dependency?
2. Why do Linux applications need dependencies?
3. What is the difference between Depends and Recommends?
4. What is a dependency chain?
5. What is a reverse dependency?
6. Which command displays package dependencies using APT?
7. What is the purpose of `apt-get -s install`?
8. What causes dependency conflicts?
9. What does `apt --fix-broken install` attempt to do?
10. Why are outdated dependencies a cybersecurity risk?

---

## 15. Practical Assignment

Complete the following tasks:

- [ ] Inspect nano dependencies using `apt depends nano`.
- [ ] Inspect nano reverse dependencies using `apt rdepends nano`.
- [ ] Display nano dependency information using `apt-cache depends nano`.
- [ ] Inspect nano metadata using `apt show nano`.
- [ ] Inspect nano using `dpkg -s nano`.
- [ ] Check nano package policy using `apt policy nano`.
- [ ] Simulate nano installation using `apt-get -s install nano`.
- [ ] Check held packages using `apt-mark showhold`.
- [ ] Run `sudo dpkg --audit`.
- [ ] Inspect curl dependencies using `apt depends curl`.
- [ ] Write down five reasons dependency management matters to cybersecurity.

## Lesson Summary

In this lesson, you learned:

- What package dependencies are.
- Why software requires additional packages.
- Different types of dependencies.
- How dependency chains work.
- How to inspect dependencies using APT and DPKG.
- How APT resolves dependency requirements.
- How to identify and troubleshoot dependency conflicts.
- Why dependency management is important for cybersecurity.

**Next Lesson: Module 09 – Lesson 12: Installing and Managing DEB Files.**