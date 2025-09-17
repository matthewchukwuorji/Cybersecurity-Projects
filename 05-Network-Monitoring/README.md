# Network Monitoring & Attack Detection Lab

## Overview
This project demonstrates how to monitor and detect common network-based attacks using **Wireshark** in a controlled lab environment.  
Attacks simulated:
- **Port Scan (SYN scan)**
- **ARP Spoofing (MITM)**
- **DoS-like traffic (ICMP/SYN flood)**

## Lab Setup
- **Kali Linux (192.168.100.56)** → Attacker
- **Parrot Security (192.168.100.57)** → Monitor (Wireshark)
- **Metasploitable2 (192.168.100.50)** → Victim
- **pfSense (192.168.100.10)** → Gateway/firewall

## Commands Used
```bash
# Port Scan
sudo nmap -sS -Pn -p1-500 192.168.100.50

# ARP Spoofing
sudo sysctl -w net.ipv4.ip_forward=1
sudo arpspoof -i eth0 -t 192.168.100.50 192.168.100.10
sudo arpspoof -i eth0 -t 192.168.100.10 192.168.100.50

# DoS-like (ICMP flood)
sudo hping3 --icmp --count 1000 192.168.100.50
