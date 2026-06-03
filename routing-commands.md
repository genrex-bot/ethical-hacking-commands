# 🗺️ Routing Commands

> Routing commands help you understand how traffic flows across a network.  
> Essential for understanding network topology and finding paths to targets.

---

## 📍 `route` — View & Manage Routing Table

```bash
route                            # Show routing table (Linux)
route -n                         # Show routing table with numeric IPs
route add default gw <IP>        # Add default gateway
route add -net <network> gw <IP> # Add route to a network
route del default gw <IP>        # Delete default gateway
```

**Example:**
```bash
route -n
```
**Use:** See all routing rules your machine uses to forward packets.

---

## 🛤️ `ip route` — Modern Routing (Recommended)

```bash
ip route                         # Show routing table
ip route show                    # Same as above
ip route add <network> via <gateway>  # Add a route
ip route del <network>           # Delete a route
ip route get <IP>                # Show which route would be used for an IP
```

**Example:**
```bash
ip route show
ip route get 8.8.8.8
```
**Use:** The modern way to manage routes on Linux systems.

---

## 🔁 `traceroute` — Trace Path to Target

```bash
traceroute <target>              # Trace route to target (Linux)
tracert <target>                 # Trace route to target (Windows)
traceroute -n <target>           # Skip DNS resolution (faster)
traceroute -m 30 <target>        # Set max hops to 30
traceroute -p 80 <target>        # Use TCP port 80
traceroute -T <target>           # Use TCP SYN (better than ICMP)
traceroute -U <target>           # Use UDP packets
```

**Example:**
```bash
traceroute -n google.com
```
**Use:** See every hop (router) between you and a target — helps map network topology.

---

## 🌍 `mtr` — My Traceroute (Live Continuous Trace)

```bash
mtr <target>                     # Live interactive traceroute
mtr -n <target>                  # No DNS resolution
mtr --report <target>            # Generate a one-time report
mtr --report-cycles 10 <target>  # Run 10 cycles then report
```

**Example:**
```bash
mtr google.com
```
**Use:** Better than traceroute — gives live stats like packet loss at each hop.

---

## 🧭 `pathping` — Windows Route Analysis

```bash
pathping <target>                # Trace route + packet loss stats (Windows only)
pathping -n <target>             # Skip DNS resolution
pathping -h 30 <target>          # Max 30 hops
```

**Use:** Windows-only tool that combines ping and traceroute.

---

## 🔍 `netstat -r` — Routing Table via Netstat

```bash
netstat -r                       # Show routing table
netstat -rn                      # Show with numeric IPs
```

**Use:** Alternative way to view routing table, works on both Linux and Windows.

---

## 🔗 BGP & Advanced Routing Recon

```bash
# Check AS (Autonomous System) number for an IP
whois -h whois.cymru.com " -v <IP>"

# Query BGP routing info
curl https://api.bgpview.io/ip/<IP>

# Traceroute with AS information
traceroute -A <target>
```

**Use:** Understand which ISPs and organizations control the routing paths to a target.

---

## 📊 Quick Routing Recon Workflow

```bash
# 1. Check your current routes
ip route show

# 2. Find path to target
traceroute -n <target>

# 3. Live path analysis
mtr --report <target>

# 4. Check what gateway would be used
ip route get <target-IP>
```

---

## 🧪 Tools Summary

| Tool | Platform | Best For |
|------|----------|----------|
| `ip route` | Linux | Modern route management |
| `route` | Linux/Windows | Classic route table view |
| `traceroute` | Linux | Path tracing |
| `tracert` | Windows | Path tracing on Windows |
| `mtr` | Linux | Live continuous tracing |
| `pathping` | Windows | Combined trace + loss stats |

---

*← Back to [Main README](../README.md)*
