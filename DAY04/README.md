# 🧪 Day 4 – VLAN Trunking Protocol (VTP) Configuration

## 🎯 Objective

Configure **VTP Server & Client switches** to automatically share VLAN information.

✔ VLANs created in Server → Automatically appear in Clients
✔ Reduces manual VLAN configuration

---

## 🖥️ Devices Used

* 1 × Server Switch
* 4 × Client Switches (S1, S2, S3, S4)

---

## 🌐 Step 1 – Configure Trunk Links

### On Server Switch

```bash
enable
configure terminal

interface range fastEthernet 0/1-2
switchport mode trunk
exit
```

---

### On Client Switches (S1, S2, S4)

```bash
enable
configure terminal

interface range fastEthernet 0/1-2
switchport mode trunk
exit
```

---

### On Client Switch S3

```bash
enable
configure terminal

interface range fastEthernet 0/1-3
switchport mode trunk
exit
```

---

## ⚙️ Step 2 – Configure VTP Server

```bash
enable
configure terminal

vtp mode server
vtp domain cisco.com
vtp password 123
vtp version 2
```

---

## ⚙️ Step 3 – Configure VTP Clients (S1–S4)

Run this on **each client switch**:

```bash
enable
configure terminal

vtp mode client
vtp domain cisco.com
vtp password 123
vtp version 2
```

---

## 🛠️ Step 4 – Create VLANs (Only on Server)

```bash
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
name AD
exit

vlan 50
name PRO
exit
```

---

## 🔍 Step 5 – Verify VLAN Propagation

Go to each **Client Switch** and run:

```bash
show vlan brief
```

✅ VLAN 10,20,30,40,50 should appear automatically

---

## 🧪 Step 6 – Verify VTP Status

Run on all switches:

```bash
show vtp status
```

Check:

* Same domain name
* Same version
* Configuration revision updated

---

## 📌 Result

* VLANs created only on Server
* Clients received VLANs automatically
* Trunk links enabled VLAN synchronization

---
