Great ✨ You’ve outlined **Session 4, 5, and 6 topics** (basic Linux command usage). I’ll create a **clean, structured, professional `README.md`** that explains these commands with examples, formatted for learning and reference.

---

# 📘 Linux Commands – Training Notes (Session 4–5)

This document covers essential **Linux command basics** with explanations, syntax, and examples. Useful for beginners, DevOps engineers, and system administrators.

---

## 📌 Command Format

```bash
command [options] [arguments]
```

* **command** → tool you want to run
* **options** → modify behavior (`-l`, `--help`, etc.)
* **arguments** → target (file, directory, etc.)

---

## 🟦 Session 4 – Command Documentation & Navigation

### `man` – Manual Pages

```bash
man ls
man cd
man mkdir
```

Shows detailed documentation of commands.

Navigation:

* `↑ / ↓` → scroll
* `/word` → search
* `q` → quit

---

### `whatis` – One-Line Summary

```bash
whatis route
whatis ls
```

---

### `whereis` – Locate Binary/Source/Man Page

```bash
whereis route
whereis ls
```

---

### `man-db` – Manual Page Database

If `whatis` or `whereis` doesn’t work, update database:

```bash
mandb
```

---

## 🟦 Session 5 – Filesystem Navigation & Listing

### `pwd` – Print Working Directory

```bash
pwd
```

### `cd` – Change Directory

```bash
cd /etc        # absolute path
cd var/log     # relative path
cd ~           # home directory
cd -           # previous directory
```

* `.` → current directory
* `..` → parent directory

---

### `ls` – List Files

```bash
ls            # simple list
ls -a         # show hidden files
ls -l         # long format
ls -lh        # human-readable sizes
ls var/log    # specific directory
```

---

### `mkdir` – Make Directory

```bash
mkdir mydir
mkdir -p /tmp/test/logs   # create parent directories
```

---

### `ls -ld` – Directory Info

```bash
ls -ld mydir
```

---

### `rm` – Remove Files/Directories

```bash
rm -f file.txt       # force delete file
rm -rf mydir/        # recursive delete directory
rm -I file.txt       # prompt before deleting many files
```

---

### `tree` – Show Directory Structure

```bash
tree
tree /etc
```

---

### `cal` – Calendar

```bash
cal > cal.txt   # save calendar output to file
```

---

## 🟦 Session 6 – File Operations & Links

### `touch` – Create File

```bash
touch Linux.txt
touch {1..100}.txt              # create 100 files
touch -t 201005100615 file.txt  # custom timestamp (YYYYMMDDhhmm)
```

---

### `file` – Show File Type

```bash
file raj.js
```

---

### Hard Link

```bash
ln original.txt hardlink.txt
```

* Points to same inode.
* File remains if original is deleted.

---

### Soft Link (Symbolic Link)

```bash
ln -s original.txt softlink.txt
```

* Works like a shortcut.
* Broken if original is deleted.

---

### Copy Files

```bash
cp anaconda-ks.cfg new.txt
cp -r /var/log/merge /tmp/Linux/monitor
```

---

### Move & Rename

```bash
mv anaconda.cfg kickstart.txt   # rename
mv kickstart.txt /tmp/          # move file
```

---

### Listing Files with `ll`

```bash
ll    # alias to ls -l
```

---

## 📌 Summary Table

| Command   | Purpose                  | Example                   |
| --------- | ------------------------ | ------------------------- |
| `man`     | Manual pages             | `man ls`                  |
| `whatis`  | One-line description     | `whatis pwd`              |
| `whereis` | Locate binary/man/source | `whereis bash`            |
| `pwd`     | Show current directory   | `pwd`                     |
| `cd`      | Change directory         | `cd /etc`                 |
| `ls`      | List files               | `ls -lh`                  |
| `mkdir`   | Create directories       | `mkdir -p /tmp/test/logs` |
| `rm`      | Remove files/directories | `rm -rf dir/`             |
| `tree`    | Directory structure      | `tree /etc`               |
| `touch`   | Create empty files       | `touch file.txt`          |
| `file`    | Identify file type       | `file script.sh`          |
| `ln`      | Hard/soft links          | `ln -s file link.txt`     |
| `cp`      | Copy files/directories   | `cp -r dir /tmp/`         |
| `mv`      | Move/rename files        | `mv file1 file2`          |

---


✅ This is a **professional training-style README** for Session 4–6, with syntax, explanation, and examples.

Would you like me to **continue this style for all future sessions (6, 8, …)**
so you get a **complete Linux training handbook** in one README?
