
# Module 11 – Lesson 13: Text Processing and Pipelines

## 1. Learning Objectives

By the end of this lesson, you should understand how to:

1. Process text from the Linux terminal.
2. Search for specific words and patterns using `grep`.
3. Extract and manipulate text using `cut`.
4. Transform text using `tr`.
5. Sort text using `sort`.
6. Remove duplicate lines using `uniq`.
7. Count lines, words, and characters using `wc`.
8. Process text using `sed` and `awk`.
9. Connect commands using pipelines.
10. Build a basic log-analysis tool for defensive cybersecurity.

---

## 2. What Is Text Processing?

Text processing means examining, filtering, searching, extracting, and modifying textual information.

Linux provides powerful command-line utilities for this purpose.

Examples of text files include:

- System logs.
- Application logs.
- Configuration files.
- CSV files.
- Network reports.
- Security event records.
- Script output.

### Why Is Text Processing Important?

In cybersecurity, analysts frequently examine large amounts of text.

For example, imagine a log file containing 5,000 lines.

Instead of reading every line manually, you can use Linux commands to:

- Find failed login attempts.
- Search for suspicious keywords.
- Count error messages.
- Extract IP addresses.
- Identify repeated events.
- Generate reports.

---

## 3. Understanding grep

`grep` searches for text patterns inside files or command output.

### Basic Syntax

```bash
grep "pattern" filename
```

### Example 1: Search for a Word

Create a sample file:

```bash
echo -e "Linux is powerful\nWindows is popular\nLinux supports automation" > notes.txt
```

Search for Linux:

```bash
grep "Linux" notes.txt
```

Expected output:

```text
Linux is powerful
Linux supports automation
```

### Example 2: Case-Insensitive Search

```bash
grep -i "linux" notes.txt
```

The `-i` option ignores uppercase and lowercase differences.

### Example 3: Display Line Numbers

```bash
grep -n "Linux" notes.txt
```

The `-n` option displays matching line numbers.

### Example 4: Count Matches

```bash
grep -c "Linux" notes.txt
```

This counts the number of matching lines.

### Example 5: Search Recursively

```bash
grep -r "password" ~/module11-lab/lesson13/
```

This searches files in the specified directory and its subdirectories.

Only use this on files you are authorised to inspect.

### Useful grep Options

| Option | Purpose |
|---|---|
| `-i` | Ignore case |
| `-n` | Show line numbers |
| `-c` | Count matching lines |
| `-v` | Show non-matching lines |
| `-r` | Search recursively |
| `-w` | Match whole words |
| `-l` | Display filenames containing matches |
| `-E` | Use extended regular expressions |
| `-q` | Quiet mode; suppress normal output |

---

## 4. Understanding cut

`cut` extracts specific sections of text.

It is especially useful for structured text files.

### Example 1: Extract by Character Position

Create a file:

```bash
echo "CYBERSECURITY" > words.txt
```

Extract the first three characters:

```bash
cut -c 1-3 words.txt
```

Output:

```text
C Y B
```

The actual output has no spaces between the characters.

### Example 2: Extract Fields

Create a CSV-style file:

```bash
echo "Abbas,Computer Science,Level 3" > students.csv
```

Extract the first field:

```bash
cut -d ',' -f 1 students.csv
```

Output:

```text
Abbas
```

Extract the second field:

```bash
cut -d ',' -f 2 students.csv
```

Output:

```text
Computer Science
```

### Understanding the Options

- `-d` specifies the delimiter.
- `-f` specifies which field to extract.
- `-c` selects character positions.

---

## 5. Understanding tr

`tr` translates or deletes characters.

### Example 1: Convert Lowercase to Uppercase

```bash
echo "linux administration" | tr 'a-z' 'A-Z'
```

Output:

```text
LINUX ADMINISTRATION
```

### Example 2: Convert Uppercase to Lowercase

```bash
echo "CYBERSECURITY" | tr 'A-Z' 'a-z'
```

Output:

```text
cybersecurity
```

### Example 3: Replace Spaces with Underscores

```bash
echo "Linux Security Course" | tr ' ' '_'
```

Output:

```text
Linux_Security_Course
```

### Example 4: Delete Numbers

```bash
echo "Linux123Security456" | tr -d '0-9'
```

Output:

```text
LinuxSecurity
```

### Useful Options

| Option | Purpose |
|---|---|
| `-d` | Delete selected characters |
| `-s` | Squeeze repeated characters |
| `-c` | Use the complement of a character set |

---

## 6. Understanding sort

`sort` arranges lines in a particular order.

Create a file:

```bash
printf "Zebra\nApple\nMango\nBanana\n" > fruits.txt
```

Sort alphabetically:

```bash
sort fruits.txt
```

Output:

```text
Apple
Banana
Mango
Zebra
```

### Reverse Sorting

```bash
sort -r fruits.txt
```

### Numeric Sorting

Create a numbers file:

```bash
printf "100\n5\n25\n3\n" > numbers.txt
```

Sort numerically:

```bash
sort -n numbers.txt
```

Output:

```text
3
5
25
100
```

### Useful Options

| Option | Purpose |
|---|---|
| `-r` | Reverse order |
| `-n` | Numeric sorting |
| `-u` | Sort and remove duplicate lines |
| `-k` | Sort using a selected field |
| `-f` | Ignore case differences |

---

## 7. Understanding uniq

`uniq` identifies or removes adjacent repeated lines.

Create a file:

```bash
printf "Linux\nWindows\nLinux\nLinux\nUbuntu\n" > operating-systems.txt
```

Run:

```bash
uniq operating-systems.txt
```

Notice that `uniq` only removes repeated adjacent lines.

To remove duplicates throughout the file, sort first:

```bash
sort operating-systems.txt | uniq
```

### Count Repeated Lines

```bash
sort operating-systems.txt | uniq -c
```

Example output:

```text
2 Linux
1 Ubuntu
1 Windows
```

### Display Only Repeated Lines

```bash
sort operating-systems.txt | uniq -d
```

---

## 8. Understanding wc

`wc` counts information in text files.

### Count Lines

```bash
wc -l notes.txt
```

### Count Words

```bash
wc -w notes.txt
```

### Count Bytes

```bash
wc -c notes.txt
```

### Count Characters

```bash
wc -m notes.txt
```

### Count Everything

```bash
wc notes.txt
```

Typical output includes:

- Number of lines.
- Number of words.
- Number of bytes.
- Filename.

### Practical Example

Count lines in a log:

```bash
wc -l system.log
```

This is useful when examining large logs.

---

## 9. Understanding sed

`sed` is a stream editor used to transform text.

It can search, replace, delete, and print selected lines.

### Example 1: Replace Text

Create a file:

```bash
echo "Linux is difficult" > sentence.txt
```

Replace `difficult` with `powerful`:

```bash
sed 's/difficult/powerful/' sentence.txt
```

Output:

```text
Linux is powerful
```

This displays the modified output without changing the original file.

### Example 2: Replace All Occurrences on Each Line

```bash
echo "Linux Linux Linux" | sed 's/Linux/Unix/g'
```

Output:

```text
Unix Unix Unix
```

The `g` flag replaces all occurrences on each line.

### Example 3: Display a Specific Line

```bash
sed -n '2p' notes.txt
```

Displays line 2.

### Example 4: Delete a Specific Line from Output

```bash
sed '2d' notes.txt
```

Displays the file without line 2.

### Important Safety Note

Avoid using `sed -i` on important configuration files while learning. It modifies files directly. Practise with copies inside your lesson directory.

---

## 10. Understanding awk

`awk` is a powerful text-processing language.

It can extract fields, perform calculations, and generate reports.

### Example 1: Print the First Field

Create a file:

```bash
echo "Abbas ComputerScience Level3" > student.txt
```

Run:

```bash
awk '{print $1}' student.txt
```

Output:

```text
Abbas
```

### Example 2: Print Multiple Fields

```bash
awk '{print $1, $3}' student.txt
```

Output:

```text
Abbas Level3
```

### Example 3: Use a Delimiter

Create a CSV file:

```bash
echo "Abbas,ComputerScience,Level3" > student.csv
```

Run:

```bash
awk -F ',' '{print $1}' student.csv
```

Output:

```text
Abbas
```

### Example 4: Display Line Numbers

```bash
awk '{print NR, $0}' notes.txt
```

- `NR` represents the current record number.
- `$0` represents the entire current line.
- `$1` represents the first field.
- `$2` represents the second field.

---

## 11. Understanding Pipelines

A pipeline connects two or more commands.

The pipe symbol is:

```bash
|
```

It sends the standard output of one command into the standard input of another.

### Example 1: Search Within Command Output

```bash
ls -l | grep ".txt"
```

This lists files and filters the output for lines containing `.txt`.

### Example 2: Count Files

```bash
ls | wc -l
```

This counts the lines produced by `ls`.

For a more reliable count of regular files in the current directory, use:

```bash
find . -maxdepth 1 -type f | wc -l
```

### Example 3: Sort and Remove Duplicates

```bash
cat operating-systems.txt | sort | uniq
```

A shorter and more efficient version is:

```bash
sort -u operating-systems.txt
```

### Example 4: Search and Count Errors

```bash
grep -i "error" system.log | wc -l
```

This searches for lines containing `error` and counts them.

### Understanding the Pipeline

```text
system.log
    |
    v
grep -i "error"
    |
    v
wc -l
    |
    v
Number of matching lines
```

Pipelines allow you to build powerful operations by combining simple commands.

---

## 12. Redirection and Pipelines

Linux uses several redirection operators.

| Operator | Purpose |
|---|---|
| `>` | Write output, replacing existing content |
| `>>` | Append output |
| `<` | Read input from a file |
| `2>` | Redirect standard error |
| `2>>` | Append standard error |
| `2>&1` | Redirect standard error to standard output |
| `|` | Connect commands |

### Example 1: Save Output

```bash
ls -l > directory-report.txt
```

### Example 2: Append Output

```bash
date >> directory-report.txt
```

### Example 3: Save Errors Separately

```bash
ls /nonexistent 2> errors.txt
```

### Example 4: Combine Output and Errors

```bash
ls /home /nonexistent > combined-report.txt 2>&1
```

The command's standard output and standard error are both directed into the report file.

---

## 13. Regular Expressions with grep

Regular expressions allow you to search for patterns instead of exact words.

### Example 1: Find Lines Starting with a Word

```bash
grep '^Linux' notes.txt
```

`^` means the beginning of a line.

### Example 2: Find Lines Ending with a Word

```bash
grep 'security$' notes.txt
```

`$` means the end of a line.

### Example 3: Search for Numbers

```bash
grep -E '[0-9]+' numbers.txt
```

### Example 4: Search for an IP-Like Pattern

```bash
grep -E '[0-9]{1,3}(\.[0-9]{1,3}){3}' network.txt
```

This finds text resembling IPv4 addresses, but it does not validate that each address is a valid IPv4 address.

For example, it may also match `999.999.999.999`.

---

## 14. Practical Laboratory

### Step 1: Create Your Lab Directory

```bash
mkdir -p ~/module11-lab/lesson13
cd ~/module11-lab/lesson13
```

### Step 2: Create a Sample Security Log

```bash
cat > security.log <<'EOF'
2026-11-15 INFO User Abbas logged in
2026-11-15 ERROR Failed login from 192.168.1.10
2026-11-15 INFO System started
2026-11-15 WARNING Multiple login attempts detected
2026-11-15 ERROR Failed login from 192.168.1.20
2026-11-15 INFO User admin logged out
2026-11-15 ERROR Failed login from 192.168.1.10
EOF
```

Display the file:

```bash
cat security.log
```

### Step 3: Find All Errors

```bash
grep "ERROR" security.log
```

### Step 4: Count Errors

```bash
grep -c "ERROR" security.log
```

Expected result:

```text
3
```

### Step 5: Find Warnings

```bash
grep "WARNING" security.log
```

### Step 6: Count All Log Entries

```bash
wc -l security.log
```

Expected result:

```text
7
```

### Step 7: Extract IP Addresses

```bash
grep -E -o '[0-9]{1,3}(\.[0-9]{1,3}){3}' security.log
```

The `-o` option prints only the matching text.

### Step 8: Count Repeated IP Addresses

```bash
grep -E -o '[0-9]{1,3}(\.[0-9]{1,3}){3}' security.log | sort | uniq -c
```

Expected result:

```text
2 192.168.1.10
1 192.168.1.20
```

### Step 9: Extract Log Categories

```bash
awk '{print $2}' security.log
```

This extracts the second field, which contains the log category.

### Step 10: Generate a Report

```bash
{
    echo "========== SECURITY LOG REPORT =========="
    echo "Total log entries:"
    wc -l < security.log

    echo
    echo "Error count:"
    grep -c "ERROR" security.log

    echo
    echo "Warning count:"
    grep -c "WARNING" security.log

    echo
    echo "Unique IP addresses:"
    grep -E -o '[0-9]{1,3}(\.[0-9]{1,3}){3}' security.log | sort -u

    echo "=========================================="
} > security-report.txt
```

Display the report:

```bash
cat security-report.txt
```

---

## 15. Practical Cybersecurity Project: Basic Log Analyzer

Now create a reusable Bash script that analyses a log file.

Create:

```bash
nano log-analyzer.sh
```

Enter:

```bash
#!/bin/bash

set -u

if [[ $# -ne 1 ]]; then
    echo "Usage: $0 logfile" >&2
    exit 1
fi

logfile="$1"

if [[ ! -f "$logfile" || ! -r "$logfile" ]]; then
    echo "ERROR: File does not exist or cannot be read." >&2
    exit 1
fi

total_lines=$(wc -l < "$logfile")
error_count=$(grep -ic "ERROR" "$logfile" || true)
warning_count=$(grep -ic "WARNING" "$logfile" || true)

echo "========== LOG ANALYZER =========="

echo "File: $logfile"
echo "Total lines: $total_lines"
echo "Errors: $error_count"
echo "Warnings: $warning_count"

echo
echo "Recent error entries:"

grep -in "ERROR" "$logfile" | tail -n 5 || true

echo
echo "IP addresses found:"

grep -E -o '[0-9]{1,3}(\.[0-9]{1,3}){3}' "$logfile" | sort -u || true

echo "=================================="
```

### Make It Executable

```bash
chmod +x log-analyzer.sh
```

### Run It

```bash
./log-analyzer.sh security.log
```

### Test Invalid Usage

```bash
./log-analyzer.sh
```

### Test a Missing File

```bash
./log-analyzer.sh missing.log
```

### What Have You Built?

A simple defensive log-analysis utility that can:

- Count log entries.
- Count error messages.
- Count warnings.
- Display recent errors.
- Extract unique IP-like strings.
- Handle missing input files.

This is an introductory tool, not a replacement for a SIEM platform or a full log-analysis system.

---

## 16. Common Text-Processing Mistakes

| Mistake | Explanation | Solution |
|---|---|---|
| Using `uniq` without sorting | Non-adjacent duplicates remain | Use `sort file \| uniq` |
| Using the wrong delimiter | Fields are extracted incorrectly | Check the file format |
| Forgetting quotes | Special characters may be interpreted by Bash | Quote variables appropriately |
| Using `>` instead of `>>` | Existing content is overwritten | Choose the correct redirection |
| Assuming an IP regex validates an IP | Pattern matching is not full validation | Use a proper IP validation method |
| Modifying original files carelessly | Important data may be lost | Practise on copies |

---

## 17. Lesson Summary

You have learned:

- `grep` searches for text.
- `cut` extracts fields and characters.
- `tr` transforms characters.
- `sort` orders lines.
- `uniq` handles adjacent duplicates.
- `wc` counts lines, words, and characters.
- `sed` edits text streams.
- `awk` processes structured text.
- `|` connects commands into pipelines.
- Redirection saves output and errors.
- Regular expressions identify text patterns.

### Important Commands

| Command | Main Purpose |
|---|---|
| `grep` | Search |
| `cut` | Extract |
| `tr` | Translate or delete characters |
| `sort` | Sort |
| `uniq` | Handle duplicates |
| `wc` | Count |
| `sed` | Edit text streams |
| `awk` | Process structured text |
| `head` | Display first lines |
| `tail` | Display last lines |
| `tee` | Display and save output |
| `find` | Locate files and directories |

---

## 18. Knowledge Check

Answer these questions in your notebook:

1. What is text processing?
2. What is the purpose of `grep`?
3. What is the difference between `grep -i` and `grep -v`?
4. What does `cut -d ',' -f 2` do?
5. What is the purpose of `tr`?
6. Why do we use `sort` before `uniq`?
7. What does `wc -l` count?
8. Explain the difference between `sed` and `awk`.
9. What is a Linux pipeline?
10. How can text-processing commands help a cybersecurity analyst investigate suspicious login activity?

### Practical Assessment

Complete these tasks:

1. Create the sample security log.
2. Find all error entries.
3. Count warnings.
4. Extract IP addresses.
5. Count repeated IP addresses.
6. Extract log categories using `awk`.
7. Generate the security report.
8. Build and test `log-analyzer.sh`.

**Lesson 13 is complete when your log analyzer runs successfully and produces the expected results.**
