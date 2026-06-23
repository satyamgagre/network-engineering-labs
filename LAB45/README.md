# 🌐 LAB 45 - Configuring Static NAT Using Cisco Packet Tracer

## 🎯 Objective

Configure Static Network Address Translation (Static NAT) to provide a one-to-one mapping between an internal private IP address and a public IP address. Verify connectivity and NAT translations between the internal network and the external network.

---

## 🖧 Devices Required

* 2 × Cisco 2911 Routers
* 1 × Cisco 2960-24TT Switch
* 2 × Host PCs
* 1 × Server

---

## 📋 IP Addressing Table

### NAT Router

| Interface      | IP Address      |
| -------------- | --------------- |
| G0/0 (Inside)  | 192.168.10.1/24 |
| G0/1 (Outside) | 200.100.50.1/30 |

### ISP Router

| Interface | IP Address      |
| --------- | --------------- |
| G0/0      | 8.8.8.1/24      |
| G0/1      | 200.100.50.2/30 |

### Host PCs

| Host | IP Address    | Default Gateway |
| ---- | ------------- | --------------- |
| P1   | 192.168.10.10 | 192.168.10.1    |
| P2   | 192.168.10.20 | 192.168.10.1    |

### Server

| Server        | IP Address | Default Gateway |
| ------------- | ---------- | --------------- |
| Google Server | 8.8.8.8    | 8.8.8.1         |

---

# ⚙️ Configure Router Interfaces

## NAT Router

> **Note:** The original instructions configured `GigabitEthernet0/0` twice. The outside interface should be `GigabitEthernet0/1`.

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 200.100.50.1 255.255.255.252
 no shutdown
 exit

end
write memory
```

---

## ISP Router

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 8.8.8.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 200.100.50.2 255.255.255.252
 no shutdown
 exit

end
write memory
```

---

# 💻 Configure Host PCs

| PC | IP Address    | Subnet Mask   | Default Gateway |
| -- | ------------- | ------------- | --------------- |
| P1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| P2 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1    |

---

# 🖥️ Configure Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 8.8.8.8       |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 8.8.8.1       |

---

# 🔄 Configure OSPF

## NAT Router

```cisco
enable
configure terminal

router ospf 10
 router-id 1.1.1.1

 network 192.168.10.0 0.0.0.255 area 0
 network 200.100.50.0 0.0.0.3 area 0

exit

end
write memory
```

---

## ISP Router

```cisco
enable
configure terminal

router ospf 10
 router-id 2.2.2.2

 network 200.100.50.0 0.0.0.3 area 0
 network 8.8.8.0 0.0.0.255 area 0

exit

end
write memory
```

---

# 🧪 Verify Connectivity Before NAT

From **P1**:

```text
ping 8.8.8.8
```

```text
tracert 8.8.8.8
```

Check for existing NAT translations:

```cisco
show ip nat translations
```

Expected Result:

* Connectivity may not work as expected.
* No NAT translations should be displayed.

---

# 🔒 Configure Static NAT

> **Note:** Your topology uses the public network `200.100.50.0/30`. The original command mapped the inside host to `200.50.50.1`, which appears to be a typo. Use the public IP assigned to the NAT router's outside interface.

```cisco
enable
configure terminal

ip nat inside source static 192.168.10.10 200.100.50.1

end
write memory
```

---

# 🌐 Configure NAT Inside and Outside Interfaces

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip nat inside
 exit

interface GigabitEthernet0/1
 ip nat outside
 exit

end
write memory
```

---

# 🔍 Verify NAT Configuration

Display the configured NAT translations.

```cisco
show ip nat translations
```

Expected Output:

```text
Pro Inside global      Inside local       Outside local      Outside global
--- 200.100.50.1       192.168.10.10      ---                ---
```

---

# 🧪 Test Connectivity After NAT

From **P1**:

```text
ping 8.8.8.8
```

Expected Result:

```text
Reply from 8.8.8.8
```

---

Trace the path:

```text
tracert 8.8.8.8
```

Expected Path:

```text
192.168.10.1
200.100.50.2
8.8.8.8
```

---

Display the active NAT translations:

```cisco
show ip nat translations
```

---

# 📊 Useful Verification Commands

Display NAT translations:

```cisco
show ip nat translations
```

Display NAT statistics:

```cisco
show ip nat statistics
```

Display OSPF neighbors:

```cisco
show ip ospf neighbor
```

Display routing table:

```cisco
show ip route
```

Display interface NAT roles:

```cisco
show running-config
```

---

# 🔐 Static NAT Mapping

| Private IP    | Public IP    |
| ------------- | ------------ |
| 192.168.10.10 | 200.100.50.1 |

---

# ✅ Result

Successfully configured Static NAT to provide a one-to-one mapping between the internal host (`192.168.10.10`) and the public IP address (`200.100.50.1`). After configuring NAT inside/outside interfaces, the internal host was able to communicate with the external server (`8.8.8.8`), and the NAT translation table verified the static mapping. 🚀
