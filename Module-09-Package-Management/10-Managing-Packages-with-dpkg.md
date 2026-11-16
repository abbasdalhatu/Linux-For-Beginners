# Module 09 – Lesson 10: Managing Packages with DPKG

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what DPKG is.
2. Understand the relationship between APT and DPKG.
3. Install local `.deb` packages using DPKG.
4. Remove packages using DPKG.
5. Inspect package installation status.
6. List installed packages and package files.
7. Identify and troubleshoot common DPKG errors.
8. Understand the security implications of manual package installation.

---

## 2. What Is DPKG?

**DPKG (Debian Package Manager)** is the low-level package management system used by Debian-based Linux distributions, including Kali Linux.

It is responsible for managing Debian software packages with the `.deb` file extension.

DPKG can:

- Install local `.deb` packages.
- Remove installed packages.
- Inspect package information.
- List installed packages.
- Verify package installation status.
- Manage package configuration.

Unlike APT, DPKG does not automatically download and resolve dependencies from online repositories.

### Simple analogy

Think of APT as a shopping assistant.

It finds the software, checks what else is required, downloads the necessary packages, and coordinates installation.

DPKG is the installation system that handles the actual Debian package files.

---

## 3. Relationship Between APT and DPKG

APT and DPKG work together.

| APT | DPKG |
|---|---|
| High-level package manager | Low-level package manager |
| Uses configured repositories | Works directly with package files and local database |
| Resolves dependencies | Does not automatically resolve dependencies |
| Downloads packages | Installs and removes Debian packages |
| Manages repository information | Maintains package installation records |

### Example using APT

```bash
sudo apt install nano
```

APT finds the package, resolves dependencies, downloads the required files, and coordinates installation through DPKG.

### Example using DPKG

```bash
sudo dpkg -i package.deb
```

DPKG attempts to install the local package file.

If dependencies are missing, it may leave the package in an incomplete state.

---

## 4. Understanding the `.deb` Package Format

A `.deb` file is a Debian software package.

It contains information and files needed to install software on Debian-based distributions.

A package may contain:

- Application files.
- Executables.
- Configuration files.
- Documentation.
- Package metadata.
- Installation and removal scripts.

Example filename:

```text
example-tool_1.0_amd64.deb
```

Let's break it down:

| Part | Meaning |
|---|---|
| `example-tool` | Package name |
| `1.0` | Package version |
| `amd64` | Package architecture |
| `.deb` | Debian package format |

The filename is illustrative.

---

## 5. Checking the DPKG Version

To check your installed DPKG version:

```bash
dpkg --version
```

This displays version information.

You can also inspect its help information:

```bash
dpkg --help
```

---

## 6. Listing Installed Packages

Use:

```bash
dpkg -l
```

This displays packages known to the local DPKG database.

Because the output may be large, you can filter it.

Example:

```bash
dpkg -l nano
```

This checks the package entry for `nano`.

### Understanding package status

| Status | Meaning |
|---|---|
| `ii` | Installed and configured |
| `rc` | Removed, but configuration files remain |
| `un` | Not installed |
| `iU` | Unpacked but not configured |
| `iF` | Configuration failed |

The status characters help administrators identify package states.

---

## 7. Inspecting Package Information

DPKG provides several useful inspection commands.

### A. Inspect package status

```bash
dpkg -s nano
```

Displays information about the package in the local package database.

### B. List package files

```bash
dpkg -L nano
```

Displays files installed by the package.

### C. Identify file ownership

```bash
dpkg -S /usr/bin/nano
```

Identifies which installed package owns the specified file, if it is tracked by DPKG.

### D. Inspect package database details

```bash
dpkg-query -W nano
```

Displays package information in a compact format.

### E. Display detailed package information

```bash
dpkg-query -s nano
```

Provides detailed status information.

---

## 8. Installing a Local `.deb` Package

DPKG can install software from a local Debian package file.

The basic syntax is:

```bash
sudo dpkg -i package.deb
```

Here:

- `sudo` provides administrative privileges.
- `dpkg` runs the Debian package manager.
- `-i` means install.
- `package.deb` is the local package file.

### Example

```bash
sudo dpkg -i example-tool_1.0_amd64.deb
```

This example assumes the file exists in your current working directory.

Do not execute this illustrative command unless you actually have a verified package file with that name.

### Important limitation

DPKG does not automatically download missing dependencies.

If the package requires additional software that is not installed, the installation may be incomplete.

APT is generally more convenient for installing packages from configured repositories.

---

## 9. Inspecting a `.deb` File Before Installation

Before installing a local package, inspect it.

### A. Display package information

```bash
dpkg-deb -I package.deb
```

This displays metadata contained in the package.

### B. List package contents

```bash
dpkg-deb -c package.deb
```

This lists files contained in the archive.

### C. Test the package archive

```bash
dpkg-deb --info package.deb
```

This provides information about the archive.

These commands help you understand what a package contains before installing it.

**Security warning:** Inspecting metadata and contents does not prove that a package is safe. Obtain packages from trusted sources and verify their authenticity.

---

## 10. Removing Packages Using DPKG

DPKG can remove installed packages.

### A. Remove a package

```bash
sudo dpkg -r nano
```

The `-r` option requests package removal.

### B. Purge a package

```bash
sudo dpkg -P nano
```

The `-P` option requests removal of the package and its package-managed configuration files.

### Comparison

| Command | Purpose |
|---|---|
| `dpkg -r package-name` | Removes the package |
| `dpkg -P package-name` | Removes the package and package-managed configuration |

### Important difference from APT

DPKG does not automatically resolve the dependency consequences of package removal.

APT is generally safer for routine package removal because it considers dependency relationships.

---

## 11. Understanding the DPKG Database

DPKG maintains information about installed packages.

An important database file is:

```text
/var/lib/dpkg/status
```

It contains records describing package states and metadata.

You can inspect it using:

```bash
less /var/lib/dpkg/status
```

Press `q` to exit.

Another important directory is:

```text
/var/lib/dpkg/info/
```

It contains package-related information files.

Inspect it using:

```bash
ls -lah /var/lib/dpkg/info/
```

### Why is the database important?

The package database allows Linux to track:

- Installed packages.
- Package versions.
- Package architecture.
- Package status.
- Dependencies.
- Package-related file information.

Do not manually edit or delete the DPKG database files.

Doing so can damage package management on your system.

---

## 12. Understanding DPKG Package States

During installation, packages may pass through different states.

For example:

1. A package is selected for installation.
2. Its files are unpacked.
3. Configuration scripts are executed.
4. The package becomes installed and configured.

If an operation fails, the package may remain partially installed.

Useful inspection command:

```bash
sudo dpkg --audit
```

This checks for partially installed or inconsistent package states.

Another useful command is:

```bash
dpkg --get-selections
```

This displays package selection information.

---

## 13. Troubleshooting DPKG Errors

### A. Dependency problems

You may see an error indicating that a package depends on another package that is not installed.

Inspect the package:

```bash
dpkg -s package-name
```

APT may be able to resolve dependencies if the required package is available from your configured repositories.

### B. Interrupted package installation

If an installation was interrupted, inspect the package database:

```bash
sudo dpkg --audit
```

Do not immediately force another installation without understanding the problem.

### C. Package file not found

Example:

```text
dpkg: error: cannot access archive: No such file or directory
```

Possible cause:

The package file does not exist in the current directory or the path is incorrect.

Check your current directory:

```bash
pwd
```

List available files:

```bash
ls -lah
```

### D. Permission errors

Use administrative privileges when installing or removing system packages:

```bash
sudo dpkg -i package.deb
```

### E. Package database lock

APT and DPKG use locks to prevent conflicting package-management operations.

If you see a lock error, another package-management operation may already be running.

Check active package-management processes:

```bash
ps -ef | grep -E 'apt|dpkg'
```

Wait for legitimate package operations to finish.

Do not delete lock files or terminate package-management processes blindly.

---

## 14. APT Versus DPKG: When Should You Use Each?

### Use APT when:

- Installing packages from repositories.
- Upgrading installed software.
- Resolving dependencies.
- Removing software safely.
- Searching configured repositories.

Examples:

```bash
sudo apt install nano
```

```bash
sudo apt remove nano
```

```bash
sudo apt upgrade
```

### Use DPKG when:

- Inspecting package status.
- Listing installed package files.
- Identifying file ownership.
- Examining local `.deb` archives.
- Installing or removing a local Debian package when appropriate.

Examples:

```bash
dpkg -s nano
```

```bash
dpkg -L nano
```

```bash
dpkg -S /usr/bin/nano
```

```bash
dpkg-deb -I package.deb
```

For everyday package management, APT is generally the preferred interface.

---

## 15. Cybersecurity Connection: Investigating Installed Software

During a Linux security investigation, an administrator may discover an unfamiliar executable.

They need to determine whether it belongs to a known installed package.

For example:

```bash
dpkg -S /usr/bin/nano
```

They may then inspect the package:

```bash
dpkg -s nano
```

And examine its files:

```bash
dpkg -L nano
```

This information can help establish:

- Which package owns the executable.
- Which version is installed.
- Whether the package is properly configured.
- Which other files belong to it.

However, package ownership is not proof that a file has not been modified.

For deeper integrity investigation, administrators may compare installed files against trusted package contents or use dedicated integrity-monitoring tools.

---

## 16. Practical Lab: Working with DPKG

In this lab, we will inspect existing packages without installing unknown software.

### Task 1: Check the DPKG version

```bash
dpkg --version
```

### Task 2: List installed packages

```bash
dpkg -l
```

### Task 3: Inspect nano

```bash
dpkg -s nano
```

### Task 4: List nano's files

```bash
dpkg -L nano
```

### Task 5: Identify nano executable ownership

```bash
dpkg -S /usr/bin/nano
```

### Task 6: Inspect package information

```bash
dpkg-query -W nano
```

### Task 7: Inspect package database status

```bash
less /var/lib/dpkg/status
```

Press `q` to exit.

### Task 8: Inspect the DPKG information directory

```bash
ls -lah /var/lib/dpkg/info/
```

### Task 9: Check for package problems

```bash
sudo dpkg --audit
```

If no output is displayed, there may be no partially installed packages requiring attention.

### Task 10: Inspect package selection information

```bash
dpkg --get-selections
```

Observe the package names and their selection states.

---

## 17. Knowledge Check

Answer these questions in your own words.

1. What is DPKG?
2. What is the difference between APT and DPKG?
3. What is a `.deb` package?
4. What does `dpkg -i` do?
5. Why might a DPKG installation fail because of dependencies?
6. What is the difference between `dpkg -r` and `dpkg -P`?
7. What does `dpkg -L` display?
8. What does `dpkg -S` do?
9. Where is the DPKG package status database stored?
10. Why should you avoid manually editing the DPKG database?

---

## 18. Practical Assignment

Complete the following tasks:

- [ ] Check DPKG version using `dpkg --version`.
- [ ] List installed packages using `dpkg -l`.
- [ ] Inspect nano using `dpkg -s nano`.
- [ ] List nano files using `dpkg -L nano`.
- [ ] Identify nano executable ownership using `dpkg -S`.
- [ ] Inspect nano using `dpkg-query -W nano`.
- [ ] Inspect `/var/lib/dpkg/status`.
- [ ] Inspect `/var/lib/dpkg/info/`.
- [ ] Run `sudo dpkg --audit`.
- [ ] Display package selection information.
- [ ] Write down five differences between APT and DPKG.

## Lesson Summary

In this lesson, you learned:

- What DPKG is.
- How APT and DPKG work together.
- How Debian packages are structured.
- How to inspect installed packages.
- How to install and remove local `.deb` packages.
- How to examine package contents.
- How to investigate DPKG errors.
- How DPKG supports Linux administration and cybersecurity investigations.

**Next Lesson: Module 09 – Lesson 11: Understanding Package Dependencies.**