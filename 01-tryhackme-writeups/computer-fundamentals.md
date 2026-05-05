# TryHackMe Writeup: Computer Fundamentals

**Platform:** TryHackMe — Pre-Security Path
**Status:** ✅ Completed
**Difficulty:** Easy
**Date Completed:** April 2026

---

## Overview

The Computer Fundamentals module covers how computers work at a fundamental level — hardware components, operating systems, file systems, and basic concepts that underpin all of computing. For cybersecurity professionals, this knowledge is essential for understanding how attackers exploit systems and how defenders protect them.

---

## What I Learned

### 1. Core Hardware Components
| Component | Function | Security Relevance |
|-----------|----------|-------------------|
| CPU (Central Processing Unit) | Processes instructions | Spectre/Meltdown vulnerabilities exploit CPU design flaws |
| RAM (Random Access Memory) | Temporary storage for running processes | Malware often runs in RAM — memory forensics can detect it |
| Storage (HDD/SSD) | Permanent data storage | Where files, logs, and evidence are stored |
| Motherboard | Connects all components | BIOS/UEFI firmware can be targeted by rootkits |
| GPU | Processes graphics | Can be used for password cracking (parallel processing) |
| NIC (Network Interface Card) | Connects device to network | MAC address spoofing targets the NIC |

### 2. How a Computer Boots
1. Power button pressed — power supplied to motherboard
2. **BIOS/UEFI** runs POST (Power-On Self-Test) — checks hardware
3. Bootloader located (usually on storage drive)
4. Operating system kernel loaded into RAM
5. OS initialises services and processes
6. Login screen presented to user

**Security relevance:** Attackers can target the boot process with **bootkit** malware — malicious code that loads before the OS, making it very hard to detect and remove.

### 3. Operating Systems
- An OS manages hardware resources and provides services for applications
- **Main OS types:** Windows, Linux, macOS
- **Key OS functions:** Process management, memory management, file system management, security controls, user management

**Windows vs Linux in Security:**
| Feature | Windows | Linux |
|---------|---------|-------|
| Market share | Dominant in enterprise | Dominant in servers |
| Attack target | Most targeted by malware | Less targeted but not immune |
| CLI | PowerShell, CMD | Bash |
| File system | NTFS | ext4, XFS |
| Logs location | Event Viewer | /var/log/ |

### 4. File Systems
- A file system organises and stores data on a storage device
- **NTFS (Windows)** — supports permissions, encryption, journaling
- **FAT32** — older, limited file size, used on USB drives
- **ext4 (Linux)** — most common Linux file system
- **File paths:**
  - Windows: `C:\Users\Devika\Documents\file.txt`
  - Linux: `/home/devika/documents/file.txt`

**Security relevance:** Understanding file systems is critical for digital forensics — investigators examine file metadata, deleted files, and access timestamps to build evidence.

### 5. Processes and Services
- A **process** is a running instance of a program
- Every process has a **PID (Process ID)**
- **Services** are background processes that run without user interaction
- **Windows Task Manager** and **Linux ps/top commands** show running processes

**Security relevance:** Malware often disguises itself as a legitimate process. SOC analysts examine running processes to identify suspicious activity — for example, a process called `svchost.exe` running from an unusual location is a red flag.

### 6. Users and Permissions
- Operating systems use user accounts to control access to resources
- **Principle of Least Privilege** — users should only have the minimum permissions needed to do their job
- **Windows permission levels:** Standard User, Administrator
- **Linux permission levels:** Regular user, sudo (superuser), root

**File permissions in Linux:**
```
-rwxr-xr-x  1  devika  devika  4096  May 2026  file.sh
```
- `r` = read, `w` = write, `x` = execute
- First 3 characters = owner permissions
- Next 3 = group permissions
- Last 3 = other/world permissions

**Security relevance:** Misconfigured permissions are a common vulnerability. Attackers look for files with excessive permissions to escalate privileges.

### 7. Virtualisation
- Virtualisation allows multiple operating systems to run on a single physical machine
- A **hypervisor** manages virtual machines (VMs)
- **Type 1 hypervisor** (bare metal) — runs directly on hardware (VMware ESXi, Hyper-V)
- **Type 2 hypervisor** — runs on top of an OS (VirtualBox, VMware Workstation)

**Security uses of virtualisation:**
- Creating isolated lab environments for malware analysis
- Running TryHackMe attack/defence labs safely
- Sandboxing suspicious files to observe behaviour without risking the host machine
- Snapshot and rollback — restore a clean state after testing

### 8. Command Line Basics (Windows)
| Command | Function |
|---------|----------|
| `dir` | List files in directory |
| `cd` | Change directory |
| `ipconfig` | Show network configuration |
| `netstat` | Show network connections |
| `tasklist` | Show running processes |
| `systeminfo` | Show system information |
| `ping` | Test connectivity |
| `whoami` | Show current user |

### 9. Binary and Hexadecimal
- Computers process data in **binary** (0s and 1s)
- **1 bit** = single 0 or 1
- **1 byte** = 8 bits
- **Hexadecimal** is base-16, used as a human-readable representation of binary data
- Hex is used extensively in malware analysis, memory forensics, and network packet analysis

**Security relevance:** Understanding binary and hex is essential for reading packet captures, analysing malware signatures, and working with hashes.

### 10. Hashing
- A hash is a fixed-length output generated from input data using a hashing algorithm
- **Common algorithms:** MD5, SHA-1, SHA-256
- Hashes are one-way — you cannot reverse a hash to get the original data
- Even a tiny change in input produces a completely different hash (avalanche effect)

**Security uses of hashing:**
- Storing passwords securely (never store plain text)
- Verifying file integrity — if the hash changes, the file has been modified
- Identifying malware — known malware files have known hash values (threat intelligence feeds)

---

## Tools Introduced
- **Windows Task Manager** — viewing and managing running processes
- **VirtualBox** — free Type 2 hypervisor for creating virtual machines
- **Kali Linux** — security-focused Linux distribution used by penetration testers and SOC analysts

---

## Interview Questions I Can Now Answer

**Q: What is the principle of least privilege?**
The principle of least privilege means that users, processes, and systems should only have the minimum level of access required to perform their function. This limits the damage an attacker can do if they compromise an account — they cannot access resources beyond what that account is permitted to use.

**Q: What is virtualisation and how is it used in cybersecurity?**
Virtualisation allows multiple operating systems to run on a single physical machine using a hypervisor. In cybersecurity, it is used to create isolated lab environments for safely analysing malware, testing tools, and practising attack and defence techniques without risking the host system.

**Q: What is hashing and why is it important in security?**
Hashing is the process of converting data into a fixed-length output using a mathematical algorithm. It is one-way — you cannot reverse it to get the original data. In security, hashing is used to store passwords securely, verify file integrity, and identify known malware through hash matching in threat intelligence feeds.

**Q: Why would a SOC analyst examine running processes?**
Malware often disguises itself as a legitimate process or runs as an unexpected process. By examining running processes — using Task Manager on Windows or the ps command on Linux — a SOC analyst can identify suspicious processes, unusual parent-child process relationships, or processes running from unexpected locations, which can indicate compromise.

---

## Reflection
Computer fundamentals provide the essential building blocks for understanding how systems work and how they can be attacked or defended. Knowledge of processes, permissions, file systems, and the boot sequence is directly applicable to incident response, digital forensics, and threat hunting. I will build on this foundation through Linux and Windows specific TryHackMe rooms and hands-on lab practice.
