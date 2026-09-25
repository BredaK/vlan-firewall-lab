# Firewall Policy — ACL Justification

## Design Philosophy
This network enforces a least-privilege security model. Each VLAN is treated as a trust zone. Traffic between zones is denied by default unless explicitly permitted. This mirrors enterprise security frameworks such as the CIS Controls and NIST SP 800-53.

---

## ACL 1 — GUEST-POLICY (Applied inbound on Gig0/0.40)

### Rules
```
deny ip 192.168.40.0 0.0.0.255 192.168.10.0 0.0.0.255
deny ip 192.168.40.0 0.0.0.255 192.168.20.0 0.0.0.255
deny ip 192.168.40.0 0.0.0.255 192.168.30.0 0.0.0.255
permit ip any any
```

### Justification
Guest devices are untrusted by nature — they may be personally owned, unmanaged, or potentially compromised. Allowing guest traffic to reach internal VLANs would expose staff workstations, internal servers, and network management interfaces to lateral movement attacks.

The final `permit ip any any` allows guest devices to reach the internet (WAN) only, which is the expected and acceptable use case for a guest network.

---

## ACL 2 — STAFF-POLICY (Applied inbound on Gig0/0.20)

### Rules
```
deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
permit ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255
permit ip any any
```

### Justification
Staff users require access to internal servers (VLAN 30) for file sharing, internal applications, and business operations. However, staff should not have access to the Management VLAN (VLAN 10), which contains network device interfaces (router/switch management IPs).

Restricting staff from the Management VLAN prevents accidental or malicious configuration changes to network infrastructure. Only designated network administrators should access management interfaces.

---

## ACL Application Points

| ACL | Applied On | Direction | Reason |
|---|---|---|---|
| GUEST-POLICY | Gig0/0.40 | Inbound | Filter guest traffic as it enters the router |
| STAFF-POLICY | Gig0/0.20 | Inbound | Filter staff traffic as it enters the router |

Inbound ACLs are applied on the source subinterface so traffic is filtered before it is routed, reducing unnecessary processing.

---

## Validation Evidence

| Test Case | Source | Destination | Expected | Actual |
|---|---|---|---|---|
| Guest to Management | 192.168.40.10 | 192.168.10.10 | BLOCKED | ✅ 100% loss |
| Guest to Staff | 192.168.40.10 | 192.168.20.10 | BLOCKED | ✅ 100% loss |
| Guest to Servers | 192.168.40.10 | 192.168.30.10 | BLOCKED | ✅ 100% loss |
| Staff to Servers | 192.168.20.10 | 192.168.30.10 | ALLOWED | ✅ 100% success |
| Staff to Management | 192.168.20.10 | 192.168.10.10 | BLOCKED | ✅ 100% loss |
