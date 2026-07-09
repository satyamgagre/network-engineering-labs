# LAB 66 - VoIP Dial-Peering on Voice Gateway Routers using Cisco Packet Tracer

## Devices Used

* **3 × Router 2811**
* **2 × Switch 2960-24TT**
* **4 × Cisco 7960 IP Phones**
* **4 × PCs**

> **Note:** Install the **IP_PHONE_POWER_ADAPTER** on each IP Phone before powering it on.

---

# Network Details

## SITE-A

| VLAN    | Purpose | Network         |
| ------- | ------- | --------------- |
| VLAN 10 | Data    | 10.10.10.0/24   |
| VLAN 11 | Voice   | 192.168.90.0/24 |

Extensions

* 1001
* 1002

---

## SITE-B

| VLAN    | Purpose | Network          |
| ------- | ------- | ---------------- |
| VLAN 20 | Data    | 10.10.20.0/24    |
| VLAN 21 | Voice   | 192.168.100.0/24 |

Extensions

* 2001
* 2002

---

# 1. Configure VLANs and Switch Ports

## Switch S1

```bash
enable
configure terminal

vlan 10
name DATA
exit

vlan 11
name VOICE
exit

interface range fastethernet 0/2-3
switchport mode access
switchport access vlan 10
switchport voice vlan 11
exit

do write
```

---

## Switch S2

```bash
enable
configure terminal

vlan 20
name DATA
exit

vlan 21
name VOICE
exit

interface range fastethernet 0/2-3
switchport mode access
switchport access vlan 20
switchport voice vlan 21
exit

do write
```

---

# 2. Configure Trunk Ports

## Switch S1

```bash
interface fastethernet 0/1
switchport mode trunk
exit

do write
```

---

## Switch S2

```bash
interface fastethernet 0/1
switchport mode trunk
exit

do write
```

---

# 3. Configure Router Subinterfaces

## Router SITE-A

```bash
enable
configure terminal

interface fastethernet 0/1
no shutdown
exit

interface fastethernet 0/1.10
encapsulation dot1Q 10
ip address 10.10.10.1 255.255.255.0
exit

interface fastethernet 0/1.11
encapsulation dot1Q 11
ip address 192.168.90.1 255.255.255.0
exit

do write
```

---

## Router SITE-B

```bash
enable
configure terminal

interface fastethernet 0/1
no shutdown
exit

interface fastethernet 0/1.20
encapsulation dot1Q 20
ip address 10.10.20.1 255.255.255.0
exit

interface fastethernet 0/1.21
encapsulation dot1Q 21
ip address 192.168.100.1 255.255.255.0
exit

do write
```

---

# 4. Configure DHCP

## Router SITE-A

```bash
enable
configure terminal

service dhcp

ip dhcp pool DATA
network 10.10.10.0 255.255.255.0
default-router 10.10.10.1
exit

ip dhcp pool VOICE
network 192.168.90.0 255.255.255.0
default-router 192.168.90.1
option 150 ip 192.168.90.1
exit

do write
```

---

## Router SITE-B

```bash
enable
configure terminal

service dhcp

ip dhcp pool DATA
network 10.10.20.0 255.255.255.0
default-router 10.10.20.1
exit

ip dhcp pool VOICE
network 192.168.100.0 255.255.255.0
default-router 192.168.100.1
option 150 ip 192.168.100.1
exit

do write
```

---

# 5. Configure Cisco CME (Telephony Service)

## Router SITE-A

```bash
enable
configure terminal

telephony-service
max-ephones 10
max-dn 10
ip source-address 192.168.90.1 port 2000
auto assign 1 to 10
exit

do write
```

---

## Router SITE-B

```bash
enable
configure terminal

telephony-service
max-ephones 10
max-dn 10
ip source-address 192.168.100.1 port 2000
auto assign 1 to 10
exit

do write
```

---

# 6. Configure Phone Extensions

## Router SITE-A

```bash
ephone-dn 1
number 1001
exit

ephone-dn 2
number 1002
exit
```

---

## Router SITE-B

```bash
ephone-dn 1
number 2001
exit

ephone-dn 2
number 2002
exit

do write
```

---

# 7. Configure WAN Interfaces

## Router SITE-A

```bash
enable
configure terminal

interface fastethernet 0/0
no shutdown
ip address 20.20.20.1 255.255.255.252
exit

do write
```

---

## Router ISP

```bash
enable
configure terminal

interface fastethernet 0/0
no shutdown
ip address 20.20.20.2 255.255.255.252
exit

interface fastethernet 0/1
no shutdown
ip address 30.30.30.2 255.255.255.252
exit
```

---

## Router SITE-B

```bash
enable
configure terminal

interface fastethernet 0/0
no shutdown
ip address 30.30.30.1 255.255.255.252
exit

do write
```

---

# 8. Configure OSPF Routing

## Router SITE-A

```bash
enable
configure terminal

router ospf 50
router-id 1.1.1.1

network 20.20.20.0 0.0.0.3 area 0
network 10.10.10.0 0.0.0.255 area 0
network 192.168.90.0 0.0.0.255 area 0

exit

do write
```

---

## Router ISP

```bash
enable
configure terminal

router ospf 50
router-id 2.2.2.2

network 20.20.20.0 0.0.0.3 area 0
network 30.30.30.0 0.0.0.3 area 0

exit

do write
```

> **Correction:** The original notes contain a typo: `router osfp 50`. The correct command is `router ospf 50`.

---

## Router SITE-B

```bash
enable
configure terminal

router ospf 50
router-id 3.3.3.3

network 30.30.30.0 0.0.0.3 area 0
network 10.10.20.0 0.0.0.255 area 0
network 192.168.100.0 0.0.0.255 area 0

exit

do write
```

---

# 9. Configure VoIP Dial Peers

## Router SITE-A

```bash
enable
configure terminal

dial-peer voice 1 voip
destination-pattern 2...
session target ipv4:192.168.100.1
exit

do write
```

---

## Router SITE-B

```bash
enable
configure terminal

dial-peer voice 1 voip
destination-pattern 1...
session target ipv4:192.168.90.1
exit

do write
```

---

# 10. Test Communication

### Verify IP Connectivity

```bash
ping 20.20.20.2
ping 30.30.30.1
```

### Verify OSPF

```bash
show ip route
show ip ospf neighbor
```

### Verify Phones

```bash
show ephone
show ephone-dn
show telephony-service
```

### Test Calls

* **1001 → 1002** (Local call)
* **2001 → 2002** (Local call)
* **1001 → 2001** (Inter-site call)
* **2002 → 1002** (Inter-site call)

---

# Useful Verification Commands

```bash
show ip interface brief
show ip route
show ip ospf neighbor
show telephony-service
show ephone
show ephone-dn
show dial-peer voice summary
show running-config
```

---

## Troubleshooting

* Ensure the IP Phones receive addresses from the **VOICE** DHCP pool.
* Verify **Option 150** points to the CME router (`ip source-address`).
* Confirm OSPF neighbors are in the **FULL** state before testing calls.
* Check that the `destination-pattern` matches the remote extension numbering plan (`1...` and `2...`).
* Verify the `session target` IP address is reachable from the remote router.
* If the phones do not register after configuration changes, power-cycle the IP Phones by disconnecting and reconnecting the power adapter.
