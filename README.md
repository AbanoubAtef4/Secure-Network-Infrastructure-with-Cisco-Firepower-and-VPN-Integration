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
### 🔹 **Router R1**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|----------|
| G0/0 | 172.16.1.49/28 | 255.255.255.240 |  Link to inside ZPF |
| G0/1 | 172.16.1.33/28 | 255.255.255.240 |  Link to  DMZ|
| S0/1/0 | 172.16.1.154/30 | 255.255.255.252 | Link to R2 ( outside ZPF ) |
| inside Server | 172.16.1.56 | 255.255.255.240 | Web , Email |
| Pc7 | 172.16.1.55 | 255.255.255.240 | For inside Users  |
| DMZ Server | 172.16.1.36 | 255.255.255.240 | Web , mail , FTP  |
| Pc6 | 172.16.1.35 | 255.255.255.240 | Pc For Management  |

---

### 🔹 **Router R2**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|-------------|
| G0/0 | 172.16.1.157/30 | 255.255.255.252 |  Link to R3 |
| S0/1/0 | 172.16.1.161/30 | 255.255.255.252 | Link to R4 |
| S0/1/1 | 172.16.1.169/30 | 255.255.255.252 | Link to R6 |
| S0/2/0 | 172.16.1.163/30 | 255.255.255.252 | Link to R5 |
| S0/2/1 | 172.16.1.153/30 | 255.255.255.252 | Link to R1 |


---

### 🔹 **Router R3**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0 | 172.16.1.158/30 | 255.255.255.252 |  Link to R2 |
| G0/1 | 172.16.1.1/28 | 255.255.255.240 |  Link to inside LAN |
| SW (Vlan1) | 172.16.1.12 | 255.255.255.240 | Management Vlan |
| Server | 172.16.1.6 | 255.255.255.240 | Web , mail , Syslog|
| Pc8 | 172.16.1.5 | 255.255.255.240 | For Users  |
| Pc0 | 172.16.1.10 | 255.255.255.240 | For Management  |

---

### 🔹 **Router R4**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0 | 200.1.1.4/25 | 255.255.255.128 |  Link to ASA |
| S0/1/0 | 172.16.1.173/30 | 255.255.255.252 | Link to R9 |
| S0/1/1 | 172.16.1.162/30 | 255.255.255.252 | Link to R2 |
| ASA-G1/1 | 200.1.1.1/25 | 255.255.255.128 | Link to R4 |
| ASA-G1/2 | 192.168.20.1/24 | 255.255.255.0 | Link to ASA-DMZ |
| ASA-G1/3 | 192.168.10.1/24 | 255.255.255.0 | Link to ASA Inside |
| DMZ-Server | 192.168.20.10 | 255.255.255.0 | Web, mail, FTP |
| Pc5 | 192.168.20.22 | 255.255.255.0 | Management Pc in ASA-DMZ |
| PC4 | 192.168.10.21 | 255.255.255.0 | User1 in ASA-Inside |
| PC3 | 192.168.10.20 | 255.255.255.0 | User2 in ASA-Inside |


---

### 🔹 **Router R5**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0.10 | 172.16.1.65/29 | 255.255.255.248 |  Link to inside VLAN 10 |
| G0/0.20 | 172.16.1.81/29 | 255.255.255.248 |  Link to inside VLAN 20 |
| G0/0.30 | 172.16.1.89/29 | 255.255.255.248 |  Link to inside VLAN 30 |
| G0/0.40 | 172.16.1.105/29 | 255.255.255.248 |  Link to inside VLAN 40 |
| G0/0.50 | 172.16.1.97/29 | 255.255.255.248 |  Link to inside VLAN 50 |
| S0/1/1 | 172.16.1.166/30 | 255.255.255.252 | Link to R2 |
| SW (Vlan 40) | 172.16.1.106/29 | 255.255.255.248 | Management VLAN |
| PC9 | 172.16.1.99 | 255.255.255.248 | PC in Vlan 50 |
| Server | 172.16.1.91 | 255.255.255.248 | Web, mail, FTP, Syslog, Radius in Vlan 30 |
| PC10 | 172.16.1.98 | 255.255.255.248 | PC in Voice Vlan 20 |
| Wireless Router | 172.16.1.66 | 255.255.255.248 | Link to Switch Vlan 10 |
| Wireless LAN | 192.168.1.1 | 255.255.255.0 | gateway for wireless lan |
| laptop | 192.168.1.2 | 255.255.255.0 | gateway for wireless lan |
| tablet | 192.168.1.4 | 255.255.255.0 | gateway for wireless lan |




---

### 🔹 **Router R1**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0 | 172.16.1.154/30 | 255.255.255.252 |  Link to inside |
| S0/1/1 | 172.16.1.166/30 | 255.255.255.252 | Link to R2 |


---

### 🔹 **Router R6**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| S0/1/0 | 172.16.1.170/30 | 255.255.255.252 | Link to R2 |
| S0/1/1 | 172.16.1.181/30 | 255.255.255.252 | Link to R8 |
| S0/2/0 | 172.16.1.177/30 | 255.255.255.252 | Link to R7 |


---

### 🔹 **Router R7**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0 | 2001:1::1 | /64 |  Link to inside LAN1 |
| G0/1 | 2001:2::1 | /64 |  Link to inside LAN2 |
| loopback1 | 2001:3::1 | /64 |  loopback interface |
| S0/1/0 | 172.16.1.178/30 | 255.255.255.252 | Link to R2 |
| PC12 | 2001:1::2 | /64 |  PC in LAN1 |
| PC13 | 2001:2::2 | /64 |  PC in LAN2 |


---

### 🔹 **Router R1**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0 | 172.16.1.154/30 | 255.255.255.252 |  Link to inside |
| G0/1 | 172.16.1.154/30 | 255.255.255.252 |  Link to outside |
| S0/1/0 | 172.16.1.154/30 | 255.255.255.252 | Link to R2 |


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

**Author:** [Abanoub Atef Ebead]  
**Contact:** [https://www.linkedin.com/in/abanoub-atef-a01129251/]  
**License:** For training/demo purposes only

---

