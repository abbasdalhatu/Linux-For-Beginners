---
---

# Lesson 13: Input, Output, Redirection and Pipes

## 1. Introduction

Linux commands communicate with users and other programs through input and output streams.

For example, when you execute:

```bash
ls
```

Linux displays the results on your terminal.

But what if you want to:
- Save the output into a file?
- Send error messages into a separate file?
- Read information from a file instead of typing it?
- Connect the output of one command to another command?
- Build automated log-processing workflows?

This is where input, output, redirection, and pipes become important.

These concepts are essential for Linux administration, Bash scripting, system monitoring, and ethical hacking.

In this lesson, we will study:

1. Standard input.
2. Standard output.
3. Standard error.
4. Output redirection.
5. Input redirection.
6. Append redirection.
7. Error redirection.
8. Pipes.
9. The `tee` command.
10. Useful text-processing commands.
11. Practical cybersecurity applications.

## 2. Understanding Standard Streams

Linux programs commonly use three standard data streams.

These are called standard input, standard output, and standard error.

| Stream | Name | File descriptor | Purpose |
|---|---|---:|---|
| Standard input | stdin | 0 | Receives input |
| Standard output | stdout | 1 | Displays normal output |
| Standard error | stderr | 2 | Displays error messages |

A file descriptor is a number used by a process to identify an open input or output stream.

### 2.1 Standard Input (stdin)

Standard input is where a program receives information.

By default, it is usually connected to the keyboard in an interactive terminal.

Example:

```bash
cat
```

Type:

```text
Hello Linux
```

Press `Ctrl + D` to indicate the end of input.

The command displays what you entered.

### 2.2 Standard Output (stdout)

Standard output carries normal program results.

Example:

```bash
echo "Linux for Ethical Hackers"
```

The text is displayed on your terminal.

### 2.3 Standard Error (stderr)

Standard error carries diagnostic and error messages.

Example:

```bash
ls nonexistent-file
```

The command displays an error because the requested file does not exist.

The message is written to standard error.

### Why are these streams important?

They allow you to control where information goes.

For example, you can:
- Save normal output into a report.
- Save error messages into an error log.
- Read input from a file.
- Send output from one command into another.

## 3. Understanding Output Redirection

Output redirection changes where a command sends its output.

The most common output redirection operator is:

```bash
>
```

It sends standard output into a file.

### Example

```bash
echo "Hello Linux" > message.txt
```

Instead of displaying the text on the terminal, Bash writes it into `message.txt`.

Display the file:

```bash
cat message.txt
```

Output:

```text
Hello Linux
```

### Important rule

The `>` operator creates a file if it does not exist.

If the file already exists, its previous contents are normally overwritten.

Example:

```bash
echo "First message" > report.txt
echo "Second message" > report.txt
```

The file will contain:

```text
Second message
```

The first message was replaced.

**Be careful when using `>` because it can destroy existing file contents.**

## 4. Understanding Append Redirection

Sometimes you want to add new information without deleting existing information.

Use:

```bash
>>
```

This operator appends standard output to the end of a file.

Example:

```bash
echo "First entry" > log.txt
echo "Second entry" >> log.txt
echo "Third entry" >> log.txt
```

Display:

```bash
cat log.txt
```

Output:

```text
First entry
Second entry
Third entry
```

### Difference between `>` and `>>`

| Operator | Function |
|---|---|
| `>` | Creates or overwrites a file |
| `>>` | Creates or appends to a file |

### Practical example

Imagine you are collecting system information.

```bash
echo "System Report" > system-report.txt
uname -a >> system-report.txt
whoami >> system-report.txt
date >> system-report.txt
```

Display the report:

```bash
cat system-report.txt
```

Each command adds its output to the report.

## 5. Understanding Input Redirection

Input redirection allows a command to receive information from a file.

The operator is:

```bash
<
```

Example:

```bash
sort < names.txt
```

Instead of receiving input from the keyboard, `sort` reads the contents of `names.txt`.

### Another example

Create a file:

```bash
echo "Linux is powerful" > input.txt
```

Read it through input redirection:

```bash
cat < input.txt
```

Output:

```text
Linux is powerful
```

### Why use input redirection?

It is useful when:
- Processing existing files.
- Automating scripts.
- Feeding data into programs.
- Performing repeatable tests.

## 6. Understanding Standard Error Redirection

Remember that standard error uses file descriptor `2`.

You can redirect error messages using:

```bash
2>
```

Example:

```bash
ls missing-file 2> errors.txt
```

The error message is saved in `errors.txt`.

Display it:

```bash
cat errors.txt
```

### Append error messages

Use:

```bash
2>>
```

Example:

```bash
ls missing-file 2>> errors.txt
```

This adds the new error message to the existing file.

### Redirect standard output and standard error separately

Example:

```bash
ls /etc /missing-directory > output.txt 2> errors.txt
```

Here:

- Normal output goes into `output.txt`.
- Error messages go into `errors.txt`.

### Redirect both streams to the same file

Use:

```bash
command > combined.log 2>&1
```

Example:

```bash
ls /etc /missing-directory > combined.log 2>&1
```

This redirects standard output to `combined.log`, then redirects standard error to the same destination.

A shorter Bash form is:

```bash
command &> combined.log
```

Both approaches are commonly used in Bash.

### Important distinction

Redirection order matters.

Compare:

```bash
command > combined.log 2>&1
```

with:

```bash
command 2>&1 > combined.log
```

The first sends both streams to the file.

The second sends standard error to wherever standard output was originally directed, then sends standard output to the file.

## 7. Understanding /dev/null

Linux provides a special device called:

```bash
/dev/null
```

It discards data sent to it.

Think of it as a digital waste container.

### Example

```bash
echo "This message will disappear" > /dev/null
```

Nothing is displayed.

### Suppress error messages

```bash
ls missing-file 2> /dev/null
```

The error message is discarded.

### Suppress normal output

```bash
ls /etc > /dev/null
```

The normal output is discarded.

### Suppress both

```bash
ls /etc missing-file > /dev/null 2>&1
```

Both normal output and errors are discarded.

**Security note:** Suppressing output can be useful, but do not discard important diagnostic information when investigating a system.

## 8. Understanding Pipes

A pipe connects the output of one command to the input of another command.

The pipe operator is:

```bash
|
```

It allows commands to work together.

### Example

```bash
ls /etc | grep ssh
```

What happens?

1. `ls /etc` lists files and directories.
2. Its standard output is sent through the pipe.
3. `grep ssh` receives that output.
4. `grep` displays matching lines.

Instead of saving the first command's output into a temporary file, the pipe passes it directly to the next command.

### Another example

```bash
ps aux | grep bash
```

This searches process information for lines containing `bash`.

### Multiple pipes

You can connect several commands together.

Example:

```bash
ps aux | grep ssh | wc -l
```

The commands work as follows:

1. `ps aux` displays running processes.
2. `grep ssh` filters matching lines.
3. `wc -l` counts the resulting lines.

### Pipe diagram

```text
Command A
   |
   v
Command B
   |
   v
Command C
   |
   v
Final Output
```

Pipes are among the most powerful features of the Linux command line.

## 9. Understanding the tee Command

The `tee` command reads from standard input and writes to both standard output and one or more files.

Example:

```bash
echo "System information" | tee report.txt
```

The message is:
- Displayed on the terminal.
- Written to `report.txt`.

### Append with tee

```bash
echo "Another entry" | tee -a report.txt
```

The `-a` option appends instead of overwriting.

### Practical example

```bash
uname -a | tee system-info.txt
```

This displays system information and saves it into a file.

### Why is tee useful?

It allows you to observe command output while saving it for later analysis.

This is useful when collecting logs, creating reports, and monitoring administrative commands.

## 10. Useful Text-Processing Commands

Redirection and pipes become especially powerful when combined with text-processing commands.

Let's study some important tools.

### 10.1 cat

Displays file contents.

```bash
cat report.txt
```

It can also combine files:

```bash
cat file1.txt file2.txt
```

### 10.2 grep

Searches text for matching patterns.

```bash
grep "error" system.log
```

Case-insensitive search:

```bash
grep -i "error" system.log
```

Display matching line numbers:

```bash
grep -n "error" system.log
```

### 10.3 wc

Counts lines, words, and bytes or characters.

Count lines:

```bash
wc -l report.txt
```

Count words:

```bash
wc -w report.txt
```

Count bytes:

```bash
wc -c report.txt
```

### 10.4 sort

Sorts lines.

```bash
sort names.txt
```

Reverse order:

```bash
sort -r names.txt
```

### 10.5 uniq

Removes adjacent duplicate lines.

```bash
sort names.txt | uniq
```

To count repeated lines:

```bash
sort names.txt | uniq -c
```

Remember that `uniq` works on adjacent duplicates, which is why sorting first is often useful.

### 10.6 head

Displays the beginning of a file.

```bash
head report.txt
```

Display the first five lines:

```bash
head -n 5 report.txt
```

### 10.7 tail

Displays the end of a file.

```bash
tail report.txt
```

Display the last five lines:

```bash
tail -n 5 report.txt
```

Follow a changing log file:

```bash
tail -f system.log
```

Press `Ctrl + C` to stop following it.

### 10.8 cut

Extracts selected fields or character positions from each line.

Example:

```bash
echo "Abbas:Linux:Student" | cut -d ':' -f 2
```

Output:

```text
Linux
```

Here:
- `-d ':'` specifies the delimiter.
- `-f 2` selects the second field.

### 10.9 tr

Translates or deletes characters.

Example:

```bash
echo "linux" | tr 'a-z' 'A-Z'
```

Output:

```text
LINUX
```

### Summary

| Command | Purpose |
|---|---|
| `cat` | Display or combine files |
| `grep` | Search text |
| `wc` | Count lines, words, or bytes |
| `sort` | Sort lines |
| `uniq` | Remove adjacent duplicates |
| `head` | Display the beginning of a file |
| `tail` | Display the end of a file |
| `cut` | Extract fields |
| `tr` | Translate or delete characters |
| `tee` | Display and save input |

## 11. Redirection and Pipes in Cybersecurity

Security professionals frequently process system logs and command output.

Redirection and pipes make this work easier.

### Example 1: Save system information

```bash
uname -a > system-report.txt
```

### Example 2: Search logs for errors

```bash
grep -i "error" /var/log/syslog
```

The exact log file and access permissions depend on your Linux distribution and logging configuration.

### Example 3: Save matching log entries

```bash
grep -i "failed" /var/log/auth.log > failed-logins.txt
```

On Kali or Debian systems, the relevant authentication log may not be present or readable. Some installations use the system journal instead.

### Example 4: Count matching entries

```bash
grep -i "failed" /var/log/auth.log | wc -l
```

### Example 5: Monitor logs

```bash
tail -f /var/log/auth.log
```

This can help administrators observe new authentication-related log entries.

### Example 6: Save and display output

```bash
ip address | tee network-report.txt
```

### Important security lesson

Logs may contain usernames, IP addresses, session details, and other sensitive information.

Protect report files, avoid sharing sensitive logs publicly, and follow applicable authorization rules when investigating systems.

## 12. Practical Lab: Input, Output, Redirection and Pipes

### Lab objective

Learn how to control command input and output, save reports, handle errors, and connect commands using pipes.

Perform these exercises inside your Kali Linux virtual machine.

### Step 1: Create a working directory

```bash
mkdir -p ~/redirection-lab
cd ~/redirection-lab
```

### Step 2: Create a file using output redirection

```bash
echo "Linux for Ethical Hackers" > course.txt
```

Display it:

```bash
cat course.txt
```

### Step 3: Practice append redirection

```bash
echo "Module 4" >> course.txt
echo "Lesson 13" >> course.txt
```

Display the complete file:

```bash
cat course.txt
```

### Step 4: Practice input redirection

```bash
cat < course.txt
```

### Step 5: Practice standard error redirection

```bash
ls missing-file 2> errors.txt
```

Display the error file:

```bash
cat errors.txt
```

### Step 6: Practice separate output and error files

```bash
ls /etc /missing-directory > output.txt 2> error-report.txt
```

Inspect both files:

```bash
cat output.txt
cat error-report.txt
```

### Step 7: Practice /dev/null

```bash
echo "Testing output" > /dev/null
```

Suppress an error:

```bash
ls missing-file 2> /dev/null
```

### Step 8: Practice pipes

```bash
ls /etc | grep ssh
```

Try:

```bash
ps aux | grep bash
```

Then:

```bash
ps aux | wc -l
```

### Step 9: Practice tee

```bash
uname -a | tee system-report.txt
```

Append another command's output:

```bash
whoami | tee -a system-report.txt
```

Display the report:

```bash
cat system-report.txt
```

### Step 10: Practice text processing

Create a sample log:

```bash
cat > security.log <<'EOF'
INFO: System started
WARNING: Unusual login attempt
ERROR: Authentication failed
INFO: User logged out
ERROR: Permission denied
WARNING: Multiple failed attempts
EOF
```

Search for errors:

```bash
grep "ERROR" security.log
```

Search without case sensitivity:

```bash
grep -i "warning" security.log
```

Count errors:

```bash
grep -c "ERROR" security.log
```

Display the first three lines:

```bash
head -n 3 security.log
```

Display the last two lines:

```bash
tail -n 2 security.log
```

### Step 11: Combine commands

Count all error and warning entries:

```bash
grep -Ei "ERROR|WARNING" security.log | wc -l
```

Save the matching entries:

```bash
grep -Ei "ERROR|WARNING" security.log | tee alerts.txt
```

Inspect:

```bash
cat alerts.txt
```

### Step 12: Clean up the lab

When you have finished examining your results:

```bash
cd ~
```

You can keep the `redirection-lab` directory as a practical reference or remove it when you no longer need it.

## 13. Knowledge Check

Answer these questions in your notebook.

1. What is standard input?
2. What is standard output?
3. What is standard error?
4. What are the file descriptors for stdin, stdout, and stderr?
5. What does the `>` operator do?
6. What is the difference between `>` and `>>`?
7. What does `<` do?
8. What does `2>` represent?
9. What is the purpose of `/dev/null`?
10. What is a pipe?
11. What does the `tee` command do?
12. What is the purpose of `grep`?
13. What does `wc -l` count?
14. What is the purpose of `tail -f`?
15. Why is redirection useful in cybersecurity?

## 14. Practical Assignment

Create a Bash script named:

`system-log-analyzer.sh`

The script should create a basic report using sample log information.

### Requirements

1. Create a report directory.
2. Create a sample security log.
3. Save the log using output redirection.
4. Append additional log entries.
5. Search for `ERROR` entries using `grep`.
6. Search for `WARNING` entries.
7. Count matching entries using `wc`.
8. Save matching entries into separate report files.
9. Use `tee` to display and save at least one report.
10. Redirect an error message into an error file.
11. Add comments explaining your commands.

### Suggested starting structure

```bash
#!/bin/bash

# System Log Analyzer

mkdir -p ~/security-report

cd ~/security-report || exit 1

# Create a sample log
cat > security.log <<'EOF'
INFO: System started
ERROR: Authentication failed
WARNING: Unusual login attempt
INFO: System running
ERROR: Permission denied
WARNING: Multiple failed attempts
EOF

# Extract errors
grep "ERROR" security.log > errors.txt

# Extract warnings
grep "WARNING" security.log > warnings.txt

# Count errors
error_count=$(wc -l < errors.txt)

# Count warnings
warning_count=$(wc -l < warnings.txt)

# Display summary
echo "Security Log Report"
echo "Errors: $error_count"
echo "Warnings: $warning_count"
```

Complete the remaining requirements yourself.

**Important:** This assignment uses sample data. Do not scan or collect logs from systems without permission.

## 15. Key Terms

| Term | Definition |
|---|---|
| stdin | Standard input |
| stdout | Standard output |
| stderr | Standard error |
| File descriptor | Number identifying an open stream |
| Redirection | Changing where input or output goes |
| Pipe | Connecting one command's output to another command's input |
| `>` | Overwrite output file |
| `>>` | Append output to a file |
| `<` | Read input from a file |
| `2>` | Redirect standard error |
| `/dev/null` | Special device that discards data |
| `tee` | Display and save input |
| `grep` | Search text for patterns |

## 16. Exam Summary

Remember these important points:

- Linux programs commonly use three standard streams.
- stdin uses file descriptor `0`.
- stdout uses file descriptor `1`.
- stderr uses file descriptor `2`.
- `>` redirects and overwrites standard output.
- `>>` appends standard output.
- `<` redirects standard input.
- `2>` redirects standard error.
- `/dev/null` discards data.
- `|` connects commands together.
- `tee` displays and saves data.
- `grep` searches text.
- `wc -l` counts lines.
- `head` displays the beginning of a file.
- `tail` displays the end of a file.
- `tail -f` follows new log entries.

## 17. Conclusion

Input, output, redirection, and pipes are fundamental Linux command-line concepts.

They allow you to control how programs receive information and where their results are sent.

You can now combine commands to process files, collect system information, inspect logs, and create basic security reports.

These skills are essential for Linux system administration and ethical hacking.

**Next Lesson: Lesson 14 — Shell Configuration and Customization.**