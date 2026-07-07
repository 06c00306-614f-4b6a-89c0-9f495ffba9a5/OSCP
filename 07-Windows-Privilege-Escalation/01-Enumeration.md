# Windows PrivEsc Enumeration

## System Information
```cmd
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
wmic os get caption,version,csdversion
wmic computersystem get name,domain,manufacturer
```

## User & Group Info
```cmd
whoami
whoami /priv
whoami /groups
net user
net localgroup Administrators
net group /domain
```

## Network
```cmd
ipconfig /all
arp -a
route print
netstat -ano
netsh advfirewall show currentprofile
```

## Processes & Services
```cmd
tasklist /SVC
wmic service list brief
wmic product get name,version,vendor
```

## Automated Tools
```cmd
winpeas.exe
powershell -ep bypass; . .\PowerUp.ps1; Invoke-AllChecks
Seatbelt.exe -group=all
```

## Quick Wins Checklist
1. `whoami /priv` - Check privileges (SeImpersonate!)
2. `systeminfo` - OS version
3. `wmic service get name,displayname,pathname,startmode` - Services
4. `cmdkey /list` - Stored creds
5. Run **WinPEAS**
