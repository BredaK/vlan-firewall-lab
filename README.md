# Enterprise VLAN Segmentation & ACL Firewall Lab

## Project Overview
A simulated enterprise network built in Cisco Packet Tracer demonstrating VLAN segmentation, inter-VLAN routing, and firewall policy enforcement using Cisco Extended ACLs.

This project mirrors real-world enterprise network design principles including network segmentation, least-privilege access control, and documented security policy.

---

## Skills Demonstrated
- VLAN design and configuration (IEEE 802.1Q)
- Trunk link configuration between switch and router
- Router-on-a-stick inter-VLAN routing
- Cisco Extended ACL firewall policy
- IP addressing and subnetting
- Network documentation and topology design

---

## Tools Used
| Tool | Purpose |
|---|---|
| Cisco Packet Tracer 9.0 | Network simulation |
| draw.io | Topology documentation |
| GitHub | Portfolio publishing |

---

## Network Topology
```
        [Internet Cloud]
               |
          [GW-Router]  ← Cisco 2911
          WAN: 203.0.113.1
          LAN subinterfaces per VLAN
               |
          [Core-SW01]  ← Cisco 2960
         /    |    |    \
     VLAN   VLAN  VLAN  VLAN
      10     20    30    40
       |      |     |     |
   PC-MGMT PC-STAFF PC-SRV PC-GUEST
```

---

## VLAN Design

| VLAN | Name | Subnet | Gateway | Purpose |
|---|---|---|---|---|
| 10 | Management | 192.168.10.0/24 | 192.168.10.1 | Network device management |
| 20 | Staff | 192.168.20.0/24 | 192.168.20.1 | End-user workstations |
| 30 | Servers | 192.168.30.0/24 | 192.168.30.1 | Internal servers |
| 40 | Guest | 192.168.40.0/24 | 192.168.40.1 | Guest/untrusted devices |

---

## Security Policy Summary

| Rule | Source | Destination | Action |
|---|---|---|---|
| 1 | VLAN 40 (Guest) | VLAN 10,20,30 | BLOCK |
| 2 | VLAN 20 (Staff) | VLAN 10 (Mgmt) | BLOCK |
| 3 | VLAN 20 (Staff) | VLAN 30 (Servers) | ALLOW |
| 4 | VLAN 10 (Mgmt) | Any | ALLOW |
| 5 | Any | WAN | ALLOW |

---

## Validation Results

| Test | Expected | Result |
|---|---|---|
| PC-GUEST → PC-MGMT | BLOCKED | ✅ 100% loss |
| PC-GUEST → PC-STAFF | BLOCKED | ✅ 100% loss |
| PC-GUEST → PC-SRV | BLOCKED | ✅ 100% loss |
| PC-STAFF → PC-SRV | ALLOWED | ✅ 100% success |
| PC-STAFF → PC-MGMT | BLOCKED | ✅ 100% loss |
| Inter-VLAN routing | WORKING | ✅ Verified |

---

## Repository Structure
```
vlan-firewall-lab/
├── README.md
├── topology/
│   └── topology-diagram.png
├── addressing/
│   └── ip-addressing-table.md
├── configs/
│   ├── GW-Router-config.txt
│   └── CoreSW01-config.txt
├── firewall-policy/
│   └── acl-policy-justification.md
├── screenshots/
│   └── (validation screenshots)
└── troubleshooting/
    └── issues-and-resolutions.md
```

---

## Author
**Breda Wawira Kathuri**  
IT Officer & Network Engineer  
CCNA | HCIA Datacom | Cisco CyberOps Associate | Network Defense Essentials  
BSc Business Information Technology — KCA University  
MSc Information Technology (ongoing) — JKUAT
