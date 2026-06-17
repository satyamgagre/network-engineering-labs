# 🌐 LAB 39 - OSPF Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure Open Shortest Path First (OSPF) on three Cisco routers to dynamically exchange routing information and provide connectivity between three different LAN networks.

---

## 🖧 Devices Required

* 3 × Cisco 2911 Routers
* 3 × Cisco 2960-24TT Switches
* 3 × Host PCs

### LAN Networks

| LAN   | Network         |
| ----- | --------------- |
| LAN 1 | 192.168.10.0/24 |
| LAN 2 | 192.168.20.0/24 |
| LAN 3 | 192.168.30.0/24 |

---

# 📋 IP Addressing Table

| Device | Interface | IP Address      |
| ------ | --------- | --------------- |
| R1     | G0/0      | 192.168.10.1/24 |
| R1     | G0/1      | 10.10.10.1/30   |
| R1     | G0/2      | 20.20.20.1/30   |
| R2     | G0/0      | 192.168.30.1/24 |
| R2     | G0/1      | 10.10.10.2/30   |
| R2     | G0/2      | 30.30.30.1/30   |
| R3     | G0/0      | 192.168.20.1/24 |
| R3     | G0/1      | 30.30.30.2/30   |
| R3     | G0/2      | 20.20.20.2/30   |

> **Note:** R3 G0/2 should be `20.20.20.2/30` to match the topology and point-to-point link with R1.

---

# 💻 Configure Host PCs

| PC | IP Address    | Subnet Mask   | Default Gateway |
| -- | ------------- | ------------- | --------------- |
| P1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| P2 | 192.168.30.10 | 255.255.255.0 | 192.168.30.1    |
| P3 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1    |

---

# ⚙️ Configure Router Interfaces

## R1

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 192.168.10.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.1 255.255.255.252
 exit

interface gigabitEthernet 0/2
 no shutdown
 ip address 20.20.20.1 255.255.255.252
 exit

do write
```

---

## R2

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 192.168.30.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.2 255.255.255.252
 exit

interface gigabitEthernet 0/2
 no shutdown
 ip address 30.30.30.1 255.255.255.252
 exit

do write
```

---

## R3

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 192.168.20.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 30.30.30.2 255.255.255.252
 exit

interface gigabitEthernet 0/2
 no shutdown
 ip address 20.20.20.2 255.255.255.252
 exit

do write
```

---

# 🔄 Configure OSPF

## R1

```cisco
enable
conf t

router ospf 10
 router-id 1.1.1.1

 network 192.168.10.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.3 area 0
 network 20.20.20.0 0.0.0.3 area 0

exit

do write
```

---

## R2

```cisco
enable
conf t

router ospf 10
 router-id 2.2.2.2

 network 192.168.30.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.3 area 0
 network 30.30.30.0 0.0.0.3 area 0

exit

do write
```

---

## R3

```cisco
enable
conf t

router ospf 10
 router-id 3.3.3.3

 network 192.168.20.0 0.0.0.255 area 0
 network 30.30.30.0 0.0.0.3 area 0
 network 20.20.20.0 0.0.0.3 area 0

exit

do write
```

---

# 🔍 Verify OSPF Routes

On any router:

```cisco
show ip route ospf
```

Routes learned through OSPF will appear with:

```text
O
```

---

# 🔍 Verify OSPF Neighbors

```cisco
show ip ospf neighbor
```

Expected neighbors:

* R1 ↔ R2
* R1 ↔ R3
* R2 ↔ R3

---

# 🧪 Test Connectivity

## Ping from P1 to P3

```bash
ping 192.168.20.10
```

Expected Result:

```text
Reply from 192.168.20.10
```

---

## Trace Route from P1 to P3

```bash
tracert 192.168.20.10
```

Expected Path:

```text
192.168.10.1
20.20.20.2
192.168.20.10
```

---

## Ping from P1 to P2

```bash
ping 192.168.30.10
```

Expected Result:

```text
Reply from 192.168.30.10
```

---

## Trace Route from P1 to P2

```bash
tracert 192.168.30.10
```

Expected Path:

```text
192.168.10.1
10.10.10.2
192.168.30.10
```

---

# 📊 Useful Verification Commands

Display OSPF neighbors:

```cisco
show ip ospf neighbor
```

Display OSPF database:

```cisco
show ip ospf database
```

Display OSPF routes:

```cisco
show ip route ospf
```

Display OSPF configuration:

```cisco
show running-config | section ospf
```

---

# ✅ Result

Successfully configured OSPF Process ID 10 on three Cisco 2911 routers. The routers formed OSPF neighbor relationships, exchanged routing information dynamically, and enabled communication between the `192.168.10.0/24`, `192.168.20.0/24`, and `192.168.30.0/24` networks. 🚀
