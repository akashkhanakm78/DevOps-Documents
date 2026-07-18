# Chapter 06: Processes and System Services

This manual explains how to monitor system processes, manage background tasks, and run services using Systemd.

---

## 1. Process Monitoring and Management

Every running program on a Linux system is tracked as a **Process** with a unique Process ID (**PID**).

### 1.1 Command Reference

* **`ps`**: Snapshot of current active processes.
  * `ps aux` - Lists every running process on the system with CPU/Memory usage and owners.
* **`top`**: Displays real-time interactive process usage (CPU, RAM, load averages).
* **`htop`**: Modern, colored, interactive process viewer (highly recommended, requires installation).
* **`kill`**: Sends signals to processes to stop or terminate.
  * `kill -15 <PID>`: Sends `SIGTERM` (graceful stop request).
  * `kill -9 <PID>`: Sends `SIGKILL` (forces immediate process termination).
* **`killall`**: Kills all processes matching a specific name.
  * `killall nginx`

---

## 2. Background and Foreground Jobs

When running scripts, you can choose where they run:

* **Foreground (Default)**: The script blocks terminal input until it completes.
* **Append `&`**: Starts the command immediately in the background, keeping the terminal responsive.
  * `python script.py &`
* **`jobs`**: Lists active jobs started from the current shell session.
* **`fg %[job_id]`**: Brings a background job back to the foreground.
* **`bg %[job_id]`**: Resumes a paused foreground process in the background.
* **`Ctrl + Z`**: Pauses the current foreground process, leaving it ready for `bg` or `fg`.

---

## 3. Managing Services with Systemd (`systemctl`)

Most Linux distributions use **Systemd** as the init system (PID 1) to manage system startup, configuration, and services. Services are defined in files called unit files (ending in `.service`).

To manage system services (like Nginx, Docker, or SSH), use `systemctl`:

```bash
# 1. Start a service immediately
sudo systemctl start docker

# 2. Stop a service
sudo systemctl stop docker

# 3. Restart a service (stop and start)
sudo systemctl restart docker

# 4. Check active status and console logs of a service
systemctl status docker

# 5. Enable service to start automatically during system boot
sudo systemctl enable docker

# 6. Disable auto-start on boot
sudo systemctl disable docker

# 7. Reload configuration files without restarting (if supported)
sudo systemctl reload nginx
```
