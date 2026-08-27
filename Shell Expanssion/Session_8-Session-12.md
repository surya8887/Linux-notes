# 🐚 Shell Expansion and Basics (Sessions 8 - 12)

This document covers essential shell concepts, including text output, variable expansions, command types, aliases, and shell debugging.

## 🖨️ Echo and Quotes

### Handling Spaces and Output
The `echo` command prints text to the terminal. Quotes affect how whitespace is handled.

```bash
# Without quotes: Multiple spaces are collapsed into a single space
echo hello Nehra classes             Family
# Output: hello Nehra classes Family

# With double quotes: Whitespace is preserved exactly
echo "Hello Nehra class     family"
# Output: Hello Nehra class     family
```

### Escape Characters
Use `echo -e` to enable interpretation of backslash escapes like newlines (`\n`) and tabs (`\t`).

```bash
echo -e "A line with \n second line \t third word with tab"
```

---

## 📦 Variables and Expansion

Variables store data for use in the shell environment.

> [!WARNING]  
> When assigning variables in Bash, there **must not be spaces** around the equals sign `=`. (e.g., `var1=100` is correct; `var1 = 100` is an error).

```bash
# Viewing environment variables
echo $SHELL

# Variable Assignment
var1=100

# Variable Expansion (Double vs. Single Quotes)
echo $var1      # Expands variable -> 100
echo "$var1"    # Double quotes: Expands variable -> 100
echo '$var1'    # Single quotes: Literal string -> $var1
```

> [!TIP]
> **Single Quotes (`'`)**: Prevent all variable expansion. What you see is exactly what you get.  
> **Double Quotes (`"`)**: Allow variable expansion but protect spaces and other special characters.

---

## 🔍 Command Types: Built-ins vs. Externals

Shell commands can either be built directly into the shell itself (like `cd` or `alias`) or exist as external executable files on the file system (like `ls` or `cal`).

```bash
# Check if a command is a shell built-in or external
type cd

# Find the exact path of an external command executable
which ls
```

---

## 🔗 Aliases

Aliases allow you to create custom shortcuts or alternate names for commands.

```bash
# Creating a dummy file for demonstration
cal > cal.txt
cat cal.txt

# Create an alias (e.g., make 'dog' act exactly like 'cat')
alias dog=cat
dog cal.txt

# View all currently defined aliases
alias

# Remove an alias
unalias dog

# Cleanup dummy file
rm cal.txt
```

> [!NOTE]
> A common default alias in many Linux distributions is `ll`, which typically maps to `ls -l` or `ls -al` (e.g., `alias ll='ls -alF'`).

---

## 🐛 Shell Debugging

You can enable debugging mode directly in the terminal (or in scripts) to trace the execution of commands.

```bash
set -x  # Enable debug mode (prints commands and their arguments as they are executed)
date
set +x  # Disable debug mode (stops printing commands)
```

---

## 💻 Miscellaneous Commands

Basic commands to check system and time information:

```bash
date       # Show current date and time
hostname   # Show the system's hostname
cal        # Show a calendar of the current month
```
