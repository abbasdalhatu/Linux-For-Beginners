# LESSON 09: UPDATING AND UPGRADING KALI LINUX

**Module:** 02 – Kali Linux Setup  
**File:** `09-Updating-and-Upgrading-Kali.md`

## 1. Introduction

Linux systems receive regular updates to improve security, fix bugs, improve performance, and introduce new features.

Kali Linux uses APT (Advanced Package Tool) to manage software packages.

In this lesson, we will learn how to update package information, upgrade installed software, and maintain our Kali Linux system.

## 2. Understanding APT

APT stands for Advanced Package Tool.

It is a package management system used by Debian-based distributions, including Kali Linux and Ubuntu.

APT helps us:

- Install software.
- Remove software.
- Update installed packages.
- Upgrade the operating system's packages.
- Resolve software dependencies.

A package is a collection of files and information required to install a particular application or component.

## 3. Understanding Kali Repositories

A software repository is a location containing software packages and package information.

Kali retrieves package information from its configured repositories.

The main repository configuration is commonly found in:

`/etc/apt/sources.list`

Additional repository configuration may be stored in:

`/etc/apt/sources.list.d/`

To inspect your repository configuration, run:

```bash
cat /etc/apt/sources.list
```

You can also inspect additional repository files:

```bash
ls /etc/apt/sources.list.d/
```

**Security rule:** Only configure trusted official repositories. Never add an unknown repository just because a website recommends it.

## 4. Update Package Information

Open your Kali terminal.

Run:

```bash
sudo apt update
```

This command downloads the latest package information from your configured repositories.

Important:

`apt update` does not upgrade all installed applications.

It refreshes the information APT uses to determine which package versions are available.

### Understanding the command

| Component | Meaning |
|---|---|
| `sudo` | Runs the command with administrative privileges |
| `apt` | Package management tool |
| `update` | Refreshes package information |

## 5. Upgrade Installed Packages

After updating package information, run:

```bash
sudo apt upgrade
```

This upgrades installed packages when newer compatible versions are available.

APT may ask you to confirm the operation.

Read the summary before accepting.

If it proposes removing important packages or making unexpected changes, stop and investigate before continuing.

## 6. Understanding Full Upgrade

Kali is a rolling-release distribution.

This means it receives continuous updates rather than relying only on occasional major-version releases.

For this reason, Kali documentation commonly recommends:

```bash
sudo apt full-upgrade
```

Unlike a normal upgrade, `full-upgrade` can install or remove packages when necessary to complete an upgrade and resolve dependencies.

**Important:** Review the proposed changes before confirming. Do not blindly approve package removals.

## 7. Difference Between APT Commands

| Command | Purpose |
|---|---|
| `sudo apt update` | Refreshes package information |
| `sudo apt upgrade` | Upgrades installed packages |
| `sudo apt full-upgrade` | Performs a full upgrade and may remove conflicting packages |
| `sudo apt install package-name` | Installs a package |
| `sudo apt remove package-name` | Removes a package but generally keeps configuration files |
| `sudo apt purge package-name` | Removes a package and its package-managed configuration |
| `sudo apt autoremove` | Removes automatically installed packages no longer needed |

## 8. Installing a Package

Let's install a simple diagnostic utility.

Run:

```bash
sudo apt install tree
```

The `tree` command displays directory structures in a tree-like format.

After installation, test it:

```bash
tree ~/Linux-Ethical-Hacking
```

Expected output will resemble:

```text
/home/kaliuser/Linux-Ethical-Hacking
├── Labs
├── Notes
├── Reports
└── Tools
```

Your home directory name may differ depending on the username you created.

## 9. Searching for Packages

To search for a package:

```bash
apt search nmap
```

This searches available package information for Nmap.

To display information about a package:

```bash
apt show nmap
```

To see whether a package is installed:

```bash
dpkg -s tree
```

## 10. Cleaning Package Cache

APT stores downloaded package files in its local cache.

To remove package files that are no longer needed:

```bash
sudo apt clean
```

To remove obsolete package files:

```bash
sudo apt autoclean
```

These commands help manage storage.

Do not confuse cleaning the package cache with removing installed applications.

## 11. Check for System Updates

You can inspect available package upgrades with:

```bash
apt list --upgradable
```

This displays installed packages for which newer versions are available.

To inspect your Kali release information:

```bash
cat /etc/os-release
```

## 12. Important Maintenance Rules

Before performing a major upgrade:

1. Make sure your VM has a working internet connection.
2. Ensure sufficient free storage.
3. Consider taking a VMware snapshot.
4. Use trusted Kali repositories.
5. Read package removal and installation summaries.
6. Avoid interrupting an upgrade.
7. Restart Kali if important system components or the kernel were upgraded and a restart is required.

**Remember:** A VMware snapshot is a convenient rollback point, but it is not a replacement for a proper backup.

## 13. Practical Assignment

Complete these tasks:

- [ ] Inspect your APT repository configuration.
- [ ] Run `sudo apt update`.
- [ ] Run `sudo apt upgrade`.
- [ ] Review the proposed changes before confirming.
- [ ] Install the `tree` package.
- [ ] Display your course workspace using `tree`.
- [ ] Search for Nmap using `apt search nmap`.
- [ ] Inspect Nmap package information using `apt show nmap`.
- [ ] Check installed package status using `dpkg -s tree`.
- [ ] Run `apt list --upgradable`.

If you decide to perform a full upgrade, use `sudo apt full-upgrade` only after reviewing the proposed changes and ensuring you have enough storage.

## 14. Knowledge Check

1. What does APT stand for?
2. What is a software repository?
3. What is the difference between `apt update` and `apt upgrade`?
4. What makes `apt full-upgrade` different from `apt upgrade`?
5. What does `apt install` do?
6. What is the purpose of `apt search`?
7. Why should we use trusted software repositories?
8. Why should we inspect package removals before confirming an upgrade?

## 15. Lesson Summary

In this lesson, we learned how to maintain Kali Linux using APT.

We covered package repositories, updating package information, upgrading installed software, installing utilities, searching for packages, and cleaning the package cache.

These skills are important because a cybersecurity professional must understand how to maintain a secure and updated Linux environment.

**Next Lesson:** Lesson 10 – Kali Users and Administrative Access.