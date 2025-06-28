# Secure-Network-Infrastructure-with-Cisco-Firepower-and-VPN-Integration

# 🛡️ Secure Network Infrastructure with Cisco Firepower and VPN Integration

Designed and implemented a secure enterprise network using Cisco Firepower NGFW and ASA firewalls. Configured an Intrusion Prevention System (IPS) to detect and block malicious traffic in real time. Deployed secure IPsec VPNs for encrypted remote access. Applied AAA protocols (RADIUS/TACACS+) for robust access control and policy enforcement. Implemented OSPF with authentication to secure routing updates and ensure network segmentation. Integrated Layer 2 security features to prevent VLAN hopping and MAC spoofing attacks.

---

## 📌 **Project Highlights**

- **Zone-Based Firewall** for granular traffic control (inside/outside/DMZ)
- **Intrusion Prevention System** with SPAN mirroring to analyze traffic
- **IPv6 ACLs** to selectively control reachability
- **Site-to-Site IPSec VPN** for branch connectivity
- **Port Security** with multiple violation modes
- **802.1x Wireless Security** with RADIUS authentication
- **Centralized Services** (NTP, SYSLOG, TACACS+)
- **L2 Security**: DHCP Snooping, Dynamic ARP Inspection, STP hardening

---

## 🧩 **Topology Overview**

- **R1**: ZBF, DMZ services (Mail/Web)
- **R3**: IPS sensor, packet analysis
- **R4 & R6**: IPSec VPN endpoints
- **R5**: Wireless LAN, VoIP VLAN, RADIUS/SYSLOG server
- **R7**: IPv6 Loopback and LANs, IPv6 ACL
- **R8**: Port security, ACLs, spoofing prevention
- **R9**: Core services (NTP, SYSLOG, TACACS+), DHCP, VLAN Mgmt

---

## 🔑 **Subnetting Table**

| # | Hosts | Network | Slash | Mask | Usable Range | Broadcast | Router |
|----|-------|---------|-------|------|---------------|-----------|--------|
| 1 | 14 | 172.16.1.0 | /28 | 255.255.255.240 | 172.16.1.1–14 | 172.16.1.15 | R3 |
| 2 | 14 | 172.16.1.16 | /28 | 255.255.255.240 | 172.16.1.17–30 | 172.16.1.31 | R8 |
| 3 | 14 | 172.16.1.32 | /28 | 255.255.255.240 | 172.16.1.33–46 | 172.16.1.47 | R1-DMZ |
| 4 | 14 | 172.16.1.48 | /28 | 255.255.255.240 | 172.16.1.49–62 | 172.16.1.63 | R1-ZPF |
| 5 | 14 | 172.16.1.64 | /28 | 255.255.255.240 | 172.16.1.65–78 | 172.16.1.79 | R5 Wireless |
| 6 | 6 | 172.16.1.80 | /29 | 255.255.255.248 | 172.16.1.81–86 | 172.16.1.87 | R5 VLAN 10 |
| 7 | 6 | 172.16.1.88 | /29 | 255.255.255.248 | 172.16.1.89–94 | 172.16.1.95 | R5 VLAN 20 |
| 8 | 6 | 172.16.1.96 | /29 | 255.255.255.248 | 172.16.1.97–102 | 172.16.1.103 | R5 VLAN 30 |
| 9 | 6 | 172.16.1.104 | /29 | 255.255.255.248 | 172.16.1.105–110 | 172.16.1.111 | R5 VLAN 40 (Mgmt) |
| 10 | 6 | 172.16.1.112 | /29 | 255.255.255.248 | 172.16.1.113–118 | 172.16.1.119 | R9 VLAN 50 |
| 11 | 6 | 172.16.1.120 | /29 | 255.255.255.248 | 172.16.1.121–126 | 172.16.1.127 | R9 VLAN 60 |
| 12 | 6 | 172.16.1.128 | /29 | 255.255.255.248 | 172.16.1.129–134 | 172.16.1.135 | R9 VLAN 70 |
| 13 | 6 | 172.16.1.136 | /29 | 255.255.255.248 | 172.16.1.137–142 | 172.16.1.143 | R9 VLAN 80 (Mgmt) |
| 14 | 6 | 172.16.1.144 | /29 | 255.255.255.248 | 172.16.1.145–150 | 172.16.1.151 | R9 Server |
| 15 | 2 | 172.16.1.152 | /30 | 255.255.255.252 | 172.16.1.153–154 | 172.16.1.155 | R1–R2 |
| 16 | 2 | 172.16.1.156 | /30 | 255.255.255.252 | 172.16.1.157–158 | 172.16.1.159 | R2–R3 |
| 17 | 2 | 172.16.1.160 | /30 | 255.255.255.252 | 172.16.1.161–162 | 172.16.1.163 | R2–R4 |
| 18 | 2 | 172.16.1.164 | /30 | 255.255.255.252 | 172.16.1.165–166 | 172.16.1.167 | R2–R5 |
| 19 | 2 | 172.16.1.168 | /30 | 255.255.255.252 | 172.16.1.169–170 | 172.16.1.171 | R2–R6 |
| 20 | 2 | 172.16.1.172 | /30 | 255.255.255.252 | 172.16.1.173–174 | 172.16.1.175 | R4–R9 |
| 21 | 2 | 172.16.1.176 | /30 | 255.255.255.252 | 172.16.1.177–178 | 172.16.1.179 | R6–R7 |
| 22 | 2 | 172.16.1.180 | /30 | 255.255.255.252 | 172.16.1.181–182 | 172.16.1.183 | R6–R8 |

---

## 🔒 **Security Measures**

- **AutoSecure & manual hardening** for all devices
- Disabled unused ports, password minimums, SSH only
- Local authentication backup for AAA failure
- SYSLOG logging for auditing all security events

---

## ✔️ **Status**

✅ Fully tested & documented  
🔗 Ready for deployment  
🗂️ Version controlled with Git

---

## 📣 **Contact**

**Author:** [Your Name]  
**Contact:** [Your Email or LinkedIn]  
**License:** For training/demo purposes only

---

