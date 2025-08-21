# Firewall Management in RHEL 10

In **RHEL 10**, the firewall continues to be managed by **nftables** (Netfilter tables), which replaced the legacy **iptables** from RHEL 7 and older versions.

---

## 🔹 What is nftables?

* Replacement for `{ip, ip6, arp, eb}tables`.
* Provides a **new in-kernel packet classification framework** based on a virtual machine (VM).
* Uses the **`nft`** command-line tool for management.
* Provides **libnftables** (high-level userspace library with JSON support).

**Main Features:**

* Unified syntax for IPv4, IPv6, ARP, bridge traffic.
* Smaller kernel codebase (intelligence shifted to userspace).
* High performance using **maps** and **concatenations**.
* Backward compatibility with iptables.

---

## 🔹 Installing & Managing FirewallD

```bash
# Check enabled repositories
dnf repolist all

# Check installed firewall packages
rpm -qa | grep firewall

# Remove firewall
dnf remove firewalld -y

# Install firewall again
dnf install firewalld -y

# Start & enable firewalld
systemctl start firewalld
systemctl enable firewalld
systemctl status firewalld

# Verify firewall state
firewall-cmd --state
```

---

## 🔹 Zones in Firewalld

Zones are **predefined sets of rules** applied to network interfaces.

### Available Zones

* **block** – Reject all incoming connections.
* **dmz** – Demilitarized zone (limited access).
* **drop** – Drop all incoming, allow only outgoing.
* **external** – For router/NAT configurations.
* **home** – Trusted LAN (limited ports).
* **internal** – Trusted internal networks.
* **public** – Untrusted networks (default for servers).
* **trusted** – All connections accepted (⚠️ not recommended).
* **work** – Workplace networks.

### Commands

```bash
# Get default zone
firewall-cmd --get-default-zone

# List all zones
firewall-cmd --get-zones

# Show active zones
firewall-cmd --get-active-zones
```

---

## 🔹 Managing Services & Ports

### List services

```bash
firewall-cmd --get-services
```

### Add/remove service

```bash
# Add HTTP service to public zone (temporary)
firewall-cmd --zone=public --add-service=http

# Add permanently
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --reload

# Remove service
firewall-cmd --permanent --zone=public --remove-service=http
firewall-cmd --reload
```

### Add/remove ports

```bash
# Open TCP 80 permanently
firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --reload

# Remove TCP 23
firewall-cmd --permanent --remove-port=23/tcp
firewall-cmd --reload

# List allowed ports
firewall-cmd --list-ports
```

---

## 🔹 Custom Services

Service files are located in `/usr/lib/firewalld/services/`. To create a custom service:

```bash
cp /usr/lib/firewalld/services/ssh.xml /etc/firewalld/services/example.xml
vim /etc/firewalld/services/example.xml
```

**Example:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>Example Service</short>
  <description>Custom service example.</description>
  <port protocol="tcp" port="7777"/>
  <port protocol="udp" port="8888"/>
</service>
```

Reload firewall to apply:

```bash
firewall-cmd --reload
firewall-cmd --get-services | grep example
```

---

## 🔹 Interface & Zone Management

```bash
# Assign interface to zone
firewall-cmd --zone=home --change-interface=ens33

# Verify active zones
firewall-cmd --get-active-zones
```

---

## 🔹 Port Forwarding & Masquerading

```bash
# Forward TCP 80 to 8080
firewall-cmd --zone=public --add-forward-port=port=80:proto=tcp:toport=8080 --permanent
firewall-cmd --reload

# Enable masquerading
firewall-cmd --zone=public --add-masquerade --permanent
firewall-cmd --reload
```

---

## 🔹 Rich Rules

Rich rules allow advanced configurations.

**Example – Allow SSH only from one IP:**

```bash
firewall-cmd --permanent --zone=public --add-rich-rule 'rule family="ipv4" source address="10.8.0.8" port port=22 protocol=tcp accept'
firewall-cmd --reload

# List rich rules
firewall-cmd --list-rich-rules --permanent
```

---

## 🔹 Example: Web Server with Firewall

```bash
# Install & start Apache
 dnf install httpd -y
 systemctl start httpd
 systemctl enable httpd

# Test locally
 echo "Nehra Classes Are Awesome !!!" > /var/www/html/index.html
 curl localhost

# Open HTTP in firewall
 firewall-cmd --permanent --add-service=http
 firewall-cmd --reload
 firewall-cmd --list-all
```

Now access the server from another machine using its IP.

---

## 🔹 Firewall GUI Tool

```bash
dnf install -y firewall-config
```

Use GUI to manage firewall easily.

---

## 🔹 Disable Firewall (Not Recommended)

```bash
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld
```

Now `firewall-cmd` will not work.

---

## 📌 References

* [nftables Project](https://www.netfilter.org/projects/nftables/)
* [DigitalOcean – How to set up a firewall using firewalld](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-using-firewalld-on-centos-10)
