# Network Sniffing & Secure Shell (SSH) Configuration in Linux

This guide provides notes and commands for **network sniffing** (packet capture/analysis) and **SSH configuration** (secure remote login) in CentOS/RHEL 10.

---

## 🔹 Why This Matters

* **Network Sniffing** → Helps troubleshoot connectivity, monitor traffic, and detect security issues.
* **SSH** → Provides secure encrypted remote login, replacing insecure protocols like Telnet.

---

# Part 1: Network Sniffing

## 1. Install Sniffing Tools

```bash
dnf install tcpdump -y
dnf install wireshark wireshark-cli -y   # optional, GUI on desktop
```

## 2. Capture Packets with tcpdump

```bash
# Capture all traffic on interface
tcpdump -i enp0s3

# Capture first 100 packets
tcpdump -i enp0s3 -c 100

# Capture and save to file
tcpdump -i enp0s3 -w capture.pcap

# Read from saved file
tcpdump -r capture.pcap
```

## 3. Useful tcpdump Filters

```bash
# Capture only ICMP (ping)
tcpdump -i enp0s3 icmp

# Capture traffic from specific host
tcpdump -i enp0s3 host 192.168.1.10

# Capture packets on port 80 (HTTP)
tcpdump -i enp0s3 port 80

# Capture traffic between two hosts
tcpdump -i enp0s3 src 192.168.1.5 and dst 192.168.1.20
```

## 4. Analyze with Wireshark (GUI)

```bash
wireshark capture.pcap
```

👉 Wireshark provides deep inspection, filtering, and graphical analysis.

---

# Part 2: Secure Shell (SSH)

## 1. Install SSH Server

```bash
dnf install openssh-server -y
```

Enable & start service:

```bash
systemctl enable sshd --now
systemctl status sshd
```

## 2. SSH Client Usage

```bash
# Connect to remote server
ssh username@192.168.1.50

# Connect with custom port
ssh -p 2222 username@192.168.1.50

# Copy files with scp
scp file.txt username@192.168.1.50:/home/username/

# Sync directories with rsync over SSH
rsync -avz /local/path username@192.168.1.50:/remote/path
```

## 3. SSH Configuration File

Located at:

```bash
/etc/ssh/sshd_config
```

Important directives:

```ini
Port 22
PermitRootLogin no
PasswordAuthentication yes
PubkeyAuthentication yes
```

Restart SSH after changes:

```bash
systemctl restart sshd
```

## 4. SSH Key-Based Authentication

Generate key pair:

```bash
ssh-keygen -t rsa -b 4096
```

Copy public key to server:

```bash
ssh-copy-id username@192.168.1.50
```

Now login without password:

```bash
ssh username@192.168.1.50
```

## 5. Hardening SSH

* Change default port (avoid 22).
* Disable root login: `PermitRootLogin no`
* Use only key authentication: `PasswordAuthentication no`
* Limit users: `AllowUsers adminuser`
* Enable firewall rule:

```bash
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload
```

---

# Troubleshooting

* Check service status:

```bash
systemctl status sshd
```

* Test port availability:

```bash
ss -tulnp | grep sshd
```

* Check logs:

```bash
tail -f /var/log/secure
```

* Verify packet capture:

```bash
tcpdump port 22 -i enp0s3
```

---

## ✅ Summary

* **Network sniffing** with `tcpdump` & Wireshark helps diagnose connectivity issues.
* **SSH** provides secure remote management.
* Combine both: sniff SSH traffic to verify encryption and connections.
* Always harden SSH for production systems.

---
