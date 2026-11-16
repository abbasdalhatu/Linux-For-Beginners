
# PRACTICAL LABS
# MODULE 01 – LINUX COMMAND-LINE MASTERY

## LAB 02: FILES AND DIRECTORY OPERATIONS

---

## 1. Lab Objectives

In this lab, you will practice managing files and directories through the Linux terminal.

By the end, you should be able to:

1. Create files and directories.
2. Copy files and directories.
3. Move and rename files.
4. Remove files and empty directories.
5. Inspect file types and metadata.
6. Create symbolic and hard links.
7. Find files using Linux commands.
8. Perform basic file-management operations safely.

## 2. Prepare Your Lab Environment

Navigate to your practice directory:

```bash
cd ~/practical-labs/module01/lab01
```

Create a separate directory for this exercise:

```bash
mkdir -p ~/practical-labs/module01/lab02
cd ~/practical-labs/module01/lab02
```

Verify:

```bash
pwd
```

All file creation and deletion exercises in this lesson must remain inside this lab directory.

---

## 3. Exercise 1: Create Files

The `touch` command creates an empty file or updates an existing file's timestamps.

Create three files:

```bash
touch notes.txt
touch commands.txt
touch report.txt
```

List them:

```bash
ls -l
```

You should see all three files.

Create several files at once:

```bash
touch file1.txt file2.txt file3.txt
```

Verify:

```bash
ls
```

### Practical challenge

Create five files named:

```text
linux.txt
security.txt
network.txt
bash.txt
admin.txt
```

---

## 4. Exercise 2: Create Directories

Use `mkdir` to create directories.

```bash
mkdir documents
mkdir scripts
mkdir reports
```

Create nested directories:

```bash
mkdir -p projects/linux/module1
```

The `-p` option creates missing parent directories automatically.

Inspect your structure:

```bash
find . -maxdepth 4 -print
```

Understand:

- `.` represents the current directory.
- `..` represents the parent directory.
- `-maxdepth` limits how deep `find` searches.

---

## 5. Exercise 3: Write Text into Files

Use `echo` to write text into a file.

```bash
echo "Linux practical laboratory" > notes.txt
```

Display the content:

```bash
cat notes.txt
```

Add another line without replacing the existing content:

```bash
echo "Learning Linux file management" >> notes.txt
```

Display again:

```bash
cat notes.txt
```

Important distinction:

| Operator | Purpose |
|---|---|
| `>` | Creates or replaces file content |
| `>>` | Adds content to the end |

Practice with `commands.txt`:

```bash
echo "pwd - Print working directory" > commands.txt
echo "ls - List directory contents" >> commands.txt
echo "cd - Change directory" >> commands.txt
```

Display:

```bash
cat commands.txt
```

---

## 6. Exercise 4: Copy Files

The `cp` command copies files.

Create a copy:

```bash
cp notes.txt notes-backup.txt
```

Verify:

```bash
ls -l
```

Copy a file into another directory:

```bash
cp notes.txt documents/
```

Verify:

```bash
ls documents/
```

Copy multiple files into a directory:

```bash
cp commands.txt report.txt documents/
```

Copy a directory and its contents:

```bash
cp -r documents documents-backup
```

The `-r` option means recursive.

Inspect:

```bash
find . -maxdepth 3 -print
```

---

## 7. Exercise 5: Move and Rename Files

Use `mv` to move a file:

```bash
mv report.txt reports/
```

Verify:

```bash
ls reports/
```

Rename a file:

```bash
mv notes-backup.txt old-notes.txt
```

Verify:

```bash
ls
```

Move and rename simultaneously:

```bash
mv old-notes.txt documents/final-notes.txt
```

Inspect:

```bash
find documents -maxdepth 2 -type f
```

Remember: `mv` can move or rename files depending on its destination.

---

## 8. Exercise 6: Remove Files and Directories

The `rm` command removes files.

Create a temporary test file:

```bash
touch temporary.txt
```

Remove it:

```bash
rm temporary.txt
```

Verify:

```bash
ls
```

Remove an empty directory:

```bash
mkdir empty-folder
rmdir empty-folder
```

Remove a non-empty directory inside your lab:

```bash
mkdir -p test-folder/subfolder
touch test-folder/example.txt
rm -r test-folder
```

The `-r` option removes directories recursively.

**Safety rules:**

- Always inspect a path before deleting its contents.
- Never experiment with `rm -rf` on system directories.
- Do not use broad wildcards with destructive commands.
- Keep deletion exercises inside your dedicated lab directory.

---

## 9. Exercise 7: Inspect File Types and Metadata

Use `file` to identify file types:

```bash
file notes.txt
```

Use `stat` to inspect metadata:

```bash
stat notes.txt
```

Display file sizes:

```bash
du -h notes.txt
```

Display directory sizes:

```bash
du -sh documents
```

List files with human-readable sizes:

```bash
ls -lh
```

Understand the information returned by `stat`:

- File size.
- File type.
- Permissions.
- Owner.
- Group.
- Access time.
- Modification time.
- Change time.

---

## 10. Exercise 8: Find Files

The `find` command searches for files and directories.

Find all text files in your lab:

```bash
find . -type f -name "*.txt"
```

Find directories:

```bash
find . -type d
```

Find files with a particular name:

```bash
find . -type f -name "notes.txt"
```

Find files larger than 1 KB:

```bash
find . -type f -size +1k
```

Search for files modified within the last day:

```bash
find . -type f -mtime -1
```

The `-mtime` option measures modification time in 24-hour periods.

---

## 11. Exercise 9: Create Symbolic and Hard Links

Linux supports different types of file links.

### Symbolic link

Create a symbolic link:

```bash
ln -s notes.txt notes-link.txt
```

Inspect:

```bash
ls -l notes-link.txt
```

Read through the link:

```bash
cat notes-link.txt
```

A symbolic link stores a path pointing to another file or directory.

### Hard link

Create a hard link:

```bash
ln notes.txt notes-hardlink.txt
```

Inspect:

```bash
ls -li notes.txt notes-hardlink.txt
```

The inode numbers should match.

A hard link is another directory entry pointing to the same inode.

### Important difference

| Symbolic link | Hard link |
|---|---|
| Stores a path | References the same inode |
| Can point to directories | Normally used for files |
| Can cross filesystem boundaries | Cannot cross filesystem boundaries |
| Can become broken if its target disappears | Remains accessible while another hard link references the inode |

---

## 12. Exercise 10: Build a Small File-Management Structure

Create this structure inside your Lab 02 directory:

```text
lab02/
├── documents/
│   ├── linux-notes.txt
│   └── security-notes.txt
├── scripts/
│   └── backup.sh
├── reports/
│   └── lab-report.txt
└── archive/
```

Commands:

```bash
mkdir -p documents scripts reports archive

touch documents/linux-notes.txt
touch documents/security-notes.txt
touch scripts/backup.sh
touch reports/lab-report.txt
```

Add some content:

```bash
echo "Linux file management notes" > documents/linux-notes.txt

echo "Linux security notes" > documents/security-notes.txt

echo "Backup script practice" > scripts/backup.sh

echo "Lab 02 file management report" > reports/lab-report.txt
```

Inspect the final structure:

```bash
find . -maxdepth 3 -print
```

---

## 13. Practical Challenge

Complete these tasks independently:

1. Create a directory named `challenge`.
2. Inside it, create `files`, `backups`, and `reports`.
3. Create three text files inside `files`.
4. Add meaningful text to each file.
5. Copy the three files into `backups`.
6. Rename one original file.
7. Move one original file into `reports`.
8. Create a symbolic link to one remaining file.
9. Inspect all file permissions and metadata.
10. Display the complete directory structure.

Do not delete your entire Lab 02 directory after completing the challenge.

---

## 14. Knowledge Check

Answer in your notebook:

1. What does `touch` do?
2. What is the purpose of `mkdir -p`?
3. Explain the difference between `cp` and `mv`.
4. What does `rm -r` do?
5. What is the purpose of `stat`?
6. What is the difference between `>` and `>>`?
7. What does `find . -type f` search for?
8. Explain symbolic links.
9. Explain hard links.
10. Why should destructive commands be tested inside dedicated lab directories?

---

## 15. Lab Completion Checklist

- [ ] Created multiple files.
- [ ] Created nested directories.
- [ ] Wrote text into files.
- [ ] Copied files.
- [ ] Copied directories.
- [ ] Moved files.
- [ ] Renamed files.
- [ ] Removed practice files and directories.
- [ ] Inspected file metadata.
- [ ] Searched for files using `find`.
- [ ] Created symbolic and hard links.
- [ ] Completed the practical challenge.

**END OF LAB 02**
