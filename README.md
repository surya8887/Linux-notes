# 🐧 Linux Notes

This repository contains my **Linux learning notes**, including commands, file paths, configurations, and important concepts.  
It is written in a **cheat-sheet + guide** style for quick reference.  

---

## 📂 Directory Structure
```
Linux-Notes/
 ├── README.md   # Main documentation
 ├── commands/   # Commands & examples
 ├── configs/    # Configuration file references
 ├── permissions/ # File ownership & permission notes
 └── networking/ # Networking concepts and commands
```

---

## 📌 Topics Covered

### 1. Basic Commands
- `pwd` → Print working directory  
- `ls -l` → List files with details  
- `cd /path` → Change directory  
- `touch file.txt` → Create file  
- `mkdir dir` → Create directory  

### 2. File Operations
- `cp source dest` → Copy files  
- `mv old new` → Move/Rename file  
- `rm file.txt` → Delete file  
- `cat file.txt` → View file content  
- `nano file.txt` / `vim file.txt` → Edit file  

### 3. Permissions & Ownership
- `ls -l` → View permissions  
- `chmod 755 file` → Change permissions  
- `chown user file` → Change owner  
- `chgrp group file` → Change group  

### 4. User Management
- `whoami` → Current user  
- `id` → User ID & Group ID  
- `adduser username` → Create user  
- `passwd username` → Change password  
- `usermod -aG group user` → Add user to group  

### 5. Networking
- `ip a` → Show IP address  
- `nmcli dev status` → Show device status  
- `ping google.com` → Test connectivity  
- `curl ifconfig.me` → Check public IP  
- Config files:
  - **CentOS**: `/etc/sysconfig/network-scripts/`  
  - **Ubuntu**: `/etc/netplan/`  

### 6. Disk Management
- `lsblk` → List block devices  
- `df -h` → Show disk usage  
- `du -sh *` → Disk usage by directory  
- `mount /dev/sdX /mnt` → Mount disk  
- `umount /mnt` → Unmount disk  

### 7. Processes & Services
- `ps aux` → Running processes  
- `top` / `htop` → Monitor system usage  
- `kill -9 PID` → Kill process  
- `systemctl start service` → Start service  
- `systemctl enable service` → Enable auto-start  

### 8. Package Management
- **Ubuntu/Debian**:  
  - `apt update && apt upgrade`  
  - `apt install package`  
- **CentOS/RHEL**:  
  - `yum install package`  
  - `dnf install package`  

---

## 🗂️ Useful File Paths
- `/etc/passwd` → User accounts  
- `/etc/shadow` → Passwords (hashed)  
- `/etc/group` → Groups  
- `/var/log/` → System logs  
- `/etc/hostname` → Hostname  
- `/etc/fstab` → Filesystem mount config  

---

## 📖 References
- [Linux Handbook](https://linuxhandbook.com)  
- [Ubuntu Docs](https://ubuntu.com/server/docs)  
- [Red Hat Docs](https://access.redhat.com/documentation)  

---

## ✍️ Author
Created by **Surya Kumar** (Learning Linux & System Administration 🚀)

