
# 🚀 LAB28: Inter-VLAN Routing + DHCP Server on Router

## 📌 Overview

This lab demonstrates **Inter-VLAN Routing using Router-on-a-Stick (ROAS)** along with **DHCP configuration on a router**.

---

## 🧱 Network Topology

* 1 Router (2911)
* 1 Multilayer Switch (3650-24PS)
* 3 Access Switches (2960-24TT)
* Each switch connects to 3 PCs

### VLANs:

| VLAN | Name | Network         |
| ---- | ---- | --------------- |
| 10   | IT   | 192.168.10.0/24 |
| 20   | HR   | 192.168.20.0/24 |
| 30   | FIN  | 192.168.30.0/24 |

---

## ⚡ Step 1: Power On MLS

* Drag AC power supply to Multilayer Switch

---

## 🔧 Step 2: Configure Access Switches

### 🔹 Switch 1 (IT)

```bash
enable
configure terminal
hostname IT-SWITCH

vlan 10
name IT
exit

interface range fastEthernet 0/2-24
switchport mode access
switchport access vlan 10
exit

interface fastEthernet 0/1
switchport mode trunk
exit

do write
```

---

### 🔹 Switch 2 (HR)

```bash
enable
configure terminal
hostname HR-SWITCH

vlan 20
name HR
exit

interface range fastEthernet 0/2-24
switchport mode access
switchport access vlan 20
exit

interface fastEthernet 0/1
switchport mode trunk
exit

do write
```

---

### 🔹 Switch 3 (FIN)

```bash
enable
configure terminal
hostname FIN-SWITCH

vlan 30
name FIN
exit

interface range fastEthernet 0/2-24
switchport mode access
switchport access vlan 30
exit

interface fastEthernet 0/1
switchport mode trunk
exit

do write
```

---

## 🔁 Step 3: Configure Multilayer Switch

```bash
enable
configure terminal

vlan 10
name IT
exit

vlan 20
name HR
exit

vlan 30
name FIN
exit

interface range gigaEthernet 1/0/1-4
switchport mode trunk
exit

do write
```

---

## 🌐 Step 4: Router Configuration (ROAS)

### Enable Interface

```bash
enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
exit
```

---

### Subinterfaces

```bash
interface gigabitEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface gigabitEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface gigabitEthernet 0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit
```

---

## 📡 Step 5: DHCP Configuration

```bash
service dhcp

ip dhcp pool IT-Pool
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit

ip dhcp pool HR-Pool
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 8.8.8.8
exit

ip dhcp pool FIN-Pool
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 8.8.8.8
exit
```

---

## 🚫 Step 6: Exclude IP Addresses

```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.100
ip dhcp excluded-address 192.168.20.1 192.168.20.100
ip dhcp excluded-address 192.168.30.1 192.168.30.100
```

---

## 💻 Step 7: Configure PCs

* Go to each PC
* Set IP Configuration to **DHCP**

---

## ✅ Expected Output

* PCs receive IP automatically:

  * VLAN 10 → 192.168.10.x
  * VLAN 20 → 192.168.20.x
  * VLAN 30 → 192.168.30.x
* Inter-VLAN communication works
* DHCP is assigned by router

---
