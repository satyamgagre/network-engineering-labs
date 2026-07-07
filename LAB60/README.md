# 🌐 LAB 60 - GRE WAN Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure a GRE (Generic Routing Encapsulation) tunnel between two remote routers over an OSPF-routed WAN and verify end-to-end connectivity.

---

## 🖧 Devices Used

- 3 × Cisco 2911 Routers
- 2 × Cisco 2960-24TT Switches
- 4 × Host PCs

> **Note:** Install the **HWIC-2T** module on all routers before connecting them with Serial cables.
>
> - Power off the router.
> - Drag **HWIC-2T** into an empty slot.
> - Power the router back on.

---

# Step 1 - Configure IP Addresses

## Router SITE-A

```bash
enable
configure terminal

interface GigabitEthernet0/0
 no shutdown
 ip address 192.168.10.1 255.255.255.0
exit

interface Serial0/3/0
 no shutdown
 ip address 10.10.10.1 255.255.255.252
exit

do write memory
```

---

## Router ISP

```bash
enable
configure terminal

interface Serial0/3/0
 no shutdown
 ip address 10.10.10.2 255.255.255.252
exit

interface Serial0/3/1
 no shutdown
 ip address 20.20.20.2 255.255.255.252
exit

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

interface GigabitEthernet0/0
 no shutdown
 ip address 192.168.20.1 255.255.255.0
exit

interface Serial0/3/0
 no shutdown
 ip address 20.20.20.1 255.255.255.252
exit

do write memory
```

---

## Configure PC IP Addresses

| PC | IP Address | Subnet Mask | Default Gateway |
|----|------------|-------------|-----------------|
| P1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| P2 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1 |
| P3 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 |
| P4 | 192.168.20.20 | 255.255.255.0 | 192.168.20.1 |

---

# Step 2 - Configure OSPF

## Router SITE-A

```bash
enable
configure terminal

router ospf 10
 router-id 1.1.1.1
 network 192.168.10.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.3 area 0
exit

do write memory
```

---

## Router ISP

```bash
enable
configure terminal

router ospf 10
 router-id 2.2.2.2
 network 10.10.10.0 0.0.0.3 area 0
 network 20.20.20.0 0.0.0.3 area 0
exit

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

router ospf 10
 router-id 3.3.3.3
 network 20.20.20.0 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.255 area 0
exit

do write memory
```

Verify OSPF neighbors:

```bash
show ip ospf neighbor
```

---

# Step 3 - Verify Connectivity

From **P1**:

```text
ping 192.168.10.20
ping 192.168.20.10
```

---

# Step 4 - Configure GRE Tunnel

## Router SITE-A

```bash
enable
configure terminal

interface Tunnel2
 ip address 192.168.50.5 255.255.255.0
 tunnel source Serial0/3/0
 tunnel destination 20.20.20.1
exit

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

interface Tunnel2
 ip address 192.168.50.10 255.255.255.0
 tunnel source Serial0/3/0
 tunnel destination 10.10.10.1
exit

do write memory
```

---

# Step 5 - Verify GRE Tunnel

Check tunnel status:

```bash
show interface tunnel 2
```

Check interface status:

```bash
show ip interface brief
```

---

# ✅ Result

Successfully configured a GRE tunnel between the two site routers over the WAN. The tunnel interface came up successfully, enabling encapsulated communication between the remote networks.
