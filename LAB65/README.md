# LAB 65 - VoIP Configuration for Data and Voice VLAN using Cisco Packet Tracer

## Devices Used

* **1 × Router 2811**
* **1 × Switch 2960-24TT**
* **4 × Cisco 7960 IP Phones**
* **4 × PCs**

### Network Details

| VLAN   | Purpose | Network             |
| ------ | ------- | ------------------- |
| **33** | Voice   | **172.16.10.0/24**  |
| **50** | Data    | **192.168.10.0/24** |

> **Note:** Install the **IP_PHONE_POWER_ADAPTER** on each IP Phone before powering it on.

---

# 1. Create Data and Voice VLANs

**Switch**

```bash
enable
configure terminal

vlan 33
name VOICE
exit

vlan 50
name DATA
exit

do write
```

---

# 2. Configure Access Ports

```bash
interface range fastethernet 0/2-5
switchport mode access
switchport access vlan 50
switchport voice vlan 33
exit

do write
```

---

# 3. Configure Trunk Port

```bash
interface fastethernet 0/1
switchport mode trunk
exit

do write
```

---

# 4. Configure Router-on-a-Stick

**Router**

```bash
enable
configure terminal

interface fastethernet 0/0
no shutdown
exit

interface fastethernet 0/0.33
encapsulation dot1Q 33
ip address 172.16.10.1 255.255.255.0
exit

interface fastethernet 0/0.50
encapsulation dot1Q 50
ip address 192.168.10.1 255.255.255.0
exit

do write
```

---

# 5. Configure DHCP Pools

```bash
service dhcp

ip dhcp excluded-address 172.16.10.1 172.16.10.10
ip dhcp excluded-address 192.168.10.1 192.168.10.10

ip dhcp pool VOICE
network 172.16.10.0 255.255.255.0
default-router 172.16.10.1
option 150 ip 172.16.10.1
exit

ip dhcp pool DATA
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
exit

do write
```

---

# 6. Configure Telephony Service

```bash
telephony-service
max-ephones 10
max-dn 10
ip source-address 172.16.10.1 port 2000
auto assign 1 to 10
exit

do write
```

---

# 7. Configure Phone Extensions

```bash
ephone-dn 1
number 1001
exit

ephone-dn 2
number 1002
exit

ephone-dn 3
number 1003
exit

ephone-dn 4
number 1004
exit

do write
```

---

# 8. Verify Communication

* Ensure all IP Phones receive an IP address from the **VOICE** DHCP pool.
* Ensure all PCs receive an IP address from the **DATA** DHCP pool.
* Make a call between phones (e.g., **1001 → 1004**) and verify the call connects successfully.

---

# Verification Commands

```bash
show vlan brief
show interfaces trunk
show ip interface brief
show ip dhcp binding
show telephony-service
show ephone
show ephone-dn
show running-config
```

---

## Troubleshooting

If the IP Phones do not receive extensions or fail to register:

* Disconnect and reconnect the **Power Adapter** to power cycle the phone.
* Verify that **Option 150** points to the router's telephony-service IP (`172.16.10.1`).
* Confirm the switch access ports have both the **Data VLAN (50)** and **Voice VLAN (33)** configured.
* Verify the trunk link between the switch and router is up and allows VLANs 33 and 50.
