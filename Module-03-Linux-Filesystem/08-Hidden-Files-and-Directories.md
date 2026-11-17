---
---

# LESSON 08: HIDDEN FILES AND DIRECTORIES

**Course:** Linux for Ethical Hackers  
**Module:** 03 – Linux Filesystem

## 1. Introduction

Linux allows users to hide files and directories from normal directory listings.

Hidden files are commonly used to store:

- Application configurations
- User preferences
- Shell settings
- SSH configurations
- Application data
- Security-related settings

In Linux, a filename beginning with a dot (`.`) is treated as hidden by ordinary directory listing commands.

Examples:

```text
.bashrc
.bash_history
.profile
.ssh
.config
```

These files are not necessarily secret or encrypted. They are simply hidden from normal listings.

## 2. How Linux Identifies Hidden Files

Linux uses a simple naming convention.

A filename beginning with a dot is considered hidden.

Examples:

```text
notes.txt
.hidden-notes.txt
.bashrc
```

Here:

- `notes.txt` is a normal file.
- `.hidden-notes.txt` is a hidden file.
- `.bashrc` is a hidden configuration file.

The dot at the beginning is what makes the difference.

## 3. Viewing Hidden Files

The normal `ls` command does not display hidden files.

Run:

```bash
ls
```

To display hidden files, use:

```bash
ls -a
```

The `-a` option means **all**.

Example output:

```text
.  ..  .bashrc  .profile  Documents  Downloads
```

You can also use:

```bash
ls -la
```

This displays hidden files together with detailed information.

For a more readable listing:

```bash
ls -lah
```

Remember:

| Command | Purpose |
|---|---|
| `ls` | Displays ordinary visible entries |
| `ls -a` | Displays hidden and visible entries |
| `ls -l` | Displays detailed information |
| `ls -la` | Displays all entries with details |
| `ls -lah` | Displays all entries with human-readable sizes |

## 4. Understanding `.` and `..`

When you execute:

```bash
ls -a
```

You will usually see:

```text
.
..
```

These have special meanings.

- `.` represents the current directory.
- `..` represents the parent directory.

For example, if your current location is:

```text
/home/kali/Documents
```

Then:

- `.` means `/home/kali/Documents`.
- `..` means `/home/kali`.

These are directory references, not ordinary hidden documents.

## 5. Creating Hidden Files

To create a hidden file, simply begin its name with a dot.

Example:

```bash
touch .secret.txt
```

Check the directory:

```bash
ls
```

You may not see the file.

Now run:

```bash
ls -a
```

You should see:

```text
.secret.txt
```

You can create a hidden directory in the same way:

```bash
mkdir .private
```

Verify it:

```bash
ls -la
```

## 6. Accessing Hidden Files

Hidden files can be opened and modified like ordinary files.

Example:

```bash
nano .secret.txt
```

You can also display their contents using:

```bash
cat .secret.txt
```

Accessing a hidden directory:

```bash
cd .private
```

The dot does not prevent access. It only affects normal directory listings.

## 7. Important Linux Configuration Files

Many important Linux configuration files are hidden.

### `.bashrc`

Location:

```text
~/.bashrc
```

Purpose:

- Configures Bash shell behavior.
- Defines aliases.
- Sets environment variables.
- Customizes the terminal prompt.

Inspect it:

```bash
cat ~/.bashrc
```

### `.profile`

Location:

```text
~/.profile
```

Purpose:

- Stores settings that may be applied when a user logs in.
- Can configure environment variables.

Inspect it:

```bash
cat ~/.profile
```

### `.bash_history`

Location:

```text
~/.bash_history
```

Purpose:

- Stores Bash command history when it is written to disk.

Inspect it:

```bash
cat ~/.bash_history
```

Note: The current shell session may contain commands that have not yet been written to this file. Shell history settings can also affect what is saved.

### `.ssh`

Location:

```text
~/.ssh
```

Purpose:

- Stores SSH configuration and key-related files.

Examples include:

```text
config
known_hosts
authorized_keys
id_ed25519
id_ed25519.pub
```

Some of these files may contain sensitive information.

**Security warning:** Never share private SSH keys or copy them into public locations.

Inspect the directory safely:

```bash
ls -la ~/.ssh
```

Do not display or share private key contents.

## 8. Hidden Files and Cybersecurity

Hidden files are important during security investigations.

An attacker may attempt to hide files or directories to make suspicious activity less noticeable.

A security analyst may investigate:

- Unexpected hidden scripts
- Suspicious configuration changes
- Unknown files inside user directories
- Unexpected SSH configuration
- Unusual startup or shell configuration entries

However, hidden does not automatically mean malicious.

Many legitimate Linux applications use hidden files.

## 9. Searching for Hidden Files

You can use the `find` command to search for hidden files.

Search your home directory:

```bash
find ~ -name ".*" -type f
```

Explanation:

- `find` searches directories.
- `~` specifies your home directory.
- `-name ".*"` matches names beginning with a dot.
- `-type f` selects regular files.

To find hidden directories:

```bash
find ~ -name ".*" -type d
```

To search for hidden files and directories together:

```bash
find ~ -name ".*"
```

Be aware that these searches may also display `.` and `..` directory entries in some contexts or encounter permission restrictions depending on the search location.

## 10. Practical Laboratory

Perform the following exercises in Kali Linux.

### Exercise 1: View Hidden Files

```bash
cd ~
ls
ls -a
ls -lah
```

Observe the difference between the outputs.

### Exercise 2: Create a Hidden File

```bash
touch .my-hidden-file
```

Check:

```bash
ls
ls -a
```

Confirm that the file appears in the second listing.

### Exercise 3: Create a Hidden Directory

```bash
mkdir .my-private-folder
```

Inspect it:

```bash
ls -la
```

### Exercise 4: Add Content to a Hidden File

```bash
echo "Linux filesystem practice" > .my-hidden-file
```

Display its content:

```bash
cat .my-hidden-file
```

### Exercise 5: Inspect Bash Configuration

```bash
ls -l ~/.bashrc
cat ~/.bashrc
```

Observe some of the configuration settings.

### Exercise 6: Inspect SSH Configuration

```bash
ls -la ~/.ssh
```

If the directory does not exist, that is not necessarily a problem.

Do not expose or share private key contents.

### Exercise 7: Search for Hidden Files

```bash
find ~ -name ".*" -type f
```

Observe the hidden files in your home directory.

### Exercise 8: Clean Up

Remove only the two items created during this laboratory:

```bash
rm ~/.my-hidden-file
rmdir ~/.my-private-folder
```

## 11. Knowledge Check

1. What makes a file hidden in Linux?
2. Which command displays hidden files?
3. Does hiding a file encrypt or protect its contents?
4. What does `.` represent?
5. What does `..` represent?
6. What is the purpose of `.bashrc`?
7. What type of information can be stored inside `.ssh`?
8. Which command searches for hidden regular files in your home directory?
9. Why might a cybersecurity analyst investigate hidden files?
10. Is every hidden file malicious?

## 12. Lesson Summary

Linux uses a dot at the beginning of a filename to identify hidden files and directories.

We learned how to:

- Display hidden files using `ls -a`.
- Create hidden files and directories.
- Access hidden configuration files.
- Inspect important Bash and SSH configuration locations.
- Search for hidden files using `find`.

**Important:** Hidden files are not encrypted or automatically secure. Their contents and permissions still need to be protected.

**Next Lesson:** File Extensions and File Names.