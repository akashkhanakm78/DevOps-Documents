# Chapter 03: Apache Fundamentals

This manual introduces the Apache HTTP Server, its process-based architecture, virtual hosts configuration, and configuration overrides via `.htaccess`.

---

## 1. What is Apache?

The **Apache HTTP Server** is a classic, open-source, process-based web server. 
* **Multi-Processing Modules (MPM)**: Apache utilizes modules to handle requests. The traditional `prefork` MPM creates a dedicated process/thread per connection. While highly stable and compatible, it consumes significantly more RAM under high traffic compared to Nginx's event loop.
* **Extensibility**: Apache supports dynamic modules loaded at runtime (e.g., `mod_rewrite`, `mod_ssl`, `mod_headers`).

---

## 2. Installing Apache (Ubuntu / Debian)

On Debian/Ubuntu systems, the Apache service package is named `apache2`:

```bash
# Update local packages database
sudo apt update

# Install Apache2
sudo apt install -y apache2

# Verify installation
apache2 -v
# Expected: Server version: Apache/2.x.x
```

### Managing the Apache Service
```bash
sudo systemctl start apache2      # Start service
sudo systemctl stop apache2       # Stop service
sudo systemctl restart apache2    # Restart service
sudo systemctl reload apache2     # Gracefully reload configuration changes
systemctl status apache2          # Check status
```

---

## 3. Apache Directory Layout & Virtual Hosts

* Main config file: `/etc/apache2/apache2.conf`
* Virtual hosts directory: `/etc/apache2/sites-available/`
* Activated virtual hosts directory: `/etc/apache2/sites-enabled/`

### 3.1 Virtual Host Configuration Template
Create a configuration file named `/etc/apache2/sites-available/myapp.conf`:

```apache
<VirtualHost *:80>
    ServerName myapp.com
    ServerAlias www.myapp.com
    ServerAdmin admin@myapp.com

    DocumentRoot /var/www/myapp

    # Configure directory permissions and overrides
    <Directory /var/www/myapp>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    # Custom log paths
    ErrorLog ${APACHE_LOG_DIR}/myapp_error.log
    CustomLog ${APACHE_LOG_DIR}/myapp_access.log combined
</VirtualHost>
```

### 3.2 Activating the Virtual Host
Apache uses helper scripts (`a2ensite` and `a2dissite`) to manage configurations:
```bash
# 1. Enable the new virtual host (creates symlink to sites-enabled)
sudo a2ensite myapp.conf

# 2. Enable rewrite module (commonly required for routing)
sudo a2enmod rewrite

# 3. Reload Apache service to apply changes
sudo systemctl reload apache2
```

---

## 4. Configuration Overrides (`.htaccess`)

An **`.htaccess`** (hypertext access) file is a directory-level configuration file supported by Apache. 
* It allows users to override global configurations for specific subdirectories without modifying the main Apache configurations.
* **Common Use**: URL rewrites (redirecting HTTP to HTTPS, routing all requests to a PHP `index.php` front controller in frameworks like WordPress or Laravel).
* **Limitation**: Can degrade performance slightly because Apache must scan folders recursively for `.htaccess` files on every web request. To use it, `AllowOverride All` must be set in the main directory configuration.
