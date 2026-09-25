# IP Addressing Table

## Router — GW-Router (Cisco 2911)

| Interface | VLAN | IP Address | Subnet Mask | Description |
|---|---|---|---|---|
| GigabitEthernet0/0 | trunk | unassigned | — | LAN trunk to Core-SW01 |
| GigabitEthernet0/0.10 | 10 | 192.168.10.1 | 255.255.255.0 | Management gateway |
| GigabitEthernet0/0.20 | 20 | 192.168.20.1 | 255.255.255.0 | Staff gateway |
| GigabitEthernet0/0.30 | 30 | 192.168.30.1 | 255.255.255.0 | Servers gateway |
| GigabitEthernet0/0.40 | 40 | 192.168.40.1 | 255.255.255.0 | Guest gateway |
| GigabitEthernet0/2 | — | 203.0.113.1 | 255.255.255.252 | WAN uplink |

## Switch — Core-SW01 (Cisco 2960)

| Interface | VLAN | Mode | Connected To |
|---|---|---|---|
| GigabitEthernet0/1 | trunk | trunk | GW-Router Gig0/0 |
| FastEthernet0/1 | 10 | access | PC-MGMT |
| FastEthernet0/2 | 20 | access | PC-STAFF |
| FastEthernet0/3 | 30 | access | PC-SRV |
| FastEthernet0/4 | 40 | access | PC-GUEST |

## End Hosts

| Host | VLAN | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC-MGMT | 10 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC-STAFF | 20 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| PC-SRV | 30 | 192.168.30.10 | 255.255.255.0 | 192.168.30.1 |
| PC-GUEST | 40 | 192.168.40.10 | 255.255.255.0 | 192.168.40.1 |

## WAN

| Segment | IP Range | Description |
|---|---|---|
| WAN link | 203.0.113.0/30 | Simulated ISP uplink |
| Default route | 0.0.0.0/0 | Via GigabitEthernet0/2 |
