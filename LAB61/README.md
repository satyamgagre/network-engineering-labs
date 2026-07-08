# 🌐 LAB 61 - Site-to-Site IPsec VPN Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure a Site-to-Site IPsec VPN between two branch routers over an OSPF-routed WAN.

---

## 🖧 Devices Used

- 3 × Cisco 2911 Routers
- 2 × Cisco 2960-24TT Switches
- 4 × Host PCs

> **Note:** Install the **HWIC-2T** module on all routers before connecting Serial cables.

- Power off the router.
- Insert the **HWIC-2T** module.
- Power the router back on.

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

interface Serial0/0/0
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

interface Serial0/0/0
 no shutdown
 ip address 10.10.10.2 255.255.255.252
exit

interface Serial0/0/1
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

interface Serial0/0/0
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
 network 192.168.20.0 0.0.0.255 area 0
 network 20.20.20.0 0.0.0.3 area 0
exit

do write memory
```

Verify connectivity:

```text
ping 192.168.10.20
ping 192.168.20.10
```

---

# Step 3 - Enable Security Technology Package

## Router SITE-A

```bash
license boot module c2900 technology-package securityk9
do write memory
reload
```

---

## Router SITE-B

```bash
license boot module c2900 technology-package securityk9
do write memory
reload
```

Verify:

```bash
show version
```

---

# Step 4 - Configure Extended ACL

## Router SITE-A

```bash
enable
configure terminal

access-list 130 permit ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

access-list 130 permit ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255

do write memory
```

---

# Step 5 - Configure IKE Phase 1 (ISAKMP)

## Router SITE-A

```bash
enable
configure terminal

crypto isakmp policy 10
 encryption aes 256
 authentication pre-share
 group 5
exit

crypto isakmp key satya01 address 20.20.20.1

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

crypto isakmp policy 10
 encryption aes 256
 authentication pre-share
 group 5
exit

crypto isakmp key satya01 address 10.10.10.1

do write memory
```

---

# Step 6 - Configure IKE Phase 2 (IPsec)

## Router SITE-A

```bash
enable
configure terminal

crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac

crypto map VPN-MAP 10 ipsec-isakmp
 description Site-A to Site-B VPN
 set peer 20.20.20.1
 set transform-set VPN-SET
 match address 130
exit

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac

crypto map VPN-MAP 10 ipsec-isakmp
 description Site-B to Site-A VPN
 set peer 10.10.10.1
 set transform-set VPN-SET
 match address 130
exit

do write memory
```

---

# Step 7 - Apply Crypto Map

## Router SITE-A

```bash
enable
configure terminal

interface Serial0/0/0
 crypto map VPN-MAP
exit

do write memory
```

---

## Router SITE-B

```bash
enable
configure terminal

interface Serial0/0/0
 crypto map VPN-MAP
exit

do write memory
```

---

# Step 8 - Verify VPN

Check IPsec Security Associations:

```bash
show crypto ipsec sa
```

Generate interesting traffic:

```text
ping 192.168.20.10
ping 192.168.20.20
```

Check Security Associations again:

```bash
show crypto ipsec sa
```

---

# ✅ Result

Successfully configured a Site-to-Site IPsec VPN between the two branch routers. Traffic between the 192.168.10.0/24 and 192.168.20.0/24 networks is securely encrypted through the IPsec tunnel.
