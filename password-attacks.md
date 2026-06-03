# 🔑 Password Attacks

> ⚠️ **Only use these on systems you own or have written permission to test.**  
> Password cracking is a key skill in ethical hacking — used to test password policies.

---

## 🔨 `hydra` — Online Password Brute Force

```bash
# SSH brute force
hydra -l <username> -P <wordlist> ssh://<target>
hydra -L <userlist> -P <wordlist> ssh://<target>

# FTP brute force
hydra -l <username> -P <wordlist> ftp://<target>

# HTTP login form
hydra -l admin -P <wordlist> <target> http-post-form "/login:username=^USER^&password=^PASS^:Invalid password"

# RDP brute force
hydra -l administrator -P <wordlist> rdp://<target>

# Telnet
hydra -l <username> -P <wordlist> telnet://<target>

# Common options
hydra -t 4 ...                   # 4 threads (be gentle on targets)
hydra -V ...                     # Verbose — show each attempt
hydra -f ...                     # Stop after first valid login found
```

**Example:**
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.10
```

---

## 🔐 `john` (John the Ripper) — Offline Password Cracking

```bash
# Basic crack
john <hashfile>

# Specify wordlist
john --wordlist=/usr/share/wordlists/rockyou.txt <hashfile>

# Crack with rules (variations of words)
john --wordlist=<wordlist> --rules <hashfile>

# Show cracked passwords
john --show <hashfile>

# Identify hash format
john --list=formats | grep -i <hash-type>

# Crack specific format
john --format=md5 --wordlist=<wordlist> <hashfile>

# Crack /etc/shadow file (requires both passwd + shadow)
unshadow /etc/passwd /etc/shadow > hashes.txt
john hashes.txt
```

---

## 💣 `hashcat` — GPU-Accelerated Password Cracking

```bash
# Basic dictionary attack (attack mode 0)
hashcat -m <hash-type> <hashfile> <wordlist>

# With rules
hashcat -m 0 <hashfile> <wordlist> -r /usr/share/hashcat/rules/best64.rule

# Brute force (attack mode 3)
hashcat -m 0 -a 3 <hashfile> ?a?a?a?a?a?a     # All chars, 6 chars long

# Common hash types (-m values)
# -m 0     MD5
# -m 100   SHA1
# -m 1400  SHA256
# -m 1800  SHA512crypt (Linux $6$)
# -m 1000  NTLM (Windows)
# -m 3200  bcrypt

# Show cracked passwords
hashcat -m 0 <hashfile> --show

# List rules
ls /usr/share/hashcat/rules/
```

**Example:**
```bash
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
```

---

## 📋 `crunch` — Generate Custom Wordlists

```bash
crunch <min> <max> <charset>                 # Generate wordlist
crunch 6 8 abc123                            # 6-8 char words from 'abc123'
crunch 4 4 0123456789 -o pins.txt            # All 4-digit PINs
crunch 8 8 -t @@@@@@@@ -o wordlist.txt       # 8 lowercase letters
crunch 6 6 -t Pass@@                         # Words starting with 'Pass' + 2 lowercase
```

**Charset shortcuts:**
```
@ = lowercase letters (a-z)
, = uppercase letters (A-Z)
% = digits (0-9)
^ = symbols (!@#$...)
```

---

## 🔤 Common Wordlists (Pre-installed on Kali)

```bash
# Most famous wordlist
/usr/share/wordlists/rockyou.txt

# Directory/file lists for web
/usr/share/wordlists/dirb/common.txt
/usr/share/wordlists/dirb/big.txt

# Dirbuster lists
/usr/share/wordlists/dirbuster/

# Seclists (install separately — highly recommended)
sudo apt install seclists
ls /usr/share/seclists/
```

---

## 🔓 Hash Identification

```bash
# Identify hash type
hash-identifier
hashid <hash>

# Online tools
# https://hashes.com/en/decrypt/hash
# https://crackstation.net
```

---

## 📊 Common Hash Examples

| Hash | Length | Example |
|------|--------|---------|
| MD5 | 32 chars | `5f4dcc3b5aa765d61d8327deb882cf99` |
| SHA1 | 40 chars | `5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8` |
| SHA256 | 64 chars | `...` |
| bcrypt | Starts with `$2a$` | `$2a$12$...` |
| NTLM | 32 chars | `8846f7eaee8fb117ad06bdd830b7586c` |
| Linux SHA512 | Starts with `$6$` | `$6$salt$...` |

---

## 📋 Password Attack Workflow

```bash
# 1. Get the hash (from a compromised database, /etc/shadow, etc.)

# 2. Identify the hash type
hashid <hash>

# 3. Try online crackers first (fast, free)
# https://crackstation.net

# 4. Try John with rockyou
john --wordlist=/usr/share/wordlists/rockyou.txt --format=<type> hash.txt

# 5. If that fails, use hashcat with rules
hashcat -m <type> hash.txt /usr/share/wordlists/rockyou.txt -r best64.rule

# 6. For online services, use hydra
hydra -l admin -P /usr/share/wordlists/rockyou.txt <service>://<target>
```

---

*← Back to [Main README](../README.md)*
