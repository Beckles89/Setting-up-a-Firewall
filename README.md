# 🔥 **Setting Up a Firewall on a Virtual Machine in VirtualBox**

---

> A practical project to install, configure, and manage a basic firewall (UFW/IPTables) on a Linux VM in Oracle VirtualBox.

---

## 📁 Recommended GitHub Repository Structure

```
virtualbox-firewall-setup/
├── README.md
├── images/
│   ├── vm-network-setup.png
│   ├── ufw-enabled.png
│   └── iptables-config.png
├── Firewall_Setup_Guide.md
├── Skills_Learned.md
├── Firewall_Rules_Examples.md
├── Troubleshooting.md
└── LICENSE
```

---

## 📄 `README.md` (Main Repo Page)

```md
# 🔥 Setting Up a Firewall on a Virtual Machine in VirtualBox

This project demonstrates how to configure and manage a firewall using UFW and IPTables on a **Linux virtual machine (VM)** within **VirtualBox**. The goal is to simulate basic firewall controls in a safe, virtual lab environment.

---

## 🎯 Objectives

- Understand and implement firewall rules
- Test blocking and allowing traffic
- Create a secure VM environment

---

## 🛠 Tools Used

- **VirtualBox** – VM platform for isolated testing
- **Ubuntu Server (22.04+)** – Target Linux OS
- **UFW** – Uncomplicated Firewall (user-friendly frontend for iptables)
- **iptables** – Powerful command-line firewall utility

---

## 🧱 Prerequisites

- VirtualBox installed
- Basic networking knowledge
- Ubuntu-based VM installed
- Internet connection for package updates

---

## 📄 Documentation

- 🔧 [Firewall Setup Guide](Firewall_Setup_Guide.md)  
- 📘 [Example Firewall Rules](Firewall_Rules_Examples.md)  
- 🚑 [Troubleshooting Tips](Troubleshooting.md)  
- 🧠 [Skills Learned](Skills_Learned.md)

---

## 🧠 `Skills_Learned.md`

```md
# 🧠 Skills Learned from Building the Firewall Project

## 🧰 Technical Skills

- **Linux Server Administration**
  - Package installation via `apt`
  - Editing configuration files
- **UFW Usage**
  - Enabling and disabling the firewall
  - Writing allow/deny rules for SSH, HTTP, and ports
- **iptables Fundamentals**
  - Using chains: INPUT, OUTPUT, FORWARD
  - Setting default policies and flushing rules
- **Network Testing**
  - Using `ping`, `curl`, `netcat`, `nmap` for testing firewall effectiveness
- **VirtualBox Networking**
  - Configuring NAT and Bridged Adapter
  - Isolating VMs for controlled testing

## 🧠 Cybersecurity Concepts

- Principle of Least Privilege
- Perimeter defense and segmentation
- Understanding ingress vs egress filtering

## 📄 Documentation & Debugging

- Creating technical setup guides
- Troubleshooting service interruptions
- Logging and interpreting rule behavior
```

---

## 🔧 `Firewall_Setup_Guide.md`

````md
# 🔧 Firewall Setup Guide (UFW and IPTables on Ubuntu)

## ✅ Step 1: VM Configuration in VirtualBox

- Create a new Ubuntu Server VM (22.04+ recommended)
- Allocate at least 2 GB RAM
- Set the **Network Adapter** to:
  - `Bridged Adapter` or `NAT` (for internal testing)
- Boot and log in to your Linux VM

---

## 🔄 Step 2: Update the System

```bash
sudo apt update && sudo apt upgrade -y
````

---

## 🔥 Step 3: Install & Configure UFW

```bash
sudo apt install ufw -y
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH if using remote access
sudo ufw allow ssh

# Enable UFW
sudo ufw enable

# Status
sudo ufw status verbose
```

✅ **Test blocked ports:**

```bash
nc -zv <vm-ip> 80
```

---

## 🔥 Step 4: (Optional) Configure iptables

```bash
sudo iptables -L
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# Allow SSH and HTTP
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# View rules
sudo iptables -S
```

---

## 💾 Step 5: Save iptables Rules

Install `iptables-persistent` to keep rules after reboot:

```bash
sudo apt install iptables-persistent
sudo netfilter-persistent save
```

---

## 🔍 Step 6: Test and Monitor

* Use `nmap`, `curl`, `ping` to test open/closed ports
* Watch logs: `sudo journalctl -xe`

---

## ⚠️ Safety Tips

* **Don’t block SSH** unless you have physical/console access.
* Always verify rules before enabling or rebooting.

````

---

## 🧾 `Firewall_Rules_Examples.md`

```md
# 🧾 Example UFW and iptables Rules

## ✅ UFW Common Rules

```bash
sudo ufw allow ssh
sudo ufw allow http
sudo ufw deny 23    # Block Telnet
sudo ufw limit ssh  # Prevent brute force attacks
````

## ✅ IPTables Examples

```bash
# Drop everything by default
iptables -P INPUT DROP
iptables -P OUTPUT ACCEPT

# Accept loopback traffic
iptables -A INPUT -i lo -j ACCEPT

# Accept SSH, HTTP
iptables -A INPUT -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

````

---

## 🚑 `Troubleshooting.md`

```md
# 🚑 Troubleshooting Firewall Configuration

## 🔧 SSH Locked Out

- **Fix**: Boot into recovery mode and disable UFW or flush iptables
```bash
sudo ufw disable
sudo iptables -F
````

## 🔧 Rules Not Applying

* UFW may be inactive: `sudo ufw enable`
* Check status: `sudo ufw status verbose`

## 🔧 VM Has No Internet

* Check VirtualBox network settings:

  * NAT: access to internet
  * Bridged: local network testing

## 🔧 IPTables Not Persistent

* Use: `sudo netfilter-persistent save`
* Or write rules in a script and use crontab to load on boot

## 🔧 Check Logs

```bash
journalctl -xe
tail -f /var/log/syslog
