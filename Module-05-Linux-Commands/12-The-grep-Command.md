---
---

# Lesson 12: The `grep` Command

## 1. Introduction

Linux systems contain many files with large amounts of text.

For example:
- System log files.
- Configuration files.
- Application logs.
- User account information.
- Network configuration.
- Script files.

Searching through these files manually can take a long time.

Linux provides a powerful command called `grep` that searches text for matching patterns.

The word `grep` is commonly expanded as **Global Regular Expression Print**.

The command searches input for lines matching a specified pattern and displays those lines.

## 2. Basic Syntax

```bash
grep [OPTIONS] PATTERN [FILE...]
```

Where:

- `grep` is the command.
- `OPTIONS` control how the search behaves.
- `PATTERN` is the text or regular expression you want to find.
- `FILE` is the file you want to search.

### Example

Create a sample file:

```bash
echo "Linux is powerful" > notes.txt
echo "Linux is useful for cybersecurity" >> notes.txt
echo "Windows is another operating system" >> notes.txt
```

Search for the word Linux:

```bash
grep "Linux" notes.txt
```

Expected output:

```text
Linux is powerful
Linux is useful for cybersecurity
```

`grep` displays the lines containing the requested pattern.

## 3. Case-Insensitive Searching

By default, `grep` distinguishes uppercase and lowercase letters.

For example:

```bash
grep "linux" notes.txt
```

This will not match `Linux`.

Use `-i` to ignore letter case:

```bash
grep -i "linux" notes.txt
```

This can match:
- Linux
- LINUX
- linux
- LiNuX

## 4. Display Line Numbers

Use `-n` to display the line number where a match occurs.

```bash
grep -n "Linux" notes.txt
```

Example output:

```text
1:Linux is powerful
2:Linux is useful for cybersecurity
```

This is useful when locating information inside large files.

## 5. Search for Complete Words

Sometimes you want to find a complete word rather than matching a word that contains the same letters.

Use `-w`.

Example:

```bash
grep -w "Linux" notes.txt
```

This searches for `Linux` as a complete word.

Compare:

```bash
grep "net" notes.txt
```

This can match words containing `net`.

```bash
grep -w "net" notes.txt
```

This matches `net` as a complete word.

## 6. Display Only Matching Parts

Use `-o` to display only the matching portion of each line.

Example:

```bash
grep -o "Linux" notes.txt
```

Possible output:

```text
Linux
Linux
```

This is useful when extracting matching text from larger lines.

## 7. Count Matching Lines

Use `-c` to count the number of lines containing a match.

```bash
grep -c "Linux" notes.txt
```

Example output:

```text
2
```

Important: `-c` counts matching lines, not necessarily the total number of times the pattern appears.

## 8. Display Lines That Do Not Match

Use `-v` to invert the search.

Example:

```bash
grep -v "Linux" notes.txt
```

This displays lines that do not contain `Linux`.

This is called an inverse match.

## 9. Search Multiple Files

You can search multiple files with one command.

Create two files:

```bash
echo "Linux administration" > file1.txt
echo "Cybersecurity training" > file2.txt
```

Search both:

```bash
grep "Linux" file1.txt file2.txt
```

The command checks both files.

To search all text files in the current directory:

```bash
grep "Linux" *.txt
```

## 10. Recursive Searching

Use `-r` to search through files in a directory and its subdirectories.

Example:

```bash
grep -r "password" ~/Documents
```

This searches files under the Documents directory for the word `password`.

For filenames and line numbers:

```bash
grep -rn "password" ~/Documents
```

For case-insensitive recursive searching:

```bash
grep -rin "password" ~/Documents
```

**Security note:** Searching for the word `password` does not mean that every match is a credential or vulnerability. Investigate the surrounding context carefully. Avoid unnecessarily displaying or sharing secrets.

## 11. Searching for Patterns with Regular Expressions

`grep` supports pattern matching using regular expressions.

Regular expressions allow you to search for more flexible patterns than ordinary text.

### 11.1 Search for Lines Beginning with a Word

The caret symbol `^` represents the beginning of a line.

```bash
grep "^Linux" notes.txt
```

This finds lines beginning with `Linux`.

### 11.2 Search for Lines Ending with a Word

The dollar symbol `$` represents the end of a line.

```bash
grep "Linux$" notes.txt
```

This finds lines ending with `Linux`.

### 11.3 Search for Any Single Character

The dot `.` represents any single character in a regular expression.

```bash
grep "L.nux" notes.txt
```

This can match patterns such as `Linux`.

### 11.4 Search for One of Several Characters

Square brackets define a character set.

```bash
grep "[Ll]inux" notes.txt
```

This matches either `Linux` or `linux`.

### 11.5 Search for Lines Containing Numbers

```bash
grep "[0-9]" notes.txt
```

This finds lines containing at least one digit.

## 12. Extended Regular Expressions

Use `-E` to enable extended regular expressions.

Extended regular expressions provide additional operators.

### Example 1: Search for Two Alternatives

```bash
grep -E "Linux|Windows" notes.txt
```

This searches for either `Linux` or `Windows`.

### Example 2: Search for Multiple Possible Words

```bash
grep -E "error|warning|failed" notes.txt
```

This finds lines containing any of the three patterns.

### Example 3: Match One or More Digits

```bash
grep -E "[0-9]+" notes.txt
```

The `+` means one or more repetitions of the preceding pattern.

## 13. Searching Compressed and Large Logs

System logs can contain a large amount of information.

You can use `grep` to filter relevant lines.

For example:

```bash
grep "error" /var/log/syslog
```

On systems using a different logging arrangement, the file may not exist. You can inspect available log files first:

```bash
ls /var/log
```

To search case-insensitively:

```bash
grep -i "error" /var/log/syslog
```

To display matching line numbers:

```bash
grep -in "error" /var/log/syslog
```

For compressed files, many systems provide `zgrep`:

```bash
zgrep "error" system.log.gz
```

## 14. Combining `grep` with Other Commands

One of the most powerful uses of `grep` is combining it with other Linux commands through pipes.

A pipe (`|`) passes the output of one command into another command.

### Example 1: Filter Running Processes

```bash
ps aux | grep "ssh"
```

This displays process-list lines containing `ssh`.

Note that the search may also match the `grep` command itself.

### Example 2: Search Network Information

```bash
ip address | grep "inet"
```

This filters network information to display lines containing `inet`.

### Example 3: Search Command History

```bash
history | grep "sudo"
```

This displays history entries containing `sudo`.

### Example 4: Count Matching Processes

```bash
ps aux | grep -c "ssh"
```

This counts matching lines, which may include the search command itself.

## 15. Important `grep` Options

| Option | Purpose |
|---|---|
| `-i` | Ignore letter case |
| `-n` | Display line numbers |
| `-w` | Match complete words |
| `-v` | Display non-matching lines |
| `-c` | Count matching lines |
| `-o` | Display only matching portions |
| `-r` | Search directories recursively |
| `-l` | Display filenames containing matches |
| `-L` | Display filenames without matches |
| `-E` | Use extended regular expressions |
| `-A N` | Display N lines after a match |
| `-B N` | Display N lines before a match |
| `-C N` | Display N lines before and after a match |

### Examples

Display filenames containing a match:

```bash
grep -rl "Linux" ~/Documents
```

Display three lines after each match:

```bash
grep -A 3 "error" system.log
```

Display two lines before each match:

```bash
grep -B 2 "error" system.log
```

Display two lines before and after each match:

```bash
grep -C 2 "error" system.log
```

## 16. Understanding `grep` Exit Status

The `grep` command returns an exit status that can be used in shell scripts.

| Exit Status | Meaning |
|---|---|
| `0` | At least one match was found |
| `1` | No matches were found |
| `2` | An error occurred |

Check the exit status:

```bash
grep "Linux" notes.txt
echo $?
```

The `$?` variable contains the exit status of the most recently executed command.

This is useful when writing scripts that search for specific information.

## 17. Practical Lab: Create a Search Environment

Create a directory:

```bash
mkdir -p ~/grep-lab
cd ~/grep-lab
```

Create a sample log file:

```bash
cat > system.log <<'EOF'
INFO: System started successfully
WARNING: High memory usage detected
ERROR: Failed login attempt
INFO: Network connection established
ERROR: Service unavailable
WARNING: Disk space is getting low
INFO: User logged out
EOF
```

### Exercise 1: Search for Errors

```bash
grep "ERROR" system.log
```

### Exercise 2: Search for Warnings

```bash
grep "WARNING" system.log
```

### Exercise 3: Search Without Case Sensitivity

```bash
grep -i "error" system.log
```

### Exercise 4: Display Line Numbers

```bash
grep -n "ERROR" system.log
```

### Exercise 5: Count Error Lines

```bash
grep -c "ERROR" system.log
```

### Exercise 6: Display Lines Without Errors

```bash
grep -v "ERROR" system.log
```

### Exercise 7: Search for Multiple Conditions

```bash
grep -E "ERROR|WARNING" system.log
```

### Exercise 8: Display Context Around Errors

```bash
grep -C 1 "ERROR" system.log
```

Observe and explain the output of each command.

## 18. Cybersecurity Applications

The `grep` command is extremely useful during authorised cybersecurity investigations.

### 18.1 Investigating Failed Login Attempts

On systems that record authentication events in `/var/log/auth.log`, you can search for failed login messages:

```bash
grep -i "failed" /var/log/auth.log
```

The exact wording and log location depend on the Linux distribution and logging configuration.

### 18.2 Investigating SSH Activity

```bash
grep -i "ssh" /var/log/auth.log
```

This can help locate SSH-related log entries.

### 18.3 Searching for Errors

```bash
grep -i "error" /var/log/syslog
```

This can help administrators investigate system problems.

### 18.4 Identifying Suspicious Activity

```bash
grep -Ei "failed|denied|unauthorized" /var/log/auth.log
```

This searches for several potentially relevant terms.

These terms are investigation clues, not proof of an attack.

### 18.5 Searching Configuration Files

```bash
grep -rn "PermitRootLogin" /etc/ssh
```

This searches SSH configuration files for the specified setting.

Only inspect systems and files you are authorised to access.

## 19. Common Mistakes

### Mistake 1: Forgetting Case Sensitivity

```bash
grep "linux" notes.txt
```

This does not automatically match `Linux`.

Use:

```bash
grep -i "linux" notes.txt
```

### Mistake 2: Forgetting the Filename

```bash
grep "Linux"
```

Without a filename or piped input, `grep` waits for input from the terminal.

### Mistake 3: Confusing `-c` with Total Occurrences

`grep -c` counts matching lines, not every occurrence of the pattern.

### Mistake 4: Searching the Wrong Location

Always check that the target file exists:

```bash
ls -l notes.txt
```

### Mistake 5: Treating Every Match as a Security Threat

A search result provides information that needs investigation. It does not automatically establish malicious activity.

## 20. Commands Summary

| Command | Purpose |
|---|---|
| `grep "Linux" file.txt` | Search for Linux |
| `grep -i "linux" file.txt` | Ignore case |
| `grep -n "Linux" file.txt` | Show line numbers |
| `grep -w "Linux" file.txt` | Match complete word |
| `grep -c "Linux" file.txt` | Count matching lines |
| `grep -v "Linux" file.txt` | Show non-matching lines |
| `grep -r "Linux" directory` | Search recursively |
| `grep -l "Linux" *.txt` | Show matching filenames |
| `grep -E "error|failed" file.txt` | Search alternative patterns |
| `grep -A 3 "error" file.txt` | Show three lines after a match |
| `grep -B 3 "error" file.txt` | Show three lines before a match |
| `grep -C 3 "error" file.txt` | Show three lines around a match |

## 21. Knowledge Check

Answer the following questions in your notebook.

1. What does `grep` stand for?
2. What is the primary purpose of the `grep` command?
3. What is the basic syntax of `grep`?
4. What is the difference between `-i` and `-n`?
5. What does `-v` do?
6. What is the purpose of `-r`?
7. What does `-c` count?
8. What is the difference between `-name` in `find` and a search pattern in `grep`?
9. What does the pipe symbol (`|`) do?
10. What is the purpose of `grep -E`?
11. What does the `^` symbol represent in a regular expression?
12. What does the `$` symbol represent?
13. What does exit status `1` mean for `grep`?
14. How can `grep` help investigate failed login attempts?
15. Why should you not assume every matching log entry proves an attack?

## 22. Practical Assignment

Complete these tasks in Kali Linux.

1. Create a directory called `grep-practice`.
2. Create a file called `users.txt`.
3. Add at least ten lines containing different usernames and descriptions.
4. Search for a specific username.
5. Search without case sensitivity.
6. Display line numbers for matching lines.
7. Count matching lines.
8. Display lines that do not contain a specified word.
9. Search for two alternative words using `grep -E`.
10. Create a sample log file containing INFO, WARNING, and ERROR entries.
11. Search for all ERROR entries.
12. Display two lines of context around each ERROR entry.
13. Search recursively through a directory containing multiple text files.
14. Explain how `grep` can assist during log analysis.

## 23. Lesson Conclusion

The `grep` command is one of the most important text-searching tools in Linux.

You have learned how to:

- Search for text inside files.
- Perform case-insensitive searches.
- Display line numbers.
- Count matching lines.
- Search multiple files.
- Search directories recursively.
- Use regular expressions.
- Combine `grep` with other commands.
- Analyse system logs.

**Key lesson:** `grep` helps you quickly identify relevant information in large amounts of text, making it an essential tool for Linux administration and cybersecurity investigations.

---

**End of Lesson 12 — The `grep` Command**