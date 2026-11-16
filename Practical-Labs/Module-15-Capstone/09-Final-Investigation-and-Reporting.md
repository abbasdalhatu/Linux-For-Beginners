
# MODULE 15 – CAPSTONE PROJECT

# LESSON 09: FINAL INVESTIGATION AND REPORTING

## 1. Learning Objectives

By the end of this lesson, you should be able to:

1. Understand the complete cybersecurity investigation process.
2. Correlate evidence from multiple sources.
3. Review network discovery and security assessment results.
4. Analyze monitoring and forensic evidence.
5. Evaluate remediation activities.
6. Build an investigation timeline.
7. Prepare a professional cybersecurity report.
8. Present technical findings and recommendations.

---

## 2. What Is a Cybersecurity Investigation?

A cybersecurity investigation is a structured process used to examine suspicious activity, identify security weaknesses, understand incidents, and establish evidence-supported conclusions.

In this capstone, you have already performed several investigation activities:

- System discovery.
- Network enumeration.
- Security assessment.
- System hardening review.
- Network monitoring.
- Digital evidence collection.
- Security automation.

This lesson brings those activities together into one final investigation.

---

## 3. Understanding Evidence Correlation

Evidence correlation means comparing information from different sources to understand relationships between events.

### Example

Suppose you observe:

1. A failed SSH login in an authentication log.
2. An unusual connection in a network capture.
3. A corresponding timestamp in system logs.
4. An unexpected account configuration.

Each observation provides part of the picture.

When examined together, they may help an investigator understand what happened.

However, matching timestamps or similar observations do not automatically prove that events share the same cause.

### Evidence Sources in This Project

| Source | Information |
|---|---|
| Nmap results | Discovered hosts and services |
| Security assessment | Potential weaknesses |
| System logs | System and authentication events |
| PCAP files | Captured network traffic |
| File hashes | Evidence integrity |
| Automation reports | System configuration observations |
| Hardening checklist | Security improvement status |

---

## 4. Practical Lab A: Review Your Project Evidence

Create a final investigation workspace:

```bash
mkdir -p ~/practical-labs/module15/final-investigation
```

Review your discovery results:

```bash
cat ~/practical-labs/module15/discovery/results/target-assessment.nmap
```

Review the vulnerability register:

```bash
cat ~/practical-labs/module15/security-assessment/vulnerability-register.csv
```

Review the monitoring report:

```bash
cat ~/practical-labs/module15/monitoring/monitoring-report.md
```

Review the forensic evidence inventory:

```bash
cat ~/practical-labs/module15/forensics/evidence-inventory.md
```

Review the automation README:

```bash
cat ~/practical-labs/module15/automation/README.md
```

Record which files exist and identify any missing evidence.

Do not invent missing results.

---

## 5. Practical Lab B: Build an Investigation Timeline

An investigation timeline organizes events chronologically.

It helps establish the sequence of observed activities.

Create:

```bash
nano ~/practical-labs/module15/final-investigation/investigation-timeline.md
```

Add:

```markdown
# INVESTIGATION TIMELINE

| Date and Time | Event | Evidence Source | Observation |
|---|---|---|---|
| [Actual time] | Network discovery | Nmap results | Host identified |
| [Actual time] | Service assessment | Nmap results | Services examined |
| [Actual time] | Security review | Assessment report | Potential weaknesses documented |
| [Actual time] | Traffic capture | PCAP file | Network traffic recorded |
| [Actual time] | Log inspection | System logs | Events examined |
| [Actual time] | Evidence hashing | SHA-256 records | Integrity checked |
| [Actual time] | Automation | Security report | System information collected |

## Timeline Notes

- Record actual timestamps where available.
- Identify the source of each timestamp.
- Distinguish event time from evidence collection time.
- Do not invent dates or event sequences.
```

Save the file.

### Important Timestamp Differences

- Event time: When an activity actually occurred.
- Collection time: When evidence was collected.
- Modification time: When file contents were last modified.
- Access time: When file contents were last accessed, subject to filesystem behavior.

These timestamps may differ.

---

## 6. Practical Lab C: Correlate Network and System Evidence

Review your saved packet capture:

```bash
tcpdump -r ~/practical-labs/module15/monitoring/lab-capture.pcap
```

Review the system log:

```bash
less ~/practical-labs/module15/forensics/evidence/system-log.txt
```

Review the system information:

```bash
less ~/practical-labs/module15/forensics/evidence/system-information.txt
```

Look for observations that can be compared.

For example:

- Was an SSH connection visible in the capture?
- Did the system logs contain a related event?
- Do timestamps fall within a comparable period?
- Does the available evidence identify the same system?

If you cannot establish a relationship, document that limitation.

### Correlation Record

Create:

```bash
nano ~/practical-labs/module15/final-investigation/evidence-correlation.md
```

Add:

```markdown
# EVIDENCE CORRELATION REPORT

## Evidence Pair 1

Source A:
Network packet capture

Source B:
System logs

Observation:
[Describe what was observed.]

Relationship:
[Confirmed / Possible / Not established]

Limitations:
[Describe missing information.]

## Evidence Pair 2

Source A:
Nmap scan

Source B:
Vulnerability register

Observation:
[Describe the relationship.]

Relationship:
[Confirmed / Possible / Not established]

Limitations:
[Describe missing information.]

## Overall Conclusion

[Summarize what the available evidence supports.]
```

---

## 7. Practical Lab D: Review Security Findings

Review your vulnerability register:

```bash
nano ~/practical-labs/module15/security-assessment/vulnerability-register.csv
```

For every entry, determine:

1. Was the service actually observed?
2. Is the identified software version reliable?
3. Is there a relevant security advisory?
4. Does the advisory apply to the observed version?
5. Was the weakness confirmed?
6. What remediation is appropriate?

Use these finding statuses:

| Status | Meaning |
|---|---|
| Confirmed | Supported by sufficient evidence |
| Unconfirmed | Requires additional validation |
| Not applicable | Does not apply to the assessed system |
| Remediated | A correction was applied and verified |
| Accepted risk | Authorized decision to retain the risk |

Do not mark a finding as remediated merely because you wrote a remediation recommendation.

---

## 8. Practical Lab E: Review Remediation Activities

Open:

```bash
nano ~/practical-labs/module15/hardening/remediation-plan.md
```

Review each recommendation.

Record:

- Original condition.
- Recommended correction.
- Whether a change was actually performed.
- Validation evidence.
- Remaining risk.

Create:

```bash
nano ~/practical-labs/module15/final-investigation/remediation-review.md
```

Add:

```markdown
# REMEDIATION REVIEW

## Finding 1
Description:
[Enter finding.]

Recommended Action:
[Enter correction.]

Implementation Status:
[Planned / Implemented / Not applicable]

Validation:
[Describe evidence.]

Remaining Risk:
[Describe remaining concern.]

## Finding 2
Description:
[Enter finding.]

Recommended Action:
[Enter correction.]

Implementation Status:
[Planned / Implemented / Not applicable]

Validation:
[Describe evidence.]

Remaining Risk:
[Describe remaining concern.]

## Overall Assessment

[Summarize remediation progress.]
```

---

## 9. Practical Lab F: Verify Evidence Integrity

Recheck the forensic sample:

```bash
cd ~/practical-labs/module15/forensics/evidence
sha256sum -c sample-evidence.sha256
```

Verify the network capture hash if you created one:

```bash
cd ~/practical-labs/module15/monitoring
sha256sum -c capture-hash.txt
```

Record the verification results.

If a verification fails, do not overwrite the original hash record to hide the discrepancy. Investigate and document it.

---

## 10. Practical Lab G: Create a Final Investigation Summary

Create:

```bash
nano ~/practical-labs/module15/final-investigation/investigation-summary.md
```

Add:

```markdown
# FINAL INVESTIGATION SUMMARY

## Project
Isolated Cybersecurity Assessment, Defense
and Digital Forensics Lab

## Investigation Scope
Authorized Kali Linux and Metasploitable 2
virtual laboratory.

## 1. Objectives

- Identify systems and network services.
- Assess potential security weaknesses.
- Review hardening opportunities.
- Monitor network and system activity.
- Preserve digital evidence.
- Automate selected security checks.

## 2. Activities Performed

[Summarize completed activities.]

## 3. Main Findings

[List actual evidence-supported findings.]

## 4. Evidence Collected

[List collected evidence files.]

## 5. Evidence Integrity

[Summarize hash verification.]

## 6. Remediation Review

[Summarize completed and pending actions.]

## 7. Limitations

[Describe scope and technical limitations.]

## 8. Recommendations

[Provide practical recommendations.]

## 9. Conclusion

[Write the final investigation conclusion.]
```

Complete the report using your actual work.

---

## 11. Practical Lab H: Prepare the Final Project Directory

Inspect your complete project structure:

```bash
find ~/practical-labs/module15 -maxdepth 3 -type f
```

Check the size of the project:

```bash
du -sh ~/practical-labs/module15
```

List the investigation files:

```bash
find ~/practical-labs/module15/final-investigation -type f
```

Your final investigation directory should contain:

```text
final-investigation/
├── investigation-timeline.md
├── evidence-correlation.md
├── remediation-review.md
└── investigation-summary.md
```

---

## 12. Professional Reporting Standards

A good cybersecurity report should be:

### Accurate
Every technical claim must be supported by evidence.

### Clear
Use understandable language and explain technical terms.

### Objective
Separate facts, assumptions, and recommendations.

### Reproducible
Document the tools, commands, and methods used.

### Actionable
Give recommendations that can realistically be implemented.

### Confidential
Protect sensitive system information and evidence.

---

## 13. Knowledge Check

1. What is a cybersecurity investigation?
2. What is evidence correlation?
3. Why is an investigation timeline important?
4. Explain event time versus collection time.
5. Why should findings be validated?
6. What is the difference between confirmed and unconfirmed findings?
7. How do hashes support evidence integrity?
8. Why should remediation be verified?
9. What makes a professional security report reliable?
10. Why must limitations be included in an investigation report?

---

## 14. Practical Completion Checklist

- [ ] I reviewed my Nmap results.
- [ ] I reviewed my vulnerability register.
- [ ] I reviewed my monitoring evidence.
- [ ] I reviewed my forensic evidence.
- [ ] I created an investigation timeline.
- [ ] I created an evidence correlation report.
- [ ] I reviewed remediation activities.
- [ ] I rechecked evidence hashes.
- [ ] I created an investigation summary.
- [ ] I inspected the final project directory.

---

## 15. Key Takeaway

A successful cybersecurity investigation combines technical observations, reliable evidence, careful analysis, and clear reporting.

**Remember: A professional investigator does not merely collect information. They explain what the evidence establishes, what remains uncertain, and what should happen next.**
