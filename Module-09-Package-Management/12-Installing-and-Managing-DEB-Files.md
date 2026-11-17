---
---

# Module 09 – Lesson 12: Installing and Managing DEB Files

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a Debian `.deb` package is.
2. Understand the internal structure of a DEB file.
3. Inspect a DEB package before installation.
4. Install local DEB packages using APT and DPKG.
5. Remove software installed from a DEB package.
6. Verify package installation.
7. Troubleshoot common DEB installation errors.
8. Identify security risks associated with untrusted DEB packages.

---

## 2. What Is a DEB File?

A `.deb` file is a software package format used by Debian-based Linux distributions.

It is commonly used by:

- Debian
- Kali Linux
- Ubuntu
- Linux Mint
- Other Debian-based distributions

A DEB file contains the files and metadata required to install a software package.

Example:

```text
example-tool_1.0_amd64.deb
```

Breaking down the filename:

| Component | Meaning |
|---|---|
| `example-tool` | Package name |
| `1.0` | Package version |
| `amd64` | Processor architecture |
| `.deb` | Debian package format |

The example filename is illustrative.

---

## 3. Understanding the Internal Structure of a DEB File

A DEB package is an archive containing several important components.

Its structure commonly includes:

### A. Debian binary format information

Identifies the archive as a Debian binary package.

### B. Control archive

Contains package metadata, including:

- Package name.
- Version.
- Architecture.
- Maintainer.
- Dependencies.
- Installation and removal scripts, when provided.

### C. Data archive

Contains the files that will be installed on the Linux system.

Examples include:

- Executables.
- Libraries.
- Documentation.
- Configuration files.

### Simple illustration

```text
example-tool.deb
│
├── Debian package information
│
├── Control archive
│   ├── Package metadata
│   ├── Dependencies
│   └── Installation scripts
│
└── Data archive
    ├── Executables
    ├── Libraries
    └── Documentation
```

This structure helps Linux understand what the package provides and how it should be installed.

---

## 4. Understanding DEB Package Architecture

Packages are built for particular processor architectures.

Common examples include:

| Architecture | Description |
|---|---|
| `amd64` | 64-bit x86 processors |
| `i386` | 32-bit x86 processors |
| `arm64` | 64-bit ARM processors |
| `armhf` | ARM hard-float architecture |
| `all` | Architecture-independent package |

Check your system architecture:

```bash
dpkg --print-architecture
```

Check additional architectures:

```bash
dpkg --print-foreign-architectures
```

A package built for an incompatible architecture may not install or run correctly.

---

## 5. Finding DEB Package Files

Before installing a local DEB package, you need to know where the file is located.

Useful commands:

### Display your current directory

```bash
pwd
```

### List files

```bash
ls -lah
```

### Find DEB files in your Downloads directory

```bash
find ~/Downloads -maxdepth 2 -type f -name "*.deb"
```

This searches the Downloads directory and its immediate subdirectories.

Do not assume that every DEB file you find is trustworthy.

---

## 6. Inspecting a DEB Package Before Installation

It is good practice to examine a package before installing it.

Assume you have a file named:

```text
example-tool.deb
```

Replace this example filename with the actual file path when performing these commands.

### A. Display package metadata

```bash
dpkg-deb -I example-tool.deb
```

This displays package information such as its name, version, architecture, and dependencies.

### B. List package contents

```bash
dpkg-deb -c example-tool.deb
```

This lists the files contained in the package.

### C. Display package information

```bash
dpkg-deb --info example-tool.deb
```

This is another way to inspect package metadata.

### D. Test the archive

```bash
dpkg-deb --contents example-tool.deb
```

This displays the archive contents.

### Why inspect first?

Inspection helps you identify:

- What software you are installing.
- Which architecture it supports.
- What dependencies it requires.
- Which files it intends to install.

However, inspecting a package does not establish that it is safe or authentic.

---

## 7. Installing DEB Files Using DPKG

DPKG can install a local DEB package.

Syntax:

```bash
sudo dpkg -i package-name.deb
```

Example:

```bash
sudo dpkg -i example-tool.deb
```

Explanation:

| Component | Purpose |
|---|---|
| `sudo` | Runs the operation with administrative privileges |
| `dpkg` | Debian package manager |
| `-i` | Installs a package |
| `example-tool.deb` | Local package file |

### Important limitation

DPKG does not automatically download and resolve missing dependencies.

If required dependencies are missing, the installation may remain incomplete.

For routine local DEB installation, APT is often more convenient.

---

## 8. Installing DEB Files Using APT

APT can install a local DEB file and attempt to resolve its dependencies from configured repositories.

Syntax:

```bash
sudo apt install ./package-name.deb
```

Example:

```bash
sudo apt install ./example-tool.deb
```

The `./` indicates that the package is a local file in the current directory.

You can also specify an absolute path:

```bash
sudo apt install /home/kali/Downloads/example-tool.deb
```

### Why use APT?

APT can:

1. Read the local DEB package.
2. Examine its dependency requirements.
3. Find available dependencies in configured repositories.
4. Propose the required package changes.
5. Install the package and its dependencies.

Review the proposed changes before confirming.

**Remember:** APT can only resolve dependencies that are available and compatible through your configured package sources.

---

## 9. Comparing DPKG and APT for Local DEB Installation

| Feature | DPKG | APT |
|---|---|---|
| Installs local DEB files | Yes | Yes |
| Resolves dependencies automatically | No | Attempts to |
| Downloads missing dependencies | No | Yes, from configured repositories |
| Works with package database | Yes | Yes, through DPKG |
| Suitable for routine installation | Less convenient | Generally preferred |

Examples:

DPKG:

```bash
sudo dpkg -i example-tool.deb
```

APT:

```bash
sudo apt install ./example-tool.deb
```

For most ordinary installations, prefer APT.

---

## 10. Verifying a DEB Installation

After installing a package, verify that it was installed correctly.

### A. Check package status

```bash
dpkg -s package-name
```

### B. Search installed packages

```bash
dpkg -l | grep package-name
```

### C. List installed files

```bash
dpkg -L package-name
```

### D. Locate the executable

```bash
command -v package-name
```

### E. Check the installed application version

```bash
package-name --version
```

The last command depends on the application providing a version option.

Verification helps confirm that installation completed successfully.

---

## 11. Removing Software Installed from a DEB File

After installation, the package is registered in the DPKG database.

You normally remove it using its package name, not the original DEB filename.

### A. Remove the package

```bash
sudo apt remove package-name
```

Alternatively:

```bash
sudo dpkg -r package-name
```

### B. Purge the package

```bash
sudo apt purge package-name
```

Alternatively:

```bash
sudo dpkg -P package-name
```

### Important distinction

- `remove` removes the installed package while generally retaining package-managed configuration.
- `purge` also removes package-managed configuration files.

APT is generally preferable because it considers dependency relationships when proposing package changes.

---

## 12. Understanding DEB Package Dependencies

A DEB package may require additional software.

For example:

```text
example-tool
    |
    ├── Library A
    |
    ├── Library B
    |
    └── Utility C
```

If these dependencies are unavailable, installation may fail.

Inspect the package dependencies:

```bash
dpkg-deb -I example-tool.deb
```

Look for the `Depends` field.

You can also inspect an installed package:

```bash
apt depends package-name
```

APT can attempt to resolve dependencies during local DEB installation.

---

## 13. Troubleshooting DEB Installation Problems

### A. Package file not found

Example:

```text
dpkg: error: cannot access archive: No such file or directory
```

Check the current directory:

```bash
pwd
```

List the files:

```bash
ls -lah
```

Make sure the package filename and path are correct.

### B. Wrong architecture

Inspect your system architecture:

```bash
dpkg --print-architecture
```

Inspect the package architecture:

```bash
dpkg-deb -f example-tool.deb Architecture
```

The package architecture must be compatible with your system.

### C. Missing dependencies

Inspect package metadata:

```bash
dpkg-deb -I example-tool.deb
```

If the package is trusted and appropriate for your system, APT may resolve missing dependencies:

```bash
sudo apt install ./example-tool.deb
```

### D. Incomplete package configuration

Inspect package problems:

```bash
sudo dpkg --audit
```

If an installation was interrupted, inspect the error messages before attempting recovery.

### E. Interrupted package configuration

When appropriate, complete pending package configuration:

```bash
sudo dpkg --configure -a
```

Do not use this command blindly if another package operation is running.

### F. Repository problems

Update package indexes:

```bash
sudo apt update
```

If repository errors appear, investigate them before retrying the installation.

---

## 14. Security Risks of DEB Files

Installing a DEB package can introduce security risks because installation may execute privileged package-maintenance scripts.

### Common risks

**1. Malicious packages**

An attacker may create a package containing harmful software.

**2. Modified packages**

A legitimate package may be modified before distribution.

**3. Supply-chain attacks**

A package may come from a compromised software publisher or distribution source.

**4. Vulnerable dependencies**

A package may depend on outdated or insecure libraries.

**5. Excessive permissions**

An application may request more privileges than its normal operation requires.

### Security best practices

1. Prefer official distribution repositories.
2. Download packages only from trusted publishers.
3. Verify package authenticity and checksums where available.
4. Check architecture and dependency information.
5. Inspect package contents before installation.
6. Avoid running unknown packages with administrative privileges.
7. Keep installed software updated.
8. Remove unnecessary packages.

### Important security note

A package can look legitimate and still be malicious. Metadata inspection is only one part of package verification.

---

## 15. Practical Lab: DEB Package Investigation

In this lab, we will investigate DEB files without installing unknown software.

### Task 1: Check system architecture

```bash
dpkg --print-architecture
```

Record the output.

### Task 2: Search for existing DEB files

```bash
find ~/Downloads -maxdepth 2 -type f -name "*.deb"
```

If no files are found, that is perfectly fine.

### Task 3: Inspect a DEB file if one is available

Replace `package.deb` with the actual file path:

```bash
dpkg-deb -I package.deb
```

### Task 4: List its contents

```bash
dpkg-deb -c package.deb
```

### Task 5: Inspect its architecture

```bash
dpkg-deb -f package.deb Architecture
```

### Task 6: Inspect its dependencies

```bash
dpkg-deb -f package.deb Depends
```

### Task 7: Inspect an already installed package

```bash
dpkg -s nano
```

### Task 8: List nano's installed files

```bash
dpkg -L nano
```

### Task 9: Check package consistency

```bash
sudo dpkg --audit
```

### Task 10: Simulate a package installation

```bash
apt-get -s install nano
```

Observe the proposed changes.

The simulation does not install anything.

**Lab safety:** Do not download or install random DEB packages for this exercise. The objective is to understand the package format and inspection process.

---

## 16. Knowledge Check

Answer these questions in your own words.

1. What is a DEB file?
2. Which Linux distributions commonly use DEB packages?
3. What is the purpose of the control archive?
4. What is the difference between `amd64` and `arm64`?
5. What does `dpkg-deb -I` do?
6. What does `dpkg-deb -c` display?
7. What is the difference between `dpkg -i` and `apt install ./package.deb`?
8. Why is APT generally preferred for installing local DEB packages?
9. How do you verify that a package is installed?
10. Why should you avoid installing DEB files from unknown sources?

---

## 17. Practical Assignment

Complete the following tasks:

- [ ] Check your system architecture.
- [ ] Search your Downloads directory for DEB files.
- [ ] Inspect a DEB file if one is available.
- [ ] Display the contents of a DEB package.
- [ ] Identify a DEB package's architecture.
- [ ] Inspect its dependency information.
- [ ] Inspect nano using `dpkg -s nano`.
- [ ] List nano's installed files.
- [ ] Run `sudo dpkg --audit`.
- [ ] Simulate a package installation using `apt-get -s install nano`.
- [ ] Write five security precautions to follow before installing a DEB file.

## Lesson Summary

In this lesson, you learned:

- What Debian DEB packages are.
- How DEB archives are structured.
- How to identify package architectures.
- How to inspect package metadata and contents.
- How to install local DEB files using DPKG and APT.
- How to verify and remove installed packages.
- How to troubleshoot installation problems.
- How to protect Linux systems from malicious packages.

**Next Lesson: Module 09 – Lesson 13: Troubleshooting APT and Package Errors.**