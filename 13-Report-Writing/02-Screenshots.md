# Screenshot Best Practices

## What to Screenshot
| Phase | Screenshots Needed |
|---|---|
| **Enumeration** | Nmap scans, web pages, directory listing results |
| **Exploitation** | Exploit code, commands run, reverse shell connection |
| **Privilege Escalation** | Commands run, script output |
| **Proof** | `whoami`, `ipconfig`, proof.txt contents |
| **AD Attacks** | BloodHound paths, kerberoast results, DA access |

## Tips
1. Include your **IP** in every shell to prove it's your session
2. Include the **target IP** in command output
3. Show both **command AND output**
4. Text must be **legible** (standard 1920x1080 screen)
5. Capture all commands needed to reproduce the exploit
