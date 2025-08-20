# Routing Table Configuration & Commands (CentOS / RHEL 10)

This guide explains how to manage routing tables and configure routes on **CentOS/RHEL 10**. It includes essential commands for viewing, adding, and troubleshooting routes.

---

## 🔹 Why Routing is Important

* Defines how network packets travel between subnets/networks.
* Allows multi-network servers (multi-homed) to forward traffic.
* Helps in static routes for private or custom networks.

---

## 🔹 1. View Current Routing Table

```bash
# Using iproute2 (preferred)
ip route show

# Traditional (still works)
netstat -rn

# Detailed info
route -n
```

Example output:

```
default via 192.168.1.1 dev enp0s3 proto dhcp metric 100
192.168.1.0/24 dev enp0s3 proto kernel scope link src 192.168.1.50 metric 100
```

---

## 🔹 2. Add a Static Route (Temporary)

```bash
# Syntax
ip route add <network>/<mask> via <gateway> dev <interface>

# Example: Add route to 10.10.0.0/16 via 192.168.1.1
ip route add 10.10.0.0/16 via 192.168.1.1 dev enp0s3
```

👉 This disappears after reboot.

---

## 🔹 3. Delete a Route

```bash
ip route del <network>/<mask> via <gateway>

# Example
i p route del 10.10.0.0/16 via 192.168.1.1
```

---

## 🔹 4. Persistent Static Routes (RHEL/CentOS 10)

Routes must be saved in **NetworkManager configuration**.

### Method 1: Add Route in Interface Config File

1. Go to:

```bash
/etc/sysconfig/network-scripts/ifcfg-enp0s3
```

2. Add static route line:

```ini
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no

# Example route
IPADDR=192.168.1.50
PREFIX=24
GATEWAY=192.168.1.1

# Add persistent route
NM_CONTROLLED=yes
```

Then create a route file:

```bash
/etc/sysconfig/network-scripts/route-enp0s3
```

Add:

```
10.10.0.0/16 via 192.168.1.1 dev enp0s3
```

Restart networking:

```bash
nmcli connection reload
nmcli connection up enp0s3
```

### Method 2: Using nmcli (Recommended)

```bash
# Add static route to connection
nmcli connection modify enp0s3 +ipv4.routes "10.10.0.0/16 192.168.1.1"

# Verify
nmcli connection show enp0s3 | grep ipv4.routes

# Reactivate connection
nmcli connection down enp0s3; nmcli connection up enp0s3
```

---

## 🔹 5. Default Gateway Configuration

```bash
# View default route
ip route | grep default

# Change default route (temporary)
ip route add default via 192.168.1.1 dev enp0s3

# Remove default route
ip route del default
```

Persistent default gateway with `nmcli`:

```bash
nmcli connection modify enp0s3 ipv4.gateway 192.168.1.1
nmcli connection up enp0s3
```

---

## 🔹 6. Enable IP Forwarding (for routing between networks)

```bash
# Check status
sysctl net.ipv4.ip_forward

# Enable temporarily
sysctl -w net.ipv4.ip_forward=1

# Make persistent
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p
```

---

## 🔹 7. Troubleshooting Commands

* Show routing cache:

```bash
ip route get 8.8.8.8
```

* Trace route path:

```bash
traceroute 8.8.8.8
```

* Monitor packets:

```bash
tcpdump -i enp0s3
```

* Verify connectivity:

```bash
ping -c 4 8.8.8.8
```

---

## ✅ Summary

* Use **`ip route`** to manage routes (modern tool).
* Use **`nmcli`** or **ifcfg/route files** for persistent routes.
* Configure **default gateway** for external connectivity.
* Enable **IP forwarding** if your server routes traffic between networks.

---

🔹 With this, your CentOS/RHEL system can handle **static routing, persistent routes, and act as a router** if needed.
