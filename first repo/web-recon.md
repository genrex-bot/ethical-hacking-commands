# 🌍 Web Reconnaissance

> Web recon is about gathering information on a web target before any attack.  
> The goal: understand the tech stack, find hidden paths, and map the attack surface.

---

## 🔎 `whatweb` — Identify Web Technologies

```bash
whatweb <url>                    # Basic tech fingerprint
whatweb -v <url>                 # Verbose output
whatweb -a 3 <url>               # Aggression level 3 (more thorough)
whatweb --log-verbose=out.txt <url>  # Save output to file
```

**Example:**
```bash
whatweb https://example.com
```
**Use:** Find CMS, frameworks, server software, JavaScript libraries used by a site.

---

## 📂 `gobuster` / `dirb` — Directory & File Brute Force

### Gobuster
```bash
gobuster dir -u <url> -w /usr/share/wordlists/dirb/common.txt  # Basic dir scan
gobuster dir -u <url> -w <wordlist> -x php,html,txt            # With file extensions
gobuster dir -u <url> -w <wordlist> -t 50                       # 50 threads (faster)
gobuster dns -d <domain> -w <wordlist>                          # DNS subdomain brute force
gobuster vhost -u <url> -w <wordlist>                           # Virtual host discovery
```

### Dirb
```bash
dirb <url>                                     # Basic scan with default wordlist
dirb <url> /usr/share/wordlists/dirb/big.txt   # Use a bigger wordlist
dirb <url> -X .php,.html                       # Scan for specific extensions
```

**Use:** Find hidden directories and files on web servers.

---

## 🕵️ `nikto` — Web Vulnerability Scanner

```bash
nikto -h <url>                   # Basic scan
nikto -h <url> -p 8080           # Scan on non-standard port
nikto -h <url> -o output.html -Format htm  # Save HTML report
nikto -h <url> -ssl              # Force SSL
```

**Example:**
```bash
nikto -h https://example.com
```
**Use:** Automatically check for common web vulnerabilities, misconfigurations, and outdated software.

---

## 🌐 `subfinder` / `amass` — Subdomain Enumeration

### Subfinder
```bash
subfinder -d <domain>            # Find subdomains
subfinder -d <domain> -o out.txt # Save to file
subfinder -d <domain> -v         # Verbose
```

### Amass
```bash
amass enum -d <domain>           # Enumerate subdomains
amass enum -passive -d <domain>  # Passive only (no direct contact)
amass intel -d <domain>          # Gather intel on organization
```

**Use:** Discover all subdomains of a target domain — more attack surface!

---

## 🔬 `curl` — Manual HTTP Requests

```bash
curl -I <url>                    # Get HTTP headers only
curl -v <url>                    # Verbose — see full request & response
curl -L <url>                    # Follow redirects
curl -b "cookie=value" <url>     # Send cookies
curl -H "X-Custom: header" <url> # Add custom header
curl -X POST -d "user=a&pass=b" <url>  # POST request with data
curl -u admin:password <url>     # Basic authentication
curl --proxy http://127.0.0.1:8080 <url>  # Route through proxy (e.g. Burp)
```

**Use:** Manually test HTTP responses, headers, and authentication.

---

## 🕸️ `burpsuite` — Web Proxy (Essential Tool)

```bash
# Launch via terminal
burpsuite

# Common uses:
# - Intercept and modify HTTP requests
# - Scan for vulnerabilities
# - Fuzz parameters
# - Replay and modify requests
```

**Use:** The #1 tool for manual web application testing. Intercept and manipulate traffic between your browser and the web server.

---

## 🔑 `wfuzz` — Web Fuzzer

```bash
wfuzz -c -w <wordlist> <url>/FUZZ           # Fuzz a URL path
wfuzz -c -w <wordlist> -d "param=FUZZ" <url> # Fuzz POST parameter
wfuzz -c -w <wordlist> -H "Cookie: session=FUZZ" <url>  # Fuzz cookie
wfuzz --hc 404 -w <wordlist> <url>/FUZZ     # Hide 404 responses
```

**Use:** Test for parameters, paths, or values that trigger different responses.

---

## 🗺️ `wafw00f` — WAF Detection

```bash
wafw00f <url>                    # Detect if a WAF is present
wafw00f -a <url>                 # Try all WAF fingerprints
wafw00f -l                       # List all supported WAFs
```

**Use:** Detect if a Web Application Firewall (WAF) is blocking your requests.

---

## 📋 Web Recon Workflow

```bash
# 1. Identify technologies
whatweb https://target.com

# 2. Find subdomains
subfinder -d target.com

# 3. Brute force directories
gobuster dir -u https://target.com -w /usr/share/wordlists/dirb/common.txt -x php,html

# 4. Auto vulnerability scan
nikto -h https://target.com

# 5. Check for WAF
wafw00f https://target.com

# 6. Manual testing with Burp Suite
# (Launch Burp and configure browser proxy to 127.0.0.1:8080)
```

---

*← Back to [Main README](../README.md)*
