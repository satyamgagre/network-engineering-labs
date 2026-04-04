
# 🔥 LAB68: Cisco ASA Firewall DHCP Configuration

## 📌 Topology Overview

* All **Inside PCs (PC0–PC4)** → connected to **Switch0 (2960)**

* **Switch0** → connected to **ASA Firewall (5506-X)**

* All **Outside Devices (PC5, PC6, XYZ Server)** → connected to **Switch1 (2960)**

* **Switch1** → connected to **ASA Firewall**

* All **DMZ Servers (Web, DNS, DHCP, Email)** → connected to **Switch2 (2960)**

* **Switch2** → connected to **ASA Firewall**

---

## ⚙️ Basic Firewall Configuration

```bash
enable
configure terminal

hostname FIREWALL

enable password 123
username admin password 123

clock set 10:00:00 3 April 2026
```

---

## 🌐 Interface Configuration

### 🟢 INSIDE Interface

```bash
interface gigabitEthernet 1/1
no shutdown
ip address 192.168.100.1 255.255.255.0
nameif INSIDE
security-level 100
exit
```

### 🔴 OUTSIDE Interface

```bash
interface gigabitEthernet 1/2
no shutdown
ip address 20.20.20.1 255.255.255.0
nameif OUTSIDE
security-level 0
exit
```

### 🟡 DMZ Interface

```bash
interface gigabitEthernet 1/3
no shutdown
ip address 10.10.10.1 255.255.255.240
nameif DMZ
security-level 70
exit
```

---

## 📡 DHCP Configuration on ASA

### 📍 Define DHCP Address Pool

```bash
dhcpd address 192.168.100.100-192.168.100.200 INSIDE
```

### 🌍 Configure DNS Server

```bash
dhcpd dns 8.8.8.8
```

### ▶️ Enable DHCP Service

```bash
dhcpd enable INSIDE
```

---

## 💾 Save Configuration

```bash
wr mem
show start
```

---

## 🖥️ Client Configuration

* Set all **Inside PCs (PC0–PC4)** to:

  * **DHCP (Automatic IP Assignment)**

---
