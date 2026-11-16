# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 10: MODULE 03 PRACTICAL ASSESSMENT

### 1. Assessment Overview

**Assessment Title:** Bash Scripting and Security Automation

**Objective:** Evaluate your ability to design, write, execute, debug, and validate Bash scripts.

You have completed nine practical labs covering:

1. Bash scripting fundamentals.
2. Variables and user input.
3. Conditional statements.
4. Loops and iteration.
5. Functions and arguments.
6. File processing and text manipulation.
7. Command-line automation.
8. Error handling and debugging.
9. Security automation scripts.

This final assessment combines those skills into practical tasks.

### 2. Assessment Rules

- Complete all exercises inside the dedicated assessment directory.
- Do not modify system files.
- Do not use `sudo` unless explicitly required.
- Use sample files for testing.
- Validate your scripts before executing them.
- Use meaningful variable and function names.
- Include error handling.
- Keep your scripts readable and organized.

### 3. Prepare the Assessment Environment

```bash
mkdir -p ~/practical-labs/module03/lab10
cd ~/practical-labs/module03/lab10
```

Create a sample data directory:

```bash
mkdir -p data reports backups scripts
```

Create sample files:

```bash
echo "Linux scripting assessment" > data/notes.txt
echo "User,Department" > data/users.csv
echo "student,Computer Science" >> data/users.csv
echo "Failed login for test" > data/security.log
```

Inspect:

```bash
find . -maxdepth 2 -type f
```

---

## SECTION A: Bash Fundamentals

### Task 1 – Personal Information Script

Create:

`personal-info.sh`

Requirements:

1. Create variables for your name, department, and course.
2. Display each variable.
3. Display the current date.
4. Display the current username.
5. Display the hostname.

Expected skills:

- Variables.
- Command substitution.
- `echo`.
- System information commands.

### Task 2 – Interactive User Input

Create:

`user-input.sh`

Requirements:

1. Ask the user for their name.
2. Ask for their department.
3. Ask for their academic level.
4. Validate that no field is empty.
5. Display a formatted summary.

Expected skills:

- `read`.
- Variables.
- Conditional statements.
- Input validation.

---

## SECTION B: Conditions and Loops

### Task 3 – File Validation Tool

Create:

`file-validator.sh`

Requirements:

1. Ask for a filename.
2. Check whether it exists inside the `data` directory.
3. Determine whether it is a regular file.
4. Check whether it is readable.
5. Display an appropriate result.
6. Return a non-zero exit status if validation fails.

Expected skills:

- `if`.
- `[[ ]]`.
- File tests.
- Exit statuses.

### Task 4 – Automated Report Generator

Create:

`report-generator.sh`

Requirements:

1. Generate five report files.
2. Name them `report-1.txt` through `report-5.txt`.
3. Write the current date into every report.
4. Include a unique report number.
5. Display a completion message.

Expected skills:

- `for` loops.
- File creation.
- Redirection.
- Variables.

---

## SECTION C: Functions and File Processing

### Task 5 – Reusable File Counter

Create:

`file-counter.sh`

Requirements:

1. Define a function called `count_files`.
2. Accept a directory as an argument.
3. Check whether the directory exists.
4. Count regular files inside it.
5. Display the result.
6. Handle invalid directory arguments.

Expected skills:

- Functions.
- Arguments.
- `find`.
- Command substitution.

### Task 6 – CSV Data Extractor

Create:

`csv-extractor.sh`

Requirements:

1. Read `data/users.csv`.
2. Display the header.
3. Extract usernames.
4. Extract departments.
5. Display the number of data records.

Expected skills:

- `head`.
- `cut`.
- `tail`.
- `wc`.
- Text processing.

---

## SECTION D: Automation and Error Handling

### Task 7 – Automated Backup Utility

Create:

`backup-utility.sh`

Requirements:

1. Check whether `data/notes.txt` exists.
2. Create a timestamp.
3. Create a backup inside `backups/`.
4. Preserve the original file.
5. Verify that the backup exists.
6. Display its location.
7. Return an appropriate exit status.

Expected skills:

- File validation.
- `mkdir`.
- `cp`.
- Timestamps.
- Error handling.

### Task 8 – Debugging Challenge

Create:

`debugging-test.sh`

Start with this intentionally incorrect script:

```bash
#!/bin/bash

set -u

username="student"

if [[ -f "data/notes.txt" ]
then
    echo "Welcome $user"
    echo "Assessment started."
fi
```

Your tasks:

1. Identify the syntax error.
2. Identify the incorrect variable reference.
3. Correct the script.
4. Add suitable error handling.
5. Validate using `bash -n`.
6. Execute using `bash -x`.

Expected skills:

- Debugging.
- Strict-mode options.
- Syntax validation.
- Execution tracing.

---

## SECTION E: Defensive Security Automation

### Task 9 – Security Log Analyzer

Create:

`security-analyzer.sh`

Requirements:

1. Read `data/security.log`.
2. Check whether the file exists.
3. Search for failed login events.
4. Count matching events.
5. Save the results in `reports/security-analysis.txt`.
6. Display a completion message.

Expected skills:

- `grep`.
- `wc`.
- Redirection.
- Conditional statements.

### Task 10 – File Integrity Checker

Create:

`integrity-checker.sh`

Requirements:

1. Check whether `data/notes.txt` exists.
2. Generate a SHA-256 baseline if one does not exist.
3. Verify the file against the existing baseline on subsequent runs.
4. Report whether the file changed.
5. Return a non-zero status when verification fails.

Save the baseline inside `backups/`.

Expected skills:

- SHA-256.
- File validation.
- Conditional statements.
- Exit statuses.

---

## SECTION F: Final Integrated Project

### Task 11 – Linux Security Administration Toolkit

<badge color="success">Capstone exercise</badge>

Create:

`linux-security-toolkit.sh`

Build a single Bash script that combines your knowledge from Module 03.

Your toolkit must contain the following functions:

**Function 1: System Information**

Collect:

- Hostname.
- Current username.
- Kernel version.
- System uptime.
- Current date and time.

**Function 2: File Inspection**

- Check whether `data/notes.txt` exists.
- Display its permissions.
- Check readability.
- Check writability.

**Function 3: Log Analysis**

- Read `data/security.log`.
- Search for failed login events.
- Count matching events.
- Save the findings to a report.

**Function 4: Backup Management**

- Check whether the source file exists.
- Generate a timestamp.
- Create a backup.
- Verify that the backup exists.
- Preserve the original file.

**Function 5: Report Generation**

Generate:

`reports/final-security-report.txt`

The report must contain:

- Assessment date.
- Host information.
- File inspection results.
- Security log findings.
- Backup status.

**Function 6: Error Handling**

Your script must:

- Check required files.
- Validate important operations.
- Display meaningful error messages.
- Use appropriate exit statuses.
- Avoid unnecessary system-level permissions.

### 4. Suggested Project Structure

Your final directory should resemble:

```text
lab10/
│
├── data/
│   ├── notes.txt
│   ├── users.csv
│   └── security.log
│
├── reports/
│   ├── security-analysis.txt
│   └── final-security-report.txt
│
├── backups/
│   ├── notes-YYYY-MM-DD_HH-MM-SS.backup
│   └── notes.sha256
│
├── scripts/
│
├── personal-info.sh
├── user-input.sh
├── file-validator.sh
├── report-generator.sh
├── file-counter.sh
├── csv-extractor.sh
├── backup-utility.sh
├── debugging-test.sh
├── security-analyzer.sh
├── integrity-checker.sh
└── linux-security-toolkit.sh
```

Your exact backup filename will contain the timestamp generated during execution.

### 5. Final Validation

Validate your scripts:

```bash
for file in *.sh
do
    echo "Checking: $file"
    bash -n "$file" || echo "Syntax problem found in $file"
done
```

Inspect your project:

```bash
find . -maxdepth 3 -type f
```

Check your report:

```bash
cat reports/final-security-report.txt
```

Check your backup directory:

```bash
ls -lh backups/
```

### 6. Assessment Rubric

| Assessment area | Marks |
|---|---:|
| Bash fundamentals | 10 |
| Variables and input | 10 |
| Conditions and loops | 15 |
| Functions and arguments | 10 |
| File processing | 10 |
| Automation and backups | 15 |
| Error handling and debugging | 10 |
| Defensive security automation | 10 |
| Integrated toolkit | 10 |
| **Total** | **100** |

Suggested performance guide:

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Requires further practice |

### 7. Final Knowledge Review

1. Explain what a Bash script is.
2. What is the purpose of a shebang?
3. Explain the difference between `$1`, `$@`, and `$#`.
4. What is the difference between `=` and `-eq` in Bash conditions?
5. What does `for` do?
6. What is the purpose of a function?
7. Explain input validation.
8. What does `$?` represent?
9. Explain `set -euo pipefail`.
10. What is the difference between `bash -n` and `bash -x`?
11. Why are backups important?
12. What is SHA-256?
13. How can Bash help with defensive cybersecurity?
14. Why should automation scripts be tested before production use?
15. Why should sensitive information not be stored in ordinary logs?

### 8. Module 03 Completion Checklist

- [ ] Completed Lab 01 – Bash Fundamentals.
- [ ] Completed Lab 02 – Variables and Input.
- [ ] Completed Lab 03 – Conditional Statements.
- [ ] Completed Lab 04 – Loops and Iteration.
- [ ] Completed Lab 05 – Functions and Arguments.
- [ ] Completed Lab 06 – File Processing.
- [ ] Completed Lab 07 – Command-Line Automation.
- [ ] Completed Lab 08 – Error Handling and Debugging.
- [ ] Completed Lab 09 – Security Automation.
- [ ] Completed Lab 10 – Practical Assessment.
- [ ] Built the Linux Security Administration Toolkit.

### 9. Final Learning Outcome

After completing Module 03, you should be able to:

- Write Bash scripts independently.
- Work with variables and user input.
- Apply conditional logic.
- Use loops and functions.
- Process files and text.
- Automate administrative tasks.
- Create basic backup utilities.
- Debug and validate scripts.
- Handle errors responsibly.
- Build basic defensive security automation tools.

---

# MODULE 03 – COMPLETION

**Module:** Bash Scripting Challenges

**Status:** Ready for assessment and review.

**Next Module:** Module 04 – Linux System Administration.