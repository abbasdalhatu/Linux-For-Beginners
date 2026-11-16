
# MODULE 10 – NETWORK ENUMERATION

# LESSON 08: WEB SERVICE ENUMERATION

## 1. Introduction

Web service enumeration is the process of identifying and examining web services running on a system.

Web services commonly use:

- HTTP
- HTTPS
- Web servers
- Application servers
- APIs
- Web applications

During an authorized security assessment, web enumeration helps identify:

- Running web services.
- HTTP response headers.
- Server information.
- Supported HTTP methods.
- Status codes.
- TLS configuration.
- Application technologies.
- Potential configuration weaknesses.

This lesson uses a local web server running on your Kali VM.

## 2. What Is HTTP?

HTTP stands for Hypertext Transfer Protocol.

It is an application-layer protocol used for communication between clients and web servers.

Example:

```text
Web Browser
    |
    | HTTP Request
    v
Web Server
    |
    | HTTP Response
    v
Web Browser
```

A browser sends a request, and the server returns a response.

## 3. What Is HTTPS?

HTTPS stands for Hypertext Transfer Protocol Secure.

It uses TLS to protect HTTP communication.

HTTPS provides:

- Encryption.
- Server authentication.
- Communication integrity.

Default ports:

| Protocol | Port |
|---|---:|
| HTTP | 80 |
| HTTPS | 443 |

Other ports may be used by development servers and custom applications.

## 4. Common Web Server Software

| Software | Description |
|---|---|
| Apache HTTP Server | Popular web server |
| Nginx | Web server and reverse proxy |
| Microsoft IIS | Windows web server |
| Caddy | Modern web server |
| LiteSpeed | Web server software |

A server banner may reveal software information, but it is not always accurate or complete.

## 5. HTTP Request Methods

HTTP methods describe the action a client wants a server to perform.

| Method | Purpose |
|---|---|
| GET | Retrieve a resource |
| POST | Submit data |
| PUT | Create or replace a resource |
| PATCH | Partially modify a resource |
| DELETE | Delete a resource |
| HEAD | Retrieve headers without the response body |
| OPTIONS | Ask which communication options are supported |

Not every web application supports every method.

## 6. HTTP Status Codes

HTTP responses contain status codes.

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 301 | Moved Permanently |
| 302 | Found / Temporary Redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

Understanding status codes helps identify how a web application responds to requests.

## 7. Practical Lab 1: Create Your Workspace

Run:

```bash
mkdir -p ~/practical-labs/module10/lab08
cd ~/practical-labs/module10/lab08
```

Check whether the required tools are available:

```bash
curl --version
```

```bash
python3 --version
```

```bash
nmap --version
```

## 8. Practical Lab 2: Create a Local Web Server

We will create a small web server that is accessible only through your Kali VM's loopback interface.

Create an HTML file:

```bash
nano index.html
```

Enter:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Local Security Lab</title>
</head>
<body>
    <h1>Web Enumeration Lab</h1>
    <p>This is my local cybersecurity practice server.</p>
</body>
</html>
```

Save and exit.

Start the server:

```bash
python3 -m http.server 8080 --bind 127.0.0.1
```

Keep this terminal open.

Your local web server is now available at:

```text
http://127.0.0.1:8080
```

The server is bound to loopback, so it is not exposed to your network.

## 9. Practical Lab 3: Inspect the Web Server

Open another Kali terminal.

Send an HTTP request:

```bash
curl http://127.0.0.1:8080
```

You should see the HTML content.

Inspect response headers:

```bash
curl -I http://127.0.0.1:8080
```

The `-I` option requests the response headers without downloading the normal response body.

Inspect a more detailed request:

```bash
curl -v http://127.0.0.1:8080
```

Look for:

- Connection information.
- HTTP request headers.
- HTTP response status.
- Response headers.
- Server information.

Remember: verbose output may contain information that should not be shared publicly.

## 10. Practical Lab 4: Examine HTTP Headers

HTTP headers provide information about requests and responses.

Common response headers include:

| Header | Purpose |
|---|---|
| Server | Identifies server software when disclosed |
| Content-Type | Identifies response content type |
| Content-Length | Indicates response size |
| Date | Indicates response date |
| Cache-Control | Controls caching behavior |
| Set-Cookie | Sets a browser cookie |
| Location | Provides a redirect destination |

Run:

```bash
curl -I http://127.0.0.1:8080
```

Record the headers you observe.

Not every server returns every header.

## 11. Practical Lab 5: Inspect HTTP Status Codes

Run:

```bash
curl -o /dev/null -s -w "%{http_code}\n" http://127.0.0.1:8080
```

This prints the HTTP response status code.

Expected result:

```text
200
```

Now request a resource that does not exist:

```bash
curl -o /dev/null -s -w "%{http_code}\n" http://127.0.0.1:8080/not-found
```

Observe the result.

The Python development server normally returns a 404 response for an unavailable resource.

## 12. Practical Lab 6: Inspect HTTP Methods

Use an OPTIONS request:

```bash
curl -i -X OPTIONS http://127.0.0.1:8080/
```

Inspect the response.

The response may include an `Allow` header, but behavior depends on the server.

Important:

A method appearing in an Allow header does not automatically mean that the method is safe to use against every resource.

In this lesson, do not send modifying requests such as PUT, PATCH, POST, or DELETE unless a specific local test application has been created for that purpose.

## 13. Practical Lab 7: Nmap Web Service Detection

With your local server still running, open another terminal.

Scan only your local loopback address:

```bash
nmap -sT -sV -p 8080 127.0.0.1
```

Observe:

- Port state.
- Service identification.
- Version information, if available.

Use Nmap's HTTP title script:

```bash
nmap -p 8080 --script http-title 127.0.0.1
```

This script requests the page title from your local server.

Do not run enumeration scripts against websites or servers without explicit authorization.

## 14. Practical Lab 8: Inspect robots.txt

Websites may provide a file called `robots.txt`.

It gives instructions to cooperating web crawlers about which paths they should avoid or access.

Create a local file:

```bash
nano robots.txt
```

Enter:

```text
User-agent: *
Disallow: /private/
Allow: /
```

Save the file.

Request it:

```bash
curl http://127.0.0.1:8080/robots.txt
```

Observe the output.

Important:

`robots.txt` is not an access-control mechanism. It does not protect confidential files or prevent unauthorized access.

## 15. Practical Lab 9: Understand HTTPS and TLS

HTTPS uses TLS to protect communication.

Important TLS concepts include:

- Certificates.
- Certificate authorities.
- Public and private keys.
- Encryption.
- Certificate validation.
- TLS versions.

You can inspect a TLS connection against an HTTPS service you own or are explicitly authorized to test using:

```bash
openssl s_client -connect HOSTNAME:443 -servername HOSTNAME
```

Replace `HOSTNAME` with an authorized server.

If you do not have a local HTTPS server, study the command without executing it against an external system.

A certificate inspection does not prove that an application is secure. Application-level security requires separate testing.

## 16. Practical Lab 10: Stop the Local Web Server

Return to the terminal running your Python HTTP server.

Press:

```text
CTRL + C
```

Verify that the server has stopped:

```bash
curl --max-time 3 http://127.0.0.1:8080
```

The connection should fail because the local server is no longer running.

This is expected.

## 17. Web Service Security Risks

Common web security problems include:

### A. Information Disclosure

A server may reveal unnecessary software or configuration information.

### B. Insecure HTTP

Unencrypted HTTP may expose sensitive communication to interception.

### C. Weak Authentication

Poor authentication can allow unauthorized access.

### D. Missing Security Headers

Missing or incorrectly configured security headers can increase certain web application risks.

### E. Outdated Software

Old web server and application versions may contain known vulnerabilities.

### F. Misconfigured Access Controls

Incorrect permissions may expose restricted resources.

### Defensive Recommendations

- Use HTTPS.
- Keep web software updated.
- Configure secure authentication.
- Apply access controls.
- Avoid unnecessary server banners.
- Review security headers.
- Monitor web server logs.
- Back up important web application data.

## 18. Practical Lab 11: Create a Web Enumeration Report

Create your report:

```bash
nano web-enumeration-report.txt
```

Use this template:

```text
========================================
WEB SERVICE ENUMERATION REPORT
========================================

1. ASSESSMENT INFORMATION

Date:
Target:
Authorized Scope:

2. WEB SERVICE INFORMATION

IP Address:
Port:
Protocol:
Service:

3. HTTP RESPONSE

Status Code:
Content Type:
Server Header:
Content Length:

4. HTTP METHODS

Observed Methods:
Allow Header:
Observations:

5. WEB CONTENT

Page Title:
robots.txt:
Other Observations:

6. NMAP RESULTS

Port State:
Service Detection:
HTTP Title:

7. TLS INFORMATION

TLS Version:
Certificate:
Observations:

8. SECURITY OBSERVATIONS

Information Disclosure:
Configuration Issues:
Potential Risks:

9. RECOMMENDATIONS

Recommendation 1:
Recommendation 2:
Recommendation 3:

10. CONCLUSION

Summary:

========================================
END OF REPORT
========================================
```

Use only the information you actually collected.

## 19. Knowledge Check

1. What does HTTP stand for?
2. What does HTTPS stand for?
3. What is the default HTTP port?
4. What is the default HTTPS port?
5. What is the purpose of an HTTP GET request?
6. What does HTTP status code 200 mean?
7. What does HTTP status code 404 mean?
8. What is the purpose of the Server header?
9. What is the purpose of `robots.txt`?
10. Why is `robots.txt` not an access-control mechanism?
11. What does the `curl -I` command do?
12. What does Nmap's `http-title` script identify?
13. What is TLS?
14. Why should web services use HTTPS?

## 20. Lesson Completion Checklist

- [ ] Understand HTTP and HTTPS.
- [ ] Understand web server software.
- [ ] Understand HTTP request methods.
- [ ] Understand HTTP status codes.
- [ ] Create a local web server.
- [ ] Inspect HTTP responses using curl.
- [ ] Examine response headers.
- [ ] Identify HTTP status codes.
- [ ] Inspect HTTP methods.
- [ ] Perform local Nmap web service detection.
- [ ] Inspect robots.txt.
- [ ] Understand TLS and certificates.
- [ ] Stop the local web server.
- [ ] Create a web enumeration report.

## 21. Lesson Summary

Web service enumeration helps security professionals understand how web servers and applications communicate.

Remember:

- HTTP is used for web communication.
- HTTPS protects HTTP communication using TLS.
- Port 80 is the default HTTP port.
- Port 443 is the default HTTPS port.
- `curl` helps inspect web responses.
- Nmap helps identify web services.
- HTTP headers provide useful information.
- Status codes describe server responses.
- Web enumeration must remain within authorized scope.

**NEXT LESSON: ENUMERATION ANALYSIS AND REPORTING.**
