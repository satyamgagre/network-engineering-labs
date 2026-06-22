# 🌐 LAB 43 - Extended ACL Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure an Extended Access Control List (ACL) to allow only the IT department PCs to access the DHCP server while denying the HR department PCs. All hosts should still be able to access the Web Server.

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

| Host | Department | IP Address    | Default Gateway |
| ---- | ---------- | ------------- | --------------- |
| P1   | IT         | 192.168.10.10 | 192.168.10.1    |
| P2   | IT         | 192.168.10.20 | 192.168.10.1    |
| P3   | HR         | 192.168.10.30 | 192.168.10.1    |
| P4   | HR         | 192.168.10.40 | 192.168.10.1    |

### Servers

| Server      | IP Address  | Default Gateway |
| ----------- | ----------- | --------------- |
| Web Server  | 10.10.10.10 | 10.10.10.1      |
| DHCP Server | 10.10.10.20 | 10.10.10.1      |

---

# ⚙️ Configure Router Interfaces

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 10.10.10.1 255.255.255.0
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

# 🖥️ Configure Servers

## DHCP Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 10.10.10.20   |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 10.10.10.1    |

---

## Web Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 10.10.10.10   |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 10.10.10.1    |

---

# 🧪 Verify Connectivity Before Applying ACL

From each PC, verify connectivity to both servers.

```text
ping 10.10.10.10
ping 10.10.10.20
```

Expected Result:

* All pings should succeed before the ACL is configured.

---

# 🔒 Configure Extended ACL

**Objective**

* Permit **P1** and **P2** (IT Department) to access the **DHCP Server**.
* Deny **P3** and **P4** (HR Department) from accessing the **DHCP Server**.
* Allow all hosts to access every other destination, including the **Web Server**.

```cisco
enable
configure terminal

access-list 120 permit ip host 192.168.10.10 host 10.10.10.20
access-list 120 permit ip host 192.168.10.20 host 10.10.10.20
access-list 120 deny ip any host 10.10.10.20
access-list 120 permit ip any any

end
write memory
```

---

# 🔗 Apply the ACL

Apply the ACL inbound on the LAN interface (closest to the source).

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip access-group 120 in
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
Extended IP access list 120
    permit ip host 192.168.10.10 host 10.10.10.20
    permit ip host 192.168.10.20 host 10.10.10.20
    deny ip any host 10.10.10.20
    permit ip any any
```

---

# 🧪 Test Connectivity After Applying ACL

## From P1 (IT)

```text
ping 10.10.10.20
```

✅ Expected: Success

```text
ping 10.10.10.10
```

✅ Expected: Success

---

## From P2 (IT)

```text
ping 10.10.10.20
```

✅ Expected: Success

```text
ping 10.10.10.10
```

✅ Expected: Success

---

## From P3 (HR)

```text
ping 10.10.10.20
```

❌ Expected: Request timed out

```text
ping 10.10.10.10
```

✅ Expected: Success

---

## From P4 (HR)

```text
ping 10.10.10.20
```

❌ Expected: Request timed out

```text
ping 10.10.10.10
```

✅ Expected: Success

---

# 📊 Useful Verification Commands

Display configured ACLs:

```cisco
show access-lists
```

Display ACL applied to interfaces:

```cisco
show running-config
```

Display interface information:

```cisco
show ip interface
```

---

# 🔐 ACL Behavior Summary

| Host    | DHCP Server (10.10.10.20) | Web Server (10.10.10.10) |
| ------- | ------------------------- | ------------------------ |
| P1 (IT) | ✅ Allowed                 | ✅ Allowed                |
| P2 (IT) | ✅ Allowed                 | ✅ Allowed                |
| P3 (HR) | ❌ Denied                  | ✅ Allowed                |
| P4 (HR) | ❌ Denied                  | ✅ Allowed                |

---

# ✅ Result

Successfully configured an Extended ACL to permit only the IT department hosts (`192.168.10.10` and `192.168.10.20`) to access the DHCP Server while denying access from the HR department. All hosts retained access to the Web Server, demonstrating selective traffic filtering using Extended ACLs. 🚀
