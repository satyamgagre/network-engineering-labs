# 🌐 LAB 37 - Configuring Routing Information Protocol (RIP) on Cisco Packet Tracer

## 🎯 Objective

Configure RIP Version 2 on three Cisco routers to dynamically exchange routing information and enable communication between two remote LANs.

---

## 🖧 Devices Required

* 3 × Cisco 2911 Routers
* 2 × Cisco 2960-24TT Switches
* 6 × Host PCs

### LANs

#### LAN 1

* 1 × Switch
* 3 × PCs
* Network: `192.168.10.0/24`

#### LAN 2

* 1 × Switch
* 3 × PCs
* Network: `192.168.20.0/24`

---

# 📋 IP Addressing Table

| Device | Interface | IP Address      |
| ------ | --------- | --------------- |
| R1     | G0/0      | 192.168.10.1/24 |
| R1     | G0/1      | 10.10.10.1/30   |
| R2     | G0/0      | 10.10.10.2/30   |
| R2     | G0/1      | 20.20.20.1/30   |
| R3     | G0/0      | 192.168.20.1/24 |
| R3     | G0/1      | 20.20.20.2/30   |

---

# 💻 Configure Host PCs

## LAN 1 PCs

| PC | IP Address    | Default Gateway |
| -- | ------------- | --------------- |
| P1 | 192.168.10.10 | 192.168.10.1    |
| P2 | 192.168.10.20 | 192.168.10.1    |
| P3 | 192.168.10.30 | 192.168.10.1    |

---

## LAN 2 PCs

| PC | IP Address    | Default Gateway |
| -- | ------------- | --------------- |
| P4 | 192.168.20.10 | 192.168.20.1    |
| P5 | 192.168.20.20 | 192.168.20.1    |
| P6 | 192.168.20.30 | 192.168.20.1    |

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

do write
```

---

## R2

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 10.10.10.2 255.255.255.252
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 20.20.20.1 255.255.255.252
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
 ip address 20.20.20.2 255.255.255.252
 exit

do write
```

---

# 🔄 Configure RIP Version 2

## R1

```cisco
enable
conf t

router rip
 version 2
 no auto-summary

 network 192.168.10.0
 network 10.10.10.0

exit

do write
```

---

## R2

```cisco
enable
conf t

router rip
 version 2
 no auto-summary

 network 10.10.10.0
 network 20.20.20.0

exit

do write
```

---

## R3

```cisco
enable
conf t

router rip
 version 2
 no auto-summary

 network 20.20.20.0
 network 192.168.20.0

exit

do write
```

---

# 🔍 Verify RIP Configuration

On any router:

```cisco
show ip route
```

You should see routes marked with:

```text
R
```

which indicates RIP-learned routes.

---

# 🧪 Test Connectivity

From **P1**:

```bash
ping 192.168.20.30
```

Expected Result:

```text
Reply from 192.168.20.30
```

---

# 🔍 Trace the Path

From **P1**:

```bash
tracert 192.168.20.30
```

### Expected Path

```text
192.168.10.1
10.10.10.2
20.20.20.2
192.168.20.30
```

---

# 📊 Verify RIP Neighbors and Routes

Display RIP routes:

```cisco
show ip route rip
```

Display RIP configuration:

```cisco
show running-config | section rip
```

---

# ✅ Result

Successfully configured RIP Version 2 on three Cisco 2911 routers. The routers dynamically exchanged routing information, allowing hosts in the `192.168.10.0/24` and `192.168.20.0/24` networks to communicate without manually configured static routes. 🚀
