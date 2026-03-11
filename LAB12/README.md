
# DAY 12 – Securing All Unused Ports in a Switch

## Devices Used

* 2 × Cisco **2960-24TT Switch**
* 18 PCs
* 1 **Hacker PC** (not connected initially)
* **Copper Cross-Over Cables**

---

# 📌 Lab Objective

* Create and name VLANs
* Assign VLANs to access ports
* Create a **Blackhole VLAN (999)** for security
* Disable all unused ports
* Configure trunk links between switches
* Prevent Blackhole VLAN from passing through trunk
* Test security using a **Hacker PC**

---

# 🌐 Network Topology

### Switch 1 Connections

| VLAN    | Department | Ports         | PCs   |
| ------- | ---------- | ------------- | ----- |
| VLAN 10 | IT         | Fa0/1 – Fa0/3 | 3 PCs |
| VLAN 20 | HR         | Fa0/4 – Fa0/6 | 3 PCs |
| VLAN 30 | FIN        | Fa0/7 – Fa0/9 | 3 PCs |

### Switch 2 Connections

| VLAN    | Department | Ports         | PCs   |
| ------- | ---------- | ------------- | ----- |
| VLAN 30 | FIN        | Fa0/1 – Fa0/3 | 3 PCs |
| VLAN 20 | HR         | Fa0/4 – Fa0/6 | 3 PCs |
| VLAN 10 | IT         | Fa0/7 – Fa0/9 | 3 PCs |

### Inter-Switch Connection

| Switch Ports    | Cable             |
| --------------- | ----------------- |
| Fa0/10 – Fa0/12 | Copper Cross-Over |

---

# 1️⃣ Create VLANs

## Switch 1

```bash
enable
configure terminal

vlan 10
name IT
exit

vlan 20
name HR
exit

vlan 30
name FIN
exit

do wr
```

---

## Switch 2

```bash
enable
configure terminal

vlan 10
name IT
exit

vlan 20
name HR
exit

vlan 30
name FIN
exit

do wr
```

---

# 2️⃣ Create Blackhole VLAN

A **Blackhole VLAN** is used to isolate unused ports so attackers cannot connect to the network.

## Switch 1

```bash
enable
configure terminal

vlan 999
name BLACKHOLE
exit

do wr
```

## Switch 2

```bash
enable
configure terminal

vlan 999
name BLACKHOLE
exit

do wr
```

---

# 3️⃣ Assign VLANs to Access Ports

## Switch 1

```bash
interface range fastEthernet 0/1-3
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/4-6
switchport mode access
switchport access vlan 20
exit

interface range fastEthernet 0/7-9
switchport mode access
switchport access vlan 30
exit
```

---

## Switch 2

```bash
interface range fastEthernet 0/1-3
switchport mode access
switchport access vlan 30
exit

interface range fastEthernet 0/4-6
switchport mode access
switchport access vlan 20
exit

interface range fastEthernet 0/7-9
switchport mode access
switchport access vlan 10
exit
```

---

# 4️⃣ Secure Unused Ports

All unused ports will be placed in **Blackhole VLAN (999)** and **shut down**.

## Switch 1

```bash
interface range fastEthernet 0/13-24 , gigabitEthernet 0/1-2
switchport mode access
switchport access vlan 999
shutdown
exit

do wr
```

---

## Switch 2

```bash
interface range fastEthernet 0/13-24 , gigabitEthernet 0/1-2
switchport mode access
switchport access vlan 999
shutdown
exit

do wr
```

---

# 5️⃣ Configure Trunk Links

These ports connect **Switch 1 and Switch 2**.

## Switch 1

```bash
interface range fastEthernet 0/10-12
switchport mode trunk
switchport trunk allowed vlan except 999
exit
```

---

## Switch 2

```bash
interface range fastEthernet 0/10-12
switchport mode trunk
switchport trunk allowed vlan except 999
exit
```

---

# 6️⃣ Verify Trunk

```bash
show interface trunk
```

You should see:

* Trunk ports → **Fa0/10–12**
* Allowed VLANs → **10,20,30**
* VLAN 999 **not allowed**

---

# 7️⃣ Security Test (Hacker PC)

1. Take the **Hacker PC**
2. Connect it to an **unused port (Fa0/13–24)**
3. The port is:

   * In **VLAN 999**
   * **Shutdown**

Result:

❌ Hacker PC **cannot access the network**

This demonstrates **switch port security using Blackhole VLAN + shutdown**.

---

✅ **Security Benefit**

* Prevents unauthorized device connection
* Stops VLAN hopping attempts
* Protects unused switch ports

---
