# 🔄 LAB 32 - Configure DHCP Relay on a Cisco Router

## 🎯 Objective

Configure a DHCP Relay Agent on a Cisco Router to allow clients in one network to obtain IP addresses from a DHCP Server located in a different network.

---

## 📋 Addressing Table

| Device         | Interface | IP Address    |
| -------------- | --------- | ------------- |
| 🛜 Router      | G0/0      | 192.168.20.1  |
| 🛜 Router      | G0/1      | 192.168.10.1  |
| 📡 DHCP Server | NIC       | 192.168.20.10 |

### 🖥️ DHCP Server Settings

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.20.10 |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.20.1  |
| DNS Server      | 192.168.20.5  |

---

# 🔧 Step 1: Configure Router Interfaces

```bash
enable
configure terminal

interface gigabitEthernet 0/0
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit

interface gigabitEthernet 0/1
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit

do wr
```

---

# 🖥️ Step 2: Configure DHCP Server

Assign the following static IP configuration:

```text
IP Address      : 192.168.20.10
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.20.1
DNS Server      : 192.168.20.5
```

---

# 📡 Step 3: Configure DHCP Pool

Navigate to:

```text
Services
→ DHCP
```

Create the following DHCP Pool:

| Parameter        | Value         |
| ---------------- | ------------- |
| Pool Name        | IT-POOL       |
| Default Gateway  | 192.168.10.1  |
| DNS Server       | 192.168.20.5  |
| Start IP Address | 192.168.10.11 |
| Subnet Mask      | 255.255.255.0 |
| Maximum Users    | 100           |

Click **Add**.

---

# 🔄 Step 4: Configure DHCP Relay Agent

Configure the router interface connected to the client LAN to forward DHCP requests to the DHCP Server.

```bash
enable
configure terminal

interface gigabitEthernet 0/1
 ip helper-address 192.168.20.10
exit

do wr
```

---

# 💻 Step 5: Configure Client PCs

For each PC:

```text
Desktop
→ IP Configuration
→ DHCP
```

### ✅ Expected DHCP Range

```text
192.168.10.11 - 192.168.10.110
```

---

# 🔍 Verification

## Verify Router Interfaces

```bash
show ip interface brief
```

Expected Output:

```text
GigabitEthernet0/0    192.168.20.1
GigabitEthernet0/1    192.168.10.1
```

---

## Verify DHCP Relay Configuration

```bash
show running-config
```

Expected Output:

```text
interface GigabitEthernet0/1
 ip helper-address 192.168.20.10
```

---

## Verify Client IP Assignment

Open any PC:

```text
Desktop
→ IP Configuration
```

Expected:

```text
IP Address : 192.168.10.x
Gateway    : 192.168.10.1
DNS Server : 192.168.20.5
```

---

## Test Connectivity

From any PC:

```bash
ping 192.168.10.1
ping 192.168.20.10
```

Both pings should be successful.

---

# 🎉 Result

✅ Router Interfaces Configured Successfully

✅ DHCP Server Configured with Static IP Address

✅ DHCP Pool Created Successfully

✅ DHCP Relay Agent Configured Using `ip helper-address`

✅ Client PCs Received IP Addresses Automatically

✅ End-to-End Connectivity Verified

🏆 **LAB 32 Completed Successfully!**
