
# 🧪 Day 3 – VLAN Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure VLANs across **2 Cisco 2960-24TT switches** and verify communication rules.

✔ Same VLAN → Communication works
✖ Different VLAN → Communication fails (no inter-VLAN routing)

---

## 🖥️ Devices Used

* 2 × Cisco 2960 Switches
* 12 × PCs

Each switch has:

| VLAN    | Department | Network         |
| ------- | ---------- | --------------- |
| VLAN 10 | IT         | 192.168.10.0/24 |
| VLAN 20 | HR         | 192.168.20.0/24 |
| VLAN 30 | FIN        | 192.168.30.0/24 |

Each VLAN → 2 PCs per switch

---

## 🌐 Step 1 – Draw Topology

* Connect PCs to both switches
* Connect **Switch1 Fa0/7 ↔ Switch2 Fa0/7** (Trunk Link)

Label VLANs:

* VLAN 10 → IT
* VLAN 20 → HR
* VLAN 30 → FIN

---

## 🖧 Step 2 – Assign IP Address to PCs

| VLAN    | PC  | IP Address    |
| ------- | --- | ------------- |
| VLAN 10 | PC1 | 192.168.10.10 |
| VLAN 10 | PC2 | 192.168.10.20 |
| VLAN 10 | PC7 | 192.168.10.30 |
| VLAN 10 | PC8 | 192.168.10.40 |

| VLAN 20 | PC3 | 192.168.20.10 |
| VLAN 20 | PC4 | 192.168.20.20 |
| VLAN 20 | PC9 | 192.168.20.30 |
| VLAN 20 | PC10 | 192.168.20.40 |

| VLAN 30 | PC5 | 192.168.30.10 |
| VLAN 30 | PC6 | 192.168.30.20 |
| VLAN 30 | PC11 | 192.168.30.30 |
| VLAN 30 | PC12 | 192.168.30.40 |

Subnet Mask for all:

```
255.255.255.0
```

---

## 🛠️ Step 3 – Create VLANs

### On Switch 1

```
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
```

---

### On Switch 2

```
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
```

---

## 🔍 Step 4 – Verify VLANs

```
show vlan brief
```

---

## 🔌 Step 5 – Assign Ports to VLANs

### Switch 1

```
interface fastEthernet 0/1
switchport mode access
switchport access vlan 10
exit

interface fastEthernet 0/2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit

interface range fastEthernet 0/5-6
switchport mode access
switchport access vlan 30
exit
```

---

### Switch 2

```
interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit

interface range fastEthernet 0/5-6
switchport mode access
switchport access vlan 30
exit
```

---

## 🔗 Step 6 – Configure Trunk Link

### Switch 1

```
interface fastEthernet 0/7
switchport mode trunk
exit
```

### Switch 2

```
interface fastEthernet 0/7
switchport mode trunk
exit
```

---

## 🧪 Step 7 – Testing

Go to PC → Desktop → Command Prompt

### Same VLAN Test ✅

```
ping 192.168.10.30
```

Result → **Success**

---

### Different VLAN Test ❌

```
ping 192.168.20.30
```

Result → **Fail**

---

## 📌 Conclusion

* VLANs successfully created
* Trunk link established
* Same VLAN devices communicate
* Different VLAN devices cannot communicate (No Router / L3 Switch)

---
