# 🌐 LAB 42 - Standard ACL Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure a Standard Access Control List (ACL) to permit only IT department PCs to access the server network while denying access to all other hosts.

---

## 🖧 Devices Required

* 1 × Cisco 2911 Router
* 2 × Cisco 2960-24TT Switches
* 4 × Host PCs
* 2 × Servers

---

## 📋 IP Addressing Table

### Router Interfaces

| Device | Interface | IP Address      |
| ------ | --------- | --------------- |
| Router | G0/0      | 192.168.10.1/24 |
| Router | G0/1      | 10.10.10.1/24   |

### Host PCs

| Host  | IP Address    | Default Gateway |
| ----- | ------------- | --------------- |
| IT-P1 | 192.168.10.10 | 192.168.10.1    |
| IT-P2 | 192.168.10.20 | 192.168.10.1    |
| HR-P1 | 192.168.10.30 | 192.168.10.1    |
| HR-P2 | 192.168.10.40 | 192.168.10.1    |

### Servers

| Server      | IP Address  | Default Gateway |
| ----------- | ----------- | --------------- |
| DHCP Server | 10.10.10.10 | 10.10.10.1      |
| Server      | 10.10.10.20 | 10.10.10.1      |

---

# ⚙️ Configure Router Interfaces

```cisco
enable
conf t

interface gigabitEthernet 0/0
 no shutdown
 ip address 192.168.10.1 255.255.255.0
 exit

interface gigabitEthernet 0/1
 no shutdown
 ip address 10.10.10.1 255.255.255.0
 exit

do write
```

---

# 🧪 Test Connectivity Before Applying ACL

From all PCs, verify connectivity to the server network.

```bash
ping 10.10.10.10
ping 10.10.10.20
```

Expected Result:

* All PCs should successfully reach both servers.

---

# 🔒 Configure Standard ACL

Create Standard ACL **10** to permit only IT department PCs.

> **Note:** The original configuration contained a duplicate entry. The second permit statement should allow IT-P2.

```cisco
enable
conf t

access-list 10 permit host 192.168.10.10
access-list 10 permit host 192.168.10.20
access-list 10 deny any

do write
```

---

# 🔗 Apply ACL to the Router Interface

Apply the ACL inbound on the LAN interface.

```cisco
interface gigabitEthernet 0/0
 ip access-group 10 in
 exit

do write
```

---

# 🔍 Verify ACL Configuration

Display the configured ACL:

```cisco
show access-lists
```

Expected Output:

```text
Standard IP access list 10
    permit host 192.168.10.10
    permit host 192.168.10.20
    deny any
```

---

# 🧪 Test Connectivity After Applying ACL

## From IT Department PCs

From **IT-P1** or **IT-P2**:

```bash
ping 10.10.10.10
ping 10.10.10.20
```

Expected Result:

```text
Reply from 10.10.10.10
Reply from 10.10.10.20
```

---

## From HR Department PCs

From **HR-P1** or **HR-P2**:

```bash
ping 10.10.10.10
ping 10.10.10.20
```

Expected Result:

```text
Request timed out.
```

---

# 📊 Useful Verification Commands

Display configured ACLs:

```cisco
show access-lists
```

Display interface ACL bindings:

```cisco
show running-config
```

Display interface status:

```cisco
show ip interface
```

---

# 🔐 ACL Behavior Summary

| Host  | Access to Server Network |
| ----- | ------------------------ |
| IT-P1 | ✅ Allowed                |
| IT-P2 | ✅ Allowed                |
| HR-P1 | ❌ Denied                 |
| HR-P2 | ❌ Denied                 |

---

# ✅ Result

Successfully configured a Standard ACL to permit only the IT department hosts (`192.168.10.10` and `192.168.10.20`) to access the server network while denying access to all other hosts. Access control was successfully verified through connectivity testing. 🚀
