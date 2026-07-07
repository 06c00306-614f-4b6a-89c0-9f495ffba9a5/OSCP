# Credential Reuse Methodology

One of the most common time-saving techniques: **found credentials rarely exist in isolation**. Always try every credential you find against every service.

## Workflow
```
Found credential → Test EVERYWHERE:
├── SSH (port 22)
├── SMB (port 445) via crackmapexec
├── WinRM (port 5985) via crackmapexec
├── RDP (port 3389)
├── FTP (port 21)
├── MySQL (port 3306)
├── MSSQL (port 1433)
├── HTTP login forms
├── Any web app admin panel
└── Email services (if present)
```

## Testing Commands
```bash
# After finding a credential user:password

# Test SMB
crackmapexec smb $TARGET_IP -u user -p password

# Test WinRM
crackmapexec winrm $TARGET_IP -u user -p password

# Test SSH
hydra -l user -p password ssh://$TARGET_IP

# Test FTP
hydra -l user -p password ftp://$TARGET_IP

# Test RDP
hydra -l user -p password rdp://$TARGET_IP

# Test MSSQL
crackmapexec mssql $TARGET_IP -u user -p password

# Test domain auth (if AD present)
crackmapexec smb $DOMAIN_CONTROLLER -u user -p password -d $DOMAIN
```

## Tips from Exam Passers
1. **Try username as password first** (e.g. `admin:admin`, `john:john`)
2. **Try file names and directory names as passwords** (e.g. a file called `backup.txt` → try password `backup`)
3. **Try every credential on every open service** — credential reuse is the #1 most common path
4. **Check for password in description fields** — LDAP or AD user descriptions often contain passwords
5. **Check browser saved passwords** if you get GUI access
6. **Try password against the domain even for local accounts** — sometimes they match
7. **Use nxc one username at a time** — it can fail silently with multi-user lists on MSSQL
