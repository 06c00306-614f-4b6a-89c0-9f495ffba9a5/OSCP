# Linux Looting for Credentials

## Files to Find
```bash
# SSH keys
find / -name id_rsa 2>/dev/null
find / -name authorized_keys 2>/dev/null
find / -name "*.pem" 2>/dev/null

# Passwords in configs
grep -r "password" /etc/ 2>/dev/null
grep -r "password" /var/www/ 2>/dev/null
grep -r "password" /home/ 2>/dev/null

# Database files
find / -name "*.db" 2>/dev/null
find / -name "*.sqlite" 2>/dev/null

# Old password files
cat /etc/security/opasswd
cat /var/backups/*
cat /etc/shadow-
```

## Loot Commands
```bash
# Bash history
cat ~/.bash_history
cat ~/.bashrc

# Application configs
cat /var/www/html/config.php
cat /var/www/html/wp-config.php
cat /var/www/html/.env
cat /etc/mysql/debian.cnf

# Logs
tail -100 /var/log/auth.log
tail -100 /var/log/apache2/access.log
cat /var/log/mysql/error.log
```
