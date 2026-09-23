# Mr. Robot CTF — Pentesting Walkthrough

## Overview

This write-up documents my learning from the Mr. Robot CTF, focusing on the methodology used during reconnaissance, enumeration, exploitation, initial access, and privilege escalation.

> Environment: Authorized CTF/lab environment

---

## Skills Practiced

- Network reconnaissance
- Nmap
- Service enumeration
- Web enumeration
- WordPress enumeration
- Directory discovery
- Credential discovery
- Initial access
- Linux enumeration
- Privilege escalation
- Post-exploitation
- CTF methodology

---

# 1. Reconnaissance

The first step was identifying the target and determining which services were exposed.

### Nmap

I used Nmap to identify open ports and running services.

```bash
nmap -sV <TARGET>

The scan helped identify the available attack surface.

Key lesson

Enumeration should come before exploitation.

Instead of immediately attacking a service, I first tried to understand:

Which ports were open?
Which services were running?
What technologies were being used?
Which services could potentially provide an entry point?
2. Web Enumeration

After identifying the web service, I investigated the web application.

Areas checked included:

Web pages
robots.txt
Hidden directories
Application technologies
WordPress endpoints

The goal was to identify information that could reveal additional attack paths.

3. WordPress Enumeration

The target was identified as using WordPress.

I investigated the WordPress installation and looked for:

Users
Login functionality
Plugins
Themes
Exposed files
Other useful information

This demonstrated why technology identification during enumeration is important.

4. Initial Access

After enumeration, I identified a path that allowed initial access to the target.

The important part of this stage was understanding the chain:

Reconnaissance
      ↓
Enumeration
      ↓
Vulnerability Identification
      ↓
Exploitation
      ↓
Initial Access

The objective was to obtain a shell in the authorized CTF environment.

5. Linux Enumeration

After obtaining access, I enumerated the compromised system.

I investigated:

Current user
User privileges
Running processes
Files
Permissions
Configuration files
Potential privilege-escalation paths

Useful commands included:

whoami
id
uname -a
pwd
ls -la
Key lesson

Getting a shell does not necessarily mean the system is fully compromised.

The next question is:

What privileges does the current user have?

6. Privilege Escalation

The next stage was identifying a method to move from a lower-privileged account toward higher privileges.

Conceptually:

Low Privilege
     ↓
System Enumeration
     ↓
Misconfiguration / Vulnerability
     ↓
Privilege Escalation
     ↓
Higher Privilege

This section helped me understand the importance of system enumeration after initial access.

7. Flags / Keys

The CTF contained multiple keys/flags that were discovered throughout the attack chain.

The important lesson was not simply obtaining the flags, but understanding how each stage of the attack led to the next stage.

8. Attack Chain

The overall attack path can be summarized as:

Reconnaissance
      ↓
Nmap
      ↓
Service Enumeration
      ↓
Web Enumeration
      ↓
WordPress Discovery
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
9. What I Learned
Technical
How Nmap contributes to attack-surface discovery
Importance of service enumeration
Web application enumeration
WordPress security testing
Linux enumeration after obtaining access
Privilege escalation methodology
Using Linux commands during CTFs
Thinking about attack chains rather than individual vulnerabilities
Methodology

The biggest lesson was:

Do not blindly run tools or commands. Understand what information each step provides and use that information to determine the next step.

10. Tools Used
Kali Linux
Nmap
Linux command line
Web browser
WordPress enumeration techniques
11. Takeaways

This CTF helped connect individual cybersecurity concepts into a complete penetration-testing workflow:

Recon → Enumeration → Exploitation → Initial Access → Privilege Escalation → Post-Exploitation

The experience also highlighted areas I want to study further:

Linux privilege escalation
Web application security
Active Directory
API security
Reverse engineering
IoT and embedded security
Disclaimer

This write-up documents learning performed in an intentionally vulnerable CTF/lab environment. The techniques described should only be used against systems where testing is explicitly authorized
