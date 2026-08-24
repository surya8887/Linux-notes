# Linux Shell Basics & Command Notes

## Session Summary
This session provides an introduction to the Linux shell, shell prompts, command formats, and getting help using man pages. It explains that the shell is a command-line interface interpreting user commands to the Linux kernel, not the terminal itself. It covers various types of shells (bash, sh, csh, tcsh, ksh, zsh) and how to switch between them. Furthermore, it details the anatomy of a Linux command (Command + Options + Arguments) and demonstrates practical examples. Finally, a significant portion of the session is dedicated to Linux documentation, showing users how to get detailed help using `man` pages, `info`, `--help`, and commands like `apropos`, `whatis`, `whereis`, and `type`.

---



## Linux Commands Discussed and Their Meanings

**Command [options] [arguments]**

- Command  --> tool you want to run
- options  --> modify behavior (-l , --help, etc)
- arguments --> target (file, directory, etc)

### Shell & Environment
*   **`cat /etc/shells`** : Lists all the available shells installed on your Linux machine.
*   **`sh`** : Switches the current session to the Bourne shell (Original Unix shell).
*   **`echo $SHELL`** : Displays the current shell environment you are using.
*   **`PS1` (Environment Variable)** : Used to customize the shell prompt (e.g., setting it to show username, hostname, etc.).

### File & Directory Operations
*   **`ls`** : Lists the contents of the present working directory.
    *   `ls -l` : Lists contents in a long tabular format, showing details like permissions, owner, and size.
    *   `ls -la` : Lists contents in a long tabular format, including hidden files and directories.
    *   `ls -lh` : Lists contents in a long tabular format, including hidden files and directories.
    *   `ls -lt` : Lists contents in a long tabular format, sorted by time.
    *   `ls -a` : Lists all files and directories, including hidden ones (those starting with a dot).
    *   `ls -ld` : Lists detailed information about a specific directory itself rather than its contents.
*   **`mkdir`** : Creates a new directory.
    *   `mkdir -p` : Creates a directory along with any necessary parent directories (parent-child order).
*   **`touch`** : Creates a new empty file.
*   **`cp`** : Copies files or directories from a source to a destination.
*   **`grep`** : Searches for specific keywords or patterns in text or command outputs (e.g., `ls -l | grep txt`).

### Help & Documentation Commands
*   **`man <command>`** : Opens the manual pages for a specific command (e.g., `man ls`).
    *   *Navigation:* Press `Enter` to move line-by-line, `Space` for page-by-page, `/string` to search for a word, `n` for next occurrence, `p` for previous, and `q` to quit.
    *   *Sections:* `man 1 <command>` for executable programs, `man 5 <file>` for configuration files (e.g., `man 1 passwd` vs `man 5 passwd`).
*   **`man -k <keyword>`** : Searches the short descriptions and manual page names for the keyword.
*   **`mandb`** : Updates the index cache for the manual pages. Useful if `apropos` or `man -k` are not finding newly installed programs.
*   **`info <command>`** : Provides a highly detailed, structured documentation guide, often more comprehensive than man pages.
*   **`apropos <keyword>`** : Lists all commands and manual pages related to a particular keyword.
*   **`whatis <command>`** : Displays a brief, one-line short description of a command from its manual page.
*   **`whereis <command>`** : Shows the location of both the binary executable file and the help manual pages for a command.
*   **`which <command>`** : Shows only the absolute path of the binary executable file for a given command.
*   **`type <command>`** : Checks and displays the type of a command, indicating if it's a shell built-in (e.g., `cd`), an external binary (e.g., `mkdir`), or an alias.
*   **`--help` / `help`** : 
    *   `ls --help` : Displays a quick summary of options for external commands.
    *   `help cd` : Provides documentation for shell built-in commands.