# MODULE 04 – LINUX KERNEL AND SHELL
## Final Module Assessment

**Course:** Linux for Ethical Hackers  
**Module:** 04 – Linux Kernel and Shell  
**Total Marks:** 100  
**Pass Mark:** 50/100  
**Assessment Type:** Theory and Practical

---

## 1. Assessment Instructions

1. Attempt every section.
2. Use your Kali Linux VMware virtual machine for practical exercises.
3. Read each question carefully before answering.
4. Write your answers in your own words.
5. Do not run commands you do not understand.
6. Perform all practical activities only inside your own virtual machine.
7. Save your practical Bash script in a separate file named `module04-assessment.sh`.

**Important:** This assessment tests your understanding, not your ability to copy commands.

---

# SECTION A – MULTIPLE-CHOICE QUESTIONS
**20 Marks – 1 Mark Each**

Choose the correct answer.

**1. What is the Linux kernel?**

A. A graphical desktop environment  
B. The core component of the operating system  
C. A text editor  
D. A command-line application

**2. Which component manages communication between hardware and software?**

A. File manager  
B. Shell  
C. Kernel  
D. Terminal emulator

**3. What is kernel space?**

A. An area where ordinary users store documents  
B. A protected memory area where the kernel operates  
C. A folder containing user files  
D. A temporary directory

**4. Which of the following normally operates in user space?**

A. Bash shell  
B. Kernel scheduler  
C. Kernel memory manager  
D. Kernel device driver

**5. What is a system call?**

A. A telephone service  
B. A request from a user-space program for a kernel service  
C. A Linux installation command  
D. A shell alias

**6. Which command displays the running kernel version?**

A. `whoami`  
B. `uname -r`  
C. `pwd`  
D. `history`

**7. What does `lsmod` display?**

A. Installed applications  
B. Loaded kernel modules  
C. User accounts  
D. Network connections

**8. Which command displays information about a kernel module?**

A. `modinfo`  
B. `mkdir`  
C. `chmod`  
D. `touch`

**9. What is the first major stage of the traditional Linux boot process?**

A. Bash  
B. Firmware initialization  
C. User login  
D. Desktop loading

**10. What is the primary role of GRUB?**

A. Manage file permissions  
B. Load and start the selected operating system kernel  
C. Manage network interfaces  
D. Edit shell scripts

**11. What is systemd?**

A. A text editor  
B. A package manager  
C. A system and service manager  
D. A programming language

**12. Which command displays the status of a service?**

A. `systemctl status`  
B. `service-list`  
C. `show-service`  
D. `statusctl`

**13. What is Bash?**

A. A Linux filesystem  
B. A command-line shell and scripting language  
C. A hardware component  
D. A network protocol

**14. Which command displays the current working directory?**

A. `ls`  
B. `cd`  
C. `pwd`  
D. `who`

**15. Which command makes a variable available to child processes?**

A. `unset`  
B. `export`  
C. `remove`  
D. `delete`

**16. What does `$?` represent?**

A. Current username  
B. Previous command's exit status  
C. Current directory  
D. Process ID

**17. Which symbol is used for command substitution?**

A. `$( )`  
B. `{ }`  
C. `[ ]`  
D. `&&`

**18. Which file commonly contains interactive Bash configuration for a user?**

A. `/etc/passwd`  
B. `~/.bashrc`  
C. `/etc/shadow`  
D. `/boot/grub`

**19. What does the pipe operator `|` do?**

A. Deletes a file  
B. Sends one command's standard output to another command's standard input  
C. Changes file permissions  
D. Starts a new shell

**20. Which file descriptor represents standard error?**

A. 0  
B. 1  
C. 2  
D. 3

---

# SECTION B – SHORT-ANSWER QUESTIONS
**20 Marks – 2 Marks Each**

Answer each question clearly.

1. Define the Linux kernel and explain two of its responsibilities.

2. Explain the difference between kernel space and user space.

3. What are system calls? Give two examples of operations that may require system calls.

4. What are kernel modules, and why are they useful?

5. Explain the difference between a kernel and a shell.

6. Describe the major stages of the Linux boot process.

7. What is the purpose of systemd?

8. Differentiate between a shell variable and an environment variable.

9. Explain the difference between single quotes and double quotes in Bash.

10. Explain the purpose of the following file descriptors:

   - `0`
   - `1`
   - `2`

---

# SECTION C – PRACTICAL COMMAND-LINE ASSESSMENT
**25 Marks**

Perform the following exercises inside your Kali Linux virtual machine.

For every task, record the command you used and briefly explain its result.

### Task 1 – Kernel Information (5 Marks)

1. Display your Linux kernel version.
2. Display complete kernel and system information.
3. Display your machine architecture.
4. Display the hostname.
5. Display the current username.

Suggested commands:

```bash
uname -r
uname -a
uname -m
hostname
whoami
```

### Task 2 – Kernel Modules (4 Marks)

1. Display currently loaded kernel modules.
2. Display information about one loaded module.
3. Identify the module's description.
4. Explain why modules are important.

Suggested commands:

```bash
lsmod
modinfo MODULE_NAME
```

Replace `MODULE_NAME` with an actual module name from your `lsmod` output.

### Task 3 – systemd and Services (4 Marks)

1. Check the status of a service available on your system.
2. Display the system's default target.
3. List active services.
4. Explain what a system service does.

Suggested commands:

```bash
systemctl status cron
systemctl get-default
systemctl list-units --type=service --state=running
```

If `cron` is not installed, select another available service.

### Task 4 – Bash Variables (4 Marks)

Run the following commands:

```bash
NAME="Linux Student"
echo "$NAME"

export COURSE="Linux for Ethical Hackers"
printenv COURSE

echo "$HOME"
echo "$PATH"

unset NAME
```

Explain what each command does.

### Task 5 – Expansion and Quoting (4 Marks)

Run:

```bash
echo "My username is $(whoami)"
echo "Today is $(date)"
echo "Calculation: $((10 + 20))"

MESSAGE="Linux Security"
echo "$MESSAGE"
echo '$MESSAGE'
```

Explain the difference between the output of the final two commands.

### Task 6 – Redirection and Pipes (4 Marks)

Create a practice directory and sample log file:

```bash
mkdir -p ~/module04-practice
cd ~/module04-practice

printf "INFO: System started\nERROR: Test error\nINFO: Service running\nWARNING: Test warning\nERROR: Another test error\n" > system.log
```

Perform these operations:

1. Display the log file.
2. Display only lines containing `ERROR`.
3. Count the number of lines.
4. Save the error lines into another file.

Suggested commands:

```bash
cat system.log
grep "ERROR" system.log
wc -l system.log
grep "ERROR" system.log > errors.log
```

Verify the result:

```bash
cat errors.log
```

---

# SECTION D – BASH SCRIPTING CHALLENGE
**20 Marks**

Create a Bash script named:

`module04-assessment.sh`

### Objective

Write a script that generates a basic Linux system information report.

Your script must:

1. Start with a valid Bash shebang.
2. Define at least five variables.
3. Display the current username.
4. Display the hostname.
5. Display the kernel version.
6. Display the system architecture.
7. Display the current shell.
8. Display the current working directory.
9. Display system uptime when the `uptime` command is available.
10. Use command substitution.
11. Use quoted variable expansions.
12. Use arithmetic expansion.
13. Define and call at least one function.
14. Use an `if` statement.
15. Save the report to a text file using output redirection.
16. Use a pipe with a command such as `grep` or `wc`.

### Additional Requirements

- Give the script a meaningful name.
- Add comments explaining important sections.
- Do not use `eval`.
- Do not expose passwords, tokens, or private information.
- Do not modify system configuration files.

### Expected Report Structure

Your generated report should look similar to this:

```text
================================
LINUX SYSTEM INFORMATION REPORT
================================

Username:
Hostname:
Kernel Version:
Architecture:
Current Shell:
Working Directory:
System Uptime:

--------------------------------
Report Summary
--------------------------------
Number of report lines:

Report generated successfully.
```

The values should come from your actual Kali system.

### Testing

After writing your script, make it executable:

```bash
chmod +x module04-assessment.sh
```

Run it:

```bash
./module04-assessment.sh
```

Confirm that the report file was created and contains the correct information.

---

# SECTION E – CYBERSECURITY SCENARIOS
**15 Marks – 5 Marks Each**

Answer the following scenarios.

### Scenario 1 – Kernel Investigation

You are investigating a Linux computer that has been behaving unusually.

Explain how you would:

- Identify its running kernel version.
- Inspect loaded kernel modules.
- Check relevant system logs.
- Explain why you should not remove unfamiliar modules without investigation.

### Scenario 2 – Shell Environment

A Bash script works when you execute it manually but fails when another process tries to run it.

Explain how environment variables and `PATH` could contribute to this problem.

Mention at least two commands you would use to investigate.

### Scenario 3 – Log Investigation

You are investigating a system log containing normal messages, warnings, and errors.

Explain how you would use:

- `grep`
- `wc`
- `|`
- `>`
- `2>`

to filter and save useful information while keeping standard output and standard error separate.

Use only your local practice log file.

---

# SECTION F – PERSONAL REFLECTION
**Optional – Not Graded**

Answer briefly:

1. Which Module 04 topic was easiest for you?
2. Which topic was most challenging?
3. Can you explain the difference between the Linux kernel and Bash without looking at your notes?
4. Can you write and execute a basic Bash script independently?
5. What do you still need to practise?

---

# MARKING GUIDE

| Section | Marks |
|---|---:|
| A – Multiple Choice | 20 |
| B – Short Answer | 20 |
| C – Practical Commands | 25 |
| D – Bash Scripting | 20 |
| E – Cybersecurity Scenarios | 15 |
| **TOTAL** | **100** |

## Performance Levels

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| 0–49 | Needs Improvement |

**Pass requirement: 50/100.**

---

# SUBMISSION CHECKLIST

Before submitting your assessment, confirm:

- [ ] I completed all 20 multiple-choice questions.
- [ ] I answered all 10 short-answer questions.
- [ ] I completed all six practical tasks.
- [ ] I created the Bash assessment script.
- [ ] I executed the script successfully.
- [ ] I verified the generated report.
- [ ] I answered all three cybersecurity scenarios.
- [ ] I reviewed my answers.

---

## END OF MODULE 04

**Module:** Linux Kernel and Shell  
**Status:** Assessment completed only after submission and marking.

**Next Module:** Module 05 – Linux Commands

**Remember:** Understanding Linux is more important than memorising commands. Learn what each command does, why it works, and when to use it.