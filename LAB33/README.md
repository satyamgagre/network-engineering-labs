# 🌐 LAB 33 - Configure DHCP Relay Agent on a Multilayer Switch

## 🎯 Objective

Configure a DHCP Relay Agent on a Cisco 3650 Multilayer Switch so that clients in a different subnet can obtain IP addresses from a centralized DHCP Server.

---

## 🖧 Devices Required

* 1 × Cisco 3650 Multilayer Switch
* 2 × Cisco 2960-24TT Switches
* 5 × Host PCs
* 1 × DHCP Server

---

# 📋 IP Addressing Table

| Device                             | IP Address     |
| ---------------------------------- | -------------- |
| Multilayer Switch Interface G1/0/2 | 192.168.11.1   |
| Multilayer Switch Interface G1/0/1 | 192.168.22.1   |
| DHCP Server                        | 192.168.11.254 |

---

# ⚙️ Configure the Multilayer Switch

```cisco
enable
conf t

ip routing

interface GigabitEthernet1/0/2
 no shutdown
 no switchport
 ip address 192.168.11.1 255.255.255.0
 exit

interface GigabitEthernet1/0/1
 no shutdown
 no switchport
 ip address 192.168.22.1 255.255.255.0
 exit

do write
```

---

# 🖥️ Configure DHCP Server

Assign the following static IP configuration:

| Parameter       | Value          |
| --------------- | -------------- |
| IP Address      | 192.168.11.254 |
| Subnet Mask     | 255.255.255.0  |
| Default Gateway | 192.168.11.1   |
| DNS Server      | 192.168.11.254 |

---

# 📡 Configure DHCP Pool

Open **Services → DHCP** and create the following pool:

| Setting          | Value          |
| ---------------- | -------------- |
| Pool Name        | HR_DEPT_POOL   |
| Default Gateway  | 192.168.22.1   |
| DNS Server       | 192.168.11.254 |
| Start IP Address | 192.168.22.11  |
| Subnet Mask      | 255.255.255.0  |
| Maximum Users    | 240            |

Save the configuration.

---

# 🔄 Configure DHCP Relay Agent

Configure the helper address on the interface connected to the client LAN.

```cisco
interface GigabitEthernet1/0/1
 ip helper-address 192.168.11.254
 exit

do write
```

---

# 💻 Configure Client PCs

On each PC:

```text
Desktop → IP Configuration → DHCP
```

The PCs should automatically receive IP addresses from the DHCP Server.

---

# 🧪 Verification

Verify that each PC receives:

* IP Address from the `192.168.22.0/24` network
* Subnet Mask `255.255.255.0`
* Default Gateway `192.168.22.1`
* DNS Server `192.168.11.254`

You can verify connectivity by opening Command Prompt on a PC:

```bash
ipconfig
```

and

```bash
ping 192.168.11.254
```

---

# ✅ Result

Successfully configured a DHCP Relay Agent on the Cisco 3650 Multilayer Switch. Client PCs in the `192.168.22.0/24` network obtained IP addresses dynamically from the DHCP Server located in the `192.168.11.0/24` network using the `ip helper-address` feature. 🚀
