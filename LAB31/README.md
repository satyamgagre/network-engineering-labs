# 🌐 LAB 31 - Configure a Dedicated DHCP, DNS, Web, Email and FTP Server

## 🎯 Objective

Configure dedicated **DHCP**, **DNS**, **Web**, **Email**, and **FTP Servers** in a centralized Data Center LAN. Client PCs should obtain IP addresses dynamically, access websites using domain names, transfer files using FTP, and exchange emails within the network.

---

## 🖧 Topology

### 🏢 Office LAN

* 1 × Cisco 2960-24TT Switch
* 5 × PCs
* Network: `192.168.10.0/24`

### 🏢 Data Center LAN

* 1 × Cisco 2960-24TT Switch
* 1 × DHCP Server
* 1 × DNS Server
* 1 × Web Server
* 1 × Email Server
* 1 × FTP Server

### 🛜 Router

* Cisco 2911 Router
* IP Address: `192.168.10.1`

---

# 📋 Addressing Table

| Device       | IP Address   |
| ------------ | ------------ |
| Router       | 192.168.10.1 |
| Web Server   | 192.168.10.5 |
| DNS Server   | 192.168.10.6 |
| DHCP Server  | 192.168.10.7 |
| Email Server | 192.168.10.8 |
| FTP Server   | 192.168.10.9 |

---

## Common Server Settings

| Parameter       | Value         |
| --------------- | ------------- |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 192.168.10.6  |

---

# ⚙️ Configure Router

```bash
enable
configure terminal

interface g0/0
 ip address 192.168.10.1 255.255.255.0
 no shutdown

exit
do write
```

---

# 🖥️ Configure Server IP Addresses

## Web Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.10.5  |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 192.168.10.6  |

### Enable HTTP Service

1. Open **Services → HTTP**
2. Turn **HTTP Service ON**
3. Open **index.html**
4. Add your HTML code
5. Save the file

---

## DNS Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.10.6  |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 192.168.10.6  |

---

## DHCP Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.10.7  |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 192.168.10.6  |

---

## Email Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.10.8  |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 192.168.10.6  |

---

## FTP Server

| Parameter       | Value         |
| --------------- | ------------- |
| IP Address      | 192.168.10.9  |
| Subnet Mask     | 255.255.255.0 |
| Default Gateway | 192.168.10.1  |
| DNS Server      | 192.168.10.6  |

---

# 📡 Configure DHCP Server

Open **Services → DHCP** and create the following pool:

| Setting          | Value           |
| ---------------- | --------------- |
| Pool Name        | OFFICE-LAN-POOL |
| Default Gateway  | 192.168.10.1    |
| DNS Server       | 192.168.10.6    |
| Start IP Address | 192.168.10.100  |
| Subnet Mask      | 255.255.255.0   |
| Maximum Users    | 100             |

Save the configuration.

---

# 💻 Configure Client PCs

On every PC:

```text
Desktop
→ IP Configuration
→ DHCP
```

Verify that an IP address is received from the DHCP Server.

---

# 🌍 Configure DNS Server

1. Open **Services → DNS**
2. Turn **DNS Service ON**
3. Create the following DNS record:

| Name        | Address      |
| ----------- | ------------ |
| example.com | 192.168.10.5 |

4. Save the configuration.

---

# 🌐 Configure Web Server

1. Open **Services → HTTP**
2. Turn **HTTP Service ON**
3. Open **index.html**
4. Paste your HTML code
5. Save the file

---

# 📁 Configure FTP Server

1. Open **Services → FTP**
2. Enable FTP Service
3. Create the following user:

| Username | Password |
| -------- | -------- |
| admin    | admin    |

4. Grant the following permissions:

   * Read
   * Write
   * Delete
   * Rename
   * List

5. Save the configuration.

---

# 🧪 Test FTP Access

On any client PC:

```text
Desktop → Command Prompt
```

Run:

```bash
ftp 192.168.10.9
```

Login using:

```text
Username: admin
Password: admin
```

Verify that FTP access is successful.

---

# 📧 Configure Email Server

1. Open **Services → Email**
2. Enable Email Service
3. Add Domain:

```text
example.com
```

Create the following accounts:

| Username | Password |
| -------- | -------- |
| admin1   | admin1   |
| admin2   | admin2   |

Save the configuration.

---

# 📨 Configure Email Client on PC1

Open:

```text
Desktop → Email
```

Enter:

| Field                | Value                                           |
| -------------------- | ----------------------------------------------- |
| Your Name            | Satyam Gagre                                    |
| Email Address        | [admin1@example.com](mailto:admin1@example.com) |
| Incoming Mail Server | 192.168.10.8                                    |
| Outgoing Mail Server | 192.168.10.8                                    |
| Username             | admin1                                          |
| Password             | admin1                                          |

Click **Save**.

---

# 📨 Configure Email Client on PC2

Open:

```text
Desktop → Email
```

Enter:

| Field                | Value                                           |
| -------------------- | ----------------------------------------------- |
| Your Name            | Ajay Kumar                                      |
| Email Address        | [admin2@example.com](mailto:admin2@example.com) |
| Incoming Mail Server | 192.168.10.8                                    |
| Outgoing Mail Server | 192.168.10.8                                    |
| Username             | admin2                                          |
| Password             | admin2                                          |

Click **Save**.

---

# 🧪 Test Email Communication

### From PC1

Compose a new email:

```text
To: admin2@example.com
Subject: Test Email
Message: Hello from PC1
```

Click **Send**.

### On PC2

1. Open Email Application
2. Click **Receive**
3. Verify that the email from **[admin1@example.com](mailto:admin1@example.com)** is received successfully.

---

# 🌐 Test Website Access

Open a web browser on any client PC and visit:

```text
http://example.com
```

Verify that the webpage hosted on the Web Server loads successfully.

---

# ✅ Result

Successfully configured dedicated **DHCP**, **DNS**, **Web**, **Email**, and **FTP Servers**. Client PCs received IP addresses dynamically, resolved domain names through DNS, accessed the web server, transferred files using FTP, and exchanged emails using the configured mail server. 🚀
