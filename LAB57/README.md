# 🌐 LAB 57 - Configuring EIGRPv6 for IPv6 Routing Using Cisco Packet Tracer

## 🎯 Objective

Configure EIGRP for IPv6 between three routers and verify end-to-end connectivity between two IPv6 LANs.

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

## Router R1

```bash
interface GigabitEthernet0/0
 no shutdown
 ipv6 address 2001:DB8:3:1::2/64
exit

interface GigabitEthernet0/1
 no shutdown
 ipv6 address 2001:DB8:3:2::2/64
exit

do write memory
```

---

## Router R2

```bash
interface GigabitEthernet0/0
 no shutdown
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

## Router R3

```bash
interface GigabitEthernet0/0
 no shutdown
 ipv6 address 2001:DB8:3:2::1/64
exit

interface GigabitEthernet0/1
 no shutdown
 ipv6 address FE80::2 link-local
 ipv6 address 2001:DB8:1:2::1/64
exit

do write memory
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

# Step 4 - Verify Local Connectivity

Before configuring EIGRPv6:

- Ping **P1 → P2** ✓
- Ping **P1 → P3** ✗ (Unreachable)

---

# Step 5 - Configure EIGRPv6

## Router R2

```bash
enable
configure terminal

ipv6 router eigrp 50
 eigrp router-id 1.1.1.1
 no shutdown
exit

interface GigabitEthernet0/0
 ipv6 eigrp 50
exit

interface GigabitEthernet0/1
 ipv6 eigrp 50
exit

do write memory
```

---

## Router R1

```bash
enable
configure terminal

ipv6 router eigrp 50
 eigrp router-id 2.2.2.2
 no shutdown
exit

interface GigabitEthernet0/0
 ipv6 eigrp 50
exit

interface GigabitEthernet0/1
 ipv6 eigrp 50
exit

do write memory
```

---

## Router R3

```bash
enable
configure terminal

ipv6 router eigrp 50
 eigrp router-id 3.3.3.3
 no shutdown
exit

interface GigabitEthernet0/0
 ipv6 eigrp 50
exit

interface GigabitEthernet0/1
 ipv6 eigrp 50
exit

do write memory
```

---

# Step 6 - Verify Connectivity

After EIGRPv6 neighbors form and routes are exchanged:

From **P1**:

```text
ping <IPv6 address of P3>
```

From **P2**:

```text
ping <IPv6 address of P4>
```

Expected Result:

- Successful replies from hosts on the remote LAN.

---

# ✅ Result

Successfully configured EIGRPv6 routing. The routers dynamically exchanged IPv6 routes, allowing communication between both IPv6 networks.
````
