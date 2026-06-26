# 🌐 LAB 48 - Hot Standby Routing Protocol (HSRP) Configuration Using Cisco Packet Tracer

## 🎯 Objective

Configure Hot Standby Routing Protocol (HSRP) to provide first-hop redundancy for the Site-A LAN. Verify failover by disconnecting the active router and confirming that the standby router automatically takes over while maintaining connectivity to Site-B.

---

## 🖧 Devices Required

- 3 × Cisco 2911 Routers
- 1 × Cisco 3650-24PS Multilayer Switch
- 2 × Cisco 2960-24TT Switches
- 4 × Host PCs

---

## 📋 IP Addressing Table

### R1 (Active Router)

| Interface | IP Address |
|-----------|------------|
| GigabitEthernet0/0 | 192.168.10.3 /24 |
| GigabitEthernet0/1 | 10.10.10.1 /30 |

---

### R2

| Interface | IP Address |
|-----------|------------|
| GigabitEthernet0/0 | 10.10.10.2 /30 |
| GigabitEthernet0/1 | 10.10.10.6 /30 |
| GigabitEthernet0/2 | 192.168.20.1 /24 |

---

### R3 (Standby Router)

| Interface | IP Address |
|-----------|------------|
| GigabitEthernet0/0 | 192.168.10.2 /24 |
| GigabitEthernet0/1 | 10.10.10.5 /30 |

---

### HSRP Virtual Router

| Virtual IP |
|------------|
| **192.168.10.1** |

---

### Host PCs

| PC | IP Address | Default Gateway |
|----|------------|-----------------|
| P1 | 192.168.10.10 | **192.168.10.1** |
| P2 | 192.168.10.20 | **192.168.10.1** |
| P3 | 192.168.20.10 | 192.168.20.1 |
| P4 | 192.168.20.20 | 192.168.20.1 |

> **Note:** Site-A hosts use the **HSRP Virtual IP (192.168.10.1)** as their default gateway.

---

# ⚙️ Configure Router Interfaces

## R1 (Active Router)

```cisco
enable
configure terminal

interface GigabitEthernet0/1
 ip address 10.10.10.1 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/0
 ip address 192.168.10.3 255.255.255.0
 no shutdown

 standby 5 priority 100
 standby 5 ip 192.168.10.1

 exit

end
write memory
```

---

## R3 (Standby Router)

```cisco
enable
configure terminal

interface GigabitEthernet0/1
 ip address 10.10.10.5 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/0
 ip address 192.168.10.2 255.255.255.0
 no shutdown

 standby 5 priority 90
 standby 5 ip 192.168.10.1

 exit

end
write memory
```

Verify HSRP status:

```cisco
show standby brief
```

---

## R2

```cisco
enable
configure terminal

interface GigabitEthernet0/0
 ip address 10.10.10.2 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 10.10.10.6 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/2
 ip address 192.168.20.1 255.255.255.0
 no shutdown
 exit

end
write memory
```

---

# 🔄 Configure OSPF

## R1

```cisco
enable
configure terminal

router ospf 10
 router-id 1.1.1.1

 network 192.168.10.0 0.0.0.255 area 0
 network 10.10.10.0 0.0.0.3 area 0

exit

end
write memory
```

---

## R2

> **Note:** The original instructions used an incorrect wildcard mask for the LAN network. The correct wildcard mask for `/24` is `0.0.0.255`.

```cisco
enable
configure terminal

router ospf 10
 router-id 2.2.2.2

 network 10.10.10.0 0.0.0.3 area 0
 network 10.10.10.4 0.0.0.3 area 0
 network 192.168.20.0 0.0.0.255 area 0

exit

end
write memory
```

---

## R3

```cisco
enable
configure terminal

router ospf 10
 router-id 3.3.3.3

 network 10.10.10.4 0.0.0.3 area 0
 network 192.168.10.0 0.0.0.255 area 0

exit

end
write memory
```

---

# 🔍 Verify OSPF

Display OSPF neighbors.

```cisco
show ip ospf neighbor
```

---

# 🧪 Test Connectivity Before Failover

From **P1**

```text
ping 192.168.20.10
```

Trace the route:

```text
tracert 192.168.20.20
```

Expected Result:

- Traffic should pass through **R1 (Active Router)**.

---

# 🔄 Simulate Router Failure

Disconnect the link between **R1** and the Multilayer Switch (or shut down the interface).

Example:

```cisco
interface GigabitEthernet0/0
shutdown
```

---

# 🧪 Test Connectivity After Failover

From **P1**

```text
ping 192.168.20.10
```

```text
tracert 192.168.20.20
```

Expected Result:

- Connectivity should continue without changing the PCs' default gateway.
- Traffic should now pass through **R3 (Standby Router)**.

---

# 🛠 Useful Verification Commands

Verify HSRP status:

```cisco
show standby brief
```

Display OSPF neighbors:

```cisco
show ip ospf neighbor
```

Display routing table:

```cisco
show ip route
```

Display running configuration:

```cisco
show running-config
```

---

# 📚 HSRP Priority

| Router | Priority | Role |
|---------|---------:|------|
| R1 | 100 | Active |
| R3 | 90 | Standby |

The router with the higher priority becomes the Active HSRP router.

---

# ✅ Result

Successfully configured Hot Standby Routing Protocol (HSRP) to provide gateway redundancy for the Site-A network. The hosts used the virtual gateway address (`192.168.10.1`) while R1 operated as the Active router and R3 as the Standby router. After simulating a failure of the Active router, the Standby router automatically assumed the Active role, maintaining uninterrupted connectivity between Site-A and Site-B.
