# 🧪 LAB 19: Overview of Layer 3 Switch

## 📌 Objective

To understand and configure **Layer 3 (Multilayer) Switching** using:

* 1 Router (Cisco 1941)
* 2 Multilayer Switches (Cisco 3650-24PS)

---

## 🖥️ Topology

* 1 Router connected to 2 Multilayer Switches
* Each switch configured with Layer 3 interfaces
* IP routing enabled on switches

---

## 🔌 Step 1: Power ON Multilayer Switch

* Drag **AC Power Supply** to turn ON the switch

---

## 🔗 Step 2: Connect Devices

* Connect router to both multilayer switches using Ethernet cables

---

## 🔁 Step 3: Enable IP Routing on Switches

### MLSW1

```bash
enable
configure terminal
ip routing
do write
```

### MLSW2

```bash
enable
configure terminal
ip routing
do write
```

---

## 🔧 Step 4: Configure Layer 3 Interfaces

> Convert switchport to routed port using `no switchport`

### MLSW1

```bash
enable
configure terminal
interface gigabitEthernet 1/0/1
no switchport
ip address 192.168.1.2 255.255.255.0
exit
do write
```

### MLSW2

```bash
enable
configure terminal
interface gigabitEthernet 1/0/1
no switchport
ip address 192.168.2.2 255.255.255.0
exit
do write
```

---

## 🌐 Step 5: Configure Router Interfaces

```bash
enable
configure terminal

interface gigabitEthernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

interface gigabitEthernet 0/1
ip address 192.168.2.1 255.255.255.0
no shutdown

exit
do write
```

---

## ✅ Step 6: Verify Connectivity (Ping Test)

### From MLSW1

```bash
ping 192.168.1.1
```

### From MLSW2

```bash
ping 192.168.2.1
```

---

## 📊 Expected Result

* MLSW1 should successfully ping Router (192.168.1.1)
* MLSW2 should successfully ping Router (192.168.2.1)
* Layer 3 communication is established

---
