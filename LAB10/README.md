
# DAY 10 – EtherChannel Configuration Using LACP

## Lab Objective

Configure **EtherChannel using LACP (Link Aggregation Control Protocol)** between multiple switches and a multilayer switch.
Multiple physical links are bundled into one **logical Port-Channel interface** to increase bandwidth and provide redundancy.

---

# Devices Used

| Device            | Model                   |
| ----------------- | ----------------------- |
| Switch            | Cisco 2960-24TT         |
| Multilayer Switch | Cisco 3650-24PS         |
| Cables            | Copper Straight Through |


```
S1 ----(3 links)---- S2 ----(3 links)---- MLSW ----(3 links)---- S3 ----(3 links)---- S4
```

| Connection | Channel Group | Mode             |
| ---------- | ------------- | ---------------- |
| S1 ↔ S2    | 1             | active / passive |
| S2 ↔ MLSW  | 2             | active / passive |
| MLSW ↔ S3  | 3             | active / passive |
| S3 ↔ S4    | 4             | active / passive |

---

# Important Notes

* Both sides must use the **same channel-group number**
* Allowed LACP modes:

| Mode    | Description                 |
| ------- | --------------------------- |
| active  | Actively sends LACP packets |
| passive | Responds to LACP packets    |

**✔ active + passive → Works**  
**✔ active + active → Works**  
**❌ passive + passive → Does NOT work**  

---

# Switch S1 Configuration

```
enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 1 mode active
exit

interface port-channel 1
switchport mode trunk
```

---

# Switch S2 Configuration

## Link to S1

```
enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 1 mode passive
exit

interface port-channel 1
switchport mode trunk
```

## Link to MLSW

```
interface range fastEthernet 0/4-6
channel-group 2 mode active
exit

interface port-channel 2
switchport mode trunk
```

---

# Multilayer Switch (MLSW) Configuration

## Link to S2

```
enable
configure terminal

interface range gigabitEthernet 1/0/1-3
channel-group 2 mode passive
exit

interface port-channel 2
switchport mode trunk
```

## Link to S3

```
interface range gigabitEthernet 1/0/4-6
channel-group 3 mode active
exit

interface port-channel 3
switchport mode trunk
```

---

# Switch S3 Configuration

## Link to MLSW

```
enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 3 mode passive
exit

interface port-channel 3
switchport mode trunk
```

## Link to S4

```
interface range fastEthernet 0/4-6
channel-group 4 mode active
exit

interface port-channel 4
switchport mode trunk
```

---

# Switch S4 Configuration

```
enable
configure terminal

interface range fastEthernet 0/1-3
channel-group 4 mode passive
exit

interface port-channel 4
switchport mode trunk
```

---

# Verification Commands

Check EtherChannel status:

```
show etherchannel summary
```

Check LACP neighbors:

```
show lacp neighbor
```

Check Port-Channel interface:

```
show interfaces port-channel
```

---

# Expected Result

* EtherChannel successfully created between switches.
* Multiple physical links operate as **one logical trunk link**.
* **Bandwidth increased and redundancy achieved**.
* LACP automatically manages link aggregation.

