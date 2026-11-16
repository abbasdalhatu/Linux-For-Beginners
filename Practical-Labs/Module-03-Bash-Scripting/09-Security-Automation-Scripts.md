# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 09: SECURITY AUTOMATION SCRIPTS

### 1. Lab Overview

**Objective:** Learn how Bash can automate basic defensive cybersecurity tasks.

Security professionals regularly perform repetitive activities such as:

- Inspecting file permissions.
- Checking running processes.
- Reviewing authentication logs.
- Identifying failed login events.
- Monitoring disk usage.
- Generating security reports.
- Checking file integrity.
- Recording system information.

Bash can automate these tasks and help administrators collect useful security evidence.

In this lab, you will build small defensive security tools that operate only on your Kali Linux VM and sample files.

**Safety boundary:** All practical activities are local and defensive. Do not scan external networks, attempt unauthorized access, or modify authentication and security configurations.

### 2. Prepare Your Lab Environment

```bash
mkdir -p ~/practical-labs/module03/lab09
cd ~/practical-labs/module03/lab09
```

Create a sample directory:

```bash
mkdir -p sample-data
```

Create a fictional security log:

```bash
cat > sample-data/security.log <<'EOF'
2026-11-15 08:00:00 INFO User admin logged in
2026-11-15 08:05:00 WARNING Failed login for user test
2026-11-15 08:06:00 WARNING Failed login for user guest
2026-11-15 08:10:00 INFO User student logged in
2026-11-15 08:15:00 ERROR Unauthorized access attempt detected
2026-11-15 08:20:00 WARNING Failed login for user admin
2026-11-15 08:25:00 INFO System backup completed
EOF
```

Inspect the file:

```bash
cat sample-data/security.log
```

These are fictional training records, not actual authentication events.

### 3. Security Automation Fundamentals

Security automation uses scripts and tools to perform security-related tasks consistently.

Consider this example:

```bash
#!/bin/bash

echo "Security inspection started."

hostname
whoami
date

echo "Security inspection completed."
```

This script collects basic information about the local machine.

Security automation is useful because it:

1. Reduces repetitive manual work.
2. Improves consistency.
3. Helps collect evidence.
4. Supports regular monitoring.
5. Makes reporting easier.

However, automation does not replace human analysis. A script can identify suspicious indicators, but an analyst must interpret the results.

### 4. Automating File Permission Auditing

Linux permissions control who can read, write, and execute files.

An incorrectly configured file may expose information or allow unauthorized modification.

Create a sample file:

```bash
echo "Sample security configuration" > sample-data/config.txt
```

Inspect its permissions:

```bash
ls -l sample-data/config.txt
```

Create:

```bash
nano permission-audit.sh
```

Enter:

```bash
#!/bin/bash

target="sample-data/config.txt"

if [[ ! -f "$target" ]]; then
    echo "ERROR: Target file does not exist."
    exit 1
fi

echo "========== PERMISSION AUDIT =========="

ls -l "$target"

if [[ -r "$target" ]]; then
    echo "Readable by current user: YES"
else
    echo "Readable by current user: NO"
fi

if [[ -w "$target" ]]; then
    echo "Writable by current user: YES"
else
    echo "Writable by current user: NO"
fi

if [[ -x "$target" ]]; then
    echo "Executable by current user: YES"
else
    echo "Executable by current user: NO"
fi

echo "======================================"
```

Execute:

```bash
bash permission-audit.sh
```

Understanding:

- `-f` checks whether a regular file exists.
- `-r` checks readability.
- `-w` checks writability.
- `-x` checks executability.
- `ls -l` displays permission information.

These checks describe access available to the current user. They do not independently prove that a file is secure against every user or process.

### 5. Automating Security Log Analysis

Security logs can contain information about authentication failures, errors, and unusual activity.

Create:

```bash
nano log-analyzer.sh
```

Enter:

```bash
#!/bin/bash

log_file="sample-data/security.log"
report="failed-login-report.txt"

if [[ ! -f "$log_file" ]]; then
    echo "ERROR: Security log not found."
    exit 1
fi

echo "Analyzing fictional security events..."

grep -i "failed login" "$log_file" > "$report" || true

echo ""
echo "Failed login events:"
cat "$report"

echo ""
echo "Number of failed login events:"

wc -l < "$report"

echo "Analysis completed."
```

Execute:

```bash
bash log-analyzer.sh
```

Inspect the generated report:

```bash
cat failed-login-report.txt
```

The `|| true` prevents `grep` finding no matches from terminating the script if it is run with strict error handling. In production scripts, distinguish between "no matches" and genuine errors more carefully.

### 6. Detecting Repeated Failed Login Events

Repeated failed login attempts may indicate:

- Incorrect password entry.
- A forgotten password.
- Automated password guessing.
- Misconfigured applications.
- Unauthorized access attempts.

Create:

```bash
nano failed-login-counter.sh
```

Enter:

```bash
#!/bin/bash

log_file="sample-data/security.log"

if [[ ! -f "$log_file" ]]; then
    echo "ERROR: Log file not found."
    exit 1
fi

count=$(grep -ic "failed login" "$log_file" || true)

echo "========== LOGIN SUMMARY =========="
echo "Failed login events: $count"

if (( count >= 3 )); then
    echo "NOTICE: Multiple failed login events detected."
else
    echo "No repeated failure threshold reached."
fi

echo "==================================="
```

Execute:

```bash
bash failed-login-counter.sh
```

Expected result: three failed login events.

**Important:** Three failed events in a fictional log do not establish an attack. In real investigations, analysts consider timestamps, source addresses, account context, normal activity, and other evidence.

### 7. Automating Process Inspection

Running processes can provide useful information about system activity.

Create:

```bash
nano process-audit.sh
```

Enter:

```bash
#!/bin/bash

report="process-report.txt"

echo "========== PROCESS AUDIT ==========" > "$report"

echo "" >> "$report"

echo "Current User:" >> "$report"
whoami >> "$report"

echo "" >> "$report"

echo "Running Processes:" >> "$report"
ps aux >> "$report"

echo "" >> "$report"

echo "Process Audit Completed." >> "$report"

echo "Report generated: $report"
```

Execute:

```bash
bash process-audit.sh
```

Inspect:

```bash
head -n 20 process-report.txt
```

Understanding:

- `ps aux` displays running processes.
- `head` displays the first lines.
- `>>` appends output to a file.
- The report helps preserve a basic snapshot of process information.

Do not automatically terminate unfamiliar processes based only on their names.

### 8. Automating File Integrity Checks

File integrity monitoring helps identify whether a file has changed.

A cryptographic hash can represent the contents of a file.

For this exercise, use SHA-256.

Create sample data:

```bash
echo "Original training configuration" > sample-data/integrity-test.txt
```

Generate a hash:

```bash
sha256sum sample-data/integrity-test.txt
```

Create:

```bash
nano integrity-check.sh
```

Enter:

```bash
#!/bin/bash

target="sample-data/integrity-test.txt"
baseline="baseline.sha256"

if [[ ! -f "$target" ]]; then
    echo "ERROR: Target file not found."
    exit 1
fi

if [[ ! -f "$baseline" ]]; then
    sha256sum "$target" > "$baseline"
    echo "Baseline hash created."
    exit 0
fi

if sha256sum -c "$baseline"; then
    echo "File integrity verified."
else
    echo "WARNING: File integrity check failed."
    exit 1
fi
```

Run it for the first time:

```bash
bash integrity-check.sh
```

The first execution creates a baseline.

Run it again:

```bash
bash integrity-check.sh
```

Now modify the file:

```bash
echo "Modified configuration" >> sample-data/integrity-test.txt
```

Run the integrity check again:

```bash
bash integrity-check.sh
```

The check should fail because the file's contents have changed.

**Security lesson:** A hash detects changes relative to a trusted baseline. If an attacker can modify both the file and its baseline, the comparison can be defeated. Store important baselines securely.

### 9. Automating System Information Collection

Security administrators often collect basic host information when investigating a machine.

Create:

```bash
nano security-inventory.sh
```

Enter:

```bash
#!/bin/bash

report="security-inventory.txt"

{
    echo "========== SECURITY INVENTORY =========="

    echo ""
    echo "Collection Date:"
    date

    echo ""
    echo "Hostname:"
    hostname

    echo ""
    echo "Current User:"
    whoami

    echo ""
    echo "Kernel Information:"
    uname -a

    echo ""
    echo "Uptime:"
    uptime

    echo ""
    echo "Disk Usage:"
    df -h

    echo ""
    echo "Memory Usage:"
    free -h

    echo ""
    echo "========================================="
} > "$report"

echo "Security inventory generated."
```

Execute:

```bash
bash security-inventory.sh
```

Inspect:

```bash
cat security-inventory.txt
```

### 10. Creating a Simple Security Event Report

Combine multiple security checks into one report.

Create:

```bash
nano security-report.sh
```

Enter:

```bash
#!/bin/bash

set -u

report="security-summary.txt"
log_file="sample-data/security.log"
config_file="sample-data/config.txt"

{
    echo "========== SECURITY SUMMARY =========="
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo "User: $(whoami)"

    echo ""
    echo "----- Failed Login Events -----"

    if [[ -f "$log_file" ]]; then
        grep -i "failed login" "$log_file" || true
    else
        echo "Log file unavailable."
    fi

    echo ""
    echo "----- File Permission Inspection -----"

    if [[ -f "$config_file" ]]; then
        ls -l "$config_file"
    else
        echo "Configuration file unavailable."
    fi

    echo ""
    echo "----- Disk Usage -----"
    df -h /

    echo ""
    echo "========== END OF REPORT =========="
} > "$report"

echo "Security report created: $report"
```

Execute:

```bash
bash security-report.sh
```

Inspect:

```bash
cat security-summary.txt
```

This is a simple local security report, not a full security assessment.

### 11. Building a Basic Security Monitoring Script

Create:

```bash
nano local-security-monitor.sh
```

Enter:

```bash
#!/bin/bash

set -u

echo "========== LOCAL SECURITY MONITOR =========="

echo ""
echo "[1] Host Information"
hostname
uname -r

echo ""
echo "[2] Current User"
whoami

echo ""
echo "[3] File Permission Check"

target="sample-data/config.txt"

if [[ -f "$target" ]]; then
    ls -l "$target"
else
    echo "Configuration file missing."
fi

echo ""
echo "[4] Failed Login Events"

log_file="sample-data/security.log"

if [[ -f "$log_file" ]]; then
    grep -ic "failed login" "$log_file" || true
else
    echo "Sample log unavailable."
fi

echo ""
echo "[5] Disk Usage"
df -h /

echo ""
echo "========== MONITOR COMPLETED =========="
```

Execute:

```bash
bash local-security-monitor.sh
```

This script collects local defensive information. It does not continuously monitor the machine or replace a dedicated security monitoring system.

### 12. Practical Exercise 1 – Permission Auditor

Create:

`permission-auditor.sh`

Requirements:

1. Inspect a sample file.
2. Confirm whether it exists.
3. Display its permissions.
4. Check readability.
5. Check writability.
6. Check executability.
7. Display a summary.

### 13. Practical Exercise 2 – Log Investigation Tool

Create:

`security-log-investigator.sh`

Requirements:

1. Read `sample-data/security.log`.
2. Count failed login events.
3. Display all warning events.
4. Display all error events.
5. Save findings to `investigation-report.txt`.
6. Display a completion message.

Use:

```bash
grep
wc
```

### 14. Practical Exercise 3 – File Integrity Monitor

Create:

`file-integrity-monitor.sh`

Requirements:

1. Use SHA-256.
2. Create a baseline on the first execution.
3. Verify the file on subsequent executions.
4. Report whether the file changed.
5. Return a non-zero status if verification fails.

Keep both the sample file and baseline inside your lab directory.

### 15. Practical Exercise 4 – System Security Inventory

Create:

`system-security-inventory.sh`

Requirements:

Collect:

- Hostname.
- Current username.
- Kernel version.
- System uptime.
- Memory usage.
- Disk usage.
- Running process information.

Save the results in:

`system-inventory.txt`

### 16. Practical Exercise 5 – Integrated Security Dashboard

Create:

`security-dashboard.sh`

Requirements:

Build one script that:

1. Displays host information.
2. Displays the current username.
3. Checks sample file permissions.
4. Counts failed login events in the fictional log.
5. Checks file integrity.
6. Displays disk usage.
7. Saves the results into `security-dashboard-report.txt`.

Use functions to organize your code.

Add basic error handling and ensure all file paths stay inside the lab directory.

### 17. Script Validation

Check your scripts:

```bash
for file in *.sh
do
    echo "Checking $file"
    bash -n "$file"
done
```

Inspect your directory:

```bash
find . -maxdepth 3 -type f
```

Review your reports:

```bash
ls -lh *.txt
```

### 18. Knowledge Check

1. What is security automation?
2. Why are file permissions important?
3. What information can Linux security logs contain?
4. What might repeated failed login events indicate?
5. What does `ps aux` display?
6. What is a cryptographic hash?
7. What is SHA-256?
8. Why is a trusted hash baseline important?
9. What is the difference between security monitoring and security auditing?
10. Why should suspicious processes not be terminated without investigation?
11. Why should security reports contain timestamps?
12. Why should automation scripts avoid exposing sensitive information?

### 19. Lab Completion Checklist

- [ ] Created the Lab 09 directory.
- [ ] Created fictional security logs.
- [ ] Audited sample file permissions.
- [ ] Analysed failed login events.
- [ ] Inspected running processes.
- [ ] Created SHA-256 integrity checks.
- [ ] Collected system information.
- [ ] Generated a security summary.
- [ ] Built a local security monitor.
- [ ] Completed all five practical exercises.
- [ ] Validated Bash syntax.

### 20. Expected Outcome

By completing this lab, you should be able to:

- Automate basic defensive security checks.
- Inspect Linux file permissions.
- Analyse sample security logs.
- Count failed login events.
- Collect process information.
- Use SHA-256 for integrity verification.
- Generate security reports.
- Build simple local monitoring scripts.
- Apply safe scripting and evidence-handling practices.

**Next Lab:** Module 03 – Practical Assessment.