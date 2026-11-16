# Lesson 14: Combining Linux Commands

## 1. Introduction

One of the most powerful features of Linux is the ability to combine multiple commands to perform complex tasks.

Instead of executing every command separately, Linux allows us to connect commands and pass information between them.

For example, imagine you have a log file containing thousands of entries.

You want to:

1. Search for failed login attempts.
2. Count how many occurred.
3. Identify repeated messages.
4. Save the results into a report.

You can accomplish this by combining several commands.

This approach is called **command chaining** or **command combination**.

## 2. Understanding Command Chaining

Command chaining means connecting two or more commands so they work together.

Linux provides several operators for this purpose.

Important operators include:

| Operator | Purpose |
|---|---|
| `;` | Execute commands sequentially |
| `&&` | Execute the next command only if the previous succeeds |
| `||` | Execute the next command only if the previous fails |
| `|` | Pass output from one command to another |
| `&` | Run a command in the background |
| `>` | Redirect output and overwrite a file |
| `>>` | Redirect output and append to a file |

These operators allow you to create efficient command workflows.

## 3. Using the Semicolon (`;`)

The semicolon executes multiple commands sequentially.

Each command runs independently of the previous command's success or failure.

### Example

```bash
pwd; ls; whoami
```

This executes:

1. `pwd` – displays the current directory.
2. `ls` – lists directory contents.
3. `whoami` – displays the current username.

### Another Example

```bash
date; hostname; uptime
```

This displays the date, system hostname, and system uptime.

**Important:** A semicolon does not check whether the previous command succeeded.

## 4. Using the AND Operator (`&&`)

The `&&` operator executes the next command only when the previous command succeeds.

A successful command normally returns exit status `0`.

### Example

```bash
mkdir project && cd project
```

Explanation:

1. Linux creates the directory.
2. If creation succeeds, Linux enters the directory.
3. If creation fails, the second command is not executed.

### Another Example

```bash
sudo apt update && sudo apt upgrade
```

The upgrade command runs only if the update command succeeds.

This is useful when later operations depend on earlier ones.

## 5. Using the OR Operator (`||`)

The `||` operator executes the next command only when the previous command fails.

### Example

```bash
mkdir project || echo "Directory creation failed"
```

If the directory creation fails, the message is displayed.

### Another Example

```bash
cd /unknown-directory || echo "Directory not found"
```

If the directory does not exist or cannot be accessed, the error message is displayed.

### Understanding the Difference

| Operator | When does the next command execute? |
|---|---|
| `&&` | When the previous command succeeds |
| `||` | When the previous command fails |

## 6. Combining AND and OR

You can combine both operators to create simple decision workflows.

Example:

```bash
mkdir project && echo "Created successfully" || echo "Operation failed"
```

This attempts to create a directory and displays a success or failure message.

Be careful with complex combinations because shell operators are evaluated according to shell grammar and command exit statuses.

For more complex workflows, use separate commands or explicit `if` statements in Bash scripts.

## 7. Understanding Pipes (`|`)

A pipe connects the output of one command to the input of another.

It is one of the most important features of Linux.

### Basic Syntax

```bash
command1 | command2
```

The output of `command1` becomes the input of `command2`.

### Example 1: Search Running Processes

```bash
ps aux | grep ssh
```

Explanation:

1. `ps aux` displays running processes.
2. The pipe passes the output to `grep`.
3. `grep ssh` filters lines containing `ssh`.

The result is a filtered process list.

Note that the output may also contain the `grep` command itself.

### Example 2: Count Files

```bash
ls | wc -l
```

This counts the lines produced by `ls`.

For a more reliable count of directory entries, including hidden entries other than `.` and `..`, use:

```bash
find . -mindepth 1 -maxdepth 1 | wc -l
```

This counts entries directly inside the current directory.

### Example 3: Find Unique Names

```bash
sort names.txt | uniq
```

The pipe passes sorted lines into `uniq`.

### Example 4: Count Unique Names

```bash
sort names.txt | uniq | wc -l
```

This counts distinct lines.

## 8. Using Multiple Pipes

You can connect more than two commands.

Example:

```bash
cat system.log | grep ERROR | sort | uniq
```

This performs the following operations:

1. `cat system.log` reads the log file.
2. `grep ERROR` filters lines containing ERROR.
3. `sort` arranges the results.
4. `uniq` removes adjacent duplicates.

A simpler and more efficient version avoids the unnecessary `cat` command:

```bash
grep ERROR system.log | sort | uniq
```

Another example:

```bash
grep ERROR system.log | sort | uniq -c | sort -nr
```

This filters error messages, groups identical lines, counts them, and ranks them by frequency.

## 9. Input and Output Redirection

Linux allows you to redirect command input and output.

This makes it possible to save results, append information, and control where command output goes.

### 9.1 Standard Input

Standard input is represented by file descriptor `0`.

It is commonly received from the keyboard or another command through a pipe.

### 9.2 Standard Output

Standard output is represented by file descriptor `1`.

It normally displays successful command output in the terminal.

### 9.3 Standard Error

Standard error is represented by file descriptor `2`.

It normally displays error messages.

## 10. The Output Redirection Operator (`>`)

The `>` operator sends standard output into a file.

It creates the file if it does not exist.

If the file already exists, its previous contents are overwritten.

Example:

```bash
echo "Linux is powerful" > notes.txt
```

The text is saved in `notes.txt`.

Another example:

```bash
ls > directory-list.txt
```

This saves the directory listing.

**Warning:** Be careful with `>`. It can permanently replace existing file contents.

## 11. The Append Operator (`>>`)

The `>>` operator appends output to the end of a file.

It does not remove existing contents.

Example:

```bash
echo "Linux is useful" >> notes.txt
```

This adds another line to `notes.txt`.

Compare:

```bash
echo "New content" > notes.txt
```

This overwrites the file.

```bash
echo "Additional content" >> notes.txt
```

This appends to the file.

## 12. Redirecting Errors (`2>`)

The `2>` operator redirects standard error.

Example:

```bash
ls /unknown-directory 2> errors.txt
```

The error message is saved in `errors.txt` instead of being displayed in the terminal.

### Append Error Messages

```bash
ls /unknown-directory 2>> errors.txt
```

This appends the error message to the existing file.

### Understanding the File Descriptors

| File Descriptor | Stream |
|---|---|
| `0` | Standard input |
| `1` | Standard output |
| `2` | Standard error |

## 13. Combining Standard Output and Standard Error

Sometimes you want to save both normal output and error messages.

Use:

```bash
command > output.txt 2>&1
```

This redirects standard output to `output.txt`, then redirects standard error to the same destination.

Example:

```bash
find /etc -name "*.conf" > results.txt 2>&1
```

Both matching results and error messages are saved in `results.txt`.

You can also use Bash shorthand:

```bash
command &> output.txt
```

This redirects both standard output and standard error to the same file in Bash.

## 14. Sending Output to `/dev/null`

Linux provides a special device called `/dev/null`.

Anything written to it is discarded.

Example:

```bash
command > /dev/null
```

This discards standard output.

To discard error messages:

```bash
command 2> /dev/null
```

To discard both:

```bash
command > /dev/null 2>&1
```

This is sometimes used when you do not need command output.

**Important:** Redirecting errors to `/dev/null` hides error messages. It does not fix the underlying problem or make the command succeed.

## 15. Using `tee`

The `tee` command reads input and sends it to both the terminal and a file.

### Basic Syntax

```bash
command | tee filename
```

Example:

```bash
ls | tee directory-list.txt
```

This displays the directory listing and saves it in `directory-list.txt`.

### Append Instead of Overwrite

Use `-a`:

```bash
ls | tee -a directory-list.txt
```

This appends the output.

### Example: Save a System Report

```bash
uname -a | tee system-report.txt
```

The system information is displayed and saved.

## 16. Background Execution (`&`)

The ampersand (`&`) runs a command in the background.

Example:

```bash
sleep 60 &
```

The command runs without keeping the terminal occupied.

The shell normally displays a job number and process ID.

To view background jobs:

```bash
jobs
```

To display running processes:

```bash
ps aux
```

To bring a background job to the foreground:

```bash
fg
```

The `&` operator is different from the pipe symbol (`|`).

- `&` runs a command in the background.
- `|` passes output between commands.

## 17. Practical Command Combinations

### Example 1: Display and Save System Information

```bash
uname -a | tee system-info.txt
```

### Example 2: Search and Count Error Messages

```bash
grep -i "error" system.log | wc -l
```

### Example 3: Identify Repeated Messages

```bash
sort system.log | uniq -c | sort -nr
```

### Example 4: Search for Failed Login Attempts

```bash
grep -i "failed" auth.log | sort | uniq -c | sort -nr
```

### Example 5: Save Matching Entries

```bash
grep -i "warning" system.log > warnings.txt
```

### Example 6: Save Results and Display Them

```bash
grep -i "error" system.log | tee errors.txt
```

### Example 7: Count Unique IP Addresses

```bash
sort ips.txt | uniq | wc -l
```

These combinations demonstrate how simple Linux commands can work together to perform more complex tasks.

## 18. Command Chaining in Cybersecurity

Command chaining is especially useful during authorised security investigations.

### 18.1 Analysing Authentication Logs

```bash
grep -i "failed" auth.log | wc -l
```

This counts matching failed-login lines.

### 18.2 Identifying Repeated Log Messages

```bash
grep -i "error" system.log | sort | uniq -c | sort -nr
```

This ranks repeated error messages.

### 18.3 Saving Investigation Results

```bash
grep -i "warning" system.log | tee security-findings.txt
```

This displays and saves matching entries.

### 18.4 Finding Configuration Files and Saving Results

```bash
find /etc -type f -name "*.conf" > config-files.txt 2> search-errors.txt
```

This separates matching paths from permission or other error messages.

### 18.5 Building a Simple Investigation Workflow

```bash
grep -Ei "failed|denied|unauthorized" auth.log | sort | uniq -c | sort -nr
```

This combines filtering, sorting, grouping, and counting.

**Security reminder:** Use these commands only on systems and logs you are authorised to inspect. Protect reports that contain usernames, IP addresses, or other sensitive information.

## 19. Practical Lab: Command Chaining

Create a directory:

```bash
mkdir -p ~/command-chaining-lab
cd ~/command-chaining-lab
```

### Step 1: Create a Sample File

```bash
cat > system.log <<'EOF'
INFO: System started
ERROR: Login failed
WARNING: High CPU usage
ERROR: Login failed
INFO: Network connected
WARNING: Low disk space
ERROR: Service unavailable
EOF
```

### Step 2: Display the Current Directory and Files

```bash
pwd; ls
```

### Step 3: Create a Directory and Enter It

```bash
mkdir reports && cd reports
```

### Step 4: Return to the Previous Directory

```bash
cd ..
```

### Step 5: Search for Errors

```bash
grep ERROR system.log
```

### Step 6: Count Errors

```bash
grep ERROR system.log | wc -l
```

### Step 7: Count Repeated Messages

```bash
grep ERROR system.log | sort | uniq -c
```

### Step 8: Save Error Messages

```bash
grep ERROR system.log > errors.txt
```

### Step 9: Display and Save Warnings

```bash
grep WARNING system.log | tee warnings.txt
```

### Step 10: View the Saved Files

```bash
ls -l
```

Then inspect:

```bash
cat errors.txt
cat warnings.txt
```

## 20. Common Mistakes

### Mistake 1: Confusing `&&` and `||`

Remember:

- `&&` continues after success.
- `||` continues after failure.

### Mistake 2: Confusing Pipes and Redirection

A pipe sends output to another command:

```bash
ls | grep txt
```

Redirection sends output to a file:

```bash
ls > files.txt
```

### Mistake 3: Accidentally Overwriting Files

This overwrites existing content:

```bash
echo "Hello" > notes.txt
```

Use `>>` when you want to append.

### Mistake 4: Forgetting That `uniq` Needs Adjacent Duplicates

Use:

```bash
sort file.txt | uniq
```

when you want to remove duplicate lines regardless of their original positions.

### Mistake 5: Hiding Errors Without Understanding Them

This:

```bash
command 2>/dev/null
```

hides error messages. It does not repair the cause of those errors.

## 21. Commands Summary

| Operator / Command | Purpose |
|---|---|
| `;` | Execute commands sequentially |
| `&&` | Execute the next command after success |
| `||` | Execute the next command after failure |
| `|` | Connect command output to another command |
| `&` | Run a command in the background |
| `>` | Overwrite a file with standard output |
| `>>` | Append standard output |
| `2>` | Redirect standard error |
| `2>&1` | Redirect standard error to standard output's current destination |
| `tee` | Display and save output |
| `/dev/null` | Discard redirected data |

## 22. Knowledge Check

Answer these questions in your notebook.

1. What is command chaining?
2. What is the purpose of the semicolon (`;`)?
3. What is the difference between `&&` and `||`?
4. What does the pipe (`|`) operator do?
5. What is the difference between a pipe and output redirection?
6. What happens when you use `>` on an existing file?
7. What is the purpose of `>>`?
8. What does file descriptor `2` represent?
9. What does `2>/dev/null` do?
10. What is the purpose of the `tee` command?
11. What does the ampersand (`&`) do?
12. Explain `command > output.txt 2>&1`.
13. Why is command chaining useful in cybersecurity investigations?
14. What are the risks of hiding error messages?
15. Explain how `grep`, `sort`, `uniq`, and `wc` can work together.

## 23. Practical Assignment

Complete the following tasks in Kali Linux.

1. Create a directory named `command-practice`.
2. Create a file containing at least ten sample log entries.
3. Execute three commands using `;`.
4. Create a directory and enter it using `&&`.
5. Use `||` to display a message when a directory cannot be accessed.
6. Use a pipe to filter process information.
7. Count matching log entries using `grep` and `wc`.
8. Sort and count repeated log messages.
9. Redirect normal command output to a file.
10. Append additional output to the same file.
11. Redirect an error message into a separate file.
12. Use `tee` to display and save system information.
13. Run a harmless background command using `&`.
14. Explain the difference between standard output and standard error.
15. Build one command pipeline that filters, sorts, counts, and ranks repeated log messages.

## 24. Lesson Conclusion

Command combination is one of the most powerful features of Linux.

You have learned how to:

- Execute multiple commands.
- Control execution using `&&` and `||`.
- Connect commands using pipes.
- Redirect output and errors.
- Append information to files.
- Save output using `tee`.
- Run commands in the background.
- Build practical command pipelines.

**Key lesson:** A Linux professional does not need a separate program for every task. By combining simple commands, you can build powerful workflows for administration, automation, and cybersecurity investigations.

---

**End of Lesson 14 — Combining Linux Commands**