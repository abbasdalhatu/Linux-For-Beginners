# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 06: FILE PROCESSING AND TEXT MANIPULATION

### 1. Lab Overview

**Objective:** Learn how to read, search, filter, count, and process text files using Bash and Linux command-line utilities.

Linux administrators and cybersecurity analysts frequently work with text files.

Examples include:

- System logs.
- Authentication records.
- Configuration files.
- Application logs.
- Security reports.
- Network information.
- User activity records.

Manually inspecting large files can be time-consuming.

Bash provides commands that help automate this work.

In this lab, you will practise:

- Reading files.
- Searching for text.
- Filtering information.
- Counting lines and words.
- Extracting specific fields.
- Processing files with loops.
- Generating simple reports.

### 2. Preparing Your Lab

Create your working directory:

```bash
mkdir -p ~/practical-labs/module03/lab06
cd ~/practical-labs/module03/lab06
```

Confirm:

```bash
pwd
```

### 3. Creating Sample Data

We will use sample data rather than modifying real system logs.

Create a sample log:

```bash
nano application.log
```

Enter:

```text
2026-11-15 08:00:00 INFO Application started
2026-11-15 08:05:00 INFO User logged in
2026-11-15 08:10:00 WARNING Disk usage is high
2026-11-15 08:15:00 ERROR Database connection failed
2026-11-15 08:20:00 INFO Application restarted
2026-11-15 08:25:00 ERROR Authentication failed
2026-11-15 08:30:00 WARNING Memory usage is high
2026-11-15 08:35:00 INFO Backup completed
```

Save the file.

This is fictional log data created for the lab.

### 4. Reading Files with cat

The `cat` command displays file contents.

```bash
cat application.log
```

To display multiple files:

```bash
cat file1.txt file2.txt
```

To display line numbers:

```bash
cat -n application.log
```

This is useful when you need to reference a particular line.

### 5. Reading Files with head

The `head` command displays the beginning of a file.

Example:

```bash
head application.log
```

Display the first three lines:

```bash
head -n 3 application.log
```

Use this when you need to inspect the beginning of a large file.

### 6. Reading Files with tail

The `tail` command displays the end of a file.

Example:

```bash
tail application.log
```

Display the final three lines:

```bash
tail -n 3 application.log
```

You can also monitor a changing file:

```bash
tail -f application.log
```

Press `CTRL + C` to stop monitoring.

For this lab, only use `tail -f` on files you control.

### 7. Searching with grep

The `grep` command searches for matching text.

Example:

```bash
grep "ERROR" application.log
```

This displays lines containing `ERROR`.

Search for warnings:

```bash
grep "WARNING" application.log
```

Search without case sensitivity:

```bash
grep -i "error" application.log
```

Display matching line numbers:

```bash
grep -n "ERROR" application.log
```

Count matching lines:

```bash
grep -c "ERROR" application.log
```

Display lines that do not contain a particular pattern:

```bash
grep -v "INFO" application.log
```

### 8. Understanding Regular Expressions

Regular expressions allow you to search for patterns rather than only exact words.

Examples:

| Expression | Meaning |
|---|---|
| `^ERROR` | Lines beginning with ERROR |
| `ERROR$` | Lines ending with ERROR |
| `ERROR|WARNING` | ERROR or WARNING when using extended regex |
| `[0-9]` | Any digit |
| `[A-Z]` | Any uppercase letter |
| `.` | Any single character |

Example:

```bash
grep -E "ERROR|WARNING" application.log
```

This displays lines containing either `ERROR` or `WARNING`.

Search for lines beginning with a particular word:

```bash
grep "^2026" application.log
```

### 9. Counting Information with wc

The `wc` command counts information in files.

Examples:

```bash
wc application.log
```

Count lines:

```bash
wc -l application.log
```

Count words:

```bash
wc -w application.log
```

Count characters:

```bash
wc -m application.log
```

Count bytes:

```bash
wc -c application.log
```

Example:

```bash
grep "ERROR" application.log | wc -l
```

This counts the number of lines containing `ERROR`.

### 10. Sorting Information with sort

The `sort` command arranges lines in order.

Example:

```bash
sort application.log
```

Reverse sorting:

```bash
sort -r application.log
```

For numeric sorting, use:

```bash
sort -n numbers.txt
```

Create a numbers file:

```bash
printf "30\n10\n50\n20\n" > numbers.txt
```

Then:

```bash
sort -n numbers.txt
```

### 11. Removing Duplicate Lines with uniq

The `uniq` command removes adjacent duplicate lines.

Create a sample file:

```bash
printf "admin\nstudent\nadmin\nanalyst\nstudent\n" > users.txt
```

Sort and remove duplicates:

```bash
sort users.txt | uniq
```

Count repeated entries:

```bash
sort users.txt | uniq -c
```

Important: `uniq` only detects adjacent duplicates. Sorting first groups identical lines together.

### 12. Extracting Fields with cut

The `cut` command extracts specific sections of text.

Create:

```bash
nano students.csv
```

Enter:

```text
name,department,level
Abbas,Computer Science,3
Musa,Cybersecurity,2
Aisha,Software Engineering,3
```

Extract the first field:

```bash
cut -d ',' -f 1 students.csv
```

Extract the second field:

```bash
cut -d ',' -f 2 students.csv
```

Extract the first and third fields:

```bash
cut -d ',' -f 1,3 students.csv
```

Explanation:

- `-d ','` identifies the comma delimiter.
- `-f 1` selects the first field.
- `-f 2` selects the second field.

### 13. Using awk

`awk` is a powerful text-processing tool.

It can extract fields, perform calculations, and process structured text.

Example:

```bash
awk -F ',' '{print $1}' students.csv
```

This extracts the first field from every row.

Extract the second field:

```bash
awk -F ',' '{print $2}' students.csv
```

Skip the header:

```bash
awk -F ',' 'NR > 1 {print $1, $2}' students.csv
```

Here:

- `-F ','` sets the field separator.
- `$1` represents the first field.
- `$2` represents the second field.
- `NR` represents the current record number.
- `NR > 1` skips the first line.

### 14. Replacing Text with sed

The `sed` command can search and transform text.

Example:

```bash
sed 's/INFO/INFORMATION/' application.log
```

This displays the modified text without changing the original file.

Replace every occurrence on each line:

```bash
sed 's/INFO/INFORMATION/g' application.log
```

Save the transformed output into a new file:

```bash
sed 's/INFO/INFORMATION/g' application.log > processed.log
```

Check the new file:

```bash
cat processed.log
```

Important: Avoid editing original system logs during investigations. Preserve evidence and work on copies or controlled samples.

### 15. Using Pipes and Redirection

A pipe (`|`) sends the output of one command to another command.

Example:

```bash
grep "ERROR" application.log | wc -l
```

The process is:

1. `grep` searches for error lines.
2. The pipe sends the matching lines to `wc`.
3. `wc -l` counts those lines.

Redirection sends output to a file.

Example:

```bash
grep "WARNING" application.log > warnings.txt
```

Append additional output:

```bash
grep "ERROR" application.log >> warnings.txt
```

Difference:

- `>` replaces the destination file's contents.
- `>>` appends to the destination file.

### 16. Reading Files Line by Line in Bash

Bash can process files one line at a time.

Create:

```bash
nano read-lines.sh
```

Enter:

```bash
#!/bin/bash

while IFS= read -r line
do
    echo "Log entry: $line"
done < application.log
```

Execute:

```bash
bash read-lines.sh
```

Understanding the syntax:

- `while` starts the loop.
- `IFS=` prevents leading and trailing whitespace from being trimmed by the read operation.
- `read -r` reads the line without interpreting backslashes as escapes.
- `$line` contains the current line.
- `< application.log` provides the file as input.

This is a reliable pattern for reading text files line by line.

### 17. Creating an Automated Log Analyzer

Now combine several commands in one script.

Create:

```bash
nano log-analyzer.sh
```

Enter:

```bash
#!/bin/bash

log_file="application.log"

if [[ ! -f "$log_file" ]]; then
    echo "Error: Log file not found."
    exit 1
fi

echo "========== LOG ANALYSIS REPORT =========="

echo ""
echo "Total Lines:"
wc -l < "$log_file"

echo ""
echo "ERROR Entries:"
grep -c "ERROR" "$log_file"

echo ""
echo "WARNING Entries:"
grep -c "WARNING" "$log_file"

echo ""
echo "INFO Entries:"
grep -c "INFO" "$log_file"

echo ""
echo "========== ERROR DETAILS =========="

grep -n "ERROR" "$log_file"

echo ""
echo "========== ANALYSIS COMPLETE =========="
```

Execute:

```bash
bash log-analyzer.sh
```

This script:

1. Checks whether the log exists.
2. Counts all lines.
3. Counts error entries.
4. Counts warning entries.
5. Counts informational entries.
6. Displays the error entries with line numbers.

It performs read-only analysis on your sample log.

### 18. Practical Exercise 1 – Error Finder

Create:

```text
error-finder.sh
```

Requirements:

- Search `application.log`.
- Display every line containing `ERROR`.
- Include line numbers.
- Count the matching entries.

### 19. Practical Exercise 2 – Log Summary

Create:

```text
log-summary.sh
```

Requirements:

Display:

- Total number of lines.
- Total number of ERROR entries.
- Total number of WARNING entries.
- Total number of INFO entries.

Use `wc` and `grep`.

### 20. Practical Exercise 3 – CSV Extractor

Create:

```text
csv-extractor.sh
```

Requirements:

- Read `students.csv`.
- Extract student names.
- Extract departments.
- Skip the header when appropriate.
- Display the extracted information.

Use either `cut` or `awk`.

### 21. Practical Exercise 4 – Duplicate User Detector

Create:

```text
duplicate-users.sh
```

Requirements:

1. Read `users.txt`.
2. Sort the usernames.
3. Count repeated entries.
4. Display the usernames and their counts.

Use:

```bash
sort users.txt | uniq -c
```

### 22. Practical Exercise 5 – Automated File Processor

Create:

```text
file-processor.sh
```

Requirements:

1. Check whether `application.log` exists.
2. Read it line by line.
3. Display each line containing `ERROR`.
4. Count how many matching lines were found.
5. Display a final summary.

Use a Bash `while` loop and an appropriate counter.

### 23. Syntax Validation

Validate your scripts:

```bash
bash -n read-lines.sh
bash -n log-analyzer.sh
bash -n error-finder.sh
bash -n log-summary.sh
bash -n csv-extractor.sh
bash -n duplicate-users.sh
bash -n file-processor.sh
```

Inspect your files:

```bash
ls -l
```

### 24. Cybersecurity Application

File processing is an important skill in security operations.

Security analysts frequently investigate:

- Failed authentication attempts.
- Suspicious application errors.
- Repeated warning messages.
- Unexpected account activity.
- System event records.

For example, a defensive analyst can search a supplied authentication-log sample for failed-login messages:

```bash
grep -i "failed" authentication-sample.log
```

Count the matching entries:

```bash
grep -ic "failed" authentication-sample.log
```

Display line numbers:

```bash
grep -in "failed" authentication-sample.log
```

Only inspect logs from systems you own or are authorized to investigate.

Remember that a matching log entry is an indicator to investigate, not proof of malicious activity.

### 25. Knowledge Check

1. What does `grep` do?
2. What is the purpose of `head`?
3. What is the difference between `head` and `tail`?
4. What does `wc -l` count?
5. What does `sort` do?
6. Why should you sort a file before using `uniq` to detect all duplicate lines?
7. What is the purpose of `cut`?
8. What is `awk` used for?
9. What does `sed` do?
10. What is the difference between `>` and `>>`?
11. Why is `read -r` useful when processing files?
12. Why should original investigation logs be preserved?

### 26. Lab Completion Checklist

- [ ] Created the Lab 06 directory.
- [ ] Created sample log data.
- [ ] Read files using `cat`, `head`, and `tail`.
- [ ] Searched files using `grep`.
- [ ] Used regular expressions.
- [ ] Counted lines and words using `wc`.
- [ ] Sorted and filtered data.
- [ ] Removed duplicate entries.
- [ ] Extracted fields using `cut`.
- [ ] Processed structured text using `awk`.
- [ ] Transformed text using `sed`.
- [ ] Used pipes and redirection.
- [ ] Read files line by line using Bash.
- [ ] Created a log analyzer.
- [ ] Completed all five practical exercises.
- [ ] Checked Bash syntax.

### 27. Expected Outcome

By completing this lab, you should understand how to:

- Read and inspect text files.
- Search for specific patterns.
- Extract useful information.
- Count and filter records.
- Process CSV files.
- Automate log analysis.
- Build simple defensive security reporting tools.

**Next Lab:** Command-Line Automation.