# 🌐 LAB 40 - Multi-Area OSPF Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure Multi-Area OSPF using four Cisco routers. Establish OSPF neighbor relationships across Area 0, Area 1, and Area 2, and verify communication between all LANs.

---

## 🖧 Devices Required

* 4 × Cisco 2911 Routers
* 3 × Cisco 2960-24TT Switches
* 6 × Host PCs

### OSPF Areas

| Area   | Network |
| ------ | ------- |
| Area 0 | R1 ↔ R0 |
| Area 1 | R0 ↔ R3 |
| Area 2 | R0 ↔ R2 |

---

# 📋 IP Addressing Table

| Device | Interface | IP Address      |
| ------ | --------- | --------------- |
| R1     | G0/0      | 192.168.10.1/24 |
| R1     | G0/1      | 10.10.10.9/30   |
| R2     | G0/0      | 192.168.20.1/24 |
| R2     | G0/1      | 10.10.10.1/30   |
| R3     | G0/0      | 192.168.30.1/24 |
| R3     | G0/1      | 10.10.10.5/30   |
| R0     | G0/0      | 10.10.10.10/30  |
| R0     | G0/1      | 10.10.10.2/30   |
| R0     | G0/2      | 10.10.10.6/30   |

---

# 💻 Configure Host PCs

| PC | IP Address    | Default Gateway |
| -- | ------------- | --------------- |
| P1 | 192.168.10.10 | 192.168.10.1    |
| P2 | 192.168.10.20 | 192.168.10.1    |
| P3 | 192.168.20.10 | 192.168.20.1    |
| P4 | 192.168.20.20 | 192.168.20.1    |
| P5 | 192.168.30.10 | 192.168.30.1    |
| P6 | 192.168.30.20 | 192.168.30.1    |

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
 ip address 10.10.10.9 255.255.255.252
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
 ip address 192.168.20.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.1 255.255.255.252
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
 ip address 192.168.30.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.5 255.255.255.252
 exit

do write
```

---

## R0

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 10.10.10.10 255.255.255.252
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.2 255.255.255.252
 exit

interface gigabitEthernet 0/2
 no shutdown
 ip address 10.10.10.6 255.255.255.252
 exit

do write
```

> **Note:** The original instructions contained a typo (`interface gigaethernet 10.10.10.x`). The correct command is `ip address`.

---

# 🔄 Configure Multi-Area OSPF

## R1 (Area 0)

```cisco
enable
conf t

router ospf 10
 router-id 1.1.1.1

 network 192.168.10.0 0.0.0.255 area 0
 network 10.10.10.8 0.0.0.3 area 0

exit

do write
```

---

## R2 (Area 2)

```cisco
enable
conf t

router ospf 10
 router-id 2.2.2.2

 network 192.168.20.0 0.0.0.255 area 2
 network 10.10.10.0 0.0.0.3 area 2

exit

do write
```

---

## R3 (Area 1)

```cisco
enable
conf t

router ospf 10
 router-id 3.3.3.3

 network 192.168.30.0 0.0.0.255 area 1
 network 10.10.10.4 0.0.0.3 area 1

exit

do write
```

---

## R0 (ABR Router)

```cisco
enable
conf t

router ospf 10
 router-id 4.4.4.4

 network 10.10.10.8 0.0.0.3 area 0
 network 10.10.10.4 0.0.0.3 area 1
 network 10.10.10.0 0.0.0.3 area 2

exit

do write
```

---

# 🔍 Verify OSPF Neighbors

```cisco
show ip ospf neighbor
```

Expected neighbors:

* R1 ↔ R0
* R2 ↔ R0
* R3 ↔ R0

---

# 🔍 Verify OSPF Database

```cisco
show ip ospf database
```

---

# 🔍 Verify OSPF Routes

```cisco
show ip route ospf
```

Routes learned through OSPF will appear with:

```text
O
```

and inter-area routes will appear with:

```text
O IA
```

---

# 🧪 Test Connectivity

## Ping from P1 to P3

```bash
ping 192.168.20.10
```

---

## Ping from P1 to P5

```bash
ping 192.168.30.10
```

---

## Trace Route from P1 to P4

```bash
tracert 192.168.20.20
```

Expected Path:

```text
192.168.10.1
10.10.10.10
10.10.10.1
192.168.20.20
```

---

## Trace Route from P1 to P6

```bash
tracert 192.168.30.20
```

Expected Path:

```text
192.168.10.1
10.10.10.10
10.10.10.5
192.168.30.20
```

---

# 📊 Useful Verification Commands

Display OSPF Neighbors:

```cisco
show ip ospf neighbor
```

Display OSPF Database:

```cisco
show ip ospf database
```

Display OSPF Routes:

```cisco
show ip route ospf
```

Display OSPF Configuration:

```cisco
show running-config | section ospf
```

---

# ✅ Result

Successfully configured Multi-Area OSPF using Area 0, Area 1, and Area 2. OSPF neighbor relationships were established through the Area Border Router (R0), and all LANs exchanged routing information dynamically, enabling communication between the `192.168.10.0/24`, `192.168.20.0/24`, and `192.168.30.0/24` networks. 🚀
