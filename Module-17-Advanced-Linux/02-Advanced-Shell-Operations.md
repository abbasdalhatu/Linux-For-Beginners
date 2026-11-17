---
---


# MODULE 17 – ADVANCED LINUX
## LESSON 02: ADVANCED SHELL OPERATIONS

---

## 1. Introduction to Advanced Shell Operations

The Linux shell is a command-line interpreter that allows users to interact with the operating system.

You already know basic commands such as:

```bash
ls
cd
pwd
mkdir
cp
mv
```

Advanced shell operations allow us to combine commands, manipulate data, automate tasks, and perform complex administration efficiently.

In this lesson, we will study:

1. Shell types.
2. Command chaining.
3. Input and output redirection.
4. Pipelines.
5. Wildcards and pattern matching.
6. Command substitution.
7. Environment variables.
8. Shell scripting fundamentals.
9. Command history and aliases.
10. Practical shell administration.

### Learning objectives

By the end of this lesson, you should be able to:

- Explain how a Linux shell processes commands.
- Combine multiple commands.
- Redirect command output into files.
- Use pipelines to process information.
- Understand shell expansion.
- Use environment variables.
- Create useful command aliases.
- Build simple command-line workflows.

---

## 2. Understanding Linux Shell Types

A shell provides an interface between the user and the operating system.

Different shells provide different features.

| Shell | Description |
|---|---|
| Bash | Bourne Again Shell; widely used |
| Zsh | Advanced interactive shell |
| Fish | User-friendly interactive shell |
| Sh | Traditional Unix shell interface |
| Ksh | KornShell |

### Identify your current shell

```bash
echo $SHELL
```

This displays your configured login shell.

To identify the shell currently executing your commands:

```bash
ps -p $$ -o comm=
```

To list available shells:

```bash
cat /etc/shells
```

### Check Bash version

```bash
bash --version
```

### Important distinction

`$SHELL` usually identifies your configured login shell. It does not necessarily identify the shell currently executing a command.

---

## 3. Command Chaining

Command chaining allows you to execute multiple commands in one command line.

Linux provides several operators.

### A. Semicolon (;)

Runs commands sequentially, regardless of whether earlier commands succeed.

Example:

```bash
pwd; ls; whoami
```

Execution:

1. Display current directory.
2. List files.
3. Display current username.

### B. AND operator (&&)

Runs the next command only if the previous command succeeds.

Example:

```bash
mkdir test-directory && cd test-directory
```

If directory creation succeeds, Linux changes into that directory.

### C. OR operator (||)

Runs the next command only if the previous command fails.

Example:

```bash
cd /directory-that-does-not-exist || echo "Directory not found"
```

### Comparison

| Operator | Behaviour |
|---|---|
| `;` | Run commands sequentially |
| `&&` | Continue when previous command succeeds |
| `||` | Continue when previous command fails |

### Practical example

```bash
mkdir -p ~/module17-lab/lesson02 && cd ~/module17-lab/lesson02
```

This creates the laboratory directory and enters it if creation succeeds.

---

## 4. Input and Output Redirection

Redirection changes where a command receives input or sends output.

Linux commonly uses three standard streams.

| Stream | Number | Purpose |
|---|---:|---|
| Standard Input (stdin) | 0 | Receives input |
| Standard Output (stdout) | 1 | Displays normal output |
| Standard Error (stderr) | 2 | Displays error messages |

### A. Output redirection (>)

Saves command output into a file.

```bash
ls > files.txt
```

If the file already exists, its previous contents are overwritten.

### B. Append redirection (>>)

Adds output to the end of a file.

```bash
date >> files.txt
```

### C. Input redirection (<)

Uses a file as command input.

```bash
sort < names.txt
```

### D. Error redirection (2>)

Saves error messages separately.

```bash
ls /invalid-directory 2> errors.txt
```

### E. Redirect both output and errors

```bash
command > output.txt 2>&1
```

This sends standard output and standard error to the same file.

A Bash-specific alternative is:

```bash
command &> output.txt
```

### F. Discard unwanted output

Linux provides a special device:

```bash
/dev/null
```

Example:

```bash
command 2>/dev/null
```

This discards error messages.

**Security note:** Do not automatically discard errors during troubleshooting. Error messages often contain important diagnostic information.

---

## 5. Understanding Linux Pipelines

A pipeline connects the output of one command to the input of another.

The pipeline operator is:

```bash
|
```

### Example 1: Search running processes

```bash
ps aux | grep bash
```

Explanation:

1. `ps aux` displays running processes.
2. The pipeline passes the output to `grep`.
3. `grep bash` searches for matching lines.

### Example 2: Count files

```bash
ls | wc -l
```

Explanation:

- `ls` lists directory entries.
- `wc -l` counts lines.

### Example 3: Sort usernames

```bash
cut -d: -f1 /etc/passwd | sort
```

Explanation:

1. `cut` extracts the first field.
2. `-d:` specifies the colon delimiter.
3. `-f1` selects the first field.
4. `sort` arranges the results alphabetically.

### Example 4: Count matching entries

```bash
grep -c "root" /etc/passwd
```

Counts matching lines containing the word `root`.

### Why pipelines matter

Pipelines are useful for:

- Log analysis.
- System administration.
- Data processing.
- Troubleshooting.
- Security monitoring.
- Automating repetitive tasks.

---

## 6. Wildcards and Pattern Matching

Wildcards allow commands to match multiple filenames.

### A. Asterisk (*)

Matches zero or more characters.

```bash
ls *.txt
```

Displays files ending in `.txt`.

### B. Question mark (?)

Matches exactly one character.

```bash
ls file?.txt
```

Can match:

```text
file1.txt
fileA.txt
file2.txt
```

It does not match `file10.txt`.

### C. Square brackets ([])

Matches one character from a specified set.

```bash
ls file[123].txt
```

Matches:

```text
file1.txt
file2.txt
file3.txt
```

### D. Character ranges

```bash
ls file[a-z].txt
```

Matches a lowercase letter in that position.

### Important distinction

Shell wildcards are expanded by the shell before a command receives its arguments.

They are different from regular expressions, which are interpreted by tools such as `grep`.

---

## 7. Command Substitution

Command substitution allows the output of one command to become part of another command.

The preferred syntax is:

```bash
$(command)
```

### Example 1

```bash
echo "Today is $(date)"
```

The shell executes `date` and inserts its output into the message.

### Example 2

```bash
CURRENT_USER=$(whoami)
echo "$CURRENT_USER"
```

The output of `whoami` is stored in a variable.

### Example 3

```bash
CURRENT_DIR=$(pwd)
echo "Working directory: $CURRENT_DIR"
```

### Older syntax

```bash
`command`
```

Backticks also perform command substitution, but `$()` is generally easier to read and nest.

---

## 8. Understanding Environment Variables

Environment variables store information that programs can use.

Examples include:

| Variable | Purpose |
|---|---|
| `$HOME` | User's home directory |
| `$USER` | Current username |
| `$PATH` | Executable search locations |
| `$SHELL` | Configured login shell |
| `$PWD` | Current working directory |
| `$LANG` | Language and locale settings |

### Display a variable

```bash
echo "$HOME"
```

### Display PATH

```bash
echo "$PATH"
```

### List environment variables

```bash
printenv
```

### Create a shell variable

```bash
COURSE="Advanced Linux"
echo "$COURSE"
```

This creates a shell variable.

### Export a variable

```bash
export COURSE="Advanced Linux"
```

The variable is now available to child processes.

### Remove a variable

```bash
unset COURSE
```

### Important distinction

A normal shell variable is not automatically inherited by child processes. Exporting it makes it part of the environment passed to child processes.

---

## 9. Command History

Bash records commands entered during a shell session.

### Display command history

```bash
history
```

### Display the last 10 commands

```bash
history 10
```

### Search command history

Press:

```text
CTRL + R
```

Then type part of a previous command.

### Clear the current in-memory history list

```bash
history -c
```

**Important:** History clearing is not a secure method of removing evidence. Commands may also be recorded by terminal logs, system auditing, or other monitoring mechanisms.

For ordinary administration, preserve useful history for troubleshooting and accountability.

---

## 10. Creating Command Aliases

An alias gives a command a shorter or more convenient name.

### Example 1

```bash
alias ll='ls -lah'
```

Now run:

```bash
ll
```

### Example 2

```bash
alias update-system='sudo apt update'
```

Run:

```bash
update-system
```

### Display aliases

```bash
alias
```

### Remove an alias

```bash
unalias ll
```

### Make aliases persistent

Open your Bash configuration:

```bash
nano ~/.bashrc
```

Add:

```bash
alias ll='ls -lah'
alias cls='clear'
alias update-system='sudo apt update'
```

Save and reload:

```bash
source ~/.bashrc
```

The aliases should now be available in new interactive Bash sessions as well.

---

## 11. Understanding Exit Status

Linux commands return an exit status after execution.

The special variable `$?` stores the exit status of the most recently completed foreground command.

Common meanings:

| Exit Status | Meaning |
|---:|---|
| 0 | Success |
| 1 | General error |
| 2 | Misuse of shell built-in or command |
| Other nonzero values | Failure or application-specific result |

### Example 1: Successful command

```bash
pwd
echo $?
```

The result will normally be:

```text
0
```

### Example 2: Failed command

```bash
ls /directory-that-does-not-exist
echo $?
```

The result should be nonzero.

### Why exit statuses matter

They allow scripts to determine whether an operation succeeded.

For example:

```bash
mkdir test && echo "Directory created successfully"
```

The success message appears only if directory creation succeeds.

---

## 12. Practical Lab: Advanced Shell Operations

### Lab objectives

You will practise:

- Command chaining.
- Redirection.
- Pipelines.
- Wildcards.
- Environment variables.
- Command substitution.
- Aliases.
- Exit-status checking.

### Step 1: Create the laboratory directory

```bash
mkdir -p ~/module17-lab/lesson02
cd ~/module17-lab/lesson02
```

### Step 2: Create sample files

```bash
touch file1.txt file2.txt file3.txt report1.log report2.log
```

Verify:

```bash
ls -l
```

### Step 3: Practise wildcards

```bash
ls *.txt
```

Then:

```bash
ls report?.log
```

### Step 4: Practise output redirection

```bash
ls -l > directory-report.txt
```

Read the report:

```bash
cat directory-report.txt
```

### Step 5: Practise append redirection

```bash
date >> directory-report.txt
```

Check:

```bash
cat directory-report.txt
```

### Step 6: Practise pipelines

```bash
ls -1 | wc -l
```

This counts the directory entries printed by `ls`.

### Step 7: Practise command substitution

```bash
CURRENT_USER=$(whoami)
echo "Current user: $CURRENT_USER"
```

### Step 8: Practise environment variables

```bash
export LAB_NAME="Module 17 Advanced Linux"
echo "$LAB_NAME"
```

### Step 9: Practise exit status

```bash
pwd
echo $?
```

Then:

```bash
ls /nonexistent-directory
echo $?
```

Compare the results.

### Step 10: Practise aliases

```bash
alias ll='ls -lah'
ll
```

### Step 11: Save your practical report

```bash
nano shell-operations-report.txt
```

Use this template:

```text
MODULE 17 - LESSON 02
ADVANCED SHELL OPERATIONS REPORT

1. Current Shell:
2. Bash Version:
3. Number of Sample Files:
4. Output Redirection Result:
5. Append Redirection Result:
6. Pipeline Result:
7. Current Username:
8. LAB_NAME Variable:
9. Successful Command Exit Status:
10. Failed Command Exit Status:

OBSERVATIONS:

Write what you learned about advanced shell operations.
```

Save using:

- CTRL + O
- ENTER
- CTRL + X

---

## 13. Knowledge Check

Answer these questions in your notebook.

1. What is a Linux shell?
2. Explain the difference between Bash and Zsh.
3. What is the purpose of the `&&` operator?
4. Explain the difference between `>` and `>>`.
5. What are stdin, stdout, and stderr?
6. What is a pipeline?
7. Explain the difference between `*` and `?`.
8. What is command substitution?
9. What is the difference between a shell variable and an environment variable?
10. What is the purpose of the `$PATH` variable?
11. What is a command alias?
12. What does exit status `0` normally represent?

---

## 14. Important Commands Summary

| Command | Purpose |
|---|---|
| `echo $SHELL` | Display configured login shell |
| `bash --version` | Display Bash version |
| `pwd; ls` | Execute sequential commands |
| `command1 && command2` | Run second command on success |
| `command1 || command2` | Run second command on failure |
| `command > file` | Redirect standard output |
| `command >> file` | Append standard output |
| `command 2> file` | Redirect standard error |
| `command1 \| command2` | Connect commands using a pipeline |
| `printenv` | Display environment variables |
| `history` | Display command history |
| `alias` | Display aliases |
| `unalias name` | Remove an alias |
| `echo $?` | Display previous command's exit status |
| `$(command)` | Perform command substitution |
| `source ~/.bashrc` | Reload Bash configuration |

---

## 15. Lesson Summary

In this lesson, you learned how to use the Linux shell more effectively.

You covered:

- Different Linux shells.
- Command chaining.
- Input and output redirection.
- Standard input, output, and error streams.
- Pipelines.
- Wildcards.
- Command substitution.
- Environment variables.
- Command history.
- Aliases.
- Exit statuses.

These skills are important for advanced Bash scripting, Linux administration, automation, and cybersecurity investigations.

### Completion checklist

- [ ] Understand shell types.
- [ ] Practise command chaining.
- [ ] Understand input and output redirection.
- [ ] Practise pipelines.
- [ ] Understand wildcards.
- [ ] Practise command substitution.
- [ ] Understand environment variables.
- [ ] Create and use aliases.
- [ ] Understand exit statuses.
- [ ] Complete the practical lab.
- [ ] Save `shell-operations-report.txt`.
- [ ] Answer the knowledge-check questions.

**END OF LESSON 02**
