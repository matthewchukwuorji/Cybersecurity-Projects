# Network Monitoring & Attack Detection Lab

Simple lab using Wireshark to capture/identify:
- Port scan (SYN) from Kali -> Metasploitable2
- ARP spoofing (MITM) via arpspoof
- DoS-like burst (ICMP/SYN) via hping3

Topology (IPs):
- Kali (attacker): 192.168.100.56
- Metasploitable2 (victim): 192.168.100.50
- Parrot (monitor): 192.168.100.57
- pfSense (gateway): 192.168.100.10

Key Wireshark filters:
- SYN scan: `tcp.flags.syn==1 && tcp.flags.ack==0 && ip.dst==192.168.100.50`
- ARP: `arp`
- ICMP flood: `icmp && ip.dst==192.168.100.50`

Commands Used in Kali
# Port scan (Kali)
sudo nmap -sS -Pn -p1-500 192.168.100.50

# ARP spoofing (Kali) — enable forwarding first
sudo sysctl -w net.ipv4.ip_forward=1
sudo arpspoof -i <KALI_IFACE> -t 192.168.100.50 192.168.100.10
sudo arpspoof -i <KALI_IFACE> -t 192.168.100.10 192.168.100.50

# Controlled ICMP burst (Kali)
sudo hping3 --icmp --count 1000 192.168.100.50


