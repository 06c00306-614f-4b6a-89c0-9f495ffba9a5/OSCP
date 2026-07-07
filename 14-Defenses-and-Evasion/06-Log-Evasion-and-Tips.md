# Log Evasion

## Linux
```bash
# Clear bash history
history -c
cat /dev/null > ~/.bash_history
unset HISTORYFILE
export HISTFILE=/dev/null

# Clear auth logs (requires root)
cat /dev/null > /var/log/auth.log
cat /dev/null > /var/log/syslog
```

## Windows
```cmd
# Clear PowerShell history
Remove-Item (Get-PSReadlineOption).HistorySavePath

# Clear event logs (requires admin)
wevtutil cl system
wevtutil cl security
wevtutil cl application
```

## OSCP Defense Mindset
- Check `whoami /priv` for token privileges
- Check Defender status (`Get-MpComputerStatus`)
- Check AppLocker rules (`Get-AppLockerPolicy`)
- Have multiple payload types (EXE, PS1, DLL, MSI)
- Have multiple transfer methods (HTTP, SMB, FTP)
- Run from writable Temp directories
- Rename payloads to look legitimate
- Use non-meterpreter payloads
