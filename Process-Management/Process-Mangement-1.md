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

End of Notes.
