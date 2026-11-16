
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 03: TEXT PROCESSING AND SEARCHING

---

## 1. Lab Objectives

In this lab, you will learn how to process, filter, search, and analyze text from the Linux command line.

By the end, you should be able to:

1. Display file contents.
2. Read large files efficiently.
3. Search for specific words and patterns.
4. Count lines, words, and characters.
5. Extract specific columns and fields.
6. Sort and remove duplicate lines.
7. Compare files.
8. Use regular expressions.
9. Perform basic log and text analysis.

## 2. Prepare Your Lab Environment

Create the working directory:

```bash
mkdir -p ~/practical-labs/module01/lab03
cd ~/practical-labs/module01/lab03
```

Verify:

```bash
pwd
```

All exercises will use files created inside this directory.

---

## 3. Exercise 1: Create Sample Data

Create a file named `students.txt`:

```bash
cat > students.txt <<'EOF'
Abbas,Computer Science,85
Musa,Information Technology,72
Amina,Computer Science,91
Fatima,Cybersecurity,88
Ibrahim,Software Engineering,76
Zainab,Cybersecurity,95
Hassan,Computer Science,67
Maryam,Information Technology,83
EOF
```

Display the file:

```bash
cat students.txt
```

Create another file named `system-log.txt`:

```bash
cat > system-log.txt <<'EOF'
INFO: System started successfully
WARNING: Disk usage is above 75 percent
ERROR: Authentication service failed
INFO: Network connection established
WARNING: Memory usage is high
ERROR: Failed login attempt detected
INFO: Backup completed successfully
ERROR: Database connection failed
INFO: User session started
WARNING: Unusual service activity detected
EOF
```

Inspect:

```bash
cat system-log.txt
```

---

## 4. Exercise 2: Display File Contents

Linux provides several commands for reading text files.

### cat

Display the entire file:

```bash
cat students.txt
```

Display line numbers:

```bash
cat -n students.txt
```

### head

Display the first five lines:

```bash
head -n 5 students.txt
```

### tail

Display the last five lines:

```bash
tail -n 5 students.txt
```

### less

Read a file page by page:

```bash
less system-log.txt
```

Press `q` to exit.

### Practical task

Display the first three lines of `system-log.txt`, followed by its last three lines.

---

## 5. Exercise 3: Search with grep

The `grep` command searches text for matching patterns.

Find all Computer Science students:

```bash
grep "Computer Science" students.txt
```

Find all cybersecurity records:

```bash
grep "Cybersecurity" students.txt
```

Search without case sensitivity:

```bash
grep -i "error" system-log.txt
```

Display matching line numbers:

```bash
grep -n "WARNING" system-log.txt
```

Count matching lines:

```bash
grep -c "INFO" system-log.txt
```

Display lines that do not contain a pattern:

```bash
grep -v "INFO" system-log.txt
```

Search recursively in the current directory:

```bash
grep -R "Computer Science" .
```

### Practical task

Find:

1. All ERROR messages.
2. All WARNING messages.
3. All INFO messages.
4. The number of ERROR messages.
5. All lines that do not contain the word INFO.

---

## 6. Exercise 4: Count Text with wc

The `wc` command counts text.

Count lines:

```bash
wc -l students.txt
```

Count words:

```bash
wc -w students.txt
```

Count bytes:

```bash
wc -c students.txt
```

Count characters:

```bash
wc -m students.txt
```

Display all counts:

```bash
wc students.txt
```

### Practical task

Count the number of lines in `system-log.txt`.

How many records are present?

---

## 7. Exercise 5: Extract Columns with cut

The `cut` command extracts fields or character positions from text.

Our student records are comma-separated.

Display student names:

```bash
cut -d',' -f1 students.txt
```

Display departments:

```bash
cut -d',' -f2 students.txt
```

Display scores:

```bash
cut -d',' -f3 students.txt
```

Display names and scores:

```bash
cut -d',' -f1,3 students.txt
```

Explanation:

- `-d','` identifies the comma delimiter.
- `-f1` selects the first field.
- `-f2` selects the second field.
- `-f3` selects the third field.

### Practical task

Extract all department names from `students.txt`.

---

## 8. Exercise 6: Sort and Remove Duplicates

Create a file named `departments.txt`:

```bash
cat > departments.txt <<'EOF'
Computer Science
Cybersecurity
Information Technology
Computer Science
Software Engineering
Cybersecurity
Computer Science
EOF
```

Sort alphabetically:

```bash
sort departments.txt
```

Sort in reverse order:

```bash
sort -r departments.txt
```

Remove adjacent duplicate lines:

```bash
sort departments.txt | uniq
```

Count occurrences:

```bash
sort departments.txt | uniq -c
```

Sort by occurrence count:

```bash
sort departments.txt | uniq -c | sort -nr
```

Important:

`uniq` removes only adjacent duplicate lines. Sorting first groups identical lines together.

### Practical task

Identify the department that appears most frequently.

---

## 9. Exercise 7: Use sed to Modify Text Output

The `sed` command is a stream editor.

It can search, replace, and transform text.

Display a modified version of the student file:

```bash
sed 's/Computer Science/CS/' students.txt
```

Replace all occurrences of a word:

```bash
sed 's/INFO/INFORMATION/g' system-log.txt
```

Display a particular line:

```bash
sed -n '3p' students.txt
```

Display lines 2 through 5:

```bash
sed -n '2,5p' students.txt
```

Important:

The commands above display transformed output without changing the original file.

### Practical task

Display a modified copy of `students.txt` where `Cybersecurity` is replaced with `Cyber Security`.

Do not overwrite your original file.

---

## 10. Exercise 8: Use awk for Structured Data

`awk` is useful for processing structured text.

Display student names:

```bash
awk -F',' '{print $1}' students.txt
```

Display departments:

```bash
awk -F',' '{print $2}' students.txt
```

Display scores:

```bash
awk -F',' '{print $3}' students.txt
```

Display names with scores:

```bash
awk -F',' '{print $1, $3}' students.txt
```

Find students with scores above 80:

```bash
awk -F',' '$3 > 80 {print $1, $3}' students.txt
```

Count records:

```bash
awk 'END {print NR}' students.txt
```

Calculate the average score:

```bash
awk -F',' '{sum += $3; count++} END {if (count > 0) print sum/count}' students.txt
```

### Practical task

Use `awk` to display students who scored below 80.

---

## 11. Exercise 9: Compare Files

Create two files:

```bash
cat > version1.txt <<'EOF'
Linux is an operating system.
Linux supports multiple users.
Linux has a powerful shell.
EOF
```

```bash
cat > version2.txt <<'EOF'
Linux is an operating system.
Linux supports multiple users.
Linux is widely used in servers.
EOF
```

Compare them:

```bash
diff version1.txt version2.txt
```

Compare side by side:

```bash
diff -y version1.txt version2.txt
```

Check whether files are identical:

```bash
cmp version1.txt version2.txt
```

If two files are identical, `cmp` normally produces no output and returns a success exit status.

### Practical task

Identify the lines that differ between the two files.

---

## 12. Exercise 10: Introduction to Regular Expressions

Regular expressions allow more advanced text matching.

Find lines beginning with ERROR:

```bash
grep '^ERROR' system-log.txt
```

Find lines ending with `successfully`:

```bash
grep 'successfully$' system-log.txt
```

Find lines containing either ERROR or WARNING:

```bash
grep -E 'ERROR|WARNING' system-log.txt
```

Find lines beginning with INFO or ERROR:

```bash
grep -E '^(INFO|ERROR)' system-log.txt
```

Explanation:

| Symbol | Meaning |
|---|---|
| `^` | Beginning of a line |
| `$` | End of a line |
| `.` | Any single character |
| `*` | Zero or more repetitions |
| `[]` | Character set |
| `|` | Alternative pattern with extended regex |

### Practical task

Find all lines in `system-log.txt` that begin with WARNING or ERROR.

---

## 13. Exercise 11: Mini Log Investigation

You are given a simulated system log.

Your task is to investigate it using text-processing commands.

First, display all error messages:

```bash
grep -n "ERROR" system-log.txt
```

Count the errors:

```bash
grep -c "ERROR" system-log.txt
```

Display warnings:

```bash
grep "WARNING" system-log.txt
```

Count all records:

```bash
wc -l system-log.txt
```

Extract the message types:

```bash
cut -d':' -f1 system-log.txt
```

Count the occurrences of each message type:

```bash
cut -d':' -f1 system-log.txt | sort | uniq -c
```

### Investigation questions

1. How many errors are present?
2. How many warnings are present?
3. How many informational messages are present?
4. What is the most common message type?
5. Which messages might require further investigation?

Remember: this is simulated data, not evidence of a real security incident.

---

## 14. Exercise 12: Build a Text Analysis Report

Create a file called `text-analysis.sh`:

```bash
nano text-analysis.sh
```

Enter:

```bash
#!/bin/bash

LOG="system-log.txt"
REPORT="analysis-report.txt"

if [ ! -f "$LOG" ]; then
    echo "Log file not found: $LOG"
    exit 1
fi

{
    echo "================================"
    echo "       TEXT ANALYSIS REPORT"
    echo "================================"

    echo
    echo "TOTAL RECORDS:"
    wc -l < "$LOG"

    echo
    echo "ERROR COUNT:"
    grep -c "ERROR" "$LOG"

    echo
    echo "WARNING COUNT:"
    grep -c "WARNING" "$LOG"

    echo
    echo "INFO COUNT:"
    grep -c "INFO" "$LOG"

    echo
    echo "ERROR MESSAGES:"
    grep "ERROR" "$LOG"

    echo
    echo "WARNING MESSAGES:"
    grep "WARNING" "$LOG"

    echo
    echo "MESSAGE TYPE SUMMARY:"
    cut -d':' -f1 "$LOG" | sort | uniq -c

    echo
    echo "ANALYSIS COMPLETED"

} > "$REPORT"

echo "Report saved to $REPORT"
```

Save the file.

Make it executable:

```bash
chmod +x text-analysis.sh
```

Run it:

```bash
./text-analysis.sh
```

Read the report:

```bash
cat analysis-report.txt
```

---

## 15. Practical Challenge

Without copying the full commands from the earlier sections, complete these tasks:

1. Create a file containing at least ten student records.
2. Display the first five records.
3. Extract all student names.
4. Extract all scores.
5. Display students who scored at least 80.
6. Calculate the average score.
7. Sort the records by name.
8. Count the number of records.
9. Create a second version of the file with at least two changes.
10. Use `diff` to identify the changes.

---

## 16. Knowledge Check

Answer in your notebook:

1. What is the purpose of `grep`?
2. Explain the difference between `head` and `tail`.
3. What does `wc -l` count?
4. What is the purpose of `cut`?
5. Why is `sort` often used before `uniq`?
6. What does `sed` do?
7. What makes `awk` useful for structured data?
8. What is the purpose of `diff`?
9. What does the `^` symbol mean in a regular expression?
10. How can text-processing commands support cybersecurity investigations?

---

## 17. Lab Completion Checklist

- [ ] Created sample text files.
- [ ] Used `cat`, `head`, `tail`, and `less`.
- [ ] Searched text with `grep`.
- [ ] Counted lines, words, and characters.
- [ ] Extracted fields using `cut`.
- [ ] Sorted data and removed duplicates.
- [ ] Used `sed` for text transformations.
- [ ] Used `awk` for structured data.
- [ ] Compared files with `diff`.
- [ ] Practiced regular expressions.
- [ ] Completed the log investigation.
- [ ] Built the text analysis script.
- [ ] Completed the practical challenge.

**END OF LAB 03**
