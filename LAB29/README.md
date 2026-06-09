# 🚀 LAB 29 - Configure Inter-VLAN Routing & DHCP on a Layer 3 Switch

## 🎯 Objective

Configure Inter-VLAN Routing using a Multilayer Layer 3 Switch and provide automatic IP addressing using DHCP.

---

## 🖧 Network Topology

```text
            ML3SW (3560)
          /      |      \
        S1       S2      S3
      VLAN10   VLAN20  VLAN30
        IT       HR      FIN

Each switch has 3 PCs connected.
```

---

## 📋 VLAN Information

| 🏢 Department | 🔢 VLAN | 🌐 Network      | 🚪 Gateway   |
| ------------- | ------- | --------------- | ------------ |
| IT            | VLAN 10 | 192.168.10.0/24 | 192.168.10.1 |
| HR            | VLAN 20 | 192.168.20.0/24 | 192.168.20.1 |
| FIN           | VLAN 30 | 192.168.30.0/24 | 192.168.30.1 |

---

# 🔧 Step 1: Configure VLANs on Access Switches

## 🖥️ S1 Configuration (IT Department)

```bash
enable
configure terminal

vlan 10
 name IT
exit

interface range fa0/2-24
 switchport mode access
 switchport access vlan 10
exit

interface fa0/1
 switchport mode trunk
exit

do wr
```

---

## 🖥️ S2 Configuration (HR Department)

```bash
enable
configure terminal

vlan 20
 name HR
exit

interface range fa0/2-24
 switchport mode access
 switchport access vlan 20
exit

interface fa0/1
 switchport mode trunk
exit

do wr
```

---

## 🖥️ S3 Configuration (FIN Department)

```bash
enable
configure terminal

vlan 30
 name FIN
exit

interface range fa0/2-24
 switchport mode access
 switchport access vlan 30
exit

interface fa0/1
 switchport mode trunk
exit

do wr
```

---

# 🏗️ Step 2: Configure VLANs on Layer 3 Switch

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
```

---

# 🔗 Step 3: Configure Trunk Links

```bash
interface range g0/1-3
 switchport mode trunk
exit
```

---

# 🔄 Step 4: Enable Inter-VLAN Routing

```bash
ip routing
```

---

# 🌍 Step 5: Configure SVI Interfaces

## VLAN 10 Gateway

```bash
interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
```

## VLAN 20 Gateway

```bash
interface vlan 20
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
```

## VLAN 30 Gateway

```bash
interface vlan 30
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit
```

💾 Save Configuration

```bash
do wr
```

---

# 🚫 Step 6: Configure DHCP Excluded Addresses

```bash
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10
```

---

# 📡 Step 7: Configure DHCP Pools

```bash
service dhcp
```

## 🏢 IT Department Pool

```bash
ip dhcp pool IT-POOL
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8
exit
```

## 👥 HR Department Pool

```bash
ip dhcp pool HR-POOL
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.1
 dns-server 8.8.8.8
exit
```

## 💰 FIN Department Pool

```bash
ip dhcp pool FIN-POOL
 network 192.168.30.0 255.255.255.0
 default-router 192.168.30.1
 dns-server 8.8.8.8
exit
```

💾 Save Configuration

```bash
do wr
```

---

# 💻 Step 8: Configure PCs for DHCP

For each PC:

```text
Desktop
→ IP Configuration
→ DHCP
```

### ✅ Expected IP Assignment

| VLAN    | DHCP Range                     |
| ------- | ------------------------------ |
| VLAN 10 | 192.168.10.11 - 192.168.10.254 |
| VLAN 20 | 192.168.20.11 - 192.168.20.254 |
| VLAN 30 | 192.168.30.11 - 192.168.30.254 |

---

# 🔍 Verification Commands

## 📋 Verify VLANs

```bash
show vlan brief
```

## 🔗 Verify Trunk Links

```bash
show interfaces trunk
```

## 📡 Verify DHCP Bindings

```bash
show ip dhcp binding
```

## 🛣️ Verify Routing Table

```bash
show ip route
```

## 🌐 Verify SVI Status

```bash
show ip interface brief
```

---

# 🧪 Testing Connectivity

### 🖥️ From IT PC

```bash
ping 192.168.20.x
ping 192.168.30.x
```

### 🖥️ From HR PC

```bash
ping 192.168.10.x
ping 192.168.30.x
```

### 🖥️ From FIN PC

```bash
ping 192.168.10.x
ping 192.168.20.x
```

---

# 🎉 Result

✅ VLAN 10 (IT) Configured

✅ VLAN 20 (HR) Configured

✅ VLAN 30 (FIN) Configured

✅ Trunk Links Configured

✅ Inter-VLAN Routing Enabled

✅ DHCP Services Configured

✅ Dynamic IP Address Assignment Working

✅ End-to-End Connectivity Verified

🏆 **Lab Completed Successfully!**
