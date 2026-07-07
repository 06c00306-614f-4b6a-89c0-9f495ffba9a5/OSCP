# OSCP+ Exam Study Notes & Techniques

## Overview

The **Offensive Security Certified Professional (OSCP+)** certification validates practical, hands-on skills in ethical hacking and penetration testing. This guide covers the PEN-200 course curriculum and exam topics.

### Exam Details (2024-2025)
- **Duration**: 23 hours 45 minutes (exam) + 24 hours (report submission)
- **Format**: Proctored, VPN into a private network with vulnerable machines
- **Passing**: Must obtain a minimum point threshold by compromising machines
- **Machines**: Mix of standalone Linux/Windows + Active Directory set(s)
- **Report**: Professional penetration test report submitted as .7z
- **Retake**: Cooling period between attempts

### Key Changes (Nov 2024+)
- OSCP+ rebranding
- Bonus points for lab reports **removed**
- Increased focus on **Active Directory**
- **Assumed breach** scenario: AD set starts with provided credentials
- Partial credit awarded for AD chain (no longer all-or-nothing)
- Buffer overflow still tested but may vary
- Updated Body of Knowledge (BoK) published by OffSec
- Certification now expires after 3 years (OSCP+) unless CEUs earned

### Exam Scoring Scenarios (70/100 to pass)
| AD Set (40pts) | Standalone 1 (20pts) | Standalone 2 (20pts) | Standalone 3 (20pts) | Total |
|---|---|---|---|---|
| 40 (full chain) | 10 (local) | 10 (local) | 10 (local) | **70** ✅ |
| 40 (full chain) | 10 (local) | 10 (local) | — | 60 ❌ |
| 20 (partial) | 20 (full) | 20 (full) | 10 (local) | **70** ✅ |
| 10 (MS01 only) | 20 (full) | 20 (full) | 20 (full) | **70** ✅ |
| 0 | 20 | 20 | 20 | 60 ❌ |

**Strategy**: Attack AD set FIRST (guaranteed 40pts if you follow the chain), then pick off standalones. The AD set has the most predictable path.

### Core Domains Covered
1. **Information Gathering** - Passive & active reconnaissance
2. **Vulnerability Scanning** - Automated & manual (NSE, OpenVAS)
3. **Web Application Attacks** - SQLi, XSS, LFI/RFI, file upload, command injection, API testing, NoSQL
4. **Client-Side Attacks** - Macros, HTA, CHM, LNK, browser exploitation
5. **Exploitation** - Public exploits, manual exploitation, Metasploit, buffer overflows
6. **Password Attacks** - Online/offline cracking, hash dumping, pass-the-hash
7. **Privilege Escalation** - Linux (SUID, sudo, cron, capabilities) & Windows (services, tokens, AlwaysInstallElevated)
8. **Active Directory Attacks** - Enumeration, kerberos attacks (Kerberoast, AS-REP), lateral movement, DCSync, ADCS, domain dominance
9. **Pivoting & Tunneling** - SSH, chisel, ligolo-ng, DNS/ICMP, proxychains
10. **AWS Cloud** - S3, IAM, EC2, Lambda enumeration & exploitation
11. **Post-Exploitation** - Persistence, credential dumping, data exfiltration
12. **Defenses & Evasion** - AV/AMSI bypass, AppLocker, firewall evasion, LOLBins
13. **Bash Scripting & Practical Tools** - Automation, one-liners, nc, socat, curl
14. **Report Writing** - Professional documentation, exam submission
