
# MODULE 14 – DIGITAL FORENSICS

# LESSON 04: FILE HASHING AND INTEGRITY

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Explain cryptographic hashing.
2. Understand how hash functions work.
3. Identify common hashing algorithms.
4. Explain hash collisions.
5. Understand the importance of SHA-256 in digital forensics.
6. Generate and verify file hashes in Linux.
7. Compare original files with forensic copies.
8. Create an evidence integrity report.

---

## 2. What Is Cryptographic Hashing?

Cryptographic hashing is a process that converts input data into a fixed-length output called a hash or digest.

In simple terms:

A hash is like a digital fingerprint for a file.

Every file produces a hash value based on its contents.

Example:

Input:

```text
Digital Forensics
```

Output:

```text
A fixed-length hash value
```

If the contents change, the resulting hash will normally change.

### Important Characteristics

A secure cryptographic hash function should have these properties:

**1. Deterministic**

The same input produces the same hash.

**2. Fixed-length output**

The output has a fixed size for a given algorithm, regardless of input size.

**3. Avalanche effect**

A small change in the input should produce a substantially different output.

**4. Preimage resistance**

It should be computationally infeasible to recover an input from its hash.

**5. Collision resistance**

It should be computationally difficult to find two different inputs that produce the same hash.

---

## 3. Common Hashing Algorithms

### 3.1 MD5

MD5 produces a 128-bit digest.

It is outdated and vulnerable to practical collision attacks.

Do not rely on MD5 when strong integrity or security guarantees are required.

### 3.2 SHA-1

SHA-1 produces a 160-bit digest.

It is also considered cryptographically broken for collision resistance.

It should not be selected for new security-sensitive applications.

### 3.3 SHA-256

SHA-256 belongs to the SHA-2 family.

It produces a 256-bit digest.

It is widely used for file integrity verification.

Example command:

```bash
sha256sum report.txt
```

### 3.4 SHA-512

SHA-512 belongs to the SHA-2 family.

It produces a 512-bit digest.

Example:

```bash
sha512sum report.txt
```

### Comparison Table

| Algorithm | Digest size | Current recommendation |
|---|---:|---|
| MD5 | 128 bits | Avoid for security |
| SHA-1 | 160 bits | Avoid for security |
| SHA-256 | 256 bits | Suitable for integrity verification |
| SHA-512 | 512 bits | Suitable for integrity verification |

---

## 4. What Is a Hash Collision?

A hash collision occurs when two different inputs produce the same hash value.

Mathematically:

\[
H(A)=H(B), \quad A\ne B
\]

Where:

- H is the hash function.
- A is the first input.
- B is the second input.

For a cryptographic hash, finding such collisions should be computationally infeasible.

However, MD5 and SHA-1 have known collision weaknesses.

This is one reason SHA-256 is preferred for modern integrity verification.

---

## 5. Why Hashing Is Important in Digital Forensics

Hashing helps investigators verify that evidence has not changed between documented checkpoints.

### Example Scenario

An investigator receives a storage device and creates a forensic image.

The investigator calculates the SHA-256 hash of the acquired image.

Later, the investigator calculates the hash again.

If both hashes match, this supports the conclusion that the image contents remained unchanged between those checks.

### Common Uses

1. Verifying forensic images.
2. Checking copied evidence files.
3. Detecting accidental modification.
4. Supporting evidence documentation.
5. Comparing files.
6. Identifying known files using trusted hash databases.

A matching hash alone does not prove that evidence is authentic, that it came from a particular device, or that it was lawfully collected. Those questions require additional documentation and investigation.

---

## 6. Hashing in Linux

Linux provides several useful commands.

### Generate SHA-256

```bash
sha256sum filename.txt
```

### Generate SHA-512

```bash
sha512sum filename.txt
```

### Generate MD5

```bash
md5sum filename.txt
```

Use MD5 only for non-security demonstrations or compatibility checks, not as a modern security integrity guarantee.

### Save a SHA-256 Hash

```bash
sha256sum filename.txt > filename.sha256
```

### Verify a Saved Hash

```bash
sha256sum -c filename.sha256
```

Possible output:

```text
filename.txt: OK
```

### Hash Multiple Files

```bash
sha256sum file1.txt file2.txt file3.txt
```

### Hash Every File in a Directory

```bash
find evidence/ -type f -exec sha256sum {} \;
```

---

## 7. File Comparison Using Hashes

Suppose you have two files:

- original.txt
- copy.txt

Calculate their hashes:

```bash
sha256sum original.txt copy.txt
```

If the hashes match, the contents are identical with extremely high confidence.

If the hashes differ, the contents are different.

### Example

Original:

```text
Hello Digital Forensics
```

Copy:

```text
Hello Digital Forensics
```

Their hashes should match.

Now change the copy:

```text
Hello Digital Forensics Lab
```

The hashes should differ.

This demonstrates the avalanche effect.

---

## 8. Hashing vs Encryption

These two concepts are different.

| Hashing | Encryption |
|---|---|
| One-way operation | Reversible with the correct key |
| Produces a digest | Produces ciphertext |
| Used for integrity checks | Used for confidentiality |
| No decryption key | Requires an appropriate decryption key |

Example:

Hashing:

```text
report.txt → SHA-256 → Hash
```

Encryption:

```text
report.txt → Encryption key → Encrypted file
```

Remember: Hashing is not encryption.

---

## 9. Practical Lab: Evidence Hash Verification

### Objective

Generate hashes for sample evidence, verify file integrity, modify a copy, and produce an integrity report.

Use your existing Module 14 practice environment.

### Step 1: Navigate to Lab 01

```bash
cd ~/practical-labs/module14/lab01
```

### Step 2: Create a Dedicated Lab 04 Directory

```bash
mkdir -p ../lab04/evidence
cd ../lab04
```

### Step 3: Create an Original Evidence File

```bash
echo "Original forensic evidence - File E001" > evidence/original.txt
```

### Step 4: Create an Exact Copy

```bash
cp evidence/original.txt evidence/copy.txt
```

### Step 5: Generate SHA-256 Hashes

```bash
sha256sum evidence/original.txt evidence/copy.txt
```

Observe the two hash values.

They should match.

### Step 6: Save the Original Hash

```bash
sha256sum evidence/original.txt > evidence/original.sha256
```

### Step 7: Verify the Original

```bash
sha256sum -c evidence/original.sha256
```

Expected:

```text
evidence/original.txt: OK
```

### Step 8: Modify the Copy

```bash
echo "Modified evidence content" >> evidence/copy.txt
```

### Step 9: Generate Hashes Again

```bash
sha256sum evidence/original.txt evidence/copy.txt
```

The two hashes should now be different.

### Step 10: Verify the Original Again

```bash
sha256sum -c evidence/original.sha256
```

The original should still report:

```text
evidence/original.txt: OK
```

### Step 11: Create a Hash Inventory

```bash
sha256sum evidence/original.txt evidence/copy.txt > hash-inventory.txt
```

View it:

```bash
cat hash-inventory.txt
```

### Step 12: Create an Integrity Report

```bash
cat > integrity-report.txt <<'EOF'
DIGITAL EVIDENCE INTEGRITY REPORT

Investigation ID: HASH-001
Environment: Personal Kali practice lab

Evidence:
- original.txt
- copy.txt

Algorithm:
SHA-256

Test 1:
The original file and its exact copy were hashed.
Their hash values matched.

Test 2:
The copy was modified.
A new hash was generated.

Result:
The modified copy produced a different hash.

Conclusion:
The exercise demonstrated how SHA-256 can support
file integrity verification and reveal content changes.

Scope:
Self-created training files only.
EOF
```

### Step 13: Review the Workspace

```bash
find . -maxdepth 2 -type f
```

Your lab should contain:

```text
./evidence/original.txt
./evidence/copy.txt
./evidence/original.sha256
./hash-inventory.txt
./integrity-report.txt
```

---

## 10. Practical Investigation Challenge

Answer these questions:

1. What is cryptographic hashing?
2. Why does a hash have a fixed output length?
3. What is the avalanche effect?
4. What is a hash collision?
5. Why should investigators avoid MD5 for security-sensitive integrity checks?
6. What is the difference between SHA-256 and encryption?
7. What does `sha256sum -c` do?
8. Why is a matching hash useful during forensic acquisition?

---

## 11. Knowledge Check

**Question 1:** Which algorithm produces a 256-bit digest?

A. MD5  
B. SHA-1  
C. SHA-256  
D. Base64

**Question 2:** What is a hash collision?

A. Two different inputs produce the same hash  
B. A file is deleted  
C. A computer crashes  
D. A password is encrypted

**Question 3:** Which Linux command verifies a saved SHA-256 checksum?

A. ping  
B. sha256sum -c  
C. chmod  
D. lsblk

**Question 4:** Which statement about hashing is correct?

A. Hashing is normally reversible  
B. Hashing produces a fixed-length digest  
C. Hashing requires a decryption key  
D. Hashing always encrypts files

**Question 5:** Why is SHA-256 useful in digital forensics?

A. It repairs damaged disks  
B. It helps verify file integrity  
C. It creates user accounts  
D. It changes file permissions

---

## 12. Lesson Completion Checklist

- [ ] I understand cryptographic hashing.
- [ ] I understand SHA-256 and SHA-512.
- [ ] I understand hash collisions.
- [ ] I understand the avalanche effect.
- [ ] I can generate file hashes.
- [ ] I can save and verify checksums.
- [ ] I compared original and modified files.
- [ ] I created an integrity report.
- [ ] I completed the knowledge check.

---

## 13. Key Takeaway

Hashing provides a practical way to check whether digital evidence has changed.

In digital forensics, SHA-256 is an important tool for documenting and verifying the integrity of acquired files and forensic images.

**Remember: If the contents change, a secure hash will normally change too.**
