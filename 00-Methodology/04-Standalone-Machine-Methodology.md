# Standalone Machine Methodology

Standalone machines (3 × 20pts = 60pts) each need: initial shell (local.txt = 10pts) + privilege escalation (proof.txt = 10pts).

## Standalone Linux Machine

### Phase 1: Initial Enumeration
```bash
# Run scans in parallel
nmap -p- --min-rate=1000 -T4 $TARGET_IP -oN nmap/ports.txt &
nmap -sU --top-ports=20 $TARGET_IP -oA nmap/udp &

# Service version scan
nmap -p $(open_ports_csv) -sC -sV -T4 $TARGET_IP -oA nmap/detailed

# Web recon (if port 80/443)
gobuster dir -u http://$TARGET_IP -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 50 -x php,html,txt &
whatweb http://$TARGET_IP &

# Check for high ports (8000, 8080, 8443, 9000, etc)
```

### Phase 2: Common Linux Initial Access Vectors
```bash
# Web app RCE (LFI→RCE, file upload, command injection)
# PHP application exploitation
# Public exploit for service version
# Weak credentials (SSH, FTP, MySQL)
# Unpatched web app (WordPress, Joomla, Drupal)
# SMB share with scripts/cron jobs
```

### Phase 3: Linux PrivEsc Flow
```bash
1. sudo -l                             # Sudo rights
2. find / -perm -4000 -type f 2>/dev/null # SUID
3. cat /etc/crontab + pspy64           # Cron jobs
4. getcap -r / 2>/dev/null             # Capabilities
5. ls -la /etc/passwd /etc/shadow      # File perms
6. history; ls -la /home/*/             # Loot
7. find / -writable -type f 2>/dev/null # Weak perms
8. uname -a → search for kernel exploit # Kernel
9. linpeas.sh                          # Full auto-enum
```

---

## Standalone Windows Machine

### Phase 1: Initial Enumeration
```bash
# Full port scan
nmap -p- --min-rate=1000 -T4 $TARGET_IP -oN nmap/ports.txt

# Targeted service scan
nmap -p $(open_ports_csv) -sC -sV -T4 $TARGET_IP -oA nmap/detailed

# SMB Checks
crackmapexec smb $TARGET_IP -u '' -p '' --shares
smbmap -H $TARGET_IP
enum4linux-ng -A $TARGET_IP

# Web checks (if IIS: 80/443/8080)
gobuster dir -u http://$TARGET_IP -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 50 -x asp,aspx,txt

# WinRM check
crackmapexec winrm $TARGET_IP -u 'admin' -p 'password'
```

### Phase 2: Common Windows Initial Access Vectors
```bash
# SMB
# - EternalBlue (MS17-010) if old/unpatched
# - SMB NULL session / guest access
# - Password spraying on SMB

# Web Application (IIS)
# - ASPX file upload
# - Directory traversal
# - Known CMS vulnerabilities

# RDP
# - BlueKeep (CVE-2019-0708) - very old
# - Password guessing

# MSSQL
# - Weak SA password → xp_cmdshell
# - SQL injection on web app

# WinRM
# - Credential brute force
```

### Phase 3: Windows PrivEsc Flow
```cmd
1. whoami /priv                        # Token privs (SeImpersonate!)
2. systeminfo                          # OS version
3. wmic service get name,displayname,pathname,startmode  # Service misconfigs
4. wmic qfe get Caption,Description,HotFixID,InstalledOn  # Patches
5. icacls "C:\Program Files\*"         # Weak perms
6. reg query ... AlwaysInstallElevated # Registry misconfigs
7. cmdkey /list                        # Stored creds
8. accesschk.exe -uwcqv "Users" *      # Service permissions
9. winpeas.exe                         # Auto-enum
```

---

## Finding Which OS You're Dealing With

```bash
# From nmap
nmap -O $TARGET_IP                     # OS detection
nmap --script smb-os-discovery -p 445 $TARGET_IP  # SMB OS info

# TTL (quick check)
ping -c1 $TARGET_IP
# TTL 64 = Linux, TTL 128 = Windows, TTL 255 = Cisco/Solaris

# From banner grabbing
nc -v $TARGET_IP 22                    # SSH banner: "SSH-2.0-OpenSSH_8.9p1 Ubuntu-3"
nc -v $TARGET_IP 80                    # HTTP Server header
nc -v $TARGET_IP 445                   # SMB protocol negotiation
```

---

## Machine Prioritization During Exam

```
EASIEST TO HARDEST (typical):

Linux standalone (web app → kernel/sudo/cron) 
  vs
Windows standalone (SMB/RDP → service/token)

Indicators of easier machines:
- Old software versions (unpatched)
- Common web apps (WordPress with plugins)
- Standard ports (80, 22, 445)
- Default/weak credentials

Indicators of harder machines:
- All high ports (obscure services)
- Custom web applications
- Very recent software (no public exploits)
- Tight firewall rules
```

---

## Typical Standalone Attack Chain

```
1. ENUMERATE EVERYTHING
   ├── Port scan (all 65535)
   ├── Version detection
   ├── Web directory brute force
   └── Run auto scripts early

2. FIND INITIAL VECTOR
   ├── Check for public exploits on versions
   ├── Try default/weak credentials
   ├── Test web application attacks
   └── Look for exposed admin panels

3. GET SHELL
   ├── Gain foothold via exploit/cracked creds
   ├── Capture local.txt
   └── Screenshot everything

4. ESCALATE PRIVILEGES
   ├── Run auto-enumeration (linpeas/winpeas)
   ├── Check sudo/services/SUID/tokens
   ├── Exploit misconfiguration
   └── Capture proof.txt

5. POST-EXPLOIT
   ├── Check for credentials/passwords
   ├── Check network routes (ipconfig, ip route)
   ├── Check other machines on subnet
   └── Check for interesting files
```
