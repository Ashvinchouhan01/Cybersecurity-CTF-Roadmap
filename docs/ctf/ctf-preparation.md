# 🏆 CTF Preparation Guide

> **C!p#3r Club IIST** | Your complete guide to competing in Capture The Flag competitions

---

## What is a CTF?

A **Capture The Flag (CTF)** is a cybersecurity competition where participants solve security challenges to find hidden strings called **"flags"**.

A flag usually looks like: `flag{s0me_r4nd0m_text_here}` or `C1P#3R{your_flag_here}`

You submit the flag to a scoring platform to earn points. The team with the most points wins!

---

## Types of CTF Competitions

| Type | How it Works | Best For |
|---|---|---|
| **Jeopardy** | Individual challenges in categories, pick what you want | Beginners (most common) |
| **Attack-Defense** | Your team defends a server while attacking others | Advanced teams |
| **King of the Hill** | Multiple teams attack the same machine | Intermediate |

> **The IIST CTF will be Jeopardy-style.** Focus on this format!

---

## CTF Challenge Categories

### 🌐 Web Exploitation

**What it is:** Find and exploit vulnerabilities in web applications.

**Common vulnerabilities:**
- SQL Injection (SQLi)
- Cross-Site Scripting (XSS)
- Local File Inclusion (LFI)
- Server-Side Template Injection (SSTI)
- Insecure Direct Object Reference (IDOR)
- Command Injection
- XXE (XML External Entity)
- SSRF (Server-Side Request Forgery)
- JWT attacks

**Tools:** Burp Suite, ffuf, gobuster, sqlmap, curl, browser DevTools

**Practice:** PortSwigger Web Academy, TryHackMe, PicoCTF

---

### 🔐 Cryptography

**What it is:** Decode, decrypt, or break encryption schemes.

**Common challenge types:**
- Classical ciphers (Caesar, Vigenere, ROT13)
- Encoding (Base64, Base32, Hex, Binary)
- RSA attacks (small exponent, common factor, wiener's attack)
- XOR cipher breaking
- Hash cracking (MD5, SHA-1)
- AES mode attacks (ECB, CBC bit-flipping)

**Tools:** CyberChef, Python, hashcat, RsaCtfTool, SageMath

**Practice:** CryptoHack, PicoCTF crypto category

---

### ⚙️ Reverse Engineering

**What it is:** Analyze a compiled binary to find how it works and extract the flag.

**Common challenge types:**
- Crackme binaries (find the correct password)
- Obfuscated code analysis
- Android APK reversing
- .NET / Java decompiling
- Packed binaries

**Tools:** Ghidra, IDA Free, GDB, Radare2, strings, ltrace, strace, jadx (Android)

**Practice:** PicoCTF, challenges.re, crackmes.one

---

### 💥 Binary Exploitation (Pwn)

**What it is:** Exploit memory vulnerabilities in compiled programs to get a shell or read the flag.

**Common vulnerabilities:**
- Buffer Overflow (stack-based)
- Format String vulnerabilities
- Return-Oriented Programming (ROP)
- Heap exploitation
- Use-After-Free

**Tools:** pwntools, GDB with PEDA/GEF/pwndbg, ROPgadget, checksec

**Practice:** pwn.college, PicoCTF pwn category, OverTheWire: Narnia

---

### 🔍 Forensics

**What it is:** Analyze files, memory dumps, disk images, or network captures to find hidden data.

**Common challenge types:**
- File analysis (metadata, file type tricks)
- Steganography (hidden data in images/audio)
- Network packet analysis (PCAP files)
- Memory forensics (RAM dumps)
- Disk forensics (deleted files)
- Log analysis

**Tools:** Wireshark, Volatility, Autopsy, binwalk, exiftool, steghide, strings, foremost

**Practice:** CyberDefenders, TryHackMe forensics rooms

---

### 🕵️ OSINT

**What it is:** Find information about a target using publicly available sources.

**Common challenge types:**
- Identify a location from an image
- Find information about a social media account
- Domain/IP investigation
- Geolocation challenges

**Tools:** Google Dorks, Sherlock, Maltego, Shodan, Censys, exiftool

**Practice:** TraceLabs OSINT CTF, Bellingcat challenges

---

### 🖼️ Steganography

**What it is:** Find data hidden inside images, audio, video, or text files.

**Common hiding methods:**
- LSB (Least Significant Bit) in images
- Hidden data in audio frequencies
- Text in whitespace
- Data in file metadata

**Tools:** steghide, stegsolve, binwalk, Audacity, zsteg, strings

---

## 🧠 CTF Strategy & Tips

### Before the Competition

- [ ] Form a team of 3-5 people with different strengths
- [ ] Set up your tools in advance (Kali Linux, Burp Suite, etc.)
- [ ] Practice on at least 10 CTF challenges in each category you plan to attempt
- [ ] Bookmark your essential resources (CyberChef, GTFOBins, HackTricks)
- [ ] Have a notes template ready for each challenge type

### During the Competition

```
1. READ ALL CHALLENGES FIRST — scan everything before starting
2. Sort by points (easy ones first — small wins build momentum)
3. Divide challenges by team member expertise
4. Set a 30-minute timer per challenge — if stuck, ask for help or move on
5. NEVER give up on a challenge you're 80% through
6. Take notes on everything you try (prevents repeating work)
7. Communicate constantly with your team
8. Check hints if available (small point penalty is worth it to solve)
```

### Common Beginner Mistakes

- ❌ Spending hours on one hard challenge while easy ones go unsolved
- ❌ Not reading the challenge description carefully (the answer is often hinted)
- ❌ Forgetting to try the obvious (default passwords, `admin:admin`)
- ❌ Not checking file metadata (`exiftool`, `strings`)
- ❌ Forgetting to check the page source in web challenges
- ❌ Not using `file` command to verify actual file type

---

## 🔧 CTF Toolkit — Ready When You Are

```bash
# One-time setup on Kali Linux

# Web
sudo apt install -y burpsuite gobuster ffuf sqlmap nikto wfuzz

# Reversing
sudo apt install -y ghidra gdb radare2 ltrace strace binutils

# Forensics
sudo apt install -y wireshark volatility3 binwalk exiftool foremost autopsy steghide

# Crypto
pip3 install pycryptodome gmpy2 sympy

# Pwn
pip3 install pwntools
sudo apt install -y gdb-peda

# All-purpose
pip3 install requests beautifulsoup4 z3-solver
```

---

## 📚 Essential CTF Resources

| Resource | Link | Use Case |
|---|---|---|
| CyberChef | [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/) | Encoding/decoding anything |
| GTFOBins | [gtfobins.github.io](https://gtfobins.github.io/) | Linux privesc |
| HackTricks | [book.hacktricks.xyz](https://book.hacktricks.xyz/) | Everything |
| RevShells | [revshells.com](https://revshells.com) | Reverse shell one-liners |
| CrackStation | [crackstation.net](https://crackstation.net/) | Hash cracking |
| RsaCtfTool | [GitHub](https://github.com/RsaCtfTool/RsaCtfTool) | RSA attacks |
| dcode.fr | [dcode.fr](https://www.dcode.fr/) | Cipher identification & decryption |
| Explainshell | [explainshell.com](https://explainshell.com/) | Understand Linux commands |

---

## 📝 Challenge Notes Template

```markdown
## Challenge: [Challenge Name]
**Category:** Web / Crypto / RE / Pwn / Forensics / OSINT
**Points:** 100
**Author:** [Author]

### Description
[Paste challenge description here]

### Files/Links
- [attachment.zip]
- http://challenge.url/

### Initial Observations
- File type: `file attachment.bin`
- Strings found: `strings attachment.bin | head -50`
- 

### Approach
1. First I tried...
2. Then I noticed...
3. 

### Solution
[What worked]

### Flag
`flag{XXXXX}`

### What I learned
- 
```

---

<div align="center">

*C!p#3r Club IIST — Decode · Defend · Dominate* 🔐

</div>
