# Lesson 13: The `sort`, `uniq`, and `wc` Commands

## 1. Introduction

Linux provides several commands for processing and analysing text.

Three important commands are:

1. `sort` – Arranges lines of text in a particular order.
2. `uniq` – Identifies or removes adjacent duplicate lines.
3. `wc` – Counts lines, words, bytes, and characters.

These commands are especially useful when working with:

- System logs.
- User lists.
- Network information.
- Text files.
- Security reports.
- Large datasets.

They become even more powerful when combined using pipes.

## 2. Understanding the `sort` Command

The `sort` command arranges lines of text into a particular order.

By default, it sorts lines alphabetically according to their character ordering.

### Basic Syntax

```bash
sort [OPTIONS] [FILE]
```

### Example 1: Create a Sample File

```bash
cat > names.txt <<'EOF'
Yusuf
Abbas
Maryam
Zainab
Ibrahim
EOF
```

### Example 2: Sort Alphabetically

```bash
sort names.txt
```

Expected output:

```text
Abbas
Ibrahim
Maryam
Yusuf
Zainab
```

The original file is not modified. The sorted result is displayed in the terminal.

### Example 3: Sort in Reverse Order

Use `-r`:

```bash
sort -r names.txt
```

This displays the sorted lines in reverse order.

### Example 4: Sort and Save the Result

```bash
sort names.txt > sorted-names.txt
```

This saves the sorted output in a new file.

### Example 5: Sort Numbers

Create a file:

```bash
cat > numbers.txt <<'EOF'
50
10
100
2
25
EOF
```

Run:

```bash
sort numbers.txt
```

By default, this sorts lexicographically, meaning it compares text characters rather than treating each line as a number.

Use `-n` for numerical sorting:

```bash
sort -n numbers.txt
```

Expected output:

```text
2
10
25
50
100
```

### Example 6: Reverse Numerical Sorting

```bash
sort -nr numbers.txt
```

This sorts numbers from highest to lowest.

## 3. Important `sort` Options

| Option | Purpose |
|---|---|
| `-r` | Reverse the sorting order |
| `-n` | Sort numerically |
| `-f` | Ignore differences in letter case |
| `-u` | Display unique lines only |
| `-k` | Sort using a specified field |
| `-t` | Specify a field separator |
| `-o` | Write output to a specified file |

### Example: Case-Insensitive Sorting

```bash
sort -f names.txt
```

### Example: Sorting CSV Data

Suppose you have:

```text
Yusuf,80
Abbas,95
Maryam,75
```

Save it as `scores.csv`.

To sort by the second comma-separated field numerically:

```bash
sort -t ',' -k2,2n scores.csv
```

Explanation:

- `-t ','` specifies a comma as the field separator.
- `-k2,2` selects the second field.
- `n` requests numerical sorting.

## 4. Understanding the `uniq` Command

The `uniq` command identifies or removes repeated adjacent lines.

The word *adjacent* is very important.

`uniq` normally checks whether neighbouring lines are identical.

### Basic Syntax

```bash
uniq [OPTIONS] [INPUT] [OUTPUT]
```

### Example 1: Create a Duplicate File

```bash
cat > duplicates.txt <<'EOF'
Linux
Linux
Windows
Windows
Kali
Ubuntu
Ubuntu
EOF
```

### Example 2: Remove Adjacent Duplicates

```bash
uniq duplicates.txt
```

Expected output:

```text
Linux
Windows
Kali
Ubuntu
```

### Important Concept

Consider this file:

```text
Linux
Windows
Linux
Ubuntu
```

Running:

```bash
uniq file.txt
```

will not remove both occurrences of Linux because they are not adjacent.

To remove duplicates regardless of their original positions, sort the file first:

```bash
sort file.txt | uniq
```

This is one of the most common combinations in Linux.

## 5. Important `uniq` Options

| Option | Purpose |
|---|---|
| `-c` | Count occurrences of adjacent lines |
| `-d` | Display only repeated lines |
| `-u` | Display only lines that occur once in adjacent groups |
| `-i` | Ignore differences in letter case |
| `-f N` | Skip the first N fields when comparing |
| `-s N` | Skip the first N characters when comparing |

### Example 1: Count Duplicates

```bash
sort duplicates.txt | uniq -c
```

Example output:

```text
2 Linux
2 Ubuntu
2 Windows
1 Kali
```

The counts indicate how many times each line occurs.

### Example 2: Display Only Duplicates

```bash
sort duplicates.txt | uniq -d
```

This displays entries occurring more than once.

### Example 3: Display Only Unique Entries

```bash
sort duplicates.txt | uniq -u
```

This displays entries occurring exactly once.

### Example 4: Ignore Letter Case

```bash
sort -f duplicates.txt | uniq -i
```

This treats differences in letter case as equivalent during comparison.

## 6. Understanding the `wc` Command

The `wc` command stands for **Word Count**.

It counts information about text input.

It can count:

- Lines.
- Words.
- Bytes.
- Characters.
- Maximum line length.

### Basic Syntax

```bash
wc [OPTIONS] [FILE]
```

### Example 1: Create a Sample File

```bash
cat > sample.txt <<'EOF'
Linux is powerful.
Linux is useful.
Cybersecurity requires practice.
EOF
```

### Example 2: Count Lines, Words, and Bytes

```bash
wc sample.txt
```

Example output:

```text
3  9  75 sample.txt
```

The values represent:

1. Number of lines.
2. Number of words.
3. Number of bytes.

The exact byte count depends on the file's contents and line endings.

### Example 3: Count Only Lines

Use `-l`:

```bash
wc -l sample.txt
```

### Example 4: Count Only Words

Use `-w`:

```bash
wc -w sample.txt
```

### Example 5: Count Only Bytes

Use `-c`:

```bash
wc -c sample.txt
```

### Example 6: Count Characters

Use `-m`:

```bash
wc -m sample.txt
```

Characters and bytes are not always the same. For example, many non-ASCII characters require multiple bytes in UTF-8.

### Example 7: Find the Longest Line Length

Use `-L`:

```bash
wc -L sample.txt
```

This reports the length of the longest line.

## 7. Important `wc` Options

| Option | Purpose |
|---|---|
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |
| `-L` | Display maximum line length |

## 8. Combining `sort`, `uniq`, and `wc`

The real power of these commands becomes clear when they are combined.

A pipe (`|`) sends the output of one command to the next.

### Example 1: Sort and Remove Duplicates

```bash
sort names.txt | uniq
```

### Example 2: Count Duplicate Entries

```bash
sort names.txt | uniq -c
```

### Example 3: Count Unique Entries

```bash
sort names.txt | uniq | wc -l
```

This counts the number of distinct lines.

### Example 4: Count All Lines

```bash
wc -l names.txt
```

### Example 5: Find the Most Frequently Repeated Entries

```bash
sort names.txt | uniq -c | sort -nr
```

Explanation:

1. `sort` groups identical lines together.
2. `uniq -c` counts each group.
3. `sort -nr` sorts the counts numerically in descending order.

This is a useful pattern for analysing repeated events.

## 9. Real-World Linux Examples

### 9.1 Count System Log Lines

```bash
wc -l /var/log/syslog
```

This counts the lines in the system log, if that file exists.

### 9.2 Count Unique Usernames in a Text File

```bash
sort users.txt | uniq | wc -l
```

This counts distinct lines in the file.

### 9.3 Identify Repeated IP Addresses

Suppose `ips.txt` contains:

```text
192.168.1.10
192.168.1.20
192.168.1.10
192.168.1.30
192.168.1.20
192.168.1.10
```

Run:

```bash
sort ips.txt | uniq -c | sort -nr
```

Example output:

```text
3 192.168.1.10
2 192.168.1.20
1 192.168.1.30
```

This helps identify frequently occurring addresses in a dataset.

It does not establish whether an address is malicious.

### 9.4 Count Error Messages

```bash
grep -i "error" system.log | wc -l
```

This counts lines containing the word `error`.

### 9.5 Find the Most Common Log Messages

```bash
sort system.log | uniq -c | sort -nr
```

This groups identical log lines and sorts them by frequency.

## 10. Cybersecurity Applications

These commands are useful during authorised security assessments and investigations.

### 10.1 Analyse Repeated Login Attempts

If you have an authorised log file containing authentication events:

```bash
grep -i "failed" auth.log | wc -l
```

This counts lines containing `failed`.

### 10.2 Identify Frequently Appearing IP Addresses

If `connections.txt` contains one IP address per line:

```bash
sort connections.txt | uniq -c | sort -nr
```

This ranks IP addresses by frequency.

### 10.3 Find Repeated Error Messages

```bash
sort errors.txt | uniq -c | sort -nr
```

This helps identify recurring error messages.

### 10.4 Count Entries in a Security Report

```bash
wc -l security-report.txt
```

This provides a quick line count.

### 10.5 Analyse a Filtered Log Dataset

```bash
grep -i "failed" auth.log | sort | uniq -c | sort -nr
```

This filters, groups, counts, and ranks matching log lines.

**Important:** Frequency is an investigation clue, not proof of an attack. Always examine timestamps, context, and other evidence.

## 11. Practical Lab: Create a Working Environment

Create a directory:

```bash
mkdir -p ~/text-analysis-lab
cd ~/text-analysis-lab
```

Create a names file:

```bash
cat > names.txt <<'EOF'
Yusuf
Abbas
Maryam
Yusuf
Ibrahim
Abbas
Zainab
Maryam
Yusuf
EOF
```

### Exercise 1: Sort the Names

```bash
sort names.txt
```

### Exercise 2: Sort in Reverse

```bash
sort -r names.txt
```

### Exercise 3: Count Repeated Names

```bash
sort names.txt | uniq -c
```

### Exercise 4: Display Only Repeated Names

```bash
sort names.txt | uniq -d
```

### Exercise 5: Display Names That Occur Once

```bash
sort names.txt | uniq -u
```

### Exercise 6: Count All Lines

```bash
wc -l names.txt
```

### Exercise 7: Count Unique Names

```bash
sort names.txt | uniq | wc -l
```

### Exercise 8: Rank Names by Frequency

```bash
sort names.txt | uniq -c | sort -nr
```

Observe the output and explain what each command does.

## 12. Practical Lab: Analyse IP Addresses

Create a file:

```bash
cat > ips.txt <<'EOF'
192.168.1.10
192.168.1.20
192.168.1.10
10.0.0.5
192.168.1.20
192.168.1.10
10.0.0.5
10.0.0.5
EOF
```

### Exercise 1: Sort the Addresses

```bash
sort ips.txt
```

### Exercise 2: Count Each Address

```bash
sort ips.txt | uniq -c
```

### Exercise 3: Rank Addresses by Frequency

```bash
sort ips.txt | uniq -c | sort -nr
```

### Exercise 4: Count Unique Addresses

```bash
sort ips.txt | uniq | wc -l
```

### Exercise 5: Count Total Entries

```bash
wc -l ips.txt
```

Write down the difference between total entries and unique addresses.

## 13. Common Mistakes

### Mistake 1: Forgetting That `uniq` Requires Adjacent Duplicates

This may not remove all duplicates:

```bash
uniq names.txt
```

A safer approach for all duplicate lines is:

```bash
sort names.txt | uniq
```

### Mistake 2: Sorting Numbers Without `-n`

Default sorting is text-based.

Use:

```bash
sort -n numbers.txt
```

for numerical ordering.

### Mistake 3: Confusing Bytes and Characters

`wc -c` counts bytes, while `wc -m` counts characters.

They can produce different results.

### Mistake 4: Confusing Total Lines with Unique Lines

```bash
wc -l names.txt
```

counts all lines.

```bash
sort names.txt | uniq | wc -l
```

counts distinct lines.

## 14. Commands Summary

| Command | Purpose |
|---|---|
| `sort file.txt` | Sort lines |
| `sort -r file.txt` | Reverse sorting |
| `sort -n file.txt` | Numerical sorting |
| `sort -u file.txt` | Sort and output unique lines |
| `sort -t ',' -k2,2n file.csv` | Sort by second CSV field numerically |
| `uniq file.txt` | Remove adjacent duplicates |
| `uniq -c file.txt` | Count adjacent duplicate groups |
| `uniq -d file.txt` | Display repeated adjacent lines |
| `uniq -u file.txt` | Display lines occurring once in adjacent groups |
| `wc file.txt` | Count lines, words, and bytes |
| `wc -l file.txt` | Count lines |
| `wc -w file.txt` | Count words |
| `wc -c file.txt` | Count bytes |
| `wc -m file.txt` | Count characters |
| `wc -L file.txt` | Find maximum line length |

## 15. Knowledge Check

Answer these questions in your notebook.

1. What is the purpose of the `sort` command?
2. What is the difference between `sort` and `sort -n`?
3. What does the `-r` option do?
4. What is the purpose of the `uniq` command?
5. Why should you usually combine `sort` with `uniq`?
6. What does `uniq -c` do?
7. What is the difference between `uniq -d` and `uniq -u`?
8. What does `wc` stand for?
9. What does `wc -l` count?
10. What is the difference between bytes and characters?
11. What does `sort file.txt | uniq | wc -l` calculate?
12. How can these commands help analyse repeated IP addresses?
13. Why does a frequently occurring IP address not automatically indicate an attack?
14. Explain how a pipe connects Linux commands.

## 16. Practical Assignment

Complete these tasks in Kali Linux.

1. Create a directory named `text-analysis-practice`.
2. Create a file containing at least 15 names with repeated entries.
3. Sort the names alphabetically.
4. Sort the names in reverse order.
5. Count how many times each name occurs.
6. Display only repeated names.
7. Display names occurring once.
8. Count the total number of lines.
9. Count the number of distinct names.
10. Create a file containing at least 10 IP addresses with repetitions.
11. Count each IP address.
12. Rank the IP addresses by frequency.
13. Create a sample log file containing repeated ERROR and WARNING entries.
14. Use `grep`, `sort`, `uniq`, and `wc` together to analyse the log.
15. Explain how these commands can assist a cybersecurity analyst.

## 17. Lesson Conclusion

The `sort`, `uniq`, and `wc` commands are essential tools for text processing and data analysis.

You have learned how to:

- Sort text alphabetically and numerically.
- Reverse sorting order.
- Identify repeated lines.
- Count duplicate entries.
- Count lines, words, bytes, and characters.
- Combine commands using pipes.
- Analyse repeated entries in logs and IP address datasets.

**Key lesson:** These commands transform raw text into organised and measurable information, making them valuable for Linux administration and cybersecurity investigations.

---

**End of Lesson 13 — The `sort`, `uniq`, and `wc` Commands**