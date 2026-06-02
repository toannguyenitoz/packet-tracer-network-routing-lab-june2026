# Verification Commands

## Router Verification

Run these commands on R1 and R2.

### Check interfaces

```text
show ip interface brief
```

Expected:

- GigabitEthernet0/0 is up/up.
- Serial0/0/0 is up/up.

### Check routing protocol

```text
show ip protocols
```

Expected:

- OSPF process 1 is running.
- Correct networks are advertised.

### Check OSPF neighbour

```text
show ip ospf neighbor
```

Expected:

- R1 and R2 should be OSPF neighbours.

### Check routing table

```text
show ip route
```

Expected on R1:

```text
O 192.168.0.0/16
```

Expected on R2:

```text
O 10.0.0.0/16
```

### Check DHCP leases

```text
show ip dhcp binding
```

Expected:

- PC0 receives an IP address from R1.
- PC2 receives an IP address from R2.

## Switch Verification

Run on Switch0 and Switch2.

```text
show ip interface brief
show running-config
```

Expected:

- VLAN 1 has the configured management IP.
- VLAN 1 is up/up if at least one active switch port is connected.

## PC Verification

On PC0:

```text
ipconfig
ping 10.0.0.1
ping 192.168.1.3
```

On PC2:

```text
ipconfig
ping 192.168.1.1
ping 10.0.0.3
```

## Remote Access Tests

### SSH to routers

From PC0:

```text
ssh -l admin 10.0.0.1
```

From PC2:

```text
ssh -l admin 192.168.1.1
```

Password:

```text
Admin@123
```

### Telnet to switches

From PC0:

```text
telnet 10.0.0.2
```

From PC2:

```text
telnet 192.168.1.2
```

Password:

```text
cisco
```

## Troubleshooting Notes

If ping fails:

1. Check cable connections.
2. Check all router interfaces are `up/up`.
3. Check serial clock rate on the DCE side.
4. Check OSPF neighbour status.
5. Check DHCP assignment on PCs.
6. Check switch VLAN 1 management status.
7. Check default gateways on switches.
