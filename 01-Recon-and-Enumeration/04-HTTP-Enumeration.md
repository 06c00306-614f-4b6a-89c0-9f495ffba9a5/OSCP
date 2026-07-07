# HTTP/HTTPS Enumeration (Port 80, 443, 8080, 8443)

```bash
# WhatWeb
whatweb http://$TARGET_IP

# Wappalyzer (browser extension)
# View page source, check comments, JS files

# curl basics
curl -s -v http://$TARGET_IP
curl -s -I http://$TARGET_IP
curl -s http://$TARGET_IP/robots.txt
curl -s http://$TARGET_IP/sitemap.xml
curl -s http://$TARGET_IP/CHANGELOG.txt     # Drupal
curl -s http://$TARGET_IP/readme.html        # WordPress
curl -s http://$TARGET_IP/LICENSE.txt        # Various

# Nikto (Web Server Scanner)
nikto -h http://$TARGET_IP
```
