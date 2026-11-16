
# MODULE 12 – WEB SECURITY

# LESSON 02: HTTP AND HTTPS PROTOCOLS

## 1. Introduction

HTTP and HTTPS are communication protocols used by web browsers and web servers.

Whenever you visit a website, your browser communicates with a web server using HTTP or HTTPS.

Examples:

- Opening a university portal.
- Logging into an online banking platform.
- Downloading a document.
- Submitting an online form.
- Accessing an API.

Understanding these protocols is essential for web security because many vulnerabilities involve how requests and responses are transmitted and processed.

## 2. What Is HTTP?

HTTP stands for Hypertext Transfer Protocol.

It is an application-layer protocol used for communication between clients and servers.

A client is usually a web browser.

A server is a computer or application that provides web resources.

Example:

```text
CLIENT                  SERVER
  |                       |
  |------ HTTP Request -->|
  |                       |
  |<----- HTTP Response --|
  |                       |
```

HTTP follows a request-response communication model.

The client sends a request, and the server returns a response.

## 3. What Is HTTPS?

HTTPS stands for Hypertext Transfer Protocol Secure.

HTTPS is HTTP communication protected by Transport Layer Security (TLS).

TLS provides:

1. Encryption.
2. Data integrity.
3. Authentication of the server through certificates.

HTTPS helps protect information transmitted between a browser and a server.

Example:

When you log into an online banking application, HTTPS helps prevent someone monitoring the network from reading your password in transit.

Important: HTTPS does not guarantee that a website is legitimate, trustworthy, or free from vulnerabilities.

## 4. HTTP vs HTTPS

| Feature | HTTP | HTTPS |
|---|---|---|
| Full name | Hypertext Transfer Protocol | HTTP Secure |
| Encryption | No TLS encryption | TLS encryption |
| Default port | 80 | 443 |
| Integrity protection | No TLS protection | Provided by TLS |
| Server authentication | No TLS certificate authentication | Uses TLS certificates |
| Security | Unsuitable for sensitive data in transit | Recommended for modern websites |

## 5. Understanding HTTP Ports

A port identifies a communication endpoint on a computer.

Common web-related ports:

| Port | Protocol or service |
|---|---|
| 80 | HTTP |
| 443 | HTTPS |
| 8080 | Alternative HTTP service |
| 8443 | Alternative HTTPS service |

These are common conventions, not absolute requirements. Services can be configured to use other ports.

Inspect listening TCP ports:

```bash
ss -tln
```

Inspect listening TCP and UDP ports:

```bash
ss -tuln
```

## 6. Understanding HTTP Requests

An HTTP request is a message sent by a client to a server.

A request commonly contains:

- Request method.
- Request target.
- HTTP version.
- Headers.
- Optional message body.

Example:

```http
GET /index.html HTTP/1.1
Host: 127.0.0.1:8000
User-Agent: Mozilla/5.0
Accept: text/html
```

Explanation:

**GET** – Requests a resource.

**/index.html** – The requested resource.

**HTTP/1.1** – The HTTP version.

**Host** – Identifies the requested host.

**User-Agent** – Describes the client software.

**Accept** – Indicates acceptable response content types.

## 7. Common HTTP Methods

HTTP methods describe the intended action of a request.

### GET

Retrieves information from a server.

Example:

```http
GET /products HTTP/1.1
```

### POST

Submits information to a server.

Example:

```http
POST /login HTTP/1.1
```

### PUT

Creates or replaces a resource at a specified location, depending on application implementation.

### PATCH

Partially modifies a resource.

### DELETE

Requests deletion of a resource.

### HEAD

Requests response headers without the response body.

### OPTIONS

Requests information about communication options supported by a server or resource.

Security note:

A server must enforce authorization and validation for state-changing methods. Hiding a method in the user interface is not a security control.

## 8. Understanding HTTP Responses

An HTTP response is sent by the server after receiving a request.

Example:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 120
```

Explanation:

- `HTTP/1.1` – Protocol version.
- `200` – Status code.
- `OK` – Status description.
- `Content-Type` – Describes the response content.
- `Content-Length` – Indicates the message body length when provided.

## 9. HTTP Status Codes

HTTP status codes are grouped into five categories.

| Category | Meaning |
|---|---|
| 1xx | Informational |
| 2xx | Successful |
| 3xx | Redirection |
| 4xx | Client error |
| 5xx | Server error |

Important status codes:

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 301 | Moved Permanently |
| 302 | Found |
| 304 | Not Modified |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 429 | Too Many Requests |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |

Remember:

- 401 generally means authentication is required or failed.
- 403 means the server understood the request but refuses to authorize it.
- 404 means the requested resource was not found or is intentionally concealed.
- 500 indicates an internal server error.

## 10. HTTP Headers

HTTP headers provide additional information about requests and responses.

### Common Request Headers

| Header | Purpose |
|---|---|
| Host | Identifies the requested host |
| User-Agent | Identifies the client software |
| Accept | Indicates acceptable content types |
| Authorization | Carries authentication credentials or tokens |
| Cookie | Sends stored cookies |
| Content-Type | Describes request body format |

### Common Response Headers

| Header | Purpose |
|---|---|
| Content-Type | Describes response content |
| Content-Length | Indicates message body length |
| Server | May identify server software |
| Set-Cookie | Instructs the browser to store cookies |
| Location | Provides a redirection target |
| Cache-Control | Defines caching behavior |

## 11. Important Web Security Headers

Security headers help browsers apply additional protections.

### A. Strict-Transport-Security (HSTS)

Instructs browsers to use HTTPS for future requests to a domain for a specified period.

Example:

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

Only configure `includeSubDomains` when all relevant subdomains support HTTPS.

### B. Content-Security-Policy (CSP)

Restricts the sources from which certain content may be loaded or executed.

Example:

```http
Content-Security-Policy: default-src 'self'
```

CSP can help reduce the impact of certain Cross-Site Scripting attacks.

### C. X-Content-Type-Options

Helps prevent browsers from interpreting a resource as a different content type.

Example:

```http
X-Content-Type-Options: nosniff
```

### D. Referrer-Policy

Controls how much referrer information a browser sends.

Example:

```http
Referrer-Policy: strict-origin-when-cross-origin
```

### E. Permissions-Policy

Controls access to certain browser features.

Example:

```http
Permissions-Policy: camera=(), microphone=(), geolocation=()
```

The appropriate policy depends on the application's requirements.

## 12. Understanding Cookies

Cookies are small pieces of information stored by a browser for a website.

They are commonly used for:

- Login sessions.
- User preferences.
- Shopping carts.
- Tracking application state.

Example:

```http
Set-Cookie: sessionid=example; Secure; HttpOnly; SameSite=Lax
```

Important cookie attributes:

**Secure**

The browser sends the cookie over secure HTTPS connections.

**HttpOnly**

Prevents ordinary JavaScript access to the cookie.

**SameSite**

Controls when cookies are sent with cross-site requests.

Common values:

- Strict.
- Lax.
- None.

A session cookie should be configured according to the application's security requirements.

Never use real passwords, session tokens, or personal account cookies in your training reports.

## 13. Understanding TLS

TLS stands for Transport Layer Security.

It protects communication between clients and servers.

TLS provides:

### Encryption

Helps prevent unauthorized parties from reading transmitted information.

### Integrity

Helps detect unauthorized modification of transmitted information.

### Authentication

Uses digital certificates to authenticate the server's identity.

TLS is the security foundation commonly used by HTTPS.

## 14. TLS Handshake Overview

Before protected application data is exchanged, the client and server establish a TLS connection.

A simplified TLS 1.3 handshake:

1. Client sends ClientHello.
2. Server responds with ServerHello.
3. The server provides authentication information and handshake data.
4. The client verifies the server certificate and establishes the necessary shared secrets.
5. Both parties confirm the handshake.
6. Encrypted application data can be exchanged.

The actual TLS 1.3 handshake is more sophisticated than this simplified description.

## 15. Digital Certificates

A digital certificate helps establish the identity of a website.

It commonly contains:

- Domain name.
- Public key.
- Issuer information.
- Validity period.
- Digital signature.

Certificate authorities (CAs) issue or sign certificates according to their validation procedures.

Browsers check certificate information when establishing HTTPS connections.

A certificate warning may indicate:

- An expired certificate.
- A hostname mismatch.
- An untrusted issuer.
- An incomplete certificate chain.
- Other certificate validation problems.

Never ignore certificate warnings when accessing sensitive services.

## 16. Practical Lab 1 – Inspect HTTP Headers

Start your local server:

```bash
cd ~/practical-labs/module12/lab01
```

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Open another terminal.

Send a HEAD request:

```bash
curl -I http://127.0.0.1:8000
```

Observe:

- HTTP status code.
- Server header.
- Content type.
- Other response headers.

Save the results:

```bash
mkdir -p ~/practical-labs/module12/lab02
```

```bash
curl -I http://127.0.0.1:8000 > ~/practical-labs/module12/lab02/http-headers.txt
```

Stop the server when finished.

## 17. Practical Lab 2 – Inspect HTTP Methods

Start the local server again:

```bash
cd ~/practical-labs/module12/lab01
```

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

From another terminal, inspect the GET response:

```bash
curl -v http://127.0.0.1:8000/
```

Inspect the HEAD response:

```bash
curl -I http://127.0.0.1:8000/
```

Try OPTIONS:

```bash
curl -i -X OPTIONS http://127.0.0.1:8000/
```

Observe the response.

Important: The Python development server may not implement every HTTP method or return a detailed Allow header. Document what actually happens rather than assuming a particular result.

Stop the server.

## 18. Practical Lab 3 – Inspect HTTP Status Codes

Start the local server:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

From another terminal, request an existing resource:

```bash
curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:8000/
```

Request a resource that does not exist:

```bash
curl -s -o /dev/null -w "%{http_code}\n" http://127.0.0.1:8000/not-found
```

Compare the results.

Expected:

- Existing resource: normally 200.
- Missing resource: normally 404.

Stop the server.

## 19. Practical Lab 4 – Inspect HTTPS Certificates

Use a public website only for normal, passive certificate inspection.

Example:

```bash
curl -Iv https://example.com
```

Inspect the certificate using OpenSSL:

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null
```

Observe:

- Certificate subject.
- Issuer.
- Validity information.
- TLS protocol information.
- Certificate verification result.

These commands establish a normal connection and inspect certificate information; they do not perform vulnerability exploitation.

If the connection fails because of DNS, network restrictions, or certificate configuration, document the error.

## 20. Practical Lab 5 – Inspect a Certificate in Detail

Run:

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -subject -issuer -dates
```

This extracts:

- Subject.
- Issuer.
- Valid-from date.
- Expiry date.

Record your findings in:

```bash
nano ~/practical-labs/module12/lab02/certificate-observations.md
```

Include:

```markdown
# HTTPS Certificate Observation

## Domain
example.com

## Certificate Subject

## Certificate Issuer

## Valid From

## Valid Until

## TLS Connection Result

## Security Observations
```

## 21. Practical Lab 6 – Understand HTTP vs HTTPS Using Wireshark

If Wireshark is installed, capture traffic generated by your own local lab.

Start the HTTP server:

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Open Wireshark.

Select the loopback interface if available.

Use this display filter:

```text
tcp.port == 8000
```

Generate a request:

```bash
curl http://127.0.0.1:8000/
```

Inspect the captured packets.

Look for:

- TCP communication.
- HTTP request.
- HTTP response.
- Readable HTTP headers.

Stop the server and capture.

For HTTPS, capture a normal connection to a public website if permitted by your network.

Compare:

- HTTP application data may be readable in the packet capture.
- HTTPS application data is protected by TLS encryption.

TLS handshake metadata may still be visible, depending on the protocol version and configuration.

Do not capture other people's private traffic.

## 22. Knowledge Check

Answer these questions in your notebook.

1. What does HTTP stand for?
2. What does HTTPS stand for?
3. What is the default HTTP port?
4. What is the default HTTPS port?
5. Explain the HTTP request-response model.
6. What is the difference between GET and POST?
7. What does HTTP status code 200 mean?
8. What does HTTP status code 403 mean?
9. What is the purpose of an HTTP header?
10. What is the purpose of the Set-Cookie header?
11. Explain the Secure cookie attribute.
12. What is TLS?
13. What is a digital certificate?
14. What is HSTS?
15. Why is HTTPS important for web security?

## 23. Lesson Summary

In this lesson, you learned:

- HTTP is an application-layer communication protocol.
- HTTPS protects HTTP communication using TLS.
- HTTP requests and responses form the basis of web communication.
- HTTP methods describe intended operations.
- Status codes describe response outcomes.
- Headers provide additional request and response information.
- Cookies help websites maintain application state.
- TLS provides encryption, integrity, and server authentication.
- Digital certificates help authenticate website identities.
- Security headers provide additional browser protections.
- Wireshark can help analyze HTTP and TLS traffic.

## 24. Lesson Completion Checklist

- [ ] Understand HTTP and HTTPS.
- [ ] Understand common web ports.
- [ ] Understand HTTP request structure.
- [ ] Understand HTTP response structure.
- [ ] Identify common HTTP methods.
- [ ] Understand status codes.
- [ ] Understand request and response headers.
- [ ] Understand cookies and security attributes.
- [ ] Understand TLS and digital certificates.
- [ ] Inspect HTTP headers.
- [ ] Inspect HTTP status codes.
- [ ] Inspect an HTTPS certificate.
- [ ] Compare HTTP and HTTPS traffic.

**END OF LESSON 02**
