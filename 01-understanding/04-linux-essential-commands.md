# Chapter 04: Essential Linux Commands

This manual outlines fundamental Linux commands, stream redirections, pipes, and text filtering.

---

## 1. Directory Navigation & Operations

| Command | Syntax | Description | Example |
| :--- | :--- | :--- | :--- |
| **`pwd`** | `pwd` | Print Working Directory (shows current path). | `pwd` |
| **`cd`** | `cd [dir]` | Change directory. | `cd /var/log` |
| **`ls`** | `ls [opts] [dir]` | List directory contents. `-l` (long listing), `-a` (show hidden files). | `ls -la` |
| **`mkdir`** | `mkdir [name]` | Create a new directory. `-p` (create parent directories if missing). | `mkdir -p app/src` |
| **`rmdir`** | `rmdir [name]` | Remove empty directory. | `rmdir old` |

---

## 2. File Operations (Create, Copy, Move, Delete)

| Command | Syntax | Description | Example |
| :--- | :--- | :--- | :--- |
| **`touch`** | `touch [file]` | Create an empty file or update access timestamp. | `touch script.sh` |
| **`cp`** | `cp [src] [dest]` | Copy files/directories. `-r` (recursive for folders). | `cp -r src/ dest/` |
| **`mv`** | `mv [src] [dest]` | Move or rename files/directories. | `mv index.html home.html` |
| **`rm`** | `rm [opts] [file]` | Remove files. `-r` (recursive), `-f` (force prompt bypass). | `rm -rf tmp/` |

---

## 3. Viewing & Searching File Contents

* **`cat`**: Prints the entire content of a file to stdout. Not recommended for large files.
  * `cat app.log`
* **`less`**: Opens files page-by-page, allowing scrolling up and down. (Exit by typing `q`).
  * `less /var/log/syslog`
* **`head`**: Prints the first 10 lines of a file by default. Use `-n` to change count.
  * `head -n 20 app.log`
* **`tail`**: Prints the last 10 lines of a file. Use `-f` to follow/stream changes in real-time.
  * `tail -f /var/log/nginx/access.log`
* **`grep`**: Searches for pattern matches inside files. Use `-i` (case-insensitive), `-n` (show line numbers).
  * `grep -i "error" app.log`

---

## 4. Redirection & Pipes

Every Linux command has three input/output streams: `stdin` (input 0), `stdout` (output 1), and `stderr` (error 2).

### 4.1 Redirection Operators
* **`>`**: Redirects stdout to a file, **overwriting** existing content.
  * `echo "Hello" > file.txt`
* **`>>`**: Redirects stdout to a file, **appending** to the bottom.
  * `echo "World" >> file.txt`
* **`2>`**: Redirects stderr to a file.
  * `ls nonexistent_folder 2> error.log`
* **`2>&1`**: Merges stderr and stdout into a single stream.
  * `my_command > output.log 2>&1`

### 4.2 Piping (`|`)
Piping sends the stdout output of one command as the stdin input to another command.
```bash
# Print syslog, filter lines containing "error", and display only the first 5 matches
cat /var/log/syslog | grep "error" | head -n 5
```
