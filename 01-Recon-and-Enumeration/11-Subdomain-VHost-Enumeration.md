# Web Subdomain & VHost Enumeration

Subdomains and virtual hosts often expose admin panels, dev environments, and internal services.

## Subdomain Enumeration (DNS)
```bash
# FFUF
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -u http://$TARGET_IP -H "Host: FUZZ.$TARGET_DOMAIN" \
  -fs <default_response_size>

# Gobuster
gobuster dns -d $TARGET_DOMAIN -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -t 50
```

## VHost Enumeration (IP-based)
```bash
# When multiple sites share the same IP
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -u http://$TARGET_IP -H "Host: FUZZ" \
  -fs <default_response_size>

# With specific domain suffix
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt \
  -u http://$TARGET_IP -H "Host: FUZZ.example.com" \
  -fs <default_response_size>
```

## Common names to try
```
dev, staging, stage, test, admin, admin1, admin2, 
portal, internal, int, management, mgmt, 
backup, git, jenkins, jira, confluence, 
api, api-dev, api-test, ws, webservice,
mail, webmail, mail1, webmail1, 
vpn, remote, access, sso, auth, 
monitor, nagios, grafana, prometheus
```

## What to look for
```bash
# After finding subdomains, add to /etc/hosts and enumerate
echo "$TARGET_IP dev.$TARGET_DOMAIN staging.$TARGET_DOMAIN" >> /etc/hosts

# Then enumerate each one
gobuster dir -u http://dev.$TARGET_DOMAIN -w /usr/share/seclists/Discovery/Web-Content/common.txt
whatweb http://dev.$TARGET_DOMAIN
```
