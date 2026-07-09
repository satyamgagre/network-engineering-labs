# LAB 64 - Configuring VoIP/Telephony Service using Cisco Packet Tracer

## Devices Used

* **1 × Router 2811**
* **1 × Switch 2960-24TT**
* **6 × Cisco 7960 IP Phones**

> **Note:** IP Phones require a power adapter in Packet Tracer. Open the **Physical** tab of each IP Phone, drag the **IP_PHONE_POWER_ADAPTER** into the power slot, and turn the phone on.

---

# 1. Configure Voice VLAN on the Switch

**Switch S1**

```bash
enable
configure terminal

interface range fastethernet 0/2-7
switchport voice vlan 1
exit

do write
```

---

# 2. Configure Trunk Port to the Router

**Switch S1**

```bash
enable
configure terminal

interface fastethernet 0/1
switchport mode trunk
exit

do write
```

---

# 3. Configure Router Interface

**Router (DHCP-ROUTER)**

```bash
enable
configure terminal

interface fastethernet 0/0
no shutdown
ip address 10.10.10.1 255.255.255.0
exit

do write
```

---

# 4. Configure DHCP Pool for IP Phones

```bash
ip dhcp pool VOICE
network 10.10.10.0 255.255.255.0
default-router 10.10.10.1
option 150 ip 10.10.10.1
exit

do write
```

---

# 5. Configure Telephony Service

```bash
telephony-service
max-ephones 6
max-dn 6
ip source-address 10.10.10.1 port 2000
auto assign 1 to 6
exit

do write
```

---

# 6. Configure Phone Extensions

```bash
ephone-dn 1
number 101
exit

ephone-dn 2
number 102
exit

ephone-dn 3
number 103
exit

ephone-dn 4
number 104
exit

ephone-dn 5
number 105
exit

ephone-dn 6
number 106
exit

do write
```

---

# 7. Verify Communication

From any IP Phone:

* Open the **GUI**.
* Dial another phone's extension (e.g., **101 → 106**).
* Answer the incoming call on the destination phone.
* Verify that the call is successfully connected.

---

# Verification Commands

```bash
show ip dhcp binding
show telephony-service
show ephone
show ephone-dn
show running-config
```

---

## Troubleshooting

If the IP Phones do not receive extensions or fail to register:

* Disconnect the **Power Adapter** from the IP Phone.
* Reconnect the adapter to power cycle the phone.
* Wait for the phone to reload and download its configuration again.

> In Cisco Packet Tracer, IP Phones sometimes do not automatically refresh their configuration after changes. A quick power cycle usually resolves the issue.
