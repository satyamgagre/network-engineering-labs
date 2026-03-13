
# 🔐 LAB 14 – VLAN Hopping Attack Prevention

### Using `switchport nonegotiate` and Disabling `CDP`

## 📌 Overview

**VLAN Hopping** is a network attack where an attacker tries to access traffic from another VLAN on the same switch.

The attacker sends specially crafted frames to **bypass VLAN segmentation** and gain access to restricted network resources.

---

# ⚠️ Types of VLAN Hopping Attacks

### 1️⃣ Double Tagging

* Attacker sends frames with **two VLAN tags**
* First switch removes the first tag
* Second switch forwards traffic to another VLAN

### 2️⃣ Switch Spoofing

* Attacker pretends to be a **switch**
* Negotiates a **trunk link**
* Gains access to **multiple VLANs**

---

# 🛡️ Prevention Techniques

* Do **not use VLAN 1 for hosts**
* Change **Native VLAN to unused VLAN**
* Disable **DTP negotiation**
* Disable **CDP**

---

# ⚙️ Step 1 – Create VLANs

## S1

```bash
enable
configure terminal
vlan 10
name BYOD
exit
vlan 100
name NATIVE
exit
do wr
```

## S2

```bash
enable
configure terminal
vlan 20
name CORPS-USERS
exit
vlan 100
name NATIVE
exit
do wr
```

## S3

```bash
enable
configure terminal
vlan 30
name DEVELOPERS
exit
vlan 100
name NATIVE
exit
do wr
```

---

# ⚙️ Step 2 – Configure Access Ports

## S1

```bash
enable
configure terminal
interface range fastEthernet 0/5-24
switchport mode access
switchport access vlan 10
switchport nonegotiate
exit
```

## S2

```bash
enable
configure terminal
interface range fastEthernet 0/5-24
switchport mode access
switchport access vlan 20
switchport nonegotiate
exit
```

## S3

```bash
enable
configure terminal
interface range fastEthernet 0/5-24
switchport mode access
switchport access vlan 30
switchport nonegotiate
exit
```

---

# ⚙️ Step 3 – Configure Trunk Ports

## S1

```bash
enable
configure terminal
interface range fastEthernet 0/1-4
switchport mode trunk
switchport trunk native vlan 100
switchport nonegotiate
exit
```

## S2

```bash
enable
configure terminal
interface range fastEthernet 0/1-4
switchport mode trunk
switchport trunk native vlan 100
switchport nonegotiate
exit
```

## S3

```bash
enable
configure terminal
interface range fastEthernet 0/1-4
switchport mode trunk
switchport trunk native vlan 100
switchport nonegotiate
exit
```

---

# ⚙️ Step 4 – Disable CDP

## On All Switches

```bash
enable
configure terminal
no cdp run
do wr
```

---

# 🔎 Why This Prevents VLAN Hopping

### `switchport nonegotiate`

* Disables **DTP**
* Prevents **Switch Spoofing Attack**

### Native VLAN changed to VLAN 100

* Prevents **Double Tagging Attack**

### Hosts not in VLAN 1

* VLAN 1 is **default and commonly targeted**

### CDP Disabled

* Prevents attacker from learning **network device information**

---

# ✅ Lab Outcome

After completing this lab:

* VLAN hopping attacks are **mitigated**
* Trunk negotiation is **disabled**
* Native VLAN is **secured**
* Switch information leakage is **prevented**

---
