
# MODULE 14 – DIGITAL FORENSICS

# LESSON 06: METADATA AND FILE ANALYSIS

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Define file metadata.
2. Identify different categories of metadata.
3. Explain EXIF metadata in images.
4. Understand document metadata.
5. Identify timestamps and their investigative importance.
6. Use ExifTool and Linux commands to inspect files.
7. Identify suspicious file characteristics.
8. Produce a basic metadata analysis report.

---

## 2. What Is File Metadata?

Metadata is information that describes another piece of information.

For example, a photograph contains visual information, but its metadata may contain:

- Camera manufacturer.
- Camera model.
- Image dimensions.
- Date and time recorded by the camera.
- Software used to edit the image.
- GPS coordinates, if recorded.

In simple terms:

**Metadata is data about data.**

During a forensic investigation, metadata can provide additional context about a file.

---

## 3. Types of File Metadata

### 3.1 File-System Metadata

Information maintained by the file system.

Examples:
- Filename.
- File size.
- Ownership.
- Permissions.
- Modification timestamp.
- Access timestamp.

Linux inspection:

```bash
stat filename.txt
```

### 3.2 Image Metadata

Images may contain EXIF metadata.

EXIF means Exchangeable Image File Format.

It may include:

- Camera make.
- Camera model.
- Image dimensions.
- Exposure settings.
- Date and time.
- GPS location.

Not every image contains all these fields.

### 3.3 Document Metadata

Documents may contain:

- Author name.
- Document title.
- Creation date.
- Modification date.
- Software used.
- Revision information.

### 3.4 Audio and Video Metadata

Media metadata may include:

- Duration.
- Codec.
- Bitrate.
- Resolution.
- Frame rate.
- Creation information.

---

## 4. Understanding File Timestamps

Timestamps can help investigators construct a timeline.

On Linux, common timestamp categories include:

| Timestamp | Meaning |
|---|---|
| atime | Last access time |
| mtime | Last content modification time |
| ctime | Last inode status change time |
| Birth time | File creation time, when supported |

Important:

Linux ctime is not the file creation time.

Timestamps must be interpreted carefully because they may be changed by applications, copying, system configuration, or user actions.

A timestamp alone does not establish who performed an action.

---

## 5. EXIF Metadata

EXIF metadata is commonly found in photographs and other image files.

Example:

A photograph might contain:

```text
Camera Make: Canon
Camera Model: EOS
Image Width: 4000
Image Height: 3000
Date Time Original: Recorded timestamp
GPS Position: May be present
```

### Why Is EXIF Important?

EXIF can help an investigator:

- Identify the camera model.
- Understand image properties.
- Compare image timestamps.
- Examine possible location information.
- Identify editing software.

### Important Privacy Warning

Photographs can contain GPS coordinates that reveal sensitive locations.

Do not publish or share private image metadata without authorization.

---

## 6. Document Metadata

Documents may contain information that is not visible in the main text.

For example, a word-processing document might contain:

- Author.
- Last modified by.
- Application name.
- Revision count.
- Creation timestamp.

Investigators may examine this information to understand document history.

However, metadata can be edited, removed, or inherited from templates. It should be corroborated with other evidence.

---

## 7. File Signature Analysis

A file extension does not always reveal the actual file format.

For example:

A file named:

`report.jpg`

may contain data that is not actually a JPEG image.

File signatures, sometimes called magic bytes, help identify formats from their internal structure.

Linux command:

```bash
file report.jpg
```

This examines file content rather than trusting only the filename extension.

### Why Is This Important?

A misleading file extension may be relevant when investigating:

- Suspicious downloads.
- Unusual attachments.
- Renamed files.
- Potentially disguised malware samples.

Never execute suspicious files during examination.

---

## 8. Useful Metadata Analysis Tools

| Tool | Purpose |
|---|---|
| ExifTool | Reads and writes metadata |
| file | Identifies file types |
| stat | Displays file metadata |
| strings | Extracts readable text from binary files |
| mediainfo | Examines audio and video metadata |
| binwalk | Identifies embedded file signatures and data structures |

For forensic examination, prefer read-only analysis of acquired copies.

---

## 9. Installing ExifTool in Kali

ExifTool is a widely used metadata analysis tool.

Check whether it is installed:

```bash
exiftool -ver
```

If unavailable, install it from Kali's official repositories:

```bash
sudo apt update
sudo apt install libimage-exiftool-perl
```

Verify:

```bash
exiftool -ver
```

---

## 10. Practical Lab: Metadata Investigation

### Objective

Create practice files, inspect their metadata, identify their file types, and prepare a metadata report.

Only use files you create yourself.

### Step 1: Create a Lab Directory

```bash
mkdir -p ~/practical-labs/module14/lab06/evidence
cd ~/practical-labs/module14/lab06
```

### Step 2: Create a Text File

```bash
echo "Digital Forensics Metadata Practice" > evidence/report.txt
```

### Step 3: Inspect File-System Metadata

```bash
stat evidence/report.txt
```

Record:

- File size.
- Permissions.
- Owner.
- Inode.
- Modification time.
- Change time.

### Step 4: Identify the File Type

```bash
file evidence/report.txt
```

Expected output should identify it as an ASCII or UTF-8 text file.

### Step 5: Generate a Sample Image

Check whether Python is available:

```bash
python3 --version
```

Create a small image with Python:

```bash
python3 - <<'PY'
import struct
import zlib

width, height = 2, 2
raw = b''.join(
    b'\x00' + bytes([255, 0, 0] * width)
    for _ in range(height)
)

def chunk(kind, data):
    body = kind + data
    return (
        struct.pack(">I", len(data)) +
        body +
        struct.pack(">I", zlib.crc32(body) & 0xffffffff)
    )

png = (
    b'\x89PNG\r\n\x1a\n' +
    chunk(b'IHDR', struct.pack(">IIBBBBB",
                               width, height, 8, 2, 0, 0, 0)) +
    chunk(b'IDAT', zlib.compress(raw)) +
    chunk(b'IEND', b'')
)

with open("evidence/sample.png", "wb") as f:
    f.write(png)
PY
```

This creates a tiny PNG image for analysis.

### Step 6: Identify the Image Format

```bash
file evidence/sample.png
```

### Step 7: Examine Image Metadata

```bash
exiftool evidence/sample.png
```

The image may have limited metadata because it was generated without a camera.

That is normal.

### Step 8: Inspect All Evidence Files

```bash
exiftool evidence/
```

### Step 9: Extract Readable Strings

```bash
strings evidence/sample.png
```

The output may contain readable PNG chunk names or other text.

### Step 10: Create a Metadata Report

```bash
cat > metadata-report.txt <<'EOF'
DIGITAL FORENSIC METADATA REPORT

Investigation ID: META-001

Evidence examined:
1. report.txt
2. sample.png

Tools:
- stat
- file
- ExifTool
- strings

Activities:
- Examined file-system metadata.
- Identified file types.
- Inspected image metadata.
- Extracted readable strings.

Observations:
The text file contains practice information.
The PNG file was generated locally.
The image contains limited metadata.

Scope:
Self-created training files only.

Conclusion:
Metadata provides useful information about files,
but findings must be interpreted in context.
EOF
```

### Step 11: Review Your Workspace

```bash
find . -maxdepth 2 -type f
```

Expected:

```text
./evidence/report.txt
./evidence/sample.png
./metadata-report.txt
```

---

## 11. Practical Investigation Challenge

Answer these questions:

1. What is metadata?
2. What is EXIF?
3. Name four fields that may appear in image metadata.
4. What is the difference between mtime and ctime?
5. Why should investigators inspect file signatures?
6. Which tool is commonly used to examine image metadata?
7. Can metadata be modified?
8. Why should investigators avoid relying on timestamps alone?

---

## 12. Knowledge Check

**Question 1:** What does EXIF commonly describe?

A. Image metadata  
B. Network routing  
C. User passwords  
D. Firewall rules

**Question 2:** Which command displays Linux file metadata?

A. stat  
B. ping  
C. ip route  
D. whoami

**Question 3:** Which tool is commonly used to examine image metadata?

A. ExifTool  
B. UFW  
C. Nmap  
D. Bash

**Question 4:** What does the Linux mtime represent?

A. Last content modification time  
B. Last computer boot time  
C. File creation time in every case  
D. Last network access

**Question 5:** Why use the `file` command?

A. To delete files  
B. To identify file types from their contents  
C. To encrypt documents  
D. To create user accounts

---

## 13. Lesson Completion Checklist

- [ ] I understand file metadata.
- [ ] I understand EXIF metadata.
- [ ] I understand document metadata.
- [ ] I understand file timestamps.
- [ ] I understand file signatures.
- [ ] I installed or verified ExifTool.
- [ ] I examined a text file.
- [ ] I examined a PNG image.
- [ ] I created a metadata report.
- [ ] I completed the knowledge check.

---

## 14. Key Takeaway

Metadata provides context about digital files and can help investigators reconstruct events, identify file properties, and develop investigative timelines.

However, metadata can be incomplete or altered.

**Remember: Metadata is valuable investigative evidence, but it must always be interpreted and corroborated carefully.**
