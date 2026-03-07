
# DAY 9 – EtherChannel Configuration using PAgP

**Devices Used**

* 1 × Multilayer Switch **3650-24PS**
* 2 × Cisco Switches **2960-24TT**
* Copper Straight-Through Cables
* AC Power Supply (for MLSwitch in Packet Tracer)

---

# 📌 Lab Objective

* Connect two access switches to a multilayer switch
* Bundle multiple physical links into a **single logical link**
* Configure **EtherChannel using PAgP**
* Configure **Trunking on Port-Channel**
* Verify EtherChannel operation

---

# 🧾 Interface Connections

| Device | Interfaces Used | Connection |
|------|------|------|
| S1 | Fa0/1 – Fa0/3 | Connected to MLSW |
| S2 | Fa0/1 – Fa0/3 | Connected to MLSW |

Each switch uses **3 physical links** bundled into **one EtherChannel**.

---

# ⚡ Power the Multilayer Switch

In **Packet Tracer**

Drag **AC Power Supply** to the **3650 MLSwitch** to power on the device.

---

# ⚙️ Switch S1 Configuration

```

enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 1 mode desirable
exit

interface port-channel 1
switchport mode trunk
exit

do write memory

```

---

# ⚙️ Switch S2 Configuration

```

enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 2 mode auto
exit

interface port-channel 2
switchport mode trunk
exit

do write memory

```

---

# ⚙️ Multilayer Switch (MLSW) Configuration

```

enable
configure terminal

interface range gigabitEthernet 1/0/1-3
channel-group 1 mode auto
exit

interface port-channel 1
switchport mode trunk
exit

do write memory

```

---

# 🔍 Verification Commands

```

show etherchannel summary
show etherchannel port-channel
show interfaces port-channel 1

```

These commands verify:

* EtherChannel status
* Member interfaces
* Port-channel configuration

---

# 📚 PAgP Modes

| Mode | Description |
|-----|-------------|
| desirable | Actively negotiates EtherChannel |
| auto | Passive mode, waits for negotiation |

---

# ✅ Valid EtherChannel Combinations

| Switch A | Switch B | Result |
|------|------|------|
| desirable | desirable | Works |
| desirable | auto | Works |
| auto | auto | Not Working |

---

# ✅ Result

Multiple physical interfaces are successfully bundled into a **single logical EtherChannel link** using **PAgP**, providing **higher bandwidth and redundancy** between switches.
```
