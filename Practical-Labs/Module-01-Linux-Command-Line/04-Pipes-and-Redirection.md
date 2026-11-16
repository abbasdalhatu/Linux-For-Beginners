
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 04: PIPES AND REDIRECTION

---

## 1. Lab Objectives

In this lab, you will learn how Linux handles command input and output.

By the end, you should understand:

1. Standard input, standard output, and standard error.
2. Output redirection.
3. Input redirection.
4. Appending output to files.
5. Pipelines.
6. Combining multiple commands.
7. Separating errors from normal output.
8. Building practical command-line workflows.

## 2. Prepare Your Lab Environment

Create your working directory:

```bash
mkdir -p ~/practical-labs/module01/lab04
cd ~/practical-labs/module01/lab04
```

Verify:

```bash
pwd
```

All exercises will use this directory.

---

## 3. Exercise 1: Understand Standard Streams

Linux programs normally use three standard data streams.

| Stream | Number | Purpose |
|---|---:|---|
| Standard input (stdin) | 0 | Receives input |
| Standard output (stdout) | 1 | Displays normal output |
| Standard error (stderr) | 2 | Displays error messages |

### Test standard output

```bash
echo "This is normal output"
```

### Test standard error

```bash
ls /directory-that-does-not-exist
```

The command should produce an error message.

### Test both streams

```bash
ls /etc /directory-that-does-not-exist
```

You should see normal output from `/etc` and an error for the missing directory.

---

## 4. Exercise 2: Redirect Standard Output

The `>` operator sends standard output into a file.

Create a report:

```bash
echo "Linux practical laboratory" > report.txt
```

Display it:

```bash
cat report.txt
```

Redirect command output:

```bash
date > date.txt
```

Inspect:

```bash
cat date.txt
```

Save directory information:

```bash
ls -lah > directory-list.txt
```

Inspect:

```bash
cat directory-list.txt
```

**Important:** `>` replaces existing file content. Use it carefully.

---

## 5. Exercise 3: Append Output

The `>>` operator adds output to the end of a file.

Run:

```bash
echo "First line" > records.txt
echo "Second line" >> records.txt
echo "Third line" >> records.txt
```

Inspect:

```bash
cat records.txt
```

Expected result:

```text
First line
Second line
Third line
```

Append system information:

```bash
hostname >> records.txt
date >> records.txt
whoami >> records.txt
```

Inspect again:

```bash
cat records.txt
```

---

## 6. Exercise 4: Redirect Standard Error

Use `2>` to redirect error messages.

Run:

```bash
ls /missing-directory 2> errors.txt
```

Display the error report:

```bash
cat errors.txt
```

Redirect normal output and errors separately:

```bash
ls /etc /missing-directory > normal.txt 2> errors.txt
```

Inspect both:

```bash
cat normal.txt
cat errors.txt
```

This is useful when troubleshooting because normal results and error messages are kept separate.

---

## 7. Exercise 5: Redirect Both Output Streams

Use:

```bash
ls /etc /missing-directory > complete-report.txt 2>&1
```

This sends standard output into the file and then redirects standard error to the same destination.

Inspect:

```bash
cat complete-report.txt
```

You can also use Bash shorthand:

```bash
ls /etc /missing-directory &> complete-report.txt
```

Note: `&>` is Bash syntax and is not equally portable across all shells.

### Important distinction

```bash
command > file 2>&1
```

and:

```bash
command 2>&1 > file
```

are not equivalent because redirections are processed from left to right.

---

## 8. Exercise 6: Use Standard Input

The `<` operator sends a file's contents to a command as standard input.

Create a file:

```bash
cat > names.txt <<'EOF'
Abbas
Musa
Amina
Fatima
Ibrahim
EOF
```

Count its lines using input redirection:

```bash
wc -l < names.txt
```

Sort the names:

```bash
sort < names.txt
```

Search for Amina:

```bash
grep "Amina" < names.txt
```

The file becomes the command's input rather than a filename argument.

---

## 9. Exercise 7: Understand Here Documents

A here document allows you to provide multiple lines of input directly to a command.

Example:

```bash
cat <<'EOF'
Linux
Cybersecurity
Networking
System Administration
EOF
```

Create a file using a here document:

```bash
cat > lab-notes.txt <<'EOF'
LAB 04: PIPES AND REDIRECTION

Topics:
- Standard streams
- Output redirection
- Input redirection
- Pipelines
- Error handling
EOF
```

Inspect:

```bash
cat lab-notes.txt
```

The quoted delimiter `'EOF'` prevents shell expansion inside the document.

---

## 10. Exercise 8: Understand Pipelines

The pipe symbol `|` passes one command's standard output to another command's standard input.

Example:

```bash
ls /etc | less
```

Count files:

```bash
ls /etc | wc -l
```

Search for configuration files:

```bash
ls /etc | grep ".conf"
```

Sort directory entries:

```bash
ls /etc | sort
```

Count matching files:

```bash
ls /etc | grep ".conf" | wc -l
```

### How a pipeline works

Consider:

```bash
cat names.txt | sort | uniq
```

The workflow is:

1. `cat` reads the file.
2. `sort` orders the output.
3. `uniq` removes adjacent duplicate lines.

A pipeline connects commands so that the output of one becomes the input of the next.

---

## 11. Exercise 9: Build a Pipeline

Create a sample log file:

```bash
cat > application.log <<'EOF'
INFO: Application started
WARNING: High memory usage
ERROR: Database connection failed
INFO: User logged in
ERROR: Authentication failed
WARNING: Disk space low
INFO: Backup completed
ERROR: Database connection failed
INFO: Application stopped
EOF
```

### Task A: Find errors

```bash
cat application.log | grep "ERROR"
```

### Task B: Count errors

```bash
cat application.log | grep "ERROR" | wc -l
```

### Task C: Find warnings

```bash
cat application.log | grep "WARNING"
```

### Task D: Count each message type

```bash
cat application.log | cut -d':' -f1 | sort | uniq -c
```

### Task E: Find the most frequent message type

```bash
cat application.log | cut -d':' -f1 | sort | uniq -c | sort -nr
```

You can simplify the first command by letting `grep` read the file directly:

```bash
grep "ERROR" application.log
```

Both approaches are useful, but unnecessary `cat` commands can make pipelines longer than needed.

---

## 12. Exercise 10: Use tee

The `tee` command displays output and writes it to a file simultaneously.

Run:

```bash
date | tee date-report.txt
```

The date should appear in the terminal and be saved in the file.

Append output:

```bash
hostname | tee -a date-report.txt
```

Inspect:

```bash
cat date-report.txt
```

### Practical task

Create a system information report while displaying the results:

```bash
{
    echo "SYSTEM INFORMATION"
    hostname
    whoami
    uname -r
    uptime
    free -h
} | tee system-report.txt
```

---

## 13. Exercise 11: Combine Commands with Conditional Operators

Linux provides operators for controlling command execution.

### Semicolon `;`

Runs commands sequentially, regardless of whether earlier commands succeed.

```bash
echo "First command"; echo "Second command"
```

### AND operator `&&`

Runs the second command only if the first succeeds.

```bash
mkdir test-directory && cd test-directory
```

### OR operator `||`

Runs the second command if the first fails.

```bash
cd missing-directory || echo "Directory not found"
```

### Practical task

Run:

```bash
mkdir lab-success && echo "Directory created successfully"
```

Then:

```bash
cd missing-directory || echo "Navigation failed"
```

Explain the difference between `&&` and `||`.

---

## 14. Exercise 12: Build a Command-Line Report Generator

Create:

```bash
nano report-generator.sh
```

Enter:

```bash
#!/bin/bash

REPORT="system-summary.txt"
ERRORS="system-errors.txt"

{
    echo "=================================="
    echo "      SYSTEM SUMMARY REPORT"
    echo "=================================="

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "CURRENT USER:"
    whoami

    echo
    echo "KERNEL:"
    uname -r

    echo
    echo "UPTIME:"
    uptime

    echo
    echo "MEMORY:"
    free -h

    echo
    echo "DISK:"
    df -h

} > "$REPORT" 2> "$ERRORS"

echo "Report saved to: $REPORT"
echo "Error output saved to: $ERRORS"
```

Make it executable:

```bash
chmod +x report-generator.sh
```

Run:

```bash
./report-generator.sh
```

Inspect:

```bash
cat system-summary.txt
cat system-errors.txt
```

The error file may be empty if no errors occurred.

---

## 15. Practical Challenge

Create a pipeline that performs the following operations on `application.log`:

1. Extract message types.
2. Sort them.
3. Count their occurrences.
4. Sort the counts from highest to lowest.
5. Save the result to `message-summary.txt`.

Then:

1. Find all ERROR messages.
2. Save them to `error-report.txt`.
3. Count the total number of errors.
4. Display the final count.
5. Save your complete system report using `tee`.

You should be able to perform these operations without modifying the original sample log.

---

## 16. Knowledge Check

Answer in your notebook:

1. What is standard input?
2. What is standard output?
3. What is standard error?
4. What does `>` do?
5. What does `>>` do?
6. What does `2>` redirect?
7. What is the purpose of `2>&1`?
8. What does the pipe symbol do?
9. What is the purpose of `tee`?
10. Explain the difference between `&&` and `||`.

---

## 17. Lab Completion Checklist

- [ ] Practiced standard input, output, and error.
- [ ] Redirected output to files.
- [ ] Appended output to files.
- [ ] Redirected errors.
- [ ] Redirected both output streams.
- [ ] Used input redirection.
- [ ] Created here documents.
- [ ] Built command pipelines.
- [ ] Used `tee`.
- [ ] Practiced conditional operators.
- [ ] Built a report generator.
- [ ] Completed the practical challenge.

**END OF LAB 04**
