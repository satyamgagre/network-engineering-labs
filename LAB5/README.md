# DAY 5 – Trunk Allowed / Denied VLAN Configuration

**Devices Used:**

* 2 × Cisco Catalyst 2960-24TT
* 6 PCs (3 per switch)
* 2 Copper Crossover Cables (Fa0/4–Fa0/5)

---

## 📌 Lab Objective

* Create 5 VLANs on both switches
* Assign access ports to VLANs
* Configure trunk links between switches
* Allow only specific VLANs on trunk
* Test VLAN communication using `ping`

---

Switch1 Fa0/4  <------>  Switch2 Fa0/4
Switch1 Fa0/5  <------>  Switch2 Fa0/5
```

* 3 PCs connected to each switch
* Fa0/4–5 used as trunk links
* Only VLAN 10, 20, 30 allowed across trunk

---

# 🌐 IP Addressing Scheme

| PC  | VLAN | IP Address    | Subnet Mask   |
| --- | ---- | ------------- | ------------- |
| PC1 | 10   | 192.168.10.10 | 255.255.255.0 |
| PC2 | 20   | 192.168.20.10 | 255.255.255.0 |
| PC3 | 30   | 192.168.30.10 | 255.255.255.0 |
| PC4 | 10   | 192.168.10.20 | 255.255.255.0 |
| PC5 | 20   | 192.168.20.20 | 255.255.255.0 |
| PC6 | 30   | 192.168.30.20 | 255.255.255.0 |

> No default gateway required (No inter-VLAN routing configured)

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

vlan 30
 name FIN
exit

vlan 40
 name HACKER1
exit

vlan 50
 name HACKER2
exit
```

Verify:

```bash
do show vlan brief
```

---

## 2️⃣ Assign Access Ports

### 🔹 Switch 1

```bash
interface fa0/1
switchport mode access
switchport access vlan 10

interface fa0/2
switchport mode access
switchport access vlan 20

interface fa0/3
switchport mode access
switchport access vlan 30
```

---

### 🔹 Switch 2

```bash
interface fa0/1
switchport mode access
switchport access vlan 10

interface fa0/2
switchport mode access
switchport access vlan 20

interface fa0/3
switchport mode access
switchport access vlan 30
```

---

# 🔗 3️⃣ Configure Trunk Links (Fa0/4–5)

### On BOTH switches:

```bash
conf t
interface range fa0/4-5
switchport mode trunk
exit
```

Verify:

```bash
show interface trunk
```

---

# 🎯 4️⃣ Allow Specific VLANs on Trunk

Apply only on trunk interfaces (Fa0/4–5).

### On BOTH switches:

```bash
interface range fa0/4-5
switchport trunk allowed vlan 10,20,30
```

### Result:

* VLAN 10 → Allowed
* VLAN 20 → Allowed
* VLAN 30 → Allowed
* VLAN 40 → Blocked
* VLAN 50 → Blocked

---
# 🧪 5️⃣ Testing Connectivity

From **PC1 (VLAN 10):**

```bash
ping 192.168.10.20
```

✅ Should Work

From **PC1 to VLAN 20 PC:**

```bash
ping 192.168.20.20
```

❌ Will Not Work (Different VLAN – No routing configured)

---

# 🏁 Lab Completed

This lab demonstrates:

* VLAN creation
* Access port configuration
* Trunk configuration
* VLAN filtering (Allow / Deny)
* Connectivity verification

---

