# TryHackMe Writeup: How the Web Works

**Platform:** TryHackMe — Pre-Security Path
**Status:** ✅ Completed
**Difficulty:** Easy
**Date Completed:** April 2026

---

## Overview

This module explains how the web works under the hood — from typing a URL in a browser to receiving a web page. For cybersecurity professionals, understanding the web is critical because most modern attacks target web applications, APIs, and web-based communication.

---

## What I Learned

### 1. DNS in Detail
- Every website has an IP address — DNS translates the human-readable domain to that IP
- **Domain hierarchy:**
  - Root domain: `.` (invisible, at the top)
  - TLD (Top Level Domain): `.com`, `.co.uk`, `.org`
  - Second Level Domain: `google` in `google.com`
  - Subdomain: `www` or `mail` in `mail.google.com`
- **DNS Record Types:**

| Record Type | Purpose | Example |
|-------------|---------|---------|
| A | Maps domain to IPv4 address | google.com → 142.250.187.46 |
| AAAA | Maps domain to IPv6 address | google.com → 2607:f8b0:... |
| CNAME | Maps domain to another domain | www → shop.example.com |
| MX | Mail server records | Points to email server |
| TXT | Text records, used for verification | SPF, DKIM records |

**Security relevance:** Misconfigured DNS records can be exploited. SPF and DKIM records protect against email spoofing.

### 2. HTTP and HTTPS
- **HTTP (HyperText Transfer Protocol)** — the foundation of data communication on the web
- **HTTPS** — HTTP with SSL/TLS encryption, protecting data in transit

**HTTP Request Structure:**
```
GET /page HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

**HTTP Response Structure:**
```
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<html>...</html>
```

### 3. HTTP Methods
| Method | Purpose | Security Note |
|--------|---------|---------------|
| GET | Retrieve data | Parameters visible in URL |
| POST | Send data to server | Data in request body |
| PUT | Update existing resource | Can overwrite data |
| DELETE | Remove a resource | Dangerous if unprotected |
| PATCH | Partially update resource | |

**Security relevance:** Insecure HTTP methods left enabled on a server can be exploited. PUT and DELETE should be restricted.

### 4. HTTP Status Codes
| Code | Meaning | Security Note |
|------|---------|---------------|
| 200 | OK — Success | |
| 201 | Created | |
| 301 | Moved Permanently | Can be abused in phishing |
| 302 | Found (Redirect) | Open redirects are a vulnerability |
| 400 | Bad Request | |
| 401 | Unauthorised | Authentication required |
| 403 | Forbidden | Authorisation denied |
| 404 | Not Found | Can reveal file structure if verbose |
| 500 | Internal Server Error | May reveal stack traces to attackers |

### 5. Cookies
- Cookies are small pieces of data stored in the browser, set by a web server
- Used to maintain sessions, remember preferences, and track users
- **Session cookies** — temporary, deleted when browser closes
- **Persistent cookies** — stored on disk with an expiry date

**Security flags on cookies:**
- **HttpOnly** — prevents JavaScript from accessing the cookie (protects against XSS)
- **Secure** — cookie only sent over HTTPS
- **SameSite** — protects against CSRF attacks

**Security relevance:** Stolen session cookies allow attackers to hijack authenticated sessions without needing a password — known as session hijacking.

### 6. How a Web Request Works — Full Process
When you type `https://google.com` in your browser:

1. Browser checks local DNS cache for google.com
2. If not cached — DNS resolver query sent to find IP address
3. TCP 3-way handshake established with the server (SYN → SYN-ACK → ACK)
4. TLS handshake performed (for HTTPS) — encrypts the connection
5. HTTP GET request sent to the server
6. Server processes request and sends HTTP response
7. Browser renders HTML, CSS, JavaScript into the web page
8. Connection closed or kept alive for further requests

### 7. Web Application Structure
- **Frontend** — what users see (HTML, CSS, JavaScript) — runs in browser
- **Backend** — server-side logic (Python, PHP, Node.js) — runs on server
- **Database** — stores data (MySQL, PostgreSQL, MongoDB)
- **API** — allows applications to communicate with each other

**Security relevance:** Vulnerabilities exist at every layer — XSS targets frontend, SQLi targets the database, broken authentication targets the backend.

### 8. Common Web Vulnerabilities (OWASP Top 10 Awareness)
| Vulnerability | Description | Example |
|--------------|-------------|---------|
| SQL Injection (SQLi) | Malicious SQL code injected into input fields | `' OR 1=1--` bypasses login |
| Cross-Site Scripting (XSS) | Malicious scripts injected into web pages | Stealing cookies via JavaScript |
| Broken Authentication | Weak login mechanisms | Brute force, credential stuffing |
| Insecure Direct Object Reference | Accessing unauthorised resources by changing parameters | Changing `user_id=123` to `user_id=124` |
| Security Misconfiguration | Default credentials, unnecessary features enabled | Admin panel exposed publicly |
| Sensitive Data Exposure | Unencrypted sensitive data | Passwords stored in plain text |

---

## Tools Introduced
- **Browser DevTools** — inspect HTTP requests, responses, cookies, and headers
- **Burp Suite** — web application security testing proxy (intercepts and modifies requests)
- **cURL** — command-line tool for making HTTP requests

---

## Interview Questions I Can Now Answer

**Q: What happens when you type google.com into a browser?**
The browser first checks its DNS cache for the IP address of google.com. If not cached, a DNS query is sent to resolve the domain. Once the IP is found, a TCP 3-way handshake is established with the server. For HTTPS, a TLS handshake follows to encrypt the connection. An HTTP GET request is sent, the server responds with the web page content, and the browser renders it for the user.

**Q: What is the difference between HTTP and HTTPS?**
HTTP is unencrypted, meaning data sent between the browser and server can be intercepted by attackers in a man-in-the-middle attack. HTTPS uses SSL/TLS to encrypt the connection, protecting data in transit. Any site handling sensitive data such as passwords or payments should always use HTTPS.

**Q: What is an XSS attack?**
Cross-Site Scripting (XSS) is a web vulnerability where an attacker injects malicious JavaScript into a web page viewed by other users. This can be used to steal session cookies, redirect users to phishing sites, or perform actions on behalf of the victim. It is prevented through input validation and output encoding.

**Q: What are cookies and why are they a security concern?**
Cookies are small data files stored by the browser, used to maintain sessions and user preferences. They are a security concern because stolen cookies can allow session hijacking — an attacker can use a valid session cookie to impersonate an authenticated user without needing their password. The HttpOnly and Secure flags help protect cookies from being stolen.

---

## Reflection
Understanding how the web works is fundamental to web application security — one of the most critical areas of cybersecurity today. The majority of modern attacks target web applications, APIs, and browser-based vulnerabilities. This module has given me a strong foundation in HTTP, DNS, cookies, and common web vulnerabilities that I will build on through further TryHackMe rooms and real-world practice.
