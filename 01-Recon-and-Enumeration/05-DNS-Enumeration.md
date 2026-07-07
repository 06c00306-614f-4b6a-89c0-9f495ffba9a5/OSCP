# DNS Enumeration (Port 53)

```bash
# DNS enumeration
dnsrecon -d $TARGET_DOMAIN -t std
dnsrecon -d $TARGET_DOMAIN -t brt -D /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt

# Zone transfer
dig axfr @$TARGET_IP $TARGET_DOMAIN
host -l $TARGET_DOMAIN $TARGET_IP

# Subdomain enumeration
gobuster dns -d $TARGET_DOMAIN -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50
```
