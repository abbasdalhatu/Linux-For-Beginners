
# BASH FILE AND DIRECTORY AUTOMATION

## 1. Introduction

Bash can automate file and directory management tasks on Linux.

Instead of performing repetitive operations manually, scripts can create, rename, copy, move, and delete files.

## 2. Creating Directories

Use `mkdir` to create a directory.

```bash
mkdir documents
```

Create multiple directories:

```bash
mkdir reports backups scripts
```

Create nested directories:

```bash
mkdir -p projects/linux/scripts
```

## 3. Creating Files

Use `touch` to create an empty file.

```bash
touch notes.txt
```

Create multiple files:

```bash
touch file1.txt file2.txt file3.txt
```

## 4. Copying Files

Use `cp` to copy files.

```bash
cp notes.txt notes-backup.txt
```

Copy a file into another directory:

```bash
cp notes.txt documents/
```

Copy a directory and its contents:

```bash
cp -r projects/ projects-backup/
```

## 5. Moving and Renaming Files

Use `mv` to move or rename files.

Rename a file:

```bash
mv oldname.txt newname.txt
```

Move a file:

```bash
mv notes.txt documents/
```

## 6. Deleting Files and Directories

Delete a file:

```bash
rm notes.txt
```

Delete an empty directory:

```bash
rmdir documents
```

Delete a directory and its contents:

```bash
rm -r documents
```

CAUTION: Deletion commands can permanently remove data. Always check the target path before running them.

## 7. Listing Files

Use `ls` to display directory contents.

```bash
ls
```

Show detailed information:

```bash
ls -l
```

Include hidden files:

```bash
ls -la
```

## 8. Checking Whether a File Exists

Bash provides conditional expressions for checking files.

```bash
#!/bin/bash

if [ -f "notes.txt" ]; then
    echo "File exists."
else
    echo "File does not exist."
fi
```

Common file tests:

| Test | Meaning |
|---|---|
| -f | Regular file exists |
| -d | Directory exists |
| -e | File or directory exists |
| -r | File is readable |
| -w | File is writable |
| -x | File is executable |

## 9. Automated Directory Backup

This script copies a directory into a backup location.

```bash
#!/bin/bash

SOURCE="documents"
DESTINATION="backups"

mkdir -p "$DESTINATION"

if [ -d "$SOURCE" ]; then
    cp -r "$SOURCE" "$DESTINATION/"
    echo "Backup completed."
else
    echo "Source directory does not exist."
fi
```

## 10. Organizing Files by Extension

The following example moves text files into a directory named `text-files`.

```bash
#!/bin/bash

mkdir -p text-files

for file in *.txt
do
    if [ -f "$file" ]; then
        mv -- "$file" text-files/
    fi
done

echo "Text files organized."
```

## 11. Important Best Practices

1. Use quotation marks around paths and variables.
2. Check that files and directories exist before operating on them.
3. Avoid unnecessary recursive deletion.
4. Test scripts using sample files first.
5. Keep backups of important information.
6. Review commands before running automated scripts.

## 12. Summary

Bash file automation makes Linux administration easier.

Important commands:

- `mkdir`: Create directories.
- `touch`: Create empty files.
- `cp`: Copy files and directories.
- `mv`: Move or rename files.
- `rm`: Delete files.
- `rmdir`: Remove empty directories.
- `ls`: List directory contents.

These commands are useful for everyday Linux administration and automation.
```

Save the file and reply **done**.