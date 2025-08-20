# 🖧 Linux Networking & Routing Configuration Guide (CentOS 10 / Ubuntu 24.04)

This document explains how to configure **routing** and **network interfaces** using  
`ip`, `route`, `netstat`, and `nmcli` commands. It also highlights **important directories and file locations** for persistent configuration.

---

## 📂 Important Directories & File Locations

| OS / Tool | Path / File | Purpose |
|-----------|-------------|---------|
| **CentOS / RHEL (NetworkManager)** | `/etc/NetworkManager/system-connections/` | Stores connection profiles in `.nmconnection` format |
| **CentOS (Legacy)** | `/etc/sysconfig/network-scripts/ifcfg-*` | Old interface config files (deprecated in CentOS 10) |
| **Ubuntu (Netplan)** | `/etc/netplan/*.yaml` | Netplan YAML configs for networking |
| **Both (Hostname)** | `/etc/hostname` | System hostname |
| **Both (Hosts File)** | `/etc/hosts` | Local hostname-to-IP mappings |
| **Both (DNS Resolution)** | `/etc/resolv.conf` | DNS servers (managed by NetworkManager/systemd-resolved) |

---

## 📊 Check Current Routing Table

```bash
# Show routing table
ip route
ip r

# Show routing table (old command)
route -n

# Show routing table with netstat
netstat -rn
```

- **ip route / ip r** → Preferred modern command for routing.  
- **route -n** → Legacy command (numeric output, avoids DNS lookup).  
- **netstat -rn** → Alternative to view routing table.  

---

## ➕ Add a Static Route

```bash
ip route add 10.0.2.0/24 via 192.168.43.223 dev ens160
```

- **10.0.2.0/24** → Destination network.  
- **via 192.168.43.223** → Gateway (next hop).  
- **dev ens160** → Outgoing interface.  

✅ This creates a **temporary static route** (lost after reboot).  

---

## ➖ Delete a Static Route

```bash
ip route del 10.0.2.0/24 via 192.168.43.223 dev ens160
```

Removes the route from the table.  

---

## 📌 Show Current Routes

```bash
ip route show
```

---

## 🔄 Apply & Reload Network Configurations

```bash
nmcli connection reload        # Reload all network connections
systemctl restart NetworkManager  # Restart NetworkManager service
```

---

## 📑 Persistent Routes

For **permanent routes**, edit configs:

- **CentOS 10 / RHEL (NetworkManager)**  
  Add routes in connection profile files:  
  `/etc/NetworkManager/system-connections/<connection>.nmconnection`  
  Example section:  
  ```ini
  [ipv4]
  method=manual
  address1=192.168.1.100/24,192.168.1.1
  dns=8.8.8.8;8.8.4.4;
  routes=10.0.2.0/24 192.168.43.223
  ```

- **Ubuntu 24.04 (Netplan)**  
  Add static routes in YAML:  
  `/etc/netplan/01-netcfg.yaml`  
  ```yaml
  network:
    version: 2
    ethernets:
      ens160:
        addresses: [192.168.1.100/24]
        gateway4: 192.168.1.1
        nameservers:
          addresses: [8.8.8.8,8.8.4.4]
        routes:
          - to: 10.0.2.0/24
            via: 192.168.43.223
  ```
  Apply changes:  
  ```bash
  sudo netplan apply
  ```

---

## ⚡ Useful Commands Summary

```bash
ip route          # Show routing table
ip route add ...  # Add temporary route
ip route del ...  # Delete route
route -n          # Show legacy routing table
netstat -rn       # Show routing table (netstat)
nmcli dev status  # Check devices
nmcli c reload    # Reload connections
systemctl restart NetworkManager  # Restart service
```

---

## 📚 References

- `man ip-route`  
- `man nmcli`  
- [Linux Routing Guide](https://www.kernel.org/doc/Documentation/networking/ip-sysctl.txt)  
- [Netplan Documentation](https://netplan.io/examples)  

---
