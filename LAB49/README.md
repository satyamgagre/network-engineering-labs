# 🌐 LAB 49 - Configure HSRP with Multiple VLANs Using Cisco Packet Tracer

## 🎯 Objective

Configure multiple VLANs with inter-VLAN routing using two multilayer switches running HSRP for gateway redundancy. Configure OSPF between the multilayer switches and ISP router, then verify connectivity and failover by disabling an SVI on the active multilayer switch.

---

## 🖧 Devices Used

- 1 × Cisco 2911 Router (ISP)
- 2 × Cisco 3650-24PS Multilayer Switches
- 4 × Cisco 2960-24TT Switches
- 6 × Host PCs
- 1 × Server

---

# 📋 Network Information

## VLAN Information

| VLAN | Name | Network | Virtual Gateway |
|------|------|---------|-----------------|
| 10 | IT | 192.168.10.0/24 | 192.168.10.1 |
| 20 | HR | 192.168.20.0/24 | 192.168.20.1 |
| 30 | FIN | 192.168.30.0/24 | 192.168.30.1 |

---

## ISP Router

| Interface | IP Address |
|-----------|------------|
| G0/0 | 10.10.10.2/30 |
| G0/1 | 10.10.10.6/30 |
| G0/2 | 8.8.8.1/24 |

---

## MLSW-1 (Active)

| Interface | IP Address |
|-----------|------------|
| G1/0/4 | 10.10.10.1/30 |

---

## MLSW-2 (Standby)

| Interface | IP Address |
|-----------|------------|
| G1/0/4 | 10.10.10.5/30 |

---

## Host PCs

| Device | IP Address | Default Gateway |
|---------|------------|-----------------|
| P1 | 192.168.10.10 | 192.168.10.1 |
| P2 | 192.168.10.20 | 192.168.10.1 |
| P3 | 192.168.20.10 | 192.168.20.1 |
| P4 | 192.168.20.20 | 192.168.20.1 |
| P5 | 192.168.30.10 | 192.168.30.1 |
| P6 | 192.168.30.20 | 192.168.30.1 |

---

## Google Server

| Device | IP Address | Default Gateway |
|---------|------------|-----------------|
| Google Server | 8.8.8.8 | 8.8.8.1 |

---

# Step 1 - Configure Layer 3 Interfaces

## MLSW-1

```cisco
enable
configure terminal

interface GigabitEthernet1/0/4
 no switchport
 ip address 10.10.10.1 255.255.255.252
 no shutdown

end
write memory
```

---

## MLSW-2

```cisco
enable
configure terminal

interface GigabitEthernet1/0/4
 no switchport
 ip address 10.10.10.5 255.255.255.252
 no shutdown

end
write memory
```

---

## ISP Router

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.10.10.2 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 ip address 10.10.10.6 255.255.255.252
 no shutdown

interface GigabitEthernet0/2
 ip address 8.8.8.1 255.255.255.0
 no shutdown

end
write memory
```

---

# Step 2 - Configure Trunk Ports on Access Switches

## S2

```cisco
enable
configure terminal

interface range FastEthernet0/3-4
 switchport mode trunk

end
write memory
```

---

## S3

```cisco
enable
configure terminal

interface range FastEthernet0/3-4
 switchport mode trunk

end
write memory
```

---

## S4

```cisco
enable
configure terminal

interface range FastEthernet0/3-4
 switchport mode trunk

end
write memory
```

---

# Step 3 - Configure VLANs on Access Switches

## S2 (VLAN 10)

```cisco
enable
configure terminal

vlan 10
 name IT

interface range FastEthernet0/1-2
 switchport mode access
 switchport access vlan 10

interface range FastEthernet0/5-24
 switchport mode access
 switchport access vlan 10

end
write memory
```

---

## S3 (VLAN 20)

```cisco
enable
configure terminal

vlan 20
 name HR

interface range FastEthernet0/1-2
 switchport mode access
 switchport access vlan 20

interface range FastEthernet0/5-24
 switchport mode access
 switchport access vlan 20

end
write memory
```

---

## S4 (VLAN 30)

```cisco
enable
configure terminal

vlan 30
 name FIN

interface range FastEthernet0/1-2
 switchport mode access
 switchport access vlan 30

interface range FastEthernet0/5-24
 switchport mode access
 switchport access vlan 30

end
write memory
```

---

# Step 4 - Configure Trunk Ports on Multilayer Switches

## MLSW-1

```cisco
enable
configure terminal

interface range GigabitEthernet1/0/1-3
 switchport mode trunk

end
write memory
```

---

## MLSW-2

```cisco
enable
configure terminal

interface range GigabitEthernet1/0/1-3
 switchport mode trunk

end
write memory
```

---

# Step 5 - Create VLANs on Both Multilayer Switches

## MLSW-1

```cisco
enable
configure terminal

vlan 10
 name IT

vlan 20
 name HR

vlan 30
 name FIN

end
write memory
```

---

## MLSW-2

```cisco
enable
configure terminal

vlan 10
 name IT

vlan 20
 name HR

vlan 30
 name FIN

end
write memory
```

---

# Step 6 - Enable Layer 3 Routing

## MLSW-1

```cisco
enable
configure terminal

ip routing

end
write memory
```

---

## MLSW-2

```cisco
enable
configure terminal

ip routing

end
write memory
```

---

# Step 7 - Configure SVIs and HSRP

## MLSW-1 (Active)

```cisco
enable
configure terminal

interface vlan 10
 ip address 192.168.10.3 255.255.255.0
 standby 10 priority 100
 standby 10 ip 192.168.10.1

interface vlan 20
 ip address 192.168.20.3 255.255.255.0
 standby 20 priority 100
 standby 20 ip 192.168.20.1

interface vlan 30
 ip address 192.168.30.3 255.255.255.0
 standby 30 priority 100
 standby 30 ip 192.168.30.1

end
write memory
```

---

## MLSW-2 (Standby)

```cisco
enable
configure terminal

interface vlan 10
 ip address 192.168.10.2 255.255.255.0
 standby 10 priority 90
 standby 10 ip 192.168.10.1

interface vlan 20
 ip address 192.168.20.2 255.255.255.0
 standby 20 priority 90
 standby 20 ip 192.168.20.1

interface vlan 30
 ip address 192.168.30.2 255.255.255.0
 standby 30 priority 90
 standby 30 ip 192.168.30.1

end
write memory
```

Verify HSRP:

```cisco
show standby brief
```

---

# Step 8 - Configure OSPF

## ISP Router

```cisco
enable
configure terminal

router ospf 10
 router-id 1.1.1.1

 network 8.8.8.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.3 area 0
 network 10.10.10.4 0.0.0.3 area 0

end
write memory
```

---

## MLSW-1

```cisco
enable
configure terminal

router ospf 10
 router-id 2.2.2.2

 network 10.10.10.0 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0

end
write memory
```

---

## MLSW-2

```cisco
enable
configure terminal

router ospf 10
 router-id 3.3.3.3

 network 10.10.10.4 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0
 network 192.168.20.0 0.0.0.255 area 0
 network 192.168.30.0 0.0.0.255 area 0

end
write memory
```

---

# Step 9 - Verify Connectivity

From any PC:

```text
ping 8.8.8.8
```

Trace the path:

```text
tracert 8.8.8.8
```

Expected Result:

- All VLANs should reach the Google Server through the Active multilayer switch.

---

# Step 10 - Test HSRP Failover

Disable any SVI on the Active multilayer switch.

Example:

```cisco
interface vlan 10
shutdown
```

or disable the uplink interface.

Test again:

```text
ping 8.8.8.8
```

```text
tracert 8.8.8.8
```

Expected Result:

- Traffic automatically switches to MLSW-2.
- Connectivity remains uninterrupted.
- Hosts continue using the same virtual gateway.

---

# Useful Verification Commands

Verify HSRP:

```cisco
show standby brief
```

Verify OSPF neighbors:

```cisco
show ip ospf neighbor
```

Verify routing table:

```cisco
show ip route
```

Verify VLANs:

```cisco
show vlan brief
```

Verify trunk ports:

```cisco
show interfaces trunk
```

Verify SVIs:

```cisco
show ip interface brief
```

---

# HSRP Configuration Summary

| VLAN | Virtual Gateway | Active MLSW | Standby MLSW |
|------|-----------------|-------------|--------------|
| VLAN 10 | 192.168.10.1 | MLSW-1 | MLSW-2 |
| VLAN 20 | 192.168.20.1 | MLSW-1 | MLSW-2 |
| VLAN 30 | 192.168.30.1 | MLSW-1 | MLSW-2 |

---

# ✅ Result

Successfully configured multiple VLANs with inter-VLAN routing using Cisco multilayer switches. HSRP was implemented to provide gateway redundancy for VLANs 10, 20, and 30, while OSPF enabled dynamic routing between the multilayer switches and the ISP router. During failover testing, the standby multilayer switch automatically assumed the active gateway role, ensuring uninterrupted network connectivity for all VLANs.
