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
---

## 🗂️ **Topology**

 <p align='center'> <img src=https://github.com/AbanoubAtef4/Secure-Network-Infrastructure-with-Cisco-Firepower-and-VPN-Integration/blob/main/Topology-ScreenShot.png></p>

---
## 🧩 **Topology Overview**

| Device | Role | Description |
|--------|------|--------------|
| **R1** | ZBF | Divides Private, Public, DMZ; class/policy maps |
| **R2** | Core Router | OSPF backbone |
| **R3** | IPS | IOS IPS + SPAN |
| **R4** | VPN Gateway | IPSec Site-to-Site with R6; connects to ASA |
| **R5** | DHCP, VLANs, RADIUS | VLAN IP pools, RADIUS Auth, Syslog |
| **R6** | VPN Peer | IPSec Site-to-Site peer |
| **R7** | IPv6 ACL | IPv6 filter + RBAC parser views |
| **R8** | ACLs | Anti-spoof, ICMP filter, port security |
| **R9** | SYSLOG, NTP, TACACS+ | Central admin/auth/log server, VLANs, DHCP Relay |
| **ASA** | NGFW | DMZ/Inside/Outside zones, NAT, ACLs, DHCP |
| **Switches** | L2 Security | 802.1X, Port Security, DAI, BPDU Guard |

---

## 🔑 **Subnetting Table**


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

### 🔹 **Router R8**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0 | 172.16.1.182/28 | 255.255.255.240 |  Link to inside LAN|
| S0/1/0 | 172.16.1.17/30 | 255.255.255.252 | Link to R6 |
| SW (Vlan 1) | 172.16.1.18 | 255.255.255.240 | Management VLAN |
| PC11 | 172.16.1.25 | 255.255.255.240 | PC in mode Protect  |
| PC22 | 172.16.1.98 | 255.255.255.248 | PC in mode Restrict  |
| PC33 | 172.16.1.98 | 255.255.255.248 | PC in mode Shutdown  |


---

### 🔹 **Router R9**

| Interface | IP Address | Mask | Purpose |
|-----------|--------|------|------------|
| G0/0.10 | 172.16.1.113/29 | 255.255.255.248 |  Link to inside LAN|
| G0/0.20 | 172.16.1.121/29 | 255.255.255.248 |  Link to inside LAN|
| G0/0.30 | 172.16.1.129/29 | 255.255.255.248 |  Link to inside LAN|
| G0/0.40 | 172.16.1.141/29 | 255.255.255.248 |  Link to inside LAN|
| G0/1 | 172.16.1.145/29 | 255.255.255.248 |  Link to inside LAN Server|
| S0/1/0 | 172.16.1.174/30 | 255.255.255.252 | Link to R4 |
| SW1 (Vlan 40) | 172.16.1.138 | 255.255.255.248 | Management VLAN |
| SW2 (Vlan 40) | 172.16.1.139 | 255.255.255.248 | Management VLAN |
| SW3 (Vlan 40) | 172.16.1.140 | 255.255.255.248 | Management VLAN |
| PC0 | 172.16.1.115 | 255.255.255.240 | PC in Vlan 10 |
| PC1 | 172.16.1.122 | 255.255.255.248 | PC in Vlan 20 |
| PC2 | 172.16.1.131 | 255.255.255.248 | PC in Vlan 30 |
| PC3 | 172.16.1.130 | 255.255.255.240 | PC in Vlan 10 |
| PC4 | 172.16.1.123 | 255.255.255.248 | PC in Vlan 20 |
| PC5 | 172.16.1.114 | 255.255.255.248 | PC in Vlan 30 |
| PC5 | 172.16.1.114 | 255.255.255.248 | NTP , Syslog , Tacacs+ |

---

## 🔐 **Key Configurations**

### ✅ R1 — Zone-Based Firewall

- **Zones:** `Private`, `Public`, `DMZ`
- **Class-Maps:** Match protocols:
  - Public → DMZ: HTTP, HTTPS, FTP, SMTP, POP3
  - Private → DMZ: HTTP, HTTPS, FTP, ICMP
  - Private → Public: TCP, UDP, ICMP
- **Policy-Maps:** Inspect matched traffic.
- **Zone-Pairs:** Apply policies for each zone pair.
- OSPF with MD5 authentication for secure routing.

---

### ✅ R2 — Core Router

- Basic hostname, privilege password, domain name.
- OSPF routing for LAN backbone.

---

### ✅ R3 — IPS

- IOS IPS with custom signature category.
- Enabled inline `deny-packet` for detected threats.
- Switch SPAN configured:
  - Source: multiple switch ports.
  - Destination: IPS listening interface.
- Syslog logging for IPS alerts.

---

### ✅ R4 & R6 — IPSec VPN

- **IKE Phase 1:**
  - AES-256, SHA, DH Group 5
- **IPSec Phase 2:**
  - AES-256, SHA-HMAC
- **Access-Lists:**
  - Defines interesting traffic for site-to-site tunnel.
- **Crypto Maps:**
  - Applied to WAN interfaces.
- Connected to ASA for internet edge protection.

---

### ✅ R5 — VLANs, DHCP, RADIUS

- Multiple DHCP pools for:
  - Wireless VLAN
  - Voice VLAN
  - Data VLANs
- Interfaces configured with dot1Q trunking.
- RADIUS server for 802.1X switch authentication.
- Syslog logs sent to R9.

---

### ✅ R7 — IPv6 ACL & Views

- IPv6 ACL allows ICMP from PC1 to Loopback but blocks PC2.
- Parser views:
  - `admin`: full show commands.
  - `user`: limited to `ping`, `traceroute`.

---

### ✅ R8 — ACL for Anti-Spoofing & Port Security

- Extended ACLs:
  - Blocks spoofed IPs (RFC1918, reserved, loopback).
  - Permits trusted ICMP replies & OSPF.
  - User-specific traffic rules for PCs.
- AAA with fallback to local auth.
- Syslog logging for ACL violations.

---

### ✅ R9 — SYSLOG, TACACS+, NTP, VLANs

- DHCP relay for VLANs.
- TACACS+ for device login.
- Central Syslog server for all logs.
- NTP master for time sync with authentication.
- Switches trust trunk ports for ARP inspection & DHCP Snooping.

---

## 🔒 Cisco ASA

### ✅ Interfaces

| Interface | Nameif | Sec-Level | IP | Purpose |
|-----------|--------|-----------|-----|---------|
| G1/1 | OUTSIDE | 0 | 200.1.1.1/25 | Public |
| G1/2 | DMZ | 50 | 192.168.20.1/24 | DMZ servers |
| G1/3 | INSIDE | 100 | 192.168.10.1/24 | Internal users |

---

### ✅ NAT

- Static NAT:
  - DMZ server `192.168.20.10` → `200.1.1.100`
- Dynamic NAT:
  - Inside subnet uses interface NAT for outbound.

---

### ✅ ACLs

- Allows:
  - HTTP, HTTPS, FTP, SMTP, POP3, IMAP, ICMP to DMZ server.
- Access-group `out-to-dmz` applied to OUTSIDE.

---

### ✅ Service Policies

- Global default inspection for:
  - DNS, FTP, HTTP, ICMP, TFTP

---

### ✅ DHCP on INSIDE

- Range: `192.168.10.20-192.168.10.250`
- Default gateway: `192.168.10.1`
- DNS: `8.8.8.8`

---

### ✅ AAA

- SSH allowed from:
  - `INSIDE`: `192.168.1.0/24`
  - `OUTSIDE`: Specific admin IPs.
- Local user: `abanob`

---

## ⚙️ Switch Security

- **802.1X**: Ports authenticate to RADIUS.
- **DHCP Snooping & DAI**: Trusted trunks only.
- **Port Security**: Sticky MACs, violation modes (`restrict`, `shutdown`).
- **BPDU Guard**: Prevent rogue switches.
- **SPAN**: For IPS traffic monitoring.

---


## 🔒 **Security Measures**

- **AutoSecure & manual hardening** for all devices
- Disabled unused ports, password minimums, SSH only
- Local authentication backup for AAA failure
- SYSLOG logging for auditing all security events

---

## 🚀 **Testing**

✔️ VPN Tunnel — verify IPSec SA  
✔️ IPS — verify blocked attacks  
✔️ ZBF — check allowed/blocked traffic  
✔️ IPv6 ACLs — ping tests for PC1 vs PC2  
✔️ SSH/TACACS+/RADIUS login  
✔️ NTP — confirm time sync on all devices  
✔️ Syslog — verify logs centralized

---


## 📣 **Contact**

**Author:** [Abanoub Atef Ebead]  
**Contact:** [https://www.linkedin.com/in/abanoub-atef-a01129251/]  
**License:** For training/demo purposes only

---

