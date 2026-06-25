# 🌐 LAB 47 - PAT (NAT Overload) Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure Port Address Translation (PAT), also known as NAT Overload, to allow multiple internal hosts to share a single public IP address for Internet access. Verify connectivity and NAT translations.

---

## 🖧 Devices Required

- 2 × Cisco 2911 Routers
- 1 × Cisco 2960-24TT Switch
- 2 × Host PCs
- 1 × Server

---

## 📋 IP Addressing Table

### NAT Router

| Interface | IP Address |
|-----------|------------|
| GigabitEthernet0/0 (Inside) | 192.168.10.1 /24 |
| GigabitEthernet0/1 (Outside) | 200.100.50.1 /24 |

### ISP Router

| Interface | IP Address |
|-----------|------------|
| GigabitEthernet0/0 | 200.100.50.2 /24 |
| GigabitEthernet0/1 | 8.8.8.1 /24 |

### Host PCs

| Device | IP Address | Default Gateway |
|--------|------------|-----------------|
| P1 | 192.168.10.10 | 192.168.10.1 |
| P2 | 192.168.10.20 | 192.168.10.1 |

### Google Server

| Device | IP Address | Default Gateway |
|--------|------------|-----------------|
| Google Server | 8.8.8.8 | 8.8.8.1 |

---

# ⚙️ Configure Router Interfaces

## NAT Router

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 200.100.50.1 255.255.255.0
 no shutdown
 exit

end
write memory
```

---

## ISP Router

> **Note:** The original instructions contained an incorrect subnet mask (`255.255.0`). The correct subnet mask is `255.255.255.0`.

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 200.100.50.2 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 8.8.8.1 255.255.255.0
 no shutdown
 exit

end
write memory
```

---

# 💻 Configure Host PCs

| PC | IP Address | Subnet Mask | Default Gateway |
|----|------------|-------------|-----------------|
| P1 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| P2 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1 |

---

# 🖥️ Configure the Server

| Parameter | Value |
|-----------|-------|
| IP Address | 8.8.8.8 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 8.8.8.1 |

---

# 🔄 Configure OSPF

## NAT Router

```cisco
enable
configure terminal

router ospf 10
 router-id 1.1.1.1

 network 192.168.10.0 0.0.0.255 area 0
 network 200.100.50.0 0.0.0.255 area 0

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

 network 200.100.50.0 0.0.0.255 area 0
 network 8.8.8.0 0.0.0.255 area 0

exit

end
write memory
```

---

# 🧪 Verify Connectivity Before PAT

From either PC:

```text
ping 8.8.8.8
```

Trace the route:

```text
tracert 8.8.8.8
```

Check for existing NAT translations:

```cisco
show ip nat translations
```

Expected Result:

- End-to-end connectivity should be available through OSPF.
- No NAT translations should exist before configuring PAT.

---

# 🔐 Create a Standard ACL

Permit the inside network for PAT.

```cisco
enable
configure terminal

access-list 50 permit 192.168.10.0 0.0.0.255

end
write memory
```

---

# 🌐 Configure PAT (NAT Overload)

Configure PAT to use the outside interface IP address.

```cisco
enable
configure terminal

ip nat inside source list 50 interface GigabitEthernet0/1 overload

end
write memory
```

---

# 🔗 Configure NAT Inside and Outside Interfaces

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

# 🧪 Test PAT

From **P1**

```text
ping 8.8.8.8
```

```text
tracert 8.8.8.8
```

From **P2**

```text
ping 8.8.8.8
```

```text
tracert 8.8.8.8
```

---

# 📊 Verify PAT Translations

Display the translation table.

```cisco
show ip nat translations
```

Example Output:

```text
Pro  Inside global      Inside local       Outside local      Outside global

icmp 200.100.50.1:3     192.168.10.10:3    8.8.8.8:3          8.8.8.8:3
icmp 200.100.50.1:4     192.168.10.20:4    8.8.8.8:4          8.8.8.8:4
```

Notice that both inside hosts share the same public IP address (`200.100.50.1`) while being differentiated by unique port numbers.

---

# 🛠 Useful Verification Commands

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

Display the routing table:

```cisco
show ip route
```

Display the running configuration:

```cisco
show running-config
```

---

# 📚 PAT vs Dynamic NAT

| Dynamic NAT | PAT (NAT Overload) |
|-------------|--------------------|
| Uses a pool of public IP addresses | Uses a single public IP address |
| One public IP per active host | Multiple hosts share one public IP |
| Limited by pool size | Supports thousands of simultaneous connections using port numbers |
| One-to-one dynamic mapping | Many-to-one translation |

---

# ✅ Result

Successfully configured PAT (NAT Overload) on the NAT Router. The internal network (`192.168.10.0/24`) was permitted through a standard ACL, and all inside hosts shared the router's outside interface IP address (`200.100.50.1`) using unique port numbers. After generating traffic, the NAT translation table confirmed successful PAT operation and Internet connectivity.
