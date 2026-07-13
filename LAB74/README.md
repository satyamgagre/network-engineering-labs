# 📡 LAB 74 - Wireless LAN Controller (WLC) Configuration using Cisco Packet Tracer (Method 2)

## 🖥️ Devices Used

* 🖥️ 1 × Server
* 🔀 1 × Cisco 2960-24TT Switch
* 📡 1 × Cisco WLC-2504 Wireless LAN Controller
* 💻 1 × Management PC
* 📶 4 × Lightweight Access Points (LAP-PT)
* 📱 4 × Tablet PCs

> ⚠️ **Note:** Install the **Power Adapter** on each Lightweight Access Point before starting the lab.

---

# 🌐 Network Information

| Device             | IP Address          |
| ------------------ | ------------------- |
| 🌐 Network         | **192.168.10.0/24** |
| 📡 WLC             | **192.168.10.5**    |
| 🖥️ DHCP Server    | **192.168.10.10**   |
| 💻 Management PC   | **192.168.10.6**    |
| 🚪 Default Gateway | **192.168.10.1**    |

---

# 🔋 1. Power On the Lightweight Access Points

* Drag and drop the **Power Adapter** onto all four **LAP-PT** devices.

---

# 🖥️ 2. Configure the DHCP Server

## Configure Static IP

```
IP Address      : 192.168.10.10
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.10.1
DNS Server      : 192.168.10.10
```

---

## 📡 Configure DHCP Service

Go to:

**Services → DHCP**

Configure:

```
Pool Name            : DHCPPOOL
Default Gateway      : 192.168.10.1
DNS Server           : 192.168.10.10
Start IP Address     : 192.168.10.21
Subnet Mask          : 255.255.255.0
Maximum Users        : 20
WLC Address          : 192.168.10.5
```

Click **Save**.

---

# 📡 3. Configure the Wireless LAN Controller

Go to:

**Config → Management**

Configure:

```
IP Address      : 192.168.10.5
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.10.1
DNS Server      : 192.168.10.10
```

---

# 💻 4. Configure the Management PC

```
IP Address      : 192.168.10.6
Subnet Mask     : 255.255.255.0
Default Gateway : 192.168.10.1
DNS Server      : 192.168.10.10
```

---

# 📶 5. Verify Connectivity

From the Management PC:

```
ping 192.168.10.5
```

The ping should be successful.

---

# 🌐 6. Perform Initial WLC Setup

Open the browser on the Management PC.

```
http://192.168.10.5
```

Login using:

```
Username : admin
Password : Admin123
```

Configure:

```
System Name          : ADMIN-WLC
Management IP        : 192.168.10.5
Subnet Mask          : 255.255.255.0
Default Gateway      : 192.168.10.1

Network Name (SSID)  : Test
Passphrase           : Test1234
```

Click **Next** and **Apply**.

---

# ✅ 7. Verify the Configuration

Again, from the Management PC:

```
ping 192.168.10.5
```

---

# 🔒 8. Login to the WLC

Open:

```
https://192.168.10.5
```

Login with:

```
Username : Admin
Password : Admin123
```

---

# 🗑️ 9. Delete the Default WLAN

Navigate to:

```
WLANs
```

Delete the default **Test** WLAN.

---

# 📶 10. Create the IT WLAN

Navigate to:

```
WLANs → Create New
```

Configure:

```
Profile Name : IT DEPT WIFI
SSID         : IT
```

Click **Apply**.

Enable:

```
Status : Enabled
```

Configure Security:

```
Layer 2 Security : WPA + WPA2
Enable WPA Policy
Enable PSK

Password : 12345678
```

Click **Apply**.

---

# 👨‍💼 11. Create the HR WLAN

```
Profile Name : HR DEPT WIFI
SSID         : HR

Status        : Enabled
Layer 2       : WPA + WPA2
Enable WPA Policy
Enable PSK

Password : 12345678
```

Click **Apply**.

---

# 💰 12. Create the FIN WLAN

```
Profile Name : FIN DEPT WIFI
SSID         : FIN

Status        : Enabled
Layer 2       : WPA + WPA2
Enable WPA Policy
Enable PSK

Password : 12345678
```

Click **Apply**.

---

# 👥 13. Create the GUEST WLAN

```
Profile Name : GUEST DEPT WIFI
SSID         : GUEST

Status        : Enabled
Layer 2       : WPA + WPA2
Enable WPA Policy
Enable PSK

Password : 12345678
```

Click **Apply**.

---

# ✅ 14. Verify All WLANs

Go to:

```
WLANs
```

You should now see:

* 📶 IT
* 📶 HR
* 📶 FIN
* 📶 GUEST

---

# 🔄 15. Reboot the Lightweight Access Points (If Required)

If the SSIDs are not visible on the Access Points:

* Remove the **Power Adapter**
* Reconnect the **Power Adapter**
* Wait for the LAPs to reboot and join the WLC.

---

# 📱 16. Connect the Tablet PCs

For each Tablet PC:

```
Config → Wireless0
```

Enter:

| Tablet        | SSID  | Password |
| ------------- | ----- | -------- |
| 📱 Tablet PC0 | IT    | 12345678 |
| 📱 Tablet PC1 | HR    | 12345678 |
| 📱 Tablet PC2 | FIN   | 12345678 |
| 📱 Tablet PC3 | GUEST | 12345678 |

Authentication:

```
WPA2-PSK
```

---

# 🧪 Verification

✔️ Ping the WLC from the Management PC.

```bash
ping 192.168.10.5
```

✔️ Verify all four WLANs appear in the WLC dashboard.

✔️ Confirm each Tablet PC successfully connects to its assigned SSID.

✔️ Hover over each Lightweight Access Point to verify the advertised WLANs.

---

# 💡 Note

> 💡 Unlike [Lab 63](../LAB63/README.md), this method does **not** use AP Groups. All Lightweight Access Points receive every WLAN configured on the WLC, allowing clients to connect to **any Access Point** as long as they have the correct SSID and WPA2-PSK password. This provides greater flexibility and supports seamless roaming between access points.
