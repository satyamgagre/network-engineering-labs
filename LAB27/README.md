
# ✅ LAB 27 – DHCP Server on Multilayer Switch 

## 🧠 Topology Overview

* 1 × Multilayer Switch (3650-24PS)
* 2 × Access Switches (2960-24TT)
* Each switch → 3 PCs
* VLANs / Networks:

  * IT Dept → `192.168.10.0/24`
  * HR Dept → `192.168.20.0/24`


---

# ⚙️ Step 1: Enable Layer 3 Routing

```bash
enable
configure terminal

ip routing
```

---

# ⚙️ Step 2: Configure Routed Ports (Layer 3 Interfaces)

```bash
interface range gigabitEthernet 1/0/1-2
 no switchport
 exit
```

Assign IPs:

```bash
interface gigabitEthernet 1/0/1
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface gigabitEthernet 1/0/2
 ip address 192.168.20.1 255.255.255.0
 no shutdown
 exit
```

---

# ⚙️ Step 3: Enable DHCP Service

```bash
service dhcp
```

---

# ⚙️ Step 4: Create DHCP Pools

## IT Department

```bash
ip dhcp pool IT-Dept-Pool
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
 exit
```

## HR Department

```bash
ip dhcp pool HR-Dept-Pool
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
 exit
```

---

# ⚙️ Step 5: Exclude Reserved IPs

```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.100
ip dhcp excluded-address 192.168.20.1 192.168.20.100
```

✔️ Good practice:

* `.1` = Gateway
* `.2–100` = Servers, printers, static devices

---

# 💻 Step 6: Configure PCs

On each PC:

* Go to IP configuration
* Select **DHCP**

✔️ They should receive:

* IP address
* Subnet mask
* Default gateway
* DNS

---

# 🔍 Verification Commands

On multilayer switch:

```bash
show ip dhcp binding
show ip dhcp pool
show ip interface brief
```

---
