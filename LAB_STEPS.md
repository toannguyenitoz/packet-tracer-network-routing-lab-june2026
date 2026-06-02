# Lab Steps

## 1. Build the Physical Topology

Use the following devices in Cisco Packet Tracer:

- 2 x Cisco 1941 routers
- 2 x Cisco 2960 switches
- 2 x PCs

## 2. Recommended Cabling

| From | To | Cable Type |
|---|---|---|
| PC0 FastEthernet0 | Switch0 FastEthernet0/1 | Copper straight-through |
| Switch0 FastEthernet0/2 | R1 GigabitEthernet0/0 | Copper straight-through |
| R1 Serial0/0/0 | R2 Serial0/0/0 | Serial DCE/DTE |
| R2 GigabitEthernet0/0 | Switch2 FastEthernet0/2 | Copper straight-through |
| Switch2 FastEthernet0/1 | PC2 FastEthernet0 | Copper straight-through |

> In Packet Tracer, use automatic cable selection if you are unsure.

## 3. Router Configuration

Apply:

```text
configs/R1-config.txt
configs/R2-config.txt
```

Important:

- The DCE side of the serial link needs the `clock rate 64000` command.
- If R1 is not the DCE side, move the clock rate command to R2.

To check the DCE side:

```text
show controllers serial 0/0/0
```

## 4. Switch Configuration

Apply:

```text
configs/Switch0-config.txt
configs/Switch2-config.txt
```

## 5. PC Configuration

On PC0 and PC2:

1. Open the PC.
2. Go to **Desktop**.
3. Go to **IP Configuration**.
4. Select **DHCP**.

Expected DHCP result:

| PC | Expected IP |
|---|---:|
| PC0 | 10.0.0.3 |
| PC2 | 192.168.1.3 |

## 6. Verify Connectivity

From PC0:

```text
ping 10.0.0.1
ping 20.20.20.2
ping 192.168.1.1
ping 192.168.1.3
```

From PC2:

```text
ping 192.168.1.1
ping 20.20.20.1
ping 10.0.0.1
ping 10.0.0.3
```

## 7. Verify OSPF

On R1 and R2:

```text
show ip ospf neighbor
show ip route
show ip protocols
```

You should see OSPF routes marked with `O`.

## 8. Verify SSH to Routers

From PC0 command prompt:

```text
ssh -l admin 10.0.0.1
```

From PC2 command prompt:

```text
ssh -l admin 192.168.1.1
```

Password:

```text
Admin@123
```

## 9. Verify Telnet to Switches

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
