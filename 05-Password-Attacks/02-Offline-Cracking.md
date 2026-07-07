# Offline Password Cracking

## Hash Identification
```bash
hashid <hash>
hash-identifier
```

## Hashcat
```bash
# Basic
hashcat -m <mode> hash.txt /usr/share/wordlists/rockyou.txt --force

# With rules
hashcat -m <mode> hash.txt /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-3000.rule --force

# Show cracked
hashcat -m <mode> hash.txt --show

# Mask attack
hashcat -m <mode> hash.txt -a 3 ?l?l?l?l?l?l?l
```

### Common Hashcat Modes
| Hash | Mode | Use |
|---|---|---|
| NTLM | 1000 | Windows SAM |
| NetNTLMv2 | 5600 | SMB capture |
| Kerberos TGS | 13100 | Kerberoast |
| AS-REP | 18200 | AS-REP roast |
| bcrypt | 3200 | Linux shadow |
| SHA512crypt | 1800 | Linux shadow ($6$) |
| KeePass | 13400/23700 | KDBX cracking |
| MD5 | 0 | Web DBs |
| SHA1 | 100 | Old systems |
| JWT | 16500 | JWT tokens |

## John the Ripper
```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt --rules hash.txt
john --show hash.txt
```

## John's Extraction Tools
| Tool | File Type |
|---|---|
| `ssh2john id_rsa` | SSH private key → mode 22921 |
| `zip2john file.zip` | ZIP file → mode 17220 |
| `pdf2john file.pdf` | PDF → mode 10500 |
| `keepass2john file.kdbx` | KeePass → mode 13400/23700 |
| `office2john file.docx` | Office docs → mode 9600 |
| `unshadow passwd shadow` | Linux combined shadow |
| `gpg2john private.asc` | PGP key |

## Wordlist Generation
```bash
cewl http://$TARGET_IP -w wordlist.txt
hashcat -r demo.rule --stdout wordlist.txt > mutated.txt
```
