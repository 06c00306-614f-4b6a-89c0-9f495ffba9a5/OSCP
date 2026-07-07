# Proxychains

## Config (/etc/proxychains.conf)
```bash
# Add at bottom:
socks4 127.0.0.1 1080
# OR
socks5 127.0.0.1 1080
```

## Usage
```bash
# Prepend proxychains to any command
proxychains nmap -sT -Pn -p 80,445 10.0.0.100
proxychains crackmapexec smb 10.0.0.100 -u admin -p pass
proxychains impacket-wmiexec $DOMAIN/admin@10.0.0.100
```

## Limitations
- Only TCP (no UDP with default config)
- No SYN scan (-sT only, with proxychains)
- Slower due to proxying
