# TryHackMe Writeup: Network Fundamentals

**Platform:** TryHackMe — Pre-Security Path
**Rooms Covered:** What is Networking? | Intro to LAN | OSI Model | Packets and Frames | Extending Your Network
**Status:** ✅ Completed | **Date:** April 2026

---

## Overview
The Network Fundamentals section builds a complete understanding of how computer networks work — from basic connectivity to how data travels across networks and how networks are extended to the internet. Essential knowledge for any SOC Analyst role.

---

## Room 1: What is Networking?

### What is a Network?
- A network connects two or more devices to share data and resources
- The internet is the world's largest network — billions of interconnected devices
- Networks can be private (home/office LAN) or public (internet)

### IP Addresses
- Every device gets an IP (Internet Protocol) address for identification
- **IPv4:** Four sets of numbers e.g. 192.168.1.1
- **IPv6:** Longer alphanumeric format — created because IPv4 addresses are running out
- **Public IP** — visible on the internet | **Private IP** — used within a local network only

### MAC Addresses
- Unique hardware identifier permanently assigned to a network interface
- Format: 12 hexadecimal characters e.g. a4:c3:f0:85:ac:2d
- Used for communication within a local network (Layer 2)
- Can be spoofed by attackers to bypass network access controls

### Ping
- Tests connectivity between two devices using ICMP protocol
- Command: `ping <IP address or domain>`
- **Security relevance:** Attackers use ping sweeps during reconnaissance to discover active hosts

---

## Room 2: Intro to LAN

### Network Topologies
- **Star** — all devices connect to a central switch. Most common. Central device = single point of failure
- **Bus** — all devices share one cable. Simple but inefficient
- **Ring** — devices connected in a loop. Data travels in one direction

### Key Devices
- **Switch** — connects devices within a LAN using MAC addresses. Sends traffic only to the intended device
- **Router** — connects different networks together using IP addresses. Routes traffic between your LAN and the internet

### Subnetting
- Divides a large network into smaller segments
- Example: 192.168.1.0/24 — /24 means first 24 bits identify the network
- **Security benefit:** Limits attacker lateral movement if one segment is compromised

### ARP (Address Resolution Protocol)
- Maps IP addresses to MAC addresses within a local network
- Device broadcasts "who has this IP?" — matching device responds with its MAC
- **ARP spoofing attack:** Attacker sends fake ARP replies to intercept traffic (man-in-the-middle)

### DHCP
- Automatically assigns IP addresses to devices joining a network
- **Security risk:** Rogue DHCP servers can redirect traffic through an attacker's machine

---

## Room 3: OSI Model

The OSI model is a 7-layer framework describing how data travels across a network:

| Layer | Name | Function | Examples |
|-------|------|----------|---------|
| 7 | Application | End-user applications | HTTP, DNS, FTP |
| 6 | Presentation | Formatting, encryption | SSL/TLS |
| 5 | Session | Managing sessions | NetBIOS |
| 4 | Transport | Reliable delivery, flow control | TCP, UDP |
| 3 | Network | Logical addressing, routing | IP, ICMP |
| 2 | Data Link | MAC addressing, error detection | Ethernet, ARP |
| 1 | Physical | Raw binary transmission | Cables, Wi-Fi |

### TCP vs UDP
| | TCP | UDP |
|-|-----|-----|
| Type | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery | No guarantee |
| Speed | Slower | Faster |
| Uses | Web, email, file transfer | DNS, streaming, gaming |

### TCP Three-Way Handshake
- **SYN** → client requests connection
- **SYN-ACK** → server acknowledges
- **ACK** → client confirms — connection established
- **Security relevance:** SYN flood attacks send thousands of SYN requests without completing the handshake, overwhelming the server

### Security Relevance of OSI
- Layer 3: IP spoofing, DDoS
- Layer 4: SYN flood attacks
- Layer 7: SQL injection, XSS, phishing
- Understanding the layer an attack targets guides the right mitigation

---

## Room 4: Packets and Frames

### Packets
- Data is broken into smaller units called packets for transmission
- Each packet has a **header** (source/destination IP, protocol) and **payload** (the actual data)
- Packets may travel different routes and are reassembled at the destination

### Frames
- Frames operate at Layer 2 and contain MAC address information
- A packet is wrapped inside a frame for local network transmission
- When crossing a router, the frame is stripped and rebuilt with new MAC addresses

### Security Relevance
- SOC analysts use **Wireshark** to capture and analyse packets
- Suspicious traffic — unusual ports, unexpected protocols, large transfers — is detected at packet level
- Understanding packet structure is essential for reading network logs and identifying threats

---

## Room 5: Extending Your Network

### Port Forwarding
- Allows external devices to access services within a private network
- Configured on the router to direct specific port traffic to an internal device
- **Security risk:** Misconfigured port forwarding exposes internal services to the internet

### Firewalls
- Monitors and controls network traffic based on defined rules (IP, port, protocol)
- **Stateful** — tracks connection state, more intelligent
- **Stateless** — checks each packet in isolation
- First line of perimeter defence — SOC analysts regularly review firewall logs

### VPN (Virtual Private Network)
- Creates an encrypted tunnel between a device and a VPN server
- Used for secure remote access and protecting data on public Wi-Fi
- Common types: PPTP, IPSec, OpenVPN

---

## Key Tools
- **Ping** — connectivity testing
- **Traceroute** — traces packet path across a network
- **Wireshark** — packet capture and analysis (core SOC tool)
- **Nmap** — network scanning and host discovery

---

## Interview Questions

**Q: What is the OSI model and why does it matter in security?**
The OSI model is a 7-layer framework describing how data moves across a network. It matters because different attacks target different layers — a SYN flood targets Layer 4 while SQL injection targets Layer 7. Understanding this helps analysts identify where an attack occurred and apply the right controls.

**Q: What is the difference between TCP and UDP?**
TCP is connection-oriented and guarantees delivery using a 3-way handshake. UDP is faster but connectionless with no delivery guarantee. TCP is used for web browsing and email; UDP for DNS and video streaming.

**Q: What is ARP spoofing?**
ARP maps IP addresses to MAC addresses on a local network. In ARP spoofing, an attacker sends fake ARP replies to associate their MAC address with a legitimate IP, allowing them to intercept network traffic in a man-in-the-middle attack.

**Q: What is subnetting and why is it important?**
Subnetting divides a large network into smaller segments. Security-wise it limits lateral movement — if one subnet is compromised, attackers cannot easily access devices on other subnets.

---

## Reflection
Network fundamentals is the most critical foundational topic for a SOC Analyst. Every security alert travels across a network. Understanding how networks are structured, how data flows, and how devices communicate gives me the foundation to analyse network logs, detect anomalies, and understand attacker movement. I will build on this through Wireshark practice and the TryHackMe SOC Level 1 path.
