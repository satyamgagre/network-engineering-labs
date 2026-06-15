# 🌐 LAB 36 - Default Static Routes with Backup

## 🎯 Objective

Configure a primary default static route through ISP-1 and a backup floating static route through ISP-2. Verify that traffic automatically switches to the backup route when the primary link fails.

---

## 🖧 Devices Required

* 3 × Cisco 2911 Routers
* 1 × Cisco 2960-24TT Switch
* 4 × Host PCs

### Routers

* Router3 (Customer Router)
* ISP-1 Router (Primary ISP)
* ISP-2 Router (Secondary ISP)

---

# 📋 IP Addressing Table

| Device  | Interface | IP Address       |
| ------- | --------- | ---------------- |
| Router3 | G0/0      | 192.168.10.1/24  |
| Router3 | G0/1      | 200.50.10.1/30   |
| Router3 | G0/2      | 200.50.10.5/30   |
| ISP-1   | G0/0      | 200.50.10.2/30   |
| ISP-2   | G0/0      | 200.50.10.6/30   |
| PC-3    | NIC       | 192.168.10.10/24 |
| PC-4    | NIC       | 192.168.10.20/24 |

---

# 💻 Configure Host PCs

## PC-3

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.10.10 |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |

---

## PC-4

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.10.20 |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |

---

# ⚙️ Configure Router3

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 192.168.10.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 200.50.10.1 255.255.255.252
 exit

interface gigabitEthernet 0/2
 no shutdown
 ip address 200.50.10.5 255.255.255.252
 exit

do write
```

---

# ⚙️ Configure ISP-1 Router

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 200.50.10.2 255.255.255.252
 exit

do write
```

---

# ⚙️ Configure ISP-2 Router

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 200.50.10.6 255.255.255.252
 exit

do write
```

---

# 🔀 Configure Primary Default Static Route

## Router3

Configure the primary default route through ISP-1.

```cisco
enable
conf t

ip route 0.0.0.0 0.0.0.0 200.50.10.2

do write
```

---

## ISP-1 Router

Configure the return route to the customer LAN.

```cisco
enable
conf t

ip route 192.168.10.0 255.255.255.0 200.50.10.1

do write
```

---

# 🧪 Verify Primary Route

From PC-3:

```bash
ping 8.8.8.8
```

Trace the route:

```bash
tracert 8.8.8.8
```

### Expected Path

```text
192.168.10.1
200.50.10.2
```

Traffic should be forwarded through ISP-1.

---

# 🛡️ Configure Backup Floating Static Route

## Router3

Configure a backup default route through ISP-2 using Administrative Distance 130.

```cisco
enable
conf t

ip route 0.0.0.0 0.0.0.0 200.50.10.6 130

do write
```

---

## ISP-2 Router

Configure the return route to the customer LAN.

```cisco
enable
conf t

ip route 192.168.10.0 255.255.255.0 200.50.10.5

do write
```

---

# 🔧 Simulate Primary Link Failure

Disconnect the cable between:

```text
Router3 G0/1 ↔ ISP-1 G0/0
```

Or shut down the interface:

```cisco
conf t

interface gigabitEthernet 0/1
 shutdown
```

---

# 🧪 Verify Backup Route

From PC-3:

```bash
ping 8.8.8.8
```

Trace the route:

```bash
tracert 8.8.8.8
```

### Expected Backup Path

```text
192.168.10.1
200.50.10.6
```

Traffic should automatically switch to ISP-2.

---

# 🔍 Verify Routing Table

On Router3:

```cisco
show ip route
```

### When ISP-1 is Active

```text
S* 0.0.0.0/0 [1/0] via 200.50.10.2
```

### When ISP-1 Fails

```text
S* 0.0.0.0/0 [130/0] via 200.50.10.6
```

---

# ✅ Result

Successfully configured a primary default static route through ISP-1 and a backup floating static route through ISP-2. When the primary route failed, traffic automatically switched to the backup ISP, ensuring continuous connectivity and network redundancy. 🚀
