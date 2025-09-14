# WiFi Handshake Capture & Crack (Simulated Lab)

**Status:** Completed — simulated / owned AP only  
**Tools used:** Airgeddon, Crunch, Aircrack-ng  
**Adapter:** Atheros USB (monitor + injection capable)  
**Host OS:** Kali Linux (or equivalent)

---

## Objective
Capture WPA2 handshake and recover WiFi password using ethical tools

---

## Important Disclaimer
This repository documents **ethical, lab-only** testing. Do **not** use these techniques on networks or devices you do not own or do not have explicit written permission to test. Do **not** commit `.cap` files or recovered credentials to a public repo.

---

## Prerequisites
- Kali Linux (or similar)
- Atheros USB wireless adapter (supports monitor mode & injection)
- Installed tools:
  - `airgeddon`
  - `crunch`
  - `aircrack-ng`

---

## Steps I performed 

### 1) Start Airgeddon and prepare capture
1. Launch Airgeddon:
  - 'sudo airgeddon'

2. Select your Atheros interface.
- Let Airgeddon put the adapter into monitor mode.
- Scan and select the lab/AP (owned AP).
- Start capturing and wait until Airgeddon shows Handshake captured for <SSID>
- Saved capture file

3. Use Airgeddon’s deauth option to force a client re-auth for handshake capture.

