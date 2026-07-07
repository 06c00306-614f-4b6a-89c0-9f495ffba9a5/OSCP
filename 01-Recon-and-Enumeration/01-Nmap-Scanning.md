# Nmap Scanning

## Full Port Scan
```bash
# Fast full TCP scan
nmap -p- --min-rate=1000 -T4 $TARGET_IP -oN nmap/ports.txt

# Full scan with OS detection (slower)
nmap -p- -sV -sC -O -T4 $TARGET_IP -oA nmap/full

# Version + default scripts on open ports only
nmap -p <PORTS> -sC -sV -T4 $TARGET_IP -oA nmap/detailed

# UDP scan (top ports)
nmap -sU --top-ports=20 $TARGET_IP -oA nmap/udp

# Quick service scan
nmap --min-rate 100 -sV -sC -T4 $TARGET_IP -oA nmap/versions

# OS detection
nmap -O $TARGET_IP
```

## NSE Scripts
```bash
# Vulnerability scan
nmap --script vuln -p <PORTS> $TARGET_IP

# SMB enumeration
nmap --script smb-enum-shares,smb-enum-users,smb-os-discovery -p 445 $TARGET_IP

# HTTP enumeration
nmap --script http-enum,http-headers,http-title,http-methods -p 80,443 $TARGET_IP

# MySQL enumeration
nmap --script mysql-enum -p 3306 $TARGET_IP

# DNS zone transfer
nmap --script dns-zone-transfer -p 53 $TARGET_IP

# SMB security mode (check signing)
nmap --script smb-security-mode -p 445 $TARGET_IP

# MSSQL info
nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-brute -p 1433 $TARGET_IP
```

## nmapAutomator (Convenience Script)
```bash
sudo nmapAutomator.sh -H $TARGET_IP -t all
sudo nmapAutomator.sh -H $TARGET_IP -t vulns
```

## Quick OS Detection
```bash
# TTL check
ping -c1 $TARGET_IP
# TTL 64 = Linux, TTL 128 = Windows, TTL 255 = Cisco
```
