
# DAY 11 – Manual EtherChannel Configuration (Mode ON)

## Devices Used

* 3 × Cisco Switch **2960-24TT**
* **Copper Cross-Over Cables**
* **3 links between each switch**

---

# 📌 Lab Objective

* Configure **EtherChannel using Manual "ON" mode**
* Bundle multiple physical links into **one logical Port-Channel**
* Configure **Trunking on EtherChannel interfaces**
* Verify EtherChannel configuration

---

# 📡 Network Topology

```
S1 ===(3 cables)== S2 ===(3 cables)== S3 ===(3 cables)== S1
```

* **S1 ↔ S2 → Port-Channel 1**
* **S2 ↔ S3 → Port-Channel 2**
* **S3 ↔ S1 → Port-Channel 3**

Each EtherChannel uses **3 FastEthernet links**.

---

# ⚙️ Switch 1 Configuration (S1)

```
enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 1 mode on
exit

interface port-channel 1
switchport mode trunk
do write

interface range fastEthernet 0/4-6
channel-group 3 mode on
exit

interface port-channel 3
switchport mode trunk
do write
```

---

# ⚙️ Switch 2 Configuration (S2)

```
enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 1 mode on
exit

interface port-channel 1
switchport mode trunk
do write

interface range fastEthernet 0/4-6
channel-group 2 mode on
exit

interface port-channel 2
switchport mode trunk
do write
```

---

# ⚙️ Switch 3 Configuration (S3)

```
enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 2 mode on
exit

interface port-channel 2
switchport mode trunk
do write

interface range fastEthernet 0/4-6
channel-group 3 mode on
exit

interface port-channel 3
switchport mode trunk
do write
```

---

# 🔎 Verification Command

Check EtherChannel status:

```
show etherchannel summary
```


### Status Meaning

| Symbol | Meaning                    |
| ------ | -------------------------- |
| **SU** | Layer2 EtherChannel in use |
| **P**  | Port successfully bundled  |
| **Po** | Port-channel interface     |

---

# 📘 Important Notes

* **Mode ON = Manual EtherChannel**
* No negotiation protocol is used.
* Both sides must be configured **manually**.
* Common protocols used instead of ON:

  * **PAgP**
  * **LACP**
---
