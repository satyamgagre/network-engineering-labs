
# 🧪 LAB 24: Configure Inter-VLAN Routing (Router-on-a-Stick)

## 📌 Overview

This lab demonstrates how to enable communication between multiple VLANs using a single router interface with subinterfaces (Router-on-a-Stick).

---

## 🎯 Objective

* Create VLANs on a Layer 2 switch
* Assign ports to VLANs
* Configure trunk link between switch and router
* Configure subinterfaces on router
* Enable inter-VLAN communication

---

## 🧱 Network Details

| VLAN          | Network         | Devices |
| ------------- | --------------- | ------- |
| VLAN 10 (IT)  | 192.168.10.0/24 | 2 PCs   |
| VLAN 20 (FIN) | 192.168.20.0/24 | 2 PCs   |
| VLAN 30 (HR)  | 192.168.30.0/24 | 2 PCs   |

---

## ⚙️ Configuration

### 🔹 Switch Configuration

```bash
enable
configure terminal

# Create VLANs
vlan 10
name IT
exit

vlan 20
name FIN
exit

vlan 30
name HR
exit

# Assign VLANs to ports
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

# Configure trunk port
interface fastEthernet 0/7
switchport mode trunk
exit

do wr
```

---

### 🔹 PC IP Configuration

| PC  | IP Address   | Subnet Mask   | Default Gateway |
| --- | ------------ | ------------- | --------------- |
| PC1 | 192.168.10.5 | 255.255.255.0 | 192.168.10.1    |
| PC2 | 192.168.10.6 | 255.255.255.0 | 192.168.10.1    |
| PC3 | 192.168.20.5 | 255.255.255.0 | 192.168.20.1    |
| PC4 | 192.168.20.6 | 255.255.255.0 | 192.168.20.1    |
| PC5 | 192.168.30.5 | 255.255.255.0 | 192.168.30.1    |
| PC6 | 192.168.30.6 | 255.255.255.0 | 192.168.30.1    |

---

### 🔹 Initial Test (Before Routing)

```bash
ping 192.168.30.6
```

❌ **Expected:** Ping should fail (no inter-VLAN routing yet)

---

### 🔹 Router Configuration

```bash
enable
configure terminal

interface gigaEthernet 0/0
no shutdown
exit

# Subinterface for VLAN 10
interface gigaEthernet 0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

# Subinterface for VLAN 20
interface gigaEthernet 0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

# Subinterface for VLAN 30
interface gigaEthernet 0/0.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit
```

---

## ✅ Expected Result

* PCs in different VLANs can communicate successfully
* Example:

```bash
ping 192.168.30.6
```

✔️ Ping should be **successful**


---

## 🏁 Conclusion

This lab demonstrates how a single router interface can route traffic between multiple VLANs using subinterfaces and trunking. It is a cost-effective method for inter-VLAN routing in small to medium networks.

---

