# TryHackMe Writeup: Network Fundamentals

**Platform:** TryHackMe — Pre-Security Path
**Status:** ✅ Completed
**Difficulty:** Easy
**Date Completed:** April 2026

---

## Overview

The Network Fundamentals module covers the core concepts of how networks work, how devices communicate, and the protocols that underpin the internet.
This is foundational knowledge for any cybersecurity professional, particularly for SOC Analyst roles where network traffic analysis is a daily task.

---

## What I Learned

### 1. What is Networking?
- A network is a collection of devices connected together to share data and resources
- Networks can be private (LAN — Local Area Network) or public (WAN — Wide Area Network)
- The internet is essentially a massive WAN connecting millions of devices globally
- Devices on a network are identified by their **IP address** and **MAC address**

### 2. IP Addresses
- An IP (Internet Protocol) address is a unique numerical label assigned to every device on a network
- **IPv4** format: 192.168.1.1 — 4 sets of numbers separated by dots (0–255)
- **IPv4** is running out of available addresses due to the massive growth of internet-connected devices
- **IPv6** was created to solve this — it uses a longer alphanumeric format (e.g. 2001:0db8:85a3:0000:0000:8a2e:0370:7334)
- **Public IP** — visible on the internet, assigned by your ISP
- **Private IP** — used within a local network (e.g. 192.168.x.x)

### 3. MAC Addresses
- A MAC (Media Access Control) address is a unique hardware identifier assigned to a network interface
- Format: 12 hexadecimal characters (e.g. a4:c3:f0:85:ac:2d)
- Unlike IP addresses, MAC addresses are permanent and tied to the physical device
- MAC addresses operate at Layer 2 (Data Link Layer) of the OSI model

### 4. The OSI Model
The OSI (Open Systems Interconnection) model is a framework that describes how data is transmitted across a network in 7 layers:

| Layer | Name | Function | Example |
|-------|------|----------|---------|
| 7 | Application | Interface for end-user applications | HTTP, FTP, DNS |
| 6 | Presentation | Data formatting, encryption | SSL/TLS |
| 5 | Session | Managing sessions between devices | NetBIOS |
| 4 | Transport | Reliable data transfer, error checking | TCP, UDP |
| 3 | Network | Logical addressing and routing | IP |
| 2 | Data Link | Physical addressing (MAC) | Ethernet |
| 1 | Physical | Physical transmission of data | Cables, Wi-Fi |

**Security relevance:** Understanding which layer an attack operates on helps analysts identify the right mitigation. 
For example, a DDoS attack operates at Layer 3/4, while phishing attacks target Layer 7.

### 5. TCP/IP Model
- A simplified 4-layer version of OSI used in practice
- Layers: Application → Transport → Internet → Network Access
- **TCP (Transmission Control Protocol)** — reliable, connection-based, uses 3-way handshake (SYN, SYN-ACK, ACK)
- **UDP (User Datagram Protocol)** — faster but unreliable, no handshake, used for streaming and DNS

### 6. Packets and Frames
- Data is broken into smaller units called **packets** for transmission
- Each packet contains a **header** (source/destination IP, protocol) and a **payload** (the actual data)
- Frames operate at Layer 2 and contain MAC address information
- **Security relevance:** SOC analysts analyse packets using tools like Wireshark to detect malicious traffic

### 7. Key Protocols
| Protocol | Port | Purpose |
|----------|------|---------|
| HTTP | 80 | Web traffic (unencrypted) |
| HTTPS | 443 | Secure web traffic (encrypted via TLS) |
| FTP | 21 | File transfer |
| SSH | 22 | Secure remote access |
| DNS | 53 | Domain name resolution |
| RDP | 3389 | Remote Desktop Protocol |
| SMTP | 25 | Email sending |
| SMB | 445 | File sharing (Windows) |

**Security relevance:** Knowing common ports helps analysts spot suspicious traffic.
For example, traffic on port 4444 often indicates a Metasploit reverse shell.

### 8. DNS (Domain Name System)
- DNS acts as the phonebook of the internet
- Converts human-readable domain names (google.com) into IP addresses (142.250.187.46)
- **DNS Resolution Process:**
  1. User types google.com in browser
  2. Device checks local cache first
  3. If not cached, query sent to DNS resolver (usually ISP)
  4. Resolver queries root nameserver → TLD nameserver → authoritative nameserver
  5. IP address returned to browser
- **Security relevance:** DNS can be exploited — DNS spoofing, DNS hijacking, and DNS tunnelling are common attack techniques

### 9. DHCP
- DHCP (Dynamic Host Configuration Protocol) automatically assigns IP addresses to devices on a network
- Without DHCP, every device would need a manually configured IP address
- **Security relevance:** Rogue DHCP servers can be used in man-in-the-middle attacks

### 10. Firewalls
- A firewall is a security device that monitors and controls incoming and outgoing network traffic
- Rules are set to allow or block traffic based on IP addresses, ports, and protocols
- **Types:** Hardware firewall, Software firewall, Cloud firewall
- **Stateful** firewalls track the state of connections; **Stateless** firewalls only check packet headers

---

## Key Security Concepts from This Module

- **Ping sweep** — scanning a range of IP addresses to find active hosts
- **Port scanning** — identifying open ports on a target (used by both attackers and defenders)
- **Man-in-the-Middle (MitM)** — attacker intercepts communication between two parties
- **Packet sniffing** — capturing network traffic to analyse or steal data (tools: Wireshark, tcpdump)

---

## Tools Introduced
- **Ping** — tests connectivity between two devices using ICMP
- **Traceroute/Tracert** — shows the path packets take across a network
- **Nmap** — network scanner for discovering hosts and open ports

---

## Interview Questions I Can Now Answer

**Q: What is the difference between TCP and UDP?**
TCP is a connection-oriented protocol that uses a 3-way handshake to establish a reliable connection before data transfer. 
UDP is connectionless and faster but does not guarantee delivery — used for DNS queries and video streaming.

**Q: What is the OSI model and why does it matter in security?**
The OSI model is a 7-layer framework describing how data moves across a network.
It matters in security because different attacks operate at different layers — understanding the model helps analysts identify where an attack occurred and 
which controls to apply.

**Q: What is DNS and how can it be abused?**
DNS translates domain names into IP addresses. Attackers can abuse DNS through DNS spoofing (redirecting users to malicious sites), 
DNS hijacking (taking over DNS records), or DNS tunnelling (exfiltrating data through DNS queries).

---

## Reflection
Network fundamentals is essential knowledge for any SOC analyst. Understanding how traffic flows, how protocols work, and how devices communicate 
is the foundation for analysing network logs, detecting anomalies, and responding to threats. I will build on this knowledge through Wireshark practice
and the TryHackMe SOC Level 1 path.


