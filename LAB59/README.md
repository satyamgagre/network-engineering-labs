# 🌐 LAB 59 - Point-to-Point (PPP) WAN Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure a PPP WAN link with CHAP authentication between two routers and verify communication between two LANs.

---

## 🖧 Devices Used

- 2 × Cisco 2911 Routers
- 2 × Cisco 2960-24TT Switches
- 4 × Host PCs

> **Note:** Install the **HWIC-2T** module on both routers before connecting them with a Serial cable.
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
 ip address 10.10.10.2 255.255.255.252
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

## Router SITE-B

```bash
enable
configure terminal

router ospf 10
 router-id 2.2.2.2
 network 192.168.20.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.3 area 0
exit

do write memory
```

---

# Step 3 - Verify Connectivity

From **P1**:

```text
ping 192.168.10.20
ping 192.168.20.10
ping 192.168.20.20
```

---

# Step 4 - Configure PPP with CHAP Authentication

## Router SITE-A

```bash
enable
configure terminal

hostname SITE-A
username SITE-B password admin123

interface Serial0/3/0
 encapsulation ppp
 ppp authentication chap
exit

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

hostname SITE-B
username SITE-A password admin123

interface Serial0/3/0
 encapsulation ppp
 ppp authentication chap
exit

do write memory
```

---

# Step 5 - Verify PPP

Check the PPP interface:

```bash
show interfaces serial 0/3/0
```

Check CHAP status:

```bash
show ppp all
```

Verify connectivity:

```text
ping 192.168.20.10
ping 192.168.20.20
```

---

# ✅ Result

Successfully configured a PPP WAN link with CHAP authentication between the two routers. OSPF routed traffic across the PPP connection, enabling communication between both LANs.
