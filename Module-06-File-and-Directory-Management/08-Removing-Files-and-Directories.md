# Module 06 – File and Directory Management

## Lesson 08: Removing Files and Directories

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand how Linux deletes files and directories.
- Remove individual files using `rm`.
- Remove empty directories using `rmdir`.
- Remove directories containing files.
- Understand important deletion options.
- Avoid accidentally deleting important system data.

### 2. Introduction

Linux provides commands for removing unwanted files and directories.

The two main commands are:

| Command | Purpose |
|---|---|
| `rm` | Removes files and directories. |
| `rmdir` | Removes empty directories. |

**Important:** Linux terminal deletion is generally permanent. Unlike a graphical desktop, commands such as `rm` do not normally move files into the Trash or Recycle Bin.

Always verify what you are deleting.

### 3. Removing a File with `rm`

The `rm` command means **remove**.

Syntax:

```bash
rm filename
```

Example:

```bash
rm notes.txt
```

This removes `notes.txt`.

Verify:

```bash
ls
```

If the file was successfully removed, it will no longer appear.

### 4. Removing Multiple Files

You can remove multiple files using one command.

Example:

```bash
rm notes.txt report.txt image.png
```

This removes all three files.

You can also use wildcards, which we will study in Lesson 14.

Example:

```bash
rm *.txt
```

This removes matching `.txt` files in the current directory.

Be extremely careful with wildcard deletion.

### 5. Interactive Deletion with `rm -i`

The `-i` option means interactive.

It asks for confirmation before removing each file.

Example:

```bash
rm -i notes.txt
```

You may see:

```text
rm: remove regular file 'notes.txt'?
```

Type:

- `y` to confirm.
- `n` to cancel.

This is useful when you want additional protection against accidental deletion.

### 6. Force Deletion with `rm -f`

The `-f` option means force.

Example:

```bash
rm -f temporary.txt
```

It attempts to remove the file without asking for confirmation and suppresses certain error messages.

Be careful: force deletion can hide mistakes and should not be used casually.

### 7. Removing Empty Directories with `rmdir`

The `rmdir` command removes empty directories.

Syntax:

```bash
rmdir directory_name
```

Example:

```bash
rmdir OldFolder
```

This works only if `OldFolder` is empty.

If the directory contains files, Linux displays an error.

Why?

Because `rmdir` is specifically designed to remove empty directories.

### 8. Removing Directories Containing Files

To remove a directory and its contents, use:

```bash
rm -r directory_name
```

The `-r` option means recursive.

Recursive deletion removes the directory and everything inside it, including its subdirectories.

Example:

```bash
rm -r OldProject
```

This removes `OldProject` and its contents.

**Security warning:** Never experiment with recursive deletion on important directories.

### 9. Interactive Recursive Deletion

You can combine `-r` and `-i`.

```bash
rm -ri OldProject
```

Linux asks for confirmation during the deletion process.

This is safer for situations where you want to inspect what is being removed.

### 10. Force and Recursive Deletion

You may encounter:

```bash
rm -rf directory_name
```

This combines:

- `-r`: recursively removes directories and their contents.
- `-f`: forces removal without interactive confirmation.

This command is powerful and dangerous.

A small path mistake can result in major data loss.

For this course, do not use `rm -rf` on system directories, home directories, or paths you have not carefully verified.

### 11. Understanding Important Options

| Option | Meaning |
|---|---|
| `-i` | Ask for confirmation. |
| `-f` | Force removal. |
| `-r` | Remove directories recursively. |
| `-v` | Display what is being removed. |

Example:

```bash
rm -iv notes.txt
```

This combines interactive and verbose behaviour.

### 12. Safe Deletion Practices

Before deleting files:

1. Check your current directory using `pwd`.
2. List the files using `ls`.
3. Confirm the exact filename.
4. Use interactive deletion when uncertain.
5. Avoid unnecessary recursive deletion.
6. Never run destructive commands against unknown paths.
7. Keep backups of important information.

Useful inspection commands:

```bash
pwd
ls -lah
```

### 13. Practical Linux Lab

We will practise only inside our dedicated course directory.

#### Step 1: Create a practice environment

```bash
mkdir -p ~/module06-lab/deletion/Project/Reports
cd ~/module06-lab/deletion
```

#### Step 2: Create practice files

```bash
touch notes.txt report.txt temporary.txt
touch Project/project-plan.txt
touch Project/Reports/final-report.txt
```

#### Step 3: Inspect the directory

```bash
find . -maxdepth 3 -print
```

#### Step 4: Remove one file

```bash
rm temporary.txt
```

Verify:

```bash
ls
```

#### Step 5: Practise interactive deletion

Create a test file:

```bash
touch test.txt
```

Run:

```bash
rm -i test.txt
```

Type `y` to confirm.

#### Step 6: Remove an empty directory

Create an empty directory:

```bash
mkdir EmptyFolder
```

Remove it:

```bash
rmdir EmptyFolder
```

#### Step 7: Try removing a non-empty directory with `rmdir`

Run:

```bash
rmdir Project
```

It should fail because `Project` contains files and another directory.

This demonstrates the difference between `rmdir` and recursive removal.

#### Step 8: Remove a directory recursively

First inspect it:

```bash
find Project -maxdepth 3 -print
```

Then remove it interactively:

```bash
rm -ri Project
```

Read the prompts carefully and confirm the requested removals.

#### Step 9: Final verification

```bash
find . -maxdepth 3 -print
```

The `Project` directory and its contents should now be gone.

### 14. Cybersecurity Applications

Understanding file deletion is important in cybersecurity.

**A. Log management**

Administrators remove outdated logs according to retention policies.

**B. Malware analysis**

Analysts may remove isolated test files after completing controlled analysis.

**C. Incident response**

Security teams manage temporary investigation files and unnecessary copies.

**D. System administration**

Administrators remove obsolete files and empty directories to maintain organised systems.

**Important:** Deleting a file does not guarantee that its data has been securely erased from storage. Data may remain recoverable depending on the storage device and filesystem.

### 15. Common Mistakes

| Mistake | Explanation |
|---|---|
| Using `rmdir` on a non-empty directory | It will fail. |
| Using `rm -r` carelessly | It can delete many files and directories. |
| Forgetting the current directory | You may delete files from the wrong location. |
| Assuming deleted files go to Trash | Terminal deletion normally bypasses Trash. |
| Using `rm -f` unnecessarily | It removes confirmation safeguards. |

### 16. Knowledge Check

Answer these questions:

1. What does `rm` stand for?
2. Which command removes an individual file?
3. What is the difference between `rm` and `rmdir`?
4. What does the `-i` option do?
5. What does the `-r` option mean?
6. Why is `rm -rf` dangerous?
7. Can `rmdir` remove a directory containing files?
8. Does normal terminal deletion guarantee secure erasure?

### 17. Lesson Summary

In this lesson, you learned:

- `rm` removes files.
- `rmdir` removes empty directories.
- `rm -r` removes directories and their contents recursively.
- `rm -i` asks for confirmation.
- `rm -f` forces removal.
- `rm -v` displays removal activity.
- Destructive commands must be used carefully.

**Next Lesson:** Lesson 09 – Viewing File Contents with `cat`.