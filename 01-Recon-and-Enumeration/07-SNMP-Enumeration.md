# SNMP Enumeration (Port 161/udp)

```bash
# Scan SNMP community strings
onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt $TARGET_IP

# Walk MIB tree
snmpwalk -v2c -c public $TARGET_IP
snmpwalk -v1 -c public $TARGET_IP 1.3.6.1.4.1.77.1.2.25   # Windows users

# Check for readable community strings
nmap --script snmp-brute -p 161 $TARGET_IP
```
