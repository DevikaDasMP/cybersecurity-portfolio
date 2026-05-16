# TryHackMe Writeup: How the Web Works

**Platform:** TryHackMe — Pre-Security Path

**Rooms Covered:** DNS in Detail | HTTP in Detail | How Websites Work | Putting It All Together

**Status:** ✅ Completed

---

## Overview
This section explains how the web works under the hood — from how domain names are resolved to how web servers respond to requests and how websites are built. For cybersecurity professionals, this is critical knowledge because the majority of modern attacks target web applications and web-based communication.

---

## Room 1: DNS in Detail

### What is DNS?
- DNS (Domain Name System) acts as the phonebook of the internet
- Translates human-readable domain names (google.com) into IP addresses (142.250.187.46)
- Without DNS, users would need to remember IP addresses for every website

### Domain Hierarchy
- **Root domain** — invisible top level (.)
- **TLD (Top Level Domain)** — .com, .co.uk, .org
- **Second Level Domain** — "google" in google.com
- **Subdomain** — "www" or "mail" in mail.google.com

### DNS Record Types
| Record | Purpose | Example |
|--------|---------|---------|
| A | Maps domain to IPv4 address | google.com → 142.250.x.x |
| AAAA | Maps domain to IPv6 address | google.com → 2607:f8b0:... |
| CNAME | Maps domain to another domain | www → shop.example.com |
| MX | Mail server records | Points to email server |
| TXT | Text records for verification | SPF, DKIM for email security |

### How a DNS Request Works
1. Browser checks local cache for the domain's IP
2. If not found — query sent to recursive DNS server (usually your ISP)
3. Recursive server checks its own cache
4. If not found — queries root nameserver → TLD nameserver → authoritative nameserver
5. IP address returned and cached for future requests

### Security Relevance
- **DNS spoofing** — attacker poisons DNS cache to redirect users to malicious sites
- **DNS hijacking** — taking over DNS records to redirect traffic
- **DNS tunnelling** — exfiltrating data hidden inside DNS queries to bypass firewalls
- SPF and DKIM TXT records protect against email spoofing

---

## Room 2: HTTP in Detail

### What is HTTP?
- HTTP (HyperText Transfer Protocol) is the foundation of data communication on the web
- Defines how requests are made from browsers to web servers and how responses are returned
- **HTTPS** = HTTP + SSL/TLS encryption — protects data in transit

### HTTP Request Structure
```
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: text/html
```

### HTTP Methods
| Method | Purpose | Security Note |
|--------|---------|---------------|
| GET | Retrieve data | Parameters visible in URL |
| POST | Send data to server | Data in request body |
| PUT | Update a resource | Can overwrite data if unprotected |
| DELETE | Remove a resource | Dangerous if misconfigured |

### HTTP Status Codes
| Code | Meaning | Security Note |
|------|---------|---------------|
| 200 | OK — Success | |
| 201 | Created | |
| 301 | Moved Permanently | Can be abused in phishing redirects |
| 302 | Found (Redirect) | Open redirects are a vulnerability |
| 401 | Unauthorised | Authentication required |
| 403 | Forbidden | Access denied |
| 404 | Not Found | Verbose errors can reveal file structure |
| 500 | Internal Server Error | May leak stack traces to attackers |

### Cookies
- Small pieces of data stored in the browser, set by the web server
- Used to maintain sessions, remember preferences, track users
- **Session cookies** — temporary, deleted when browser closes
- **Persistent cookies** — stored on disk with an expiry date

**Security flags on cookies:**
- **HttpOnly** — prevents JavaScript accessing the cookie (protects against XSS cookie theft)
- **Secure** — cookie only sent over HTTPS connections
- **SameSite** — protects against CSRF attacks

### Security Relevance
- Stolen session cookies allow attackers to hijack authenticated sessions without a password
- Unencrypted HTTP traffic can be intercepted in a man-in-the-middle attack
- Missing security flags on cookies are a common web application vulnerability

---

## Room 3: How Websites Work

### Frontend vs Backend
- **Frontend** — what users see in the browser (HTML, CSS, JavaScript)
- **Backend** — server-side logic that processes requests (Python, PHP, Node.js)
- **Database** — stores and retrieves data (MySQL, PostgreSQL, MongoDB)

### HTML Basics
- HTML (HyperText Markup Language) structures web page content
- Uses tags to define elements: `<h1>` headings, `<p>` paragraphs, `<a>` links, `<img>` images
- **Security relevance:** Attackers inject malicious HTML/JavaScript into pages via XSS attacks

### JavaScript
- Makes web pages interactive and dynamic
- Runs inside the user's browser
- **Security relevance:** Malicious JavaScript injected via XSS can steal cookies, redirect users, or perform actions on their behalf

### Sensitive Data Exposure
- Developers sometimes accidentally leave sensitive information in HTML source code
- Comments, API keys, credentials, hidden form fields
- **Security relevance:** Attackers view page source (Ctrl+U) to find exposed sensitive data — a common reconnaissance technique

### Common Web Vulnerabilities Introduced
- **HTML Injection** — injecting HTML code into a page to change its appearance or redirect users
- **Sensitive Data in Source** — credentials or API keys left in page comments
- These form the foundation of understanding OWASP Top 10 vulnerabilities

---

## Room 4: Putting It All Together

### How a Web Request Works — Complete Process
When you type `https://google.com` in your browser:

1. **DNS resolution** — browser checks cache, then queries DNS to find google.com's IP address
2. **TCP connection** — 3-way handshake (SYN → SYN-ACK → ACK) established with the server
3. **TLS handshake** — for HTTPS, encryption is negotiated between browser and server
4. **HTTP request** — browser sends GET request to the web server
5. **Server processing** — web server receives request, queries database if needed, generates response
6. **HTTP response** — server sends back HTML, CSS, JavaScript files
7. **Browser rendering** — browser interprets and displays the web page to the user

### Key Components Working Together
- **Load Balancers** — distribute traffic across multiple servers to prevent overload
- **CDN (Content Delivery Network)** — serves static content from servers closest to the user
- **Databases** — store dynamic content (user accounts, posts, products)
- **WAF (Web Application Firewall)** — filters malicious requests before they reach the server

### Security Relevance
- Understanding the full web request lifecycle helps analysts identify where in the chain an attack occurred
- A WAF sits between the internet and web server — analysts review WAF logs to detect attack patterns
- Load balancers and CDNs can be abused in DDoS attacks

---

## Key Tools Introduced
- **Browser DevTools** — inspect HTTP requests, responses, cookies, and page source
- **Burp Suite** — intercepts and modifies HTTP requests for web application testing
- **cURL** — command-line tool for making HTTP requests

---

## Interview Questions

**Q: What happens when you type google.com into a browser?**
The browser checks its DNS cache for google.com's IP. If not found, a DNS query resolves the domain. A TCP 3-way handshake is established with the server, followed by a TLS handshake for HTTPS encryption. The browser sends an HTTP GET request, the server responds with the web page content, and the browser renders it.

**Q: What is the difference between HTTP and HTTPS?**
HTTP is unencrypted — data can be intercepted in a man-in-the-middle attack. HTTPS uses SSL/TLS to encrypt the connection, protecting data in transit. Any site handling sensitive information should always use HTTPS.

**Q: What are cookies and why are they a security concern?**
Cookies are small data files stored by the browser to maintain sessions and preferences. They are a security concern because stolen session cookies allow an attacker to hijack an authenticated session without needing the user's password. The HttpOnly and Secure flags help protect cookies from theft.

**Q: What is sensitive data exposure in web applications?**
Sensitive data exposure occurs when a web application accidentally reveals confidential information such as API keys, credentials, or internal system details — for example in HTML source code comments or error messages. Attackers use this information to plan further attacks.

---

## Reflection
Understanding how the web works is fundamental to web application security — one of the most heavily attacked areas in cybersecurity today. HTTP, DNS, cookies, and the full web request lifecycle are concepts I will encounter daily as a SOC Analyst when analysing web traffic logs, investigating incidents involving web applications, and detecting malicious HTTP requests. I will build on this through Burp Suite practice and the OWASP Top 10 study.
