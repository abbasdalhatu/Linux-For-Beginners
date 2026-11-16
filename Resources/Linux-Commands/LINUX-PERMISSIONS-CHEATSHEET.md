
# LINUX FILE PERMISSIONS CHEAT SHEET

**Category:** Linux Administration  
**Level:** Beginner to Intermediate

---

## 1. Understanding Linux Permissions

Linux permissions determine who can read, write, or execute a file.

There are three permission groups:

| Symbol | Meaning |
|---|---|
| u | User (owner) |
| g | Group |
| o | Others |
| a | All users |

## 2. Permission Types

| Permission | Symbol | Numeric Value | Meaning |
|---|---|---:|---|
| Read | r | 4 | View file contents |
| Write | w | 2 | Modify file contents |
| Execute | x | 1 | Execute a file |

## 3. Understanding Permission Numbers

Permission numbers are calculated by adding values.

| Number | Permission |
|---:|---|
| 0 | --- |
| 1 | --x |
| 2 | -w- |
| 3 | -wx |
| 4 | r-- |
| 5 | r-x |
| 6 | rw- |
| 7 | rwx |

Example:

```bash
chmod 755 script.sh
```

Meaning:

- Owner: Read, write, execute.
- Group: Read, execute.
- Others: Read, execute.

## 4. Common Permission Settings

| Command | Meaning |
|---|---|
| chmod 777 file | Everyone has full permissions |
| chmod 755 file | Owner full access; others read and execute |
| chmod 700 file | Only owner has access |
| chmod 644 file | Owner reads/writes; others read |
| chmod 600 file | Only owner reads/writes |

Avoid using 777 unless there is a specific, justified requirement.

## 5. Symbolic Permission Commands

```bash
chmod u+x script.sh
chmod g-w file.txt
chmod o+r file.txt
chmod a+r file.txt
chmod u=rw,g=r,o= file.txt
```

## 6. Viewing Permissions

```bash
ls -l
stat file.txt
```

Example output:

```text
-rwxr-xr-- 1 user group 1200 script.sh
```

Breakdown:

- First character: File type.
- Next three: Owner permissions.
- Next three: Group permissions.
- Last three: Other permissions.

## 7. Changing Ownership

```bash
sudo chown user file.txt
sudo chown user:group file.txt
sudo chown -R user:group folder/
```

## 8. Special Permissions

| Permission | Numeric Value | Purpose |
|---|---:|---|
| SUID | 4000 | Execute with file owner's effective privileges |
| SGID | 2000 | Group inheritance or group execution privileges |
| Sticky bit | 1000 | Restricts deletion in shared directories |

Examples:

```bash
chmod 4755 file
chmod 2755 folder
chmod 1777 shared-folder
```

Use special permissions carefully.

## 9. Security Best Practices

1. Apply least privilege.
2. Avoid unnecessary write permissions.
3. Protect sensitive configuration files.
4. Review executable permissions.
5. Audit SUID and SGID files.
6. Avoid broad recursive permission changes.

## 10. Practice Exercises

1. Create a file called `test.txt`.
2. Set its permissions to 644.
3. Display its permissions.
4. Change the permissions to 600.
5. Create a script and make it executable.
6. Explain the difference between 755 and 700.

---

**End of Linux Permissions Cheat Sheet**
