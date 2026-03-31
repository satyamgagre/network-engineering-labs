# **LAB 26 – DHCP Server Configuration on Router**

## **Topology Overview**

* **1 Router:** Cisco 2911
* **2 Switches:** 2960-24TT
* Each switch connected to **3 PCs**
* Two networks:

  * IT Department → `192.168.10.0/24`
  * HR Department → `192.168.20.0/24`

---

## **Step 1: Configure Router Interfaces**

Configure IP addresses for both networks.

```
enable
configure terminal

interface gigabitEthernet 0/0
no shutdown
ip address 192.168.10.1 255.255.255.0
exit

interface gigabitEthernet 0/1
no shutdown
ip address 192.168.20.1 255.255.255.0
exit

do write
```

---

## **Step 2: Enable DHCP Service on Router**

```
service dhcp
```

> This activates DHCP functionality on the router.

---

## **Step 3: Create DHCP Pools**

### **IT Department Pool**

```
ip dhcp pool IT-Dept-Pool
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 192.168.10.1
exit
```

### **HR Department Pool**

```
ip dhcp pool HR-Dept-Pool
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 192.168.20.1
exit
```

---

## **Step 4: Exclude Reserved IP Addresses**

Avoid assigning gateway and reserved IPs to clients:

```
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
```

---

## **Step 5: Configure PCs (Clients)**

On each PC:

* Go to **IP Configuration**
* Select **DHCP (Automatic)**

The PCs will automatically receive:

* IP Address
* Subnet Mask
* Default Gateway
* DNS Server

---

## **Verification Commands (IMPORTANT for RHCSA-style mindset / networking exams)**

Run these on router:

```
show ip dhcp binding
```

→ Displays assigned IPs

```
show ip dhcp pool
```

→ Shows pool usage

```
show running-config
```

→ Verify full configuration

---
