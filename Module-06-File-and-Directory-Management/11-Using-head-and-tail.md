# Module 06 – File and Directory Management

## Lesson 11: Using `head` and `tail`

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand the purpose of `head` and `tail`.
- Display the first lines of a file.
- Display the last lines of a file.
- Control how many lines are displayed.
- Display specific amounts of data in bytes.
- Monitor log files in real time.
- Apply these commands to cybersecurity investigations.

### 2. Introduction

Linux files can contain thousands or even millions of lines.

For example, a system log might contain thousands of recorded events.

Sometimes, you do not need to read the entire file. You may only want to see:

- The first few lines.
- The most recent lines.
- The latest security events.
- The beginning of a configuration file.

Linux provides two commands for this purpose:

| Command | Purpose |
|---|---|
| `head` | Displays the beginning of a file. |
| `tail` | Displays the end of a file. |

### 3. Understanding the `head` Command

The `head` command displays the beginning of a file.

By default, it displays the first 10 lines.

Syntax:

```bash
head [OPTIONS] filename
```

Example:

```bash
head notes.txt
```

This displays the first 10 lines of `notes.txt`.

### 4. Displaying a Specific Number of Lines with `head`

You can control the number of lines using `-n`.

Example:

```bash
head -n 5 notes.txt
```

This displays the first five lines.

Another example:

```bash
head -n 20 security.log
```

This displays the first 20 lines of the log file.

### 5. Understanding the `tail` Command

The `tail` command displays the end of a file.

By default, it displays the last 10 lines.

Syntax:

```bash
tail [OPTIONS] filename
```

Example:

```bash
tail security.log
```

This displays the last 10 lines.

### 6. Displaying a Specific Number of Lines with `tail`

You can specify how many lines to display using `-n`.

Example:

```bash
tail -n 5 security.log
```

This displays the last five lines.

Another example:

```bash
tail -n 20 system.log
```

This displays the last 20 lines.

### 7. Monitoring Files in Real Time with `tail -f`

One of the most important features of `tail` is real-time monitoring.

The `-f` option means follow.

Example:

```bash
tail -f security.log
```

This displays the end of the file and continues watching it for new content.

When another process adds new lines, they appear automatically in your terminal.

To stop monitoring, press:

**CTRL + C**

This is particularly useful when monitoring system logs and application activity.

### 8. Understanding `tail -F`

You may also encounter:

```bash
tail -F security.log
```

The `-F` option follows the filename and retries if the file is temporarily unavailable or replaced.

This can be useful when log rotation occurs.

### 9. Displaying Data in Bytes

Both commands can work with bytes instead of lines.

Examples:

```bash
head -c 50 notes.txt
```

Displays the first 50 bytes.

```bash
tail -c 50 notes.txt
```

Displays the last 50 bytes.

This can be useful when inspecting the beginning or end of a file at the byte level.

### 10. Understanding the Difference

| Feature | `head` | `tail` |
|---|---|---|
| Default output | First 10 lines | Last 10 lines |
| Display specific lines | `-n` | `-n` |
| Display bytes | `-c` | `-c` |
| Real-time monitoring | Not its normal purpose | `-f` |
| Common use | Inspect file beginnings | Inspect recent events |

Remember:

- `head` = beginning.
- `tail` = ending.

### 11. Practical Linux Lab

We will create a sample security log and practise both commands.

#### Step 1: Create the practice directory

```bash
mkdir -p ~/module06-lab/head-tail
cd ~/module06-lab/head-tail
```

#### Step 2: Create a sample log file

```bash
nano security.log
```

Enter:

```text
[INFO] System initialized.
[INFO] Network interface activated.
[INFO] Firewall service started.
[INFO] User authentication successful.
[WARNING] Multiple login attempts detected.
[ERROR] Failed authentication attempt.
[INFO] Security monitoring started.
[WARNING] Unusual network connection detected.
[ERROR] Failed authentication attempt.
[INFO] System backup completed.
[WARNING] Suspicious process detected.
[INFO] Administrator reviewed the alert.
[ERROR] Failed authentication attempt.
[INFO] Security scan completed.
[INFO] System operating normally.
```

Save the file:

- Press `CTRL + O`.
- Press Enter.
- Press `CTRL + X`.

#### Step 3: Display the first 10 lines

```bash
head security.log
```

Observe the output.

#### Step 4: Display the first five lines

```bash
head -n 5 security.log
```

#### Step 5: Display the last 10 lines

```bash
tail security.log
```

#### Step 6: Display the last five lines

```bash
tail -n 5 security.log
```

#### Step 7: Display the first 50 bytes

```bash
head -c 50 security.log
```

#### Step 8: Display the last 50 bytes

```bash
tail -c 50 security.log
```

#### Step 9: Practise real-time monitoring

First, run:

```bash
tail -f security.log
```

Keep this terminal open.

Open another terminal window and execute:

```bash
echo "[WARNING] New login attempt detected." >> ~/module06-lab/head-tail/security.log
```

Return to the first terminal.

You should see the new line appear automatically.

Stop monitoring with `CTRL + C`.

#### Step 10: Verify your file

```bash
tail -n 3 security.log
```

The newly added warning should appear.

### 12. Cybersecurity Applications

These commands are useful in several security activities.

**A. Monitoring authentication events**

```bash
tail -f /var/log/auth.log
```

You can observe new authentication events as they are recorded, if that log exists and you have permission to read it.

**B. Investigating recent system activity**

```bash
tail -n 20 /var/log/syslog
```

Displays the most recent 20 lines of the system log, if available.

**C. Inspecting configuration files**

```bash
head -n 10 /etc/os-release
```

Displays the beginning of the distribution information file.

**D. Monitoring application logs**

```bash
tail -f application.log
```

Useful for observing new application events.

**E. Incident response**

During an investigation, analysts can use `tail` to quickly inspect recent events without reading an entire log file.

### 13. Common Mistakes

| Mistake | Explanation |
|---|---|
| Confusing `head` and `tail` | One displays the beginning; the other displays the end. |
| Forgetting `-n` | The default is 10 lines. |
| Forgetting to stop `tail -f` | It continues monitoring until interrupted. |
| Monitoring a nonexistent file | The command cannot follow a file that is not available. |
| Assuming all systems use the same logs | Log locations and services vary by distribution. |

### 14. Knowledge Check

Answer these questions:

1. What is the purpose of `head`?
2. What is the purpose of `tail`?
3. How many lines do both commands display by default?
4. Which option controls the number of displayed lines?
5. How do you display the first five lines of a file?
6. How do you display the last five lines?
7. What does `tail -f` do?
8. Which keyboard shortcut stops real-time monitoring?
9. What is the difference between `tail -f` and `tail -F`?
10. How can `tail` help during a cybersecurity investigation?

### 15. Lesson Summary

In this lesson, you learned:

- `head` displays the beginning of a file.
- `tail` displays the end of a file.
- Both commands display 10 lines by default.
- `-n` controls the number of lines.
- `-c` controls the number of bytes.
- `tail -f` monitors new content in real time.
- `tail -F` retries when a followed file is replaced or becomes temporarily unavailable.
- `CTRL + C` stops real-time monitoring.

**Next Lesson:** Lesson 12 – File Information and Metadata.