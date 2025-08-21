Perfect 👍 You want me to extend your **Linux training README** with commands for file **viewing, redirection, and paging** (`head`, `tail`, `cat`, `echo`, `tac`, `less`, `more`).

---

# 🟦 Session 7 – File Viewing & Redirection

### `head` – View First Lines of a File

```bash
head -10 raj.txt   # show first 10 lines
head -n 20 file.txt  # show first 20 lines
```

---

### `tail` – View Last Lines of a File

```bash
tail -10 raju.txt    # show last 10 lines
tail -f logfile.txt  # follow file (live updates, useful for logs)
```

---

### `cat` – Create & View Files

```bash
cat > new.txt
# enter text, press CTRL+D to save
```

👉 Overwrites existing file if present.

```bash
cat >> newfile
# append content instead of overwriting
```

---

### `echo` – Write Content to Files

```bash
echo "Welcome to Nehra youtube content." > new.txt
```

* `>` → overwrite file
* `>>` → append to file

---

### `tac` – Display File in Reverse Order

```bash
tac file.txt
```

---

### `less` – View Large Files (scrollable)

```bash
less /var/log/messages
```

* Navigation:

  * `↑/↓` → scroll
  * `/word` → search
  * `q` → quit

---

### `more` – View Large Files (page by page)

```bash
more file.txt
```

* Press `SPACE` → next page
* Press `q` → quit

---

## 📌 Quick Reference

| Command   | Purpose                         | Example                   |
| --------- | ------------------------------- | ------------------------- |
| `head`    | Show first N lines              | `head -10 file.txt`       |
| `tail`    | Show last N lines / live follow | `tail -f logfile.log`     |
| `cat >`   | Create file (overwrite)         | `cat > new.txt`           |
| `cat >>`  | Append to file                  | `cat >> notes.txt`        |
| `echo >`  | Write/overwrite content         | `echo "Hello" > file.txt` |
| `echo >>` | Append content                  | `echo "Log" >> file.txt`  |
| `tac`     | Display file in reverse         | `tac file.txt`            |
| `less`    | View file (scroll & search)     | `less bigfile.log`        |
| `more`    | View file (page by page)        | `more file.txt`           |

---

✅ This keeps the same **professional style** as your earlier sessions (4–6).

Do you want me to **merge Session 7 into your main README** so you have a **single consolidated Linux training file**, or keep it as a **separate session file** (like `Session7.md`)?
