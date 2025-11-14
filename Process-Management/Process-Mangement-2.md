# Process Management Notes (Red Hat Enterprise Linux 10)

## 1. Special Shell Variables

* **`$$`** → Prints the current shell's *process ID* (PID).
* **`$PPID`** → Prints the *parent process ID*.
* **`echo $$ $PPID`** → Prints both shell PID and parent PID.

---

## 2. Process Listing Commands

### **`ps` Commands**

* `ps fx` → Shows process hierarchy in tree-like format.
* `ps -ef` → Shows all system processes in full format.

### **`pgrep`**

* `pgrep bash` → Shows the PIDs of all running **bash** processes.

### **`pstree`**

* `pstree` → Display processes in a tree structure.
* `pstree -p` → Show processes **with PIDs**.
* `pstree -p | grep bash` → Filter tree to show bash-related entries.
* `pstree -u surya` → Show processes owned by user **surya**.
* `pstree -p -s 36736` → Show parent chain for process **36736**.

### **`jobs`**

* Shows background and suspended jobs in the current shell.

---

## 3. Starting, Stopping, and Inspecting Processes

* `sleep 200` → Sleep for 200 seconds.
* `sleep 60 &` → Run sleep in background.
* `ps -C sleep` → Show processes with command name **sleep**.
* `ps fx | grep 36736` → Find specific process via grep.

---

## 4. Killing Processes

### **Signal List**

* `kill -l` → List all signals.

### **Important Signals**

* `kill -15 PID` → **SIGTERM** (safe terminate).
* `kill -9 PID` → **SIGKILL** (force kill, cannot be ignored).
* `kill -19 PID` → **SIGSTOP** (pause process).
* `kill -18 PID` → **SIGCONT** (resume process).

### **Tracing System Calls**

* `strace -p 36946` → Attach strace to running process.

---

## 5. Process State Codes

### **General Linux Process States**

* **D** → Uninterruptible Sleep
* **R** → Running
* **S** → Sleeping
* **T** → Stopped
* **W** → Paging
* **X** → Dead
* **Z** → Zombie (defunct)
* **I** → Idle

### **BSD Format Flags**

* `<` → High‑priority process
* `N` → Low‑priority process
* `L` → Pages are locked into memory
* `s` → Session leader
* `I` → Idle state
* `+` → Foreground process

---

## README Format (For Surya – RHEL 10)

```
# RHEL 10 Process Management - README

This README contains important commands related to:
- Process listing
- Process hierarchy
- Signals and killing processes
- Background jobs
- Process states

Use this document as a quick reference while working on RHEL 10.
```

---

## 6. CPU State Codes (top Command)

* **us** → User CPU time
* **sy** → System CPU time
* **ni** → CPU time with manually set nice value
* **id** → Idle CPU time
* **wa** → Waiting for I/O
* **hi** → Hardware interrupts
* **si** → Software interrupts
* **st** → Stolen time by hypervisor

---

## 7. Priority & Nice Values

### **Linux Priority Range**

* Priority values range from **0 to 139**.
* **0–99** → Real-time priority (root only)
* **100–139** → Normal user processes

### **Nice Value Range**

* Range: **-20 (highest priority)** to **+19 (lowest priority)**
* Only root can set **negative nice values**.

---

## 8. top Command Options

* `top -o %CPU` → Sort by CPU usage.
* `top -H` → Show threads.
* `top -u username` → Show processes for specific user.
* `top -v` → Show version.

---

## 9. Zombie Process Example

```
ll zombie.c
```

```
gcc zombie.c
./a.out &
```

```
ps -C a.out
```

---

## 10. Infinite Script + Priority Example

### **infinite.sh**

```
#!/bin/bash
while true; do
  echo "Running..."
  sleep 1
done
```

Run it:

```
chmod +x infinite.sh
./infinite.sh &
```

### **Start Process With Nice Value**

```
nice -n 10 ./infinite.sh &
```

(**Lower priority**)

```
nice -n -5 ./infinite.sh &   # root only
```

(**Higher priority**)

### **Check Priority**

```
ps -l
```

Look for fields: **NI** (nice) and **PRI** (priority)

---

## 11. Change Priority Using renice

```
renice 15 -p <PID>
```

Increase nice value (lower priority)

```
renice -5 -p <PID>    # root only
```

Decrease nice value (higher priority)

---

End of Notes.
