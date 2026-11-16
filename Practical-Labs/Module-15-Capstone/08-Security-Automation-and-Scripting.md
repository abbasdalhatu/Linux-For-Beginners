
# MODULE 15 – CAPSTONE PROJECT

# LESSON 08: SECURITY AUTOMATION AND SCRIPTING

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain security automation.
2. Understand the role of Bash in cybersecurity.
3. Create scripts for system information collection.
4. Automate log inspection.
5. Automate file integrity checks.
6. Generate security reports.
7. Handle errors in scripts.
8. Apply safe scripting practices.

---

## 2. What Is Security Automation?

Security automation uses scripts and tools to perform repetitive security tasks with minimal manual effort.

Examples include:

- Collecting system information.
- Checking running services.
- Monitoring disk usage.
- Reviewing authentication logs.
- Calculating file hashes.
- Generating reports.
- Checking whether important files have changed.

### Real-World Example

A security administrator manages 50 Linux servers.

Manually checking every server each day would take considerable time.

A script can automate basic checks and produce reports for further review.

Automation improves consistency, but its output must still be reviewed by a human.

---

## 3. Introduction to Bash Automation

Bash is a command-line shell and scripting language commonly available on Linux systems.

A Bash script contains commands that execute in sequence.

Example:

```bash
#!/usr/bin/env bash

echo "Security automation started"
date
hostname
whoami
```

The first line is called the shebang. It identifies the interpreter used to run the script.

---

## 4. Practical Lab A: Create Your Automation Workspace

Create the directory:

```bash
mkdir -p ~/practical-labs/module15/automation
cd ~/practical-labs/module15/automation
```

Confirm your location:

```bash
pwd
```

All scripts in this lesson will be created inside this directory.

---

## 5. Practical Lab B: Create a System Information Script

Create:

```bash
nano system-info.sh
```

Add:

```bash
#!/usr/bin/env bash

set -u

echo "================================="
echo "SYSTEM INFORMATION REPORT"
echo "================================="

echo
echo "Hostname:"
hostname

echo
echo "Operating System:"
cat /etc/os-release | grep PRETTY_NAME

echo
echo "Current User:"
whoami

echo
echo "Date and Time:"
date

echo
echo "System Uptime:"
uptime

echo
echo "IP Addresses:"
ip -br addr

echo
echo "Disk Usage:"
df -h

echo
echo "Memory Usage:"
free -h

echo
echo "System information collection completed."
```

Save the file.

Make it executable:

```bash
chmod +x system-info.sh
```

Run:

```bash
./system-info.sh
```

### Understand the Script

| Command | Purpose |
|---|---|
| `hostname` | Displays system hostname |
| `whoami` | Displays current user |
| `date` | Displays date and time |
| `uptime` | Shows system uptime |
| `ip -br addr` | Displays network addresses |
| `df -h` | Displays disk usage |
| `free -h` | Displays memory usage |

---

## 6. Practical Lab C: Create an Automated Service Checker

Create:

```bash
nano service-check.sh
```

Add:

```bash
#!/usr/bin/env bash

set -u

echo "SERVICE SECURITY CHECK"
echo "======================"

services=("ssh" "cron" "NetworkManager")

for service in "${services[@]}"; do
    echo
    echo "Checking: $service"

    if systemctl is-active --quiet "$service"; then
        echo "$service is ACTIVE"
    else
        echo "$service is NOT ACTIVE"
    fi
done

echo
echo "Service inspection completed."
```

Make it executable:

```bash
chmod +x service-check.sh
```

Run:

```bash
./service-check.sh
```

### Understanding the Logic

The script:

1. Defines a list of services.
2. Loops through each service.
3. Checks whether the service is active.
4. Displays its status.
5. Continues until every service has been checked.

The service names are examples. A service may be absent or inactive by design.

An inactive service is not automatically a security problem.

---

## 7. Practical Lab D: Automate Log Inspection

Create:

```bash
nano log-check.sh
```

Add:

```bash
#!/usr/bin/env bash

set -u

LOG_FILE="/var/log/auth.log"

echo "AUTHENTICATION LOG INSPECTION"
echo "============================="

if [[ -f "$LOG_FILE" ]]; then
    echo "Authentication log found."
    echo
    echo "Recent failed login entries:"
    grep -i "failed" "$LOG_FILE" | tail -n 10
else
    echo "Authentication log not found."
    echo "This system may use a different logging configuration."
    echo "Try inspecting journalctl instead."
fi

echo
echo "Log inspection completed."
```

Make executable:

```bash
chmod +x log-check.sh
```

Run:

```bash
./log-check.sh
```

If permission is denied:

```bash
sudo ./log-check.sh
```

### Alternative Using journalctl

Create:

```bash
nano journal-check.sh
```

Add:

```bash
#!/usr/bin/env bash

set -u

echo "RECENT SYSTEM ERRORS"
echo "===================="

journalctl -p err -n 20 --no-pager

echo
echo "Journal inspection completed."
```

Run:

```bash
chmod +x journal-check.sh
./journal-check.sh
```

---

## 8. Practical Lab E: Automate File Integrity Checking

File integrity monitoring helps identify unexpected changes to important files.

We will use SHA-256.

Create a directory:

```bash
mkdir -p integrity-files
```

Create a sample file:

```bash
echo "Important laboratory configuration" > integrity-files/config.txt
```

Create the hashing script:

```bash
nano integrity-check.sh
```

Add:

```bash
#!/usr/bin/env bash

set -euo pipefail

TARGET_DIR="integrity-files"
HASH_FILE="integrity-baseline.sha256"

if [[ ! -d "$TARGET_DIR" ]]; then
    echo "Error: Target directory does not exist."
    exit 1
fi

echo "Creating SHA-256 integrity baseline..."

find "$TARGET_DIR" -type f -print0 |
    sort -z |
    xargs -0 -r sha256sum > "$HASH_FILE"

echo "Baseline saved to: $HASH_FILE"
```

Make executable:

```bash
chmod +x integrity-check.sh
```

Run:

```bash
./integrity-check.sh
```

Verify:

```bash
sha256sum -c integrity-baseline.sha256
```

Expected result:

```text
integrity-files/config.txt: OK
```

### Test File Modification Detection

Modify the sample:

```bash
echo "Unexpected modification" >> integrity-files/config.txt
```

Verify again:

```bash
sha256sum -c integrity-baseline.sha256
```

The output should indicate that the file no longer matches the baseline.

This demonstrates integrity checking.

Important: Protect the baseline itself. An attacker who can alter both the file and its baseline may conceal changes.

---

## 9. Practical Lab F: Create an Automated Security Report

Create:

```bash
nano security-report.sh
```

Add:

```bash
#!/usr/bin/env bash

set -u

REPORT_DIR="$HOME/practical-labs/module15/automation/reports"

mkdir -p "$REPORT_DIR"

REPORT_FILE="$REPORT_DIR/security-report-$(date +%Y%m%d-%H%M%S).txt"

{
    echo "================================="
    echo "AUTOMATED SECURITY REPORT"
    echo "================================="

    echo
    echo "Collection Time:"
    date -Is

    echo
    echo "Hostname:"
    hostname

    echo
    echo "Current User:"
    whoami

    echo
    echo "IP Addresses:"
    ip -br addr

    echo
    echo "Listening Network Sockets:"
    ss -tuln

    echo
    echo "Disk Usage:"
    df -h

    echo
    echo "Memory Usage:"
    free -h

    echo
    echo "Running Services:"
    systemctl list-units --type=service --state=running --no-pager

    echo
    echo "Report Generation Completed."

} > "$REPORT_FILE"

echo "Report saved to:"
echo "$REPORT_FILE"
```

Make executable:

```bash
chmod +x security-report.sh
```

Run:

```bash
./security-report.sh
```

List the reports:

```bash
ls -lh reports/
```

Open the generated report:

```bash
less reports/$(ls -1t reports/ | head -n 1)
```

---

## 10. Practical Lab G: Create a Main Automation Launcher

Create:

```bash
nano run-security-checks.sh
```

Add:

```bash
#!/usr/bin/env bash

set -u

SCRIPT_DIR="$(cd -- "$(dirname -- "${BASH_SOURCE[0]}")" && pwd)"

echo "================================="
echo "MODULE 15 SECURITY TOOLKIT"
echo "================================="

echo
echo "[1] System Information"

"$SCRIPT_DIR/system-info.sh"

echo
echo "[2] Service Inspection"

"$SCRIPT_DIR/service-check.sh"

echo
echo "[3] Log Inspection"

"$SCRIPT_DIR/log-check.sh"

echo
echo "[4] Security Report"

"$SCRIPT_DIR/security-report.sh"

echo
echo "================================="
echo "AUTOMATION COMPLETED"
echo "================================="
```

Make executable:

```bash
chmod +x run-security-checks.sh
```

Run:

```bash
./run-security-checks.sh
```

This script launches the other scripts in sequence.

Some checks may report missing services or log files. That is expected and should be documented rather than automatically treated as an incident.

---

## 11. Understanding Bash Error Handling

Good automation should handle errors carefully.

Three useful Bash options are:

```bash
set -e
```

Stops execution when certain commands fail.

```bash
set -u
```

Treats references to unset variables as errors.

```bash
set -o pipefail
```

Makes a pipeline report failure when a command within it fails.

A common combination is:

```bash
set -euo pipefail
```

These options improve reliability but do not replace explicit error checking.

### Example

```bash
#!/usr/bin/env bash

set -euo pipefail

FILE="sample.txt"

if [[ -f "$FILE" ]]; then
    echo "File exists."
    cat "$FILE"
else
    echo "File does not exist."
fi
```

---

## 12. Practical Lab H: Document Your Automation Toolkit

Create:

```bash
cat > README.md <<'EOF'
# MODULE 15 SECURITY AUTOMATION TOOLKIT

## Purpose

Automate selected security inspection tasks
inside an authorized training environment.

## Scripts

1. system-info.sh
   Collects basic system information.

2. service-check.sh
   Checks selected system services.

3. log-check.sh
   Inspects authentication log entries.

4. journal-check.sh
   Displays recent system errors.

5. integrity-check.sh
   Creates a SHA-256 integrity baseline.

6. security-report.sh
   Generates a system security report.

7. run-security-checks.sh
   Runs selected scripts in sequence.

## Environment

Kali Linux virtual machine.

## Scope

Local system inspection only.

## Safety

The toolkit does not exploit systems,
modify firewall policies, disable services,
or perform unauthorized network scanning.

## Limitations

Automated output must be reviewed manually.
A detected anomaly is not automatically proof
of malicious activity.
EOF
```

Review:

```bash
cat README.md
```

---

## 13. Practical Lab I: Verify Your Scripts

List your scripts:

```bash
ls -l *.sh
```

Check Bash syntax:

```bash
for script in *.sh; do
    echo "Checking $script"
    bash -n "$script" || exit 1
done
```

Expected outcome:

All scripts pass syntax checking without errors.

Run the main toolkit:

```bash
./run-security-checks.sh
```

Inspect generated reports:

```bash
find reports -type f -maxdepth 1
```

Calculate script hashes:

```bash
sha256sum *.sh > toolkit-hashes.sha256
```

Verify:

```bash
sha256sum -c toolkit-hashes.sha256
```

---

## 14. Safe Automation Principles

Always follow these principles:

1. Use scripts only on authorized systems.
2. Understand every command before executing it.
3. Avoid destructive commands.
4. Do not store passwords or secrets in scripts.
5. Validate input and file paths.
6. Handle errors appropriately.
7. Protect generated reports.
8. Keep backups of important files.
9. Review automation output.
10. Document script purpose and limitations.

---

## 15. Knowledge Check

1. What is security automation?
2. What is Bash?
3. What is the purpose of a shebang?
4. What does `chmod +x` do?
5. What does `systemctl is-active` check?
6. Why is log inspection useful?
7. What is file integrity monitoring?
8. What does SHA-256 provide?
9. Explain the purpose of `set -euo pipefail`.
10. Why should automated findings be reviewed manually?

---

## 16. Practical Completion Checklist

- [ ] I created an automation workspace.
- [ ] I created a system information script.
- [ ] I created a service checker.
- [ ] I created a log inspection script.
- [ ] I created a file integrity checker.
- [ ] I created a security report generator.
- [ ] I created a main automation launcher.
- [ ] I documented the toolkit.
- [ ] I checked Bash syntax.
- [ ] I executed the automation toolkit.
- [ ] I verified script hashes.

---

## 17. Key Takeaway

Security automation makes repetitive administrative and defensive tasks faster, more consistent, and easier to document.

However, automation must be carefully designed, tested, and monitored.

**Remember: Automate repetitive work, but never automate blindly.**
