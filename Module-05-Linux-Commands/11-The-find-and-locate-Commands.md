---
---

# Lesson 11: The `find` and `locate` Commands

## 1. Introduction

Linux systems can contain thousands of files and directories.

As a Linux administrator or cybersecurity professional, you may need to locate:

- Configuration files.
- Log files.
- Executable programs.
- Recently modified files.
- Files with specific extensions.
- Files with particular permissions.
- Large files consuming storage space.

Linux provides two powerful commands for locating files:

1. `find`
2. `locate`

Both commands help you search for files, but they work differently.

- `find` searches directories and their contents directly.
- `locate` searches a previously created database of file paths.

Understanding both commands is essential for Linux administration and ethical hacking.

## 2. Understanding the `find` Command

The `find` command searches for files and directories based on specified conditions.

It can search using:

- File names.
- File types.
- File sizes.
- Modification times.
- Permissions.
- Ownership.
- Other filesystem attributes.

### Basic Syntax

```bash
find [PATH] [OPTIONS] [EXPRESSION]
```

Where:

- `find` is the command.
- `PATH` specifies where the search begins.
- `OPTIONS` modify search behaviour.
- `EXPRESSION` defines what you want to find.

### Example 1: Search the Current Directory

```bash
find .
```

The dot (`.`) represents the current working directory.

This command lists files and directories starting from your current location, including entries in subdirectories.

### Example 2: Search Your Home Directory

```bash
find ~
```

This searches your home directory and its subdirectories.

### Example 3: Search a Specific Directory

```bash
find /etc
```

This searches under `/etc`.

You may see permission errors if your user cannot access some locations.

## 3. Searching by Filename

One of the most common uses of `find` is searching for a file by name.

### 3.1 Exact Filename Search

```bash
find ~ -name "notes.txt"
```

This searches your home directory for entries named exactly `notes.txt`.

### 3.2 Case-Insensitive Search

```bash
find ~ -iname "notes.txt"
```

The `-iname` option ignores differences in letter case.

For example, it can match:

```text
notes.txt
Notes.txt
NOTES.TXT
```

### 3.3 Search Using Wildcards

You can use wildcard patterns.

Find all files ending in `.txt`:

```bash
find ~ -name "*.txt"
```

Find all files beginning with `report`:

```bash
find ~ -name "report*"
```

Find entries containing the word `config`:

```bash
find /etc -name "*config*"
```

The asterisk (`*`) matches zero or more characters.

**Important:** Quote wildcard patterns when passing them to `find`. This prevents the shell from expanding them before `find` receives them.

## 4. Searching by File Type

The `-type` option allows you to search for specific filesystem object types.

Common types include:

| Type | Meaning |
|---|---|
| `f` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `b` | Block device |
| `c` | Character device |
| `p` | Named pipe |
| `s` | Socket |

### 4.1 Find Regular Files

```bash
find ~ -type f
```

### 4.2 Find Directories

```bash
find ~ -type d
```

### 4.3 Find Symbolic Links

```bash
find ~ -type l
```

### 4.4 Find Text Files

```bash
find ~ -type f -name "*.txt"
```

This combines file-type and filename conditions.

## 5. Searching by File Size

The `find` command can locate files based on their size.

Use the `-size` option.

Examples:

Find files larger than 100 MB:

```bash
find ~ -type f -size +100M
```

Find files smaller than 1 MB:

```bash
find ~ -type f -size -1M
```

Find files exactly 10 KB in size:

```bash
find ~ -type f -size 10k
```

Common size suffixes include:

| Suffix | Meaning |
|---|---|
| `c` | Bytes |
| `k` | 1024-byte units |
| `M` | 1024²-byte units |
| `G` | 1024³-byte units |

The `+` and `-` symbols specify greater-than and less-than size ranges.

## 6. Searching by Modification Time

Linux records file timestamps, including modification time.

The `-mtime` option searches based on modification time in 24-hour periods.

### 6.1 Files Modified Within the Last 24 Hours

```bash
find ~ -type f -mtime -1
```

### 6.2 Files Modified More Than Seven Days Ago

```bash
find ~ -type f -mtime +7
```

### 6.3 Files Modified Exactly Seven 24-Hour Periods Ago

```bash
find ~ -type f -mtime 7
```

These values use rounded 24-hour periods, so they are not exact clock-time measurements.

### 6.4 Search by Minutes

Use `-mmin` for modification time in minutes.

Find files modified within the last 30 minutes:

```bash
find ~ -type f -mmin -30
```

Find files modified more than 60 minutes ago:

```bash
find ~ -type f -mmin +60
```

## 7. Searching by Permissions

The `-perm` option allows you to search for files based on permission settings.

For example:

```bash
find ~ -type f -perm 644
```

This finds files whose permissions match the specified mode.

To find files with any executable permission bit set:

```bash
find ~ -type f -perm /111
```

This is useful when inspecting executable files.

You will study Linux permissions in greater detail in Module 07.

## 8. Searching by Owner

You can search for files belonging to a particular user.

Example:

```bash
find ~ -user kali
```

To search for files belonging to a particular group:

```bash
find ~ -group kali
```

Replace the username or group with the relevant account on your system.

These searches can help administrators inspect filesystem ownership.

## 9. Combining Search Conditions

One of the most powerful features of `find` is combining multiple conditions.

### Example 1: Find All Text Files

```bash
find ~ -type f -name "*.txt"
```

### Example 2: Find Large Text Files

```bash
find ~ -type f -name "*.txt" -size +1M
```

### Example 3: Find Recently Modified Log Files

```bash
find /var/log -type f -name "*.log" -mtime -1
```

### Example 4: Find Executable Files

```bash
find ~ -type f -executable
```

### Example 5: Find Empty Files

```bash
find ~ -type f -empty
```

These combinations make `find` a flexible investigation tool.

## 10. Performing Actions on Search Results

The `find` command can perform actions on matching entries.

### 10.1 Display Results

```bash
find ~ -type f -name "*.txt" -print
```

The `-print` action displays matching paths.

It is the default action in many ordinary `find` expressions.

### 10.2 Display Detailed Information

```bash
find ~ -type f -name "*.txt" -exec ls -lh {} \;
```

Explanation:

- `-exec` runs another command.
- `ls -lh` displays detailed information.
- `{}` represents the current matching path.
- `\;` marks the end of the command passed to `-exec`.

### 10.3 Execute a Command on Multiple Results

```bash
find ~ -type f -name "*.txt" -exec ls -lh {} +
```

The `+` terminator allows `find` to pass multiple matching paths to one command invocation when possible.

**Safety note:** Be especially careful when using `-exec` with commands that modify or delete files. First run a search that only displays the matching paths. Verify the results before performing any action.

## 11. Understanding the `locate` Command

The `locate` command searches for file paths using a database.

Unlike `find`, which searches directories directly, `locate` normally searches a database generated by an indexing service.

### Basic Syntax

```bash
locate [OPTIONS] PATTERN
```

Example:

```bash
locate passwd
```

This displays indexed paths matching the search pattern.

Possible output includes:

```text
/etc/passwd
/usr/share/man/man5/passwd.5.gz
```

The exact results depend on your system's database.

## 12. How `locate` Works

The general process is:

1. A database indexing tool scans filesystem locations.
2. It stores file paths in a database.
3. The `locate` command searches that database.
4. Matching paths are displayed.

This makes `locate` fast because it does not normally scan the entire filesystem every time you run it.

However, the database may not include newly created files or reflect recent changes.

## 13. Updating the Locate Database

On many Debian-based systems, including Kali Linux, the database can be updated using:

```bash
sudo updatedb
```

This may require administrative privileges.

After the update, search again:

```bash
locate notes.txt
```

**Important:** `locate` and `updatedb` may not be installed by default. Your system may use a different indexing implementation or database configuration.

If `locate` is unavailable, inspect its availability:

```bash
command -v locate
```

On Kali, the package commonly used to provide `locate` is `plocate`.

If it is missing and you want to install it:

```bash
sudo apt update
sudo apt install plocate
```

Only install packages from trusted repositories configured for your Kali system.

## 14. Important `locate` Options

### 14.1 Case-Insensitive Search

```bash
locate -i notes.txt
```

This ignores differences in letter case.

### 14.2 Limit the Number of Results

```bash
locate -n 10 passwd
```

This displays up to ten matching paths.

### 14.3 Count Matching Results

```bash
locate -c passwd
```

This displays the number of matching paths.

### 14.4 Search Using Wildcards

```bash
locate "*.conf"
```

This searches for indexed paths matching the specified pattern.

## 15. Comparing `find` and `locate`

| Feature | `find` | `locate` |
|---|---|---|
| Search method | Direct filesystem traversal | Database search |
| Speed | Depends on search scope | Usually very fast |
| Database required | No | Yes |
| Newly created files | Can find them immediately if accessible | May be missing until database update |
| Search by permissions | Yes | No general permission filtering |
| Search by file size | Yes | No general file-size filtering |
| Search by modification time | Yes | No general modification-time filtering |
| Best use | Detailed filesystem searches | Quickly locating indexed paths |

**Remember:** Use `find` when you need detailed search conditions or current filesystem results. Use `locate` when you want to find a path quickly from an up-to-date database.

## 16. Practical Lab: Using `find`

Perform these exercises in Kali Linux.

### Step 1: Search Your Home Directory

```bash
find ~
```

### Step 2: Find Text Files

```bash
find ~ -type f -name "*.txt"
```

### Step 3: Find Directories

```bash
find ~ -type d
```

### Step 4: Find Hidden Files

```bash
find ~ -type f -name ".*"
```

### Step 5: Find Large Files

```bash
find ~ -type f -size +10M
```

### Step 6: Find Recently Modified Files

```bash
find ~ -type f -mtime -1
```

### Step 7: Find Empty Files

```bash
find ~ -type f -empty
```

### Step 8: Find Executable Files

```bash
find ~ -type f -executable
```

Observe the results and understand what each search condition does.

## 17. Practical Lab: Create a Search Environment

We will create a small environment for practising file searches.

### Step 1: Create the Directory Structure

```bash
mkdir -p ~/find-lab/Documents
mkdir -p ~/find-lab/Logs
mkdir -p ~/find-lab/Scripts
```

### Step 2: Create Test Files

```bash
touch ~/find-lab/Documents/report.txt
touch ~/find-lab/Documents/notes.txt
touch ~/find-lab/Logs/system.log
touch ~/find-lab/Scripts/backup.sh
touch ~/find-lab/.hidden-file
```

### Step 3: Find All Text Files

```bash
find ~/find-lab -type f -name "*.txt"
```

### Step 4: Find All Directories

```bash
find ~/find-lab -type d
```

### Step 5: Find Hidden Files

```bash
find ~/find-lab -type f -name ".*"
```

### Step 6: Find the Log File

```bash
find ~/find-lab -type f -name "*.log"
```

### Step 7: Display Detailed Information

```bash
find ~/find-lab -type f -exec ls -lh {} \;
```

### Step 8: Clean Up

After completing the exercise:

```bash
cd ~
```

You may remove the practice directory:

```bash
rm -r ~/find-lab
```

Verify the path before running the removal command. It permanently removes the specified directory and its contents.

## 18. Practical Lab: Using `locate`

First check whether it is installed:

```bash
command -v locate
```

If available, try:

```bash
locate passwd
```

Then:

```bash
locate -i bash
```

Count matching results:

```bash
locate -c passwd
```

Limit the results:

```bash
locate -n 10 passwd
```

If a newly created file does not appear, the indexing database may be outdated.

If `updatedb` is available, update the database:

```bash
sudo updatedb
```

Then search again.

## 19. Common Mistakes

### Mistake 1: Searching the Entire Filesystem Unnecessarily

This command can generate a large amount of output:

```bash
find /
```

It may also produce permission errors.

For practice, start with:

```bash
find ~
```

Use a narrower search path whenever possible.

### Mistake 2: Forgetting Quotes Around Wildcards

Prefer:

```bash
find ~ -name "*.txt"
```

This ensures the wildcard pattern is passed to `find`.

### Mistake 3: Expecting `locate` to Find Newly Created Files

The locate database may not have been updated.

Use `updatedb` when available, or use `find` for a direct search.

### Mistake 4: Ignoring Permission Errors

Some directories cannot be accessed by ordinary users.

A permission error does not necessarily mean the file does not exist.

### Mistake 5: Using Destructive Actions Too Quickly

Do not combine `find` with deletion commands until you have checked exactly which files match.

Start with a display-only search.

## 20. Cybersecurity Importance

The `find` and `locate` commands are valuable during authorised security assessments.

### 20.1 Locating Configuration Files

```bash
find /etc -type f -name "*.conf"
```

This can help identify configuration files.

### 20.2 Searching for Log Files

```bash
find /var/log -type f -name "*.log"
```

This helps locate log files for further investigation.

### 20.3 Identifying Executable Files

```bash
find ~ -type f -executable
```

This helps inspect executable files in a user's home directory.

### 20.4 Finding Recently Modified Files

```bash
find ~ -type f -mtime -1
```

This can help identify files modified within the last 24-hour period.

### 20.5 Identifying Large Files

```bash
find ~ -type f -size +100M
```

This helps identify large files that may require investigation.

### 20.6 Investigating Unusual Files

During an authorised investigation, you may search for hidden files, unexpected executables, or files with unusual ownership and permissions.

A suspicious filename or location alone is not proof of malicious activity. Further investigation is necessary.

**Security rule:** Use file-search commands to collect evidence and understand the filesystem before making changes.

## 21. Important Commands Summary

| Command | Purpose |
|---|---|
| `find .` | Search from the current directory |
| `find ~` | Search your home directory |
| `find ~ -name "*.txt"` | Find text files |
| `find ~ -iname "notes.txt"` | Case-insensitive filename search |
| `find ~ -type f` | Find regular files |
| `find ~ -type d` | Find directories |
| `find ~ -type l` | Find symbolic links |
| `find ~ -size +100M` | Find entries larger than 100 MB |
| `find ~ -mtime -1` | Find entries modified within the last 24 hours |
| `find ~ -empty` | Find empty entries |
| `find ~ -executable` | Find executable entries |
| `locate passwd` | Search the locate database |
| `locate -i passwd` | Case-insensitive locate search |
| `locate -n 10 passwd` | Limit results |
| `sudo updatedb` | Update the locate database |

## 22. Knowledge Check

Answer these questions in your notebook.

1. What is the main purpose of the `find` command?
2. What is the main difference between `find` and `locate`?
3. What does `find .` do?
4. What is the difference between `-name` and `-iname`?
5. What does `-type f` identify?
6. What does `-type d` identify?
7. How can you find files larger than 100 MB?
8. What does `-mtime -1` mean?
9. Why might `locate` fail to display a newly created file?
10. What is the purpose of `updatedb`?
11. What does `{}` represent in a `find -exec` expression?
12. Why should you inspect search results before performing deletion operations?

## 23. Practical Assignment

Complete the following tasks in Kali Linux.

1. Find all `.txt` files inside your home directory.
2. Find all directories inside your home directory.
3. Find all hidden regular files inside your home directory.
4. Find all executable regular files inside your home directory.
5. Find files larger than 10 MB.
6. Find files modified within the last 24 hours.
7. Create a directory called `search-practice`.
8. Create three text files and one hidden file inside it.
9. Use `find` to locate each file.
10. Use `find -exec` to display detailed information about the test files.
11. Check whether `locate` is installed.
12. If available, search for `passwd` using `locate`.
13. Explain why `find` is more suitable for certain detailed security investigations.
14. Explain why `locate` is usually faster for simple path searches.

## 24. Lesson Conclusion

The `find` and `locate` commands are essential Linux search tools.

You have learned how to:

- Search by filename.
- Search by file type.
- Search by size.
- Search by modification time.
- Search by permissions and ownership.
- Combine multiple search conditions.
- Execute commands on search results.
- Search indexed file paths using `locate`.

**Key lesson:** Use `find` for detailed, direct filesystem searches and `locate` for fast database-based searches.

---

**End of Lesson 11 — The `find` and `locate` Commands**