---
---

# LESSON 04 – THE `man` AND `info` COMMANDS

## 1. Introduction

Linux provides built-in documentation systems that help users understand commands, utilities, system calls, configuration files, and programming interfaces.

Two important documentation systems are:

- `man` – Manual pages.
- `info` – GNU Info documentation.

Both help users understand how Linux software works and how to use it correctly.

Although they serve similar purposes, they have different documentation structures and navigation methods.

## 2. Understanding the `man` Command

The word `man` is short for manual.

The `man` command displays manual pages containing detailed information about Linux commands and other system components.

### General Syntax

```bash
man [OPTIONS] PAGE
```

### Basic Example

```bash
man ls
```

This opens the manual page for the `ls` command.

Another example:

```bash
man grep
```

This opens documentation for `grep`.

### What Information Does a Manual Page Contain?

A typical manual page may contain:

1. NAME – The command's name and a short description.
2. SYNOPSIS – How the command should be written.
3. DESCRIPTION – Detailed explanation of its behaviour.
4. OPTIONS – Available options and their meanings.
5. EXAMPLES – Usage examples, when provided.
6. FILES – Related files.
7. SEE ALSO – Other related manual pages.
8. BUGS – Known issues, when documented.

Not every manual page contains every section.

## 3. Understanding Manual Page Sections

Linux manual pages are divided into numbered sections.

| Section | Description | Example |
|---|---|---|
| 1 | User commands | `ls` |
| 2 | System calls | `open` |
| 3 | Library functions | `printf` |
| 4 | Special files and devices | Device interfaces |
| 5 | File formats and configuration | `passwd` |
| 6 | Games | Games |
| 7 | Miscellaneous | Filesystem concepts |
| 8 | System administration commands | `mount` |

The section number helps distinguish entries with the same name.

### Example: The `printf` Manual

Run:

```bash
man 1 printf
```

This opens documentation for the user-level `printf` command.

Now run:

```bash
man 3 printf
```

This opens documentation for the C library `printf()` function, if available.

They have the same name but describe different things.

### Searching for a Manual Page

Use:

```bash
man -k network
```

This searches manual-page descriptions for the word `network`.

It is similar to:

```bash
apropos network
```

### Finding a Specific Section

Use:

```bash
man 1 ls
```

This requests section 1 of the `ls` manual.

## 4. Navigating Manual Pages

Manual pages commonly open in a pager such as `less`.

A pager allows you to read long documents one screen at a time.

Useful navigation keys:

| Key | Function |
|---|---|
| Arrow Up | Move up |
| Arrow Down | Move down |
| Space | Move down one page |
| `b` | Move back one page |
| `g` | Go to the beginning |
| `G` | Go to the end |
| `/word` | Search for a word |
| `n` | Next search result |
| `N` | Previous search result |
| `q` | Quit |

### Example

Run:

```bash
man ls
```

Then:

1. Press `/` and type `--all`.
2. Press Enter.
3. Press `n` to find another match.
4. Press `q` to exit.

## 5. Understanding the `info` Command

The GNU Info system provides structured documentation.

Unlike traditional manual pages, Info documents can contain multiple connected sections called nodes.

These nodes allow readers to move between related topics.

### Basic Syntax

```bash
info COMMAND
```

Example:

```bash
info coreutils
```

This opens documentation for GNU Coreutils.

GNU Coreutils provides many fundamental Linux utilities, including:

- `ls`
- `cp`
- `mv`
- `rm`
- `cat`
- `mkdir`
- `chmod`

### Another Example

```bash
info bash
```

This opens Bash documentation if the relevant Info document is installed.

## 6. Navigating Info Documentation

The Info viewer supports several navigation keys.

| Key | Function |
|---|---|
| `n` | Next node |
| `p` | Previous node |
| `u` | Move up one level |
| `t` | Top-level directory |
| `l` | Return to previous node |
| `Enter` | Follow a selected menu item or link |
| `q` | Quit |

### Example

Run:

```bash
info coreutils
```

Practise:

1. Moving to the next node.
2. Moving to the previous node.
3. Moving up one level.
4. Returning to the top-level directory.
5. Exiting with `q`.

## 7. Differences Between `man` and `info`

| Feature | man | info |
|---|---|---|
| Documentation style | Manual pages | Structured documents |
| Organisation | Numbered manual sections | Connected nodes |
| Navigation | Pager-based | Node-based |
| Main purpose | Command and system reference | Detailed GNU documentation |
| Availability | Common across Linux systems | Depends on installed documentation |
| Navigation keys | `less`-style keys | Info-specific keys |

Neither system completely replaces the other.

Use whichever provides the information you need.

## 8. Reading Documentation Efficiently

You do not always need to read an entire manual page.

A practical approach is:

### Step 1 – Identify the Command

Example:

```bash
man grep
```

### Step 2 – Read the NAME Section

Understand the command's purpose.

### Step 3 – Examine the SYNOPSIS

Learn the correct command structure.

### Step 4 – Inspect Relevant Options

Find the option you need.

### Step 5 – Read Examples or Related Documentation

Use the available examples and references to improve your understanding.

### Step 6 – Test Safely

Try the command on sample files or inside your own lab.

## 9. Finding Documentation Files

The `manpath` command displays directories used to locate manual pages.

```bash
manpath
```

You can also inspect the manual directories:

```bash
ls /usr/share/man
```

The Info documentation directory is commonly:

```bash
ls /usr/share/info
```

These locations depend on the Linux distribution and installed packages.

## 10. What If Documentation Is Missing?

You may encounter:

```text
No manual entry for COMMAND
```

Possible reasons include:

- The relevant documentation package is missing.
- The manual database is incomplete.
- The command does not have a manual page.
- The command name is incorrect.

On Kali Linux, you can install common documentation packages when needed:

```bash
sudo apt update
sudo apt install man-db manpages info
```

Do not install them again if they are already installed.

You can check whether the programs exist:

```bash
command -v man
command -v info
```

## 11. Practical Laboratory

Perform these exercises inside your Kali Linux VM.

### Exercise 1 – Explore Manual Pages

Open:

```bash
man ls
```

Find the following information:

- Command description.
- Meaning of the `-a` option.
- Meaning of the `-l` option.
- How to exit the manual.

### Exercise 2 – Explore Manual Sections

Run:

```bash
man 1 printf
```

Then:

```bash
man 3 printf
```

Explain why the two entries are different.

### Exercise 3 – Search Documentation

Run:

```bash
man -k file
```

Identify three commands related to files.

### Exercise 4 – Explore GNU Info

Run:

```bash
info coreutils
```

Practise moving between nodes and exiting.

If the documentation is unavailable, record the error instead of assuming the command is broken.

### Exercise 5 – Find Documentation Directories

Run:

```bash
manpath
ls /usr/share/man
ls /usr/share/info
```

Observe the directories and explain what they contain.

## 12. Security Applications

Documentation is important when working with cybersecurity tools.

For example, before using a command to inspect system processes, you can read:

```bash
man ps
```

Before changing file permissions, you can read:

```bash
man chmod
```

Before managing system services, you can read:

```bash
man systemctl
```

Documentation helps prevent accidental changes and incorrect command usage.

Always understand the effect of a command before running it with administrator privileges.

## 13. Lesson Summary

In this lesson, you learned:

1. What the `man` command does.
2. How manual pages are organised.
3. The purpose of manual sections.
4. How to navigate manual pages.
5. What the GNU Info system is.
6. How Info documentation is structured.
7. How to navigate Info nodes.
8. The differences between `man` and `info`.
9. How to locate documentation directories.
10. How documentation improves safe Linux administration.

## 14. Knowledge Check

Answer these questions:

1. What does `man` stand for?
2. What is the purpose of a manual page?
3. What information is normally found in the SYNOPSIS section?
4. What is manual section 1 used for?
5. What is manual section 5 used for?
6. Why might you use `man 3 printf` instead of `man 1 printf`?
7. What is GNU Info?
8. What is the difference between manual pages and Info nodes?
9. Which key exits the Info viewer?
10. Why should you read documentation before executing unfamiliar administrative commands?

## 15. Practical Assignment

Complete these tasks:

1. Open the manual page for `mkdir`.
2. Find the meaning of the `-p` option.
3. Open the manual page for `chmod`.
4. Open the documentation for Bash using `info bash`, if available.
5. Search manual descriptions for commands related to processes.
6. Locate your system's manual documentation directories.

Record the commands you used and explain what you discovered.

**End of Lesson 04 – The `man` and `info` Commands.**