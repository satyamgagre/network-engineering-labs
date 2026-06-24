# 🌐 LAB 46 - Dynamic NAT Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure Dynamic Network Address Translation (Dynamic NAT) to allow multiple internal hosts to access an external network using a pool of public IP addresses. Verify connectivity, routing, and NAT translations between the private network and the external network.

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
| PC0 | 192.168.10.10 | 192.168.10.1 |
| PC1 | 192.168.10.20 | 192.168.10.1 |

### Google Server

| Device | IP Address | Default Gateway |
|--------|------------|-----------------|
| Server | 8.8.8.8 | 8.8.8.1 |

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
| PC0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1 |
| PC1 | 192.168.10.20 | 255.255.255.0 | 192.168.10.1 |

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

# 🧪 Verify Connectivity Before NAT

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
- No NAT translations should exist before configuring Dynamic NAT.

---

# 🔐 Create a Standard ACL

Permit the internal LAN to use Dynamic NAT.

```cisco
enable
configure terminal

access-list 50 permit 192.168.10.0 0.0.0.255

end
write memory
```

---

# 🌐 Configure a Dynamic NAT Pool

```cisco
enable
configure terminal

ip nat pool DYNAMIC-NAT 200.100.50.1 200.100.50.10 netmask 255.255.255.0

end
write memory
```

---

# 🔗 Bind the ACL to the NAT Pool

```cisco
enable
configure terminal

ip nat inside source list 50 pool DYNAMIC-NAT

end
write memory
```

---

# 🌍 Configure NAT Inside and Outside Interfaces

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

Display the NAT translation table.

```cisco
show ip nat translations
```

Initially, the translation table may be empty until traffic passes through the router.

---

# 🧪 Test Dynamic NAT

From **PC0**

```text
ping 8.8.8.8
```

```text
tracert 8.8.8.8
```

From **PC1**

```text
ping 8.8.8.8
```

```text
tracert 8.8.8.8
```

---

# 📊 Verify NAT Translations

After generating traffic, verify the translations.

```cisco
show ip nat translations
```

Example Output:

```text
Pro  Inside global      Inside local       Outside local      Outside global
icmp 200.100.50.1       192.168.10.10      8.8.8.8            8.8.8.8
icmp 200.100.50.2       192.168.10.20      8.8.8.8            8.8.8.8
```

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

# ✅ Result

Successfully configured Dynamic NAT using a NAT pool on the NAT Router. The inside network (`192.168.10.0/24`) was permitted through an ACL and dynamically translated to available public IP addresses from the configured pool (`200.100.50.1–200.100.50.10`). After generating traffic, the NAT translation table displayed active mappings, confirming successful Dynamic NAT operation.
