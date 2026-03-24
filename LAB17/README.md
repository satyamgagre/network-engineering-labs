
# 🔐 Day 17 – Port Security Configuration (Cisco Switch)

## 📌 Topology Overview

* **1 Switch**
* **5 Legitimate PCs**
* **1 Hacker PC (for testing)**

### IP Address Allocation

| PC  | IP Address  |
| --- | ----------- |
| PC1 | 192.168.1.1 |
| PC2 | 192.168.1.2 |
| PC3 | 192.168.1.3 |
| PC4 | 192.168.1.4 |
| PC5 | 192.168.1.5 |

---

## ⚙️ Objectives

* Configure **Port Security**
* Use **MAC Sticky / Manual**
* Apply different **violation modes**
* Limit number of devices per port
* Test with unauthorized (Hacker) PC

---

## 🧩 Batch-wise Configuration

---

## 🔹 BATCH 1 (Fa0/1 – Fa0/2)

### Requirements:

* Max devices: **1 per port**
* MAC learning: **Sticky**
* Violation mode: **Shutdown**

### Configuration:

```bash
enable
configure terminal

interface range fastEthernet 0/1-2
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
exit

do write memory
```

---

## 🔹 BATCH 2 (Fa0/3 – Fa0/4)

### Requirements:

* Max devices: **2 per port**
* MAC learning: **Sticky**
* Violation mode: **Restrict**

### Configuration:

```bash
interface range fastEthernet 0/3-4
switchport mode access
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
switchport port-security violation restrict
exit

do write memory
```

---

## 🔹 BATCH 3 (Fa0/5)

### Requirements:

* Max devices: **1**
* MAC type: **Manual**
* Violation mode: **Protect**

### Configuration:

```bash
interface fastEthernet 0/5
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address 0007.EC75.1B64
switchport port-security violation protect
exit

do write memory
```

---

## 🖥️ PC Configuration

Assign IP addresses manually:

```bash
PC1 → 192.168.1.1
PC2 → 192.168.1.2
PC3 → 192.168.1.3
PC4 → 192.168.1.4
PC5 → 192.168.1.5
```

---

## 🔍 Verification Commands

```bash
show running-config
show port-security
show port-security interface fa0/1
```

---

## 🧪 Testing (Hacker Simulation)

### Steps:

1. Connect **Hacker PC** to switch
2. Assign IP:

```bash
192.168.1.5
```

3. Try to ping other PCs

---

## ✅ Expected Results

| Batch   | Behavior                                  |
| ------- | ----------------------------------------- |
| Batch 1 | Port goes **shutdown** on violation       |
| Batch 2 | Packets **dropped + logged**              |
| Batch 3 | Unauthorized traffic **silently ignored** |

---

## 🎯 Conclusion

* **Shutdown mode** = most strict (port disabled)
* **Restrict mode** = drops + logs violations
* **Protect mode** = drops silently (no logs)
* **Sticky MAC** = auto-learns MAC
* **Manual MAC** = highest control (most secure)
