
# DAY 13 – STP PortFast, BPDU Guard, Root Guard Configuration (STP Attack Prevention)

## Topology

* **S1, S2, S3** connected to each other using **copper crossover cables**
* **S1 → 5 PCs**
* **S2 → 5 PCs**
* **S4** initially not connected
* Later **S4 connects to S3 (Fa0/4)**

Purpose:
Prevent **STP attacks and loops** using PortFast, BPDU Guard, and Root Guard.

---

# 1️⃣ Configure Trunk Ports (Between Switches)

### S1

```
enable
configure terminal
interface range fastEthernet 0/1-2
switchport mode trunk
exit
do write
```

### S2

```
enable
configure terminal
interface range fastEthernet 0/1-2
switchport mode trunk
exit
do write
```

### S3

```
enable
configure terminal
interface range fastEthernet 0/1-2
switchport mode trunk
exit
do write
```

These ports are used **between switches**, so they must be **trunk ports**.

---

# 2️⃣ Configure Access Ports with PortFast and BPDU Guard

These ports connect to **PCs**, so we enable **PortFast** and **BPDU Guard**.

## S1

```
enable
configure terminal

interface range fastEthernet 0/3-24, gigabitEthernet 0/1-2
switchport mode access
spanning-tree portfast
spanning-tree bpduguard enable
exit

spanning-tree portfast default
spanning-tree portfast bpduguard default
do write
```

---

## S2

```
enable
configure terminal

interface range fastEthernet 0/3-24, gigabitEthernet 0/1-2
switchport mode access
spanning-tree portfast
spanning-tree bpduguard enable
exit

spanning-tree portfast default
spanning-tree portfast bpduguard default
do write
```

### Result

* PCs connect **immediately (no STP delay)**
* If someone connects a **switch to a PC port**, the port will **shutdown automatically**

---

# 3️⃣ Configure Root Guard

Purpose:
Prevent **another switch from becoming root bridge**.

Now connect:

```
S4 → S3 using copper straight-through cable
Interface: Fa0/4
```

### On S3

```
enable
configure terminal

interface fastEthernet 0/4
switchport mode trunk
spanning-tree guard root
exit

do write
```

### Result

If **S4 tries to become Root Bridge**, the port will go to **root-inconsistent state**.

---

# 4️⃣ Verification Commands

### Check PortFast status

```
show spanning-tree interface fastEthernet 0/4 portfast
```

### Check STP summary

```
show spanning-tree summary
```

---

# 5️⃣ Testing

Test the following:

✔ Connect a **PC to access port**
→ Should connect **immediately (PortFast)**

✔ Connect a **switch to access port**
→ Port should **shutdown (BPDU Guard)**

✔ Connect **S4 to S3 Fa0/4**
→ If S4 sends **superior BPDU**, port becomes **root-inconsistent (Root Guard)**

---

✅ **What this lab demonstrates**

* Faster PC connectivity (**PortFast**)
* Protection from **unauthorized switches (BPDU Guard)**
* Prevent **root bridge manipulation (Root Guard)**
* Basic **STP attack prevention**

---
