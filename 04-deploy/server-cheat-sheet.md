# Server Management Cheat Sheet

Quick command reference sheet and configuration templates for Nginx and Apache.

---

## 1. Nginx Service Management
```bash
sudo systemctl start nginx                # Start Nginx
sudo systemctl stop nginx                 # Stop Nginx
sudo systemctl restart nginx              # Restart Nginx (Downtime)
sudo systemctl reload nginx               # Gracefully reload configurations (No downtime)
sudo systemctl enable nginx               # Enable Nginx to start on system boot
sudo nginx -t                             # Test configuration syntax validity
```

## 2. Nginx Reverse Proxy Template
Add inside `server {}` block:
```nginx
location / {
    proxy_pass http://127.0.0.1:3000;      # Backend application port
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

## 3. Nginx Upstream Load Balancer Template
Add inside `http {}` context:
```nginx
upstream my_backend {
    least_conn;                            # Load balancing algorithm
    server 192.168.1.10:3000;
    server 192.168.1.11:3000;
}
```

## 4. Apache Service Management
```bash
sudo systemctl start apache2              # Start Apache (Debian/Ubuntu)
sudo systemctl restart httpd             # Restart Apache (CentOS/RHEL)
sudo systemctl reload apache2             # Gracefully reload configurations
sudo a2ensite myapp.conf                  # Enable Virtual Host site config
sudo a2dissite 000-default.conf           # Disable Default site config
sudo a2enmod rewrite                      # Enable mod_rewrite module
```

## 5. Apache Virtual Host Template
```apache
<VirtualHost *:80>
    ServerName myapp.com
    DocumentRoot /var/www/myapp

    <Directory /var/www/myapp>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

## 6. Logs & Debugging Commands
```bash
tail -f /var/log/nginx/error.log          # Stream Nginx error logs in real-time
tail -f /var/log/apache2/error.log        # Stream Apache error logs in real-time
sudo ss -tulpn | grep -E "nginx|apache"   # View which ports Nginx or Apache are bound to
```
