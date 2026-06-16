# 🌐 LAB 38 - EIGRP Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure Enhanced Interior Gateway Routing Protocol (EIGRP) on four Cisco routers to dynamically exchange routing information and provide connectivity between two remote LANs through redundant paths.

---

## 🖧 Devices Required

* 4 × Cisco 2911 Routers
* 2 × Cisco 2960-24TT Switches
* 4 × Host PCs

### LANs

#### LAN 1

* 1 × Switch
* 2 × PCs
* Network: `192.168.10.0/24`

#### LAN 2

* 1 × Switch
* 2 × PCs
* Network: `192.168.20.0/24`

---

# 📋 IP Addressing Table

| Device | Interface | IP Address      |
| ------ | --------- | --------------- |
| R4     | G0/0      | 192.168.10.1/24 |
| R4     | G0/1      | 10.10.10.1/30   |
| R4     | G0/2      | 10.10.10.5/30   |
| R1     | G0/0      | 10.10.10.2/30   |
| R1     | G0/1      | 10.10.10.9/30   |
| R2     | G0/0      | 192.168.20.1/24 |
| R2     | G0/1      | 10.10.10.10/30  |
| R2     | G0/2      | 10.10.10.14/30  |
| R3     | G0/0      | 10.10.10.13/30  |
| R3     | G0/1      | 10.10.10.6/30   |

---

# 💻 Configure Host PCs

## LAN 1

| PC | IP Address    | Default Gateway |
| -- | ------------- | --------------- |
| P1 | 192.168.10.10 | 192.168.10.1    |
| P2 | 192.168.10.20 | 192.168.10.1    |

---

## LAN 2

| PC | IP Address    | Default Gateway |
| -- | ------------- | --------------- |
| P3 | 192.168.20.10 | 192.168.20.1    |
| P4 | 192.168.20.20 | 192.168.20.1    |

---

# ⚙️ Configure Router Interfaces

## R4

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
 ip address 10.10.10.5 255.255.255.252
 exit

do write
```

---

## R1

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 10.10.10.2 255.255.255.252
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
 ip address 10.10.10.10 255.255.255.252
 exit

interface gigabitEthernet 0/2
 no shutdown
 ip address 10.10.10.14 255.255.255.252
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
 ip address 10.10.10.13 255.255.255.252
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.6 255.255.255.252
 exit

do write
```

---

# 🔄 Configure EIGRP

## R4

```cisco
enable
conf t

router eigrp 10

network 192.168.10.0 0.0.0.255
network 10.10.10.0 0.0.0.3
network 10.10.10.4 0.0.0.3

exit

do write
```

---

## R1

```cisco
enable
conf t

router eigrp 10

network 10.10.10.0 0.0.0.3
network 10.10.10.8 0.0.0.3

exit

do write
```

---

## R2

```cisco
enable
conf t

router eigrp 10

network 10.10.10.8 0.0.0.3
network 10.10.10.12 0.0.0.3
network 192.168.20.0 0.0.0.255

exit

do write
```

---

## R3

```cisco
enable
conf t

router eigrp 10

network 10.10.10.4 0.0.0.3
network 10.10.10.12 0.0.0.3

exit

do write
```

---

# 🔍 Verify EIGRP Neighbors

On any router:

```cisco
show ip eigrp neighbors
```

Expected result:

* Neighbor relationships should form successfully between connected routers.

---

# 🔍 Verify EIGRP Routes

```cisco
show ip route eigrp
```

Routes learned via EIGRP will appear with:

```text
D
```

where `D` indicates a route learned through EIGRP.

---

# 🧪 Test Connectivity

From **P1**:

```bash
ping 192.168.20.10
```

Expected Result:

```text
Reply from 192.168.20.10
```

---

# 🔍 Trace the Path

From **P1**:

```bash
tracert 192.168.20.20
```

### Expected Path

The traffic may use either of the EIGRP paths depending on the metric calculation.

#### Path 1

```text
192.168.10.1
10.10.10.2
10.10.10.10
192.168.20.20
```

#### Path 2

```text
192.168.10.1
10.10.10.6
10.10.10.14
192.168.20.20
```

---

# 📊 Useful Verification Commands

Display EIGRP Neighbors:

```cisco
show ip eigrp neighbors
```

Display EIGRP Topology:

```cisco
show ip eigrp topology
```

Display EIGRP Routes:

```cisco
show ip route eigrp
```

Display Running Configuration:

```cisco
show running-config | section eigrp
```

---

# ✅ Result

Successfully configured EIGRP Autonomous System 10 on four Cisco 2911 routers. The routers dynamically exchanged routing information and established neighbor relationships, allowing communication between the `192.168.10.0/24` and `192.168.20.0/24` networks through redundant paths with automatic route selection based on EIGRP metrics. 🚀
