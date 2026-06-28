# 🌐 LAB 52 - DHCPv6 Configuration on Router Using Cisco Packet Tracer

## 🎯 Objective

Configure a Cisco router as a DHCPv6 server to automatically assign IPv6 addresses to client PCs.

---

## 🖧 Devices Used

- 1 × Cisco 2911 Router
- 1 × Cisco 2960-24TT Switch
- 5 × Host PCs

---

# 📋 Network Information

| Network | Prefix |
|---------|--------|
| LAN A | 2001:0BB9:AABB:1234::/64 |

---

# Step 1 - Enable IPv6 Routing

On the router:

```bash
enable
configure terminal

ipv6 unicast-routing
```

---

# Step 2 - Create a DHCPv6 Pool

```bash
ipv6 dhcp pool TEST
 address prefix 2001:0BB9:AABB:1234::/64
 dns-server 2001:0BB9:AABB:1234::1
 domain-name example.com
exit
```

Save the configuration:

```bash
do write memory
```

---

# Step 3 - Configure the Router Interface

```bash
interface GigabitEthernet0/0
 no shutdown
 ipv6 address FE80::3 link-local
 ipv6 address 2001:0BB9:AABB:1234::1/64

 ipv6 dhcp server TEST
 ipv6 nd managed-config-flag
exit
```

Save the configuration:

```bash
do write memory
```

---

# Step 4 - Configure the PCs

For **P1, P2, P3, P4, and P5**:

```
Desktop → IP Configuration
```

Set:

```
IPv6 Configuration → Automatic
```

Each PC should automatically receive:

- IPv6 Address
- Prefix Length
- DNS Server
- Domain Name

from the router's DHCPv6 pool.

---

# Step 5 - Verify IPv6 Address Assignment

On any PC, open:

```
Desktop → Command Prompt
```

Run:

```text
ipconfig
```

Verify that the PC has received an IPv6 address from:

```
2001:0BB9:AABB:1234::/64
```

---

# Step 6 - Verify Connectivity

From any PC:

```text
ping 2001:0BB9:AABB:1234::1
```

Expected Result:

- Successful replies from the router.

---

# ✅ Result

Successfully configured a Cisco router as a DHCPv6 server. All client PCs automatically received IPv6 configuration from the router and successfully communicated over the IPv6 network.
