# Module 06 – File and Directory Management

## Lesson 13: Hard Links and Symbolic Links

### 1. Learning Objectives

By the end of this lesson, you should be able to:

- Understand what links are in Linux.
- Explain hard links.
- Explain symbolic links.
- Create hard links using `ln`.
- Create symbolic links using `ln -s`.
- Identify links using `ls -l` and `stat`.
- Understand the differences between hard links and symbolic links.
- Apply links in Linux administration and cybersecurity.

### 2. Introduction to Linux Links

In Linux, a link allows you to access a file through another name or path.

Think of a link as another way to reach a file.

Linux supports two major types of links:

1. Hard links.
2. Symbolic links, also called soft links.

They are useful for organising files, sharing access to data, and creating convenient paths.

### 3. Understanding Inodes Before Links

Before understanding links, remember what you learned in Lesson 12.

An inode is a filesystem data structure that stores information about a file.

A directory entry connects a filename to an inode.

For example:

```text
Filename: report.txt
       |
       v
    Inode 12345
       |
       v
    File Data
```

A hard link creates another directory entry that points to the same inode.

This is the foundation of hard links.

### 4. Understanding Hard Links

A hard link is another filename that points directly to the same inode as the original file.

Example:

```text
report.txt ─────┐
                |
                v
             Inode 12345
                |
                v
              Data
                ^
                |
backup.txt ────┘
```

Both filenames refer to the same underlying file data.

If you modify the content through one filename, the change is visible through the other.

### 5. Creating a Hard Link

The `ln` command creates links.

Syntax:

```bash
ln TARGET LINK_NAME
```

Example:

```bash
ln report.txt backup.txt
```

This creates a hard link called `backup.txt` pointing to the same inode as `report.txt`.

Verify:

```bash
ls -li
```

You should see that both files have the same inode number.

Example:

```text
12345 -rw-r--r-- 2 user user 100 report.txt
12345 -rw-r--r-- 2 user user 100 backup.txt
```

Notice:

- Both files have inode number `12345`.
- The link count is `2`.

### 6. Understanding Hard Link Behaviour

Hard links have important properties:

- They share the same inode.
- They refer to the same underlying data.
- Editing one changes the content seen through the other.
- Removing one filename does not necessarily remove the data.
- The data remains accessible while at least one hard link exists.

For example:

```bash
rm report.txt
```

If `backup.txt` is still present, the data remains accessible through `backup.txt`.

### 7. Limitations of Hard Links

Hard links have some restrictions:

1. Hard links generally cannot cross filesystem boundaries.
2. Ordinary users cannot create hard links to directories.
3. They point directly to an inode rather than another pathname.
4. They do not behave like shortcuts to directories.

These restrictions help maintain filesystem consistency.

### 8. Understanding Symbolic Links

A symbolic link, also called a soft link, is a special file that stores a pathname pointing to another file or directory.

Think of it like a shortcut.

Example:

```text
original.txt
     |
     v
symbolic-link.txt
```

Unlike a hard link, a symbolic link has its own inode.

It points to the target using a pathname.

### 9. Creating a Symbolic Link

Use `ln` with the `-s` option.

Syntax:

```bash
ln -s TARGET LINK_NAME
```

Example:

```bash
ln -s report.txt report-shortcut.txt
```

This creates a symbolic link called `report-shortcut.txt` pointing to `report.txt`.

Verify:

```bash
ls -l
```

Possible output:

```text
lrwxrwxrwx 1 user user 10 report-shortcut.txt -> report.txt
```

The arrow indicates the target of the symbolic link.

The first character `l` indicates that the entry is a symbolic link.

### 10. Understanding Symbolic Link Behaviour

Symbolic links have different properties from hard links:

- They have their own inode.
- They store the target pathname.
- They can point to directories.
- They can cross filesystem boundaries.
- They can become broken if the target is removed or moved.

A symbolic link that points to a missing target is called a **broken symbolic link**.

### 11. Hard Links vs Symbolic Links

| Feature | Hard Link | Symbolic Link |
|---|---|---|
| Shares target inode | Yes | No |
| Has its own inode | No, shares target inode | Yes |
| Stores target pathname | No | Yes |
| Can point to directories | Normally no | Yes |
| Can cross filesystems | No | Yes |
| Can become broken when target is removed | No, if another hard link remains | Yes |
| Created with | `ln` | `ln -s` |

### 12. Useful Commands for Working with Links

**Create a hard link:**

```bash
ln original.txt hardlink.txt
```

**Create a symbolic link:**

```bash
ln -s original.txt symlink.txt
```

**Display link information:**

```bash
ls -l
```

**Display inode numbers:**

```bash
ls -li
```

**Inspect link metadata:**

```bash
stat symlink.txt
```

**Inspect the symbolic link itself rather than its target:**

```bash
stat -c '%N %i' symlink.txt
```

**Display the target of a symbolic link:**

```bash
readlink symlink.txt
```

### 13. Practical Linux Lab

We will create both link types and compare their behaviour.

#### Step 1: Create the practice directory

```bash
mkdir -p ~/module06-lab/links
cd ~/module06-lab/links
```

#### Step 2: Create an original file

```bash
echo "Linux filesystem link practice." > original.txt
```

Verify:

```bash
cat original.txt
```

#### Step 3: Create a hard link

```bash
ln original.txt hardlink.txt
```

Verify:

```bash
ls -li
```

Observe that `original.txt` and `hardlink.txt` share the same inode number.

#### Step 4: Create a symbolic link

```bash
ln -s original.txt symlink.txt
```

Verify:

```bash
ls -l
```

You should see:

```text
symlink.txt -> original.txt
```

#### Step 5: Compare inode numbers

```bash
ls -li
```

Observe:

- `original.txt` and `hardlink.txt` share an inode.
- `symlink.txt` has a different inode.

#### Step 6: Read through the hard link

```bash
cat hardlink.txt
```

#### Step 7: Read through the symbolic link

```bash
cat symlink.txt
```

Both should display the original content.

#### Step 8: Modify the file through the hard link

```bash
echo "This content was added through the hard link." >> hardlink.txt
```

Now inspect:

```bash
cat original.txt
cat hardlink.txt
cat symlink.txt
```

The new content should appear through all three names.

#### Step 9: Remove the original filename

```bash
rm original.txt
```

Now inspect:

```bash
cat hardlink.txt
cat symlink.txt
```

Expected behaviour:

- `hardlink.txt` still works because it refers directly to the shared inode.
- `symlink.txt` is broken because its target pathname, `original.txt`, no longer exists.

#### Step 10: Verify the broken symbolic link

```bash
ls -l
```

You should see the symbolic link pointing to `original.txt`, even though that pathname is missing.

You can also run:

```bash
readlink symlink.txt
```

#### Step 11: Clean up the practice directory

Remove the remaining test links:

```bash
rm hardlink.txt symlink.txt
```

Verify:

```bash
ls -la
```

Your practice directory should now be empty apart from `.` and `..`.

### 14. Cybersecurity Applications

Understanding links is important in system administration and security investigations.

**A. Linux System Administration**

Symbolic links provide convenient paths to commonly used files and directories.

**B. File Organisation**

Hard links can provide multiple filenames for the same file data.

**C. Digital Forensics**

Investigators can examine inode numbers and link relationships when analysing filesystem structures.

**D. Security Monitoring**

Unexpected symbolic links may be worth investigating, especially when they point to sensitive files or unusual locations.

**E. Suspicious File Analysis**

Analysts should understand that deleting a filename does not necessarily remove the underlying data if another hard link exists.

**Security note:** A symbolic link can point to sensitive files. Before opening or modifying an unfamiliar link, inspect its target using `readlink` and verify that the target is safe to access.

### 15. Common Mistakes

| Mistake | Explanation |
|---|---|
| Assuming hard links have separate data | They share the same inode and underlying data. |
| Assuming symbolic links share an inode | A symbolic link has its own inode. |
| Forgetting `-s` | Without it, `ln` creates a hard link by default. |
| Removing a symbolic link's target | The symbolic link may become broken. |
| Assuming a filename is the actual file | Different names can refer to the same inode. |

### 16. Knowledge Check

Answer these questions:

1. What is a hard link?
2. What is a symbolic link?
3. Which command creates a hard link?
4. Which command creates a symbolic link?
5. What is an inode?
6. Do hard links share the same inode?
7. Does a symbolic link have its own inode?
8. Can symbolic links point to directories?
9. What is a broken symbolic link?
10. Why might deleting one hard-linked filename not delete the underlying data?

### 17. Lesson Summary

In this lesson, you learned:

- Linux supports hard links and symbolic links.
- Hard links share the same inode.
- Symbolic links have their own inode.
- `ln` creates hard links.
- `ln -s` creates symbolic links.
- `ls -li` helps compare inode numbers.
- `readlink` displays a symbolic link's target.
- Symbolic links can become broken when their targets disappear.

**Next Lesson:** Lesson 14 – Wildcards and File Patterns.