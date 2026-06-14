# 🌐 LAB 34 - Static Routing Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure static routing between two remote LANs using four Cisco 2911 routers connected through point-to-point networks. Verify end-to-end connectivity using ping and traceroute.

---

## 🖧 Devices Required

* 4 × Cisco 2911 Routers
* 2 × Cisco 2960-24TT Switches
* 6 × Host PCs

### LANs

#### 🇷🇺 Russia LAN

* 1 × Switch
* 3 × PCs
* Network: `192.168.10.0/24`

#### 🇮🇳 India LAN

* 1 × Switch
* 3 × PCs
* Network: `192.168.20.0/24`

---

# 📋 IP Addressing Table

| Device     | Interface | IP Address      |
| ---------- | --------- | --------------- |
| Russia R1  | G0/0      | 192.168.10.1/24 |
| Russia R1  | G0/1      | 10.10.10.1/30   |
| England R1 | G0/0      | 10.10.10.2/30   |
| England R1 | G0/1      | 20.20.20.1/30   |
| America R1 | G0/0      | 20.20.20.2/30   |
| America R1 | G0/1      | 30.30.30.1/30   |
| India R1   | G0/0      | 30.30.30.2/30   |
| India R1   | G0/1      | 192.168.20.1/24 |

---

# ⚙️ Configure Routers

## 🇷🇺 Russia R1

```cisco
enable
conf t

interface gigabitethernet 0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface gigabitethernet 0/1
 ip address 10.10.10.1 255.255.255.252
 no shutdown
 exit

do write
```

---

## 🏴 England R1

```cisco
enable
conf t

interface gigabitethernet 0/0
 ip address 10.10.10.2 255.255.255.252
 no shutdown
 exit

interface gigabitethernet 0/1
 ip address 20.20.20.1 255.255.255.252
 no shutdown
 exit

do write
```

---

## 🇺🇸 America R1

```cisco
enable
conf t

interface gigabitethernet 0/0
 ip address 20.20.20.2 255.255.255.252
 no shutdown
 exit

interface gigabitethernet 0/1
 ip address 30.30.30.1 255.255.255.252
 no shutdown
 exit

do write
```

---

## 🇮🇳 India R1

```cisco
enable
conf t

interface gigabitethernet 0/0
 ip address 30.30.30.2 255.255.255.252
 no shutdown
 exit

interface gigabitethernet 0/1
 ip address 192.168.20.1 255.255.255.0
 no shutdown
 exit

do write
```

---

# 💻 Configure Host PCs

## Russia LAN

| PC  | IP Address    | Default Gateway |
| --- | ------------- | --------------- |
| PC0 | 192.168.10.10 | 192.168.10.1    |
| PC1 | 192.168.10.20 | 192.168.10.1    |
| PC2 | 192.168.10.30 | 192.168.10.1    |

---

## India LAN

| PC  | IP Address    | Default Gateway |
| --- | ------------- | --------------- |
| PC3 | 192.168.20.10 | 192.168.20.1    |
| PC4 | 192.168.20.20 | 192.168.20.1    |
| PC5 | 192.168.20.30 | 192.168.20.1    |

---

# 🔀 Configure Static Routes

### Note

* Configure forward routes using the next-hop IP address.
* Configure backward routes using the next-hop IP address.

---

## 🇷🇺 Russia R1

```cisco
enable
conf t

ip route 192.168.20.0 255.255.255.0 10.10.10.2

do write
```

---

## 🏴 England R1

```cisco
enable
conf t

ip route 192.168.20.0 255.255.255.0 20.20.20.2
ip route 192.168.10.0 255.255.255.0 10.10.10.1

do write
```

---

## 🇺🇸 America R1

```cisco
enable
conf t

ip route 192.168.20.0 255.255.255.0 30.30.30.2
ip route 192.168.10.0 255.255.255.0 20.20.20.1

do write
```

---

## 🇮🇳 India R1

```cisco
enable
conf t

ip route 192.168.10.0 255.255.255.0 30.30.30.1

do write
```

---

# 🧪 Test Communication

From **PC0**:

```bash
ping 192.168.20.10
```

Successful replies indicate end-to-end connectivity.

---

# 🔍 Trace the Path

From **PC0**:

```bash
tracert 192.168.20.10
```

### Expected Path

```text
192.168.10.1
10.10.10.2
20.20.20.2
30.30.30.2
192.168.20.10
```

---

# ✅ Result

Successfully configured static routing between the Russia LAN (`192.168.10.0/24`) and India LAN (`192.168.20.0/24`) through England and America routers. Verified connectivity using both `ping` and `tracert` commands. 🚀
