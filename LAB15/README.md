# 🔐 LAB 15 – DHCP Snooping Configuration

### Protecting Network from Rogue DHCP Servers

## 📌 Overview

**DHCP Snooping** is a **Layer 2 security feature** on switches that protects the network from **unauthorized DHCP servers**.

It allows DHCP responses only from **trusted ports** and blocks DHCP replies coming from **untrusted ports**.

This helps ensure that clients receive IP addresses only from **legitimate DHCP servers**.

---

# 🛡️ Why DHCP Snooping is Important

* Blocks **Rogue DHCP Servers**
* Prevents **Man-in-the-Middle attacks**
* Ensures clients receive IP from **authorized DHCP server**
* Protects **internal organizational networks**
* Improves **network security and integrity**

---

# 🌐 Network Topology

Devices used in this lab:

* **Switch:** Cisco 2960-24TT  
* **Router:** ISR4331  
* **Legitimate DHCP Server**  
* **Fake DHCP Server**  
* **3 Host PCs**

⚠️ The fake DHCP server exists but **is not connected initially**.

---

# ⚙️ Step 1 – Configure Router Interface

## Router1 (R1)

```bash
enable
configure terminal
interface gigabitEthernet 0/0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
````

---

# ⚙️ Step 2 – Configure Legitimate DHCP Server

## Server Network Configuration

```
IP Address: 192.168.10.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
```

## DHCP Pool Configuration

```
Pool Name: DHCPPool
Default Gateway: 192.168.10.1
DNS Server: 0.0.0.0
Start IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Maximum Users: 100
```

---

# ⚙️ Step 3 – Configure Fake DHCP Server

## Server Network Configuration

```
IP Address: 192.168.10.100
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
```

## DHCP Pool Configuration

```
Pool Name: DHCPPool
Default Gateway: 192.168.10.1
DNS Server: 0.0.0.0
Start IP Address: 192.168.10.10
Subnet Mask: 255.255.255.0
Maximum Users: 100
```

---

# ⚙️ Step 4 – Configure PCs to Use DHCP

On each **Host PC**:

1. Open **IP Configuration**
2. Select **DHCP**

The PCs will automatically receive an IP address from the DHCP server.

---

# ⚙️ Step 5 – Enable DHCP Snooping on the Switch

## Switch1

```bash
enable
configure terminal
ip dhcp snooping
```

---

# ⚙️ Step 6 – Configure Trusted Ports

Ports connected to the **router and legitimate DHCP server** should be marked as **trusted ports**.

## Switch1

```bash
enable
configure terminal
interface range fastEthernet 0/4-5
ip dhcp snooping trust
exit
do wr
```

---

# ⚙️ Step 7 – Enable DHCP Snooping for VLAN

Assign DHCP Snooping to the VLAN currently being used.

## Switch1

```bash
enable
configure terminal
ip dhcp snooping vlan 1
```

---

# 🔎 How DHCP Snooping Works

### Trusted Ports

* Allowed to send **DHCP responses**
* Typically connected to **DHCP server or router**

### Untrusted Ports

* Connected to **host devices**
* DHCP responses from these ports are **blocked**

---

# 🚫 What Happens with a Rogue DHCP Server

If a **fake DHCP server** is connected to an **untrusted port**:

* The switch **blocks DHCP offer packets**
* Clients **cannot receive IP addresses** from the rogue server

---

# ✅ Lab Outcome

After completing this lab:

* Rogue DHCP servers are **blocked**
* Only **trusted interfaces** can send DHCP responses
* Clients receive IP addresses only from the **legitimate DHCP server**
* Network security is **significantly improved**

---

