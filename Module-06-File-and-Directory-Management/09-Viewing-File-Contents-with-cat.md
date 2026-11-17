---
---

# Module 06 – File and Directory Management

## Lesson 09: Viewing File Contents with `cat`

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand the purpose of the `cat` command.
- Display file contents in the terminal.
- Read multiple files.
- Combine files using `cat`.
- Display line numbers.
- Append and redirect file content.
- Understand how `cat` is useful in cybersecurity.

### 2. Introduction to the `cat` Command

The Linux `cat` command stands for **concatenate**.

It is used to display, combine, and manage text from files.

For example, imagine you have a file called `notes.txt` containing:

```text
Linux is an operating system.
Linux is widely used in cybersecurity.
```

You can display its contents with:

```bash
cat notes.txt
```

Output:

```text
Linux is an operating system.
Linux is widely used in cybersecurity.
```

The contents appear directly in your terminal.

### 3. Basic Syntax

```bash
cat [OPTIONS] [FILE]
```

Where:

- `cat` is the command.
- `OPTIONS` are optional settings.
- `FILE` is the file you want to read.

### 4. Displaying a Single File

To display a file:

```bash
cat notes.txt
```

This prints the entire content of `notes.txt`.

Another example:

```bash
cat /etc/os-release
```

This displays information about your Linux distribution.

### 5. Displaying Multiple Files

You can display multiple files with one command.

Example:

```bash
cat notes.txt report.txt
```

Linux displays the contents of both files in sequence.

The output of the first file appears before the output of the second.

### 6. Displaying Line Numbers

The `-n` option numbers every line.

Example:

```bash
cat -n notes.txt
```

Possible output:

```text
     1  Linux is an operating system.
     2  Linux is widely used in cybersecurity.
     3  Linux supports command-line tools.
```

Another useful option is `-b`.

```bash
cat -b notes.txt
```

The `-b` option numbers only non-empty lines.

### 7. Displaying Special Characters

The `-A` option displays certain invisible characters and line endings.

Example:

```bash
cat -A notes.txt
```

This can help you identify formatting issues, such as tabs and line endings.

It is useful when troubleshooting text files and configuration files.

### 8. Combining Files

Because `cat` means concatenate, it can combine multiple files.

Suppose you have:

- `part1.txt`
- `part2.txt`

To display both together:

```bash
cat part1.txt part2.txt
```

To save their combined content into another file:

```bash
cat part1.txt part2.txt > combined.txt
```

The `>` symbol redirects output into a file.

**Important:** If `combined.txt` already exists, `>` replaces its previous contents.

### 9. Understanding Output Redirection

Linux uses redirection symbols to control where command output goes.

#### A. Output Redirection: `>`

```bash
cat notes.txt > backup.txt
```

This copies the displayed text from `notes.txt` into `backup.txt`.

If `backup.txt` already exists, its contents are overwritten.

#### B. Append Redirection: `>>`

```bash
cat notes.txt >> backup.txt
```

This adds the contents of `notes.txt` to the end of `backup.txt`.

It does not remove the existing content.

Remember:

| Symbol | Meaning |
|---|---|
| `>` | Create or overwrite a file. |
| `>>` | Append to a file. |

### 10. Creating a File with `cat`

You can create a text file using `cat` and input redirection.

Example:

```bash
cat > introduction.txt
```

After pressing Enter, type:

```text
Linux is powerful.
Linux is useful for system administration.
Linux is important in cybersecurity.
```

To finish and save, press:

**CTRL + D**

You have now created a text file using `cat`.

Verify:

```bash
cat introduction.txt
```

### 11. Appending Text Using `cat`

You can add more content to an existing file.

Example:

```bash
cat >> introduction.txt
```

Type:

```text
Linux also supports scripting and automation.
```

Press `CTRL + D` to finish.

Verify:

```bash
cat introduction.txt
```

The new sentence should appear after the original content.

### 12. Important `cat` Options

| Option | Purpose |
|---|---|
| `-n` | Number all output lines. |
| `-b` | Number non-empty lines. |
| `-A` | Show certain non-printing characters. |
| `-s` | Suppress repeated empty lines. |
| `-E` | Show `$` at the end of each line. |
| `-T` | Display tab characters visibly. |

Examples:

```bash
cat -n notes.txt
cat -b notes.txt
cat -E notes.txt
cat -T notes.txt
cat -A notes.txt
```

### 13. Limitations of `cat`

Although `cat` is useful, it is not always the best tool for reading large files.

For example:

```bash
cat /var/log/syslog
```

A large log file may produce too much output to read comfortably.

For large files, Linux provides tools such as:

- `less`
- `more`
- `head`
- `tail`

We will study these in upcoming lessons.

### 14. Practical Linux Lab

All exercises will take place inside your course practice directory.

#### Step 1: Create the practice environment

```bash
mkdir -p ~/module06-lab/cat-practice
cd ~/module06-lab/cat-practice
```

#### Step 2: Create your first file

```bash
cat > notes.txt
```

Enter:

```text
Linux is an operating system.
Linux is used by system administrators.
Linux is important in cybersecurity.
```

Press `CTRL + D`.

#### Step 3: Display the file

```bash
cat notes.txt
```

#### Step 4: Display numbered lines

```bash
cat -n notes.txt
```

#### Step 5: Create another file

```bash
cat > security.txt
```

Enter:

```text
Cybersecurity involves protecting systems.
Ethical hacking requires authorization.
Linux provides useful security tools.
```

Press `CTRL + D`.

#### Step 6: Display both files

```bash
cat notes.txt security.txt
```

#### Step 7: Combine both files

```bash
cat notes.txt security.txt > combined.txt
```

Verify:

```bash
cat combined.txt
```

#### Step 8: Append more content

```bash
cat >> combined.txt
```

Enter:

```text
Learning Linux requires consistent practice.
```

Press `CTRL + D`.

Verify:

```bash
cat combined.txt
```

#### Step 9: Inspect special characters

```bash
cat -A combined.txt
```

Observe the `$` symbols at the ends of lines.

#### Step 10: Inspect all practice files

```bash
ls -lh
```

You should have:

```text
notes.txt
security.txt
combined.txt
```

### 15. Cybersecurity Applications

The `cat` command is useful in several cybersecurity activities.

**A. Inspecting system information**

```bash
cat /etc/os-release
```

**B. Reading configuration files**

```bash
cat /etc/hostname
```

**C. Examining small log files**

```bash
cat application.log
```

**D. Reviewing security-related settings**

```bash
cat /etc/ssh/sshd_config
```

Note: Some configuration files may require elevated permissions to read. Do not change system configurations unless you understand the consequences.

**E. Reviewing investigation notes**

Security analysts use text files to record findings, observations, and incident information.

### 16. Common Mistakes

| Mistake | Explanation |
|---|---|
| Forgetting the filename | `cat` needs input or a file to display. |
| Using `>` instead of `>>` | Existing content may be overwritten. |
| Forgetting CTRL + D | Interactive input remains open. |
| Using `cat` on huge files | Too much output may fill the terminal. |
| Assuming `cat` edits files | It displays and combines text; it is not a full text editor. |

### 17. Knowledge Check

Answer these questions:

1. What does `cat` stand for?
2. What is the main purpose of the `cat` command?
3. Which option displays line numbers?
4. What is the difference between `cat -n` and `cat -b`?
5. What does the `>` symbol do?
6. What does the `>>` symbol do?
7. How do you create a file using `cat`?
8. Which keyboard shortcut ends interactive `cat` input?
9. Why is `cat` not ideal for very large files?
10. Which command displays your Linux distribution information?

### 18. Lesson Summary

In this lesson, you learned:

- `cat` displays file contents.
- `cat -n` numbers all lines.
- `cat -b` numbers non-empty lines.
- `cat -A` displays certain special characters.
- `>` redirects and overwrites output.
- `>>` appends output.
- `cat` can combine multiple files.
- `CTRL + D` ends interactive input.

**Next Lesson:** Lesson 10 – Viewing Files with `less` and `more`.