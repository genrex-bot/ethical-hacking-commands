# 🔍 Scanning & Mapping — Nmap

> **Nmap** (Network Mapper) is the most popular and powerful scanning tool used by ethical hackers.  
> It helps you discover hosts, open ports, running services, and even OS versions.

---

## ⚡ Basic Scans

```bash
nmap <target>                    # Basic scan (top 1000 ports)
nmap <IP range>                  # Scan a range e.g. 192.168.1.1-50
nmap <subnet>                    # Scan entire subnet e.g. 192.168.1.0/24
nmap -iL targets.txt             # Scan from a list of targets in a file
```

**Example:**
```bash
nmap 192.168.1.1
nmap 192.168.1.0/24
```

---

## 🔎 Port Scanning Options

```bash
nmap -p 80 <target>              # Scan only port 80
nmap -p 80,443,22 <target>       # Scan specific ports
nmap -p 1-1000 <target>          # Scan ports 1 to 1000
nmap -p- <target>                # Scan ALL 65535 ports
nmap --top-ports 100 <target>    # Scan top 100 most common ports
```

**Use:** Focus on specific services or do a full port sweep.

---

## 🚀 Scan Speed (Timing Templates)

```bash
nmap -T0 <target>                # Paranoid — very slow, avoids detection
nmap -T1 <target>                # Sneaky — slow
nmap -T2 <target>                # Polite — slower than default
nmap -T3 <target>                # Normal — default speed
nmap -T4 <target>                # Aggressive — faster (recommended for labs)
nmap -T5 <target>                # Insane — fastest, may miss results
```

**Tip:** Use `-T4` for CTFs and labs. Use `-T1` or `-T2` when trying to be stealthy in authorized pen tests.

---

## 🧩 Service & Version Detection

```bash
nmap -sV <target>                # Detect service versions
nmap -sV --version-intensity 9 <target>  # Maximum version detection
```

**Example:**
```bash
nmap -sV 192.168.1.1
```
**Use:** Find out what software/version is running on each port — useful for finding vulnerable services.

---

## 💻 OS Detection

```bash
nmap -O <target>                 # Detect operating system
nmap -O --osscan-guess <target>  # Guess OS even if uncertain
```

**Example:**
```bash
sudo nmap -O 192.168.1.1
```
**Note:** Requires root/sudo privileges.

---

## 🧠 Aggressive Scan (All-in-One)

```bash
nmap -A <target>                 # OS detection + version + scripts + traceroute
```

**Example:**
```bash
nmap -A 192.168.1.1
```
**Use:** Get maximum information from a single scan — great for beginners in lab environments.

---

## 📡 Scan Types

```bash
nmap -sS <target>                # SYN scan (Stealth scan) — most common
nmap -sT <target>                # TCP connect scan — no root needed
nmap -sU <target>                # UDP scan — slower but important
nmap -sN <target>                # Null scan — no flags set
nmap -sF <target>                # FIN scan
nmap -sX <target>                # Xmas scan (FIN+PSH+URG flags)
```

**Tip:** `-sS` is default for root users and is stealthier than `-sT`.

---

## 📜 NSE Scripts (Nmap Scripting Engine)

```bash
nmap --script=default <target>           # Run default scripts
nmap --script=vuln <target>              # Check for known vulnerabilities
nmap --script=http-title <target>        # Get webpage titles on web ports
nmap --script=smb-vuln-ms17-010 <target> # Check for EternalBlue vulnerability
nmap --script=ftp-anon <target>          # Check for anonymous FTP login
nmap --script=ssh-brute <target>         # Brute force SSH (lab only)
```

**Example:**
```bash
nmap --script=vuln 192.168.1.1
```
**Use:** Automated vulnerability checks right from nmap.

---

## 💾 Save Output

```bash
nmap -oN output.txt <target>     # Save as normal text
nmap -oX output.xml <target>     # Save as XML
nmap -oG output.gnmap <target>   # Save as grepable format
nmap -oA output <target>         # Save in all 3 formats at once
```

**Use:** Always save your scan results when doing a pen test for reporting.

---

## 🔒 Firewall Evasion (Advanced)

```bash
nmap -f <target>                 # Fragment packets
nmap -D RND:10 <target>          # Use 10 random decoy IPs
nmap --source-port 53 <target>   # Spoof source port as DNS (port 53)
nmap -sI <zombie> <target>       # Idle/zombie scan
```

**Note:** Only use these techniques on networks you are authorized to test.

---

## 📋 Common Combos

```bash
# Quick full recon on a single host
nmap -A -T4 -p- <target>

# Stealthy scan with service detection
sudo nmap -sS -sV -T2 <target>

# Vulnerability scan
nmap --script=vuln -sV <target>

# Full subnet discovery + service detection
nmap -sV -T4 192.168.1.0/24
```

---

*← Back to [Main README](../README.md)*
