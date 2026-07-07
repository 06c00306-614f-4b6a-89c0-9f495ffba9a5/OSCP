# 00 - Penetration Testing Methodology

## General Workflow

```
Reconnaissance → Enumeration → Vulnerability Analysis → Exploitation → Post-Exploitation → Privilege Escalation → Lateral Movement → Reporting
```

## Phase 1: Information Gathering
- **Passive**: Google dorking, Shodan, DNS recon, WHOIS, social media
- **Active**: Port scanning, service enumeration, OS fingerprinting

## Phase 2: Enumeration (80% of time spent here)
_"If you spend 80% of your time on enumeration, the other 20% will take care of itself."_

- Always scan **ALL 65535 ports** (not just top 1000)
- Always run version detection on every open port
- Always enumerate HTTP/HTTPS thoroughly
- Always check for default credentials

## Phase 3: Vulnerability Assessment
- Cross-reference service versions with known CVEs
- Look for misconfigurations
- Check for weak/default credentials
- Test web application attacks

## Phase 4: Exploitation
- Use public exploits (Searchsploit, Exploit-DB)
- Manual exploitation (SQLi, LFI→RCE, command injection)
- Buffer overflow (if applicable)
- Password spraying / credential reuse
- Metasploit (limited usage on exam - know manual alternatives)

## Phase 5: Privilege Escalation
- **Linux**: Kernel exploits, SUID, sudo misconfigs, cron jobs, capabilities
- **Windows**: Service misconfigs, vulnerable services, token impersonation, always-install-elevated

## Phase 6: Post-Exploitation & Lateral Movement
- Dump credentials (mimikatz, sam dump, hash extraction)
- Pivot to other machines (SSH tunneling, chisel, ligolo-ng)
- Active Directory attacks (Kerberoasting, AS-REP roasting, DCSync)

## Phase 7: Documentation
- Take screenshots at every step
- Note commands used and output
- Document proof.txt/local.txt contents

## Pro Tips
1. **Read ALL output** - don't skim
2. **Multiple machines**: Don't get stuck on one - rotate
3. **Reboot & retry** if a service seems broken
4. **Enumeration scripts** can miss things - verify manually
5. **Check /robots.txt, /sitemap.xml** on every web server
6. **Environment variables** - use `$TARGET_IP` to avoid mistakes
7. **Keep a timeline** of what you've tried
