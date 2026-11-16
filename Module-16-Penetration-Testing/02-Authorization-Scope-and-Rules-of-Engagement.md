# MODULE 16 – PENETRATION TESTING
## LESSON 02: AUTHORIZATION, SCOPE AND RULES OF ENGAGEMENT

## 1. Introduction

Before a penetration tester performs any security assessment, they must understand three important concepts:

1. Authorization
2. Scope
3. Rules of Engagement (RoE)

These concepts establish the legal permission, boundaries, and conditions under which security testing can take place.

A skilled penetration tester must understand these requirements before using any scanning or testing tool.

**Remember:** Technical ability does not give someone permission to test a system.

---

## 2. What Is Authorization?

Authorization is formal permission granted by the owner or an appropriately authorized representative of a system to conduct specified security testing.

For example:

A company hires a cybersecurity professional to assess its internal network.

The company provides written permission specifying:
- Which systems may be tested.
- Which testing activities are permitted.
- When testing may take place.
- Who is responsible for approving the assessment.

Without proper authorization, testing may violate laws, policies, or contractual obligations.

### Why Is Authorization Important?

Authorization:

1. Establishes legitimate permission.
2. Protects the tester and organization.
3. Prevents misunderstandings.
4. Defines acceptable testing activities.
5. Establishes accountability.
6. Reduces the risk of unauthorized access or disruption.

### Examples of Authorization

| Situation | Authorized? |
|---|---|
| Testing your own isolated lab | Yes, within your ownership and configuration |
| Testing a company system with written permission | Yes, within the approved agreement |
| Testing a university portal without permission | No |
| Scanning a stranger's public IP address | No, unless explicitly authorized |
| Testing a deliberately vulnerable VM you own | Yes, within the lab boundaries |

---

## 3. What Is Scope?

Scope defines the systems, applications, networks, activities, and boundaries included in an assessment.

It answers the question:

**Exactly what am I allowed to test?**

A penetration testing engagement should have a clearly documented scope before technical work begins.

### Components of Scope

A scope document may include:

| Component | Description |
|---|---|
| Target systems | Computers, servers, or applications included |
| IP addresses | Approved network addresses |
| Domains | Approved website or domain names |
| Applications | Specific applications to assess |
| Testing period | Approved dates and times |
| Testing methods | Permitted assessment techniques |
| Exclusions | Systems and activities that must not be tested |
| Contacts | People responsible for the engagement |

### Example Scope

Imagine a company authorizes a test of its training environment.

**Included:**
- Training server A.
- Training web application.
- Isolated test database.

**Excluded:**
- Production database.
- Employee personal devices.
- Third-party services.
- External customer systems.

Even if an excluded system is accessible from an included system, it remains outside the approved scope.

---

## 4. What Are Rules of Engagement (RoE)?

Rules of Engagement are documented instructions that explain how an authorized penetration test must be performed.

They establish the operational boundaries of the assessment.

### Common Rules of Engagement

1. Testing must take place only during approved hours.
2. Only approved systems may be assessed.
3. Destructive testing is prohibited unless specifically authorized.
4. Sensitive information must be protected.
5. Testers must maintain accurate activity records.
6. Unexpected disruption must be reported immediately.
7. Testing must stop when instructed by the designated contact.
8. Evidence must be handled securely.
9. Testing methods must follow the approved agreement.
10. The final report must be delivered to the designated recipients.

### Example

A company authorizes a web application assessment but prohibits denial-of-service testing.

This means the tester may assess permitted application security weaknesses but must not perform activities intended to overwhelm the service.

---

## 5. Authorization vs Scope vs Rules of Engagement

These three concepts work together, but they have different purposes.

| Concept | Main Question | Purpose |
|---|---|---|
| Authorization | Do I have permission? | Establishes permission |
| Scope | What can I test? | Defines boundaries |
| Rules of Engagement | How must I test? | Defines testing conditions |

### Simple Analogy

Imagine you are invited to inspect a university computer laboratory.

- Authorization: The university gives you permission to inspect it.
- Scope: You may inspect only computers in Laboratory A.
- Rules of Engagement: You may inspect them between 10 AM and 2 PM, but you must not modify student files.

This is how professional security assessments are controlled.

---

## 6. What Is a Statement of Work (SOW)?

A Statement of Work is a document describing the services, deliverables, responsibilities, and expectations of a security engagement.

It may include:

- Project objectives.
- Assessment scope.
- Activities to be performed.
- Project timeline.
- Responsibilities.
- Deliverables.
- Fees and payment terms.
- Acceptance criteria.

A Statement of Work helps the client and testing team understand what has been agreed.

## 7. What Is a Get-Out-of-Scope Condition?

An out-of-scope condition occurs when a testing activity or target falls outside the approved boundaries.

Examples:

- Discovering an unrelated external server.
- Encountering a third-party system.
- Finding an employee device that was not included.
- Identifying a production database excluded from the engagement.

### What Should the Tester Do?

1. Stop the activity affecting the out-of-scope target.
2. Avoid accessing or modifying its data.
3. Record the relevant observation without collecting unnecessary information.
4. Notify the authorized engagement contact.
5. Wait for written clarification or updated authorization before proceeding.

Do not assume that discovering a system automatically authorizes testing it.

---

## 8. What Is a Stop Condition?

A stop condition is a situation in which testing must immediately pause or terminate.

Examples:

- A system becomes unstable.
- A service unexpectedly goes offline.
- Sensitive information is exposed.
- The client requests that testing stop.
- Testing reaches an excluded system.
- An unexpected security incident occurs.

A professional tester prioritizes safety over completing a test.

---

## 9. Evidence Handling and Confidentiality

During an assessment, testers may encounter sensitive information.

Examples include:
- Usernames.
- Email addresses.
- System configuration details.
- Database records.
- Authentication information.
- Internal network documentation.

Evidence must be handled responsibly.

### Evidence Handling Principles

1. Collect only what is necessary.
2. Avoid copying entire databases.
3. Redact personal or sensitive information in reports.
4. Store evidence in an approved secure location.
5. Restrict access to authorized personnel.
6. Follow the agreed retention and deletion policy.
7. Do not publish findings without permission.

### Example

If a security weakness exposes a database record, the tester should capture only the minimum evidence required to demonstrate the issue, using approved methods.

---

## 10. Professional Documentation

A penetration tester should document authorization and assessment boundaries.

A basic engagement record may contain:

| Field | Example |
|---|---|
| Engagement name | Internal Training Lab Assessment |
| Client | Example Organization |
| Authorized representative | Security Manager |
| Testing team | Approved Security Team |
| Start date | Agreed project date |
| End date | Agreed project date |
| Approved targets | Listed test systems |
| Excluded targets | Production systems |
| Permitted activities | Approved assessment methods |
| Stop conditions | Service disruption or client instruction |
| Deliverable | Security assessment report |

This is an example of documentation, not a substitute for a signed agreement.

---

## 11. Practical Exercise – Create Your Rules of Engagement

In this exercise, you will create a Rules of Engagement document for your own isolated VMware learning lab.

### Step 1: Open your lesson directory

```bash
mkdir -p ~/module16-lab/lesson02
cd ~/module16-lab/lesson02
```

### Step 2: Create the document

```bash
nano rules-of-engagement.txt
```

Enter:

```text
PENETRATION TESTING RULES OF ENGAGEMENT

PROJECT:
Local VMware Penetration Testing Lab

PURPOSE:
Educational security testing and practical learning.

AUTHORIZED ENVIRONMENT:
Kali Linux virtual machine.
Metasploitable virtual machine.

AUTHORIZATION:
The lab owner authorizes testing of the designated
virtual machines within the isolated environment.

SCOPE:
Only the explicitly designated lab virtual machines.

OUT OF SCOPE:
Public websites.
University production systems.
Unrelated local devices.
Third-party networks.
External servers.

PERMITTED ACTIVITIES:
Lab planning.
System inspection.
Controlled discovery.
Service identification.
Vulnerability assessment.
Safe validation.
Evidence documentation.

PROHIBITED ACTIVITIES:
Unauthorized external scanning.
Destructive testing.
Denial-of-service activities.
Data theft or exfiltration.
Stealth and evasion techniques.
Testing unrelated devices.

STOP CONDITIONS:
Unexpected service disruption.
Exposure of unrelated sensitive information.
Loss of lab isolation.
Any instruction to stop testing.

EVIDENCE HANDLING:
Collect minimum necessary evidence.
Avoid storing sensitive information.
Keep assessment notes within the lab workspace.

FINAL DELIVERABLE:
A documented educational penetration testing report.
```

Save using:
- `CTRL + O`
- `ENTER`
- `CTRL + X`

### Step 3: Verify the document

```bash
cat rules-of-engagement.txt
```

### Step 4: Check your lab workspace

```bash
pwd
ls -l
```

Your document should be located inside:

`~/module16-lab/lesson02/`

---

## 12. Knowledge Check

**Question 1:** What does authorization establish?

A. The attacker's identity  
B. Permission to perform specified security testing  
C. The operating system version  
D. The network speed  

**Question 2:** What defines the systems included in a penetration test?

A. Scope  
B. Payload  
C. Exploit  
D. Firewall  

**Question 3:** What do Rules of Engagement describe?

A. How testing must be performed  
B. How to steal credentials  
C. How to disable antivirus  
D. How to delete evidence  

**Question 4:** What should a tester do after discovering an out-of-scope system?

A. Test it immediately  
B. Ignore the agreement  
C. Stop and notify the authorized contact  
D. Attempt to access its database  

**Question 5:** Which of the following is an example of a stop condition?

A. Completing a report  
B. Unexpected service disruption  
C. Creating a test directory  
D. Reading documentation  

---

## 13. Practical Thinking Questions

Answer these in your notebook:

1. Why should penetration testers obtain written authorization?
2. Explain the difference between scope and Rules of Engagement.
3. What is a Statement of Work?
4. Give three examples of out-of-scope systems.
5. Explain why evidence handling is important.
6. What should a tester do when an unexpected system disruption occurs?
7. Why is it dangerous to assume that every reachable device is an authorized target?

## 14. Lesson Summary

In this lesson, you learned:

- What authorization means.
- How penetration testing scope is defined.
- The purpose of Rules of Engagement.
- The role of a Statement of Work.
- How to recognize out-of-scope systems.
- When to stop a penetration test.
- How to protect evidence and confidential information.
- How to document a basic testing agreement.

### Key Principle

**Authorization gives permission. Scope establishes boundaries. Rules of Engagement control how testing is performed.**

All three are essential before a professional penetration test begins.

**End of Lesson 02 – Authorization, Scope and Rules of Engagement**