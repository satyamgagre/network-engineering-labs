# 🧪 Lab 01 – Basic Switch Configuration (Cisco 2960-24TT)

---

## 🎯 Objective

To perform the initial configuration of a Cisco Layer 2 switch using console access in **Cisco Packet Tracer**.

This lab covers:

* Accessing the switch via console
* Securing console and VTY lines
* Setting hostname and passwords
* Configuring management IP address
* Saving configuration

---

## 🖥️ Devices Used

* **1 × Cisco 2960-24TT Switch**
* **1 × PC**
* **1 × Console Cable (RS232 → Console Port)**

---

## 🔌 Physical Connection

| Device | Port  | Connected To        |
| ------ | ----- | ------------------- |
| PC     | RS232 | Switch Console Port |

**Access Method:**
`PC → Desktop → Terminal → OK`

---

## ⚙️ Configuration Steps

```bash
enable
configure terminal
hostname switch_1

banner motd #This is test Switch#

enable password 123

line console 0
 password 123
 login
 exec-timeout 3 0
 logging synchronous
exit

line vty 0 15
 password 123
 login
 exec-timeout 3 0
 logging synchronous
exit

no ip domain-lookup
ip domain-name cisco.com
username admin password 123
service password-encryption
exit

clock set 01:02:30 March 03 2026

configure terminal
interface vlan 1
 ip address 192.168.100.100 255.255.255.0
 no shutdown
exit

ip default-gateway 192.168.100.1

do write
do show startup-config
```

---

## 🔐 Security Configured

* Console password enabled
* VTY password enabled
* Enable password configured
* Password encryption enabled
* Auto logout after 3 minutes

---

## 🌐 Management Configuration

| Parameter       | Value           |
| --------------- | --------------- |
| Management IP   | 192.168.100.100 |
| Subnet Mask     | 255.255.255.0   |
| Default Gateway | 192.168.100.1   |

> The IP address configured on **VLAN 1** is used for remote management (Telnet/SSH/Ping).

---

## 💾 Save Configuration

```bash
write memory
```

or

```bash
copy running-config startup-config
```

---

## 📌 Key Concepts Learned

* Cisco CLI modes
* Console configuration
* VTY configuration
* Password security
* Switch management IP
* Saving configuration

---
