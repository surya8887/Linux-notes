# 🖧 Network Interface Teaming on Linux (CentOS 10 / RHEL)

This guide explains how to configure **network interface teaming** using **`teamd`** and `nmcli` commands.  
Teaming (a.k.a. NIC bonding) provides **network redundancy** and/or **increased throughput** by combining multiple interfaces into one logical interface.

---

## 📌 Prerequisites

- Root or `sudo` privileges.  
- Installed `teamd` package.  
- NetworkManager enabled and running.  

```bash
# Verify if teamd is available
rpm -qa | grep teamd

# Install if not present
sudo dnf install -y teamd
```

---

## 📊 Check Network Devices

Before creating a team interface, check available network devices:

```bash
nmcli dev status
```

This shows device names (`ens224`, `ens256`, etc.), states, and types.

---

## ⚙️ Create a Team Interface

Create a new team interface named **`team0`** with **active-backup mode** (one interface active, others standby for redundancy):

```bash
nmcli connection add type team con-name team0 ifname team0 \
config '{"runner": {"name": "activebackup"}}'
```

- **type team** → Defines this connection as a team interface.  
- **con-name team0** → Connection name.  
- **ifname team0** → Logical interface name.  
- **runner activebackup** → Only one port active at a time, others act as backup.  

---

## 🌐 Assign IP and DNS

Configure static IPv4 address and DNS for `team0`:

```bash
nmcli connection modify team0 \
ipv4.addresses 192.168.227.140/24 \
ipv4.dns "8.8.8.8 8.8.4.4" \
connection.autoconnect yes \
ipv4.method manual
```

- **ipv4.addresses** → Assigns IP with subnet.  
- **ipv4.dns** → Sets DNS servers.  
- **connection.autoconnect yes** → Enables auto-connect at boot.  
- **ipv4.method manual** → Disables DHCP, uses static IP.  

---

## 🔗 Add Slave Interfaces

Add physical interfaces as slaves (ports) under `team0`:

```bash
nmcli connection add type team-slave con-name team0-port1 ifname ens224 master team0
nmcli connection add type team-slave con-name team0-port2 ifname ens256 master team0
```

- **type team-slave** → Defines connection as a team slave.  
- **ifname ens224 / ens256** → Physical NIC names.  
- **master team0** → Assigns these interfaces to the `team0` master.  

---

## 🔄 Apply Configuration

Reload connections and bring them up:

```bash
nmcli connection reload

nmcli connection up team0
nmcli connection up team0-port1
nmcli connection up team0-port2
```

---

## ✅ Verify Teaming Status

Check the state of the team interface:

```bash
teamdctl team0 state
```

This shows runner mode, active/backup ports, and interface statistics.

---

## 📌 Notes

- **Teaming vs Bonding**: Teaming is newer and more flexible compared to bonding. It uses `teamd` as a user-space daemon.  
- **Active-Backup Mode**: Recommended for **high availability** – if one NIC fails, the other takes over.  
- **Other Modes**: `loadbalance`, `broadcast`, `roundrobin`, etc.  

---

## 🔍 Useful References

- [Red Hat Networking Guide - Teaming](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-network_team_driver)  
- `man teamd.conf`  
- `man nmcli`  

---
