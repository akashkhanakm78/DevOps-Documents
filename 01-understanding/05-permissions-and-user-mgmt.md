# Chapter 05: Linux Permissions and User Management

This manual details how Linux manages users, groups, and file permissions in both symbolic and octal formats.

---

## 1. User and Group Architecture

Linux is a multi-user OS. Access is regulated by permissions associated with users and groups.
* **Owner (User)**: The user account that created the file or directory.
* **Group**: A collection of users grouped together to share access policies.
* **Others**: Any user on the system who is not the file owner and is not in the group.

### User Configuration Files
* `/etc/passwd`: Stores user account details.
* `/etc/group`: Stores group memberships.
* `/etc/shadow`: Stores encrypted passwords (readable only by root).

---

## 2. File Permission Representation

When you run `ls -l`, files are listed with an initial 10-character metadata string:

```
  -  r w x  r - x  r - -
  ^  \___/  \___/  \___/
  |    |      |      |
Type Owner  Group  Others
```
* **First character (Type)**: `-` represents a regular file, `d` represents a directory, and `l` represents a symbolic link.
* **r (Read)**: Allow viewing file content, or listing directory files.
* **w (Write)**: Allow modifying file content, or creating/deleting files in a directory.
* **x (Execute)**: Allow running the file as a program, or entering the directory (`cd`).

---

## 3. Octal vs. Symbolic Permission Formats

Permissions are modified using the `chmod` command. You can define permission changes in two ways:

### 3.1 Octal Representation (Numbers)
Each permission mode is assigned a binary numeric weight:
* **`r` (Read)** = 4
* **`w` (Write)** = 2
* **`x` (Execute)** = 1
* **No permission (`-`)** = 0

To calculate permission settings, add the weights together for Owner, Group, and Others:

| Octal Value | Symbol | Permission Meaning |
| :--- | :--- | :--- |
| **`7`** | `rwx` | Read + Write + Execute (Full control). |
| **`6`** | `rw-` | Read + Write (Standard file edit). |
| **`5`** | `r-x` | Read + Execute (Read & run/enter). |
| **`4`** | `r--` | Read only. |
| **`0`** | `---` | No permissions. |

#### Command Example
```bash
# Assign Read-Write to Owner (6), Read to Group (4), and Read to Others (4)
chmod 644 script.sh
```

### 3.2 Symbolic Representation (Letters)
Use category identifiers (`u` for User, `g` for Group, `o` for Others, `a` for All) and operators (`+` to add, `-` to remove, `=` to set exactly).

```bash
# Add execute permissions to the Owner
chmod u+x script.sh

# Remove write permissions from Group and Others
chmod go-w script.sh
```

---

## 4. Ownership Modification (`chown`)

To change who owns a file or group, use `chown`:
```bash
# Change owner to 'john'
sudo chown john script.sh

# Change owner to 'john' and group to 'devs' recursively on a directory
sudo chown -R john:devs /var/www/html
```

---

## 5. User Management Commands

```bash
# 1. Create a new user
sudo useradd -m -s /bin/bash newuser
# (-m creates home directory, -s sets login shell)

# 2. Set user password
sudo passwd newuser

# 3. Add user to secondary group (e.g. sudo privileges)
sudo usermod -aG sudo newuser

# 4. Delete user (and home directory)
sudo userdel -r newuser
```
