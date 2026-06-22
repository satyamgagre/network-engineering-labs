# 🌐 LAB 44 - ACL for VTY (Remote Access) Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure SSH remote access on a Cisco router and apply a Standard Access Control List (ACL) on the VTY lines to allow only the IT department PCs to remotely access the router while denying access to the HR department PCs.

---

## 🖧 Devices Required

* 1 × Cisco 2911 Router
* 1 × Cisco 2960-24TT Switch
* 4 × Host PCs

---

## 📋 IP Addressing Table

### Router

| Device | Interface | IP Address      |
| ------ | --------- | --------------- |
| Router | G0/0      | 192.168.10.1/24 |

### Host PCs

| Host | Department | IP Address    | Default Gateway |
| ---- | ---------- | ------------- | --------------- |
| P1   | IT         | 192.168.10.10 | 192.168.10.1    |
| P2   | IT         | 192.168.10.20 | 192.168.10.1    |
| P3   | HR         | 192.168.10.30 | 192.168.10.1    |
| P4   | HR         | 192.168.10.40 | 192.168.10.1    |

---

# ⚙️ Configure Router Interface

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

end
write memory
```

---

# 💻 Configure Host PCs

Assign the following IP configuration:

| PC | IP Address    | Subnet Mask   | Default Gateway |
| -- | ------------- | ------------- | --------------- |
| P1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| P2 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1    |
| P3 | 192.168.10.30 | 255.255.255.0 | 192.168.10.1    |
| P4 | 192.168.10.40 | 255.255.255.0 | 192.168.10.1    |

---

# 🔐 Configure SSH Remote Access

```cisco
enable
configure terminal

hostname R1
ip domain-name example.com

username admin password 123

crypto key generate rsa
1024

line vty 0 15
 login local
 transport input ssh
 exit

end
write memory
```

---

# 🧪 Test SSH Before Applying ACL

From any PC:

```bash
ssh -l admin 192.168.10.1
```

Password:

```text
123
```

Expected Result:

* All PCs should be able to establish an SSH session with the router.

---

# 🔒 Configure Standard ACL for VTY Access

Create ACL **20** to permit only the IT department PCs.

```cisco
enable
configure terminal

access-list 20 permit host 192.168.10.10
access-list 20 permit host 192.168.10.20
access-list 20 deny any

end
write memory
```

---

# 🔗 Apply ACL to VTY Lines

Bind the ACL to the VTY lines using the `access-class` command.

```cisco
enable
configure terminal

line vty 0 15
 access-class 20 in
 exit

end
write memory
```

---

# 🔍 Verify ACL Configuration

Display the configured ACL.

```cisco
show access-lists
```

Expected Output:

```text
Standard IP access list 20
    permit host 192.168.10.10
    permit host 192.168.10.20
    deny any
```

---

# 🧪 Test SSH After Applying ACL

## From P1 (IT)

```bash
ssh -l admin 192.168.10.1
```

Password:

```text
123
```

✅ Expected Result:

```text
R1>
```

SSH login succeeds.

---

## From P2 (IT)

```bash
ssh -l admin 192.168.10.1
```

Password:

```text
123
```

✅ Expected Result:

SSH login succeeds.

---

## From P3 (HR)

```bash
ssh -l admin 192.168.10.1
```

Password:

```text
123
```

❌ Expected Result:

```text
% Connection refused by remote host
```

or

```text
Connection closed
```

---

## From P4 (HR)

```bash
ssh -l admin 192.168.10.1
```

Password:

```text
123
```

❌ Expected Result:

SSH access is denied.

---

# 📊 Useful Verification Commands

Display configured ACLs:

```cisco
show access-lists
```

Display VTY configuration:

```cisco
show running-config | section line vty
```

Display SSH status:

```cisco
show ip ssh
```

Display active SSH sessions:

```cisco
show ssh
```

---

# 🔐 VTY Access Summary

| Host    | SSH Access |
| ------- | ---------- |
| P1 (IT) | ✅ Allowed  |
| P2 (IT) | ✅ Allowed  |
| P3 (HR) | ❌ Denied   |
| P4 (HR) | ❌ Denied   |

---

# ✅ Result

Successfully configured SSH remote access on the Cisco router and secured the VTY lines using a Standard ACL. Only the IT department hosts (`192.168.10.10` and `192.168.10.20`) were permitted to establish SSH sessions, while the HR department hosts were denied remote access, demonstrating secure management access using VTY ACLs. 🚀
