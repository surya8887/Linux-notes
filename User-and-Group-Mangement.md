# Linux User & Group Management + Environment Cheat Sheet
````markdown
# 📒 Linux User & Group Management + Environment Cheat Sheet
**Author:** Surya Kumar  

---

## 1. User Information
```bash
whoami                          # Show current logged-in user
who                             # Show all logged-in users
who am i                        # Show original login user & terminal
w                               # Show logged-in users with activity
id                              # Show UID, GID, and groups
````

---

## 2. Switching Users

```bash
su username                     # Switch user (keep current env)
su - username                   # Switch user & load env
su - root                       # Login as root (needs root password)
sudo su -                       # Login as root without root password (if sudo rights)
exit                            # Exit session
```

---

## 3. Add & Delete Users

```bash
useradd -m -d /home/surya -c "Surya Kumar" -s /bin/bash surya   # Add new user
userdel -r username             # Delete user + home dir
userdel username                # Delete user only (keep home dir)
```

---

## 4. Sudo Privileges

```bash
visudo                          # Edit sudoers file safely
surya ALL=(ALL) NOPASSWD:ALL                                   # All commands allowed
surya ALL=(ALL) NOPASSWD:ALL, !/usr/sbin/useradd               # All except 'useradd'
surya ALL=(ALL) NOPASSWD:/usr/sbin/useradd, /usr/bin/passwd    # Only specific commands
```

---

## 5. User & Password Files

```bash
/etc/passwd                     # User accounts
/etc/shadow                     # Encrypted passwords
tail -1 /etc/passwd              # Show last user entry
cat /etc/default/useradd         # Default settings
grep ^PASS /etc/login.defs       # Password policy defaults
```

---

## 6. Password Management

```bash
passwd username                  # Change password for user
passwd                           # Change own password
su - root                        # Root login
passwd surya                     # Root changes user password

# Encrypted password generation
dnf install -y openssl
openssl passwd redhat
openssl passwd -salt 42 redhat

# Add user with encrypted password
useradd -m -p $(openssl passwd redhat) Mohamed
```

---

## 7. Account Expiry & Aging

```bash
chage -l Mohamed                 # Show password aging info
chage Mohamed                    # Modify aging interactively
```

---

## 8. Lock / Unlock Accounts

```bash
usermod -L Nehra                 # Lock account
cat /etc/shadow | grep Nehra
usermod -U Nehra                 # Unlock account
cat /etc/shadow | grep Nehra
passwd -l Nehra                  # Lock password
passwd -u Nehra                  # Unlock password
```

---

## 9. Modify User

```bash
usermod -s /bin/zsh surya        # Change default shell
chsh -l                          # List available shells
dnf install zsh -y               # Install zsh
```

---

## 10. Manual Home Directory

```bash
mkdir /home/laura                # Create home dir
useradd laura                    # Add user
chown laura:laura /home/laura    # Set ownership
chmod 700 /home/laura            # Restrict access
```

---

## 11. Skeleton Directory

```bash
ls -la /etc/skel                 # Default files for new users
```

---

## 12. Safe Editing

```bash
vipw                             # Safely edit /etc/passwd
vigr                             # Safely edit /etc/group
/bin/nologin                     # Prevent user login
```

---

# 👥 Group Management

### 1. Primary vs Secondary Groups

* **Primary group**: default group of a user (only one).
* **Secondary groups**: additional groups (can be many).

### 2. Check Group Info

```bash
cat /etc/passwd              # Users & their primary group IDs
cat /etc/group               # List of groups & members
groups                       # Groups of current user
groups linux                 # Groups of user 'linux'
id vikashmehra               # UID, GID, and all groups of 'vikashmehra'
```

### 3. Create a New Group

```bash
groupadd Unix                # Create group 'Unix'
groupadd -g 2020 Admin       # Create group 'Admin' with custom GID 2020
```

### 4. Add a User to a Group

```bash
usermod -aG Unix dinga       # Add 'dinga' to secondary group 'Unix'
gpasswd -a vikashmehra Unix  # Add 'vikashmehra' to 'Unix'
gpasswd -d vikashmehra Unix  # Remove 'vikashmehra' from 'Unix'
```

### 5. Change Primary Group of a User

```bash
usermod -g Unix linux        # Set 'Unix' as primary group of 'linux'
grep linux /etc/passwd       # Verify primary group
```

### 6. Rename a Group

```bash
groupmod -n Redhat Unix      # Rename 'Unix' → 'Redhat'
```

### 7. Delete a Group

```bash
groupdel Redhat              # Delete group 'Redhat'
```

### 8. Make Group Admin

```bash
gpasswd -A vikashmehra Unix  # Make 'vikashmehra' admin of 'Unix'
```

### 9. File Group Ownership

```bash
chgrp Unix file.txt          # Change file group
ls -l file.txt               # Verify group ownership
chown :Unix file.txt         # Alternate way
```

### 10. Special Case: SGID on Directory

```bash
chmod g+s /project           # SGID → new files inherit group of dir
ls -ld /project              # Verify 's' in group perms
```

### 11. Group Commands Summary

* `groupadd` → create group
* `groupdel` → delete group
* `groupmod` → rename/modify group
* `usermod` → manage user’s primary/secondary groups
* `gpasswd` → manage group members/admins
* `chgrp` → change file’s group
* `id / groups` → check memberships

---

# 🌐 User Environment Files

## `~/.bash_profile` (Login Shell)

```bash
export PATH=$PATH:$HOME/bin
export PS1="[\u@\h \w]\$ "
export HOSTNAME=$(hostname)
MYVAR=1000; export MYVAR

echo "User logged in to $HOSTNAME"
echo "Welcome $(whoami), a new shell started!"
echo "Custom variable MYVAR = $MYVAR"

if [ -f ~/.bashrc ]; then
   source ~/.bashrc
fi
```

## `~/.bashrc` (Interactive Shell)

```bash
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

HISTSIZE=1000
HISTFILESIZE=2000
export HISTCONTROL=ignoredups

echo "A new interactive shell started for $(whoami)"
```

## `~/.bash_logout` (Logout)

```bash
echo "Goodbye $(whoami), you have logged out of $HOSTNAME."
```

---

✅ This cheat sheet is now in **Markdown format**, perfect for GitHub README or notes.

Do you also want me to add a **table of contents with clickable links** at the top for quick navigation?
