# Web Directory Enumeration

## Gobuster
```bash
# Directories
gobuster dir -u http://$TARGET_IP:80 -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 50 -x php,html,txt,asp,aspx,jsp

# With extensions
gobuster dir -u http://$TARGET_IP -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 100 -x php,html,txt

# Recursive
gobuster dir -u http://$TARGET_IP -w /usr/share/wordlists/dirb/common.txt -r

# VHOST fuzzing
gobuster vhost -u http://$TARGET_IP -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50
```

## Dirsearch
```bash
dirsearch -u http://$TARGET_IP -x 403,400,404 -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -R 2 -e php
```

## FFUF (Fast)
```bash
# Simple
ffuf -u http://$TARGET_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt

# With extensions
ffuf -u http://$TARGET_IP/FUZZ -w /usr/share/seclists/Discovery/Web-Content/common.txt -e .php,.html,.txt

# Subdomain vhost fuzzing
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://$TARGET_IP -H "Host: FUZZ.$TARGET_DOMAIN"
```
