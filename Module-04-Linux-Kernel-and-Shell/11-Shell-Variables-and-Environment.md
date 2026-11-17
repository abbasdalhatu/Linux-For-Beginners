---
---

# Lesson 11: Shell Variables and Environment

## 1. Introduction

Variables are an essential part of Linux shell programming.

They allow you to store information and reuse it whenever needed.

In Bash, variables can store:
- Usernames.
- File paths.
- Network addresses.
- Configuration values.
- Command output.
- System information.

Understanding shell variables is important because they help you write flexible scripts instead of repeatedly typing the same information.

In this lesson, we will study:

1. What shell variables are.
2. How to create and use variables.
3. Local shell variables.
4. Environment variables.
5. Exporting variables.
6. Special Bash variables.
7. The PATH variable.
8. Environment configuration files.
9. How variables are used in cybersecurity.

## 2. What Is a Shell Variable?

A shell variable is a named location used to store a value that can be accessed by the shell.

Example:

```bash
username="Abbas"
```

Here:

- `username` is the variable name.
- `Abbas` is the stored value.

To display the value:

```bash
echo "$username"
```

Output:

```text
Abbas
```

The `$` symbol tells Bash that you want to access the value stored in the variable.

### Basic variable syntax

```bash
variable_name="value"
```

### Important rules

1. Variable names are case-sensitive.
2. Variable names should not contain spaces.
3. Do not put spaces around the assignment operator.
4. Variable names should begin with a letter or underscore.
5. Use `$variable_name` to access a variable's value.

Correct:

```bash
name="Abbas"
```

Incorrect:

```bash
name = "Abbas"
```

Bash interprets the incorrect example as a command rather than a variable assignment.

## 3. Creating and Using Variables

Let's create some variables.

```bash
name="Abbas"
course="Linux for Ethical Hackers"
level="Beginner"
```

Display them:

```bash
echo "$name"
echo "$course"
echo "$level"
```

You can also combine variables in a sentence:

```bash
echo "My name is $name and I am studying $course"
```

Output:

```text
My name is Abbas and I am studying Linux for Ethical Hackers
```

### Why are variables useful?

Imagine writing a script that uses the same directory path 20 times.

Instead of writing the full path repeatedly, you can store it in a variable.

Example:

```bash
target_directory="/home/kali/Documents"

echo "$target_directory"
```

If the directory changes, you only need to update the variable.

## 4. Understanding Variable Types in Bash

Bash is not a traditional statically typed programming language.

Most ordinary Bash variables are treated as strings, although Bash supports integer arithmetic and indexed or associative arrays.

For example:

```bash
age=21
```

You can use the value in arithmetic:

```bash
echo $((age + 1))
```

Output:

```text
22
```

Bash also supports arrays.

Example:

```bash
tools=("Nmap" "Wireshark" "tcpdump")
```

Display the first element:

```bash
echo "${tools[0]}"
```

Output:

```text
Nmap
```

Bash array indexing starts at zero.

We will study more advanced expansion and array syntax in later shell lessons.

## 5. Understanding Local Shell Variables

A normal variable created in Bash is available within the current shell.

Example:

```bash
username="kali"
```

Display it:

```bash
echo "$username"
```

The variable is available to commands executed directly by that shell only if it is exported when those commands need it as an environment variable.

### Important concept

A normal shell variable is not automatically inherited by child processes.

For example:

```bash
MYNAME="Abbas"
bash -c 'echo "$MYNAME"'
```

The new Bash process normally will not receive `MYNAME` as an environment variable.

This is because the variable was not exported.

## 6. Understanding Environment Variables

An environment variable is a variable made available to a process through its environment.

Environment variables help programs understand their operating environment.

Examples include:

- `HOME`
- `USER`
- `SHELL`
- `PATH`
- `PWD`
- `LANG`
- `TERM`

### Display an environment variable

```bash
echo "$HOME"
```

This displays your home directory.

Another example:

```bash
echo "$USER"
```

This displays the username associated with the current environment.

### Display all environment variables

```bash
printenv
```

This displays the environment variables available to the command.

You can also use:

```bash
env
```

Both commands are commonly used to inspect the process environment.

## 7. Difference Between Shell Variables and Environment Variables

This is an important distinction.

| Shell variable | Environment variable |
|---|---|
| Available in the current shell | Available to the process environment |
| Not automatically inherited by child processes | Inherited by child processes |
| Can be exported | Is already exported |
| Useful for shell-specific information | Useful for sharing configuration with programs |

### Example

Create a normal shell variable:

```bash
MYNAME="Abbas"
```

Check it:

```bash
echo "$MYNAME"
```

Now start a child Bash process:

```bash
bash -c 'echo "$MYNAME"'
```

The child normally displays an empty value.

Now export the variable:

```bash
export MYNAME
```

Start another child process:

```bash
bash -c 'echo "$MYNAME"'
```

This time, it should display:

```text
Abbas
```

The difference is that the exported variable becomes part of the environment inherited by the child process.

## 8. Using the export Command

The `export` command makes a shell variable available to child processes.

### Method 1: Create and export separately

```bash
COURSE="Linux for Ethical Hackers"
export COURSE
```

### Method 2: Create and export together

```bash
export COURSE="Linux for Ethical Hackers"
```

Display the value:

```bash
echo "$COURSE"
```

Inspect it:

```bash
printenv COURSE
```

### Remove an exported variable

Use:

```bash
unset COURSE
```

This removes the variable from the current shell environment.

Verify:

```bash
printenv COURSE
```

No value should be displayed if the variable is no longer present.

**Important:** `unset` affects the current shell and its environment. It does not remove a variable from another independent process that already inherited it.

## 9. Understanding Important Environment Variables

Linux uses many environment variables to control program behavior.

Let's examine some important ones.

### 9.1 HOME

Stores the current user's home directory.

```bash
echo "$HOME"
```

Example output:

```text
/home/kali
```

### 9.2 USER

Usually contains the username associated with the current environment.

```bash
echo "$USER"
```

### 9.3 SHELL

Contains the configured login shell path in many environments.

```bash
echo "$SHELL"
```

Remember that it may not identify the exact shell currently executing your commands.

### 9.4 PATH

Contains directories searched when locating executable commands.

```bash
echo "$PATH"
```

### 9.5 PWD

Contains the current working directory.

```bash
echo "$PWD"
```

### 9.6 LANG

Controls the default locale setting for many programs.

```bash
echo "$LANG"
```

### 9.7 TERM

Identifies the terminal type.

```bash
echo "$TERM"
```

### Summary

| Variable | Purpose |
|---|---|
| `HOME` | User's home directory |
| `USER` | Username associated with the environment |
| `SHELL` | Configured login shell path |
| `PATH` | Executable search directories |
| `PWD` | Current working directory |
| `LANG` | Locale configuration |
| `TERM` | Terminal type |

## 10. Understanding the PATH Variable

The `PATH` variable is particularly important.

It tells the shell which directories to search when locating executable commands.

Inspect it:

```bash
echo "$PATH"
```

Example:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

The colon `:` separates individual directories.

### How PATH works

When you enter:

```bash
ls
```

Bash searches for the command using its command-resolution rules.

If it needs to locate an external executable, it searches directories listed in `PATH`.

### Locate an executable

```bash
command -v ls
```

You may receive:

```text
/usr/bin/ls
```

The exact location can vary.

### Temporarily modify PATH

You can add a directory to the beginning of your PATH:

```bash
PATH="/home/kali/bin:$PATH"
```

This modifies PATH in the current shell.

It is temporary unless saved in a shell startup configuration file.

### Security warning

PATH manipulation can create security problems.

If an untrusted directory appears before trusted system directories, an attacker may be able to cause an unintended executable to run.

Avoid adding writable or untrusted directories to privileged execution paths.

## 11. Understanding Special Bash Variables

Bash provides special variables that contain information about commands, scripts, and processes.

These are extremely useful when writing scripts.

### 11.1 `$0`

Contains the name used to invoke the current script or shell.

Example:

```bash
echo "$0"
```

Inside a script, it commonly displays the script's invocation name.

### 11.2 `$1`, `$2`, `$3`

These represent positional parameters passed to a script or function.

Example:

```bash
#!/bin/bash

echo "First argument: $1"
echo "Second argument: $2"
```

Run:

```bash
bash script.sh Abbas Linux
```

Output:

```text
First argument: Abbas
Second argument: Linux
```

### 11.3 `$#`

Contains the number of positional parameters.

```bash
echo "$#"
```

### 11.4 `$@`

Represents all positional parameters individually.

Example:

```bash
printf '%s\n' "$@"
```

This prints each argument on a separate line when used with proper quoting.

### 11.5 `$?`

Contains the exit status of the most recently executed command.

```bash
echo "$?"
```

### 11.6 `$$`

Contains the process ID of the current shell.

```bash
echo "$$"
```

### 11.7 `$!`

Contains the process ID of the most recently started background job.

### Summary

| Variable | Meaning |
|---|---|
| `$0` | Script or shell invocation name |
| `$1` | First positional parameter |
| `$2` | Second positional parameter |
| `$#` | Number of positional parameters |
| `$@` | All positional parameters |
| `$?` | Last command's exit status |
| `$$` | Current shell's process ID |
| `$!` | Most recent background job's process ID |

## 12. Understanding Bash Startup Files

Linux shells can read configuration files when they start.

These files allow users to configure their shell environment.

Common Bash startup files include:

| File | Purpose |
|---|---|
| `/etc/profile` | System-wide login-shell configuration |
| `~/.bash_profile` | User-specific login configuration, when present |
| `~/.bash_login` | Alternative user login configuration |
| `~/.profile` | User login configuration |
| `~/.bashrc` | User-specific interactive non-login Bash configuration |
| `/etc/bash.bashrc` | System-wide interactive Bash configuration on Debian-based systems |

The exact startup behavior depends on whether Bash is interactive, a login shell, or a non-interactive shell.

### What is the difference between login and interactive shells?

A login shell is started as a user's login environment.

An interactive shell is a shell that accepts commands directly from a user.

A shell can be both login and interactive, interactive but not login, or non-interactive.

### Why are startup files useful?

They can configure:
- Environment variables.
- PATH settings.
- Aliases.
- Shell functions.
- Prompt appearance.
- Other interactive behavior.

**Safety rule:** Do not copy unknown startup commands into your configuration files. A malicious startup command can execute automatically whenever the shell starts.

## 13. Understanding Temporary and Persistent Variables

Variables can exist for different lengths of time.

### Temporary variable

```bash
COURSE="Linux for Ethical Hackers"
```

This variable exists in the current shell session.

When the shell exits, the variable normally disappears.

### Exported temporary variable

```bash
export COURSE="Linux for Ethical Hackers"
```

This variable is available to child processes but normally disappears when the environment ends.

### Persistent variable

You can define a variable in a suitable startup configuration file.

For example, a user may add:

```bash
export COURSE="Linux for Ethical Hackers"
```

to `~/.bashrc` for interactive Bash sessions.

The variable will then be set whenever that configuration is read.

Do not modify startup files merely to complete this lesson. We are studying the concept first.

## 14. Shell Variables and Cybersecurity

Shell variables are important in cybersecurity because scripts frequently need to store and process system information.

### Example 1: Store a username

```bash
username=$(whoami)

echo "Current user: $username"
```

### Example 2: Store a kernel version

```bash
kernel=$(uname -r)

echo "Running kernel: $kernel"
```

### Example 3: Store a hostname

```bash
host=$(hostname)

echo "System hostname: $host"
```

### Example 4: Store a network interface name

```bash
interface="eth0"

echo "Inspecting interface: $interface"
```

The interface name is only an example. Your system may use a different name.

### Example 5: Create a simple system report

```bash
#!/bin/bash

username=$(whoami)
hostname_name=$(hostname)
kernel=$(uname -r)
architecture=$(uname -m)

echo "SYSTEM REPORT"
echo "Username: $username"
echo "Hostname: $hostname_name"
echo "Kernel: $kernel"
echo "Architecture: $architecture"
```

### Security considerations

When writing security scripts:

1. Quote variable expansions when appropriate.
2. Validate user-supplied values.
3. Avoid placing passwords and tokens in shell history.
4. Avoid exposing secrets through command-line arguments.
5. Do not trust environment variables blindly.
6. Be careful when modifying PATH.
7. Avoid executing files from untrusted directories.

## 15. Practical Lab: Exploring Shell Variables and Environment

### Lab objective

Learn how to create, inspect, export, and remove shell variables.

You will also explore special Bash variables and environment configuration.

### Step 1: Inspect important environment variables

```bash
echo "$HOME"
echo "$USER"
echo "$SHELL"
echo "$PATH"
echo "$PWD"
echo "$LANG"
echo "$TERM"
```

Record their outputs.

### Step 2: Display all environment variables

```bash
printenv
```

Observe the information available to your shell.

Do not publish sensitive environment values if any are present.

### Step 3: Create a normal shell variable

```bash
MYNAME="Abbas"
```

Display it:

```bash
echo "$MYNAME"
```

### Step 4: Test variable inheritance

```bash
bash -c 'echo "$MYNAME"'
```

Observe the result.

### Step 5: Export the variable

```bash
export MYNAME
```

Now run:

```bash
bash -c 'echo "$MYNAME"'
```

Compare the output.

### Step 6: Remove the variable

```bash
unset MYNAME
```

Verify:

```bash
echo "$MYNAME"
```

### Step 7: Create multiple variables

```bash
name="Abbas"
course="Linux for Ethical Hackers"
level="Beginner"
```

Display them:

```bash
echo "$name"
echo "$course"
echo "$level"
```

### Step 8: Practice command substitution

```bash
current_user=$(whoami)
current_kernel=$(uname -r)
current_host=$(hostname)
```

Display:

```bash
echo "$current_user"
echo "$current_kernel"
echo "$current_host"
```

### Step 9: Explore special Bash variables

```bash
echo "$0"
echo "$#"
echo "$?"
echo "$$"
```

Observe what each one represents in your current shell.

### Step 10: Create a positional parameter script

Create a file:

```bash
nano arguments.sh
```

Enter:

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Total arguments: $#"

echo "All arguments:"
printf '%s\n' "$@"
```

Save the file.

Execute:

```bash
bash arguments.sh Abbas Linux
```

Observe the output.

### Step 11: Inspect your PATH

```bash
echo "$PATH"
```

Then:

```bash
command -v bash
```

Observe the executable location.

### Step 12: Inspect Bash startup files

```bash
ls -la ~
```

Look for files such as `.bashrc` and `.profile`.

Do not edit or delete them during this exercise.

## 16. Knowledge Check

Answer these questions in your notebook.

1. What is a shell variable?
2. What is an environment variable?
3. What is the difference between a shell variable and an environment variable?
4. What is the purpose of `export`?
5. What does `unset` do?
6. What is the purpose of the HOME variable?
7. What does PATH contain?
8. What does `$?` represent?
9. What does `$$` represent?
10. What is the purpose of `$1`?
11. What does `$#` represent?
12. What is the purpose of `$@`?
13. What is the difference between temporary and persistent variables?
14. What is the purpose of `.bashrc`?
15. Why can unsafe PATH modifications create security problems?

## 17. Practical Assignment

Create a Bash script named:

`environment-report.sh`

Your script must display:

1. The current username.
2. The hostname.
3. The home directory.
4. The configured shell.
5. The current working directory.
6. The kernel version.
7. The system architecture.
8. The number of arguments passed to the script.
9. All arguments passed to the script.

### Requirements

- Use at least five variables.
- Use command substitution.
- Use positional parameters.
- Use `$#` and `$@`.
- Include comments.
- Quote variable expansions appropriately.
- Execute the script with at least two arguments.

### Example starting structure

```bash
#!/bin/bash

# Environment information report

username=$(whoami)
hostname_name=$(hostname)
home_directory="$HOME"
shell_name="$SHELL"

echo "SYSTEM ENVIRONMENT REPORT"

echo "Username: $username"
echo "Hostname: $hostname_name"
echo "Home: $home_directory"
echo "Shell: $shell_name"

echo "Arguments supplied: $#"
printf '%s\n' "$@"
```

Complete the remaining requirements yourself.

## 18. Key Terms

| Term | Definition |
|---|---|
| Variable | Named storage for a value |
| Shell variable | Variable available in the current shell |
| Environment variable | Variable passed through a process environment |
| `export` | Makes a shell variable available to child processes |
| `unset` | Removes a variable from the current shell |
| PATH | Directories searched for executable commands |
| HOME | User's home directory |
| PWD | Current working directory |
| Positional parameters | Arguments passed to a script or function |
| `$?` | Exit status of the previous command |
| `$@` | All positional parameters |
| `$#` | Number of positional parameters |
| `.bashrc` | User-specific interactive Bash configuration file |

## 19. Exam Summary

Remember these important points:

- Variables store reusable information.
- Bash variables are generally untyped strings.
- Variable assignment does not use spaces around `=`.
- Environment variables are available to child processes.
- `export` makes a variable available to child processes.
- `unset` removes a variable from the current shell.
- HOME stores the user's home directory.
- PATH contains executable search directories.
- `$0` identifies the script or shell invocation name.
- `$1` and `$2` represent positional parameters.
- `$#` counts positional parameters.
- `$@` represents all positional parameters.
- `$?` stores the last command's exit status.
- `$$` stores the current shell's process ID.
- `.bashrc` is commonly used for interactive Bash configuration.

## 20. Conclusion

Shell variables and environment variables are essential for Linux administration and Bash scripting.

You have learned how to create variables, export them, inspect environment information, and use special Bash variables.

You have also explored startup configuration files and security considerations related to environment variables.

These concepts will help you write more flexible and useful automation scripts.

**Next Lesson: Lesson 12 — Shell Expansion and Quoting.**