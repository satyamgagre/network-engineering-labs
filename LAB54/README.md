# 🌐 LAB 54 - IPv6 SVI Inter-VLAN Routing Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure IPv6 Inter-VLAN Routing using Switch Virtual Interfaces (SVIs) on a multilayer switch. Create VLANs, assign IPv6 addresses to SVIs, configure IPv6 on hosts, and verify communication between VLANs.

---

## 🖧 Devices Used

- 1 × Cisco 3650-24PS Multilayer Switch
- 6 × Host PCs

---

# 📋 IPv6 Addressing

| VLAN | Department | IPv6 Network | SVI Link-Local |
|------|------------|--------------|----------------|
| 10 | HR | `2001:ABCD:A8::/64` | `FE80::1` |
| 20 | FIN | `2001:ABCD:B8::/64` | `FE80::2` |
| 30 | IT | `2001:ABCD:C8::/64` | `FE80::3` |

---

# Step 1 - Create VLANs and Assign Ports

On the multilayer switch:

```bash
enable
configure terminal

ipv6 unicast-routing

vlan 10
 name HR
exit

vlan 20
 name FIN
exit

vlan 30
 name IT
exit

interface range GigabitEthernet1/0/1-2
 switchport mode access
 switchport access vlan 10
exit

interface range GigabitEthernet1/0/3-4
 switchport mode access
 switchport access vlan 20
exit

interface range GigabitEthernet1/0/5-6
 switchport mode access
 switchport access vlan 30
exit

do write memory
```

---

# Step 2 - Configure IPv6 on SVIs

```bash
interface Vlan10
 ipv6 address FE80::1 link-local
 ipv6 address 2001:ABCD:A8::1/64
 no shutdown
exit

interface Vlan20
 ipv6 address FE80::2 link-local
 ipv6 address 2001:ABCD:B8::1/64
 no shutdown
exit

interface Vlan30
 ipv6 address FE80::3 link-local
 ipv6 address 2001:ABCD:C8::1/64
 no shutdown
exit

do write memory
```

---

# Step 3 - Configure IPv6 on PCs

| PC | IPv6 Address | Prefix | Default Gateway |
|----|--------------|--------|-----------------|
| P1 | `2001:ABCD:A8::5` | `/64` | `2001:ABCD:A8::1` |
| P2 | `2001:ABCD:A8::6` | `/64` | `2001:ABCD:A8::1` |
| P3 | `2001:ABCD:B8::5` | `/64` | `2001:ABCD:B8::1` |
| P4 | `2001:ABCD:B8::6` | `/64` | `2001:ABCD:B8::1` |
| P5 | `2001:ABCD:C8::5` | `/64` | `2001:ABCD:C8::1` |
| P6 | `2001:ABCD:C8::6` | `/64` | `2001:ABCD:C8::1` |

---

# Step 4 - Verify Inter-VLAN Connectivity

From any PC, test communication with devices in other VLANs.

Example:

```text
ping 2001:ABCD:C8::6
ping 2001:ABCD:B8::5
```

---

# ✅ Result

Successfully configured IPv6 Inter-VLAN Routing using Switch Virtual Interfaces (SVIs) on a multilayer switch. Devices in different VLANs can communicate successfully over IPv6.
