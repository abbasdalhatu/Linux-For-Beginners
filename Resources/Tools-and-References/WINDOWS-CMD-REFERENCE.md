
# WINDOWS CMD REFERENCE

## 1. Introduction

Command Prompt (CMD) is a Windows command-line interface used to execute commands, manage files, inspect network settings, and troubleshoot computer problems.

## 2. Basic Navigation

Display the current directory:

```cmd
cd
```

List files and folders:

```cmd
dir
```

Change directory:

```cmd
cd Desktop
```

Move up one directory:

```cmd
cd ..
```

Change drive:

```cmd
D:
```

Clear the screen:

```cmd
cls
```

## 3. File and Folder Management

Create a directory:

```cmd
mkdir MyFolder
```

Create an empty file:

```cmd
type nul > notes.txt
```

Copy a file:

```cmd
copy notes.txt backup.txt
```

Move a file:

```cmd
move notes.txt MyFolder
```

Rename a file:

```cmd
ren old.txt new.txt
```

Delete a file:

```cmd
del notes.txt
```

Remove an empty directory:

```cmd
rmdir MyFolder
```

Be careful with deletion commands.

## 4. Network Diagnostics

Display network configuration:

```cmd
ipconfig /all
```

Test connectivity:

```cmd
ping example.com
```

Check DNS resolution:

```cmd
nslookup example.com
```

Display network routes:

```cmd
route print
```

Display active connections:

```cmd
netstat -ano
```

Display the ARP table:

```cmd
arp -a
```

## 5. Process Management

Display running processes:

```cmd
tasklist
```

Find a process:

```cmd
tasklist | findstr chrome
```

Display process information:

```cmd
tasklist /v
```

Terminate a process using its PID:

```cmd
taskkill /PID 1234 /F
```

Replace 1234 with the correct process ID. Force termination can cause unsaved work to be lost.

## 6. System Information

Display system information:

```cmd
systeminfo
```

Display computer name:

```cmd
hostname
```

Display Windows version:

```cmd
ver
```

Display current username:

```cmd
whoami
```

Display date and time:

```cmd
date /t
time /t
```

## 7. Windows Troubleshooting

Check and repair protected system files:

```cmd
sfc /scannow
```

Check Windows image health:

```cmd
DISM /Online /Cleanup-Image /CheckHealth
```

Scan the Windows image:

```cmd
DISM /Online /Cleanup-Image /ScanHealth
```

Repair the Windows image:

```cmd
DISM /Online /Cleanup-Image /RestoreHealth
```

Check a drive for filesystem errors:

```cmd
chkdsk C:
```

Some repair operations require administrator privileges or a restart.

## 8. Useful Windows Tools

Open Task Manager:

```cmd
taskmgr
```

Open Device Manager:

```cmd
devmgmt.msc
```

Open Disk Management:

```cmd
diskmgmt.msc
```

Open Event Viewer:

```cmd
eventvwr.msc
```

Open Services:

```cmd
services.msc
```

Open System Configuration:

```cmd
msconfig
```

## 9. Command-Line Safety

1. Understand a command before executing it.
2. Verify file paths before deleting or moving files.
3. Avoid running unknown scripts.
4. Use administrator privileges only when necessary.
5. Back up important files before major repairs.
6. Never share passwords or recovery keys.

## 10. Summary

CMD is useful for Windows administration and troubleshooting.

Important command groups include:
- Navigation and file management.
- Network diagnostics.
- Process management.
- System information.
- Windows repair and diagnostics.

Learning CMD builds a strong foundation for system administration and cybersecurity.
