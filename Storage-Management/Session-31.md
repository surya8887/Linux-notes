markdown
# 📦 Session-21: Storage Management in Linux

This session explains **storage management** in Linux, focusing on **disk types, file systems (ext4, XFS)** and key commands (`fdisk`, `lsblk`, `dmesg`, `dd`, etc.) with **meanings and examples**.

---

## 🔹 1. Storage Management Basics

Linux represents all devices as files under `/dev/`.

Examples:
- `/dev/sda` → First SATA/SCSI hard disk
- `/dev/sdb` → Second SATA/SCSI disk
- `/dev/nvme0n1` → First NVMe SSD

### Storage Types:
- **HDD** → Slower, magnetic storage, cheap.  
- **SSD** → Faster, uses flash memory.  
- **NVMe** → High-speed SSD interface (PCIe).  

---

## 🔹 2. File System Types

A **file system** decides how data is organized on a partition.

### 🗂 **ext4 (Fourth Extended Filesystem)**
- Default in most Linux distros (Ubuntu/Debian).
- Supports up to **1 exabyte volume**.
- Journaling (protects from corruption).
- Efficient for small files.

### ⚡ **XFS**
- High-performance journaling filesystem.
- Optimized for **large files & parallel I/O**.
- Supports online defragmentation/resizing.
- Used in **RHEL, CentOS** enterprise systems.

### Other Types
- **ext2/ext3** → Legacy Linux filesystems.  
- **Btrfs** → Advanced features (snapshots, subvolumes).  
- **ZFS** → Enterprise-level reliability.  

---

## 🔹 3. Disk Partitioning & Info Commands

### View Command Manuals
bash
man fdisk     # Opens manual for fdisk command
man dmesg     # Opens manual for dmesg command


### Check Disk Partitions

```bash
fdisk -l               # List all disks and partitions
fdisk -l | grep nvme   # Show only NVMe drives
fdisk /dev/sda         # Open fdisk utility for /dev/sda
```

Inside `fdisk`:

* `m` → help
* `p` → print partition table
* `n` → new partition
* `d` → delete partition
* `w` → write/save changes

### Check Kernel Messages

```bash
dmesg              # View system boot & hardware logs
dmesg | grep sda   # Show messages related to sda disk
```

### Hardware & Storage Information

```bash
lshw | grep nvme    # Show NVMe device info
lsscsi              # List SCSI devices
cat /proc/scsi/scsi # Show detected SCSI devices
lsblk               # Display block devices and mountpoints
```

---

## 🔹 4. Erasing a Hard Disk (⚠️ Dangerous Commands)

### Method 1: Using `badblocks`

```bash
badblocks -ws /dev/sda
```

* `-w` → write test mode (DESTROYS DATA).
* `-s` → show progress.

### Method 2: Using `dd`

```bash
dd if=/dev/zero of=/dev/sda bs=1M status=progress
```

* `if=/dev/zero` → input file = stream of zeros.
* `of=/dev/sda` → write to disk.
* `bs=1M` → block size = 1MB.
* `status=progress` → show progress.

👉 More secure option (random overwrite):

```bash
dd if=/dev/urandom of=/dev/sda bs=1M status=progress
```

---

## 🔹 5. Creating & Mounting File Systems

After partitioning (`fdisk`), format the partition.

### Format Partition

```bash
mkfs.ext4 /dev/sda1   # Create ext4 filesystem on partition
mkfs.xfs /dev/sda1    # Create XFS filesystem on partition
```

### Mount Partition

```bash
mount /dev/sda1 /mnt    # Mount temporarily to /mnt
umount /mnt             # Unmount
```

### Permanent Mount via `/etc/fstab`

Add this line inside `/etc/fstab`:

```
/dev/sda1   /data   ext4   defaults   0  2
```

This ensures `/dev/sda1` mounts automatically on boot to `/data`.

---

## 🔹 6. Complete Workflow

1. **Detect disks**

   ```bash
   lsblk
   fdisk -l
   ```
2. **Check kernel logs**

   ```bash
   dmesg | grep sda
   ```
3. **Partition disk**

   ```bash
   fdisk /dev/sda
   ```
4. **Format filesystem**

   ```bash
   mkfs.ext4 /dev/sda1
   ```
5. **Mount & use**

   ```bash
   mount /dev/sda1 /mnt
   ```

---

## 🔹 7. Visual Overview

```
[ Physical Disk (/dev/sda) ]
          │
          ├── [ Partition 1 → /dev/sda1 ]
          │          │
          │          └── [ ext4 or xfs filesystem ]
          │                       │
          │                       └── Mounted at /mnt or /data
          │
          ├── [ Partition 2 → /dev/sda2 ]
```

---

## ✅ Summary

* Use `fdisk` / `lsblk` for partitioning.
* Format partitions using `mkfs.ext4` or `mkfs.xfs`.
* Mount partitions temporarily (`mount`) or permanently (`/etc/fstab`).
* Use `badblocks` or `dd` for erasing disks.

---

## 📚 References

* Linux `man` pages (`man fdisk`, `man mkfs.xfs`)
* RHEL & Ubuntu storage guides
* Linux Documentation Project

---

```

---

👉 This `README.md` is ready to drop into your **GitHub repo or notes**.  

Do you want me to also prepare a **short cheat sheet version** (just commands + one-line meaning) alongside this detailed README for quick revision?
```
