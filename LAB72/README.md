# 📘 LAB72 – Advanced Firewall Inspection Policies Configuration using Cisco ASA Firewall

## 📌 Topology Overview

* **INSIDE Network**: `192.168.100.0/24`
* **ASA INSIDE**: `10.10.10.1/30`
* **ASA OUTSIDE**: `20.20.20.1/30`
* **ISP Network**: `8.8.8.0/24`
* **DMZ Network**: `172.16.10.0/28`

---

# ⚙️ Step 1: Basic Configuration (ASA Firewall)

```bash
enable
configure terminal

hostname FIREWALL
enable password 123
username admin password 123

clock set 02:43:45 5 April 2026

wr mem
```

---

# 🌐 Step 2: Configure ASA Interfaces

```bash
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
ip address 172.16.10.1 255.255.255.240
no shutdown
exit
```

---

# 🖧 Step 3: Configure Inside Router

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
```

---

# 🌍 Step 4: Configure ISP Router

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
```

---

# 🔁 Step 5: Configure OSPF Routing

## 🔹 Inside Router

```bash
router ospf 10
router-id 1.1.1.1

network 192.168.100.0 0.0.0.255 area 0
network 10.10.10.0 0.0.0.3 area 0
```

## 🔹 ISP Router

```bash
router ospf 10
router-id 2.2.2.2

network 8.8.8.0 0.0.0.255 area 0
network 20.20.20.0 0.0.0.3 area 0
```

## 🔹 ASA Firewall (Packet Tracer Format)

```bash
router ospf 10
router-id 3.3.3.3

network 10.10.10.0 255.255.255.252 area 0
network 20.20.20.0 255.255.255.252 area 0
network 172.16.10.0 255.255.255.240 area 0
```

---

# 🌐 Step 6: Configure Default Route (ASA)

```bash
route OUTSIDE 0.0.0.0 0.0.0.0 20.20.20.2
```

---

# 🔁 Step 7: Configure NAT (ASA)

```bash
object network INSIDE-OUT
subnet 192.168.100.0 255.255.255.0
nat (INSIDE,OUTSIDE) dynamic interface

object network DMZ-OUT
subnet 172.16.10.0 255.255.255.240
nat (DMZ,OUTSIDE) dynamic interface
```

---

# 🔐 Step 8: Configure Advanced Firewall Policies (ACL)

## 🔹 Remove Existing ACL Policies

```bash
configure terminal

no access-group DMZ-ACCESS in interface DMZ
no access-group INTERNET-ACCESS in interface OUTSIDE

no access-list DMZ-ACCESS
no access-list INTERNET-ACCESS
```

---

## 🔹 Configure DMZ Access Policy

### Allow DHCP

```bash
access-list DMZ-ACCESS extended permit udp 192.168.100.0 255.255.255.0 host 172.16.10.10 eq 67
```

### Allow DNS

```bash
access-list DMZ-ACCESS extended permit tcp 192.168.100.0 255.255.255.0 host 172.16.10.7 eq 53

access-list DMZ-ACCESS extended permit udp 192.168.100.0 255.255.255.0 host 172.16.10.7 eq 53
```

### Allow ICMP Only for IT Administrator

```bash
access-list DMZ-ACCESS extended permit icmp host 192.168.100.15 172.16.10.0 255.255.255.240
```

### Allow Web Server Access

```bash
access-list DMZ-ACCESS extended permit tcp any host 172.16.10.9 eq 80

access-list DMZ-ACCESS extended permit tcp any host 172.16.10.9 eq 8080
```

### Apply DMZ ACL

```bash
access-group DMZ-ACCESS in interface DMZ
```

---

## 🔹 Configure Internet Access Policy

### Allow HTTP

```bash
access-list INTERNET-ACCESS extended permit tcp 192.168.100.0 255.255.255.0 host 8.8.8.8 eq 80
```

### Allow HTTP Alternate

```bash
access-list INTERNET-ACCESS extended permit tcp 192.168.100.0 255.255.255.0 host 8.8.8.8 eq 8080
```

### Allow ICMP

```bash
access-list INTERNET-ACCESS extended permit icmp 192.168.100.0 255.255.255.0 8.8.8.0 255.255.255.0
```

### Apply Internet ACL

```bash
access-group INTERNET-ACCESS in interface OUTSIDE
```

---

# 📡 Step 9: Configure DHCP (Inside Router)

```bash
ip dhcp pool DHCP-Pool
network 192.168.100.0 255.255.255.0
default-router 192.168.100.1
dns-server 172.16.10.7
```

---

# 🧪 Step 10: Testing & Verification Commands

## 🔍 Check IP Assignment (PC)

```bash
ipconfig
```

---

## 🔍 Test Connectivity

### ✔ Inside → Router

```bash
ping 192.168.100.1
```

### ✔ Inside → ASA

```bash
ping 10.10.10.1
```

### ✔ Inside → DMZ DNS Server

```bash
ping 172.16.10.7
```

### ✔ Inside → DMZ Web Server

```bash
ping 172.16.10.9
```

### ✔ Inside → Internet

```bash
ping 8.8.8.8
```

---

## 🔍 Verify OSPF

### On Router

```bash
show ip ospf neighbor
show ip route
```

### On ASA

```bash
show route
show ospf neighbor
```

---

## 🔍 Verify NAT

```bash
show nat
show xlate
```

---

## 🔍 Verify ACL Configuration

```bash
show access-list
show run access-list
show access-group
```

---

# ✅ Expected Results

* ✔ PCs receive IP addresses through DHCP.
* ✔ OSPF routes are exchanged successfully.
* ✔ Dynamic NAT is functioning correctly.
* ✔ Only authorized services (DHCP, DNS, HTTP, HTTP Alternate, ICMP) are permitted.
* ✔ Only the IT Administrator (`192.168.100.15`) can ping the DMZ network.
* ✔ Inside hosts can access the DMZ Web and DNS servers.
* ✔ Internet access is restricted according to the configured ACLs.
* ✔ Unauthorized traffic is blocked by the ASA firewall.
* ✔ ACL hit counters increase when permitted traffic matches the rules.

---
