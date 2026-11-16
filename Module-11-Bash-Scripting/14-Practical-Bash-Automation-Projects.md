
# Module 11 – Lesson 14: Practical Bash Automation Projects

## 1. Learning Objectives

In this lesson, we will combine everything learned in Module 11 to build practical automation tools.

By the end, you should be able to:

1. Design a Bash automation script.
2. Automate repetitive Linux tasks.
3. Create system information reports.
4. Monitor files and directories.
5. Build backup automation.
6. Analyze logs automatically.
7. Create interactive command-line tools.
8. Apply Bash scripting to defensive cybersecurity tasks.

---

## 2. What Is Automation?

Automation means using technology to perform tasks with minimal manual intervention.

Imagine you perform the same task every day:

- Check disk space.
- Review system information.
- Inspect log files.
- Create backups.
- Count files.
- Check whether important commands are available.

Instead of typing every command manually, you can create a Bash script that performs the tasks automatically.

### Real-World Applications

| Area | Automation Example |
|---|---|
| System Administration | Disk-space monitoring |
| Cybersecurity | Log analysis |
| Networking | Network information reports |
| Software Engineering | Testing and deployment scripts |
| File Management | Backup automation |
| IT Support | System diagnostic reports |

---

## 3. Project 1: System Information Reporter

### Objective

Create a script that collects basic information about a Linux system.

It will display:

- Hostname.
- Current user.
- Linux kernel version.
- Operating system information.
- CPU information.
- Memory information.
- Disk usage.

### Step 1: Create the Script

```bash
nano system-reporter.sh
```

Enter:

```bash
#!/bin/bash

set -u

echo "===================================="
echo "       LINUX SYSTEM REPORT"
echo "===================================="

echo
echo "Hostname:"
hostname

echo
echo "Current User:"
whoami

echo
echo "Kernel Information:"
uname -r

echo
echo "Operating System:"
if [[ -f /etc/os-release ]]; then
    grep '^PRETTY_NAME=' /etc/os-release
else
    echo "OS information unavailable."
fi

echo
echo "CPU Information:"
if command -v lscpu >/dev/null 2>&1; then
    lscpu | grep -E 'Model name|Architecture|CPU\(s\)' | head -n 5
else
    echo "CPU information unavailable."
fi

echo
echo "Memory Usage:"
free -h

echo
echo "Disk Usage:"
df -h /

echo
echo "===================================="
echo "Report completed."
```

### Step 2: Execute

```bash
chmod +x system-reporter.sh
./system-reporter.sh
```

### What You Learned

- System information commands.
- Conditional statements.
- Command availability checks.
- Pipelines.
- Report formatting.

---

## 4. Project 2: Disk Space Monitor

### Objective

Create a script that checks disk usage and warns you when usage reaches a specified threshold.

This is useful for Linux administrators who need to prevent storage exhaustion.

### Step 1: Create the Script

```bash
nano disk-monitor.sh
```

Enter:

```bash
#!/bin/bash

set -euo pipefail

threshold=80

usage=$(df -P / | awk 'NR==2 {gsub("%","",$5); print $5}')

echo "========== DISK SPACE MONITOR =========="

echo "Filesystem: /"
echo "Current usage: ${usage}%"
echo "Warning threshold: ${threshold}%"

if (( usage >= threshold )); then
    echo "WARNING: Disk usage has reached the threshold."
else
    echo "STATUS: Disk usage is within the threshold."
fi

echo "========================================"
```

### Step 2: Execute

```bash
chmod +x disk-monitor.sh
./disk-monitor.sh
```

### Understanding the Script

- `df -P /` displays disk usage for the root filesystem.
- `awk` extracts the percentage.
- `gsub("%","",$5)` removes the percentage sign.
- `(( ))` performs an arithmetic comparison.
- `threshold=80` sets the warning level.

**Important:** This script only reports disk usage. It does not automatically delete files or change storage settings.

---

## 5. Project 3: Automatic Backup Tool

### Objective

Create a script that copies a selected file into a backup directory.

For this exercise, all files will remain inside your lesson laboratory.

### Step 1: Create the Backup Script

```bash
nano backup-tool.sh
```

Enter:

```bash
#!/bin/bash

set -euo pipefail

source_dir="$HOME/module11-lab/lesson14"
backup_dir="$source_dir/backups"

mkdir -p "$backup_dir"

if [[ $# -ne 1 ]]; then
    echo "Usage: $0 filename" >&2
    exit 1
fi

filename="$1"
source_file="$source_dir/$filename"

if [[ ! -f "$source_file" ]]; then
    echo "ERROR: Source file does not exist." >&2
    exit 1
fi

if [[ ! -r "$source_file" ]]; then
    echo "ERROR: Source file cannot be read." >&2
    exit 1
fi

timestamp=$(date '+%Y%m%d_%H%M%S')

backup_file="$backup_dir/${filename}.${timestamp}.bak"

cp -- "$source_file" "$backup_file"

echo "Backup completed successfully."
echo "Backup location: $backup_file"

if cmp -s -- "$source_file" "$backup_file"; then
    echo "Integrity check: PASSED"
else
    echo "Integrity check: FAILED" >&2
    exit 1
fi
```

### Step 2: Create a Test File

```bash
echo "This is my backup practice file." > important.txt
```

### Step 3: Make the Script Executable

```bash
chmod +x backup-tool.sh
```

### Step 4: Run the Backup

```bash
./backup-tool.sh important.txt
```

### Step 5: Inspect the Backup

```bash
ls -l backups/
```

### Step 6: Understand the Timestamp

A timestamp such as:

```text
20261115_143000
```

represents:

- Year: 2026
- Month: 11
- Day: 15
- Time: 14:30:00

Timestamps help prevent backups from overwriting one another.

**Security note:** This is a basic local backup demonstration. A production backup system should also consider encryption, retention, access controls, and recovery testing.

---

## 6. Project 4: File Organizer

### Objective

Create a script that sorts selected file types into separate folders.

We will only work inside the lesson laboratory.

### Step 1: Create the Script

```bash
nano file-organizer.sh
```

Enter:

```bash
#!/bin/bash

set -euo pipefail

base_dir="$HOME/module11-lab/lesson14"

cd "$base_dir"

mkdir -p documents images scripts archives other

for file in *; do

    [[ -f "$file" ]] || continue

    case "$file" in
        *.txt|*.pdf|*.docx)
            destination="documents"
            ;;
        *.png|*.jpg|*.jpeg)
            destination="images"
            ;;
        *.sh)
            destination="scripts"
            ;;
        *.zip|*.tar|*.gz)
            destination="archives"
            ;;
        *)
            destination="other"
            ;;
    esac

    case "$file" in
        file-organizer.sh)
            continue
            ;;
    esac

    if [[ "$destination" == "other" ]]; then
        continue
    fi

    mv -- "$file" "$destination/"

    echo "Moved $file into $destination/"
done

echo "File organization completed."
```

### Step 2: Create Test Files

```bash
touch report.txt
touch photo.jpg
touch program.sh
touch backup.zip
touch notes.pdf
```

### Step 3: Execute

```bash
chmod +x file-organizer.sh
./file-organizer.sh
```

### Step 4: Inspect the Folders

```bash
find . -maxdepth 2 -type f
```

### What You Learned

- Loops.
- File extensions.
- `case` statements.
- Directory creation.
- File movement.
- Safe quoting of filenames.

**Important:** This project moves files. Test it only with the sample files created inside your lesson directory.

---

## 7. Project 5: Security Log Analyzer

### Objective

Create an automation tool that checks a log file for:

- Failed login messages.
- Errors.
- Warnings.
- IP-like addresses.
- Repeated IP addresses.

### Step 1: Create a Sample Log

```bash
cat > application.log <<'EOF'
INFO Application started
ERROR Database connection failed
WARNING Multiple requests detected
FAILED LOGIN from 192.168.1.10
FAILED LOGIN from 192.168.1.20
FAILED LOGIN from 192.168.1.10
INFO Application running
ERROR File access problem
EOF
```

### Step 2: Create the Analyzer

```bash
nano security-analyzer.sh
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
    echo "ERROR: Log file missing or unreadable." >&2
    exit 1
fi

failed_count=$(grep -ic "FAILED LOGIN" "$logfile" || true)
error_count=$(grep -ic "ERROR" "$logfile" || true)
warning_count=$(grep -ic "WARNING" "$logfile" || true)

echo "========== SECURITY ANALYSIS =========="

echo "Log file: $logfile"
echo "Failed login entries: $failed_count"
echo "Error entries: $error_count"
echo "Warning entries: $warning_count"

echo
echo "Failed login records:"
grep -in "FAILED LOGIN" "$logfile" || true

echo
echo "IP-like addresses and frequencies:"
grep -E -o '[0-9]{1,3}(\.[0-9]{1,3}){3}' "$logfile" |
    sort |
    uniq -c || true

echo "======================================="
```

### Step 3: Execute

```bash
chmod +x security-analyzer.sh
./security-analyzer.sh application.log
```

### Step 4: Inspect the Results

Compare the report against the original log.

You should find:

- 3 failed login entries.
- 2 error entries.
- 1 warning entry.
- `192.168.1.10` appearing twice.
- `192.168.1.20` appearing once.

### Cybersecurity Application

A security analyst can use similar techniques to investigate authentication logs and identify repeated suspicious activity.

However, repeated failed logins alone do not prove an attack. Analysts must investigate context, source, timing, and other evidence.

---

## 8. Project 6: Interactive Linux Administration Menu

### Objective

Build a menu-driven program that allows a user to select different system information operations.

### Step 1: Create the Script

```bash
nano admin-menu.sh
```

Enter:

```bash
#!/bin/bash

set -u

while true; do

    echo
    echo "========== LINUX ADMIN MENU =========="
    echo "1. Show current user"
    echo "2. Show hostname"
    echo "3. Show kernel information"
    echo "4. Show memory usage"
    echo "5. Show disk usage"
    echo "6. Show IP information"
    echo "7. Exit"
    echo "======================================"

    read -r -p "Select an option: " choice

    case "$choice" in
        1)
            whoami
            ;;
        2)
            hostname
            ;;
        3)
            uname -a
            ;;
        4)
            free -h
            ;;
        5)
            df -h
            ;;
        6)
            if command -v ip >/dev/null 2>&1; then
                ip address
            else
                echo "The ip command is unavailable."
            fi
            ;;
        7)
            echo "Exiting administration menu."
            break
            ;;
        *)
            echo "Invalid selection. Try again."
            ;;
    esac

done
```

### Step 2: Execute

```bash
chmod +x admin-menu.sh
./admin-menu.sh
```

Try every option.

---

## 9. Project 7: Automated File Integrity Checker

### Objective

Build a script that creates and verifies SHA-256 hashes of files inside a dedicated directory.

This introduces a basic file-integrity monitoring technique.

### Step 1: Create a Protected Test Directory

```bash
mkdir -p integrity-lab
echo "Important test configuration" > integrity-lab/config.txt
echo "Another test document" > integrity-lab/readme.txt
```

### Step 2: Create the Script

```bash
nano integrity-checker.sh
```

Enter:

```bash
#!/bin/bash

set -euo pipefail

lab_dir="$HOME/module11-lab/lesson14/integrity-lab"
hash_file="$lab_dir/checksums.sha256"

if [[ ! -d "$lab_dir" ]]; then
    echo "ERROR: Integrity directory not found." >&2
    exit 1
fi

cd "$lab_dir"

case "${1:-}" in
    create)
        if [[ -f "$hash_file" ]]; then
            echo "ERROR: A baseline already exists." >&2
            echo "Use verify, or deliberately create a new baseline." >&2
            exit 1
        fi

        find . -maxdepth 1 -type f ! -name "checksums.sha256" \
            -print0 |
            sort -z |
            xargs -0 -r sha256sum > "$hash_file"

        echo "Integrity baseline created."
        ;;
    verify)
        if [[ ! -f "$hash_file" ]]; then
            echo "ERROR: Baseline does not exist." >&2
            exit 1
        fi

        sha256sum --check "$hash_file"
        ;;
    *)
        echo "Usage: $0 {create|verify}" >&2
        exit 1
        ;;
esac
```

### Step 3: Execute

```bash
chmod +x integrity-checker.sh
```

Create the baseline:

```bash
./integrity-checker.sh create
```

Verify the files:

```bash
./integrity-checker.sh verify
```

### Step 4: Test Integrity Detection

Change the test configuration:

```bash
echo "Unexpected modification" >> integrity-lab/config.txt
```

Run verification again:

```bash
./integrity-checker.sh verify
```

The modified file should fail its hash check.

### Important Security Concept

A hash can detect changes relative to a trusted baseline.

However, if an attacker can modify both the protected file and its checksum baseline, they may hide the modification. Production integrity monitoring should protect the baseline separately.

---

## 10. Understanding Automation Best Practices

Good automation should be:

### 1. Safe

Avoid unnecessary destructive commands.

### 2. Reliable

Check important operations and handle failures.

### 3. Reusable

Use functions and arguments instead of hardcoding everything.

### 4. Understandable

Use meaningful variable names and comments.

### 5. Secure

Avoid exposing passwords, tokens, or other secrets in scripts.

### 6. Testable

Test scripts with valid inputs, invalid inputs, and unexpected conditions.

### 7. Documented

Explain the script's purpose, requirements, and usage.

---

## 11. Bash Script Testing Checklist

Before trusting a script, check:

| Test | Purpose |
|---|---|
| `bash -n script.sh` | Check syntax |
| `bash -x script.sh` | Trace execution |
| Valid input | Confirm normal operation |
| Invalid input | Confirm validation |
| Missing file | Confirm error handling |
| Permission issue | Confirm failure handling |
| Empty input | Confirm edge-case handling |
| Expected output | Confirm correctness |

You should never assume that a script is reliable simply because it runs once.

---

## 12. Practical Final Challenge

Create a directory called:

```bash
mkdir -p ~/module11-lab/lesson14/final-project
cd ~/module11-lab/lesson14/final-project
```

Build a small Bash administration toolkit containing these scripts:

1. `system-reporter.sh`
2. `disk-monitor.sh`
3. `backup-tool.sh`
4. `security-analyzer.sh`
5. `admin-menu.sh`

### Requirements

Your toolkit must:

- Display system information.
- Check disk usage.
- Back up a selected test file.
- Analyze a sample security log.
- Provide an interactive menu.
- Validate input.
- Display useful error messages.
- Use appropriate exit codes.
- Pass a Bash syntax check.

### Final Verification

Run:

```bash
for script in *.sh; do
    echo "Checking $script"
    bash -n "$script" || exit 1
done
```

If all scripts pass, you should see each filename without a syntax error.

Remember: syntax validation does not guarantee that the script behaves correctly. Execute and test each script as well.

---

## 13. Lesson Summary

In this lesson, you built practical automation tools using:

- Variables.
- Functions.
- Conditional statements.
- Loops.
- File operations.
- Pipelines.
- Text processing.
- Exit codes.
- Error handling.
- Logging.
- SHA-256 checksums.

You have now applied Bash to basic Linux administration and defensive cybersecurity tasks.

---

## 14. Knowledge Check

Answer these questions in your notebook:

1. What is automation?
2. Why is Bash useful for Linux administration?
3. What information can a system reporter collect?
4. How does a disk-space monitor work?
5. Why are timestamps useful in backups?
6. What is the purpose of a file organizer?
7. How can Bash scripts help analyze security logs?
8. What is the difference between a checksum and encryption?
9. Why should a backup be tested?
10. What are five characteristics of a good automation script?

### Practical Assessment

Successfully demonstrate:

- System information reporting.
- Disk-space monitoring.
- Local file backup.
- File organization.
- Security log analysis.
- Interactive menu operation.
- SHA-256 file integrity verification.

**Lesson 14 is complete when you have tested your projects and your final toolkit passes its syntax checks.**
