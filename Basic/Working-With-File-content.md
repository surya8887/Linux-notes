# 📂 Working With File Content in Linux

This guide covers essential commands for viewing, creating, and manipulating file content in Linux, including file viewing, redirection, and paging.

## 📖 Viewing File Content

### `head` – View the Beginning of a File
Displays the first few lines of a file (default is 10 lines).

```bash
head file.txt        # Show the first 10 lines
head -n 20 file.txt  # Show the first 20 lines
```

### `tail` – View the End of a File
Displays the last few lines of a file. Essential for monitoring logs.

```bash
tail file.txt        # Show the last 10 lines
tail -n 20 file.txt  # Show the last 20 lines
tail -f app.log      # Follow file (live updates, useful for logs)
```

### `tac` – View File in Reverse
Displays the contents of a file starting from the last line to the first.

```bash
tac file.txt
```

---

## 📝 Creating and Modifying Files

### `cat` – Concatenate and Display
Used to view small files, create new ones, or append content.

```bash
cat file.txt         # View the entire content of a file
cat > new.txt        # Create a new file (overwrite if exists). Press CTRL+D to save.
cat >> existing.txt  # Append content to an existing file
```
> [!WARNING]
> Using `>` will overwrite the existing file entirely. Use `>>` to append safely.

### `echo` – Write Output to Files
Prints text to the terminal, often redirected to a file.

```bash
echo "System initialized." > status.txt   # Create/Overwrite file with text
echo "Error: Disk full." >> status.txt    # Append text to the file
```

---

## 📜 Paging Large Files

### `less` – Advanced Pager
Views large files interactively without loading the entire file into memory.

```bash
less /var/log/syslog
```
**Navigation Shortcuts:**
* `↑` / `↓` : Scroll up and down
* `SPACE` : Next page
* `/pattern` : Search forward for a pattern
* `q` : Quit

### `more` – Basic Pager
Views files page by page (older and less feature-rich than `less`).

```bash
more /var/log/syslog
```
* `SPACE` : Next page
* `ENTER` : Next line
* `q` : Quit

---

## 📌 Quick Reference Table

| Command | Primary Use | Example |
| :--- | :--- | :--- |
| `head` | Show first N lines | `head -n 10 file.txt` |
| `tail` | Show last N lines / live follow | `tail -f logfile.log` |
| `cat` | View/Create/Append files | `cat > new.txt` |
| `echo` | Write string to standard output | `echo "Hello" > file.txt` |
| `tac` | Display file in reverse order | `tac file.txt` |
| `less` | View large files (scroll & search) | `less bigfile.log` |
| `more` | View large files (page by page) | `more file.txt` |
