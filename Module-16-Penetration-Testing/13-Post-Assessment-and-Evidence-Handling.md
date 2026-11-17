---
---


# MODULE 16 – LESSON 13
# POST-ASSESSMENT AND EVIDENCE HANDLING

## 1. Introduction

After completing security testing, a penetration tester must carefully document findings, preserve evidence, remove temporary testing artifacts where appropriate, and communicate results to the organization.

This stage is known as post-assessment.

The goal is to ensure that the assessment ends safely and that the organization receives useful information for improving its security.

### Simple Explanation

Imagine a doctor examining a patient.

After the examination, the doctor:
- Records observations.
- Identifies problems.
- Explains the findings.
- Recommends treatment.
- Keeps appropriate records.

A penetration tester follows a similar process.

After security testing, the tester records vulnerabilities, explains their impact, preserves evidence, and recommends remediation.

## 2. What Is Post-Assessment?

Post-assessment is the process of completing and reviewing all activities after penetration testing.

It includes:

1. Reviewing findings.
2. Validating evidence.
3. Removing authorized temporary artifacts.
4. Restoring systems when necessary.
5. Reviewing access and accounts used during testing.
6. Protecting collected information.
7. Preparing the final report.
8. Communicating remediation recommendations.

## 3. Evidence in Cybersecurity

Evidence is information that supports a security finding.

It helps demonstrate that a vulnerability exists or that a security control is working correctly.

Examples include:

- Screenshots.
- Terminal output.
- Application logs.
- Configuration records.
- Network observations.
- Error messages.
- File metadata.
- Hash values.

### Why Evidence Matters

Evidence helps:
- Confirm findings.
- Support risk assessments.
- Avoid unsupported conclusions.
- Help developers reproduce problems.
- Demonstrate remediation.
- Maintain accountability.

A professional report should distinguish confirmed findings from assumptions.

## 4. Types of Evidence

| Evidence Type | Example |
|---|---|
| Screenshot | Image showing an application error |
| Log file | Authentication failure records |
| Command output | Results of local system inspection |
| Configuration | File showing a security setting |
| Network evidence | Captured authorized traffic |
| Hash | SHA-256 value used to check file integrity |

Evidence should be relevant, accurate, and collected within the approved scope.

## 5. Evidence Integrity

Evidence integrity means ensuring that collected evidence has not been improperly altered.

One way to support integrity is cryptographic hashing.

A cryptographic hash produces a fixed-length value from a file.

For example, SHA-256 can be used to calculate a file's fingerprint.

If the file changes, its hash will normally change.

### Example

Suppose you collect a report named:

`assessment-report.txt`

You calculate its SHA-256 hash.

```bash
sha256sum assessment-report.txt
```

You can later calculate the hash again and compare the results.

Matching hashes provide evidence that the file contents have not changed, assuming the original hash was securely recorded.

Important: A hash alone does not prove who created a file, when it was created, or whether it was trustworthy before hashing.

## 6. Chain of Custody

Chain of custody is the documented history of evidence handling.

It records:
- Who collected the evidence.
- When it was collected.
- Where it was stored.
- Who accessed it.
- Whether it was transferred.
- Whether it was modified.
- Who received the final evidence.

### Example

| Date and Time | Person | Action |
|---|---|---|
| 09:00 | Tester | Collected screenshot |
| 09:10 | Tester | Calculated SHA-256 hash |
| 09:20 | Tester | Stored evidence securely |
| 11:00 | Reviewer | Reviewed evidence |
| 12:00 | Tester | Included evidence reference in report |

Use consistent timestamps and identify the timezone.

## 7. Evidence Handling Principles

A professional security tester should follow these principles.

### A. Confidentiality

Keep evidence accessible only to authorized people.

### B. Integrity

Protect evidence against unauthorized modification.

### C. Availability

Ensure authorized reviewers can access the evidence when required.

### D. Data Minimization

Collect only information necessary for the assessment.

### E. Secure Storage

Use appropriate access controls and protected storage.

### F. Retention

Follow the organization's evidence-retention requirements.

Do not retain sensitive evidence indefinitely without a valid reason.

## 8. Data Classification

Organizations classify information according to sensitivity.

A common example is:

| Classification | Meaning |
|---|---|
| Public | Information approved for public access |
| Internal | Information intended for organizational use |
| Confidential | Sensitive business or personal information |
| Restricted | Highly sensitive information requiring strict access controls |

Penetration testing evidence may contain confidential or restricted information.

For example:
- Usernames.
- Email addresses.
- Authentication records.
- Internal IP addresses.
- Configuration details.
- Personal information.

Never include unnecessary secrets in a report.

## 9. Cleanup After an Assessment

Testing can sometimes create temporary files, accounts, sessions, or configuration changes.

After testing, the tester should review and remove only those artifacts that were authorized for removal.

Examples:
- Temporary test accounts.
- Test files.
- Temporary configuration changes.
- Test sessions.
- Test applications.

### Important

Do not blindly delete files or accounts.

Before cleanup:
1. Identify the artifact.
2. Confirm that it was created during the assessment.
3. Confirm that removal is authorized.
4. Check whether the organization needs it for investigation.
5. Record the cleanup action.

In a virtual lab, restoring a snapshot may be an appropriate recovery method.

## 10. Restoring Systems

A system should be returned to its agreed operational state.

Possible actions include:
- Restoring a VMware snapshot.
- Reverting approved configuration changes.
- Removing temporary test accounts.
- Confirming that services operate normally.
- Verifying that test data is removed where appropriate.

Always follow the agreed recovery plan.

A tester should not independently make production changes outside the approved scope.

## 11. Vulnerability Retesting

After an organization fixes a vulnerability, the tester may perform a retest.

The purpose is to verify whether the remediation was successful.

### Retesting Process

1. Review the original finding.
2. Understand the proposed remediation.
3. Confirm authorization for retesting.
4. Repeat the relevant low-risk validation.
5. Compare the new result with the original evidence.
6. Record the outcome.

Possible outcomes:

| Status | Meaning |
|---|---|
| Fixed | The original issue is no longer present |
| Partially Fixed | Some weakness remains |
| Not Fixed | The issue still exists |
| Unable to Verify | Testing could not establish the result |
| Regression | A previously resolved issue has returned |

## 12. Practical Lab A: Create an Evidence Directory

Create your lesson directory:

```bash
mkdir -p ~/module16-lab/lesson13/evidence
cd ~/module16-lab/lesson13
```

Create a sample evidence file:

```bash
echo "Sample evidence collected during a local training exercise." > evidence/sample-evidence.txt
```

View the file:

```bash
cat evidence/sample-evidence.txt
```

List its metadata:

```bash
stat evidence/sample-evidence.txt
```

This provides basic information about the file.

## 13. Practical Lab B: Calculate Evidence Hashes

Calculate the SHA-256 hash:

```bash
sha256sum evidence/sample-evidence.txt
```

Save the hash:

```bash
sha256sum evidence/sample-evidence.txt > evidence/sample-evidence.sha256
```

Verify the file:

```bash
sha256sum -c evidence/sample-evidence.sha256
```

Expected result:

```text
evidence/sample-evidence.txt: OK
```

The result indicates that the file currently matches the recorded hash.

## 14. Practical Lab C: Demonstrate Integrity Checking

Create another sample file:

```bash
echo "Original training evidence." > evidence/integrity-test.txt
```

Record its hash:

```bash
sha256sum evidence/integrity-test.txt > evidence/integrity-test.sha256
```

Verify it:

```bash
sha256sum -c evidence/integrity-test.sha256
```

Now modify the file:

```bash
echo "Additional training information." >> evidence/integrity-test.txt
```

Verify it again:

```bash
sha256sum -c evidence/integrity-test.sha256
```

The verification should fail because the file contents have changed.

This demonstrates why integrity checks are useful.

## 15. Practical Lab D: Create a Chain-of-Custody Record

Create a record:

```bash
nano evidence/chain-of-custody.txt
```

Enter:

```text
MODULE 16 - LESSON 13
CHAIN OF CUSTODY

Evidence ID:
Evidence Name:
Collection Date:
Collection Time:
Timezone:
Collected By:
Collection Method:
Original Location:
Storage Location:
SHA-256 Hash:
Accessed By:
Transfer History:
Final Status:
Additional Notes:
```

Use fictional training details or your own local lab evidence.

Do not include real passwords, private keys, or unnecessary personal information.

## 16. Practical Lab E: Organize Evidence

Your directory should resemble:

```text
lesson13/
│
├── evidence/
│   ├── sample-evidence.txt
│   ├── sample-evidence.sha256
│   ├── integrity-test.txt
│   ├── integrity-test.sha256
│   └── chain-of-custody.txt
│
└── evidence-handling-report.txt
```

Check the directory:

```bash
find ~/module16-lab/lesson13 -maxdepth 2 -type f
```

## 17. Evidence Handling Report

Create:

```bash
nano evidence-handling-report.txt
```

Use this structure:

```text
MODULE 16 - LESSON 13
POST-ASSESSMENT AND EVIDENCE HANDLING REPORT

1. ASSESSMENT INFORMATION

Assessment Name:
Assessment Date:
Authorized Scope:

2. EVIDENCE COLLECTION

Evidence ID:
Evidence Name:
Collection Method:
Collection Date:
Collected By:

3. INTEGRITY VERIFICATION

Hash Algorithm:
Original SHA-256:
Verification Result:

4. EVIDENCE STORAGE

Storage Location:
Access Controls:
Classification:
Retention Requirements:

5. CHAIN OF CUSTODY

Collected By:
Reviewed By:
Transferred To:
Transfer Date:
Final Status:

6. CLEANUP AND RECOVERY

Temporary Artifacts:
Cleanup Actions:
Snapshot Restoration:
System Verification:

7. RETESTING

Original Finding:
Remediation:
Retest Result:
Final Status:

8. CONCLUSION

Evidence Integrity:
Cleanup Status:
Outstanding Issues:
Recommendations:
```

## 18. Common Evidence Handling Mistakes

1. Editing original evidence without documenting the change.
2. Storing sensitive evidence in public folders.
3. Sharing screenshots containing passwords or personal information.
4. Failing to calculate integrity hashes.
5. Losing track of who accessed evidence.
6. Collecting unnecessary sensitive information.
7. Deleting evidence before authorization.
8. Failing to document cleanup activities.
9. Treating a matching hash as proof of authenticity.
10. Failing to verify remediation.

## 19. Knowledge Check

Answer these questions in your notebook.

1. What is post-assessment?
2. What is digital evidence?
3. Why is evidence important in penetration testing?
4. What is evidence integrity?
5. What is cryptographic hashing?
6. What is chain of custody?
7. Explain confidentiality in evidence handling.
8. What is data minimization?
9. Why should evidence be classified?
10. What is a cleanup process?
11. Why are VMware snapshots useful?
12. What is vulnerability retesting?
13. What does a SHA-256 verification result of OK mean?
14. Does a matching hash prove who created a file?
15. Why should sensitive evidence be protected?

## 20. Practical Thinking Questions

1. Why should original evidence be preserved?
2. What could happen if evidence is modified without documentation?
3. Why should an organization restrict access to penetration testing reports?
4. Why should cleanup activities be documented?
5. What is the difference between vulnerability validation and vulnerability retesting?
6. Why should a tester collect only necessary information?
7. Why is a secure evidence-storage location important?
8. What should a tester do if an unexpected sensitive file is discovered?

## 21. Lesson Summary

In this lesson, you learned:

- Post-assessment procedures.
- Digital evidence.
- Evidence integrity.
- SHA-256 verification.
- Chain of custody.
- Data classification.
- Confidentiality and secure storage.
- Evidence cleanup.
- System recovery.
- Vulnerability retesting.
- Evidence organization.
- Professional reporting.

### Key Takeaway

A penetration test is not complete when testing stops. Evidence must be protected, temporary changes must be handled responsibly, systems must be restored where required, and findings must be communicated accurately.

**NEXT LESSON: MODULE 16 – LESSON 14: PENETRATION TESTING REPORT AND PRACTICAL PROJECTS**
