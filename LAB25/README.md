
# 📘 LAB 25: Inter-VLAN Routing Using Multilayer Switch (SVI)

## 🧠 Objective

Configure **Inter-VLAN Routing** using a **Multilayer Switch (SVI - Switch Virtual Interface)** and verify communication between different VLANs.

---

## 📌 VLAN Details

| VLAN    | Department | Network         |
| ------- | ---------- | --------------- |
| VLAN 10 | IT         | 192.168.10.0/24 |
| VLAN 20 | HR         | 192.168.20.0/24 |
| VLAN 30 | FIN        | 192.168.30.0/24 |

---

## ⚙️ Device Setup

* 1 Multilayer Switch (3560)
* 1 Access Switch (2960)
* 6 PCs (2 per VLAN)

---

## 🧩 Step 1: Configure VLANs on Multilayer Switch

```bash
enable
configure terminal
hostname MLSW

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

## 🔌 Step 2: Assign Ports to VLANs (MLSW)

```bash
interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit
```

---

## 🔧 Step 3: Configure Access Switch (VLAN 30)

```bash
enable
configure terminal

vlan 30
name FIN
exit

interface range fastEthernet 0/2-3
switchport mode access
switchport access vlan 30
exit

do write
```

---

## 🔗 Step 4: Configure Trunk Link

### On Access Switch

```bash
interface fastEthernet 0/1
switchport mode trunk
exit
```

### On Multilayer Switch

```bash
interface fastEthernet 0/5
switchport trunk encapsulation dot1q
switchport mode trunk
exit
```

---

## 💻 Step 5: Configure PC IP Addresses

### 🟣 IT Department (VLAN 10)

* PC1 → 192.168.10.5 /24 → GW: 192.168.10.1
* PC2 → 192.168.10.6 /24 → GW: 192.168.10.1

### 🟢 HR Department (VLAN 20)

* PC3 → 192.168.20.5 /24 → GW: 192.168.20.1
* PC4 → 192.168.20.6 /24 → GW: 192.168.20.1

### 🟡 FIN Department (VLAN 30)

* PC5 → 192.168.30.5 /24 → GW: 192.168.30.1
* PC6 → 192.168.30.6 /24 → GW: 192.168.30.1

---

## ❌ Step 6: Test Before Routing

```bash
ping 192.168.30.5
```

👉 Result: **Ping will FAIL**

---

## 🌐 Step 7: Configure SVI (Switch Virtual Interface)

```bash
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

interface vlan 30
ip address 192.168.30.1 255.255.255.0
no shutdown
exit
```

---

## 🚀 Step 8: Enable Routing

```bash
ip routing
```

---

## ✅ Step 9: Final Test

```bash
ping 192.168.30.5
```

👉 Result: **Ping SUCCESSFUL ✅**

