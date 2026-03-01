
# 🧪 Lab 02 – Basic Router Configuration (Cisco 2911)

---

## 🎯 Objective

To perform the initial configuration of a Cisco 2911 router using console access in **Cisco Packet Tracer**.

This lab covers:

* Accessing the router via console
* Securing console and VTY lines
* Setting hostname and passwords
* Configuring security policies
* Saving configuration

---

## 🖥️ Devices Used

* **1 × Cisco 2911 Router**
* **1 × PC**
* **1 × Console Cable (RS232 → Console Port)**

---

## 🔌 Physical Connection

| Device | Port  | Connected To        |
| ------ | ----- | ------------------- |
| PC     | RS232 | Router Console Port |

**Access Method:**
`PC → Desktop → Terminal → OK`

---

## ⚙️ Configuration Steps

```bash
enable
configure terminal
hostname R1

banner motd #This is a testing router#

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

clock set 01:50:10 March 03 2026

login block-for 180 attempts 3 within 50

do show running-config
do write
do show startup-config
```

---

## 🔐 Security Configured

* Console password enabled
* VTY password enabled
* Enable password configured
* Password encryption enabled
* Login block configured (Brute-force protection)
* Auto logout after 3 minutes

---

## 🛡️ Login Security Feature

```bash
login block-for 180 attempts 3 within 50
```

This means:

* If **3 failed attempts**
* Within **50 seconds**
* Login will be blocked for **180 seconds**

---

## 🧪 Verification Commands

```bash
show running-config
show startup-config
```

---

## 💾 Save Configuration

```bash
write memory
```
---

## 📌 Key Concepts Learned

* Router CLI modes
* Console configuration
* VTY configuration
* Basic router security
* Login attack prevention
* Saving configuration

