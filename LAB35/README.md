# 🌐 LAB 35 - Floating Static Routing Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure Floating Static Routing between the Russia LAN and India LAN by adding an alternate path through the Australia Router. The backup route should only be used when the primary static route becomes unavailable.

---

## 🖧 Devices Required

* 5 × Cisco 2911 Routers
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

| Device       | Interface | IP Address      |
| ------------ | --------- | --------------- |
| Russia R1    | G0/0      | 192.168.10.1/24 |
| Russia R1    | G0/1      | 10.10.10.1/30   |
| Russia R1    | G0/2      | 40.40.40.1/30   |
| England R1   | G0/0      | 10.10.10.2/30   |
| England R1   | G0/1      | 20.20.20.1/30   |
| America R1   | G0/0      | 20.20.20.2/30   |
| America R1   | G0/1      | 30.30.30.1/30   |
| India R1     | G0/0      | 30.30.30.2/30   |
| India R1     | G0/1      | 192.168.20.1/24 |
| India R1     | G0/2      | 50.50.50.2/30   |
| Australia R1 | G0/0      | 40.40.40.2/30   |
| Australia R1 | G0/1      | 50.50.50.1/30   |

---

# ⚙️ Configure Additional Interfaces

## 🇷🇺 Russia R1

```cisco
enable
conf t

interface gigabitethernet 0/2
 ip address 40.40.40.1 255.255.255.252
 no shutdown
 exit

do write
```

---

## 🇦🇺 Australia R1

```cisco
enable
conf t

interface gigabitethernet 0/0
 ip address 40.40.40.2 255.255.255.252
 no shutdown
 exit

interface gigabitethernet 0/1
 ip address 50.50.50.1 255.255.255.252
 no shutdown
 exit

do write
```

---

## 🇮🇳 India R1

```cisco
enable
conf t

interface gigabitethernet 0/2
 ip address 50.50.50.2 255.255.255.252
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

# 🔀 Existing Primary Static Routes

These routes remain configured from LAB 34 and serve as the primary path:

### Russia → England → America → India

```text
Russia → England → America → India
```

---

# 🛡️ Configure Floating Static Routes

### Note

Floating static routes use a higher Administrative Distance (AD) than the primary static route. They remain inactive until the primary route fails.

---

## 🇷🇺 Russia R1

Configure a backup route to the India LAN through Australia.

```cisco
enable
conf t

ip route 192.168.20.0 255.255.255.0 40.40.40.2 130

do write
```

---

## 🇦🇺 Australia R1

Configure the route to the India LAN.

```cisco
enable
conf t

ip route 192.168.20.0 255.255.255.0 50.50.50.2

do write
```

---

## 🇮🇳 India R1

Configure a backup route to the Russia LAN through Australia.

```cisco
enable
conf t

ip route 192.168.10.0 255.255.255.0 50.50.50.1 130

do write
```

---

# 🧪 Verify Primary Route

From **PC0**:

```bash
ping 192.168.20.10
```

Trace the route:

```bash
tracert 192.168.20.10
```

### Expected Primary Path

```text
192.168.10.1
10.10.10.2
20.20.20.2
30.30.30.2
192.168.20.10
```

---

# 🔧 Simulate Primary Link Failure

Shut down one of the primary-path interfaces.

Example on England R1:

```cisco
conf t

interface gigabitethernet 0/1
 shutdown

exit
```

---

# 🧪 Verify Floating Static Route

Run the traceroute again from PC0:

```bash
tracert 192.168.20.10
```

### Expected Backup Path

```text
192.168.10.1
40.40.40.2
50.50.50.2
192.168.20.10
```

The route should automatically switch to the Australia backup path.

---

# 🔍 Verify Routing Table

On Russia R1:

```cisco
show ip route
```

You should see:

* Primary static route when all links are operational.
* Floating static route becomes active when the primary route fails.

---

# ✅ Result

Successfully configured Floating Static Routing between the Russia LAN (`192.168.10.0/24`) and India LAN (`192.168.20.0/24`). The network automatically switched to the backup path through Australia when the primary route became unavailable, providing redundancy and improved network reliability. 🚀
