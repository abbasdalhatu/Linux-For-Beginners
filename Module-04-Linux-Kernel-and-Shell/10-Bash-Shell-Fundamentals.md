# Lesson 10: Bash Shell Fundamentals

## 1. Introduction

Bash is one of the most widely used command-line shells in Linux.

It allows users to interact with the operating system, execute commands, manage files, and automate tasks.

Bash stands for **Bourne Again SHell**.

It is an improved version of the original Bourne shell (sh).

As an ethical hacker, Bash is important because it allows you to:
- Execute Linux commands.
- Automate repetitive tasks.
- Manage files and directories.
- Process text and command output.
- Write scripts for system administration.
- Build simple security automation tools.

In this lesson, you will learn the fundamentals of Bash and how to execute commands and basic scripts.

## 2. What Is Bash?

Bash is a command interpreter and scripting language.

It performs two major roles:

1. **Interactive shell:** Allows users to enter commands directly into a terminal.
2. **Scripting language:** Allows users to write files containing commands that Bash executes in sequence.

### Example: Interactive Bash

Open your Kali terminal and enter:

```bash
echo "Hello, Linux"
```

Output:

```text
Hello, Linux
```

Bash interprets the command and displays the message.

### Example: Bash script

A Bash script is a text file containing Bash commands.

Example:

```bash
#!/bin/bash

echo "Welcome to Linux"
echo "Learning Bash fundamentals"
```

When executed, it displays both messages.

## 3. Checking Whether Bash Is Installed

Most Kali Linux installations include Bash.

Check its location:

```bash
command -v bash
```

You may see:

```text
/usr/bin/bash
```

Check its version:

```bash
bash --version
```

This displays the installed Bash version.

You can also inspect your current shell process:

```bash
ps -p $$ -o pid,comm,args
```

This helps determine whether your current command environment is running Bash.

## 4. Understanding Bash Command Structure

A Bash command generally follows this structure:

```text
command [options] [arguments]
```

Example:

```bash
ls -lah /home/kali
```

Breakdown:

| Component | Meaning |
|---|---|
| `ls` | Command |
| `-l` | Detailed listing |
| `-a` | Include hidden entries |
| `-h` | Human-readable sizes |
| `/home/kali` | Target directory |

Bash interprets the command and passes the relevant information to the command being executed.

## 5. Running Multiple Commands

Bash allows you to execute multiple commands in one line.

### Method 1: Semicolon (;)

A semicolon separates commands.

Example:

```bash
pwd; whoami; hostname
```

The commands execute sequentially.

The next command normally executes even if the previous command fails.

### Method 2: Logical AND (&&)

The `&&` operator runs the next command only if the previous command succeeds.

Example:

```bash
mkdir BashLab && cd BashLab
```

If the directory is created successfully, Bash moves into it.

### Method 3: Logical OR (||)

The `||` operator runs the next command only if the previous command fails.

Example:

```bash
ls /directory-that-does-not-exist || echo "Directory not found"
```

If `ls` fails, the message is displayed.

### Comparison

| Operator | Meaning |
|---|---|
| `;` | Run the next command regardless of the previous result |
| `&&` | Run the next command if the previous command succeeds |
| `||` | Run the next command if the previous command fails |

These operators are useful for building command sequences.

## 6. Understanding Bash Quoting

Bash uses different types of quotation marks to interpret text.

The three important forms are:

- Single quotes `' '`
- Double quotes `" "`
- Backslash `\`

### A. Single quotes

Single quotes preserve the literal meaning of almost everything inside them.

Example:

```bash
echo 'Hello, Linux'
```

Output:

```text
Hello, Linux
```

Variable expansion does not occur inside single quotes.

Example:

```bash
name="Abbas"
echo '$name'
```

Output:

```text
$name
```

### B. Double quotes

Double quotes preserve most characters while allowing variable expansion and certain special expansions.

Example:

```bash
name="Abbas"
echo "Hello, $name"
```

Output:

```text
Hello, Abbas
```

### C. Backslash

The backslash can prevent a special character from being interpreted in its usual way.

Example:

```bash
echo "The price is \$50"
```

Output:

```text
The price is $50
```

### Comparison

| Quoting method | Variable expansion |
|---|---|
| `'Single quotes'` | No |
| `"Double quotes"` | Yes |
| `\` | Can escape special characters |

## 7. Understanding Bash Variables

A variable stores information that can be reused in a shell session or script.

Example:

```bash
name="Abbas"
```

This creates a variable named `name`.

Display its value:

```bash
echo "$name"
```

Output:

```text
Abbas
```

### Important Bash variable rules

1. Variable names are case-sensitive.
2. Do not put spaces around the assignment operator.
3. Variable names should begin with a letter or underscore.
4. Variable values are referenced using `$`.

Correct:

```bash
username="kali"
```

Incorrect:

```bash
username = "kali"
```

### Multiple variables

```bash
name="Abbas"
course="Linux for Ethical Hackers"
level="Beginner"

echo "$name"
echo "$course"
echo "$level"
```

## 8. Understanding the export Command

By default, a normal shell variable is available to the current shell environment.

The `export` command makes a variable available to child processes through their environment.

Example:

```bash
username="kali"
export username
```

Now child processes launched from this shell can inherit the variable.

You can also assign and export in one command:

```bash
export COURSE="Linux for Ethical Hackers"
```

Display it:

```bash
echo "$COURSE"
```

Check it using:

```bash
printenv COURSE
```

**Important:** Exported variables are not automatically permanent. Their lifetime normally ends when the relevant environment ends unless they are defined in a startup configuration file.

## 9. Understanding Command Substitution

Command substitution allows Bash to execute a command and use its output as part of another command.

The modern recommended syntax is:

```bash
$(command)
```

Example:

```bash
current_user=$(whoami)
echo "$current_user"
```

Bash runs `whoami`, stores its output in the variable, and displays the result.

Another example:

```bash
current_directory=$(pwd)
echo "Current directory: $current_directory"
```

### Why is command substitution useful?

It allows scripts to collect information dynamically.

For example, a system information script can collect:
- Current username.
- Hostname.
- Kernel version.
- Current directory.
- System uptime.

## 10. Understanding Bash Comments

Comments are notes written inside a script.

Bash ignores comments during normal execution.

A comment begins with the `#` symbol.

Example:

```bash
#!/bin/bash

# Display a welcome message
echo "Welcome to Bash"

# Display the current username
whoami
```

### Why are comments important?

Comments:
- Explain what a script does.
- Make code easier to understand.
- Help with troubleshooting.
- Improve collaboration.
- Make scripts easier to maintain.

The first line, `#!/bin/bash`, is called the shebang. It tells the operating system which interpreter should execute the script when it is run as an executable file.

## 11. Creating Your First Bash Script

Now let's create a simple Bash script.

### Step 1: Create a project directory

```bash
mkdir BashPractice
```

### Step 2: Enter the directory

```bash
cd BashPractice
```

### Step 3: Create a script

```bash
nano first-script.sh
```

Enter:

```bash
#!/bin/bash

# My first Bash script

echo "================================"
echo "       BASH PRACTICE"
echo "================================"

echo "Welcome to Linux for Ethical Hackers"

echo "Current user:"
whoami

echo "Current directory:"
pwd

echo "Kernel version:"
uname -r

echo "Script completed successfully"
```

Save the file:

- Press `Ctrl + O`.
- Press `Enter`.
- Press `Ctrl + X`.

### Step 4: Execute the script using Bash

```bash
bash first-script.sh
```

This executes the script through Bash without requiring the executable permission bit.

### Step 5: Make the script executable

```bash
chmod +x first-script.sh
```

### Step 6: Execute it directly

```bash
./first-script.sh
```

The `./` tells the shell to execute the file from the current directory.

## 12. Understanding Script Permissions

Linux uses file permissions to control access to files.

For a Bash script to be executed directly as a program, it generally needs the executable permission bit.

Inspect the permissions:

```bash
ls -l first-script.sh
```

Example:

```text
-rwxr-xr-x
```

The `x` characters indicate executable permissions for the corresponding user categories.

Change the permissions:

```bash
chmod +x first-script.sh
```

Remove executable permissions:

```bash
chmod -x first-script.sh
```

Even without executable permission, you can generally run a readable script by passing it explicitly to Bash:

```bash
bash first-script.sh
```

## 13. Understanding Bash Exit Status

A command or script can return an exit status indicating whether it completed successfully.

Bash uses:

```bash
echo $?
```

to display the exit status of the most recently executed command.

Common convention:

| Exit status | Meaning |
|---|---|
| `0` | Success |
| Non-zero | Failure or another result |

### Example

Run:

```bash
mkdir TestDirectory
```

Then:

```bash
echo $?
```

If the directory was successfully created, the exit status should normally be `0`.

Now run:

```bash
mkdir TestDirectory
```

Again, the command should report that the directory already exists.

Check the exit status:

```bash
echo $?
```

The result should normally be non-zero.

### Why does this matter?

Exit statuses allow scripts to make decisions.

A script can check whether a command succeeded before continuing.

## 14. Understanding the PATH Variable

`PATH` is an environment variable containing directories that Bash searches when locating executable commands.

Inspect it:

```bash
echo "$PATH"
```

You may see directories such as:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

The actual value depends on your system.

### Why is PATH important?

When you enter:

```bash
ls
```

Bash searches the directories in `PATH` to locate the executable if the command is not a shell built-in, function, or alias.

### Inspect the location of a command

```bash
command -v ls
```

This helps identify the command that Bash would use.

### Security warning

An unsafe `PATH` configuration can cause an unintended executable to run instead of the expected command.

Avoid adding untrusted or user-writable directories to privileged execution paths.

## 15. Bash and Ethical Hacking

Bash is widely used in cybersecurity because it can automate administrative and investigative tasks.

### Example 1: System information

```bash
#!/bin/bash

echo "System Information"

echo "User:"
whoami

echo "Hostname:"
hostname

echo "Kernel:"
uname -r

echo "Uptime:"
uptime
```

### Example 2: Network information

```bash
#!/bin/bash

echo "Network Interfaces"
ip addr

echo "Listening Sockets"
ss -tuln
```

### Example 3: Searching logs

```bash
#!/bin/bash

echo "Searching system logs for errors"

journalctl -b | grep -i error
```

These examples demonstrate basic defensive administration and system inspection.

Only run security assessments against systems you own or have explicit authorization to test.

## 16. Practical Lab: Bash Fundamentals

### Lab objective

Practice Bash commands, variables, command substitution, quoting, and script execution.

### Step 1: Check Bash

```bash
bash --version
```

### Step 2: Check your current shell

```bash
ps -p $$ -o pid,comm,args
```

### Step 3: Run multiple commands

```bash
pwd; whoami; hostname
```

### Step 4: Test logical AND

```bash
mkdir BashLab && cd BashLab
```

### Step 5: Test logical OR

```bash
ls /not-a-real-directory || echo "The directory does not exist"
```

### Step 6: Practice variables

```bash
name="Abbas"
course="Linux for Ethical Hackers"

echo "$name"
echo "$course"
```

### Step 7: Practice quoting

```bash
echo '$name'
echo "$name"
```

Compare the outputs.

### Step 8: Practice command substitution

```bash
user=$(whoami)
echo "Current user: $user"
```

### Step 9: Practice exporting a variable

```bash
export COURSE="Linux for Ethical Hackers"
printenv COURSE
```

### Step 10: Create your second Bash script

```bash
nano system-info.sh
```

Enter:

```bash
#!/bin/bash

# Basic system information

echo "================================"
echo "       SYSTEM INFORMATION"
echo "================================"

echo "Username: $(whoami)"
echo "Hostname: $(hostname)"
echo "Kernel: $(uname -r)"
echo "Architecture: $(uname -m)"
echo "Current directory: $(pwd)"

echo "================================"
echo "       END OF REPORT"
echo "================================"
```

Save the script.

### Step 11: Execute the script

```bash
bash system-info.sh
```

### Step 12: Make it executable

```bash
chmod +x system-info.sh
```

### Step 13: Execute it directly

```bash
./system-info.sh
```

### Step 14: Inspect script permissions

```bash
ls -l system-info.sh
```

### Step 15: Check the exit status

```bash
echo $?
```

Record the result.

## 17. Knowledge Check

Answer these questions in your notebook.

1. What does Bash stand for?
2. What are the two main roles of Bash?
3. What command checks the installed Bash version?
4. What is the purpose of the semicolon operator?
5. What is the difference between `&&` and `||`?
6. What is the difference between single and double quotes?
7. What is a Bash variable?
8. What is the purpose of `export`?
9. What is command substitution?
10. What is the purpose of the shebang?
11. What does `chmod +x` do?
12. What is an exit status?
13. What does `PATH` contain?
14. Why is it important to quote variable expansions?
15. Give three examples of how Bash can help an ethical hacker.

## 18. Practical Assignment

Create a Bash script named:

`linux-report.sh`

Your script should display:

1. A welcome message.
2. The current username.
3. The hostname.
4. The kernel version.
5. The system architecture.
6. The current working directory.
7. The system uptime.
8. A closing message.

Requirements:

- Start the script with the correct shebang.
- Use at least three Bash variables.
- Use command substitution for at least two values.
- Include comments explaining important sections.
- Make the script executable.
- Execute it successfully.

Example structure:

```bash
#!/bin/bash

# Linux system report

username=$(whoami)
hostname_name=$(hostname)

echo "Linux System Report"
echo "Username: $username"
echo "Hostname: $hostname_name"

# Add the remaining information here
```

Do not simply copy the example and submit it as the complete assignment. Finish the remaining sections yourself.

## 19. Key Terms

| Term | Definition |
|---|---|
| Bash | Bourne Again SHell |
| Script | File containing commands to execute |
| Shebang | First-line interpreter declaration |
| Variable | Named storage for a value |
| export | Makes a variable available to child processes |
| Command substitution | Uses the output of a command in another expression |
| Comment | Text ignored by Bash during normal execution |
| Exit status | Numeric result returned by a command |
| PATH | Environment variable containing executable search directories |
| chmod | Command for changing file permissions |
| Executable | File that can be executed as a program |
| Quoting | Method of controlling how Bash interprets characters |

## 20. Exam Summary

Remember these important points:

- Bash stands for Bourne Again SHell.
- Bash is both an interactive shell and a scripting language.
- `bash --version` displays the installed Bash version.
- `;` separates commands.
- `&&` executes the next command after success.
- `||` executes the next command after failure.
- Single quotes prevent variable expansion.
- Double quotes allow variable expansion.
- Variables store reusable information.
- `export` passes variables to child processes.
- `$(command)` performs command substitution.
- `#!/bin/bash` identifies the script interpreter.
- `chmod +x` adds executable permission.
- Exit status `0` conventionally indicates success.
- `PATH` contains directories used to locate executable commands.

## 21. Conclusion

Bash is a fundamental Linux skill.

You have learned how Bash works, how to execute commands, how to use variables, and how to create and run simple scripts.

These skills provide the foundation for automation and more advanced Linux administration.

As we continue, you will learn how to manage variables and environment settings in greater depth.

**Next Lesson: Lesson 11 — Shell Variables and Environment.**