# Lab Setup & Environment Variables

## Kali Linux Configuration

### Essential Setup
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install essential tools
sudo apt install -y gobuster dirsearch seclists exploitdb bloodhound python3-impacket

# Create organized workspace
mkdir -p ~/oscp/{enum,exploit,privesc,loot,report}
```

### Environment Variables (per machine)
```bash
# Create .env file for each target
export TARGET_IP="10.10.10.10"
export TARGET_DOMAIN="corp.local"
export ATTACK_IP="10.10.14.10"
export PORT=80
export WORDLIST="/usr/share/wordlists/rockyou.txt"

# Then source it
source .env
```

### tmux Tips
```bash
# Split vertically: Ctrl+B %
# Split horizontally: Ctrl+B "
# New window: Ctrl+B c
# Synchronize panes: Ctrl+B :
#   then type: setw synchronize-panes on
```

### Terminal Multiplexer Workflow
- Pane 1: Nmap scan or service enumeration
- Pane 2: Web recon / directory busting
- Pane 3: Notes / CherryTree / Obsidian
- Pane 4: Listener (nc, metasploit)

### Important Paths
| Tool/Wordlist | Path |
|---|---|
| SecLists | `/usr/share/seclists/` |
| RockYou | `/usr/share/wordlists/rockyou.txt` (extract with `gunzip`) |
| Searchsploit exploits | `/usr/share/exploitdb/` |
| NSE scripts | `/usr/share/nmap/scripts/` |
