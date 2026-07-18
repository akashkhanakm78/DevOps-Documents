# Chapter 07: Linux Networking and Remote Connections

This manual covers diagnostic utilities, remote terminal access via SSH, and network port validation.

---

## 1. Network Diagnostics

DevOps engineers must frequently troubleshoot network connectivity between host servers and container workloads.

| Command | Usage | Description | Example |
| :--- | :--- | :--- | :--- |
| **`ping`** | `ping [host]` | Tests basic ICMP layer-3 connectivity to a remote host. | `ping google.com` |
| **`curl`** | `curl [url]` | Command-line tool to transfer data to or from a server (HTTP/HTTPS/FTP). | `curl -I https://google.com` |
| **`nslookup`** | `nslookup [domain]` | Queries Domain Name System (DNS) details for IP mapping. | `nslookup google.com` |
| **`dig`** | `dig [domain]` | Advanced DNS lookup utility. | `dig google.com` |
| **`ss`** / **`netstat`**| `ss [opts]` | Displays active network connections, routing tables, and listening ports. | `ss -tulpn` |
| **`ip`** | `ip addr` | Displays and configures network interfaces, IP addresses, and routing. | `ip addr show` |

* **`ss -tulpn` breakdown**:
  * `-t` (TCP sockets), `-u` (UDP sockets), `-l` (listening sockets only), `-p` (show process PID owning the socket), `-n` (show numeric port numbers instead of service names).

---

## 2. Remote Connections via SSH

**Secure Shell (SSH)** is a cryptographic network protocol used for secure operating system logins and remote command execution.

### 2.1 Standard Login (Password)
To log in to a remote Linux server:
```bash
ssh username@remote_host_ip
```

### 2.2 Key-Based Authentication (Passwordless & Secure)
Key-based login is standard practice in DevOps to automate system deployments.

```bash
# Step 1: Generate a private/public keypair on your local machine
ssh-keygen -t rsa -b 4096
# (This creates ~/.ssh/id_rsa [private key] and ~/.ssh/id_rsa.pub [public key])

# Step 2: Copy the public key to the remote server
ssh-copy-id username@remote_host_ip

# Step 3: Connect securely without entering password
ssh username@remote_host_ip
```

---

## 3. Basic Port Visualizations

Ports are virtual endpoints that allow different services to share network traffic on a single IP address:

```
                          [ Client Request ]
                                  |
                                  v
                       [ Remote Host Server ]
                         IP: 192.168.1.50
                                  |
            +---------------------+---------------------+
            | Port 22             | Port 80             | Port 443
            v                     v                     v
       [ SSH Daemon ]      [ Nginx HTTP ]       [ Nginx HTTPS ]
```
* **Port 22**: Default SSH communication port.
* **Port 80**: Default unencrypted HTTP web port.
* **Port 443**: Default encrypted HTTPS web port.
