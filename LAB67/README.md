
# 🔥 LAB67 – Cisco ASA Firewall Basic Configuration

## 📌 Topology Overview

This lab demonstrates a basic Cisco ASA firewall setup with three zones: **INSIDE**, **OUTSIDE**, and **DMZ**.

---

## 🌐 Network Topology

### 🔹 Inside Network

* PCs **PC0 – PC4** are connected to **Switch0 (2960)**
* Switch0 is connected to the **ASA Firewall (5506-X)** via the **INSIDE interface**

### 🔹 Outside Network

* Devices **PC5, PC6, XYZ Server** are connected to **Switch1 (2960)**
* Switch1 is connected to the **ASA Firewall** via the **OUTSIDE interface**

### 🔹 DMZ Network

* Servers (**Web, DNS, DHCP, Email**) are connected to **Switch2 (2960)**
* Switch2 is connected to the **ASA Firewall** via the **DMZ interface**

---

## 🧠 Concept

* **INSIDE (Security Level 100):** Trusted network (internal users)
* **DMZ (Security Level 70):** Semi-trusted network (servers)
* **OUTSIDE (Security Level 0):** Untrusted network (internet)

---

## 🔗 Connectivity Flow

```
INSIDE PCs → Switch0 → ASA (INSIDE)

OUTSIDE Devices → Switch1 → ASA (OUTSIDE)

DMZ Servers → Switch2 → ASA (DMZ)
```

---

## ⚙️ Configuration Steps

### 🔹 Basic Setup

```bash
enable
configure terminal
hostname FIREWALL
enable password 123
username admin password 123
clock set 06:43:30 3 April 2026
```

---

### 🔹 Interface Configuration

#### INSIDE Interface

```bash
interface gigabitEthernet 1/1
no shutdown
ip address 192.168.100.1 255.255.255.0
nameif INSIDE
security-level 100
exit
```

---

#### OUTSIDE Interface

```bash
interface gigabitEthernet 1/2
no shutdown
ip address 20.20.20.1 255.255.255.0
nameif OUTSIDE
security-level 0
exit
```

---

#### DMZ Interface

```bash
interface gigabitEthernet 1/3
no shutdown
ip address 10.10.10.1 255.255.255.240
nameif DMZ
security-level 70
exit
```

---

### 💾 Save Configuration

```bash
write memory
```

---

### 🔍 Verify Configuration

```bash
show running-config
```

---
