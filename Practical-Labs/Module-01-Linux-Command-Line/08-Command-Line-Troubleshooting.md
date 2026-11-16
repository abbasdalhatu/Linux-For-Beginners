
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 08: COMMAND-LINE TROUBLESHOOTING

---

## 1. Lab Objectives

By the end of this lab, you should be able to:

1. Diagnose common Linux command errors.
2. Troubleshoot file and directory problems.
3. Investigate permission-denied errors.
4. Diagnose command-not-found errors.
5. Troubleshoot storage and memory issues.
6. Investigate process problems.
7. Diagnose basic network connectivity problems.
8. Inspect Linux logs for errors.
9. Use a systematic troubleshooting methodology.
10. Build a Linux troubleshooting report.

## 2. Prepare Your Lab Environment

```bash
mkdir -p ~/practical-labs/module01/lab08
cd ~/practical-labs/module01/lab08
```

Verify:

```bash
pwd
```

---

## 3. Exercise 1: Understand the Troubleshooting Process

Troubleshooting is the process of identifying, investigating, and resolving technical problems.

A professional Linux administrator should avoid guessing.

Use this methodology:

1. Identify the problem.
2. Collect information.
3. Reproduce the problem safely.
4. Investigate possible causes.
5. Test a solution.
6. Verify that the problem is resolved.
7. Document the findings.

### Example

Problem: A command returns `command not found`.

Investigation:

```bash
command -v command_name
```

Check PATH:

```bash
echo "$PATH"
```

Check whether a package provides the command:

```bash
apt-cache search package_name
```

The objective is to identify the actual cause rather than immediately installing random software.

---

## 4. Exercise 2: Troubleshoot Command-Not-Found Errors

Run an intentionally invalid command:

```bash
linuxcommand123
```

Expected result:

```text
command not found
```

### Step 1: Check whether a command exists

```bash
command -v linuxcommand123
```

### Step 2: Inspect PATH

```bash
echo "$PATH"
```

### Step 3: Inspect command availability

For a real command, use:

```bash
command -v ls
```

You can also inspect its location:

```bash
which ls
```

### Step 4: Check installed packages

For example:

```bash
dpkg -l | grep curl
```

Check whether `curl` is available:

```bash
command -v curl
```

If it is missing, inspect the package information:

```bash
apt-cache policy curl
```

Only install software when necessary and from trusted Kali repositories.

---

## 5. Exercise 3: Troubleshoot File-Not-Found Errors

Create a sample file:

```bash
echo "Linux troubleshooting practice" > sample.txt
```

Read it:

```bash
cat sample.txt
```

Now try:

```bash
cat missing.txt
```

Expected result: an error indicating that the file does not exist.

### Step 1: Check the current directory

```bash
pwd
```

### Step 2: List files

```bash
ls -lah
```

### Step 3: Search for the file

```bash
find ~/practical-labs/module01/lab08 -name "sample.txt"
```

### Step 4: Check the exact filename

Linux filenames are case-sensitive.

For example:

```text
Sample.txt
sample.txt
SAMPLE.txt
```

These are three different filenames.

### Step 5: Verify the file type

```bash
file sample.txt
```

---

## 6. Exercise 4: Troubleshoot Permission-Denied Errors

Linux uses file permissions to control access.

Create a test file:

```bash
echo "Permission troubleshooting" > restricted.txt
```

Remove read permission from the file's owner:

```bash
chmod u-r restricted.txt
```

Try reading it:

```bash
cat restricted.txt
```

Depending on your account and permissions, you should receive a permission-denied error.

### Step 1: Inspect permissions

```bash
ls -l restricted.txt
```

### Step 2: Inspect ownership

```bash
stat restricted.txt
```

### Step 3: Restore the permission

```bash
chmod u+r restricted.txt
```

### Step 4: Test again

```bash
cat restricted.txt
```

You should now be able to read the file.

### Understand the permission symbols

| Symbol | Meaning |
|---|---|
| `r` | Read |
| `w` | Write |
| `x` | Execute |
| `-` | Permission not granted |

**Security lesson:** Permission errors are not always problems to bypass. They may be intentional security controls.

Do not solve every permission error using `sudo` or broad permissions such as `chmod 777`.

---

## 7. Exercise 5: Troubleshoot Directory Problems

Create a directory:

```bash
mkdir test-directory
```

Enter it:

```bash
cd test-directory
```

Try removing it from inside:

```bash
rmdir test-directory
```

You should receive an error because the directory is not in the current directory.

Return to the parent:

```bash
cd ..
```

Inspect:

```bash
ls -la
```

Remove the empty directory:

```bash
rmdir test-directory
```

Verify:

```bash
ls -la
```

### Common directory troubleshooting commands

```bash
pwd
ls -la
find .
stat directory_name
```

Always verify your current location before performing destructive operations.

---

## 8. Exercise 6: Troubleshoot Disk Space

Inspect filesystem usage:

```bash
df -h
```

Inspect your lab directory:

```bash
du -sh ~/practical-labs
```

Find the largest files in the current lab:

```bash
find . -type f -printf '%s %p\n' | sort -nr | head -n 10
```

Inspect storage devices:

```bash
lsblk -f
```

### Understand common disk problems

| Symptom | Possible cause |
|---|---|
| Cannot create files | Filesystem may be full |
| Applications fail to save | Insufficient available storage |
| System becomes slow | Storage pressure or heavy I/O |
| Package installation fails | Insufficient space in relevant filesystem |

### Safe troubleshooting

If storage appears full:

1. Identify the affected filesystem using `df -h`.
2. Identify large directories using `du`.
3. Determine which files are unnecessary.
4. Back up important information.
5. Remove only files you understand and are authorized to remove.
6. Recheck available space.

Do not blindly delete files from `/var`, `/usr`, `/etc`, or other system directories.

---

## 9. Exercise 7: Troubleshoot Memory Problems

Inspect memory:

```bash
free -h
```

Inspect processes using the most memory:

```bash
ps aux --sort=-%mem | head
```

Inspect CPU and memory activity:

```bash
top
```

Look for processes consuming unexpectedly large amounts of resources.

### Inspect out-of-memory messages

```bash
journalctl -k | grep -i "out of memory"
```

You can also try:

```bash
dmesg | grep -i "out of memory"
```

Access may require administrative privileges.

### Important concept

Linux may use available memory for caching. High memory usage alone does not prove a memory leak.

Investigate the available memory, process behavior, swap usage, and relevant logs together.

---

## 10. Exercise 8: Troubleshoot Process Problems

Inspect running processes:

```bash
ps aux
```

Find a process:

```bash
pgrep -a sleep
```

Inspect CPU usage:

```bash
ps aux --sort=-%cpu | head
```

### Create a controlled test process

```bash
sleep 300 &
```

Inspect it:

```bash
pgrep -a sleep
```

Record its PID.

Inspect its state:

```bash
ps -o pid,ppid,stat,comm -p PID
```

Replace `PID` with the actual number.

Stop your test process:

```bash
kill -TERM PID
```

Verify:

```bash
pgrep -a sleep
```

If multiple sleep processes exist, make sure you identify the PID belonging to your own test process before sending a signal.

---

## 11. Exercise 9: Troubleshoot Network Connectivity

Network problems can have several causes:

- Interface is disconnected.
- IP address is missing.
- Default route is missing.
- DNS resolution is failing.
- Remote host is unavailable.
- Firewall rules are blocking traffic.

### Step 1: Inspect network interfaces

```bash
ip -brief addr
```

### Step 2: Inspect routing

```bash
ip route
```

### Step 3: Inspect DNS configuration

```bash
cat /etc/resolv.conf
```

### Step 4: Test local network configuration

```bash
ip link
```

### Step 5: Test DNS resolution

```bash
getent hosts debian.org
```

### Step 6: Test connectivity to a hostname

```bash
ping -c 4 debian.org
```

### Step 7: Test connectivity to a known public IP

```bash
ping -c 4 1.1.1.1
```

These tests help distinguish name-resolution problems from other connectivity problems, but ping responses may be blocked by network policy.

### Interpret the results

| Observation | Possible issue |
|---|---|
| Interface is down | Interface or virtual network issue |
| No IP address | DHCP or interface configuration issue |
| No default route | Routing configuration issue |
| IP works but hostname fails | Possible DNS issue |
| Hostname resolves but ping fails | ICMP may be blocked or host unavailable |

Only test external destinations for ordinary connectivity diagnostics. Do not scan or probe systems you do not own or have permission to test.

---

## 12. Exercise 10: Troubleshoot DNS Resolution

DNS translates domain names into IP addresses.

Check whether the DNS lookup utility exists:

```bash
command -v dig
```

If available:

```bash
dig debian.org
```

Try:

```bash
nslookup debian.org
```

You can also use:

```bash
getent ahosts debian.org
```

### Compare hostname and IP connectivity

First:

```bash
ping -c 2 debian.org
```

Then:

```bash
ping -c 2 1.1.1.1
```

If IP connectivity works but hostname resolution fails, DNS is one possible cause.

Do not automatically change `/etc/resolv.conf`. First inspect how DNS is managed on your system.

---

## 13. Exercise 11: Troubleshoot Package Manager Problems

Kali Linux uses APT for package management.

Inspect package information:

```bash
apt-cache policy curl
```

Check installed package status:

```bash
dpkg -l curl
```

Inspect configured repositories:

```bash
cat /etc/apt/sources.list
```

Kali may also use repository files under:

```text
/etc/apt/sources.list.d/
```

Update package metadata:

```bash
sudo apt update
```

If an update fails, inspect the actual error.

Common causes include:

- No internet connection.
- DNS resolution failure.
- Incorrect repository configuration.
- Repository server unavailable.
- Incorrect system time.
- Temporary package mirror problems.

Do not randomly add third-party repositories or disable package-signature verification to fix an installation problem.

---

## 14. Exercise 12: Inspect System Logs for Troubleshooting

Check recent system messages:

```bash
journalctl -n 50
```

Inspect current boot errors:

```bash
journalctl -b -p err
```

Inspect recent kernel messages:

```bash
dmesg | tail -n 40
```

Search for failed services:

```bash
systemctl --failed
```

Inspect a service:

```bash
systemctl status ssh
```

The SSH service may not exist on your system.

### Troubleshooting principle

Logs provide evidence about what happened.

A good investigation records:

- Time of the event.
- Error message.
- Affected component.
- Relevant process or service.
- Actions taken.
- Result after testing.

---

## 15. Exercise 13: Practice a Simulated Troubleshooting Investigation

Create a simulated application log:

```bash
cat > simulated-errors.log <<'EOF'
10:00 INFO Application started
10:01 ERROR Configuration file missing
10:02 WARNING Memory usage elevated
10:03 ERROR Database connection failed
10:04 INFO User session created
10:05 ERROR Database connection failed
10:06 WARNING Disk usage elevated
10:07 INFO Application stopped
EOF
```

### Task A: Display all errors

```bash
grep "ERROR" simulated-errors.log
```

### Task B: Count errors

```bash
grep -c "ERROR" simulated-errors.log
```

### Task C: Display warnings

```bash
grep "WARNING" simulated-errors.log
```

### Task D: Count message categories

```bash
awk '{print $2}' simulated-errors.log | sort | uniq -c
```

### Task E: Save an investigation report

```bash
{
    echo "SIMULATED APPLICATION INVESTIGATION"
    echo "Date: $(date)"
    echo
    echo "ERROR COUNT:"
    grep -c "ERROR" simulated-errors.log
    echo
    echo "ERROR DETAILS:"
    grep "ERROR" simulated-errors.log
    echo
    echo "WARNING DETAILS:"
    grep "WARNING" simulated-errors.log
} | tee troubleshooting-report.txt
```

Inspect:

```bash
cat troubleshooting-report.txt
```

This is a simulated investigation, not evidence of an actual system incident.

---

## 16. Exercise 14: Build a Linux Troubleshooting Script

Create:

```bash
nano troubleshooting-check.sh
```

Enter:

```bash
#!/bin/bash

REPORT="troubleshooting-check.txt"

{
    echo "===================================="
    echo "      LINUX TROUBLESHOOTING CHECK"
    echo "===================================="

    echo
    echo "DATE:"
    date

    echo
    echo "HOSTNAME:"
    hostname

    echo
    echo "OPERATING SYSTEM:"
    grep PRETTY_NAME /etc/os-release

    echo
    echo "KERNEL:"
    uname -r

    echo
    echo "MEMORY:"
    free -h

    echo
    echo "DISK SPACE:"
    df -h

    echo
    echo "SYSTEM UPTIME:"
    uptime

    echo
    echo "NETWORK INTERFACES:"
    ip -brief addr

    echo
    echo "ROUTING TABLE:"
    ip route

    echo
    echo "FAILED SERVICES:"
    systemctl --failed

    echo
    echo "RECENT KERNEL MESSAGES:"
    dmesg | tail -n 10

    echo
    echo "===================================="
    echo "       CHECK COMPLETED"
    echo "===================================="

} 2>&1 | tee "$REPORT"

echo
echo "Report saved to: $REPORT"
```

Make it executable:

```bash
chmod +x troubleshooting-check.sh
```

Run:

```bash
./troubleshooting-check.sh
```

Inspect:

```bash
less troubleshooting-check.txt
```

Some commands may produce permission warnings. Record them rather than immediately changing security settings.

---

## 17. Practical Challenge

Perform these troubleshooting exercises:

1. Investigate a command-not-found error.
2. Investigate a missing-file error.
3. Diagnose a permission-denied error using a test file.
4. Inspect disk usage.
5. Inspect memory usage.
6. Identify a running test process.
7. Inspect your IP address and routing table.
8. Test DNS resolution.
9. Inspect recent system errors.
10. Analyze the simulated application log.
11. Generate your troubleshooting report.

For each investigation, record:

- Problem.
- Evidence collected.
- Possible cause.
- Action taken.
- Verification result.

---

## 18. Knowledge Check

Answer these questions in your notebook:

1. What is Linux troubleshooting?
2. Why should you collect evidence before changing system settings?
3. What causes a command-not-found error?
4. What causes a permission-denied error?
5. How can you investigate a missing file?
6. What is the difference between `df` and `du`?
7. How can you inspect memory usage?
8. What is the purpose of `journalctl`?
9. What does `systemctl --failed` display?
10. What is the purpose of `ip route`?
11. How can you distinguish a DNS problem from an IP connectivity problem?
12. Why should you avoid blindly deleting system files?
13. Why should you avoid using `chmod 777` as a general troubleshooting solution?
14. What information should be included in a troubleshooting report?

---

## 19. Lab Completion Checklist

- [ ] Investigated command-not-found errors.
- [ ] Investigated missing-file errors.
- [ ] Practiced permission troubleshooting.
- [ ] Investigated directory problems.
- [ ] Inspected disk usage.
- [ ] Inspected memory usage.
- [ ] Investigated process problems.
- [ ] Practiced network diagnostics.
- [ ] Investigated DNS resolution.
- [ ] Inspected package manager configuration.
- [ ] Examined system logs.
- [ ] Analyzed simulated application errors.
- [ ] Built the troubleshooting script.
- [ ] Completed the practical challenge.

**END OF LAB 08**
