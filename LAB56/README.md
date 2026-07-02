# 🌐 LAB 56 - RIPng Routing for IPv6 Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure RIPng (Routing Information Protocol next generation) to enable dynamic IPv6 routing between two remote LANs and verify end-to-end connectivity.

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

## Router1

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

Before enabling RIPng:

- Ping from **P1 → P2** (same LAN) ✓
- Ping from **P1 → P3** (different LAN) ✗

---

# Step 5 - Configure RIPng

## Router1

```bash
enable
configure terminal

ipv6 router rip IPV6_RIP
exit

interface GigabitEthernet0/0
 ipv6 rip IPV6_RIP enable
exit

interface GigabitEthernet0/1
 ipv6 rip IPV6_RIP enable
exit

do write memory
```

---

## Router2

```bash
enable
configure terminal

ipv6 router rip IPV6_RIP
exit

interface GigabitEthernet0/0
 ipv6 rip IPV6_RIP enable
exit

interface GigabitEthernet0/1
 ipv6 rip IPV6_RIP enable
exit

do write memory
```

---

## Router3

```bash
enable
configure terminal

ipv6 router rip IPV6_RIP
exit

interface GigabitEthernet0/0
 ipv6 rip IPV6_RIP enable
exit

interface GigabitEthernet0/1
 ipv6 rip IPV6_RIP enable
exit

do write memory
```

---

# Step 6 - Verify Connectivity

After RIPng converges, test communication between the two sites.

From **P1**:

```text
ping <IPv6 address of P3>
```

From **P2**:

```text
ping <IPv6 address of P4>
```

Expected Result:

- Successful replies from remote hosts.

---

# ✅ Result

Successfully configured RIPng for IPv6 routing. All routers dynamically exchanged IPv6 routes, allowing hosts on different LANs to communicate successfully.
