
# 📘 DAY 18: ACL for VTY Interfaces Configuration

## 🧠 Objective

Configure **SSH access control using ACL** so that only the **SysAdmin PC (IT Department)** can access the switch via VTY lines.

---

## 🖥️ Network Topology

* **SysAdmin PC (IT Dept)**
  IP: `192.168.1.5`
  Connected to Router `G0/0 (192.168.1.1)`

* **Router Interfaces**

  * `G0/0 → 192.168.1.1`
  * `G0/1 → 192.168.2.1`

* **Switch (Management VLAN)**

  * VLAN 1 IP: `192.168.2.254`

* **Sales PCs (via Switch)**

  * `192.168.2.5`
  * `192.168.2.6`
  * `192.168.2.7`
  * `192.168.2.8`

---

## ⚙️ Step 1: Assign IP Addresses

Assign IPs to:

* All PCs
* Router interfaces

Test connectivity:

```bash
ping <destination-ip>
```

---

## ⚙️ Step 2: Configure Management VLAN on Switch

```bash
enable
configure terminal

interface vlan 1
no shutdown
ip address 192.168.2.254 255.255.255.0

do write
exit
```

---

## ⚙️ Step 3: Configure Default Gateway on Switch

```bash
ip default-gateway 192.168.2.1
```

Test:

```bash
ping 192.168.2.1
```

---

## ⚙️ Step 4: Configure Basic Security

```bash
enable
configure terminal

hostname Test-SSH
enable password 123
ip domain-name cisco.com

username cisco password 123

do write
exit
```

---

## ⚙️ Step 5: Generate RSA Keys

```bash
crypto key generate rsa
1024
```

---

## ⚙️ Step 6: Configure VTY for SSH Only

```bash
configure terminal

line vty 0 15
login local
transport input ssh
exit
```

---

## ⚙️ Step 7: Enable SSH Version 2

```bash
ip ssh version 2
```

---

## 🔐 Step 8: Test SSH Access (Before ACL)

From any PC:

```bash
ssh -l cisco 192.168.2.254
```

Password:

```
123
```

✅ Result: All PCs can access SSH

---

## 🚫 Step 9: Configure ACL (Restrict Access)

Allow only SysAdmin PC:

```bash
configure terminal

access-list 10 permit host 192.168.1.5

line vty 0 15
access-class 10 in

exit
do write
```

---

## 🔍 Step 10: Verify ACL

### From SysAdmin PC:

```bash
ssh -l cisco 192.168.2.254
```

✅ Should work

### From Sales PCs:

```bash
ssh -l cisco 192.168.2.254
```

❌ Should fail

---

## ✅ Final Result

| Device      | SSH Access |
| ----------- | ---------- |
| SysAdmin PC | ✅ Allowed  |
| Sales PCs   | ❌ Denied   |

---
