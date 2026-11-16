# Module 06 – File and Directory Management

## Lesson 10: Viewing Files with `less` and `more`

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand why Linux provides `less` and `more`.
- Display large files without flooding the terminal.
- Navigate through files page by page.
- Search for words and phrases inside files.
- Understand the differences between `less` and `more`.
- Use these commands to inspect Linux logs and configuration files.

### 2. Introduction

In the previous lesson, you learned how to use `cat` to display file contents.

However, imagine opening a log file containing 10,000 lines using:

```bash
cat system.log
```

The terminal may fill with thousands of lines, making it difficult to find the information you need.

Linux provides two useful commands:

- `more`
- `less`

Both allow you to read files page by page instead of displaying everything at once.

### 3. Understanding the `more` Command

The `more` command displays file contents one screen at a time.

Syntax:

```bash
more filename
```

Example:

```bash
more notes.txt
```

The file opens in a paginated view.

#### Important navigation keys

| Key | Function |
|---|---|
| `Space` | Move forward one screen. |
| `Enter` | Move forward one line. |
| `b` | Move backward one screen where supported. |
| `/word` | Search forward for a word. |
| `q` | Quit and return to the terminal. |

**Note:** Navigation capabilities can vary slightly between implementations.

### 4. Understanding the `less` Command

The `less` command is a more flexible file viewer.

Syntax:

```bash
less filename
```

Example:

```bash
less notes.txt
```

Unlike traditional `more`, `less` allows convenient forward and backward navigation.

#### Important navigation keys

| Key | Function |
|---|---|
| `Space` | Move forward one screen. |
| `b` | Move backward one screen. |
| `Enter` | Move down one line. |
| `k` | Move up one line. |
| `g` | Go to the beginning. |
| `G` | Go to the end. |
| `/word` | Search forward. |
| `?word` | Search backward. |
| `n` | Find the next search match. |
| `N` | Find the previous search match. |
| `q` | Quit. |

### 5. Searching Inside Files

One of the most useful features of `less` is searching.

Suppose you are reading a security log and want to find the word `failed`.

Open the file:

```bash
less security.log
```

Inside the viewer, type:

```text
/failed
```

Press Enter.

The viewer searches forward for matching text.

To find another occurrence, press:

```text
n
```

To search backward, type:

```text
?failed
```

This is particularly useful when examining long logs.

### 6. Understanding the Difference Between `more` and `less`

| Feature | `more` | `less` |
|---|---|---|
| Page-by-page viewing | Yes | Yes |
| Forward navigation | Yes | Yes |
| Backward navigation | Limited or implementation-dependent | Yes |
| Search | Yes | Yes |
| Go to beginning | Limited | Yes |
| Go to end | Limited | Yes |
| Best for flexible navigation | Less suitable | More suitable |

A useful way to remember:

- `more` provides basic paginated viewing.
- `less` provides more flexible navigation.

### 7. Viewing Linux System Files

These commands can help you inspect system information.

Example:

```bash
less /etc/os-release
```

This displays information about your Linux distribution.

Another example:

```bash
less /etc/passwd
```

This displays account information recorded in the system's password database file.

**Security note:** `/etc/passwd` normally contains account information, not users' actual password hashes. Those are generally stored in `/etc/shadow`, which has restricted permissions.

### 8. Viewing Log Files

Linux systems maintain logs that record system and application activities.

Examples of common log locations include:

```text
/var/log/
/var/log/auth.log
/var/log/syslog
```

The exact log filenames depend on the distribution and logging configuration.

You can inspect an available log with:

```bash
less /var/log/auth.log
```

If the file is unavailable or permission is denied, do not assume your system is broken. Check which logs exist and whether your account has permission to read them.

### 9. Practical Linux Lab

We will create a sample log file and practise navigating it.

#### Step 1: Create your practice directory

```bash
mkdir -p ~/module06-lab/less-more
cd ~/module06-lab/less-more
```

#### Step 2: Create a sample log file

```bash
nano security.log
```

Enter the following sample information:

```text
[INFO] System started.
[INFO] User Abbas logged in.
[WARNING] Multiple login attempts detected.
[ERROR] Failed authentication attempt.
[INFO] Firewall service started.
[WARNING] Unusual network activity detected.
[INFO] Security scan completed.
[ERROR] Failed authentication attempt.
[INFO] System backup completed.
[WARNING] Suspicious process detected.
[INFO] Administrator reviewed the system.
[ERROR] Failed authentication attempt.
```

Save the file in Nano:

- Press `CTRL + O`.
- Press Enter.
- Press `CTRL + X`.

#### Step 3: Open the file using `more`

```bash
more security.log
```

Practise:

- Moving forward.
- Moving one line at a time.
- Searching for `ERROR`.
- Exiting with `q`.

#### Step 4: Open the file using `less`

```bash
less security.log
```

Practise:

- Pressing Space.
- Pressing `b`.
- Pressing `g`.
- Pressing `G`.
- Searching for `/WARNING`.
- Pressing `n` to find another occurrence.
- Exiting with `q`.

#### Step 5: Inspect a real Linux information file

```bash
less /etc/os-release
```

Read the information and exit using `q`.

#### Step 6: Verify your practice files

```bash
ls -lh
```

You should see:

```text
security.log
```

### 10. Cybersecurity Applications

The `less` and `more` commands are useful in security investigations.

**A. Authentication log analysis**

Investigators can search through authentication logs to identify failed login attempts.

```bash
less /var/log/auth.log
```

**B. System troubleshooting**

Administrators can inspect application and system logs.

**C. Incident response**

Security analysts can review recorded events to understand what happened on a system.

**D. Configuration inspection**

System administrators can examine configuration files without modifying them.

**E. Large evidence files**

When investigating text-based evidence, `less` allows analysts to inspect large files without flooding the terminal.

### 11. Common Mistakes

| Mistake | Explanation |
|---|---|
| Forgetting to press `q` | The viewer remains open. |
| Typing a search command outside the viewer | Search keys must be used inside `less` or `more`. |
| Confusing `/word` and `?word` | `/` searches forward; `?` searches backward. |
| Assuming every log exists | Log locations vary by Linux distribution. |
| Using `cat` for huge files | It can produce overwhelming terminal output. |

### 12. Knowledge Check

Answer these questions:

1. What is the purpose of `more`?
2. What is the purpose of `less`?
3. Which command allows flexible backward navigation?
4. Which key moves one screen forward?
5. Which key exits `less`?
6. How do you search forward for the word `failed`?
7. What does the `g` key do in `less`?
8. What does the `G` key do?
9. Why is `less` useful when investigating large log files?
10. What is one important difference between `more` and `less`?

### 13. Lesson Summary

In this lesson, you learned:

- `more` displays files one screen at a time.
- `less` supports flexible forward and backward navigation.
- `Space` moves forward one screen.
- `b` moves backward one screen in `less`.
- `/word` searches forward.
- `?word` searches backward.
- `n` finds the next match.
- `q` exits the viewer.

**Next Lesson:** Lesson 11 – Using `head` and `tail`.