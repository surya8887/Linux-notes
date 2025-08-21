# that explains all these Linux commands, permissions, `umask`, and file operations clearly with **comments, definitions, and examples**. 

---

````markdown
# Linux File Permissions and Commands

This document explains basic Linux file handling, ownership, and permission concepts with examples.

---

## 1. File Creation and Ownership

```bash
cal > cal.txt                 # Save calendar output to file
chown surya cal.txt           # Change file owner to 'surya'
chgrp developer cal.txt       # Change file group to 'developer'
chown root:root cal.txt       # Change both owner and group to 'root'
````

---

## 2. File Permissions (chmod)

* **Permission Types:**

  * `r` → Read (4)
  * `w` → Write (2)
  * `x` → Execute (1)

* **Permission Levels:**

  * `u` → User (Owner)
  * `g` → Group
  * `o` → Others
  * `a` → All

### Examples:

```bash
chmod 600 cal.txt             # Owner: rw, Group: -, Others: -
chmod 777 cal.txt             # Everyone: rwx (full permission)

chmod u-x cal.txt             # Remove execute for user
chmod g-x cal.txt             # Remove execute for group
chmod o-x cal.txt             # Remove execute for others

chmod u+x cal.txt             # Add execute for user
chmod o+x cal.txt             # Add execute for others

chmod u=rw,g=r,o=r cat.txt    # Explicit: user=rw, group=r, others=r
```

---

## 3. Default File Permission (umask)

* **Default Permission:**

  * Files: `666` (rw-rw-rw-) → Max allowed
  * Directories: `777` (rwxrwxrwx)

* **Umask**: A filter that removes permissions from the default.

```bash
touch newfile.txt             # Creates with default 644 (if umask=0022)

umask 0022                    # Set default umask
touch file.txt                # Creates file with 644 permission
```

### Calculation:

```
Max file permission  = 666
Umask                = 022
Effective permission = 666 - 022 = 644

Max directory perm   = 777
Umask                = 022
Effective permission = 777 - 022 = 755
```

---

## 4. Copying Files with Attributes

```bash
cp -p /temp/newdir/t .        # Copy while preserving permissions, ownership, and timestamps
```

---

## 5. Summary of Key Points

* `chown`: Change file owner
* `chgrp`: Change file group
* `chmod`: Change file permissions
* `umask`: Defines default permissions for newly created files/directories
* **Max Permissions**:

  * File: `666` (no execute by default)
  * Directory: `777`

---

## 6. Permissions Reference Table

| Symbolic  | Octal | Meaning                        |
| --------- | ----- | ------------------------------ |
| rwx------ | 700   | Full for user                  |
| rw-r--r-- | 644   | User rw, others read           |
| rwxr-xr-x | 755   | User rwx, others r-x           |
| rw------- | 600   | User rw only                   |
| rw-rw-rw- | 666   | Everyone rw                    |
| rwxrwxrwx | 777   | Everyone rwx (not recommended) |

---

✍️ **Author**: Surya Kumar
📖 **Purpose**: Quick reference for Linux file handling and permission management.

```

---

👉 Do you want me to also add **diagrams/illustrations (like tree view of permissions)** in the README for better visualization, or keep it only text-based?
```
