# Secure VLAN-Based Enterprise Network Architecture

## Executive Summary
This project presents the design and implementation of a **secure enterprise campus network**
built using **architectural security principles** rather than ad-hoc controls.
The network enforces strong **Layer-2 and Layer-3 trust boundaries**, centralizes critical
infrastructure services, and limits the blast radius of common network attacks.

The design was implemented and validated using **Cisco Packet Tracer** and documented as a
professional lab report suitable for academic and portfolio use.

---

## Design Goals
- Segment users by function using VLANs
- Allocate IP space efficiently using VLSM
- Centralize infrastructure services in a protected zone
- Prevent rogue infrastructure services
- Limit broadcast and attack propagation
- Improve auditability and operational clarity

---

## Network Architecture Overview

### VLAN Segmentation
| VLAN | Purpose        |
|-----:|----------------|
| 30   | Research       |
| 40   | Faculty        |
| 50   | Students       |
| 200  | Infrastructure Services |
| 1000 | Management     |

- Access switches provide Layer-2 connectivity
- A Layer-3 distribution switch performs inter-VLAN routing
- Infrastructure services are isolated in a trusted VLAN

---

## IP Addressing Strategy
The departmental network uses the address block **10.50.0.0/21**, subnetted using
**Variable-Length Subnet Masking (VLSM)** to match host requirements.

| VLAN | Role     | Subnet | Purpose |
|-----:|----------|--------|--------|
| 50   | Students | /23    | High-density user access |
| 40   | Faculty  | /25    | Medium-density access |
| 30   | Research | /25    | Controlled research access |

This approach minimizes address waste while allowing future expansion.

---

## Security Architecture

### DHCP Security
- Centralized DHCP server located in **VLAN 200**
- DHCP relay (`ip helper-address`) configured on Layer-3 switch
- DHCP snooping enabled on all access switches
- Only infrastructure uplinks marked as trusted
- Rogue DHCP servers blocked automatically
- DHCP binding table enforced:
  - MAC ↔ IP ↔ VLAN ↔ Port

### DNS Security
- Single authorized internal DNS server
- DNS server isolated in Infrastructure VLAN
- DNS information distributed exclusively via DHCP
- No user VLAN permitted to host DNS services

### NTP Security
- Internal NTP server acts as authoritative time source
- Network devices synchronize only with internal NTP
- External time sources intentionally excluded

### ARP Containment
- ARP broadcasts confined to individual VLANs
- MAC address learning restricted to local broadcast domains
- ARP spoofing impact limited to a single VLAN

---

## Threat Model and Risk Reduction

| Threat | Mitigation |
|------|-----------|
| Rogue DHCP server | DHCP snooping + trusted ports |
| ARP spoofing | VLAN isolation limits blast radius |
| DNS poisoning | Single internal DNS authority |
| Lateral movement | VLAN segmentation |
| Broadcast storms | Reduced broadcast domains |

This architecture prioritizes **containment**, ensuring failures or attacks
do not propagate across departmental boundaries.

---

## Validation and Testing
- DHCP lease acquisition verified across VLANs
- DHCP snooping binding tables inspected
- DNS resolution validated from client devices
- NTP synchronization confirmed on distribution switch
- ARP and MAC tables analyzed at multiple layers
---

## Tools and Technologies
- Cisco Packet Tracer
- Cisco IOS (Layer-2 and Layer-3 switching)
- GitHub
- Microsoft Word / PDF

---

## Key Takeaways
This project demonstrates that **security emerges from architecture**.
By enforcing trust boundaries at the network design level, the environment
becomes easier to manage, monitor, and defend.

---

## Author
Oyeyemi Oyebode  
Graduate Student – Computer Science 

