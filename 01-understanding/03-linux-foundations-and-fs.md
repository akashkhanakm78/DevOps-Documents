# Chapter 03: Linux Foundations & Filesystem Hierarchy

This manual introduces Linux foundations, the differences between the Kernel and Shell, and the Filesystem Hierarchy Standard.

---

## 1. What is Linux?

**Linux** is an open-source, Unix-like Operating System kernel created by Linus Torvalds in 1991. The term "Linux" is commonly used to describe complete Linux Distributions (Distros) that bundle the Linux kernel with utility tools, shells, package managers, and software interfaces.

### 1.1 Kernel vs. Shell
* **Kernel**: The core engine of the OS. It interacts directly with the physical hardware (CPU, RAM, Disks, Network Adapters) and manages system resources.
* **Shell**: A user-space command interpreter interface. It reads instructions entered by users and executes them by sending requests to the kernel (e.g., Bash, Zsh, Sh).

```
   [ User Input ] ===> [ Shell (Bash) ] ===> [ Kernel ] ===> [ Hardware ]
```

### 1.2 Common Distributions
* **Debian / Ubuntu**: Popular in development environments, utilizes the `apt` package manager.
* **Red Hat Enterprise Linux (RHEL) / CentOS / Rocky Linux**: Standard in corporate production environments, utilizes the `yum` or `dnf` package managers.

---

## 2. Linux Filesystem Hierarchy Standard (FHS)

Unlike Windows (which utilizes separate drive letters like `C:` and `D:`), Linux represents all physical drives, networks, and logical volumes within a single unified tree starting at the root directory `/`.

```
                                [ / ] (Root)
                                  |
      +-------+------+------+-----+-----+------+------+
      |       |      |      |     |     |      |      |
    /bin   /boot   /etc   /home  /opt  /root  /var   /tmp
```

### Directory Meanings:
* **`/` (Root)**: The starting directory of the entire filesystem tree.
* **`/bin`**: Holds essential command binary executables required for all users (e.g., `ls`, `cd`, `cp`).
* **`/sbin`**: Holds system binary executables reserved for system administrators (e.g., `fdisk`, `reboot`, `iptables`).
* **`/boot`**: Contains files necessary to boot the system (like the kernel, initrd, and GRUB loader configs).
* **`/etc`**: The administrative configuration directory. Contains config text files for all installed services (e.g., `/etc/nginx/nginx.conf`, `/etc/passwd`).
* **`/home`**: User home directories. Every standard user has a private folder here (e.g., `/home/username`).
* **`/root`**: The private home directory of the system superuser (Administrator).
* **`/var`**: Variable files directory. Contains data that is constantly modified as the system runs, including log files (`/var/log`) and database storages.
* **`/tmp`**: Contains temporary files created by applications. Files are often deleted on system reboot.
* **`/opt`**: Holds optional third-party software packages not installed by default distribution package managers.
