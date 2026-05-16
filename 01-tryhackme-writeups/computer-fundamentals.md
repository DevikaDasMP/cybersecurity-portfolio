# TryHackMe Writeup: Computer Fundamentals

**Platform:** TryHackMe — Pre-Security Path

**Rooms Covered:** Inside a Computer System | Computer Types | Client Server Basics | Virtualisation Basics | Cloud Computing Fundamentals

**Status:** ✅ Completed

---

## Overview
The Computer Fundamentals section covers how computers are built from the inside out, the different types of computers that exist in the world, how devices communicate with each other over the internet, and how modern IT infrastructure uses virtualisation and cloud computing to deliver scalable and flexible services. This knowledge is foundational for understanding how cyberattacks target systems and how defenders protect them.

---

## Room 1: Inside a Computer System

### Core Components
Every computer system is made up of key hardware components that work together:

| Component | Function | Security Relevance |
|-----------|----------|-------------------|
| CPU | Processes all instructions | Spectre/Meltdown exploit CPU design |
| RAM | Temporary storage for running programs | Malware runs in RAM — memory forensics detects it |
| Storage (HDD/SSD) | Permanent data storage | Where logs, files, and malware persist |
| Motherboard | Connects all components | BIOS/UEFI firmware targeted by rootkits |
| GPU | Processes graphics | Used for password cracking |
| NIC | Connects device to network | MAC address spoofing targets the NIC |

### The Boot Process
The boot process is the sequence a computer follows when it powers on:

1. Power supplied to the motherboard
2. **BIOS/UEFI** runs POST (Power-On Self-Test) — checks all hardware
3. Bootloader located on the storage drive
4. Operating system kernel loaded into RAM
5. OS initialises services and processes
6. Login screen presented to the user

**Security relevance:** The boot process is sometimes targeted by hackers. **Bootkits** are malware that load before the operating system — making them extremely difficult to detect and remove. Understanding the boot sequence helps defenders identify firmware-level tampering.

---

## Room 2: Computer Types

### The Eight Types of Computers
Computers come in many forms — from devices you can see to silent chips embedded in everyday objects:

| Type | Description | Example |
|------|-------------|---------|
| Personal Computer | General purpose desktop or laptop | Home and office use |
| Server | Provides services to other computers | Web servers, file servers |
| Mainframe | Large systems for massive data processing | Banking and government |
| Supercomputer | Extremely fast, complex calculations | Weather forecasting |
| Embedded Computer | Built into other devices, often invisible | Traffic lights, medical devices |
| Microcontroller | Tiny chip controlling a single function | Coffee machines, door locks |
| Smartphone | Portable personal computer | iOS and Android devices |
| IoT Device | Internet-connected smart devices | Smart home devices, sensors |

### Key Insight
As the room concluded: *"The most critical computers are not always the fastest or flashiest. Sometimes, they are the silent chips that keep doors opening, planes flying, and coffee machines brewing."*

**Security relevance:** IoT and embedded devices are increasingly targeted by attackers because they often have weak security controls and limited update capabilities. Understanding the diversity of computer types helps security professionals assess the full attack surface of an organisation.

---

## Room 3: Client Server Basics

### The Client-Server Model
The client-server model describes how devices on the internet offer and consume services:

- **Client** — the device that initiates communication and requests a service
- **Server** — the device that receives the request and responds

This is similar to ordering a pizza:
- You (the client) place an order (send a request)
- The pizza shop (the server) delivers the pizza (sends a response)

### HTTP — A Practical Example
HTTP (HyperText Transfer Protocol) is the protocol used for websites and follows the client-server model:

**Client HTTP Request:**
```
GET /index.html HTTP/1.1
Host: example.com
```

**Server HTTP Response:**
```
HTTP/1.1 200 OK
Content-Type: text/html
<html>Welcome!</html>
```

### Key Concepts
- **Protocol** — rules that define how devices communicate
- **Request** — the client asking for something
- **Response** — the server replying with the requested resource
- **Port** — numbered entry point apps use to communicate (HTTP = port 80, HTTPS = port 443)

**Security relevance:** Understanding the client-server model is fundamental to web application security. Attacks like SQL injection, XSS, and man-in-the-middle attacks all exploit weaknesses in how clients and servers communicate.

---

## Room 4: Virtualisation Basics

### What is Virtualisation?
Virtualisation enables a single physical computer to act like multiple separate computers simultaneously. A special software layer called a **hypervisor** manages this process.

### Key Terminology

| Term | Definition |
|------|-----------|
| Virtualisation | Technology enabling one physical machine to run multiple virtual machines |
| Hypervisor | The manager software that creates and runs virtual machines |
| Virtual Machine (VM) | A complete virtual computer running inside a physical one |
| Container | A lightweight isolated environment for running a single application |
| Container Image | A pre-packaged template used to create containers consistently |
| Network Port | A numbered entry point applications use to communicate over a network |

### Key Benefits of Virtualisation
- Cost savings — run multiple systems on one physical machine
- Better resource usage — maximise hardware efficiency
- Safe testing for cybersecurity — isolate malware analysis in a VM
- Faster deployment — spin up new environments quickly
- Flexibility and portability — move VMs between machines
- Scalability — easily add more virtual resources
- Centralised management — manage multiple systems from one place

**Security relevance:** Virtualisation is essential in cybersecurity for creating **isolated lab environments** to safely analyse malware, test tools, and practice attack and defence techniques without risking production systems. Snapshots allow analysts to restore a clean state after testing.

---

## Room 5: Cloud Computing Fundamentals

### What is Cloud Computing?
Cloud computing provides flexible, scalable, and cost-effective access to computing resources over the internet — without needing to own or manage physical hardware.

### Cloud Deployment Models

| Model | Description |
|-------|-------------|
| Public Cloud | Shared cloud services accessed over the internet |
| Private Cloud | Cloud infrastructure built exclusively for one organisation |
| Hybrid Cloud | A combination of public and private clouds that share data |

### Cloud Service Models

| Model | What You Get | Example |
|-------|-------------|---------|
| IaaS (Infrastructure as a Service) | Rent basic computer parts like servers and storage | Amazon EC2 |
| PaaS (Platform as a Service) | Ready-to-use environment to build and run apps | Google App Engine |
| SaaS (Software as a Service) | Software you use online without installing anything | Gmail, Zoom |

### Key Cloud Services
- **EC2** — Amazon's cloud computers that you can quickly create, use, and resize whenever you need them

### Key Benefits of Cloud Computing
- Scalability — instantly scale resources up or down
- On-demand self-service — provision resources without human intervention
- Pay only for what you use — no upfront hardware costs
- Security — major providers invest heavily in security controls
- High availability — designed to minimise downtime
- Global access — access resources from anywhere in the world

**Security relevance:** Cloud security is one of the fastest-growing areas of cybersecurity. Key concerns include misconfigured cloud storage, identity and access management (IAM), and the shared responsibility model. SOC analysts increasingly monitor cloud environments using tools like Microsoft Sentinel and AWS Security Hub.

---

## Interview Questions

**Q: What is virtualisation and why is it important in cybersecurity?**
Virtualisation allows a single physical machine to run multiple virtual machines simultaneously using a hypervisor. In cybersecurity it is critical for creating isolated lab environments where analysts can safely analyse malware, test tools, and practice attack and defence techniques without risking production systems.

**Q: What is the difference between IaaS, PaaS and SaaS?**
IaaS provides raw infrastructure like servers and storage — you manage everything above that. PaaS provides a development environment — you just manage your applications. SaaS provides ready-to-use software — you manage nothing. Each model has a different shared responsibility for security.

**Q: What is the client-server model?**
The client-server model describes how devices communicate on a network — the client initiates requests and the server responds. Almost all internet communication follows this model. It is fundamental to understanding web application attacks where malicious clients send crafted requests to exploit server-side vulnerabilities.

**Q: Why are IoT devices a security concern?**
IoT and embedded devices are increasingly targeted because they often have weak or default security configurations, limited ability to receive security updates, and are connected to critical infrastructure. A compromised IoT device can provide an attacker with a foothold into a broader network.

---

## Reflection
The Computer Fundamentals section has given me a comprehensive understanding of how computers work from the inside out — from individual hardware components through to modern cloud infrastructure. This knowledge is directly applicable to cybersecurity work: understanding how systems boot helps me recognise firmware attacks, understanding virtualisation helps me work safely in lab environments, and understanding cloud deployment models helps me assess the security posture of modern organisations. I will build on this foundation as I progress through the Operating Systems section and into the TryHackMe SOC Level 1 path.

