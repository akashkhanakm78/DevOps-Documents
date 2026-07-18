# Chapter 01: Nginx Fundamentals

This manual introduces Nginx, its event-driven architecture, installation guidelines, and core configuration syntax.

---

## 1. What is Nginx?

**Nginx** (pronounced "engine-x") is a high-performance HTTP web server, reverse proxy, and mail proxy. 
* **Event-Driven Architecture**: Unlike traditional web servers (like Apache) that create a new process or thread for every incoming connection, Nginx utilizes an asynchronous, non-blocking, event-driven architecture.
* A single Nginx **worker process** can handle thousands of concurrent connections simultaneously with minimal CPU and memory overhead. This makes it highly efficient for serving static files and acting as an entry point (ingress) for high-traffic networks.

```
                    [ CLIENTS / BROWSERS ]
                   (Thousands of Connections)
                               ||
                               \/
                  +--------------------------+
                  |    Nginx Master Process  |
                  +------------+-------------+
                               |
                        +------+------+
                        |             |
                        v             v
                  [ Worker 1 ]   [ Worker 2 ]  <-- Event loop (Handles many clients)
```

---

## 2. Installing Nginx (Ubuntu / Debian)

```bash
# Update local packages database
sudo apt update

# Install Nginx
sudo apt install -y nginx

# Verify installation
nginx -v
# Expected: nginx version: nginx/1.x.x
```

### Managing the Nginx Service
```bash
sudo systemctl start nginx      # Start server
sudo systemctl stop nginx       # Stop server
sudo systemctl restart nginx    # Restart server (kills connections, restarts process)
sudo systemctl reload nginx     # Reload configuration changes (graceful, no downtime)
systemctl status nginx          # Check status
```

---

## 3. Nginx Configuration Architecture

The main Nginx configuration file is located at `/etc/nginx/nginx.conf`. Configurations are structured as a hierarchy of blocks (contexts) containing key-value directives.

```
  [ Core Context (Global Settings) ]
     |
     +---> [ HTTP Context (Web configuration) ]
              |
              +---> [ Server Context (Virtual Host definition) ]
                       |
                       +---> [ Location Context (Route-specific rules) ]
```

### 3.1 Basic Server Block Configuration Template
Server blocks (virtual hosts) are typically written in separate files inside `/etc/nginx/sites-available/` and symlinked to `/etc/nginx/sites-enabled/` to activate them.

```nginx
# Define virtual host configuration
server {
    listen 80;                         # Port to listen on (HTTP)
    server_name myapp.local www.myapp.com; # Domains matching this block

    root /var/www/myapp;               # Root folder for website static files
    index index.html index.htm;        # Default entry file

    # Location block: defines how to handle specific request URLs
    location / {
        try_files $uri $uri/ =404;      # Serve file or directory, fallback to 404
    }

    # Location block: route matching custom API path
    location /images/ {
        alias /var/www/media/images/;  # Map path to a different directory
    }
}
```
* **`try_files $uri $uri/ =404`**: Checks for the existence of a file matching the requested URI (`$uri`). If it doesn't exist, it checks for a directory (`$uri/`). If neither exists, it returns a 404 error.
* **`reload`**: After modifying Nginx files, always validate syntax before reloading:
  ```bash
  sudo nginx -t
  # Expected: nginx: configuration file /etc/nginx/nginx.conf syntax is ok
  sudo systemctl reload nginx
  ```
