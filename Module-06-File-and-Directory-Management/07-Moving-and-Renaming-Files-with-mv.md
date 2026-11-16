# Module 06 – File and Directory Management

## Lesson 07: Moving and Renaming Files with `mv`

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand the purpose of the `mv` command.
- Move files from one directory to another.
- Rename files and directories.
- Move multiple files.
- Understand important `mv` options.
- Use `mv` safely when managing Linux systems.

---

### 2. Introduction to the `mv` Command

Linux provides the `mv` command to move and rename files and directories.

The name `mv` stands for **move**.

Unlike the `cp` command, which creates a copy while keeping the original, `mv` changes the original file's location or name.

For example:

- `cp notes.txt backup.txt` creates another copy.
- `mv notes.txt backup.txt` renames the original file.

### 3. Basic Syntax

```bash
mv [OPTIONS] SOURCE DESTINATION
```

Where:

- `mv` is the command.
- `OPTIONS` are optional settings.
- `SOURCE` is the original file or directory.
- `DESTINATION` is the new location or name.

### 4. Renaming Files

To rename a file, provide its current name followed by its new name.

Example:

```bash
mv notes.txt lecture-notes.txt
```

This changes the filename from `notes.txt` to `lecture-notes.txt`.

To verify:

```bash
ls
```

**Important:** Renaming does not create a second copy. The original name is replaced by the new name.

### 5. Moving Files Between Directories

Suppose you have this structure:

```text
Documents/
├── notes.txt
└── Backups/
```

To move `notes.txt` into the `Backups` directory:

```bash
mv notes.txt Backups/
```

The file is now inside `Backups`.

To verify:

```bash
ls
ls Backups/
```

### 6. Moving and Renaming a File at the Same Time

You can move a file and give it a new name in one command.

Example:

```bash
mv notes.txt Backups/important-notes.txt
```

This moves `notes.txt` into `Backups` and renames it to `important-notes.txt`.

### 7. Moving Multiple Files

You can move multiple files into one destination directory.

Example:

```bash
mv file1.txt file2.txt file3.txt Backups/
```

All three files will be moved into `Backups/`.

The destination directory must already exist.

### 8. Moving Directories

The `mv` command can also move directories.

Example:

```bash
mv Projects Documents/
```

This moves the entire `Projects` directory into `Documents`.

You do not need the `-r` option when moving directories with `mv`.

### 9. Important `mv` Options

#### A. Interactive Mode: `-i`

```bash
mv -i notes.txt Backups/
```

Asks for confirmation before overwriting an existing destination file.

#### B. No-Clobber Mode: `-n`

```bash
mv -n notes.txt Backups/
```

Prevents overwriting an existing destination file.

#### C. Verbose Mode: `-v`

```bash
mv -v notes.txt Backups/
```

Displays what the command is doing.

#### D. Update Mode: `-u`

```bash
mv -u notes.txt Backups/
```

Moves the source only when the destination is missing or older than the source.

### 10. Understanding File Overwriting

Be careful when moving files.

Suppose `Backups/notes.txt` already exists.

Running:

```bash
mv notes.txt Backups/
```

may replace the existing destination file without asking, depending on the command's options and environment.

Safer alternatives include:

```bash
mv -i notes.txt Backups/
```

or:

```bash
mv -n notes.txt Backups/
```

For important files, inspect the destination before moving anything.

### 11. Absolute and Relative Paths

You can use both absolute and relative paths with `mv`.

**Relative path:**

```bash
mv notes.txt Backups/
```

**Absolute path:**

```bash
mv /home/kali/Documents/notes.txt /home/kali/Backups/
```

An absolute path starts from the root directory `/`.

A relative path is interpreted from your current working directory.

### 12. Practical Linux Lab

We will practise inside a dedicated directory so that no important system files are affected.

#### Step 1: Create a practice environment

```bash
mkdir -p ~/module06-lab/moving
cd ~/module06-lab/moving
```

#### Step 2: Create practice files

```bash
touch notes.txt report.txt evidence.txt
```

Verify:

```bash
ls
```

#### Step 3: Rename a file

```bash
mv notes.txt study-notes.txt
```

Verify:

```bash
ls
```

#### Step 4: Create a destination directory

```bash
mkdir Backups
```

#### Step 5: Move a file

```bash
mv report.txt Backups/
```

Verify:

```bash
ls
ls Backups/
```

#### Step 6: Move and rename a file

```bash
mv evidence.txt Backups/security-evidence.txt
```

Verify:

```bash
ls Backups/
```

#### Step 7: Practise verbose mode

Create another file:

```bash
touch temporary.txt
```

Move it:

```bash
mv -v temporary.txt Backups/
```

Observe the output.

#### Step 8: Practise interactive mode

Create two files:

```bash
touch sample.txt
touch Backups/sample.txt
```

Now execute:

```bash
mv -i sample.txt Backups/
```

If prompted about overwriting, type `n` and press Enter to protect the existing destination file.

#### Step 9: Inspect your final directory structure

```bash
find ~/module06-lab/moving -maxdepth 2 -type f
```

You should see:

```text
study-notes.txt
Backups/report.txt
Backups/security-evidence.txt
Backups/temporary.txt
Backups/sample.txt
```

The original `Backups/sample.txt` should remain unchanged because you declined the overwrite.

### 13. Cybersecurity Applications

The `mv` command is useful in cybersecurity and system administration.

Examples include:

- Organising investigation files.
- Moving log files into dedicated directories.
- Renaming suspicious files during controlled malware analysis.
- Organising penetration-testing reports.
- Moving completed backups into archive directories.

**Important security note:** Renaming a suspicious file does not make it safe. Never execute an unknown file simply because you changed its name.

### 14. Common Mistakes

| Mistake | Explanation |
|---|---|
| Using the wrong source path | Linux cannot find the file. |
| Using a destination that does not exist | The move may fail. |
| Forgetting overwrite risks | Existing destination data may be replaced. |
| Confusing `cp` and `mv` | `cp` copies; `mv` moves or renames. |
| Assuming `mv` needs `-r` | It can move directories without `-r`. |

### 15. Knowledge Check

Answer these questions:

1. What does `mv` stand for?
2. What is the difference between `cp` and `mv`?
3. Which command renames `old.txt` to `new.txt`?
4. How do you move `report.txt` into a directory called `Backups`?
5. Which option asks for confirmation before overwriting?
6. Which option prevents overwriting an existing destination?
7. Can `mv` move directories without using `-r`?
8. What does `mv -v` do?

### 16. Lesson Summary

In this lesson, you learned that:

- `mv` moves files and directories.
- `mv` can rename files and directories.
- You can move multiple files with one command.
- `mv -i` requests confirmation before overwriting.
- `mv -n` prevents overwriting.
- `mv -v` displays the operation.
- `mv` does not require `-r` to move directories.

**Next Lesson:** Lesson 08 – Removing Files and Directories.