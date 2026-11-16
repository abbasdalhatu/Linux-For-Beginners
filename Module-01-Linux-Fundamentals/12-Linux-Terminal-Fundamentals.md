# Module 01: Linux Fundamentals

# Lesson 12 – Linux Terminal Fundamentals

**Course:** Linux for Ethical Hackers  
**Module:** 01 – Introduction to Linux  
**Lesson:** 12 of 15  
**Level:** Beginner to Intermediate

---

## 1. Introduction

The Linux terminal is one of the most important tools for anyone learning Linux administration, software development, or cybersecurity.

It provides a text-based environment where users can communicate with the operating system through commands.

In the previous lesson, you learned about the Command Line Interface (CLI), command structures, options, arguments, pipes, and redirection.

In this lesson, we will go deeper into the practical use of the Linux terminal.

You will learn how to:

- Understand terminal sessions.
- Navigate the Linux filesystem.
- Use command history.
- Work with command completion.
- Manage terminal processes.
- Understand environment variables.
- Use terminal shortcuts.
- Inspect terminal and shell information.
- Apply terminal skills to cybersecurity tasks.

## 2. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain what a Linux terminal is.
2. Distinguish between a terminal, shell, and command.
3. Understand Linux terminal prompts.
4. Navigate the filesystem confidently.
5. Use command history and autocomplete.
6. Manage foreground and background processes.
7. Understand environment variables.
8. Use important terminal keyboard shortcuts.
9. Inspect terminal sessions and shell information.
10. Apply terminal skills in cybersecurity.

## 3. Understanding the Linux Terminal

A terminal is an application that provides access to a command-line environment.

It allows users to enter commands and view the results.

In Kali Linux, you can open the terminal through the applications menu or a configured keyboard shortcut.

### Terminal, Shell, and Command

These three concepts are related, but they are not the same.

| Concept | Meaning |
|---|---|
| Terminal | Application that provides the text interface |
| Shell | Program that interprets commands |
| Command | Instruction executed by the shell |

### Example

When you open the Kali terminal:

1. The terminal application starts.
2. The shell starts inside the terminal.
3. The shell displays a prompt.
4. You type a command.
5. The shell interprets the command.
6. The requested operation is performed.
7. The output appears in the terminal.

A common shell in Linux is Bash.

## 4. Understanding the Terminal Prompt

When you open a Kali Linux terminal, you may see something similar to:

```text
kali@kali:~$
```

Let's examine its components.

| Component | Explanation |
|---|---|
| `kali` | Username |
| `@` | Separates username and hostname |
| `kali` | Hostname |
| `~` | Home directory shorthand |
| `$` | Common ordinary-user prompt symbol |

A root shell commonly displays:

```text
root@kali:~#
```

The `#` symbol is commonly used for a root prompt.

**Important:** Prompt appearance can be customised. Use `whoami` to verify your current identity.

## 5. Understanding the Linux Filesystem

Linux organises files and directories in a hierarchical structure.

The top of this structure is called the root directory.

It is represented by:

```text
/
```

Unlike Windows, which commonly uses drive letters such as `C:\`, Linux organises its filesystem beneath a single root directory.

### Important Linux directories

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/home` | Ordinary users' home directories |
| `/root` | Root user's home directory |
| `/etc` | System configuration files |
| `/bin` | Essential command executables; may be linked into `/usr/bin` |
| `/usr` | User-space programs, libraries and shared resources |
| `/var` | Variable data such as logs and caches |
| `/tmp` | Temporary files |
| `/dev` | Device interfaces |
| `/proc` | Virtual filesystem exposing process and kernel information |
| `/sys` | Virtual filesystem exposing device and kernel information |
| `/boot` | Boot-related files |

### Example filesystem structure

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
│   └── kali
├── lib
├── proc
├── root
├── sys
├── tmp
├── usr
└── var
```

This is a simplified example. The exact structure may differ between Linux distributions.

### Practical inspection

Run:

```bash
ls /
```

This displays entries in the root directory.

Inspect the home directory:

```bash
ls /home
```

Check your current directory:

```bash
pwd
```

## 6. Navigating the Terminal

Navigation means moving between directories.

The most important navigation commands are `pwd`, `ls`, and `cd`.

### 6.1 Print Working Directory

```bash
pwd
```

Displays your current location.

### 6.2 List Directory Contents

```bash
ls
```

Displays files and directories.

Show hidden entries:

```bash
ls -a
```

Show detailed information:

```bash
ls -l
```

### 6.3 Change Directory

Enter Documents:

```bash
cd Documents
```

Return to your home directory:

```bash
cd ~
```

Move one directory up:

```bash
cd ..
```

Move to the filesystem root:

```bash
cd /
```

### 6.4 Navigate Using Absolute Paths

```bash
cd /home/kali/Documents
```

This uses an absolute path beginning from `/`.

### 6.5 Navigate Using Relative Paths

```bash
cd Documents
```

This uses a path relative to your current location.

### 6.6 Return to the Previous Directory

```bash
cd -
```

This switches to the previous working directory.

## 7. Hidden Files and Directories

Linux files whose names begin with a dot are normally treated as hidden by standard directory listings.

Examples:

```text
.bashrc
.profile
.ssh
.config
```

Hidden does not mean encrypted or protected.

It simply means ordinary `ls` listings do not display these entries unless requested.

Display hidden files:

```bash
ls -a
```

Display detailed information, including hidden files:

```bash
ls -la
```

Inspect your Bash configuration:

```bash
cat ~/.bashrc
```

Be careful when editing configuration files because incorrect changes can affect your shell environment.

## 8. Command History

Linux shells can remember commands entered during a session and, depending on their configuration, across sessions.

Command history is useful because it allows you to reuse commands without typing them again.

### Display command history

```bash
history
```

Display the most recent commands:

```bash
history 10
```

Search your command history:

```bash
history | grep nmap
```

This searches the displayed history for entries containing `nmap`.

### Reuse previous commands

Press the Up Arrow key to move backward through previous commands.

Press the Down Arrow key to move forward.

You can also execute a command by its history number:

```bash
!25
```

This runs history entry number 25.

**Warning:** History expansion can execute a command immediately. Check the command before using this feature.

## 9. Command Autocompletion

Autocompletion helps users enter commands and filenames more quickly.

In Bash, pressing the Tab key attempts to complete a command, filename, or directory name.

### Example

Suppose you have a directory named:

```text
Cybersecurity
```

Instead of typing the entire name, enter:

```bash
cd Cyber
```

Then press Tab.

If the match is unambiguous, Bash may complete the directory name.

If multiple entries match, pressing Tab twice may display possible matches.

### Why autocomplete matters

- Reduces typing.
- Helps avoid spelling mistakes.
- Makes long paths easier to enter.
- Improves terminal efficiency.

## 10. Important Terminal Keyboard Shortcuts

Keyboard shortcuts make terminal work faster.

| Shortcut | Function |
|---|---|
| `Ctrl + C` | Sends an interrupt signal to the foreground process |
| `Ctrl + Z` | Suspends the foreground process |
| `Ctrl + D` | Sends end-of-file input; may exit an interactive shell |
| `Ctrl + L` | Clears the visible terminal screen |
| `Ctrl + A` | Moves the cursor to the beginning of the line |
| `Ctrl + E` | Moves the cursor to the end of the line |
| `Ctrl + U` | Removes text before the cursor on the current line |
| `Ctrl + K` | Removes text after the cursor on the current line |
| `Ctrl + W` | Removes the previous word |
| `Up Arrow` | Displays the previous command |
| `Down Arrow` | Moves forward through command history |
| `Tab` | Attempts command or filename completion |

These shortcuts are commonly supported by Bash's line-editing interface.

## 11. Understanding Foreground and Background Processes

A process is a running instance of a program.

When you execute a command in the terminal, it normally runs in the foreground.

This means the terminal waits for the command to finish before returning to the prompt.

### Foreground process

Example:

```bash
sleep 30
```

The command waits for 30 seconds.

During this time, the terminal is occupied by that foreground process.

### Interrupting a process

Press:

```text
Ctrl + C
```

This sends an interrupt signal, commonly SIGINT, to the foreground process.

### Suspending a process

Run:

```bash
sleep 100
```

Press:

```text
Ctrl + Z
```

The shell suspends the foreground job.

### Display suspended and background jobs

```bash
jobs
```

### Resume a suspended job in the foreground

```bash
fg
```

### Resume a suspended job in the background

```bash
bg
```

### Start a command directly in the background

```bash
sleep 60 &
```

The `&` symbol tells the shell to start the command as a background job.

**Important:** Background jobs are still processes running under your user account. They do not automatically become independent system services.

## 12. Environment Variables

Environment variables are named values that provide information to processes and programs.

They can store settings such as:

- Current username.
- Home directory.
- Shell location.
- Command search paths.
- Language preferences.

### Display all environment variables

```bash
env
```

Another command is:

```bash
printenv
```

### Display a specific variable

```bash
echo "$USER"
```

Displays the current username.

```bash
echo "$HOME"
```

Displays the home directory.

```bash
echo "$SHELL"
```

Displays the configured login shell path.

```bash
echo "$PATH"
```

Displays directories searched by the shell when locating executable commands.

### Create a temporary shell variable

```bash
COURSE="Linux for Ethical Hackers"
```

Display it:

```bash
echo "$COURSE"
```

### Export a variable

```bash
export COURSE="Linux for Ethical Hackers"
```

Exporting makes the variable available to child processes started by that shell.

Environment variables are useful for configuration and automation.

## 13. Understanding the PATH Variable

The PATH environment variable contains a list of directories that the shell searches when a command is entered without an explicit path.

Display it:

```bash
echo "$PATH"
```

Example output may look like:

```text
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

The colon separates individual directories.

### Why PATH matters

When you execute:

```bash
ls
```

The shell searches directories listed in PATH to locate the executable, unless `ls` is already defined as an alias, function, or built-in.

Find the executable location:

```bash
command -v ls
```

This helps you understand how commands are located.

**Security note:** An unsafe PATH configuration can cause an unintended program to execute. Avoid adding untrusted or writable directories to PATH, especially before system directories.

## 14. Terminal Sessions

A terminal session is an active command-line environment associated with a terminal and shell process.

You can inspect the current terminal information using:

```bash
tty
```

This may display something similar to:

```text
/dev/pts/0
```

The exact number depends on the terminal session.

### Display current user

```bash
whoami
```

### Display logged-in users

```bash
who
```

### Display system uptime

```bash
uptime
```

### Display the current shell process

```bash
ps -p $$ -o pid,ppid,comm
```

This displays information about the current shell process.

### Display terminal information

```bash
echo "$TERM"
```

The TERM variable identifies the terminal type or capabilities description used by applications.

## 15. Terminal Skills in Cybersecurity

The Linux terminal is essential for many cybersecurity activities.

### 15.1 System Enumeration

System enumeration means collecting information about a system.

Examples:

```bash
whoami
```

```bash
hostname
```

```bash
uname -a
```

```bash
id
```

These commands help identify the current user, machine name, operating-system kernel information, and user/group identities.

### 15.2 Network Inspection

Inspect network interfaces:

```bash
ip addr
```

Inspect listening sockets:

```bash
ss -tuln
```

Check connectivity to your own local system:

```bash
ping -c 4 127.0.0.1
```

### 15.3 Process Inspection

Display running processes:

```bash
ps aux
```

Display a live process monitor:

```bash
top
```

### 15.4 Log Inspection

Display recent system logs:

```bash
journalctl -n 20
```

### 15.5 File Inspection

Display file details:

```bash
ls -lah
```

Identify a file type:

```bash
file filename.txt
```

These operations help administrators and security analysts investigate system behaviour.

## 16. Practical Lab: Linux Terminal Fundamentals

**Lab objective:** Develop confidence using terminal navigation, command history, autocomplete, process management, environment variables, and system inspection.

Perform this lab inside your Kali Linux virtual machine.

### Part A – Open the terminal

Open Kali Linux and launch the terminal.

Run:

```bash
whoami
```

Then:

```bash
pwd
```

Then:

```bash
hostname
```

Record the results.

### Part B – Explore the filesystem

Run:

```bash
ls /
```

Then:

```bash
ls -la ~
```

Inspect the configuration directory:

```bash
ls /etc
```

Display your current location:

```bash
pwd
```

### Part C – Explore hidden files

Run:

```bash
ls -a ~
```

Look for hidden configuration files such as `.bashrc`.

Inspect the file:

```bash
cat ~/.bashrc
```

Do not modify it during this exercise.

### Part D – Practise command history

Execute these commands:

```bash
date
whoami
hostname
pwd
uname -a
```

Display your history:

```bash
history
```

Display the last five commands:

```bash
history 5
```

Practise using the Up and Down Arrow keys.

### Part E – Practise autocomplete

Run:

```bash
cd /home
```

Type:

```bash
cd ka
```

Press Tab and observe whether Bash completes the directory name.

Return to your home directory:

```bash
cd ~
```

### Part F – Practise terminal shortcuts

Run:

```bash
sleep 60
```

Press `Ctrl + C` to interrupt it.

Run it again:

```bash
sleep 100
```

Press `Ctrl + Z` to suspend it.

Display your jobs:

```bash
jobs
```

Resume it in the background:

```bash
bg
```

Display your jobs again:

```bash
jobs
```

Terminate the background job:

```bash
kill %1
```

If the job number differs, use the number shown by `jobs`.

### Part G – Explore environment variables

Run:

```bash
echo "$USER"
```

```bash
echo "$HOME"
```

```bash
echo "$SHELL"
```

```bash
echo "$PATH"
```

Create a temporary variable:

```bash
COURSE="Linux for Ethical Hackers"
```

Display it:

```bash
echo "$COURSE"
```

Export it:

```bash
export COURSE
```

Verify it:

```bash
printenv COURSE
```

### Part H – Inspect your terminal session

Run:

```bash
tty
```

Then:

```bash
echo "$TERM"
```

Inspect your shell process:

```bash
ps -p $$ -o pid,ppid,comm
```

### Part I – Perform basic system inspection

Run:

```bash
whoami
```

```bash
id
```

```bash
hostname
```

```bash
uname -a
```

```bash
ip addr
```

```bash
ss -tuln
```

Observe the information returned by each command.

### Part J – Finish the lab

Clear the terminal:

```bash
clear
```

Return to your home directory:

```bash
cd ~
```

You have completed the practical exercises for this lesson.

## 17. Important Terms

| Term | Meaning |
|---|---|
| Terminal | Application providing command-line access |
| Shell | Program that interprets commands |
| Prompt | Text indicating that the shell is ready for input |
| Filesystem | Organisation of files and directories |
| Hidden File | File whose name normally begins with a dot |
| Command History | Record of previously entered commands |
| Autocompletion | Automatic completion of commands or paths |
| Process | Running instance of a program |
| Foreground Job | Process currently associated with terminal interaction |
| Background Job | Process running without occupying the foreground terminal |
| Environment Variable | Named value available to processes |
| PATH | Directories searched for executable commands |
| Terminal Session | Active terminal and shell environment |

## 18. Knowledge Check

Answer these questions without looking at the lesson.

1. What is a Linux terminal?
2. Explain the difference between a terminal and a shell.
3. What does the `~` symbol represent?
4. What is the purpose of the root directory `/`?
5. What is a hidden file?
6. Which command displays command history?
7. What does pressing Tab do in Bash?
8. What is the purpose of `Ctrl + C`?
9. Explain the difference between foreground and background processes.
10. What does the `jobs` command display?
11. What is an environment variable?
12. What information is stored in the PATH variable?
13. What does the `tty` command display?
14. Which command identifies the current user?
15. Why are terminal skills important in cybersecurity?

## 19. Practical Assignment

Complete the following tasks:

1. Define the Linux terminal.
2. Explain the difference between a terminal, shell, and command.
3. Identify ten important Linux directories and explain their purposes.
4. Explain how command history works.
5. Describe how Bash autocomplete works.
6. Explain five important terminal keyboard shortcuts.
7. Demonstrate how to suspend and resume a process.
8. Define environment variables and provide four examples.
9. Explain the purpose of the PATH variable.
10. Explain how terminal commands assist cybersecurity professionals.

Save your assignment as:

`Linux-Terminal-Fundamentals-Assignment.md`

## 20. Exam-Ready Summary

The Linux terminal is an application that provides access to a command-line environment. It allows users to interact with the operating system through a shell, which interprets commands. Linux terminals support filesystem navigation, command history, autocomplete, process management, environment variables, and system inspection. Important commands include `pwd`, `ls`, `cd`, `history`, `jobs`, `whoami`, `tty`, and `ps`. Keyboard shortcuts such as Ctrl+C and Ctrl+Z help control foreground processes. Environment variables store information used by programs, while PATH identifies directories searched for executable commands. Terminal knowledge is essential for Linux administration, automation, and cybersecurity.

## 21. Lesson Conclusion

You have now learned how to work more confidently inside the Linux terminal.

You understand filesystem navigation, hidden files, command history, autocomplete, keyboard shortcuts, processes, environment variables, and terminal sessions.

These skills will become increasingly important as you move into Linux administration, scripting, networking, and ethical hacking.

**Next Lesson:** Lesson 13 – Understanding Root and Sudo.