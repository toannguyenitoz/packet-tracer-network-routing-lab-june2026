# IP Address Planning and Subnetting

This section explains how the IP address plan was created from the given **network IP / prefix** and the required **number of hosts**.

## 1. Given Requirements

| Segment | Given Network / Prefix | Required Hosts | Purpose |
|---|---:|---:|---|
| Left LAN | 10.0.0.0/16 | 250 hosts | PC0, Switch0, R1 LAN interface |
| WAN Link | 20.20.20.0/30 | 2 hosts | Point-to-point link between R1 and R2 |
| Right LAN | 192.168.0.0/16 | 120 hosts | PC2, Switch2, R2 LAN interface |

## 2. Subnetting Formula

To calculate the required subnet size:

```text
Usable hosts = 2^h - 2
```

Where:

```text
h = number of host bits
```

The `-2` is because each subnet has:

- 1 network address
- 1 broadcast address

## 3. Left LAN Calculation

### Requirement

```text
Network block: 10.0.0.0/16
Required hosts: 250
```

### Find the minimum host bits

```text
2^h - 2 >= 250
```

Test values:

```text
2^7 - 2 = 126   not enough
2^8 - 2 = 254   enough
```

So:

```text
Host bits = 8
Network bits = 32 - 8 = 24
```

### Minimum subnet required

```text
10.0.0.0/24
```

### Subnet details

| Item | Value |
|---|---:|
| Network address | 10.0.0.0 |
| Prefix | /24 |
| Subnet mask | 255.255.255.0 |
| Usable host range | 10.0.0.1 - 10.0.0.254 |
| Broadcast address | 10.0.0.255 |
| Total addresses | 256 |
| Usable hosts | 254 |

### Address assignment

| Device | Interface | IP Address | Purpose |
|---|---|---:|---|
| R1 | G0/0 | 10.0.0.1/24 | Default gateway |
| Switch0 | VLAN 1 | 10.0.0.2/24 | Switch management |
| PC0 | DHCP | 10.0.0.3/24 | Client PC |

## 4. WAN Link Calculation

### Requirement

```text
Network block: 20.20.20.0/30
Required hosts: 2
```

A point-to-point WAN link only needs two usable IP addresses.

### /30 subnet details

| Item | Value |
|---|---:|
| Network address | 20.20.20.0 |
| Prefix | /30 |
| Subnet mask | 255.255.255.252 |
| Usable host range | 20.20.20.1 - 20.20.20.2 |
| Broadcast address | 20.20.20.3 |
| Total addresses | 4 |
| Usable hosts | 2 |

### Address assignment

| Device | Interface | IP Address | Purpose |
|---|---|---:|---|
| R1 | S0/0/0 | 20.20.20.1/30 | WAN link to R2 |
| R2 | S0/0/0 | 20.20.20.2/30 | WAN link to R1 |

## 5. Right LAN Calculation

### Requirement

```text
Network block: 192.168.0.0/16
Required hosts: 120
```

### Find the minimum host bits

```text
2^h - 2 >= 120
```

Test values:

```text
2^6 - 2 = 62    not enough
2^7 - 2 = 126   enough
```

So:

```text
Host bits = 7
Network bits = 32 - 7 = 25
```

### Minimum subnet required

```text
192.168.1.0/25
```

`192.168.1.0/25` is selected because the original topology labels the right PC as `192.168.1.3`.

This subnet still belongs inside the larger given block:

```text
192.168.0.0/16
```

### Subnet details

| Item | Value |
|---|---:|
| Network address | 192.168.1.0 |
| Prefix | /25 |
| Subnet mask | 255.255.255.128 |
| Usable host range | 192.168.1.1 - 192.168.1.126 |
| Broadcast address | 192.168.1.127 |
| Total addresses | 128 |
| Usable hosts | 126 |

### Address assignment

| Device | Interface | IP Address | Purpose |
|---|---|---:|---|
| R2 | G0/0 | 192.168.1.1/25 | Default gateway |
| Switch2 | VLAN 1 | 192.168.1.2/25 | Switch management |
| PC2 | DHCP | 192.168.1.3/25 | Client PC |

## 6. Final Recommended VLSM Addressing Plan

| Segment | Required Hosts | Selected Subnet | Subnet Mask | Usable Hosts | Usable Range | Broadcast |
|---|---:|---:|---:|---:|---:|---:|
| Left LAN | 250 | 10.0.0.0/24 | 255.255.255.0 | 254 | 10.0.0.1 - 10.0.0.254 | 10.0.0.255 |
| WAN Link | 2 | 20.20.20.0/30 | 255.255.255.252 | 2 | 20.20.20.1 - 20.20.20.2 | 20.20.20.3 |
| Right LAN | 120 | 192.168.1.0/25 | 255.255.255.128 | 126 | 192.168.1.1 - 192.168.1.126 | 192.168.1.127 |

## 7. Lab Topology Addressing vs Optimised VLSM

The original topology shows:

```text
10.0.0.0/16
192.168.0.0/16
```

Those are large network blocks. They work in Packet Tracer, but they are not efficient for the required number of hosts.

For better IP address planning, the recommended VLSM design is:

```text
10.0.0.0/24       for 250 hosts
20.20.20.0/30     for 2 router-link hosts
192.168.1.0/25    for 120 hosts
```

## 8. Why This Matters in IT Support and Networking

Good IP planning helps with:

- Avoiding IP address waste
- Reducing broadcast domain size
- Making networks easier to troubleshoot
- Creating clear documentation
- Supporting future network growth
- Improving professional network design habits

## 9. Optional: If Following the Topology Exactly

If the assessment requires the exact prefixes shown in the diagram, use:

| Segment | Network | Mask |
|---|---:|---:|
| Left LAN | 10.0.0.0/16 | 255.255.0.0 |
| WAN Link | 20.20.20.0/30 | 255.255.255.252 |
| Right LAN | 192.168.0.0/16 | 255.255.0.0 |

If the assessment expects subnet optimisation from host requirements, use the recommended VLSM plan instead.
