# MODULE 09: LINUX PACKAGE MANAGEMENT

## LESSON 02: UNDERSTANDING LINUX PACKAGES

### 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what Linux packages contain.
2. Understand Debian package files.
3. Identify package names and versions.
4. Understand package architecture.
5. Explain package metadata.
6. Understand package dependencies.
7. Identify installed and uninstalled packages.
8. Inspect package information using Linux commands.

---

### 2. What Is a Linux Package?

A Linux package is an organized collection of files and instructions used to install and manage software.

Instead of distributing an application as unrelated files, developers and maintainers organize its components into a package.

A package may contain:

- Executable programs.
- Configuration files.
- Shared libraries.
- Documentation.
- Installation scripts.
- Metadata.
- Dependency information.

For example, when you install Nano using APT, Linux receives a package containing the files needed to run the Nano text editor.

### 3. Understanding Debian Packages

Kali Linux is based on Debian, so it uses Debian package formats.

The most common Debian package file extension is:

```text
.deb
```

A `.deb` file is an archive containing software files and package-control information.

Example:

```text
nano_7.2-1_amd64.deb
```

This is an illustrative filename. Actual versions may differ.

Let's break it down:

| Component | Meaning |
|---|---|
| nano | Package name |
| 7.2-1 | Package version |
| amd64 | Target architecture |
| .deb | Debian package format |

A `.deb` file can be installed using DPKG or APT.

### 4. Understanding Package Names

A package name identifies a particular software package.

Examples include:

| Package | Purpose |
|---|---|
| nano | Text editor |
| curl | Data transfer utility |
| wget | File retrieval utility |
| git | Version control system |
| htop | Interactive process monitor |
| tree | Directory structure viewer |

Package names are important because they are used in installation, removal, searching, and inspection commands.

Example:

```bash
apt show curl
```

This displays information about the curl package.

### 5. Understanding Package Versions

Software developers release different versions of applications.

A package version helps identify which release is available or installed.

Example:

```text
2.8.1-1
```

A Debian package version can contain several components:

- Epoch, if present.
- Upstream version.
- Debian revision.

A version may look like:

```text
1:2.8.1-1
```

Here:

- `1` is the epoch.
- `2.8.1` is the upstream version.
- `1` is the Debian revision.

The epoch is used when package-version ordering needs special handling.

Do not assume that every package version follows exactly the same pattern.

### 6. Understanding Package Architecture

Computer processors and operating systems support different architectures.

A package may be built for a particular architecture.

Common Debian architecture names include:

| Architecture | Description |
|---|---|
| amd64 | 64-bit x86 processors |
| i386 | 32-bit x86 processors |
| arm64 | 64-bit ARM processors |
| armhf | ARM hard-float architecture |
| all | Architecture-independent package |

Check your Kali Linux architecture:

```bash
dpkg --print-architecture
```

Check whether foreign architectures are enabled:

```bash
dpkg --print-foreign-architectures
```

Your architecture determines which binary packages are compatible with your system.

### 7. Understanding Package Metadata

Package metadata describes a package.

It can contain:

- Package name.
- Version.
- Architecture.
- Maintainer.
- Description.
- Dependencies.
- Download size.
- Installed size.
- Homepage.
- Package section.

Use:

```bash
apt show nano
```

Example of illustrative package information:

```text
Package: nano
Version: 7.2-1
Architecture: amd64
Maintainer: Example Maintainer
Installed-Size: 2900
Depends: libc6
Description: Small, friendly text editor
```

Actual output will depend on the package version and repository information available on your system.

Package metadata helps administrators understand what they are installing.

### 8. Understanding Installed and Available Packages

A package may exist in a repository without being installed on your computer.

There are two important situations:

**Available package**

The package exists in configured repositories and can potentially be installed.

**Installed package**

The package has already been installed on your system.

Check whether a package is installed:

```bash
dpkg -s nano
```

List installed packages:

```bash
dpkg -l
```

Search for a package:

```bash
apt search nano
```

Remember that search results do not automatically mean a package is installed.

### 9. Understanding Package Installation Size

Packages have two important size measurements.

**Download size**

The amount of data downloaded to obtain the package.

**Installed size**

The approximate amount of disk space occupied after installation.

These values may differ significantly.

For example, a compressed package may be relatively small but occupy much more space after extraction.

Inspect package information:

```bash
apt show nano
```

Look for:

```text
Size:
Installed-Size:
```

The exact fields and values depend on the package.

### 10. Understanding Package Control Information

Debian packages contain control information describing how the package should be handled.

This information can include:

- Package name.
- Version.
- Architecture.
- Dependencies.
- Conflicting packages.
- Package description.

For an installed package, inspect its control information with:

```bash
dpkg-query -s nano
```

If Nano is not installed, use a package that is already installed.

The package database stores information that helps DPKG track installed software.

### 11. Understanding Package File Contents

A package installs files into different locations in the Linux filesystem.

For example:

| Directory | Typical Purpose |
|---|---|
| `/usr/bin` | User commands |
| `/usr/sbin` | Administrative commands |
| `/usr/lib` | Libraries |
| `/etc` | System configuration |
| `/usr/share/doc` | Documentation |
| `/usr/share/man` | Manual pages |

To list files installed by a package:

```bash
dpkg -L nano
```

This displays files associated with the installed Nano package.

To search for a particular file:

```bash
dpkg -L nano | grep '/bin/'
```

The exact locations depend on how the package is built.

### 12. Understanding Package Maintainers

A package maintainer is responsible for preparing and maintaining a package.

Depending on the distribution and project, maintainers may:

- Prepare package builds.
- Apply distribution-specific patches.
- Manage package dependencies.
- Fix packaging problems.
- Update package metadata.
- Maintain security updates.

Package metadata can display the maintainer information:

```bash
apt show nano
```

Using trusted distribution repositories helps reduce the risk of installing packages from unknown sources.

### 13. Practical Laboratory: Inspecting Linux Packages

**Objective:** Learn how to inspect package names, versions, architecture, metadata, and installed files.

#### Step 1: Check your architecture

```bash
dpkg --print-architecture
```

Record the result.

#### Step 2: Check foreign architectures

```bash
dpkg --print-foreign-architectures
```

#### Step 3: Inspect Nano package information

```bash
apt show nano
```

Record:

- Package name.
- Version.
- Architecture.
- Maintainer.
- Installed size.
- Dependencies.

#### Step 4: Check whether Nano is installed

```bash
dpkg -s nano
```

If Nano is not installed, choose an installed package for the remaining exercises.

#### Step 5: Display installed package files

```bash
dpkg -L nano
```

#### Step 6: Display package status

```bash
dpkg-query -s nano
```

#### Step 7: List installed packages

```bash
dpkg -l | less
```

Press `q` to exit.

#### Step 8: Search for packages

```bash
apt search curl
```

#### Step 9: Inspect curl package information

```bash
apt show curl
```

#### Step 10: Identify installed curl files

If curl is installed:

```bash
dpkg -L curl
```

If it is not installed, use another installed package.

### 14. Package Management and Cybersecurity

Understanding package information is important during security investigations.

**1. Version verification**

Security professionals inspect software versions to identify potentially outdated applications.

**2. Package integrity**

Package verification tools can help detect unexpected modifications to installed package files.

**3. Supply-chain security**

Package metadata helps identify the origin and dependencies of software.

**4. Attack surface analysis**

Knowing which packages are installed helps administrators identify unnecessary software.

**5. Vulnerability investigation**

Security teams compare installed package versions with relevant security advisories.

**6. Software auditing**

Package databases provide an inventory of installed software.

### 15. Lesson Summary

In this lesson, we learned:

1. A Linux package contains software and management information.
2. Debian packages commonly use the `.deb` extension.
3. Package names identify software.
4. Package versions identify particular releases.
5. Package architecture describes binary compatibility.
6. Package metadata provides information about software.
7. Download size and installed size are different.
8. DPKG maintains information about installed packages.
9. `dpkg -L` lists files belonging to an installed package.
10. Package information supports software auditing and security investigations.

### 16. Knowledge Check

Answer these questions in your own words.

1. What is a Linux software package?
2. What is the file extension commonly used by Debian packages?
3. Explain the components of `nano_7.2-1_amd64.deb`.
4. What is package architecture?
5. What does `amd64` represent?
6. What is package metadata?
7. Explain the difference between download size and installed size.
8. What is the role of a package maintainer?
9. Which command lists files installed by a package?
10. Why is package-version information important in cybersecurity?

### 17. Practical Assignment

- [ ] Identify your Kali Linux architecture.
- [ ] Check foreign architectures.
- [ ] Inspect Nano package information.
- [ ] Check whether Nano is installed.
- [ ] List the files belonging to Nano if installed.
- [ ] Inspect the curl package.
- [ ] Search for curl using APT.
- [ ] Identify an installed package and inspect its files.
- [ ] Record five important pieces of package metadata.
- [ ] Complete all ten knowledge-check questions.

**Safety reminder:** This laboratory is observational. You do not need to install or remove any packages.

**END OF LESSON 02**

**NEXT LESSON: UNDERSTANDING APT AND DPKG**