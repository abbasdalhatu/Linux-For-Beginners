
# MODULE 12 – WEB SECURITY

# LESSON 10: MODULE 12 PRACTICAL ASSESSMENT

## 1. Assessment Overview

This is the final practical assessment for Module 12 – Web Security.

You will demonstrate your ability to:

1. Explain web security fundamentals.
2. Inspect HTTP and HTTPS communication.
3. Understand web application architecture.
4. Perform basic web server enumeration.
5. Recognize OWASP Top 10 risks.
6. Evaluate authentication and session security.
7. Identify input validation and injection risks.
8. Use common web security testing tools.
9. Assess vulnerabilities and write professional reports.

### Assessment Rules

- Work only within your own local lab.
- Use your Kali Linux VM.
- Do not scan public websites or networks.
- Do not test systems without explicit authorization.
- Do not perform destructive testing.
- Record your observations and findings.

**Suggested duration:** 2–3 hours.

**Total marks:** 100.

---

# PART A – THEORY (20 MARKS)

Answer the following questions.

Each question carries 2 marks.

1. Define web application security.
2. Explain the difference between HTTP and HTTPS.
3. What is the purpose of an HTTP response header?
4. Explain the difference between authentication and authorization.
5. What is session hijacking?
6. Define SQL injection.
7. What is Cross-Site Scripting (XSS)?
8. What is the purpose of the OWASP Top 10?
9. Explain the difference between vulnerability scanning and penetration testing.
10. What is a false positive in vulnerability assessment?

---

# PART B – WEB ENVIRONMENT SETUP (15 MARKS)

## Task 1: Create Your Assessment Workspace

Create:

```bash
mkdir -p ~/practical-labs/module12/final-assessment
cd ~/practical-labs/module12/final-assessment
```

Create these files:

```bash
touch scope.md
touch reconnaissance.md
touch http-analysis.md
touch findings.md
touch final-report.md
```

Verify:

```bash
ls -l
```

**Required evidence:**

Record the five files in your assessment notes.

## Task 2: Start a Local Web Server

Create a test webpage:

```bash
mkdir -p website
```

Create the HTML file:

```bash
nano website/index.html
```

Add:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Web Security Assessment Lab</title>
</head>
<body>
    <h1>Local Web Security Lab</h1>
    <p>This application is for authorized testing.</p>
</body>
</html>
```

Start the server:

```bash
cd website
python3 -m http.server 8000 --bind 127.0.0.1
```

Keep it running.

Open another terminal and test:

```bash
curl http://127.0.0.1:8000
```

**Required evidence:**

- Screenshot or copied response.
- Server address and port.
- Confirmation that the webpage is accessible.

---

# PART C – HTTP ANALYSIS (15 MARKS)

## Task 3: Inspect HTTP Headers

Run:

```bash
curl -I http://127.0.0.1:8000
```

Record:

1. HTTP status code.
2. Server header.
3. Content-Type.
4. Date header.
5. Other observed headers.

## Task 4: Inspect HTTP Methods

Run:

```bash
curl -i -X OPTIONS http://127.0.0.1:8000/
```

Record the response.

Remember: A response to OPTIONS is not automatically proof that every listed method is safely implemented or enabled for every resource.

## Task 5: Inspect the Webpage

Use your browser to open:

```text
http://127.0.0.1:8000
```

Open Developer Tools.

Inspect:

- Elements.
- Network.
- Console.
- Page response.

Record your observations in:

```text
http-analysis.md
```

---

# PART D – WEB SERVER ENUMERATION (10 MARKS)

## Task 6: Perform Local Port Inspection

With your local server running, execute:

```bash
nmap -sT -sV -p 8000 127.0.0.1
```

This checks the specific port on your own loopback interface.

Record:

- Target address.
- Port.
- Port state.
- Service information.
- Version information, if detected.

Save your results in:

```text
reconnaissance.md
```

Do not scan any other address for this assessment.

---

# PART E – WEB SECURITY ANALYSIS (15 MARKS)

## Task 7: Analyze the OWASP Top 10

Choose five OWASP web application risk categories and explain:

1. What each risk means.
2. How it can affect an application.
3. One example.
4. One prevention method.

Include at least these topics:

- Broken Access Control.
- Security Misconfiguration.
- Injection.
- Cryptographic Failures.
- Authentication Failures.

## Task 8: Review Secure Coding Practices

Explain how developers can prevent:

- SQL injection.
- Cross-Site Scripting.
- Command injection.
- Insecure session management.
- Broken access control.

Use examples where possible.

Do not attempt to exploit a real application.

---

# PART F – VULNERABILITY ASSESSMENT (15 MARKS)

## Task 9: Identify and Document Observations

Review your local HTTP server and previous test results.

Document at least three observations.

Examples may include:

- A missing security header.
- Server information disclosure.
- Lack of HTTPS in the local HTTP-only test environment.

For each observation, include:

```text
Finding ID:
Finding Title:
Affected Component:
Description:
Evidence:
Potential Impact:
Severity:
Recommendation:
Status:
```

Important:

An observation is not automatically a confirmed vulnerability. Clearly distinguish confirmed weaknesses, configuration observations, and items requiring further investigation.

## Task 10: Prepare a Remediation Plan

For each finding, recommend a suitable corrective action.

Consider:

- Secure HTTP response headers.
- HTTPS deployment.
- Secure authentication.
- Safe session handling.
- Input validation.
- Parameterized database queries.
- Software updates.

---

# PART G – PROFESSIONAL REPORT (10 MARKS)

## Task 11: Complete Your Final Report

Open:

```bash
nano final-report.md
```

Use this structure:

```markdown
# WEB APPLICATION SECURITY ASSESSMENT

## 1. Executive Summary

Summarize the assessment and its purpose.

## 2. Scope

Target:
Environment:
Date:
Authorized activities:

## 3. Methodology

Describe the tools and procedures used.

## 4. Tools Used

List the tools and their purposes.

## 5. Technical Observations

Document the results of HTTP inspection,
browser inspection and local enumeration.

## 6. Vulnerability Findings

Include the finding IDs, descriptions,
evidence, severity and recommendations.

## 7. Risk Summary

Summarize the security risks identified.

## 8. Remediation Plan

Explain how the identified issues
should be addressed.

## 9. Limitations

Explain what was not tested.

## 10. Conclusion

Summarize the assessment.
```

Make sure your report is accurate and does not claim tests you did not perform.

---

# PART H – FINAL KNOWLEDGE CHECK (BONUS)

Try answering these without looking at your notes.

1. Why is HTTPS important?
2. What happens during an HTTP request and response?
3. Why should web applications validate user input?
4. What is the purpose of secure session cookies?
5. Why are parameterized SQL queries safer?
6. What is the difference between a vulnerability and an exploit?
7. Why should penetration testing have written authorization?
8. What is the purpose of Burp Suite?
9. What is the purpose of OWASP ZAP?
10. Why must security findings include evidence?

---

# 3. MARKING SCHEME

| Assessment Section | Marks |
|---|---:|
| Part A – Theory | 20 |
| Part B – Environment Setup | 15 |
| Part C – HTTP Analysis | 15 |
| Part D – Enumeration | 10 |
| Part E – Security Analysis | 15 |
| Part F – Vulnerability Assessment | 15 |
| Part G – Professional Report | 10 |
| **TOTAL** | **100** |

## Performance Guide

| Score | Performance |
|---|---|
| 90–100 | Excellent |
| 80–89 | Very Good |
| 70–79 | Good |
| 60–69 | Satisfactory |
| 50–59 | Pass |
| Below 50 | Requires further practice |

---

# 4. FINAL COMPLETION CHECKLIST

- [ ] Created the assessment workspace.
- [ ] Created all five assessment files.
- [ ] Started the local web server.
- [ ] Tested HTTP connectivity.
- [ ] Inspected HTTP headers.
- [ ] Inspected HTTP methods.
- [ ] Used browser Developer Tools.
- [ ] Performed local Nmap inspection.
- [ ] Reviewed OWASP risks.
- [ ] Documented security observations.
- [ ] Prepared remediation recommendations.
- [ ] Completed the final report.

---

# 5. MODULE 12 COMPLETION

Upon completing this assessment, you should be able to explain and perform the foundational stages of a web application security assessment within an authorized environment.

You have studied:

1. Web Security Fundamentals.
2. HTTP and HTTPS Protocols.
3. Web Application Architecture.
4. Web Server Enumeration.
5. OWASP Top 10.
6. Web Authentication and Session Security.
7. Web Input Validation and Injection.
8. Web Security Testing Tools.
9. Web Vulnerability Assessment and Reporting.
10. Final Practical Assessment.

**Congratulations on reaching the end of Module 12 – Web Security.**

# END OF MODULE 12
