# DAY 6 – 802.1Q / Native VLAN Configuration

**Devices Used:**

* 2 × Cisco Catalyst 2960-24TT
* 2 PCs (1 per switch)
* 2 Copper Crossover Cables (Fa0/2–Fa0/3)

---

## 📌 Lab Objective

* Create VLANs on both switches
* Configure trunk links between switches
* Change default Native VLAN
* Verify trunk configuration

---

Switch1 Fa0/2  <------>  Switch2 Fa0/2  
Switch1 Fa0/3  <------>  Switch2 Fa0/3

* 1 PC connected to each switch
* Fa0/2–Fa0/3 used as trunk links
* VLAN 99 configured as Native VLAN

---

# ⚙️ Configuration Steps

---

## 1️⃣ Create VLANs (On BOTH Switches)

```bash
enable
conf t

vlan 10
 name IT
exit

vlan 20
 name HR
exit

vlan 99
 name NATIVE
exit
````

Verify:

```bash
do show vlan brief
```

---

# 🔗 2️⃣ Configure Trunk Links (Fa0/2–3)

### On BOTH switches:

```bash
conf t
interface range fa0/2-3
switchport mode trunk
exit
```

Verify:

```bash
show interface trunk
```

Default Native VLAN → **1**

---

# ⚙️ 3️⃣ Configure Native VLAN

### On BOTH switches:

```bash
conf t
interface range fa0/2-3
switchport trunk native vlan 99
exit
```

---

# 🔎 4️⃣ Verify Configuration

```bash
show interface trunk
```

Expected:

```
Port        Mode   Encapsulation  Status    Native vlan
Fa0/2       trunk  802.1q         trunking  99
Fa0/3       trunk  802.1q         trunking  99
```

---

# 🏁 Lab Completed

This lab demonstrates:

* VLAN creation
* 802.1Q trunk configuration
* Native VLAN configuration
* Trunk verification

```
```
