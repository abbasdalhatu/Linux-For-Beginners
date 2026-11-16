# Module 09 – Lesson 14: Linux Package Management Security

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain why package management is important to Linux security.
2. Understand trusted software repositories.
3. Explain package signatures and cryptographic verification.
4. Identify risks associated with third-party repositories.
5. Understand software supply-chain attacks.
6. Investigate installed packages for suspicious activity.
7. Apply security best practices when installing software.
8. Use package-management tools during security investigations.

---

## 2. Introduction to Package Management Security

Package management is not only about installing and removing software.

It is also an important part of protecting a Linux operating system.

Every package installed on a system introduces software, files, libraries, and sometimes background services.

If an attacker distributes a malicious package or compromises a software repository, users may unknowingly install harmful software.

This makes package management an important security responsibility.

### Why package security matters

Package management helps protect systems against:

- Malicious software.
- Vulnerable applications.
- Compromised software repositories.
- Modified installation packages.
- Supply-chain attacks.
- Unauthorized software installation.

A secure Linux administrator must understand where software comes from and how its authenticity is verified.

---

## 3. Understanding Trusted Software Repositories

A software repository is a location where software packages and package information are stored.

Linux distributions maintain repositories containing software that users can install through package managers.

For example, Kali Linux maintains official package repositories.

### Types of repositories

**A. Official repositories**

These are maintained by the Linux distribution or its authorized maintainers.

They are generally the preferred source for ordinary software installation.

**B. Third-party repositories**

These are maintained by external developers or organizations.

They may provide additional applications or newer versions of existing software.

However, they introduce additional trust and security considerations.

**C. Personal package archives or unofficial package sources**

These may be maintained by individuals or smaller communities.

Their quality, maintenance, and security practices can vary considerably.

### Security principle

Install software from trusted sources whenever possible.

Do not add a repository simply because a website tells you to.

---

## 4. Understanding Package Signing

Package repositories use cryptographic signatures to help establish authenticity and integrity.

A digital signature is created using a private cryptographic key.

Users and package managers verify signatures using the corresponding trusted public key.

### Simplified signing process

```text
Repository Maintainer
        |
        v
Creates repository metadata
        |
        v
Signs metadata with private key
        |
        v
Publishes signed repository
        |
        v
Linux package manager downloads metadata
        |
        v
Verifies signature using trusted key
        |
        v
Accepts or rejects metadata
```

### Why are signatures important?

They help protect against:

1. Unauthorized repository metadata changes.
2. Certain types of repository impersonation.
3. Modification of repository metadata during transmission.
4. Installation based on untrusted repository information.

### Important distinction

APT normally verifies signed repository metadata. It does not mean that every package is independently authenticated by a separate package signature.

The repository's signed metadata provides the trust mechanism used to authenticate package information and package checksums.

---

## 5. Understanding Cryptographic Checksums

A checksum is a value calculated from data using a hash function.

For example, SHA-256 produces a 256-bit hash value.

A checksum can help detect whether a file has changed.

### Example using SHA-256

```bash
sha256sum package.deb
```

Example output:

```text
a1b2c3d4... package.deb
```

The displayed value is illustrative.

If the file changes, its SHA-256 hash will normally change.

### Verify a package against an official checksum

If the software publisher provides a trusted SHA-256 checksum, you can compare it with the locally calculated value.

For example:

```bash
sha256sum package.deb
```

Compare the result with the checksum obtained through the publisher's trusted channel.

**Important:** A checksum downloaded from the same compromised source as the package does not independently establish authenticity.

A checksum helps verify integrity only when the expected checksum itself is trusted.

---

## 6. Understanding APT Repository Security

APT uses repository configuration files to determine where software information and packages are obtained.

Important locations include:

```text
/etc/apt/sources.list
```

And:

```text
/etc/apt/sources.list.d/
```

Inspect the main repository configuration:

```bash
cat /etc/apt/sources.list
```

Inspect additional repository files:

```bash
ls -lah /etc/apt/sources.list.d/
```

Inspect repository policy:

```bash
apt policy
```

### Why inspect repositories?

You may discover:

- Unexpected repositories.
- Old repository entries.
- Third-party software sources.
- Incorrect distribution suites.
- Sources that are no longer maintained.

### Security warning

Avoid mixing Kali repositories with unrelated Ubuntu or Debian Stable repositories.

Mixing incompatible distribution repositories can create package conflicts and unexpected system changes.

---

## 7. Understanding Repository Signing Keys

APT relies on trusted signing keys to verify repository metadata.

Modern Debian-based systems commonly store repository-specific keys under:

```text
/etc/apt/keyrings/
```

Another traditional location is:

```text
/usr/share/keyrings/
```

Inspect these directories:

```bash
ls -lah /etc/apt/keyrings/
```

```bash
ls -lah /usr/share/keyrings/
```

### Understanding `signed-by`

Repository configurations can specify which keyring should authenticate a particular repository.

For example, a repository configuration may include:

```text
signed-by=/etc/apt/keyrings/example-archive-keyring.gpg
```

This helps restrict which trusted key is used for that repository.

### Important security principle

Do not blindly import signing keys from unknown websites.

A signing key should come from a trusted publisher through an authenticated distribution channel.

---

## 8. Understanding Software Supply-Chain Attacks

A software supply-chain attack occurs when an attacker compromises a component or process involved in creating, distributing, or maintaining software.

For example:

```text
Software Developer
        |
        v
Software Build Process
        |
        v
Package Repository
        |
        v
User Downloads Package
        |
        v
Compromised Software Installed
```

The attacker may target:

- A developer's account.
- A software build environment.
- A package repository.
- A dependency.
- A software update mechanism.

### Examples of supply-chain risks

**1. Malicious dependencies**

A legitimate application may depend on a compromised library.

**2. Compromised repositories**

An attacker may gain unauthorized access to a software distribution system.

**3. Modified installation packages**

A legitimate package may be modified before distribution.

**4. Dependency confusion**

An attacker may exploit naming or repository-priority mistakes to cause software systems to retrieve an unintended package.

### How to reduce these risks

- Use trusted repositories.
- Verify software checksums where appropriate.
- Keep dependencies updated.
- Review repository configuration.
- Avoid unnecessary third-party packages.
- Monitor security advisories.
- Investigate unexpected package changes.

---

## 9. Investigating Installed Packages

Linux administrators should periodically inspect installed software.

### A. List installed packages

```bash
dpkg -l
```

### B. Search for a specific package

```bash
dpkg -l nano
```

### C. Inspect package information

```bash
dpkg -s nano
```

### D. List package files

```bash
dpkg -L nano
```

### E. Identify file ownership

```bash
dpkg -S /usr/bin/nano
```

### F. Inspect package versions

```bash
apt policy nano
```

These commands help you understand what software is installed and how it is managed.

---

## 10. Investigating Suspicious Packages

Imagine you discover an unfamiliar package on a Linux system.

Do not immediately assume it is malicious.

Investigate it systematically.

### Step 1: Identify the package

```bash
dpkg -l
```

### Step 2: Inspect its details

```bash
dpkg -s package-name
```

### Step 3: Examine installed files

```bash
dpkg -L package-name
```

### Step 4: Inspect its dependencies

```bash
apt depends package-name
```

### Step 5: Check package origin and version

```bash
apt policy package-name
```

### Step 6: Review package activity

```bash
less /var/log/dpkg.log
```

### Step 7: Review APT installation history

```bash
less /var/log/apt/history.log
```

### Step 8: Investigate suspicious files

```bash
dpkg -S /path/to/suspicious-file
```

If the file is not owned by a package, further investigation may be necessary.

Remember that package ownership does not prove a file is unmodified or safe.

---

## 11. Understanding Software Updates and Security Patches

Software developers and Linux distributions regularly release security updates.

These updates may correct:

- Software vulnerabilities.
- Authentication weaknesses.
- Memory corruption bugs.
- Privilege-escalation vulnerabilities.
- Remote code execution vulnerabilities.
- Information disclosure problems.

### Update package indexes

```bash
sudo apt update
```

### Review available upgrades

```bash
apt list --upgradable
```

### Inspect a package version

```bash
apt policy nano
```

### Upgrade packages

```bash
sudo apt upgrade
```

Review the proposed changes before confirming.

### Why updates matter

Running outdated software can leave a system exposed to vulnerabilities that have already been publicly documented.

However, administrators should also test important upgrades where necessary, especially on production systems.

---

## 12. Understanding the Principle of Least Privilege

The principle of least privilege means giving users and applications only the permissions they need.

Package installation normally requires administrative privileges because it changes system software.

Example:

```bash
sudo apt install nano
```

However, you should not run every ordinary command with `sudo`.

For example:

```bash
apt search nano
```

Does not normally require administrative privileges.

### Why does this matter?

Excessive administrative privileges can increase the consequences of mistakes or malicious activity.

Use administrative privileges only when required.

---

## 13. Package Management and Attack Surface

The attack surface is the collection of points through which an attacker may attempt to interact with or compromise a system.

Every unnecessary application or service may introduce additional risks.

For example:

- An outdated web server.
- An unnecessary remote-access service.
- An unmaintained library.
- An unused network utility.

Reducing unnecessary software can help reduce the attack surface.

### Inspect installed packages

```bash
dpkg -l
```

### Inspect a package

```bash
dpkg -s package-name
```

### Remove an unnecessary package

After confirming that the package is not required:

```bash
sudo apt remove package-name
```

Do not remove important system packages without understanding their dependencies and consequences.

---

## 14. Practical Lab: Linux Package Security Audit

In this lab, we will perform a basic package-security review of your Kali VM.

All activities are observational. Do not change repository configuration or remove packages during this exercise.

### Task 1: Inspect configured repositories

```bash
cat /etc/apt/sources.list
```

Record which repositories are configured.

### Task 2: Inspect additional repository files

```bash
ls -lah /etc/apt/sources.list.d/
```

### Task 3: Inspect repository policy

```bash
apt policy
```

### Task 4: Inspect trusted keyring directories

```bash
ls -lah /etc/apt/keyrings/
```

If the directory does not exist, record that observation.

### Task 5: Inspect the system keyring directory

```bash
ls -lah /usr/share/keyrings/
```

### Task 6: List installed packages

```bash
dpkg -l
```

### Task 7: Inspect nano

```bash
dpkg -s nano
```

### Task 8: Inspect nano package files

```bash
dpkg -L nano
```

### Task 9: Review package versions

```bash
apt policy nano
```

### Task 10: Review DPKG logs

```bash
less /var/log/dpkg.log
```

### Task 11: Review APT history

```bash
less /var/log/apt/history.log
```

### Task 12: Calculate a file checksum

Create a harmless test file in your home directory:

```bash
echo "Linux package security lab" > ~/package-security-test.txt
```

Calculate its SHA-256 hash:

```bash
sha256sum ~/package-security-test.txt
```

Modify the test file:

```bash
echo "Modified content" >> ~/package-security-test.txt
```

Calculate the hash again:

```bash
sha256sum ~/package-security-test.txt
```

Compare the two values.

They should be different because the file contents changed.

Remove only your test file when finished:

```bash
rm ~/package-security-test.txt
```

---

## 15. Knowledge Check

Answer these questions in your own words.

1. Why is package management important to Linux security?
2. What is a trusted software repository?
3. What is a digital signature?
4. How does APT use repository signatures?
5. What is the purpose of SHA-256?
6. What is a software supply-chain attack?
7. Why can third-party repositories introduce security risks?
8. What does the principle of least privilege mean?
9. How can unnecessary packages increase attack surface?
10. Why is package ownership not proof that a file is safe?

---

## 16. Practical Assignment

Complete the following tasks:

- [ ] Inspect `/etc/apt/sources.list`.
- [ ] Inspect `/etc/apt/sources.list.d/`.
- [ ] Review repository policy using `apt policy`.
- [ ] Inspect `/etc/apt/keyrings/`.
- [ ] Inspect `/usr/share/keyrings/`.
- [ ] List installed packages using `dpkg -l`.
- [ ] Inspect nano using `dpkg -s nano`.
- [ ] List nano's installed files.
- [ ] Inspect nano's installed and candidate versions.
- [ ] Review `/var/log/dpkg.log`.
- [ ] Review `/var/log/apt/history.log`.
- [ ] Complete the SHA-256 file integrity experiment.
- [ ] Write five ways to improve Linux package-management security.

## Lesson Summary

In this lesson, you learned:

- Why package management is a security responsibility.
- How trusted repositories work.
- How repository signatures support authenticity and integrity.
- How SHA-256 checksums help detect file changes.
- How to investigate repository configuration.
- How software supply-chain attacks work.
- How to inspect unfamiliar installed packages.
- Why security updates are important.
- How least privilege and attack-surface reduction improve security.

**Next Lesson: Module 09 – Lesson 15: Module 09 Assessment.**