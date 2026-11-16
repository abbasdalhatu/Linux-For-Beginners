# Lesson 09: The `clear` and `history` Commands

## 1. Introduction

When working in Linux, you may execute dozens or even hundreds of commands in a single terminal session.

Over time, the terminal can become crowded with output, making it difficult to follow your work.

Linux provides commands that help manage the terminal environment and review previously executed commands.

Two important commands are:

1. `clear`
2. `history`

- `clear` clears the visible terminal screen.
- `history` displays commands previously entered into the shell.

These commands are useful for productivity, troubleshooting, administration, and cybersecurity investigations.

## 2. Understanding the `clear` Command

The `clear` command clears the visible terminal display.

### Basic Syntax

```bash
clear
```

Example:

```bash
echo "First command"
echo "Second command"
echo "Third command"
clear
```

After executing `clear`, the previous terminal output is no longer visible in the normal terminal display.

**Important:** `clear` does not delete files, remove command history, or erase system logs. It only clears the terminal display.

## 3. How `clear` Works

Terminal emulators use control sequences to manage screen output.

The `clear` command uses the terminal's configured capabilities to clear the display.

You can inspect your terminal type using:

```bash
echo "$TERM"
```

The output may look like:

```text
xterm-256color
```

The exact value depends on your terminal environment.

You can also inspect the terminal capability information using:

```bash
infocmp
```

This command displays terminal capability data when the relevant terminfo entry is available.

## 4. Alternative Ways to Clear the Terminal

### 4.1 Using Ctrl + L

Press:

```text
Ctrl + L
```

This is a common Bash and terminal shortcut for clearing or refreshing the visible terminal display.

### 4.2 Using the `clear` Command

```bash
clear
```

Both methods are commonly used to make the terminal easier to read.

## 5. Understanding the `history` Command

The `history` command displays commands previously entered into the current shell's history list.

### Basic Syntax

```bash
history
```

Example output:

```text
1  pwd
2  ls
3  cd /etc
4  clear
5  whoami
```

The numbers represent history event numbers.

The history list can help you remember commands and repeat previous work.

## 6. Displaying a Limited Number of Commands

Sometimes your history contains many commands.

To display only the most recent entries:

```bash
history 10
```

This displays up to the last ten entries.

You can change the number:

```bash
history 20
history 50
```

This is useful when reviewing recent terminal activity.

## 7. Searching Command History

You can search the history output using `grep`.

Example:

```bash
history | grep "sudo"
```

This displays history entries containing the word `sudo`.

Another example:

```bash
history | grep "apt"
```

This can help you locate previous package-management commands.

You can also search for a command interactively using:

```text
Ctrl + R
```

Type part of a previously used command to search backward through your Bash history.

Press `Ctrl + R` again to find an earlier match.

Press Enter to execute the displayed command, or use the arrow keys to edit it before running.

**Safety note:** Always inspect a recalled command before executing it, especially if it modifies files, changes permissions, or uses administrative privileges.

## 8. Repeating Previous Commands

Bash provides history expansion features for recalling commands.

### 8.1 Repeat the Previous Command

```bash
!!
```

This executes the previous command again.

For example:

```bash
echo "Linux"
!!
```

The second command repeats the previous `echo` command.

### 8.2 Execute a Command by History Number

Suppose your history contains:

```text
10  pwd
11  ls
12  whoami
```

You can execute event 11 using:

```bash
!11
```

This runs the command associated with history number 11.

### 8.3 Repeat the Most Recent Matching Command

```bash
!ls
```

This attempts to execute the most recent command beginning with `ls`.

**Important:** History expansion can execute commands immediately. Do not use these shortcuts blindly.

## 9. Understanding Bash History Storage

Bash can save command history in a file.

The common history file is:

```text
~/.bash_history
```

You can inspect it using:

```bash
cat ~/.bash_history
```

The file may contain commands from previous shell sessions.

However, the current shell's newest commands may not yet have been written to the file.

Bash commonly writes its session history when the shell exits.

The exact behaviour depends on your Bash configuration.

## 10. Important History Environment Variables

Bash uses environment variables to control history behaviour.

### 10.1 HISTFILE

Specifies the file used to store command history.

```bash
echo "$HISTFILE"
```

A common value is:

```text
/home/kali/.bash_history
```

### 10.2 HISTSIZE

Controls how many commands Bash keeps in the current session's in-memory history list.

```bash
echo "$HISTSIZE"
```

### 10.3 HISTFILESIZE

Controls the maximum number of history entries kept in the history file.

```bash
echo "$HISTFILESIZE"
```

These values depend on your Bash configuration.

## 11. Clearing Command History

There is an important difference between clearing the terminal and clearing command history.

### 11.1 Clear the Visible Terminal

```bash
clear
```

This does not remove command history.

### 11.2 Clear the Current Session's History List

```bash
history -c
```

This clears the current shell's in-memory history list.

Depending on Bash configuration and later shell operations, history-file contents may still exist or be written again.

### 11.3 Delete the History File

The Bash history file is commonly:

```text
~/.bash_history
```

Deleting it is different from clearing the terminal display.

Do not delete or alter shell history to conceal unauthorised activity. In cybersecurity work, preserve relevant evidence and follow the applicable investigation procedures.

**Important:** Clearing local shell history does not erase system logs, audit records, remote logs, or records maintained by other services.

## 12. Practical Lab: Using `clear`

Perform these exercises in Kali Linux.

### Step 1: Display Several Messages

```bash
echo "Linux Practice"
echo "Learning Terminal Commands"
echo "Cybersecurity Training"
```

### Step 2: Clear the Terminal

```bash
clear
```

Observe the result.

### Step 3: Display Your Terminal Type

```bash
echo "$TERM"
```

### Step 4: Use the Keyboard Shortcut

Press:

```text
Ctrl + L
```

Observe how the terminal display changes.

## 13. Practical Lab: Using `history`

### Step 1: Execute Some Commands

```bash
pwd
whoami
ls
date
uname -a
```

### Step 2: Display Your History

```bash
history
```

Observe the command numbers.

### Step 3: Display the Last Ten Commands

```bash
history 10
```

### Step 4: Search for a Specific Command

```bash
history | grep "ls"
```

### Step 5: Check Your History File

```bash
echo "$HISTFILE"
```

Then inspect it:

```bash
cat "$HISTFILE"
```

The file may not contain commands from your current session that have not yet been saved.

### Step 6: Check History Settings

```bash
echo "$HISTSIZE"
echo "$HISTFILESIZE"
```

Observe the values.

## 14. Common Mistakes

### Mistake 1: Thinking `clear` Deletes History

Incorrect assumption:

```bash
clear
```

does not delete previously entered commands.

Use `history` to inspect the shell's history list.

### Mistake 2: Assuming All Commands Are Saved Immediately

Some recent commands may exist only in the current shell's memory until history is written.

### Mistake 3: Running Recalled Commands Without Checking Them

History shortcuts such as:

```bash
!!
```

and:

```bash
!15
```

can execute commands immediately.

Inspect commands before repeating them.

### Mistake 4: Confusing Shell History with System Logs

Shell history records commands entered into a shell, subject to configuration.

System logs record other kinds of system events.

They serve different purposes.

## 15. Cybersecurity Importance

### 15.1 Reviewing Previous Administrative Commands

During troubleshooting, administrators can inspect previous commands to understand what they did.

### 15.2 Investigating Command Activity

Shell history can provide useful context during an authorised investigation.

For example:

```bash
history | grep "useradd"
```

This may reveal recorded attempts to create user accounts.

However, history is incomplete and can be modified. It is not definitive proof of every action performed on a system.

### 15.3 Troubleshooting

You can search for previously executed commands to identify mistakes or repeat known procedures.

### 15.4 Understanding Operational Activity

History can help you reconstruct parts of a command-line workflow.

For reliable security investigations, correlate shell history with system logs, audit records, file timestamps, and other available evidence.

### 15.5 Protecting Sensitive Information

Avoid entering passwords, API keys, access tokens, or other secrets directly as command-line arguments because they may be exposed through shell history or process information.

Use safer secret-handling methods appropriate to the tool.

## 16. Important Commands Summary

| Command | Purpose |
|---|---|
| `clear` | Clear the visible terminal screen |
| `Ctrl + L` | Clear or refresh the terminal display |
| `history` | Display shell command history |
| `history 10` | Display the last ten history entries |
| `history \| grep "sudo"` | Search history for matching entries |
| `!!` | Repeat the previous command |
| `!10` | Execute history event 10 |
| `!ls` | Execute the most recent command beginning with `ls` |
| `echo "$HISTFILE"` | Display the history file location |
| `echo "$HISTSIZE"` | Display the in-memory history limit |
| `echo "$HISTFILESIZE"` | Display the history-file limit |
| `history -c` | Clear the current shell's in-memory history list |

## 17. Knowledge Check

Answer these questions in your notebook.

1. What is the purpose of the `clear` command?
2. Does `clear` delete command history?
3. What keyboard shortcut commonly clears the terminal display?
4. What is the purpose of the `history` command?
5. How can you display only the last ten commands?
6. What does `Ctrl + R` do in Bash?
7. What does `!!` do?
8. What is the purpose of the `HISTFILE` variable?
9. What is the difference between `HISTSIZE` and `HISTFILESIZE`?
10. Why should shell history not be treated as complete forensic evidence?

## 18. Practical Assignment

Complete these tasks in Kali Linux.

1. Execute at least five Linux commands.
2. Clear your terminal using `clear`.
3. Clear it again using `Ctrl + L`.
4. Display your command history.
5. Display the last ten history entries.
6. Search your history for commands containing `cd`.
7. Use `Ctrl + R` to search for a previous command.
8. Display the location of your Bash history file.
9. Inspect your history configuration variables.
10. Explain the difference between shell history and system logs.
11. Write down three reasons why command history is useful during authorised cybersecurity investigations.

## 19. Lesson Conclusion

The `clear` and `history` commands help users manage their terminal environment and review previous command activity.

You have learned how to:

- Clear the terminal display.
- Review previous commands.
- Search command history.
- Repeat previous commands.
- Understand Bash history storage.
- Recognise the limitations of shell history during investigations.

**Key lesson:** A clean terminal improves readability, while command history helps you understand and review your previous work.

---

**End of Lesson 09 — The `clear` and `history` Commands**