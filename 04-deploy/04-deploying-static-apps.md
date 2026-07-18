# Chapter 04: Deploying Static Applications

This manual guides you through deploying static web applications (HTML, CSS, JavaScript, and compiled React/Vue/Angular build outputs) to Nginx and Apache.

---

## 1. Directory Structure Setup

Normally, web server static assets are served from `/var/www/`.

### 1.1 Create directories and permissions
```bash
# 1. Create a directory for your application
sudo mkdir -p /var/www/portfolio

# 2. Assign ownership to your current non-root user to copy files easily
sudo chown -R $USER:$USER /var/www/portfolio

# 3. Create a basic index.html file
echo "<h1>My Portfolio Site</h1>" > /var/www/portfolio/index.html
```

---

## 2. Deploying on Nginx

### Step 1: Create a Server Block Configuration
Create `/etc/nginx/sites-available/portfolio` using `sudo`:

```nginx
server {
    listen 80;
    server_name portfolio.local;

    root /var/www/portfolio;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### Step 2: Enable the configuration
```bash
# Create a symbolic link to sites-enabled
sudo ln -s /etc/nginx/sites-available/portfolio /etc/nginx/sites-enabled/

# Verify configuration syntax
sudo nginx -t

# Reload Nginx service
sudo systemctl reload nginx
```

---

## 3. Deploying on Apache

### Step 1: Create a Virtual Host Configuration
Create `/etc/apache2/sites-available/portfolio.conf` using `sudo`:

```apache
<VirtualHost *:80>
    ServerName portfolio.local
    DocumentRoot /var/www/portfolio

    <Directory /var/www/portfolio>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

### Step 2: Enable the configuration
```bash
# Enable the site configuration
sudo a2ensite portfolio.conf

# Disable the default index site config if needed to prevent conflicts
sudo a2dissite 000-default.conf

# Reload Apache service
sudo systemctl reload apache2
```

---

## 4. Permission Adjustments (The `www-data` user)

Both Nginx and Apache run as a background system user named `www-data` (on Ubuntu/Debian) or `nginx`/`apache` (on RHEL/CentOS).
* The web server process must have **Read** (`r`) permissions on all files to serve them, and **Execute** (`x`) permissions on parent folders to access them.
* If you see permission errors (HTTP 403 Forbidden), adjust permissions:
  ```bash
  # Change group ownership of the files to the web server user
  sudo chown -R $USER:www-data /var/www/portfolio
  
  # Ensure directories are accessible and files are readable
  find /var/www/portfolio -type d -exec chmod 755 {} \;
  find /var/www/portfolio -type f -exec chmod 644 {} \;
  ```
* **Explanation**: Directory permissions `755` allow the web server to enter (`x`) and list (`r`) folders. File permissions `644` allow the web server to read (`r`) and send files to clients.
