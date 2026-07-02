# 🌐 LAB 55 - IPv6 Static Routing Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure IPv6 static routes between two LANs connected through three routers and verify end-to-end connectivity using SLAAC.

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

# Step 2 - Configure IPv6 Addresses on Routers

## Router2 (Site A)

```bash
interface GigabitEthernet0/1
 no shutdown
 ipv6 address FE80::1 link-local
 ipv6 address 2001:DB8:1:1::1/64
exit

interface GigabitEthernet0/0
 no shutdown
 ipv6 address 2001:DB8:3:1::1/64
exit

do write memory
```

---

## Router1 (Core Router)

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

## Router3 (Site B)

```bash
interface GigabitEthernet0/0
 no shutdown
 ipv6 address 2001:DB8:3:2::1/64
exit

interface GigabitEthernet0/1
 no shutdown
 ipv6 address FE80::3 link-local
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

# Step 4 - Configure IPv6 Static Routes

## Router2

```bash
ipv6 route 2001:DB8:1:2::/64 2001:DB8:3:1::2
```

---

## Router1

```bash
ipv6 route 2001:DB8:1:1::/64 2001:DB8:3:1::1
ipv6 route 2001:DB8:1:2::/64 2001:DB8:3:2::1
```

---

## Router3

```bash
ipv6 route 2001:DB8:1:1::/64 2001:DB8:3:2::2
```

Save the configuration:

```bash
do write memory
```

---

# Step 5 - Verify Connectivity

Test communication between the two sites.

From **P1**:

```text
ping <IPv6 address of P4>
```

From **P4**:

```text
ping <IPv6 address of P1>
```

---

# ✅ Result

Successfully configured IPv6 static routing between two remote LANs. All hosts automatically obtained IPv6 addresses using SLAAC and communicated successfully across the network through static IPv6 routes.
