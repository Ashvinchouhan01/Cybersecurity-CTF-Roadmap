# 📋 Linux Command Cheatsheet

> **C!p#3r Club IIST** | Quick reference for CTF and penetration testing

---

## 📂 File System Navigation

```bash
pwd                        # Print working directory
ls                         # List files
ls -la                     # List all files with details (includes hidden)
ls -lah                    # Human-readable sizes
cd /path/to/dir            # Change directory
cd ..                      # Go up one level
cd ~                       # Go to home directory
cd -                       # Go to previous directory
```

---

## 📄 File Operations

```bash
cat file.txt               # Print file contents
cat -n file.txt            # Print with line numbers
head -20 file.txt          # First 20 lines
tail -20 file.txt          # Last 20 lines
tail -f file.txt           # Follow log file in real-time
less file.txt              # Scrollable viewer (q to quit)

cp source dest             # Copy file
cp -r source/ dest/        # Copy directory recursively
mv source dest             # Move / rename
rm file                    # Delete file
rm -rf directory/          # Delete directory (careful!)
mkdir -p /path/to/dir      # Create directory (with parents)
touch file.txt             # Create empty file
```

---

## 🔍 Search & Find

```bash
# Find files
find / -name "flag.txt" 2>/dev/null
find / -name "*.txt" -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null    # SUID files (privesc!)
find / -writable -type f 2>/dev/null     # Writable files

# Search in files
grep "flag" file.txt                     # Search in file
grep -r "flag" /directory/               # Recursive search
grep -i "flag" file.txt                  # Case-insensitive
grep -rn "password" /etc/ 2>/dev/null    # With line numbers
grep -v "comment" file.txt               # Invert match

# Which / locate
which nmap                               # Find binary path
locate filename                          # Find file (faster, uses database)
```

---

## 📝 Text Processing

```bash
# sed — stream editor
sed 's/old/new/g' file.txt              # Replace text
sed -n '5,10p' file.txt                 # Print lines 5-10
sed '/pattern/d' file.txt              # Delete matching lines

# awk — field processor
awk '{print $1}' file.txt              # Print first column
awk -F: '{print $1}' /etc/passwd       # Custom delimiter (: here)
awk 'NR==5' file.txt                   # Print line 5

# cut
cut -d: -f1 /etc/passwd                # Cut by delimiter
cut -c1-10 file.txt                    # Cut characters 1-10

# sort & uniq
sort file.txt                          # Alphabetical sort
sort -n file.txt                       # Numeric sort
sort -u file.txt                       # Sort + remove duplicates
uniq file.txt                          # Remove adjacent duplicates
sort file.txt | uniq -c                # Count occurrences

# wc — word count
wc -l file.txt                         # Count lines
wc -c file.txt                         # Count bytes
```

---

## 🔐 Permissions

```bash
# View permissions: rwxrwxrwx (owner/group/others)
ls -la

# chmod — change permissions
chmod 755 file           # rwxr-xr-x
chmod +x script.sh       # Add execute
chmod -w file            # Remove write
chmod u+s binary         # Set SUID bit

# chown — change owner
chown user:group file
chown -R user:group directory/

# Permission numbers:
# 4 = read, 2 = write, 1 = execute
# 7 = rwx, 6 = rw-, 5 = r-x, 4 = r--
```

---

## 👤 Users & Groups

```bash
whoami                   # Current user
id                       # User/group IDs
who                      # Logged-in users
w                        # Detailed who
su username              # Switch user
sudo command             # Run as root
sudo -l                  # List sudo permissions (privesc check!)
sudo -i                  # Root shell

cat /etc/passwd          # List all users
cat /etc/shadow          # Password hashes (root only)
cat /etc/group           # Groups

adduser newuser          # Add user
usermod -aG sudo user    # Add to sudo group
```

---

## 🌐 Networking Commands

```bash
# IP information
ip a                     # Network interfaces & IPs
ip route                 # Routing table
ifconfig                 # (older) network info
hostname -I              # Quick IP

# Connections
ss -tulpn                # Listening ports & services
netstat -ano             # All connections (older)
lsof -i :80              # Who's using port 80

# DNS & connectivity
ping target.com
nslookup target.com
dig target.com
host target.com
whois target.com
curl -I http://target.com   # HTTP headers only
curl -s http://target.com   # Silent mode
wget http://target.com/file  # Download file

# Netcat
nc -lvnp 4444              # Listen (catch reverse shell)
nc target-ip 4444          # Connect
nc -z target-ip 1-1024     # Port scan
```

---

## 📦 Archives

```bash
# tar
tar -xvf archive.tar          # Extract
tar -xvzf archive.tar.gz      # Extract gzip
tar -xvjf archive.tar.bz2     # Extract bzip2
tar -cvzf archive.tar.gz dir/ # Create gzip archive

# zip/unzip
unzip archive.zip
unzip archive.zip -d /output/
zip -r archive.zip directory/

# 7zip
sudo apt install -y p7zip-full
7z x archive.7z
7z l archive.7z               # List contents

# gzip
gzip file                     # Compress
gunzip file.gz                # Decompress
```

---

## 🔧 Process Management

```bash
ps aux                   # All running processes
ps aux | grep nginx      # Find specific process
top                      # Interactive process viewer
htop                     # Better top (install: apt install htop)

kill PID                 # Send SIGTERM to process
kill -9 PID              # Force kill (SIGKILL)
killall processname      # Kill by name

jobs                     # List background jobs
bg                       # Resume in background
fg                       # Bring to foreground
command &                # Run in background
nohup command &          # Run even if terminal closes
```

---

## 🔗 Pipes & Redirection

```bash
command | command2       # Pipe output to next command
command > file           # Redirect stdout to file (overwrite)
command >> file          # Redirect stdout to file (append)
command 2> errors.txt    # Redirect stderr
command &> all.txt       # Redirect both stdout and stderr
command < input.txt      # Read from file

# Useful pipe chains:
cat /etc/passwd | cut -d: -f1 | sort     # Get sorted usernames
ps aux | grep python | awk '{print $2}'  # Get PIDs of python
netstat -tulpn | grep LISTEN             # Listening ports only
```

---

## 🔒 SSH

```bash
ssh user@target-ip                        # Connect
ssh user@target-ip -p 2222               # Custom port
ssh -i keyfile.pem user@target-ip        # Use private key
ssh user@target-ip -L 8080:localhost:80  # Port forwarding

# Generate key pair
ssh-keygen -t rsa -b 4096

# Copy public key to server
ssh-copy-id user@target-ip

# SCP — secure copy
scp file.txt user@target:/path/          # Upload
scp user@target:/path/file.txt ./        # Download
scp -r directory/ user@target:/path/     # Upload directory
```

---

## 📜 Bash Scripting Quick Reference

```bash
#!/bin/bash
# Variables
NAME="cipher"
echo "Hello, $NAME"

# Arguments
echo "Script: $0"
echo "Arg1: $1"
echo "All args: $@"

# Conditionals
if [ "$1" == "hello" ]; then
    echo "Hi there!"
elif [ "$1" == "bye" ]; then
    echo "Goodbye!"
else
    echo "Unknown"
fi

# Loops
for i in {1..10}; do
    echo "Number: $i"
done

for file in *.txt; do
    echo "Processing: $file"
done

while [ condition ]; do
    # commands
done

# Functions
my_function() {
    echo "I am a function"
    return 0
}
my_function

# Read input
read -p "Enter password: " -s password
echo ""
echo "Got: $password"
```

---

## 🎯 CTF-Specific Commands

```bash
# String/encoding
echo "hello" | base64              # Encode base64
echo "aGVsbG8=" | base64 -d        # Decode base64
echo "hello" | xxd                 # Hex dump
echo "68656c6c6f" | xxd -r -p     # Hex to string

# File analysis
file unknown_file                  # Identify file type
strings binary | grep -i flag      # Find strings in binary
strings binary | grep -E "flag\{|CTF\{"
xxd binary | head -20              # Hex view
binwalk firmware.bin               # Embedded file detection

# Hashing
echo -n "password" | md5sum        # MD5 (no newline)
echo -n "password" | sha256sum     # SHA256
md5sum file.txt                    # Hash a file

# ROT13
echo "uryyb" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# URL decode
python3 -c "import urllib.parse; print(urllib.parse.unquote('%48%65%6c%6c%6f'))"
```

---

<div align="center">

*C!p#3r Club IIST — Decode · Defend · Dominate* 🔐

[Back to README](../../README.md)

</div>
