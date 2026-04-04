
# 🔐 LAB69 – Cisco ASA Firewall SSH Configuration

## 📌 Topology Overview

* **Inside Network:** PC0–PC4 → Switch0 → ASA Firewall
* **Outside Network:** PC5, PC6, XYZ Server → Switch1 → ASA Firewall
* **DMZ Network:** Web, DNS, DHCP, Email Servers → Switch2 → ASA Firewall

---

## ⚙️ Basic ASA Configuration

```bash
enable
configure terminal

hostname FIREWALL

enable password 123
username admin password 123

clock set 07:10:00 4 April 2026

write memory
```

---

## 🌐 Interface Configuration

### INSIDE Interface

```bash
interface gigabitEthernet 1/1
no shutdown
ip address 192.168.100.1 255.255.255.0
nameif INSIDE
security-level 100
exit
```

### OUTSIDE Interface

```bash
interface gigabitEthernet 1/2
no shutdown
ip address 20.20.20.1 255.255.255.0
nameif OUTSIDE
security-level 0
exit
```

### DMZ Interface

```bash
interface gigabitEthernet 1/3
no shutdown
ip address 10.10.10.1 255.255.255.0
nameif DMZ
security-level 70
exit
```

---

## 📡 DHCP Configuration

### Assign IP range for INSIDE network
```bash
dhcpd address 192.168.100.101-192.168.100.199 INSIDE
```

### Configure DNS server
```bash
dhcpd dns 8.8.8.8
```

### Enable DHCP service on INSIDE
```bash
dhcpd enable INSIDE
```

👉 Set all PCs to **DHCP mode** to receive IP automatically.

---

## 🔑 SSH Configuration

### Enable AAA Authentication

```bash
aaa authentication ssh console LOCAL
```

### Generate RSA Keys

```bash
crypto key generate rsa modulus 1024
```

### Allow SSH Access (INSIDE Network Only)

```bash
ssh 192.168.100.0 255.255.255.0 INSIDE
```

### Set SSH Timeout

```bash
ssh timeout 3
```

```bash
write memory
```

---

## 🧪 Verification Commands

```bash
show running-config
show ssh
show aaa authentication
```

---

## 💻 Test SSH from PC

```bash
ssh -l admin 192.168.100.1
```

Password:

```
123
```

---
