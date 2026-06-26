# 🌐 LAB 50 - IPv6 Configuration on Cisco Packet Tracer

## 🎯 Objective

Configure IPv6 addressing on a Cisco router using both Global Unicast Addresses (GUA) and Link-Local Addresses. Configure IPv6 on end devices using both Static addressing and Stateless Address Autoconfiguration (SLAAC), then verify IPv6 connectivity within and between LANs.

---

## 🖧 Devices Used

- 1 × Cisco 2911 Router
- 2 × Cisco 2960-24TT Switches
- 4 × Host PCs

---

# 📋 IPv6 Addressing Plan

## Router R1

| Interface | Link-Local Address | Global Unicast Address (GUA) |
|-----------|--------------------|------------------------------|
| G0/0 | FE80::1 | 2001:0BB9:AABB:1234::1/64 |
| G0/1 | FE80::2 | 2001:0BB9:CDD:5678::1/64 |

---

## LAN A (2001:0BB9:AABB:1234::/64)

| Device | Configuration |
|---------|---------------|
| P1 | Automatic (SLAAC) |
| P2 | Static |

---

## LAN B (2001:0BB9:CDD:5678::/64)

| Device | Configuration |
|---------|---------------|
| P3 | Static |
| P4 | Automatic (SLAAC) |

---

# Step 1 - Enable IPv6 Routing

On **R1**:

```cisco
enable
configure terminal

ipv6 unicast-routing
```

---

# Step 2 - Configure IPv6 Addresses on Router

```cisco
interface GigabitEthernet0/0
 no shutdown
 ipv6 address FE80::1 link-local
 ipv6 address 2001:0BB9:AABB:1234::1/64
 exit

interface GigabitEthernet0/1
 no shutdown
 ipv6 address FE80::2 link-local
 ipv6 address 2001:0BB9:CDD:5678::1/64
 exit

end
write memory
```

---

# Step 3 - Configure IPv6 on PCs

## P1 (LAN A)

Configure:

- IPv6 Configuration → **Automatic**

Packet Tracer will automatically assign:

- Global Unicast Address
- Link-Local Address
- Default Gateway

using SLAAC.

---

## P2 (LAN A)

Configure manually:

| Setting | Value |
|---------|-------|
| IPv6 Address | 2001:0BB9:AABB:1234::10 |
| Prefix Length | 64 |
| Default Gateway | 2001:0BB9:AABB:1234::1 |

---

## P3 (LAN B)

Configure manually:

| Setting | Value |
|---------|-------|
| IPv6 Address | 2001:0BB9:CDD:5678::10 |
| Prefix Length | 64 |
| Default Gateway | 2001:0BB9:CDD:5678::1 |

---

## P4 (LAN B)

Configure:

- IPv6 Configuration → **Automatic**

Packet Tracer automatically configures the IPv6 address using SLAAC.

---

# Step 4 - Verify IPv6 Address Assignment

On PCs configured using SLAAC, verify that:

- A Global Unicast Address (GUA) has been assigned.
- A Link-Local Address (FE80::/10) has been generated.
- The default gateway has been learned automatically.

---

# Step 5 - Test IPv6 Connectivity Within LAN A

From **P1**:

```text
ping 2001:0BB9:AABB:1234::10
```

Expected Result:

- Successful replies from P2.

---

# Step 6 - Test IPv6 Connectivity Between LANs

From **P1**:

```text
ping 2001:0BB9:CDD:5678::10
```

Expected Result:

- Successful replies from P3 through Router R1.

---

# Useful Verification Commands

On Router R1:

View IPv6 interfaces:

```cisco
show ipv6 interface brief
```

View IPv6 routing table:

```cisco
show ipv6 route
```

View IPv6 neighbors:

```cisco
show ipv6 neighbors
```

Display running configuration:

```cisco
show running-config
```

---

# IPv6 Concepts Used

## Global Unicast Address (GUA)

- Publicly routable IPv6 address.
- Equivalent to a public IPv4 address.
- Prefix used in this lab:

```
2001:0BB9:AABB:1234::/64
```

and

```
2001:0BB9:CDD:5678::/64
```

---

## Link-Local Address

- Exists only on the local network segment.
- Automatically created or manually configured.
- Always begins with:

```
FE80::
```

- Used for neighbor discovery and communication on the same link.

---

## SLAAC (Stateless Address Autoconfiguration)

SLAAC allows IPv6 hosts to automatically configure:

- IPv6 Address
- Prefix Length
- Default Gateway

without requiring a DHCPv6 server.

The router advertises the network prefix using Router Advertisements (RA), allowing hosts to generate their own Global Unicast Address.

---

# Verification Summary

| Test | Expected Result |
|------|-----------------|
| P1 → P2 | Successful |
| P1 → P3 | Successful |
| Router IPv6 Interfaces | Configured |
| IPv6 Routing Table | Populated |
| SLAAC Hosts | Automatically receive IPv6 configuration |

---

# ✅ Result

Successfully configured IPv6 networking using Global Unicast Addresses (GUA) and Link-Local Addresses on a Cisco router. End devices were configured using both Static IPv6 addressing and Stateless Address Autoconfiguration (SLAAC). IPv6 connectivity was successfully verified within each LAN and between different IPv6 networks.
