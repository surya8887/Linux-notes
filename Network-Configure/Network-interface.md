
# It will include **file paths, commands, and configuration methods** (`nmcli`, `nmtui`, Netplan, config files).

Here’s the file:

---

````markdown
# 🖧 Linux Networking Configuration Guide (CentOS 10 & Ubuntu 24.04)

This guide explains how to configure network interfaces on **CentOS Stream 10** and **Ubuntu 24.04 LTS**.  
It covers **network modes, file paths, commands, and configuration tools**.

---

## 🔹 VM Network Modes (VirtualBox/VMware)

| Mode        | Description |
|-------------|-------------|
| **Host-only** | VM can only talk to host. No internet. |
| **Internal**  | VM can only talk to other VMs in same internal network. |
| **Bridged**   | VM acts like a separate machine on LAN with its own IP. |
| **NAT**       | VM shares host’s IP for internet access. |
| **NATservice**| NAT + port forwarding support. |

---

## 🔹 CentOS Stream 10 (RHEL 10+)

### 📂 File Paths
- `/etc/NetworkManager/system-connections/` → Main connection configs (`*.nmconnection`)  
- (Old RHEL 7 & CentOS 7 only: `/etc/sysconfig/network-scripts/ifcfg-*`)


### 🔧 Commands

#### Service Control

``` bash

sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager

```

#### Device & Connection Info

```bash
nmcli dev status            # Show device status
nmcli device show           # Show device details
nmcli connection show       # Show available connections
nmcli c s                   # Short form
```

#### Bring Interfaces UP/DOWN

```bash
nmcli connection up ens160
nmcli connection down ens160
ifdown ens160     # Old method
ifup ens160       # Old method
```

#### Create a New Connection

```bash
nmcli con add type ethernet con-name ens224 ifname ens224
```

#### Configure Static IP

```bash
nmcli con add type ethernet con-name static2 ifname ens256 ip4 192.168.0.50/24 gw4 192.168.0.1
```

#### Set DNS

```bash
nmcli con mod static2 ipv4.dns "8.8.8.8"
nmcli con mod static2 -ipv4.dns 8.8.8.8   # Remove DNS
```

#### Autoconnect

```bash
nmcli con mod static2 connection.autoconnect yes
nmcli con mod static2 connection.autoconnect no
```

#### User Permissions

```bash
nmcli con mod static2 connection.permissions user:vikash,surya,karan
```

#### Connection Management

```bash
nmcli con reload
nmcli con edit static2
nmcli con del static2
```

#### Tools

```bash
nmtui       # Interactive text UI
vim ifcfg-ens224   # Manual edit (old style, CentOS 7)
```

---

## 🔹 Ubuntu 24.04 LTS

### 📂 File Paths

* `/etc/netplan/` → YAML-based network config (`*.yaml`)
* `/etc/NetworkManager/system-connections/` → Used if NetworkManager is installed (Desktop edition).

### 🔧 Commands

#### Service Control

```bash
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager
```

#### Device Info

```bash
ip addr        # Show IP addresses
ip a
ifconfig       # Deprecated, may require: sudo apt install net-tools
```

---

### 📝 Configure Networking with Netplan (Default in Ubuntu)

1. Edit the netplan config:

   ```bash
   sudo nano /etc/netplan/01-netcfg.yaml
   ```

2. Example static IP config:

   ```yaml
   network:
     version: 2
     ethernets:
       ens33:
         dhcp4: no
         addresses: [192.168.0.50/24]
         gateway4: 192.168.0.1
         nameservers:
           addresses: [8.8.8.8, 8.8.4.4]
   ```

3. Apply changes:

   ```bash
   sudo netplan apply
   ```

---

### 📝 Configure Networking with nmcli (if NetworkManager is installed)

```bash
nmcli dev status
nmcli con add type ethernet con-name static-ens33 ifname ens33 ip4 192.168.0.50/24 gw4 192.168.0.1
nmcli con mod static-ens33 ipv4.dns "8.8.8.8"
nmcli con up static-ens33
```

---

## 🔹 Useful Network Tools (Both CentOS & Ubuntu)

```bash
ping 8.8.8.8              # Test connectivity
ping google.com           # Test DNS resolution
traceroute google.com     # Trace route (may need: sudo apt/yum install traceroute)
curl ifconfig.me          # Show public IP
```

---

## ✅ Summary

* **CentOS Stream 10 / RHEL 10+** → Uses **NetworkManager** (`nmcli`, `nmtui`, config files in `/etc/NetworkManager/system-connections/`).
* **Ubuntu 24.04** → Uses **netplan** (`/etc/netplan/*.yaml`), but can also use **NetworkManager** on desktop editions.
* Both support `nmcli` if NetworkManager is installed.
* Older **CentOS 7** → Used `/etc/sysconfig/network-scripts/ifcfg-*`.

---

```

---

👉 Do you want me to also include **sample `ifcfg-*` file (CentOS 7 old style)** and a **full sample `.nmconnection` file (CentOS 10/Ubuntu with NetworkManager)** inside the same README for reference?
```
