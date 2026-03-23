

# 🔐 DAY 16 – IPSG + DAI + DHCP Snooping Configuration

## 📌 Network Topology

* 1 Router
* 1 Switch
* 2 Servers
* 3 Host PCs

---

## ⚙️ Step 1: Router Configuration

```bash
enable
configure terminal

interface gigabitEthernet 0/0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
```

---

## 🖥️ Step 2: Legitimate Server Configuration

```
IP Address:      192.168.1.100
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.1.1
```

---

## 📡 Step 3: DHCP Pool Configuration

```
Pool Name:        DHCPPool
Default Gateway:  192.168.1.1
DNS Server:       0.0.0.0
Start IP Address: 192.168.1.100
Subnet Mask:      255.255.255.0
Maximum Users:    100
```

---

## 🛡️ Step 4: Enable DHCP Snooping (Switch)

```bash
enable
configure terminal
ip dhcp snooping
```

---

## 🌐 Step 5: Enable DHCP Snooping for VLAN

```bash
configure terminal
ip dhcp snooping vlan 1
```

---

## 🔐 Step 6: Configure Trusted Ports (DHCP Snooping)

```bash
enable
configure terminal

interface range fastEthernet 0/4-5
ip dhcp snooping trust
exit

do write memory
```

---

## 🔍 Step 7: Enable Dynamic ARP Inspection (DAI)

```bash
enable
configure terminal
ip arp inspection vlan 1
```

---

## ✅ Step 8: Configure Trusted Ports for DAI

```bash
enable
configure terminal

interface fastEthernet 0/4
ip arp inspection trust
exit
```

---

# 🚫 Step 9: Enable IP Source Guard (IPSG)

👉 IPSG prevents IP spoofing by allowing traffic only from valid IP-MAC bindings (from DHCP Snooping table)

### Enable IPSG on Access Ports (Host Ports)

```bash
enable
configure terminal

interface range fastEthernet 0/1-3
ip verify source
exit
```

---

## ⚠️ Important Notes for IPSG

* Works **only with DHCP Snooping**
* Apply **ONLY on untrusted/access ports**
* Do NOT enable on trunk or trusted ports
* Uses DHCP Snooping binding table automatically

---

## 🔎 Verification Commands

```bash
show ip dhcp snooping
show ip dhcp snooping binding
show ip arp inspection
show ip verify source
```

