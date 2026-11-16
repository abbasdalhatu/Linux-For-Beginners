
# LINUX COMMANDS REFERENCE

**Category:** Linux Administration  
**Level:** Beginner to Intermediate

---

## 1. Directory Navigation

| Command | Description |
|---|---|
| pwd | Display current directory |
| ls | List directory contents |
| ls -la | List all files with details |
| cd / | Navigate to root |
| cd .. | Move one directory backward |
| cd ~ | Navigate to home directory |
| clear | Clear terminal screen |

## 2. File Management

| Command | Description |
|---|---|
| touch file.txt | Create an empty file |
| cat file.txt | Display file contents |
| less file.txt | Read a file page by page |
| cp file.txt backup.txt | Copy a file |
| mv old.txt new.txt | Move or rename a file |
| rm file.txt | Delete a file |
| file file.txt | Identify file type |

## 3. Directory Management

| Command | Description |
|---|---|
| mkdir folder | Create a directory |
| mkdir -p a/b/c | Create nested directories |
| rmdir folder | Remove an empty directory |
| rm -r folder | Remove a directory and contents |
| tree | Display directory structure |

## 4. Permissions

| Command | Description |
|---|---|
| ls -l | View permissions |
| chmod 755 file | Set permissions |
| chmod +x script.sh | Make script executable |
| chmod 644 file | Set standard file permissions |
| chown user file | Change file owner |
| umask | Display default permission mask |

## 5. System Information

| Command | Description |
|---|---|
| uname -a | Display kernel information |
| hostname | Display hostname |
| whoami | Display current username |
| id | Display user and group IDs |
| uptime | Show system uptime |
| free -h | Display memory usage |
| df -h | Display disk usage |
| lsblk | Display storage devices |

## 6. Process Management

| Command | Description |
|---|---|
| ps aux | List running processes |
| top | Monitor processes |
| htop | Interactive process monitor |
| pgrep name | Find process by name |
| kill PID | Send termination signal |
| jobs | Display background jobs |
| bg | Resume job in background |
| fg | Resume job in foreground |

## 7. Package Management (Debian/Kali)

| Command | Description |
|---|---|
| sudo apt update | Refresh package lists |
| sudo apt upgrade | Upgrade installed packages |
| sudo apt install package | Install a package |
| sudo apt remove package | Remove a package |
| apt search keyword | Search packages |
| apt show package | View package information |
| dpkg -l | List installed packages |

## 8. Help and Documentation

| Command | Description |
|---|---|
| man command | Open manual |
| command --help | Display command help |
| whatis command | Short command description |
| apropos keyword | Search manual descriptions |
| history | Display command history |

## 9. Important Safety Notes

- Check a command before executing it.
- Be careful with `rm -r` and `rm -rf`.
- Avoid running unfamiliar scripts as root.
- Do not change permissions unnecessarily.
- Back up important files before destructive operations.

## 10. Practice Exercises

1. Display your current directory.
2. List all hidden files.
3. Create a directory called practice.
4. Create a file inside it.
5. Display the file contents.
6. Change the file permissions to 644.
7. Display your Linux kernel information.
8. Check available disk space.

---

**End of Linux Commands Reference**
