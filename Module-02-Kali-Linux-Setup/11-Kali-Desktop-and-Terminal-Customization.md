---
---

# LESSON 11: KALI DESKTOP AND TERMINAL CUSTOMIZATION

**Module:** 02 – Kali Linux Setup  
**File:** `11-Kali-Desktop-and-Terminal-Customization.md`

## 1. Introduction

Kali Linux provides a graphical desktop environment that allows users to interact with applications, files, windows, and system settings.

The terminal provides a command-line interface for executing Linux commands.

In this lesson, we will explore the Kali desktop, understand the XFCE environment, customize the terminal, and learn basic shell configuration.

## 2. Understanding the Desktop Environment

A desktop environment (DE) provides the graphical interface of an operating system.

It includes:

- Desktop background.
- Application menu.
- Window manager.
- File manager.
- Panels and taskbars.
- System settings.
- Desktop utilities.

Kali Linux supports several desktop environments, including:

| Desktop environment | Description |
|---|---|
| XFCE | Lightweight and resource-efficient |
| GNOME | Modern graphical interface |
| KDE Plasma | Highly customizable desktop |

We are using XFCE because it works well with our laptop's available resources.

## 3. Explore the Kali Desktop

Log in to your Kali virtual machine.

Identify these components:

1. Applications menu.
2. Top or bottom panel.
3. Desktop background.
4. System notification area.
5. Network indicator.
6. File manager.
7. Terminal application.

Open the Applications menu and explore the available categories.

Do not launch unfamiliar security tools just to experiment with them.

## 4. Understanding the XFCE Terminal

The terminal allows users to interact with the Linux shell.

Open the terminal using:

`Ctrl + Alt + T`

A terminal prompt may look like:

```text
kaliuser@kali:~$
```

Explanation:

| Component | Meaning |
|---|---|
| `kaliuser` | Current username |
| `@` | Separates username and hostname |
| `kali` | Computer hostname |
| `~` | Current user's home directory |
| `$` | Normal user prompt |

The root account commonly uses `#` as its prompt symbol.

## 5. Basic Terminal Appearance Customization

You can change the appearance of the terminal without modifying the operating system.

In XFCE Terminal:

1. Open Terminal.
2. Open the Edit menu.
3. Select Preferences.
4. Explore Appearance.
5. Change the font size.
6. Explore the color settings.
7. Adjust the background transparency if supported.

Choose settings that make text easy to read.

Avoid extremely small fonts.

## 6. Understanding Shells

A shell is a program that interprets commands and allows users to interact with the operating system.

Common Linux shells include:

| Shell | Description |
|---|---|
| Bash | Bourne Again Shell |
| Zsh | Z Shell |
| Fish | Friendly Interactive Shell |
| Dash | Lightweight POSIX-compatible shell |

Kali commonly uses Bash as the default interactive shell, although configurations may vary.

Check your current shell:

```bash
echo $SHELL
```

Check your current shell process:

```bash
ps -p $$ -o comm=
```

## 7. Understanding Shell Configuration Files

Shell configuration files control how your shell behaves.

For Bash, important files include:

| File | Purpose |
|---|---|
| `~/.bashrc` | Configures interactive Bash sessions |
| `~/.bash_profile` | Login-shell configuration when present |
| `~/.profile` | General login environment configuration |
| `/etc/bash.bashrc` | System-wide interactive Bash configuration |

The symbol `~` represents your home directory.

Inspect your Bash configuration:

```bash
cat ~/.bashrc
```

You can also inspect your home directory:

```bash
ls -la ~
```

## 8. Understanding Environment Variables

Environment variables store information used by programs and the shell.

Examples include:

| Variable | Purpose |
|---|---|
| `HOME` | User's home directory |
| `USER` | Current username |
| `SHELL` | User's configured shell |
| `PATH` | Directories searched for executable commands |
| `PWD` | Current working directory |

Display your username:

```bash
echo $USER
```

Display your home directory:

```bash
echo $HOME
```

Display your command search path:

```bash
echo $PATH
```

Display your current directory:

```bash
echo $PWD
```

## 9. Creating a Custom Bash Alias

An alias is a shortcut for a command.

For example, instead of typing:

```bash
clear
```

You could create a shorter command:

```bash
alias c='clear'
```

Now execute:

```bash
c
```

The terminal should clear.

This alias is temporary. It disappears when the current shell session ends.

To make it persistent, open your Bash configuration file:

```bash
nano ~/.bashrc
```

Add this line at the bottom:

```bash
alias c='clear'
```

Save the file:

1. Press `Ctrl + O`.
2. Press Enter.
3. Press `Ctrl + X`.

Reload the configuration:

```bash
source ~/.bashrc
```

Now the alias should work in future interactive Bash sessions.

## 10. Create Useful Terminal Aliases

You can add these optional aliases to your `~/.bashrc`:

```bash
alias ll='ls -lah'
alias update='sudo apt update'
alias upgrade='sudo apt upgrade'
alias ports='ss -tuln'
alias myip='ip addr'
```

Explanation:

| Alias | Actual command |
|---|---|
| `ll` | `ls -lah` |
| `update` | `sudo apt update` |
| `upgrade` | `sudo apt upgrade` |
| `ports` | `ss -tuln` |
| `myip` | `ip addr` |

Reload your configuration:

```bash
source ~/.bashrc
```

Test:

```bash
ll
```

And:

```bash
ports
```

Remember that `ss -tuln` displays listening TCP and UDP sockets, not every network connection.

## 11. Customizing the Terminal Prompt

Bash uses the `PS1` environment variable to control the primary command prompt.

Inspect its current value:

```bash
echo "$PS1"
```

For a temporary experiment, run:

```bash
PS1='\u@\h:\w\$ '
```

The prompt components mean:

| Symbol | Meaning |
|---|---|
| `\u` | Username |
| `\h` | Hostname |
| `\w` | Current working directory |
| `\$` | `$` for normal users and `#` for root |

This change applies only to the current shell session unless you save it in a configuration file.

## 12. Understanding Terminal Safety

Customization should improve productivity without weakening security.

Follow these rules:

1. Do not run unknown commands copied from untrusted websites.
2. Avoid executing unknown scripts with `sudo`.
3. Do not place passwords or secret keys in aliases.
4. Back up configuration files before making major changes.
5. Understand commands before executing them.

You can create a backup of your Bash configuration using:

```bash
cp ~/.bashrc ~/.bashrc.backup
```

If you make a mistake, you can restore it with:

```bash
cp ~/.bashrc.backup ~/.bashrc
```

## 13. Practical Assignment

Complete these exercises:

- [ ] Explore the Kali XFCE desktop.
- [ ] Open the Applications menu.
- [ ] Open the file manager.
- [ ] Open the terminal.
- [ ] Change the terminal font size.
- [ ] Check your current shell using `echo $SHELL`.
- [ ] Inspect your `.bashrc` file.
- [ ] Display your environment variables.
- [ ] Create the `c` alias.
- [ ] Add the useful aliases to `.bashrc`.
- [ ] Reload `.bashrc`.
- [ ] Test the aliases.
- [ ] Create a backup of `.bashrc`.

## 14. Knowledge Check

1. What is a desktop environment?
2. What is XFCE?
3. What is a Linux shell?
4. What does Bash stand for?
5. What is the purpose of `.bashrc`?
6. What is an environment variable?
7. What is a Bash alias?
8. What does the `PATH` variable control?
9. What does `source ~/.bashrc` do?
10. Why should we back up configuration files before making major changes?

## 15. Lesson Summary

In this lesson, we explored the Kali desktop environment and terminal.

We learned about XFCE, Bash, shell configuration files, environment variables, aliases, and prompt customization.

These skills help Linux users work more efficiently and prepare for advanced Bash scripting and system administration.

**Next Lesson:** Lesson 12 – Kali Networking Configuration.