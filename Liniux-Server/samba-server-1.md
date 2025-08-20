# Samba Server Setup on RHEL/Fedora

This guide explains how to set up a **Samba server** on RHEL/Fedora-based systems for both **anonymous** and **secured shares**.

---

## 🔹 Why Use Samba?

* **Cross-Platform Sharing** → Share files and printers between Linux, Windows, and macOS.
* **Active Directory Integration** → Authenticate Linux systems against Windows AD.
* **Flexible Access Control** → Anonymous (public) and secured (user-based) shares.
* **Widely Used** → Default protocol (SMB/CIFS) for enterprise networks.

---

## 🔹 Protocols, Ports & Services

### Protocols

* **SMB (Server Message Block):** File/printer sharing protocol (modern systems use TCP/445).
* **NetBIOS:** Legacy support for name resolution and browsing.

### Ports Used by Samba

| Port        | Protocol | Service | Purpose                                       |
| ----------- | -------- | ------- | --------------------------------------------- |
| **137/UDP** | NetBIOS  | nmbd    | NetBIOS Name Service – resolves names to IPs. |
| **138/UDP** | NetBIOS  | nmbd    | NetBIOS Datagram Service – browsing shares.   |
| **139/TCP** | NetBIOS  | smbd    | SMB over NetBIOS.                             |
| **445/TCP** | SMB      | smbd    | Direct SMB over TCP (modern).                 |

### Services

| Service      | Description                                     | Ports        |
| ------------ | ----------------------------------------------- | ------------ |
| **smbd**     | Handles file sharing, authentication, printing. | TCP 139, 445 |
| **nmbd**     | Provides NetBIOS name resolution, browsing.     | UDP 137, 138 |
| **winbindd** | Integrates Linux with Windows AD.               | Dynamic      |

---

## 🔹 1. Verify Networking & Hostname

```bash
ip a

hostnamectl set-hostname sambaserver.surya.local
hostnamectl
```

---

## 🔹 2. Enable Required Repositories

```bash
dnf repolist all
```

---

## 🔹 3. Install Samba Packages

```bash
dnf install samba samba-common samba-client -y
```

Check Samba workstation config:

```bash
net config workstation
```

---

## 🔹 4. Configure Anonymous Share

Create directory:

```bash
mkdir -p /srv/samba/anonymous
```

Set permissions:

```bash
chmod -R 0755 /srv/samba/anonymous
chown -R nobody:nobody /srv/samba/anonymous
```

Verify & set SELinux context:

```bash
ls -ldZ /srv/samba/anonymous
chcon -t samba_share_t /srv/samba/anonymous
```

---

## 🔹 5. Configure Secured Share

Create directory:

```bash
mkdir -p /srv/samba/secured
```

Create group & user:

```bash
groupadd sambashare
useradd -M -d /srv/samba/secured -s /sbin/nologin sambauser -G sambashare
passwd sambauser
smbpasswd -a sambauser
```

Set permissions:

```bash
chown -R sambauser:sambashare /srv/samba/secured
chmod -R 0770 /srv/samba/secured

chcon -t samba_share_t /srv/samba/secured
```

---

## 🔹 6. Backup & Edit Samba Config

Backup config:

```bash
cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

Edit:

```bash
vim /etc/samba/smb.conf
```

Example configuration:

```ini
[global]
    workgroup = WORKGROUP
    server string = Samba Server %v
    netbios name = rhel10
    security = user
    map to guest = Bad User
    dns proxy = no

[Anonymous]
    path = /srv/samba/anonymous
    browsable = yes
    writable = yes
    guest ok = yes
    read only = no

[Secured]
    path = /srv/samba/secured
    valid users = @sambashare
    browsable = yes
    writable = yes
    guest ok = no
    read only = no
```

---

## 🔹 7. Verify Config

```bash
testparm
```

---

## 🔹 8. Configure Firewall

```bash
firewall-cmd --permanent --add-service=samba
firewall-cmd --reload
```

---

## 🔹 9. Enable and Start Services

```bash
systemctl enable smb --now
systemctl enable nmb --now

systemctl status smb
systemctl status nmb
```

---

## 🔹 10. Test Access

From a client machine:

```bash
# Anonymous share
smbclient -L //sambaserver.surya.local/Anonymous -U guest
smbclient //sambaserver.surya.local/Anonymous -U guest

# Secured share
smbclient //sambaserver.surya.local/Secured -U sambauser
```

Or mount:

```bash
mount -t cifs //sambaserver.surya.local/Anonymous /mnt -o guest
mount -t cifs //sambaserver.surya.local/Secured /mnt -o username=sambauser
```

---

## 🔹 Troubleshooting

* **Check SELinux:**

  ```bash
  getsebool -a | grep samba
  setsebool -P samba_enable_home_dirs on
  ```
* **Logs:** `/var/log/samba/`
* **Manual:** `man smb.conf`

---

✅ Samba is now configured with both **Anonymous** and **Secured** shares, ready for production use.
