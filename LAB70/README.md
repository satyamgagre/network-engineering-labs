
# 🔥 LAB70 – Cisco ASA Firewall (OSPF + Static Routing)

## 📌 Overview

This lab demonstrates the configuration of a **Cisco ASA Firewall integrated with OSPF and Static Routing**. It includes three network zones:

* **Inside Network**
* **Outside Network**
* **DMZ Network**

---

## 🧩 Topology

### 🔹 Inside Network

* PC0–PC4 → Switch0
* Switch0 → Inside Router
* Inside Router → ASA Firewall (INSIDE)

### 🔹 Outside Network

* PC5, PC6, Server0 → Switch1
* Switch1 → ISP Router
* ISP Router → ASA Firewall (OUTSIDE)

### 🔹 DMZ Network

* Web, DNS, DHCP, Email Servers → Switch2
* Switch2 → ASA Firewall (DMZ)

---

## ⚙️ Configuration

---

## 🔐 ASA Firewall Configuration

```bash
enable
configure terminal

hostname FIREWALL
enable password 123
username admin password 123

clock set 10:45:00 Apr 4 2026

! ================= INTERFACES =================

interface gigabitEthernet 1/1
nameif INSIDE
security-level 100
ip address 10.10.10.1 255.255.255.252
no shutdown
exit

interface gigabitEthernet 1/2
nameif OUTSIDE
security-level 0
ip address 20.20.20.1 255.255.255.252
no shutdown
exit

interface gigabitEthernet 1/3
nameif DMZ
security-level 70
ip address 192.16.10.1 255.255.255.240
no shutdown
exit

! ================= OSPF =================

router ospf 10
router-id 1.1.1.3
network 10.10.10.0 255.255.255.252 area 0
network 20.20.20.0 255.255.255.252 area 0
network 192.16.10.0 255.255.255.240 area 0
exit

! ================= DEFAULT ROUTE =================

route OUTSIDE 0.0.0.0 0.0.0.0 20.20.20.2

wr mem
```

---

## 🖧 Inside Router Configuration

```bash
enable
configure terminal

hostname INSIDE-RTR

interface gigabitEthernet 0/0
ip address 192.168.100.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 10.10.10.2 255.255.255.252
no shutdown
exit

! ================= OSPF =================

router ospf 10
router-id 1.1.1.1
network 192.168.100.0 0.0.0.255 area 0
network 10.10.10.0 0.0.0.3 area 0
exit

do write
```

---

## 🌐 ISP Router Configuration

```bash
enable
configure terminal

hostname ISP-RTR

interface gigabitEthernet 0/0
ip address 8.8.8.1 255.255.255.0
no shutdown
exit

interface gigabitEthernet 0/1
ip address 20.20.20.2 255.255.255.252
no shutdown
exit

! ================= OSPF =================

router ospf 10
router-id 1.1.1.2
network 8.8.8.0 0.0.0.255 area 0
network 20.20.20.0 0.0.0.3 area 0
exit

do write
```

---

## 🧠 Routing Logic

* OSPF dynamically shares routes between:

  * Inside Router ↔ ASA Firewall
  * ASA Firewall ↔ ISP Router

* ASA uses a **default static route** to forward traffic to ISP:

```bash
0.0.0.0/0 → 20.20.20.2
```

---

## ✅ Verification Commands

### 🔹 ASA Firewall

```bash
show ospf neighbor
show route
show interface ip brief
```

### 🔹 Routers

```bash
show ip ospf neighbor
show ip route
```

---

## ⚠️ Notes

* ASA uses **subnet mask in OSPF**, not wildcard
* Routers use **wildcard masks**
* Ensure all interfaces are **no shutdown**
* Check OSPF neighbor adjacency if routing fails

---

## 🚀 Expected Result

* Inside network can reach ISP network (8.8.8.0)
* OSPF neighbors form successfully
* Traffic flows:

```
Inside → Router → ASA → ISP → Internet
```
