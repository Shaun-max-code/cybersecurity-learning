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
