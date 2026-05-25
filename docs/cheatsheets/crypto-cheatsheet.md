# 🔐 Cryptography Cheatsheet

> **C!p#3r Club IIST** | Quick reference for crypto CTF challenges

---

## 🔎 Identify the Encoding / Cipher First

| Pattern | Likely Encoding | Decode With |
|---|---|---|
| Ends with `=` or `==`, A-Z/a-z/0-9/+/ | Base64 | `base64 -d` or CyberChef |
| Only 0-9 and A-F (hex chars) | Hexadecimal | `xxd -r -p` or CyberChef |
| 0s and 1s only | Binary | CyberChef "From Binary" |
| Numbers 0-255 separated by spaces | ASCII decimal | CyberChef "From Charcode" |
| Morse code (dots and dashes) | Morse | dcode.fr |
| Looks like shifted alphabet | Caesar / ROT | Try all 26 rotations |
| Encoded but with weird chars | Maybe Base32, Base58, Base85 | CyberChef |
| Numbers that look like dates/coords | Maybe a custom encoding | Think creatively |

```bash
# Quick identification helpers
echo "SGVsbG8=" | base64 -d        # Test base64
echo "48656c6c6f" | xxd -r -p     # Test hex
python3 -c "print(chr(72)+chr(101)+chr(108)+chr(108)+chr(111))"  # Test decimal ASCII
```

---

## 📦 Encoding (Not Encryption!)

### Base64

```bash
# Encode
echo "hello" | base64                  # aGVsbG8K (with newline)
echo -n "hello" | base64               # aGVsbG8= (no newline)

# Decode
echo "aGVsbG8=" | base64 -d
base64 -d encoded.txt

# Python
import base64
base64.b64encode(b"hello")             # b'aGVsbG8='
base64.b64decode("aGVsbG8=")          # b'hello'
```

### Hexadecimal

```bash
# String to hex
echo -n "hello" | xxd -p              # 68656c6c6f
echo -n "hello" | od -A x -t x1z -v

# Hex to string
echo "68656c6c6f" | xxd -r -p         # hello

# Python
"hello".encode().hex()                 # '68656c6c6f'
bytes.fromhex("68656c6c6f").decode()  # 'hello'
```

### ROT13

```bash
echo "uryyb" | tr 'A-Za-z' 'N-ZA-Mn-za-m'  # hello

# Python
import codecs
codecs.decode("uryyb", "rot13")        # 'hello'
```

---

## 🔄 Classical Ciphers

### Caesar Cipher

Shift each letter by a fixed amount.

```python
def caesar_decrypt(text, shift):
    result = ""
    for char in text:
        if char.isalpha():
            base = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - base - shift) % 26 + base)
        else:
            result += char
    return result

# Brute force all 26 shifts
ciphertext = "KHOOR"
for shift in range(26):
    print(f"Shift {shift}: {caesar_decrypt(ciphertext, shift)}")
```

### Vigenere Cipher

```python
# Use dcode.fr → Vigenere decoder (autosolve)
# Or use: https://www.mygeocachingprofile.com/codebreaker.vigenerecipher.aspx

# Python brute force (if you know key length)
from itertools import cycle
def vigenere_decrypt(ciphertext, key):
    key_cycle = cycle(key.upper())
    result = ""
    for char in ciphertext:
        if char.isalpha():
            shift = ord(next(key_cycle)) - ord('A')
            base = ord('A') if char.isupper() else ord('a')
            result += chr((ord(char) - base - shift) % 26 + base)
        else:
            result += char
    return result
```

---

## ⚡ XOR

XOR is everywhere in CTF crypto challenges!

```python
# XOR two equal-length byte strings
def xor_bytes(a, b):
    return bytes(x ^ y for x, y in zip(a, b))

# XOR with single key byte
def xor_single(data, key):
    return bytes(b ^ key for b in data)

# XOR with repeating key
def xor_key(data, key):
    return bytes(data[i] ^ key[i % len(key)] for i in range(len(data)))

# Brute force single-byte XOR
for key in range(256):
    result = xor_single(ciphertext, key)
    try:
        print(f"Key {key}: {result.decode()}")
    except:
        pass

# XOR tool (known plaintext attack)
# If you know part of the plaintext ("flag{"):
known = b"flag{"
partial_key = xor_bytes(ciphertext[:5], known)
print(f"Key starts with: {partial_key}")
```

---

## 🔏 RSA

RSA is the most common crypto challenge in CTFs. Key variables:

```
n = p × q          (public modulus)
e = public exponent (usually 65537)
d = private exponent
c = ciphertext
m = plaintext (the flag)

Encrypt: c = m^e mod n
Decrypt: m = c^d mod n
```

### Common RSA Attacks

```python
from sympy import integer_nthroot, factorint

# 1. Small e attack (e=3, m^e < n)
# If m^e < n, then c = m^e without modulo
# Just take the e-th root of c
m, is_perfect = integer_nthroot(c, e)
if is_perfect:
    flag = bytes.fromhex(hex(m)[2:]).decode()
    print(flag)

# 2. Factor n (small primes) — check factordb.com first!
# http://factordb.com/  ← paste n here

# 3. Compute d from p, q, e
from Crypto.Util.number import inverse
p = ...  # known prime factor
q = n // p
phi_n = (p-1) * (q-1)
d = inverse(e, phi_n)
m = pow(c, d, n)
flag = bytes.fromhex(hex(m)[2:]).decode()
print(flag)
```

### RsaCtfTool (Automated)

```bash
git clone https://github.com/RsaCtfTool/RsaCtfTool.git
cd RsaCtfTool && pip3 install -r requirements.txt

# With public key file:
python3 RsaCtfTool.py --publickey pub.pem --uncipher cipher.txt

# With n and e directly:
python3 RsaCtfTool.py -n 123456789... -e 65537 --uncipher hex_ciphertext
```

---

## #️⃣ Hash Cracking

```bash
# Identify hash type first
hash-identifier                           # Interactive tool
hashid -m "5f4dcc3b5aa765d61d8327deb882cf99"  # -m for hashcat mode

# Hash types by length:
# 32 chars → MD5
# 40 chars → SHA1
# 64 chars → SHA256
# 128 chars → SHA512

# Online cracking (try first!)
# https://crackstation.net
# https://md5decrypt.net

# John the Ripper
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
john --format=raw-md5 hash.txt
john --show hash.txt

# Hashcat
hashcat -m 0 hash.txt wordlist.txt     # MD5
hashcat -m 100 hash.txt wordlist.txt   # SHA1
hashcat -m 1400 hash.txt wordlist.txt  # SHA256
hashcat -m 1800 hash.txt wordlist.txt  # SHA512crypt

# Create hashes (for reference)
echo -n "password" | md5sum
echo -n "password" | sha1sum
echo -n "password" | sha256sum
```

---

## 🧪 Python Crypto Toolkit

```python
# Install: pip3 install pycryptodome gmpy2 sympy

# AES decryption
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
import base64

key = b'secretkey1234567'  # 16, 24, or 32 bytes
iv = b'initializationvc'   # 16 bytes
ciphertext = base64.b64decode("...")

cipher = AES.new(key, AES.MODE_CBC, iv)
plaintext = unpad(cipher.decrypt(ciphertext), AES.block_size)
print(plaintext.decode())

# All AES modes:
AES.MODE_ECB   # Electronic Codebook (vulnerable!)
AES.MODE_CBC   # Cipher Block Chaining (needs IV)
AES.MODE_CTR   # Counter mode
AES.MODE_GCM   # Galois/Counter Mode (authenticated)

# Integer operations for RSA
import gmpy2
# Check if n is a perfect square (p=q attack)
if gmpy2.is_square(n):
    p = gmpy2.isqrt(n)
    print(f"p = q = {p}")
```

---

## 🌐 Online Tools

| Tool | URL | Use Case |
|---|---|---|
| CyberChef | [gchq.github.io/CyberChef](https://gchq.github.io/CyberChef/) | Everything |
| dcode.fr | [dcode.fr](https://www.dcode.fr/) | Classical ciphers |
| CrackStation | [crackstation.net](https://crackstation.net/) | Hash cracking |
| FactorDB | [factordb.com](http://factordb.com/) | Factor large numbers |
| CryptoHack | [cryptohack.org](https://cryptohack.org/) | Practice crypto CTF |
| RSACTFTool | [GitHub](https://github.com/RsaCtfTool/RsaCtfTool) | RSA attacks |

---

<div align="center">

*C!p#3r Club IIST — Decode · Defend · Dominate* 🔐

[Back to README](../../README.md)

</div>
