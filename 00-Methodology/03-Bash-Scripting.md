# Bash Scripting Basics

## Variables & Environment
```bash
name="target"
echo $name
export TARGET_IP="10.10.10.10"
echo $TARGET_IP

# Special variables
$0    # Script name
$1    # First argument
$#    # Number of arguments
$@    # All arguments
$?    # Last command exit code
```

## Loops
```bash
# For loop
for ip in 10.10.10.{1..254}; do
    ping -c 1 $ip | grep "bytes from" | cut -d" " -f4 | cut -d":" -f1
done

# While loop read lines
while read line; do echo $line; done < file.txt
```

## Conditionals
```bash
if [ -f /etc/passwd ]; then echo "File exists"; fi

# Common tests
[ -f file ]   # File exists
[ -d dir ]    # Directory exists
[ -z "$var" ] # String is empty
[ "$a" = "$b" ] # Strings equal
[ $num -eq 10 ] # Numeric equal
```

## Functions
```bash
scan_port() {
    nc -zv -w1 $1 $2 2>&1 | grep succeeded
}
scan_port $TARGET_IP 80
```

## Useful One-Liners
```bash
# Port scanner with bash
for port in $(seq 1 1000); do (echo >/dev/tcp/$TARGET_IP/$port) 2>/dev/null && echo "$port open"; done

# Check multiple hosts
for ip in 10.10.10.{1..10}; do (ping -c1 $ip &>/dev/null && echo "$ip up") & done; wait

# Extract IPs from text
grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" file.txt

# Remove duplicates
awk '!seen[$0]++' file.txt
```
