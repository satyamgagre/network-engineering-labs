# LAB20: Layer 3 EtherChannel (PAgP / LACP / ON Modes)

## 🧪 Objective
Configure Layer 3 EtherChannel using:
- **LACP (active/passive)**
- **PAgP (desirable/auto)**
- **ON mode (static EtherChannel)**

---

## 🖥️ Topology
- 3 Multilayer Switches (3650-24PS)
- Each switch connected to others using **3 interfaces**
- Copper crossover cables used
- Each MLSW connected to **2 host PCs**

---

## ⚡ Step 1: Power ON Devices
- Drag AC power supply to each MLSW in Packet Tracer

---

## ⚙️ Step 2: Enable IP Routing & Set Hostnames

### MLSW1
```

enable
configure terminal
hostname MLSW1
ip routing
do wr

```

### MLSW2
```

enable
configure terminal
hostname MLSW2
ip routing
do wr

```

### MLSW3
```

enable
configure terminal
hostname MLSW3
ip routing
do wr

```

---

## 🔌 Step 3: Configure Layer 3 Interfaces

> Convert interfaces to Layer 3 using `no switchport`

### On ALL MLSWs
```

enable
configure terminal
interface range gigabitEthernet 1/0/1-6
no switchport
do wr

```

---

## 🔗 Step 4: Configure EtherChannel

---

### 🔹 MLSW1 Configuration

#### LACP (Active Mode)
```

interface range gigabitEthernet 1/0/4-6
channel-group 1 mode active
exit

interface port-channel 1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
do wr

```

#### PAgP (Desirable Mode)
```

interface range gigabitEthernet 1/0/1-3
channel-group 2 mode desirable
exit

interface port-channel 2
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
do wr

```

---

### 🔹 MLSW2 Configuration

#### PAgP (Auto Mode)
```

interface range gigabitEthernet 1/0/1-3
channel-group 2 mode auto
exit

interface port-channel 2
ip address 192.168.2.2 255.255.255.0
no shutdown
exit
do wr

```

#### Static EtherChannel (ON Mode)
```

interface range gigabitEthernet 1/0/4-6
channel-group 3 mode on
exit

interface port-channel 3
ip address 192.168.3.1 255.255.255.0
no shutdown
exit
do wr

```

---

### 🔹 MLSW3 Configuration

#### Static EtherChannel (ON Mode)
```

interface range gigabitEthernet 1/0/1-3
channel-group 3 mode on
exit

interface port-channel 3
ip address 192.168.3.2 255.255.255.0
no shutdown
exit
do wr

```

#### LACP (Passive Mode)
```

interface range gigabitEthernet 1/0/4-6
channel-group 1 mode passive
exit

interface port-channel 1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit
do wr

```

---

## 🔍 Step 5: Verification

Run the following command on any MLSW:

```

show etherchannel port

```

---

## 📌 Key Notes

- **LACP**:
  - `active` ↔ `passive`
- **PAgP**:
  - `desirable` ↔ `auto`
- **ON Mode**:
  - No negotiation (both sides must be `on`)
- Ensure:
  - Same speed, duplex, VLAN, and Layer type on all interfaces
  - Proper IP addressing on Port-Channels

---
