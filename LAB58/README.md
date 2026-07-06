# 🌐 LAB 58 - Configuring OSPFv3 for IPv6 Routing Using Cisco Packet Tracer

## 🎯 Objective

Configure OSPFv3 for IPv6 routing between three routers and verify communication between two IPv6 LANs.

---

## 🖧 Devices Used

- 3 × Cisco 2911 Routers
- 2 × Cisco 2960-24TT Switches
- 4 × Host PCs

---

# Step 1 - Enable IPv6 Routing

On **all routers**:

```bash
enable
configure terminal

ipv6 unicast-routing
```

---

# Step 2 - Configure IPv6 Addresses

## Router R2

```bash
interface GigabitEthernet0/0
 no shutdown
 ipv6 address FE80::8 link-local
 ipv6 address 2001:DB8:3:1::1/64
exit

interface GigabitEthernet0/1
 no shutdown
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:1:1::1/64
exit

do write memory
```

---

## Router R1

```bash
interface GigabitEthernet0/0
 no shutdown
 ipv6 address FE80::9 link-local
 ipv6 address 2001:DB8:3:1::2/64
exit

interface GigabitEthernet0/1
 no shutdown
 ipv6 address FE80::6 link-local
 ipv6 address 2001:DB8:3:2::2/64
exit

do write memory
```

---

## Router R3

```bash
interface GigabitEthernet0/0
 no shutdown
 ipv6 address FE80::7 link-local
 ipv6 address 2001:DB8:3:2::1/64
exit

interface GigabitEthernet0/1
 no shutdown
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:1:2::1/64
exit

do write memory
```

Verify:

```bash
show ipv6 interface brief
```

---

# Step 3 - Configure IPv6 on PCs

On all PCs:

```
Desktop → IP Configuration
```

Set:

```
IPv6 Configuration → Automatic (SLAAC)
```

---

# Step 4 - Verify Connectivity

Before configuring OSPFv3:

- Ping **P1 → P2** ✓
- Ping **P1 → P4** ✗ (Unreachable)

---

# Step 5 - Configure OSPFv3

## Router R2

```bash
enable
configure terminal

ipv6 router ospf 10
 router-id 1.1.1.1
exit

interface GigabitEthernet0/0
 ipv6 ospf 10 area 0
exit

interface GigabitEthernet0/1
 ipv6 ospf 10 area 0
exit

do write memory
```

Verify:

```bash
show ipv6 ospf neighbor
```

---

## Router R1

```bash
enable
configure terminal

ipv6 router ospf 10
 router-id 2.2.2.2
exit

interface GigabitEthernet0/0
 ipv6 ospf 10 area 0
exit

interface GigabitEthernet0/1
 ipv6 ospf 10 area 0
exit

do write memory
```

---

## Router R3

```bash
enable
configure terminal

ipv6 router ospf 10
 router-id 3.3.3.3
exit

interface GigabitEthernet0/0
 ipv6 ospf 10 area 0
exit

interface GigabitEthernet0/1
 ipv6 ospf 10 area 0
exit

do write memory
```

Verify:

```bash
show ipv6 ospf neighbor
```

---

# Step 6 - Verify Connectivity

After OSPFv3 neighbors are established:

From **P1**:

```text
ping <IPv6 address of P2>
```

Expected Result:

- Successful reply ✓

From **P1**:

```text
ping <IPv6 address of P4>
```

Expected Result:

- Successful reply ✓

---

# ✅ Result

Successfully configured OSPFv3 for IPv6 routing. OSPFv3 dynamically exchanged IPv6 routes, enabling communication between both IPv6 networks.
