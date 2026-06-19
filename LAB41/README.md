# 🌐 LAB 41 - Border Gateway Protocol (BGP) Using Cisco Packet Tracer

## 🎯 Objective

Configure Border Gateway Protocol (BGP) between three Autonomous Systems (AS) to enable communication between two remote LAN networks through an intermediate transit Autonomous System.

---

## 🖧 Devices Required

* 3 × Cisco 2911 Routers
* 2 × Cisco 2960-24TT Switches
* 4 × Host PCs

### Autonomous Systems

| Router | Autonomous System |
| ------ | ----------------- |
| R1     | AS 20             |
| R2     | AS 35             |
| R3     | AS 50             |

---

# 📋 IP Addressing Table

| Device | Interface | IP Address      |
| ------ | --------- | --------------- |
| R1     | G0/0      | 192.168.10.1/24 |
| R1     | G0/1      | 10.10.10.2/30   |
| R2     | G0/0      | 10.10.10.1/30   |
| R2     | G0/1      | 20.20.20.1/30   |
| R3     | G0/0      | 192.168.20.1/24 |
| R3     | G0/1      | 20.20.20.2/30   |

---

# 💻 Configure Host PCs

| PC | IP Address    | Default Gateway |
| -- | ------------- | --------------- |
| P1 | 192.168.10.10 | 192.168.10.1    |
| P2 | 192.168.10.20 | 192.168.10.1    |
| P3 | 192.168.20.10 | 192.168.20.1    |
| P4 | 192.168.20.20 | 192.168.20.1    |

---

# ⚙️ Configure Router Interfaces

## R1 (AS 20)

```cisco id="0r5zvo"
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 192.168.10.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.2 255.255.255.252
 exit

do write
```

---

## R2 (AS 35)

```cisco id="5vl95f"
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 10.10.10.1 255.255.255.252
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 20.20.20.1 255.255.255.252
 exit

do write
```

---

## R3 (AS 50)

```cisco id="g8v8y7"
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 192.168.20.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 20.20.20.2 255.255.255.252
 exit

do write
```

---

# 🔄 Configure BGP

## R1 (AS 20)

```cisco id="0p4z4s"
enable
conf t

router bgp 20
 bgp router-id 1.1.1.1

 neighbor 10.10.10.1 remote-as 35

 network 10.10.10.0 mask 255.255.255.252
 network 192.168.10.0 mask 255.255.255.0

exit

do write
```

---

## R2 (AS 35)

```cisco id="t7bd86"
enable
conf t

router bgp 35
 bgp router-id 2.2.2.2

 neighbor 10.10.10.2 remote-as 20
 neighbor 20.20.20.2 remote-as 50

 network 10.10.10.0 mask 255.255.255.252
 network 20.20.20.0 mask 255.255.255.252

exit

do write
```

---

## R3 (AS 50)

```cisco id="f3bhv3"
enable
conf t

router bgp 50
 bgp router-id 3.3.3.3

 neighbor 20.20.20.1 remote-as 35

 network 192.168.20.0 mask 255.255.255.0
 network 20.20.20.0 mask 255.255.255.252

exit

do write
```

---

# 🔍 Verify BGP Neighbors

On any router:

```cisco id="25jox7"
show ip bgp neighbors
```

Verify that BGP neighbor relationships are established successfully.

---

# 🔍 Verify BGP Summary

```cisco id="pglxts"
show ip bgp summary
```

Expected output:

* Neighbor state should display a number indicating the number of prefixes received.
* The state should not remain in `Idle` or `Active`.

---

# 🔍 Verify BGP Routing Table

```cisco id="yjlwmq"
show ip bgp
```

Routes learned through BGP will appear with:

```text id="h3kq6d"
B
```

---

# 🧪 Test Connectivity

From **P1**:

```bash id="lhxllh"
ping 192.168.20.10
```

Expected Result:

```text id="5yvab5"
Reply from 192.168.20.10
```

---

# 🔍 Trace the Path

From **P1**:

```bash id="l1xgth"
tracert 192.168.20.20
```

### Expected Path

```text id="6rjlwm"
192.168.10.1
10.10.10.1
20.20.20.2
192.168.20.20
```

---

# 📊 Useful Verification Commands

Display BGP Neighbors:

```cisco id="9fdzjr"
show ip bgp neighbors
```

Display BGP Summary:

```cisco id="0eql6o"
show ip bgp summary
```

Display BGP Table:

```cisco id="wb7x9s"
show ip bgp
```

Display BGP Routes:

```cisco id="4v6qeq"
show ip route bgp
```

Display Running Configuration:

```cisco id="4f4e8e"
show running-config | section bgp
```

---

# ✅ Result

Successfully configured Border Gateway Protocol (BGP) between Autonomous Systems **AS 20**, **AS 35**, and **AS 50**. BGP neighbor relationships were established, routing information was exchanged between the autonomous systems, and end-to-end connectivity between the `192.168.10.0/24` and `192.168.20.0/24` networks was achieved. 🚀
