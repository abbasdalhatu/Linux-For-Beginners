# PRACTICAL LABS
# MODULE 03 – BASH SCRIPTING CHALLENGES

## LAB 07: COMMAND-LINE AUTOMATION

### 1. Lab Overview

**Objective:** Learn how to combine Linux commands and Bash scripts to automate repetitive administrative tasks.

Command-line automation means using scripts and commands to perform tasks automatically instead of executing each operation manually.

For example, an administrator might need to:

- Create project directories.
- Generate reports.
- Organize files.
- Check disk usage.
- Collect system information.
- Create backups.
- Monitor selected resources.

Instead of repeating these operations manually, you can create scripts that perform them consistently.

### 2. Preparing Your Lab

Create your working directory:

```bash
mkdir -p ~/practical-labs/module03/lab07
cd ~/practical-labs/module03/lab07
```

Confirm your location:

```bash
pwd
```

All exercises will take place inside this directory.

### 3. Understanding Command-Line Automation

Consider these commands:

```bash
mkdir project
cd project
touch notes.txt
echo "Linux automation" > notes.txt
cat notes.txt
```

They create a directory, create a file, write text, and display its contents.

A script can combine these commands into one automated operation.

Create:

```bash
nano project-setup.sh
```

Enter:

```bash
#!/bin/bash

mkdir -p project

touch project/notes.txt

echo "Linux automation project" > project/notes.txt

echo "Project setup completed."
```

Execute:

```bash
bash project-setup.sh
```

Inspect the result:

```bash
ls -R project
cat project/notes.txt
```

### 4. Automating Directory Creation

Create:

```bash
nano directory-automation.sh
```

Enter:

```bash
#!/bin/bash

mkdir -p reports
mkdir -p backups
mkdir -p logs
mkdir -p scripts

echo "Required directories created."

echo ""
echo "Directory structure:"
ls -d */
```

Execute:

```bash
bash directory-automation.sh
```

The `mkdir -p` command creates directories without reporting an error when they already exist.

This makes repeated execution safer.

### 5. Automating File Creation

Create:

```bash
nano file-automation.sh
```

Enter:

```bash
#!/bin/bash

for number in {1..5}
do
    touch "report-$number.txt"
done

echo "Five report files created."
```

Execute:

```bash
bash file-automation.sh
```

Inspect:

```bash
ls -l report-*.txt
```

This demonstrates how loops can automate repetitive file creation.

### 6. Automating Report Generation

Administrators often need to collect system information.

Create:

```bash
nano system-report.sh
```

Enter:

```bash
#!/bin/bash

report="system-report.txt"

{
    echo "========== SYSTEM REPORT =========="
    echo ""

    echo "Date:"
    date

    echo ""
    echo "Hostname:"
    hostname

    echo ""
    echo "Current User:"
    whoami

    echo ""
    echo "Kernel:"
    uname -r

    echo ""
    echo "Uptime:"
    uptime

    echo ""
    echo "Memory:"
    free -h

    echo ""
    echo "Disk Usage:"
    df -h

    echo ""
    echo "========== REPORT COMPLETE =========="
} > "$report"

echo "Report generated: $report"
```

Execute:

```bash
bash system-report.sh
```

Read the report:

```bash
cat system-report.txt
```

The `{ ... }` group runs multiple commands together and redirects their output into one report file.

### 7. Automating File Backups

Backups protect important information against accidental deletion or corruption.

For this exercise, you will create a backup of a sample file inside your lab directory.

Create sample data:

```bash
echo "Important cybersecurity training notes." > important-notes.txt
```

Create:

```bash
nano backup.sh
```

Enter:

```bash
#!/bin/bash

source_file="important-notes.txt"
backup_file="important-notes.backup"

if [[ ! -f "$source_file" ]]; then
    echo "Error: Source file not found."
    exit 1
fi

cp -- "$source_file" "$backup_file"

echo "Backup completed successfully."
echo "Backup file: $backup_file"
```

Execute:

```bash
bash backup.sh
```

Verify:

```bash
ls -l important-notes*
```

Compare both files:

```bash
cmp important-notes.txt important-notes.backup
```

If the files are identical, `cmp` produces no output and exits successfully.

**Important:** This is a simple demonstration, not a complete backup strategy. Real backup systems should also consider retention, encryption, permissions, recovery testing, and storage separation.

### 8. Creating Timestamped Backups

A timestamp allows you to distinguish different backups.

Create:

```bash
nano timestamp-backup.sh
```

Enter:

```bash
#!/bin/bash

source_file="important-notes.txt"
backup_directory="backups"

if [[ ! -f "$source_file" ]]; then
    echo "Error: Source file not found."
    exit 1
fi

mkdir -p "$backup_directory"

timestamp=$(date +"%Y-%m-%d_%H-%M-%S")

backup_file="$backup_directory/notes-$timestamp.backup"

cp -- "$source_file" "$backup_file"

echo "Timestamped backup created:"
echo "$backup_file"
```

Execute:

```bash
bash timestamp-backup.sh
```

Inspect:

```bash
ls -l backups/
```

The `date` command creates a timestamp that becomes part of the backup filename.

### 9. Automating Disk Usage Checks

System administrators monitor storage to identify potential capacity problems.

Create:

```bash
nano disk-check.sh
```

Enter:

```bash
#!/bin/bash

echo "========== DISK USAGE REPORT =========="

df -h

echo ""
echo "Lab Directory Usage:"

du -sh ~/practical-labs/module03/lab07

echo ""
echo "======================================="
```

Execute:

```bash
bash disk-check.sh
```

Understanding the commands:

- `df -h` displays filesystem capacity and available space.
- `du -sh` displays the total size of a directory.
- `-h` produces human-readable output.
- `-s` displays a summary rather than every file.

### 10. Automating File Organization

You can use scripts to organize files into folders based on their extensions.

For this exercise, create some sample files:

```bash
touch notes.txt report.txt script.sh data.csv
```

Create:

```bash
nano file-organizer.sh
```

Enter:

```bash
#!/bin/bash

mkdir -p text-files
mkdir -p shell-scripts
mkdir -p csv-files

for file in *.txt
do
    if [[ -f "$file" ]]; then
        mv -- "$file" text-files/
    fi
done

for file in *.sh
do
    if [[ -f "$file" ]]; then
        mv -- "$file" shell-scripts/
    fi
done

for file in *.csv
do
    if [[ -f "$file" ]]; then
        mv -- "$file" csv-files/
    fi
done

echo "File organization completed."
```

Execute:

```bash
bash file-organizer.sh
```

Inspect:

```bash
find . -maxdepth 2 -type f
```

**Safety note:** This script moves matching files. Only run it in this dedicated lab directory, where the sample files were created. Never test file-moving automation on your home directory or system directories.

### 11. Automating a Simple Health Check

A health-check script can collect basic information about a Linux machine.

Create:

```bash
nano health-check.sh
```

Enter:

```bash
#!/bin/bash

echo "========== LINUX HEALTH CHECK =========="

echo ""
echo "Hostname:"
hostname

echo ""
echo "System Uptime:"
uptime -p

echo ""
echo "Memory Information:"
free -h

echo ""
echo "Disk Information:"
df -h /

echo ""
echo "Current Load:"
uptime

echo ""
echo "Health check completed."
```

Execute:

```bash
bash health-check.sh
```

This is an informational health check. It does not modify system settings or automatically diagnose every possible problem.

### 12. Scheduling Automation with cron

Linux provides a scheduling service called `cron`.

It allows users to schedule commands and scripts to run at specified times.

View your current user's cron jobs:

```bash
crontab -l
```

Edit your cron jobs:

```bash
crontab -e
```

A cron schedule uses five time fields:

```text
minute hour day-of-month month day-of-week
```

Example:

```text
0 8 * * * /home/kali/practical-labs/module03/lab07/system-report.sh
```

This schedules the script to run daily at 8:00 AM, assuming the path is correct and the script is executable.

Another example:

```text
*/5 * * * * /home/kali/practical-labs/module03/lab07/health-check.sh
```

This schedules the script every five minutes.

**Important:** These are examples only. Do not install recurring jobs during this exercise. We will first practise running and validating scripts manually.

### 13. Understanding Automation Safety

Automated scripts can perform many operations quickly.

A mistake can therefore affect multiple files or resources.

Follow these principles:

1. Test scripts inside dedicated lab directories.
2. Inspect scripts before execution.
3. Validate file paths.
4. Quote variable expansions.
5. Avoid unnecessary `sudo`.
6. Avoid destructive commands such as unrestricted `rm -rf`.
7. Test scripts with sample files first.
8. Preserve important original data.
9. Check exit statuses.
10. Keep backups of important work.

### 14. Practical Exercise 1 – Project Directory Generator

Create:

```text
project-generator.sh
```

Requirements:

- Create a directory named `cyber-project`.
- Inside it, create `reports`, `logs`, `backups`, and `scripts`.
- Create a `README.txt` file.
- Write a short project description into the file.
- Display the resulting directory structure.

### 15. Practical Exercise 2 – Automated System Report

Create:

```text
automated-report.sh
```

Requirements:

- Collect the current date.
- Collect the hostname.
- Collect the current username.
- Collect the kernel version.
- Collect memory information.
- Collect disk usage.
- Save everything in `automated-report.txt`.

### 16. Practical Exercise 3 – Backup Automation

Create:

```text
backup-automation.sh
```

Requirements:

1. Create a sample file named `project-data.txt`.
2. Create a `backup-storage` directory.
3. Check that the source file exists.
4. Generate a timestamp.
5. Copy the file into the backup directory.
6. Display the backup location.

Do not modify or delete the original file.

### 17. Practical Exercise 4 – File Organizer

Create:

```text
automatic-organizer.sh
```

Requirements:

- Create sample `.txt`, `.sh`, and `.csv` files.
- Create separate destination folders.
- Move each sample file into the matching folder.
- Display the resulting directory structure.

Only organize the sample files you created in the lab directory.

### 18. Practical Exercise 5 – Linux Health Monitor

Create:

```text
linux-health-monitor.sh
```

Requirements:

Display:

- Hostname.
- Current uptime.
- Memory usage.
- Root filesystem usage.
- Current date and time.

Save the results into:

```text
health-report.txt
```

The script should display a confirmation after successfully creating the report.

### 19. Script Validation

Check the syntax of your scripts:

```bash
bash -n project-setup.sh
bash -n directory-automation.sh
bash -n system-report.sh
bash -n backup.sh
bash -n timestamp-backup.sh
bash -n disk-check.sh
bash -n file-organizer.sh
bash -n health-check.sh
```

Inspect your files:

```bash
find . -maxdepth 2 -type f
```

### 20. Cybersecurity Application

Command-line automation is important in security operations.

Examples include:

**Security log collection**

```bash
#!/bin/bash

echo "Collecting sample security logs..."

if [[ -f "application.log" ]]; then
    grep -i "failed" application.log > failed-events.txt
    echo "Security event report generated."
else
    echo "Sample log file not found."
fi
```

**File permission inspection**

```bash
#!/bin/bash

for file in *.sh
do
    if [[ -f "$file" ]]; then
        ls -l -- "$file"
    fi
done
```

**System information collection**

```bash
#!/bin/bash

echo "Hostname: $(hostname)"
echo "Kernel: $(uname -r)"
echo "User: $(whoami)"
echo "Uptime: $(uptime -p)"
```

These examples demonstrate defensive administration and local inspection. They do not scan external systems or modify security settings.

### 21. Knowledge Check

1. What is command-line automation?
2. Why is automation useful in Linux administration?
3. What does `mkdir -p` do?
4. What is the purpose of `cp`?
5. Why are timestamps useful in backup filenames?
6. What does `df -h` display?
7. What does `du -sh` display?
8. What is the purpose of the cron service?
9. What are the five cron schedule fields?
10. Why should scripts be tested in a dedicated directory?
11. Why should automated backup scripts preserve original files?
12. What risks can poorly written automation introduce?

### 22. Lab Completion Checklist

- [ ] Created the Lab 07 directory.
- [ ] Automated directory creation.
- [ ] Automated file creation.
- [ ] Generated system reports.
- [ ] Created sample backups.
- [ ] Created timestamped backups.
- [ ] Checked disk usage.
- [ ] Organized sample files.
- [ ] Created a system health check.
- [ ] Studied cron scheduling.
- [ ] Completed all five practical exercises.
- [ ] Validated Bash syntax.

### 23. Expected Outcome

By completing this lab, you should understand how to:

- Automate repetitive Linux commands.
- Create directory structures.
- Generate system reports.
- Perform basic file backups.
- Organize files using scripts.
- Monitor basic system information.
- Understand scheduled automation.
- Apply safe scripting practices.

**Next Lab:** Error Handling and Debugging.