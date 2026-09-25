# Troubleshooting Log

## Issue 1 — QEMU/pfSense Virtualization Conflict

**Symptom:** pfSense VM failed to start in GNS3 with error "HAXM acceleration support is not installed."

**Root Cause:** Windows Hyper-V was active on the host machine, which takes exclusive control of CPU virtualization extensions (VT-x). HAXM and Hyper-V cannot coexist.

**Diagnosis steps:**
- Ran `bcdedit /enum | findstr hypervisor` — confirmed hypervisorlaunchtype was active
- Ran `dism /online /get-featureinfo /featurename:Microsoft-Hyper-V` — confirmed Hyper-V was enabled
- Ran `sc query intelhaxm` — HAXM service showed STOPPED with exit code 31

**Resolution:** Migrated from pfSense/QEMU to Cisco Packet Tracer for the simulation environment. This eliminated all virtualization dependency while preserving the lab objectives. Router-on-a-stick with Cisco ACLs demonstrates equivalent or greater enterprise relevance than pfSense for a network engineering portfolio.

**Lesson learned:** Always verify host virtualization compatibility before selecting simulation tools. On machines with Hyper-V enabled (required for WSL2/Docker), QEMU-based appliances require either Hyper-V disabled or WHPX acceleration explicitly configured.

---

## Issue 2 — Incorrect Boot Order in QEMU

**Symptom:** pfSense ISO was set as the primary hard disk (hda) instead of the CD/DVD drive, causing boot failure.

**Root Cause:** GNS3 QEMU wizard defaulted to placing the ISO in the hda slot.

**Resolution:** Moved ISO to CD/DVD slot and created a separate blank 8GB qcow2 disk image for hda. Set boot priority to "CD/DVD-ROM or HDD."

---

## Issue 3 — Switch Trunk Port Not Passing VLAN Traffic

**Symptom:** After configuring VLANs on the switch, inter-VLAN pings failed even after router subinterfaces were configured.

**Root Cause:** The GigabitEthernet0/1 port connecting the switch to the router was left in default access mode (VLAN 1) instead of trunk mode.

**Resolution:**
```
interface GigabitEthernet0/1
switchport mode trunk
```

Verified with `show interfaces trunk` to confirm VLANs 10, 20, 30, 40 were allowed on the trunk.

---

## Issue 4 — Router Stuck in Setup Wizard

**Symptom:** On first boot, Cisco 2911 router entered interactive setup wizard, blocking manual CLI configuration.

**Resolution:** Pressed Ctrl+C to abort the wizard and entered privileged EXEC mode manually with `enable` then `configure terminal`.
