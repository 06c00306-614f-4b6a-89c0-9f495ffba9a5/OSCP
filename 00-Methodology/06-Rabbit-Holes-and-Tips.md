# Common Rabbit Holes & Time-Savers

## What Experienced Passers Say Saves the Most Time

### 1. Reverse Shell Not Connecting?
```bash
# Don't keep debugging your shellcode — first check:
# - Is the port open outbound? Try different ports (80, 443, 53)
# - Is the firewall blocking? Use common allowed ports
# - Try a different shell type (python → nc → bash)
# - Check if the target even has netcat installed
# - Try connecting back to yourself to verify: nc -lvnp 4444 on attack box
```

### 2. Public Exploit Fails?
```bash
# Most common reasons (in order):
# 1. Wrong offset/return address for your target's OS version
# 2. Bad characters differ from the exploit author's target
# 3. Missing dependencies (Python 2 vs 3, missing libraries)
# 4. The target is patched (but this is least likely in OSCP)
# 
# Fix: search for alternative exploits for same CVE, check comments
```

### 3. Web Application Access Issues
```bash
# If a web page doesn't load as expected:
curl -s -v http://$TARGET_IP   # Check for HTTP headers
# Look for: redirects, cookie requirements, WAF blocks, TLS issues
```

### 4. Stuck? Try This Order
1. Run a **UDP scan** in background — this often reveals missed services
2. Re-read your **nmap output carefully** — don't skim
3. Check for **non-standard ports** running standard services
4. Try **default credentials** on every exposed service
5. Try **credential reuse** — all found creds on all services
6. Fuzz for **hidden files and directories** (different wordlists)
7. Check for **subdomains/vhosts** (even on IP-based targets)
8. Look at **page source code, JavaScript files, comments**
9. Go back to **basics** — run linpeas/winpeas again with fresh eyes

### 5. Port Knocking / Service Guards
```bash
# Some OSCP boxes use port knocking or only expose services
# after certain conditions are met:
# - Try accessing services in sequence
# - Check if a firewall rule opens on specific connections
# - Check if a cron job periodically starts/stops services
```

### 6. When Everything Fails
- Take a **30-minute break** away from the screen
- **Switch to a different machine** — rotate every 2-3 hours
- Read your notes out loud — you might catch what you missed
- Ask yourself: "What haven't I enumerated yet?"
