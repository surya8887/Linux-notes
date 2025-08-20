Perfect 👍 You want me to prepare a **professional `README.md` file** that includes all the **nmcli commands** I gave above, properly formatted, so you can keep it as a permanent reference.

Here’s the complete `README.md` file for you:

---

````markdown
# 🌐 nmcli Complete Professional Command Reference

This README provides a **comprehensive guide** to using `nmcli` (NetworkManager CLI) for managing networking on **CentOS** and **Ubuntu**.  
It includes commands for **IPv4, IPv6, devices, connections, Wi-Fi, DNS, routes, VPN, VLAN, Bonding, Bridging, Teaming, and troubleshooting**.

---

## 📌 General Notes

- **CentOS/RHEL config files** → `/etc/NetworkManager/system-connections/`
- **Ubuntu config files** → `/etc/netplan/` (for netplan) + `/etc/NetworkManager/system-connections/` (if using NetworkManager)
- Service control:
  ```bash
  sudo systemctl enable NetworkManager
  sudo systemctl start NetworkManager
  sudo systemctl status NetworkManager
````

---

## 🔹 General NetworkManager Info

```bash
nmcli -h                                 # Show help
nmcli -t -f ALL general                  # Show all general info
nmcli general status                     # Show overall NM status
nmcli general hostname                   # Get hostname
nmcli general hostname new-hostname       # Set hostname
nmcli networking {on|off}                # Enable/disable networking
nmcli radio all {on|off}                 # Enable/disable all radios
nmcli radio wifi {on|off}                # Enable/disable Wi-Fi
nmcli radio wwan {on|off}                # Enable/disable mobile broadband
nmcli monitor                            # Monitor NM events in real time
nmcli general permissions                # Show NM permissions
nmcli general logging level DEBUG domains ALL   # Debug logging
```

---

## 🔹 Device Management

```bash
nmcli device status                      # Show all devices
nmcli device show                        # Detailed info (all devices)
nmcli device show eth0                   # Detailed info (specific device)
nmcli device connect eth0                # Connect device
nmcli device disconnect eth0             # Disconnect device
nmcli device reapply eth0                # Reapply configuration
nmcli device delete eth0                 # Remove unmanaged device
nmcli device wifi list                   # List Wi-Fi networks
nmcli device wifi rescan                 # Rescan for Wi-Fi
nmcli device set eth0 managed yes        # Set device under NM control
```

---

## 🔹 Connection Management

```bash
nmcli connection show                        # Show all saved connections
nmcli connection show --active               # Show active connections
nmcli connection add type ethernet ifname eth0 con-name my-eth
nmcli connection add type wifi ifname wlan0 ssid "SSID" con-name mywifi
nmcli connection add type vlan ifname vlan10 dev eth0 id 10 con-name vlan10
nmcli connection add type bond ifname bond0 mode active-backup con-name bond0
nmcli connection add type bridge ifname br0 con-name br0
nmcli connection add type team ifname team0 con-name team0
nmcli connection add type vpn vpn-type openvpn con-name myvpn
nmcli connection up my-eth                   # Bring up a connection
nmcli connection down my-eth                 # Bring down a connection
nmcli connection reload                      # Reload all connections
nmcli connection modify my-eth autoconnect yes
nmcli connection delete my-eth               # Delete connection
```

---

## 🔹 IPv4 Configuration

```bash
nmcli connection modify my-eth ipv4.method auto             # DHCP
nmcli connection modify my-eth ipv4.method manual           # Static
nmcli connection modify my-eth ipv4.addresses 192.168.1.100/24
nmcli connection modify my-eth ipv4.gateway 192.168.1.1
nmcli connection modify my-eth ipv4.dns "8.8.8.8 8.8.4.4"
nmcli connection modify my-eth ipv4.ignore-auto-dns yes
nmcli connection modify my-eth ipv4.routes "192.168.2.0/24 192.168.1.1"
```

---

## 🔹 IPv6 Configuration

```bash
nmcli connection modify my-eth ipv6.method auto             # SLAAC/DHCPv6
nmcli connection modify my-eth ipv6.method manual           # Static
nmcli connection modify my-eth ipv6.method ignore           # Disable IPv6
nmcli connection modify my-eth ipv6.addresses 2001:db8::100/64
nmcli connection modify my-eth ipv6.gateway 2001:db8::1
nmcli connection modify my-eth ipv6.dns "2001:4860:4860::8888"
nmcli connection modify my-eth ipv6.ignore-auto-dns yes
nmcli connection modify my-eth ipv6.routes "2001:db8:abcd::/64 2001:db8::1"
```

---

## 🔹 Routes (IPv4 + IPv6)

```bash
nmcli connection modify my-eth +ipv4.routes "192.168.2.0/24 192.168.1.1"
nmcli connection modify my-eth +ipv6.routes "2001:db8:abcd::/64 2001:db8::1"
nmcli -f IP4.ROUTE,IP6.ROUTE device show eth0
```

---

## 🔹 Wi-Fi Management

```bash
nmcli device wifi list                                   # Show Wi-Fi networks
nmcli device wifi connect "SSID" password "PASSWORD"
nmcli device wifi hotspot ifname wlan0 ssid myhotspot password "mypassword"
nmcli connection modify mywifi wifi-sec.key-mgmt wpa-psk
nmcli connection modify mywifi wifi-sec.psk "PASSWORD"
```

---

## 🔹 Bonding / Bridging / VLAN / Teaming

```bash
# Bond
nmcli connection add type bond ifname bond0 mode active-backup con-name bond0
nmcli connection add type bond-slave ifname eth0 master bond0
nmcli connection add type bond-slave ifname eth1 master bond0

# Bridge
nmcli connection add type bridge ifname br0 con-name br0
nmcli connection add type bridge-slave ifname eth0 master br0

# VLAN
nmcli connection add type vlan ifname vlan10 dev eth0 id 10 con-name vlan10

# Team
nmcli connection add type team ifname team0 con-name team0
nmcli connection add type team-slave ifname eth1 master team0
```

---

## 🔹 VPN Connections

```bash
nmcli connection add type vpn vpn-type openvpn con-name myvpn \
    -- vpn.data "gateway=vpn.example.com,username=user"
nmcli connection up myvpn
nmcli connection down myvpn
```

---

## 🔹 DNS Settings

```bash
nmcli connection modify my-eth ipv4.dns "1.1.1.1 8.8.8.8"
nmcli connection modify my-eth ipv6.dns "2606:4700:4700::1111"
nmcli connection modify my-eth ipv4.ignore-auto-dns yes
nmcli connection modify my-eth ipv6.ignore-auto-dns yes
```

---

## 🔹 Useful Queries

```bash
nmcli -t -f all device show                  # All device details
nmcli -p device show                         # Pretty output
nmcli device show eth0 | grep IP4            # IPv4 details only
nmcli device show eth0 | grep IP6            # IPv6 details only
```

---

## 🔹 Advanced & Troubleshooting

```bash
nmcli connection reload                      # Reload configs
nmcli device reapply eth0                    # Apply new config
nmcli networking connectivity check          # Check connectivity
nmcli general logging level INFO domains ALL # Reset logging
```

---

## ✅ Summary

This guide covers:

* **General commands** (status, logging, monitoring)
* **Devices** (show, connect, disconnect, reapply)
* **Connections** (add, modify, up, down, delete)
* **IPv4 + IPv6 config**
* **Routes (static IPv4 + IPv6)**
* **Wi-Fi management**
* **Bonding, Bridging, VLAN, Teaming**
* **VPN setup**
* **DNS configuration**
* **Troubleshooting commands**

---

📌 Keep this README handy for **Linux networking administration on CentOS, RHEL, and Ubuntu**.

```

---

👉 Do you want me to also add **example real-world configurations** (like a static IPv6 + IPv4 setup example for `eth0` and a Wi-Fi + VPN config) inside this README for quick copy-paste use?
```
