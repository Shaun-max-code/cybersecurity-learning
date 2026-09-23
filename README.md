# Cybersecurity Learning & Pentesting Roadmap

A structured record of my cybersecurity learning journey, combining hands-on penetration testing, web application security, network security, Linux, and my ECE background with a long-term focus on IoT and Embedded Security.

---

## 🎯 Current Focus

* Penetration Testing
* Web Application Security
* Network Security
* Linux Security
* API Security
* Vulnerability Assessment
* CTFs and Security Labs
* IoT & Embedded Security
* Reverse Engineering
* Defensive Security / SOC Fundamentals

---

# 1. Fundamentals

### Networking

* IP addressing
* TCP/IP
* TCP and UDP
* Ports and services
* DNS
* HTTP / HTTPS
* SSH
* FTP
* SMB
* Network enumeration
* Packet analysis

### Linux

* Linux filesystem
* Users and groups
* Permissions
* Processes
* Services
* Networking commands
* Bash
* File permissions
* Privilege concepts

---

# 2. Reconnaissance & Enumeration

## Reconnaissance

Reconnaissance is the process of gathering information about a target before security testing or exploitation.

### Passive Reconnaissance

Information is collected without directly interacting with the target.

Examples:

* Search engines
* Public DNS information
* WHOIS
* Publicly available documentation
* Public repositories

### Active Reconnaissance

The target is directly interacted with to discover information.

Examples:

* Port scanning
* Service enumeration
* Directory enumeration
* Technology identification

---

## Nmap

Nmap is used for network discovery and security auditing.

Topics practiced:

* Host discovery
* Port scanning
* Service detection
* Version detection
* Basic enumeration

Example:

```bash
nmap -sV <target>
```

### Learning objective

Understand not only how to run a scan, but how discovered services contribute to the target's attack surface.

---

# 3. Web Application Security

Hands-on practice has primarily been performed using Burp Suite and PortSwigger Web Security Academy.

## SQL Injection (SQLi)

SQL Injection occurs when attacker-controlled input is improperly incorporated into a SQL query, allowing the query's intended logic to be manipulated.

Potential impact:

* Authentication bypass
* Unauthorized database access
* Data extraction
* Data modification
* Potential further compromise depending on the environment

---

## Cross-Site Scripting (XSS)

XSS occurs when attacker-controlled content is executed in a victim's browser within the context of a vulnerable web application.

Types:

* Reflected XSS
* Stored XSS
* DOM-based XSS

---

## Authentication Vulnerabilities

Authentication determines whether a user is who they claim to be.

Security testing areas include:

* Weak authentication
* Authentication bypass
* Credential attacks
* Password attacks
* Session weaknesses

---

## Authorization & Access Control

Authorization determines what an authenticated user is allowed to access or perform.

A broken access-control vulnerability occurs when an application fails to properly enforce those permissions.

### IDOR / BOLA

Insecure Direct Object Reference (IDOR), commonly discussed as Broken Object Level Authorization (BOLA) in API security, occurs when an application exposes an object reference without properly verifying whether the requester is authorized to access it.

---

## Session Security

A session allows a web application to maintain a user's authenticated state.

Topics:

* Session identifiers
* Cookies
* Session management
* Session hijacking concepts
* Session replay
* Session expiration

---

## Path Traversal

Path traversal occurs when an application allows user-controlled input to access files outside the intended directory.

Concept:

```text
Application directory
        |
        +-- intended file
        |
        +-- ../
              |
              +-- unintended location
```

---

## File Inclusion

File inclusion vulnerabilities occur when an application improperly allows user-controlled input to determine which file is loaded.

Types:

* Local File Inclusion (LFI)
* Remote File Inclusion (RFI)

---

## Command Injection

Command injection occurs when attacker-controlled input reaches an operating-system command and allows unintended command execution.

Important distinction:

```text
SQL Injection       → Database query
Command Injection   → Operating-system command
```

---

## File Upload Vulnerabilities

Security issues can occur when an application improperly validates or handles uploaded files.

Testing areas include:

* File type validation
* Extension validation
* Content validation
* Filename handling
* Storage location
* Server-side processing
* Access controls

---

## WAF & WAF Bypass Concepts

A Web Application Firewall (WAF) monitors web requests and attempts to identify and block malicious traffic.

During SQL injection labs, I encountered WAF filtering and studied how different input representations can sometimes be interpreted differently by security controls and backend applications.

Key lesson:

> A filtering mechanism should not be confused with fixing the underlying vulnerability.

---

# 4. API Security

Current learning area.

Topics to study:

* REST APIs
* HTTP methods
* JSON
* Authentication
* Authorization
* JWT
* OAuth
* API keys
* Rate limiting
* BOLA / IDOR
* Mass assignment
* Input validation
* Excessive data exposure
* Business logic vulnerabilities

Goal:

> Understand how the same security principles applied to web applications translate into API security.

---

# 5. Linux Privilege Escalation

Privilege escalation occurs when an attacker obtains privileges beyond those originally authorized.

### Vertical Privilege Escalation

```text
Low-privileged user
        ↓
Administrator / root
```

### Areas to Study

* SUID / SGID
* sudo configuration
* Cron jobs
* File permissions
* Writable files
* PATH manipulation
* Linux capabilities
* Services
* Credentials in configuration files
* Containers
* Kernel vulnerabilities

---

# 6. Reverse Shells

A reverse shell occurs when a compromised machine initiates a connection back to another system and provides shell interaction.

Conceptually:

```text
Attacker
    ↑
    │
    │ connection
    │
Compromised Host
```

This is an important concept in understanding post-exploitation.

---

# 7. CTF & Practical Labs

## PortSwigger Web Security Academy

Focus areas:

* SQL Injection
* Authentication
* Access Control
* XSS
* Session-related vulnerabilities
* WAF behavior
* HTTP request manipulation
* Web application testing

---

## OverTheWire

Practiced Linux and command-line fundamentals through the Bandit challenges.

Focus:

* SSH
* Linux commands
* File discovery
* Permissions
* Encoding
* Basic security problem solving

---

## Mr. Robot CTF

Completed a full CTF-style attack chain involving:

```text
Reconnaissance
      ↓
Enumeration
      ↓
Web Application Discovery
      ↓
WordPress Enumeration
      ↓
Initial Access
      ↓
Shell
      ↓
Linux Enumeration
      ↓
Privilege Escalation
      ↓
Flags / Keys
```

The main objective was to understand the methodology behind the commands rather than simply completing the challenge.

---

# 8. Network Security

Current and upcoming areas:

* Nmap
* Service enumeration
* Wireshark
* Packet analysis
* TCP/IP
* DNS
* HTTP
* HTTPS/TLS
* ARP
* DHCP
* FTP
* SSH
* SMB
* MITM concepts
* Network sniffing
* Network segmentation

---

# 9. Common Attack Concepts Studied

The following attack categories have been studied at a conceptual or practical level:

### Web

* SQL Injection
* XSS
* Authentication bypass
* Broken access control
* IDOR / BOLA
* Session attacks
* Path traversal
* LFI / RFI
* Command injection
* File upload vulnerabilities
* API attacks
* WAF bypass concepts

### Network

* Port scanning
* Service enumeration
* Eavesdropping
* Network sniffing
* MITM concepts
* DoS
* DDoS

### Credentials & Social Engineering

* Brute-force attacks
* Credential attacks
* Credential stuffing
* Phishing
* Social engineering
* Stolen credentials
* Identity theft

### Malware & Threats

* Malware
* Trojans
* Viruses
* Worms
* Ransomware
* Botnets
* Drive-by downloads
* APTs

### System Security

* Privilege escalation
* Reverse shells
* Linux enumeration
* Misconfiguration-based attacks

---

# 10. IoT & Embedded Security

Long-term specialization area based on my Electronics & Communication Engineering background.

## Hardware Security

Topics to learn:

* UART
* JTAG
* SPI
* I²C
* GPIO
* Debug interfaces
* Logic analyzers
* Serial communication

## Firmware Security

Topics:

* Firmware extraction
* Firmware analysis
* Filesystem extraction
* Binwalk
* Strings
* ELF binaries
* Ghidra
* Reverse engineering
* Hardcoded credentials
* Insecure firmware updates

## IoT Protocol Security

Topics:

* MQTT
* CoAP
* BLE
* Wi-Fi
* Zigbee

## Embedded Security

Areas of interest:

* Secure Boot
* Firmware integrity
* Debug interface security
* Authentication
* Cryptographic implementation
* Secure storage
* Hardware trust
* Embedded Linux security

---

# 11. Reverse Engineering

Reverse engineering is the process of analyzing software, binaries, firmware, or hardware to understand how they operate without relying entirely on the original source code or design documentation.

Tools to learn:

* Ghidra
* GDB
* x64dbg
* objdump
* strings
* radare2

Learning path:

```text
Binary
   ↓
File identification
   ↓
Strings
   ↓
Imports / exports
   ↓
Disassembly
   ↓
Control-flow analysis
   ↓
Static analysis
   ↓
Dynamic analysis
   ↓
Debugging
```

---

# 12. Defensive Security / SOC

Offensive security is only one side of cybersecurity.

Planned defensive topics:

* Security logs
* Windows Event Logs
* Linux logs
* SIEM
* Indicators of Compromise (IOCs)
* Tactics, Techniques & Procedures (TTPs)
* Threat hunting
* Incident response
* Detection engineering
* MITRE ATT&CK

Goal:

> Understand how attacks are detected and investigated, not only how they are performed.

---

# 13. Vulnerability Assessment

For every vulnerability, the goal is to understand:

1. What is the vulnerability?
2. Why does it occur?
3. How can it be identified?
4. How can it be safely reproduced?
5. What is the potential impact?
6. How can it be remediated?
7. How should it be documented?

Important concepts:

* CVE
* CVSS
* CWE
* Vulnerability validation
* False positives
* Remediation
* Security reporting

---

# 14. Professional Pentesting Methodology

My long-term goal is to follow a structured methodology rather than simply running individual tools.

```text
Reconnaissance
      ↓
Enumeration
      ↓
Attack Surface Mapping
      ↓
Vulnerability Discovery
      ↓
Validation
      ↓
Exploitation
      ↓
Initial Access
      ↓
Privilege Escalation
      ↓
Post-Exploitation
      ↓
Evidence Collection
      ↓
Reporting
      ↓
Remediation
      ↓
Retesting
```

---

# 15. Tools Currently Used / Studied

### Network

* Nmap
* Wireshark

### Web

* Burp Suite
* PortSwigger Web Security Academy
* Browser developer tools

### Linux

* Kali Linux
* Linux command line
* SSH

### CTF / Labs

* OverTheWire
* Mr. Robot CTF
* PentestGarage
* TryHackMe
* Hack The Box

### Reverse Engineering

* Ghidra
* GDB
* strings
* objdump

### Future IoT / Embedded

* Binwalk
* Logic analyzer
* UART tools
* JTAG tools

---

# 16. Current Learning Status

### ✅ Practiced

* Linux fundamentals
* Nmap
* Network enumeration
* Service enumeration
* Burp Suite
* HTTP request interception
* SQL Injection
* WAF behavior
* PortSwigger labs
* OverTheWire Bandit
* CTF methodology
* Mr. Robot CTF
* Linux enumeration
* Privilege escalation concepts

### 📚 Studied Conceptually

* XSS
* Authentication vulnerabilities
* Authorization
* Access control
* IDOR
* Session attacks
* Session replay
* Path traversal
* LFI/RFI
* Command injection
* API attacks
* MITM
* Eavesdropping
* DoS/DDoS
* Phishing
* Social engineering
* Credential attacks
* Botnets
* Malware
* APTs
* Drive-by downloads
* Identity theft

### 🔄 Currently Developing

* Advanced web security
* API security
* Linux privilege escalation
* Network pentesting
* Wireshark
* Windows security
* Active Directory

### 🎯 Future Focus

* Active Directory
* Reverse engineering
* Firmware security
* IoT security
* Embedded security
* Hardware security
* SOC / detection
* Professional penetration-testing reports

---

# 17. Learning Philosophy

The objective is not to memorize attack payloads or collect tool names.

For every vulnerability, I want to understand:

```text
Cause
 ↓
Detection
 ↓
Exploitation
 ↓
Impact
 ↓
Evidence
 ↓
Remediation
```

The ultimate goal is to develop practical security skills while combining my **Electronics & Communication Engineering background with cybersecurity**, particularly in **IoT, embedded systems, hardware, and connected devices**.

---

## 📌 Disclaimer

All practical security testing documented in this repository is performed against intentionally vulnerable applications, CTF environments, labs, or systems for which I have authorization.

This repository is intended for cybersecurity education and defensive security research.
