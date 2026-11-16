# Lesson 14: Shell Configuration and Customization

## 1. Introduction

The Linux shell is highly customizable.

You can configure how your terminal looks, how commands behave, which shortcuts are available, and which environment variables are loaded whenever you start a shell.

For example, you can:
- Customize your terminal prompt.
- Create command aliases.
- Define reusable shell functions.
- Configure environment variables.
- Customize command history.
- Configure Bash startup files.
- Improve your command-line workflow.

Shell configuration is important for Linux administrators and ethical hackers because it helps create an efficient working environment.

In this lesson, we will study:

1. What shell configuration means.
2. Bash startup files.
3. Login and interactive shells.
4. Understanding `.bashrc`.
5. Understanding `.profile`.
6. Customizing the Bash prompt.
7. Creating aliases.
8. Creating shell functions.
9. Configuring command history.
10. Environment configuration.
11. Security considerations.
12. Practical customization.

## 2. What Is Shell Configuration?

Shell configuration is the process of adjusting the behavior and appearance of your shell.

Bash allows you to configure different settings using startup files and shell commands.

For example:

```bash
alias ll='ls -lah'
```

This creates a shortcut called `ll`.

Whenever you type:

```bash
ll
```

Bash runs:

```bash
ls -lah
```

You can also change the appearance of your terminal prompt.

Example:

```bash
PS1='\u@\h:\w\$ '
```

This changes how Bash displays the command prompt.

## 3. Understanding Bash Startup Files

Bash reads certain configuration files when it starts.

These files contain commands that configure your shell environment.

Important files include:

| File | Purpose |
|---|---|
| `/etc/profile` | System-wide login-shell configuration |
| `~/.bash_profile` | User-specific login-shell configuration |
| `~/.bash_login` | Alternative login configuration |
| `~/.profile` | User login configuration |
| `~/.bashrc` | User-specific interactive Bash configuration |
| `/etc/bash.bashrc` | System-wide interactive Bash configuration on Debian-based systems |

The exact files used depend on how Bash is started and how the system is configured.

### 3.1 Understanding `.bashrc`

The `.bashrc` file is commonly used to configure interactive non-login Bash shells.

It can contain:
- Aliases.
- Shell functions.
- Prompt settings.
- Environment settings for interactive sessions.
- Command history settings.

Inspect it:

```bash
cat ~/.bashrc
```

You can also open it with a text editor:

```bash
nano ~/.bashrc
```

For this lesson, we will create a separate practice configuration file rather than changing your existing `.bashrc` immediately.

### 3.2 Understanding `.profile`

The `.profile` file is commonly used for user-specific login environment configuration.

It can define environment variables and other commands needed during login.

Inspect it:

```bash
cat ~/.profile
```

It may not exist on every installation.

### 3.3 Understanding `.bash_profile`

Bash checks certain files when starting as a login shell.

The usual preference order is:

1. `~/.bash_profile`
2. `~/.bash_login`
3. `~/.profile`

Bash reads the first available file in that order.

A `.bash_profile` can also be configured to load `.bashrc`.

For example:

```bash
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi
```

The dot command sources the file into the current shell.

### 3.4 Login shells and interactive shells

A login shell is started as a login environment.

An interactive shell is a shell that accepts commands directly from the user.

These are not the same thing.

A shell can be:
- Interactive and login.
- Interactive and non-login.
- Non-interactive.

To inspect whether Bash considers the current shell interactive:

```bash
case $- in
    *i*) echo "Interactive shell" ;;
    *) echo "Non-interactive shell" ;;
esac
```

To inspect whether the current Bash shell is a login shell:

```bash
shopt -q login_shell && echo "Login shell" || echo "Non-login shell"
```

## 4. Understanding the Bash Prompt

The Bash prompt is the text displayed when the shell is ready to receive a command.

Example:

```text
kali@kali:~$
```

The prompt is controlled by the `PS1` variable.

### Common prompt escape sequences

| Symbol | Meaning |
|---|---|
| `\u` | Current username |
| `\h` | Hostname |
| `\H` | Full hostname |
| `\w` | Current working directory |
| `\W` | Current directory name |
| `\d` | Current date |
| `\t` | Current time |
| `\$` | Displays `$` for a normal user or `#` for root |

### Example 1: Basic prompt

```bash
PS1='\u@\h:\w\$ '
```

Example:

```text
kali@kali:~/Documents$
```

### Example 2: Username and current directory

```bash
PS1='\u:\w\$ '
```

### Example 3: Show the hostname

```bash
PS1='\h:\w\$ '
```

### Important

Changing `PS1` directly in the terminal normally affects only the current shell session.

It does not automatically make the change permanent.

Permanent configuration can be placed in an appropriate startup file.

## 5. Understanding Terminal Colors

Bash supports terminal color sequences using ANSI escape codes.

A common format is:

```bash
\033[COLOR_CODEm
```

For example:

```bash
echo -e "\033[32mGreen text\033[0m"
```

This displays green text in terminals that support ANSI colors.

### Common color codes

| Code | Color |
|---|---|
| 30 | Black |
| 31 | Red |
| 32 | Green |
| 33 | Yellow |
| 34 | Blue |
| 35 | Magenta |
| 36 | Cyan |
| 37 | White |

The code `0` resets the formatting.

Example:

```bash
echo -e "\033[31mWARNING\033[0m"
echo -e "\033[32mSYSTEM OK\033[0m"
echo -e "\033[36mLINUX TERMINAL\033[0m"
```

Colors can make important information easier to identify.

## 6. Understanding Aliases

An alias is a shortcut for a command or command sequence.

Aliases are useful when you repeatedly execute long commands.

### Create an alias

```bash
alias ll='ls -lah'
```

Now execute:

```bash
ll
```

Bash runs the command associated with the alias.

### More examples

```bash
alias cls='clear'
alias update='sudo apt update'
alias ports='ss -tuln'
alias myip='ip address'
```

These are examples, not commands you must permanently install.

### View existing aliases

```bash
alias
```

### View one alias

```bash
alias ll
```

### Remove an alias

```bash
unalias ll
```

### Important limitation

Aliases are primarily intended for interactive shell use.

They are not normally expanded in non-interactive Bash scripts.

For scripts, use functions or executable programs instead.

## 7. Making Aliases Persistent

An alias created directly in the terminal usually disappears when that shell exits.

To make it persistent for interactive Bash sessions, you can place it in `~/.bashrc`.

Example:

```bash
alias ll='ls -lah'
alias cls='clear'
alias ports='ss -tuln'
```

When Bash reads the configuration file, the aliases become available.

You can reload the file in your current shell:

```bash
source ~/.bashrc
```

The `source` command executes the contents of a file in the current shell environment.

**Security warning:** Only source configuration files that you trust. Sourcing a file can execute the commands it contains.

## 8. Understanding Shell Functions

A shell function is a reusable group of commands.

Functions are more flexible than simple aliases.

### Basic function syntax

```bash
function_name() {
    commands
}
```

Example:

```bash
greet() {
    echo "Welcome to Linux"
}
```

Execute it:

```bash
greet
```

Output:

```text
Welcome to Linux
```

### Function with an argument

```bash
greet() {
    echo "Hello, $1"
}
```

Execute:

```bash
greet Abbas
```

Output:

```text
Hello, Abbas
```

### Function with multiple commands

```bash
system_info() {
    echo "SYSTEM INFORMATION"
    echo "Username: $(whoami)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
}
```

Execute:

```bash
system_info
```

### Remove a function

```bash
unset -f system_info
```

### Why are functions useful?

Functions allow you to:
- Reuse commands.
- Organize shell operations.
- Avoid repeating code.
- Build small administrative utilities.
- Simplify interactive workflows.

## 9. Understanding Command History

Bash stores a history of commands entered by the user.

This helps you review and reuse previous commands.

### Display command history

```bash
history
```

### Display the last ten commands

```bash
history 10
```

### Search command history

Press:

```text
Ctrl + R
```

Then type part of a previous command.

Bash searches backward through your command history.

### Execute a previous command

The following command executes the command numbered 20 in the current history list:

```bash
!20
```

Be careful when using history expansion because it can execute commands immediately.

### Important history variables

| Variable | Purpose |
|---|---|
| `HISTFILE` | File used to store command history |
| `HISTSIZE` | Number of commands kept in the current shell's history list |
| `HISTFILESIZE` | Maximum number of lines stored in the history file |

Inspect them:

```bash
echo "$HISTFILE"
echo "$HISTSIZE"
echo "$HISTFILESIZE"
```

### Inspect the history file

```bash
cat ~/.bash_history
```

The file may not contain every command from your currently running shell until history is written.

### Security consideration

Command history may contain sensitive information.

Avoid typing passwords, API keys, or other secrets directly into commands because they may be saved in history or exposed through process information.

## 10. Understanding Environment Configuration

Shell configuration files can define environment variables.

Example:

```bash
export EDITOR=nano
```

This tells programs that use the `EDITOR` environment variable to prefer Nano.

Inspect it:

```bash
echo "$EDITOR"
```

You can also configure PATH.

Example:

```bash
export PATH="$HOME/bin:$PATH"
```

This adds the user's `bin` directory to the beginning of PATH.

Before adding a directory to PATH, make sure it is trusted and appropriately protected from unauthorized modification.

### Temporary versus persistent configuration

Temporary:

```bash
export EDITOR=nano
```

This affects the current shell and its child processes.

Persistent:

Add the command to a suitable startup file, such as `~/.bashrc` for interactive Bash sessions.

## 11. Using a Separate Practice Configuration File

Instead of changing your existing configuration, create a dedicated practice file.

This is safer and makes it easier to understand what each command does.

Create a working directory:

```bash
mkdir -p ~/shell-config-lab
cd ~/shell-config-lab
```

Create a file:

```bash
nano practice.bashrc
```

Enter:

```bash
# Practice Bash configuration

# Custom aliases
alias ll='ls -lah'
alias cls='clear'

# Environment variable
export EDITOR=nano

# Custom prompt
PS1='\u@\h:\w\$ '

# Simple function
system_info() {
    echo "SYSTEM INFORMATION"
    echo "User: $(whoami)"
    echo "Host: $(hostname)"
    echo "Kernel: $(uname -r)"
}
```

Save the file.

Load it into the current shell:

```bash
source ~/shell-config-lab/practice.bashrc
```

Test the alias:

```bash
ll
```

Test the function:

```bash
system_info
```

Inspect the editor variable:

```bash
echo "$EDITOR"
```

You have now practiced shell customization without modifying your normal `.bashrc`.

## 12. Security Considerations

Shell configuration is powerful, but it can also introduce security risks.

### 12.1 Malicious startup files

A malicious command in a startup file may execute automatically when a shell starts.

Only use configuration files from trusted sources.

### 12.2 Unsafe PATH configuration

An attacker may exploit an unsafe executable directory in PATH.

Do not place untrusted or writable directories ahead of trusted system directories, especially in privileged environments.

### 12.3 Sensitive command history

History files can contain information that should not be exposed.

Avoid entering secrets directly into shell commands.

### 12.4 Untrusted aliases and functions

Aliases and functions can change what commands appear to do.

Inspect unfamiliar shell configuration before using it.

### 12.5 Avoid unnecessary root privileges

Do not run shell configuration commands as root unless system-wide changes are genuinely required.

For this course, use your normal Kali user account for the exercises.

## 13. Practical Lab: Shell Configuration and Customization

### Lab objective

Learn how to customize Bash using aliases, functions, prompt settings, and environment variables.

### Step 1: Inspect your current shell

```bash
echo "$SHELL"
```

Check whether it is interactive:

```bash
case $- in
    *i*) echo "Interactive shell" ;;
    *) echo "Non-interactive shell" ;;
esac
```

### Step 2: Inspect your startup files

```bash
ls -la ~
```

Look for:

- `.bashrc`
- `.profile`
- `.bash_profile`
- `.bash_history`

Some files may not exist.

### Step 3: Inspect existing aliases

```bash
alias
```

### Step 4: Create a temporary alias

```bash
alias ll='ls -lah'
```

Test:

```bash
ll
```

### Step 5: Remove the alias

```bash
unalias ll
```

### Step 6: Create a shell function

```bash
system_info() {
    echo "SYSTEM INFORMATION"
    echo "User: $(whoami)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
    echo "Architecture: $(uname -m)"
}
```

Execute:

```bash
system_info
```

### Step 7: Customize the prompt temporarily

```bash
PS1='\u@\h:\w\$ '
```

Observe the new prompt.

You can open a fresh terminal session to return to the normal prompt.

### Step 8: Inspect command history

```bash
history 10
```

### Step 9: Inspect history configuration

```bash
echo "$HISTFILE"
echo "$HISTSIZE"
echo "$HISTFILESIZE"
```

### Step 10: Create your practice configuration file

```bash
mkdir -p ~/shell-config-lab
nano ~/shell-config-lab/practice.bashrc
```

Enter:

```bash
# Linux for Ethical Hackers
# Practice shell configuration

alias ll='ls -lah'
alias cls='clear'

export EDITOR=nano

system_info() {
    echo "SYSTEM INFORMATION"
    echo "User: $(whoami)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
}
```

Save the file.

### Step 11: Load the configuration

```bash
source ~/shell-config-lab/practice.bashrc
```

Test:

```bash
ll
system_info
echo "$EDITOR"
```

### Step 12: Inspect the practice file

```bash
cat ~/shell-config-lab/practice.bashrc
```

Understand every command you added.

Do not place this practice file into an automatic startup location yet.

## 14. Knowledge Check

Answer these questions in your notebook.

1. What is shell configuration?
2. What is the purpose of `.bashrc`?
3. What is the purpose of `.profile`?
4. What is a login shell?
5. What is an interactive shell?
6. What does the `PS1` variable control?
7. What is an alias?
8. What does `unalias` do?
9. What is a shell function?
10. How is a function different from an alias?
11. What does the `history` command display?
12. What is the purpose of `HISTFILE`?
13. What does `source` do?
14. How can a malicious startup file compromise a system?
15. Why should PATH be configured carefully?

## 15. Practical Assignment

Create a Bash configuration file named:

`ethical-hacker-config.sh`

Your file must contain:

1. A comment identifying the configuration.
2. At least three useful aliases.
3. An environment variable named `EDITOR`.
4. A custom Bash prompt setting.
5. A function called `system_info`.
6. A function called `network_info`.
7. A function called `show_date`.
8. Clear comments explaining each section.

### Suggested structure

```bash
#!/bin/bash

# Linux for Ethical Hackers
# Custom Bash Configuration

# Aliases
alias ll='ls -lah'
alias cls='clear'
alias ports='ss -tuln'

# Environment configuration
export EDITOR=nano

# Custom prompt
PS1='\u@\h:\w\$ '

# System information function
system_info() {
    echo "SYSTEM INFORMATION"
    echo "User: $(whoami)"
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
}

# Network information function
network_info() {
    echo "NETWORK INFORMATION"
    ip address
}

# Date function
show_date() {
    date
}
```

Load the file:

```bash
source ethical-hacker-config.sh
```

Test all three functions and your aliases.

**Important:** Keep this as a practice file. Do not automatically execute it every time your shell starts until you understand and have reviewed every command.

## 16. Key Terms

| Term | Definition |
|---|---|
| Shell configuration | Customizing shell behavior |
| `.bashrc` | Interactive Bash configuration |
| `.profile` | User login configuration |
| `.bash_profile` | Bash login configuration |
| PS1 | Primary Bash prompt variable |
| Alias | Shortcut for a command |
| Function | Reusable group of shell commands |
| `source` | Executes a file in the current shell |
| `history` | Displays previously entered commands |
| HISTFILE | Bash history file setting |
| PATH | Executable search directories |
| EDITOR | Preferred text editor setting |

## 17. Exam Summary

Remember these important points:

- Bash can be customized using startup files.
- `.bashrc` commonly configures interactive non-login Bash shells.
- `.profile` is commonly used for login configuration.
- Login shells and interactive shells are different concepts.
- `PS1` controls the primary command prompt.
- Aliases create command shortcuts.
- Functions group commands for reuse.
- `history` displays previous commands.
- `source` loads commands into the current shell.
- Environment variables can be configured in startup files.
- Unsafe startup files can execute malicious commands.
- PATH should contain trusted executable directories.

## 18. Conclusion

Shell configuration and customization allow you to build a Linux environment that suits your workflow.

You have learned how to configure aliases, functions, prompt appearance, command history, and environment variables.

These skills will help you work more efficiently when administering Linux systems and performing authorized cybersecurity exercises.

**Next Lesson: Lesson 15 — Module 04 Assessment.**