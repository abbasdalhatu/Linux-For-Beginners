# Module 09 – Lesson 09: Searching and Inspecting Packages

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Search for software packages using APT.
2. Find installed and available packages.
3. Inspect package descriptions and metadata.
4. Identify installed and candidate package versions.
5. Find which package owns a particular file.
6. List files installed by a package.
7. Use package-query commands for Linux administration and cybersecurity.

---

## 2. What Is Package Searching?

Package searching is the process of finding software packages available through configured repositories.

Imagine you want to install a network troubleshooting utility but do not know its exact package name.

Instead of guessing, you can search for it.

APT provides commands that help you discover software, inspect package details, and determine whether a package is installed.

For example:

```bash
apt search network
```

This searches package information for the term `network`.

---

## 3. Understanding `apt search`

The command:

```bash
apt search package-name
```

searches available package information for matching names and descriptions.

### Example 1: Search for curl

```bash
apt search curl
```

### Example 2: Search for network utilities

```bash
apt search network
```

### Example 3: Search for text editors

```bash
apt search editor
```

### Example 4: Search for security-related tools

```bash
apt search security
```

Search results may contain many packages.

The package name and short description help you identify relevant results.

**Important:** A search result does not mean that the package is installed.

It only means APT found matching package information.

---

## 4. Understanding Search Results

An APT search result may look like this:

```text
curl/kali-rolling 8.x amd64
  command line tool for transferring data with URL syntax
```

This is an illustrative example.

Let's understand the parts:

| Part | Meaning |
|---|---|
| `curl` | Package name |
| `kali-rolling` | Repository suite |
| `8.x` | Illustrative package version |
| `amd64` | Package architecture |
| Description | Short explanation of the software |

Package versions and repository names will vary.

---

## 5. Understanding `apt show`

After finding a package, you may want to inspect its details.

Use:

```bash
apt show package-name
```

Example:

```bash
apt show curl
```

APT may display:

- Package name.
- Version.
- Architecture.
- Maintainer.
- Installed size.
- Download size.
- Dependencies.
- Description.
- Repository information.

### Why is this useful?

Before installing unfamiliar software, you should understand what the package is and what it requires.

Package metadata helps you make informed installation decisions.

However, metadata alone does not prove that software is safe. Always consider the repository source and package authenticity.

---

## 6. Understanding `apt policy`

The command:

```bash
apt policy package-name
```

displays version and repository priority information.

Example:

```bash
apt policy nano
```

You may see output similar to:

```text
nano:
  Installed: 7.2-1
  Candidate: 7.2-1
  Version table:
     7.2-1 500
```

The versions above are illustrative.

### Important fields

| Field | Meaning |
|---|---|
| `Installed` | Version currently installed |
| `Candidate` | Version APT would select for installation or upgrade |
| `Version table` | Available package versions and priorities |

### Why is `apt policy` important?

It helps you investigate:

- Whether a package is installed.
- Which version is installed.
- Which version is available.
- Which repository supplies a version.
- Whether repository priorities affect package selection.

---

## 7. Understanding `dpkg -l`

The command:

```bash
dpkg -l
```

lists packages known to the DPKG package database.

The output may be very large.

To inspect one package:

```bash
dpkg -l nano
```

Example:

```text
ii  nano  7.2-1  amd64  small, friendly text editor
```

The example is illustrative.

### Understanding package status

The first two characters indicate package state.

Common examples:

| Status | Meaning |
|---|---|
| `ii` | Package is installed and configured |
| `rc` | Package has been removed but configuration files remain |
| `un` | Package is not installed |
| `iU` | Package is unpacked but not configured |
| `iF` | Package is in a failed configuration state |

The exact output depends on the package's current state.

---

## 8. Understanding `dpkg -s`

The command:

```bash
dpkg -s package-name
```

displays detailed information about a package in the local package database.

Example:

```bash
dpkg -s nano
```

You can use it to inspect:

- Package status.
- Version.
- Architecture.
- Dependencies.
- Description.
- Maintainer.

This is particularly useful when checking whether a package is installed correctly.

---

## 9. Understanding `dpkg -L`

The command:

```bash
dpkg -L package-name
```

lists the files installed by a package.

Example:

```bash
dpkg -L nano
```

This can display paths such as:

```text
/usr/bin/nano
/usr/share/doc/nano
/usr/share/man/man1/nano.1.gz
```

The exact file list depends on the package version.

### Why is this useful?

System administrators use this command to:

- Locate executables.
- Find documentation.
- Inspect configuration-related files.
- Understand where package files are stored.
- Investigate software installations.

---

## 10. Understanding `dpkg -S`

Sometimes you discover a file on your system and want to know which installed package owns it.

Use:

```bash
dpkg -S /path/to/file
```

For example:

```bash
dpkg -S /usr/bin/nano
```

This searches the local DPKG database for the package that owns the specified file.

### Real-world example

Imagine you find an unfamiliar executable:

```text
/usr/bin/example-tool
```

You can investigate its package ownership:

```bash
dpkg -S /usr/bin/example-tool
```

If a package owns it, DPKG may identify that package.

If no package owns it, the file may have been created manually, installed through another method, or belong to software not tracked by DPKG.

Do not assume that an unowned file is malicious.

---

## 11. Understanding `apt list`

APT provides another useful command:

```bash
apt list
```

It lists packages known through the current package information.

Because the output can be very large, you can filter it.

### List installed packages

```bash
apt list --installed
```

### List upgradeable packages

```bash
apt list --upgradable
```

### Search installed packages

```bash
apt list --installed | grep nano
```

### Search for a package

```bash
apt list 2>/dev/null | grep curl
```

Remember that searching output with `grep` is a text-matching operation. It is not always as precise as inspecting a package directly.

---

## 12. Using `apt-cache` for Package Inspection

APT also provides the `apt-cache` utility.

### A. Search package information

```bash
apt-cache search curl
```

### B. Display package metadata

```bash
apt-cache show curl
```

### C. Display package policy

```bash
apt-cache policy curl
```

### D. Display package dependencies

```bash
apt-cache depends curl
```

### E. Display reverse dependencies

```bash
apt-cache rdepends curl
```

Reverse dependencies show packages that depend on the selected package.

This is useful when investigating whether removing a package might affect other software.

---

## 13. Comparing Important Package Inspection Commands

| Command | Purpose |
|---|---|
| `apt search name` | Search package information |
| `apt show name` | Display package metadata |
| `apt policy name` | Display installed and candidate versions and priorities |
| `dpkg -l` | List packages in the local package database |
| `dpkg -s name` | Inspect a package's installed database status |
| `dpkg -L name` | List files belonging to a package |
| `dpkg -S file` | Identify the package that owns a file |
| `apt list --installed` | List installed packages |
| `apt-cache depends name` | Display package dependencies |
| `apt-cache rdepends name` | Display reverse dependencies |

Learning these commands makes Linux package investigation much easier.

---

## 14. Practical Lab: Searching and Inspecting Packages

We will investigate three packages:

- `nano`
- `curl`
- `wget`

### Task 1: Search for nano

```bash
apt search nano
```

Identify the package name and description.

### Task 2: Inspect nano

```bash
apt show nano
```

Identify:

- Version.
- Architecture.
- Maintainer.
- Installed size.
- Dependencies.

### Task 3: Check nano's package policy

```bash
apt policy nano
```

Record the installed and candidate versions.

### Task 4: Inspect nano's installation status

```bash
dpkg -s nano
```

Find its package status.

### Task 5: List nano's installed files

```bash
dpkg -L nano
```

Identify its executable location.

### Task 6: Identify the owner of nano's executable

```bash
dpkg -S /usr/bin/nano
```

### Task 7: Search for curl

```bash
apt search curl
```

### Task 8: Inspect curl

```bash
apt show curl
```

### Task 9: Inspect curl dependencies

```bash
apt-cache depends curl
```

### Task 10: Inspect curl reverse dependencies

```bash
apt-cache rdepends curl
```

### Task 11: List installed packages

```bash
apt list --installed
```

### Task 12: List available upgrades

```bash
apt list --upgradable
```

Observe the output and identify any packages with available upgrades.

---

## 15. Cybersecurity Connection: Package Investigation

Package inspection is useful during security investigations.

Imagine you are examining a Linux computer and discover an unfamiliar executable.

You may need to determine:

1. What is the executable?
2. Which package installed it?
3. What version is installed?
4. Where did the package come from?
5. Which files belong to it?
6. Does the package have dependencies?

Useful commands include:

```bash
dpkg -S /path/to/file
```

```bash
dpkg -s package-name
```

```bash
dpkg -L package-name
```

```bash
apt policy package-name
```

These commands provide useful evidence during system administration and incident investigation.

However, a legitimate package name does not guarantee that every file on a system is trustworthy. Further investigation may be necessary.

---

## 16. Knowledge Check

Answer these questions in your own words.

1. What is package searching?
2. What does `apt search` do?
3. What is the purpose of `apt show`?
4. What information does `apt policy` provide?
5. What does `dpkg -l` display?
6. What is the difference between `dpkg -L` and `dpkg -S`?
7. What does the package status `ii` mean?
8. What is the purpose of `apt-cache depends`?
9. Why is package inspection useful during cybersecurity investigations?
10. Does finding a package in APT search results mean it is installed?

---

## 17. Practical Assignment

Complete the following:

- [ ] Search for `nano` using `apt search`.
- [ ] Inspect it using `apt show nano`.
- [ ] Check its policy using `apt policy nano`.
- [ ] Inspect its status using `dpkg -s nano`.
- [ ] List its files using `dpkg -L nano`.
- [ ] Identify the owner of `/usr/bin/nano` using `dpkg -S`.
- [ ] Search for `curl`.
- [ ] Inspect curl dependencies.
- [ ] Inspect curl reverse dependencies.
- [ ] List installed packages.
- [ ] List upgradeable packages.
- [ ] Write down five differences between the package inspection commands.

---

## Lesson Summary

In this lesson, you learned:

- How to search for software packages.
- How to inspect package metadata.
- How to identify installed and candidate versions.
- How to inspect installed package status.
- How to list files belonging to a package.
- How to identify package ownership of a file.
- How to inspect dependencies and reverse dependencies.
- How package investigation supports cybersecurity.

**Next Lesson: Module 09 – Lesson 10: Managing Packages with DPKG.**