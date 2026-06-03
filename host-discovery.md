# 🖥️ Host Discovery

> Before hacking anything, you need to know what's alive on the network.  
> Host discovery is the first step — finding which IP addresses have active machines.

---

## 🏓 Ping Sweep (Find Live Hosts)

### Using Nmap
```bash
nmap -sn 192.168.1.0/24          # Ping sweep — no port scan, just find live hosts
nmap -sn 192.168.1.1-50          # Scan a specific range
nmap -sP 192.168.1.0/24          # Older flag (same as -sn)
```

**Example:**
```bash
nmap -sn 192.168.1.0/24
```
**Use:** Quickly find all live hosts on a subnet without triggering port scan alerts.

---

## 📡 ARP Scan (Best for Local Network)

### `arp-scan`
```bash
sudo arp-scan --localnet          # Scan local network using ARP
sudo arp-scan -I eth0 --localnet  # Specify network interface
sudo arp-scan 192.168.1.0/24      # Scan specific subnet
```

**Use:** ARP scans are extremely reliable on local networks — can't be blocked by firewalls.

### `netdiscover`
```bash
sudo netdiscover                  # Auto-discover live hosts on local network
sudo netdiscover -r 192.168.1.0/24  # Scan specific range
sudo netdiscover -i eth0          # Use specific interface
sudo netdiscover -p               # Passive mode — just listen, don't send
```

**Use:** Great for stealthy passive host discovery.

---

## 🌐 ICMP-Based Discovery

```bash
# Nmap ICMP Echo (ping)
nmap -PE -sn 192.168.1.0/24

# Nmap ICMP Timestamp
nmap -PP -sn 192.168.1.0/24

# Nmap ICMP Netmask
nmap -PM -sn 192.168.1.0/24
```

**Use:** Different ICMP types help bypass firewalls that block standard pings.

---

## 🔌 TCP/UDP Discovery

```bash
# TCP SYN ping on port 80
nmap -PS80 -sn 192.168.1.0/24

# TCP SYN ping on common ports
nmap -PS22,80,443 -sn 192.168.1.0/24

# TCP ACK ping
nmap -PA80 -sn 192.168.1.0/24

# UDP ping
nmap -PU53 -sn 192.168.1.0/24
```

**Use:** When ICMP is blocked, use TCP/UDP pings to discover hosts.

---

## 🛠️ `fping` — Fast Parallel Pinging

```bash
fping -a -g 192.168.1.0/24       # Ping entire subnet, show only alive hosts
fping -a -g 192.168.1.1 192.168.1.50  # Ping a range
fping -c 1 192.168.1.1            # Send 1 ping to a host
```

**Use:** Much faster than regular `ping` for scanning large ranges.

---

## 🔍 `masscan` — Ultra-Fast Port/Host Scanner

```bash
sudo masscan -p80 192.168.1.0/24              # Find hosts with port 80 open
sudo masscan -p1-65535 192.168.1.0/24         # Full port scan
sudo masscan -p80 10.0.0.0/8 --rate=1000      # Scan with rate limit
```

**Note:** Masscan is extremely fast — be careful not to overwhelm networks. Always use in authorized environments.

---

## 🧲 `hping3` — Advanced Packet Crafting

```bash
hping3 -1 <target>               # ICMP ping
hping3 -S -p 80 <target>         # TCP SYN to port 80
hping3 -A -p 80 <target>         # TCP ACK to port 80
hping3 --udp -p 53 <target>      # UDP ping on port 53
hping3 -S --scan 1-1000 <target> # TCP SYN scan ports 1-1000
```

**Use:** Custom packet crafting for advanced recon and firewall testing.

---

## 📋 Common Host Discovery Workflow

```bash
# Step 1: Quick ping sweep to find live hosts
nmap -sn 192.168.1.0/24

# Step 2: ARP scan for local network confirmation
sudo arp-scan --localnet

# Step 3: Once you find a live host, do a full scan
nmap -A -T4 <live-host-IP>
```

---

## 🧪 Tools Summary

| Tool | Best For | Speed |
|------|----------|-------|
| `nmap -sn` | General purpose | Medium |
| `arp-scan` | Local network | Fast |
| `netdiscover` | Passive discovery | Slow (passive) |
| `fping` | Large ranges | Fast |
| `masscan` | Internet-scale | Very Fast |
| `hping3` | Custom packets | Medium |

---

*← Back to [Main README](../README.md)*
