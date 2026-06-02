# IP Addressing Table

> Detailed subnetting calculations are available in `SUBNETTING_PLAN.md`.

## Recommended VLSM Summary

| Segment | Required Hosts | Recommended Subnet | Subnet Mask | Usable Range | Broadcast |
|---|---:|---:|---:|---:|---:|
| Left LAN | 250 | 10.0.0.0/24 | 255.255.255.0 | 10.0.0.1 - 10.0.0.254 | 10.0.0.255 |
| WAN Link | 2 | 20.20.20.0/30 | 255.255.255.252 | 20.20.20.1 - 20.20.20.2 | 20.20.20.3 |
| Right LAN | 120 | 192.168.1.0/25 | 255.255.255.128 | 192.168.1.1 - 192.168.1.126 | 192.168.1.127 |


## Networks

| Network | Subnet Mask | Purpose |
|---|---:|---|
| 10.0.0.0/16 | 255.255.0.0 | Left LAN |
| 20.20.20.0/30 | 255.255.255.252 | Router-to-router WAN link |
| 192.168.0.0/16 | 255.255.0.0 | Right LAN |

## Devices

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---:|---:|---:|
| R1 | G0/0 | 10.0.0.1 | 255.255.0.0 | N/A |
| R1 | S0/0/0 | 20.20.20.1 | 255.255.255.252 | N/A |
| R2 | S0/0/0 | 20.20.20.2 | 255.255.255.252 | N/A |
| R2 | G0/0 | 192.168.1.1 | 255.255.0.0 | N/A |
| Switch0 | VLAN 1 | 10.0.0.2 | 255.255.0.0 | 10.0.0.1 |
| Switch2 | VLAN 1 | 192.168.1.2 | 255.255.0.0 | 192.168.1.1 |
| PC0 | DHCP | 10.0.0.3 expected | 255.255.0.0 | 10.0.0.1 |
| PC2 | DHCP | 192.168.1.3 expected | 255.255.0.0 | 192.168.1.1 |

## DHCP Excluded Addresses

| Router | Excluded Range | Reason |
|---|---|---|
| R1 | 10.0.0.1 - 10.0.0.2 | Gateway and switch management IP |
| R2 | 192.168.0.1 - 192.168.1.2 | Reserve addresses before expected PC lease |
