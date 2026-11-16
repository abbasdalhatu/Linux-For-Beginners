
# LINUX FILE SYSTEM REFERENCE

## 1. What Is a File System?

A file system is the method an operating system uses to organize, store, and retrieve files.

Linux uses a hierarchical file system that starts from the root directory `/`.

## 2. Linux Directory Structure

| Directory | Purpose |
|---|---|
| / | Root of the entire file system |
| /home | Personal user directories |
| /root | Home directory of the root user |
| /etc | System configuration files |
| /var | Logs and changing data |
| /tmp | Temporary files |
| /bin | Essential user commands |
| /sbin | System administration commands |
| /usr | Applications and shared resources |
| /opt | Optional software packages |
| /dev | Device files |
| /proc | Process and kernel information |
| /boot | Boot-related files |
| /mnt | Temporary mount points |
| /media | Removable media mount points |

## 3. Navigation Commands

Display your current directory:
```bash
pwd
```

List files:
```bash
ls
```

Show hidden files:
```bash
ls -a
```

Display detailed information:
```bash
ls -l
```

Move to the root directory:
```bash
cd /
```

Move to your home directory:
```bash
cd ~
```

Move one directory backward:
```bash
cd ..
```

## 4. Creating Files and Directories

Create a file:
```bash
touch notes.txt
```

Create a directory:
```bash
mkdir Documents
```

Create nested directories:
```bash
mkdir -p Projects/Linux/Notes
```

## 5. Copying, Moving and Removing

Copy a file:
```bash
cp notes.txt backup.txt
```

Move a file:
```bash
mv notes.txt Documents/
```

Rename a file:
```bash
mv oldname.txt newname.txt
```

Remove a file:
```bash
rm notes.txt
```

Remove an empty directory:
```bash
rmdir Documents
```

Remove a directory and its contents:
```bash
rm -r Projects
```

Be careful with `rm -r`; deleted files may be difficult or impossible to recover.

## 6. File Types

Linux supports different file types:

- Regular files: Documents, scripts, images and programs.
- Directories: Containers for files and other directories.
- Symbolic links: References to other files or directories.
- Device files: Interfaces to hardware and devices.
- Named pipes: Communication channels between processes.
- Sockets: Communication endpoints between processes.

Check a file type:
```bash
file notes.txt
```

## 7. Absolute and Relative Paths

### Absolute path

A complete path starting from `/`.

Example:
```bash
/home/user/Documents/notes.txt
```

### Relative path

A path based on your current working directory.

Example:
```bash
Documents/notes.txt
```

## 8. Mounting Storage Devices

Display storage devices:
```bash
lsblk
```

Display mounted file systems:
```bash
df -h
```

Display disk usage:
```bash
du -sh Documents
```

Display file system information:
```bash
mount
```

Mounting makes a storage device or file system accessible through a directory.

## 9. Important Special Locations

- `.` means the current directory.
- `..` means the parent directory.
- `~` represents the current user's home directory.
- `/` represents the root directory.

## 10. Cybersecurity Applications

Understanding the Linux file system helps security professionals:

- Locate system configuration files.
- Investigate suspicious files.
- Examine application logs.
- Understand where user data is stored.
- Identify unusual files and symbolic links.
- Investigate permissions and access restrictions.

## 11. Practice Exercises

1. Display the current working directory.
2. Navigate to `/etc`.
3. List all files, including hidden files.
4. Create a directory named SecurityLab.
5. Create a file inside SecurityLab.
6. Display available disk space.
7. Explain the difference between `/` and `/root`.

## Summary

The Linux file system organizes information in a hierarchical structure. Understanding its directories, paths, and storage commands is essential for Linux administration and cybersecurity.
