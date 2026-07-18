# Chapter 06: Deployments Hands-on Labs

Follow these lab exercises step-by-step to practice configuring static Virtual Hosts and Load Balancers.

---

## Lab 1: Deploy a Static Website using Nginx Virtual Hosts

**Goal**: Configure an Nginx virtual host to serve a custom website on a local domain address.

### Steps
```bash
# 1. Create a directory structure for website
sudo mkdir -p /var/www/mytestsite
sudo chown -R $USER:$USER /var/www/mytestsite

# 2. Add sample HTML code
echo "<h1>Welcome to My Test Site</h1>" > /var/www/mytestsite/index.html

# 3. Create site configuration file
# Use nano or your editor to create /etc/nginx/sites-available/mytestsite
sudo bash -c 'cat > /etc/nginx/sites-available/mytestsite <<EOF
server {
    listen 80;
    server_name mytestsite.local;

    root /var/www/mytestsite;
    index index.html;

    location / {
        try_files \$uri \$uri/ =404;
    }
}
EOF'

# 4. Enable the site configuration
sudo ln -sf /etc/nginx/sites-available/mytestsite /etc/nginx/sites-enabled/

# 5. Check syntax configurations
sudo nginx -t

# 6. Reload Nginx service
sudo systemctl reload nginx

# 7. Add local domain mapping inside your hosts file
# Edit /etc/hosts (or C:\Windows\System32\drivers\etc\hosts on Windows)
# Add: 127.0.0.1 mytestsite.local

# 8. Test connection
curl -I http://mytestsite.local
```

### Expected Output for Step 8
```text
HTTP/1.1 200 OK
Server: nginx/1.x.x
...
```

---

## Lab 2: Configure a Load Balancer to Distribute Traffic

**Goal**: Configure Nginx as a load balancer to distribute requests between two mockup backend services running on ports `8081` and `8082`.

### Steps

```bash
# 1. Run two simple background web services on ports 8081 and 8082 using python HTTP server
python3 -m http.server 8081 &
python3 -m http.server 8082 &

# 2. Configure Nginx upstream configuration block
# Create file /etc/nginx/sites-available/load-balancer
sudo bash -c 'cat > /etc/nginx/sites-available/load-balancer <<EOF
upstream test_servers {
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

server {
    listen 80;
    server_name lb.test.local;

    location / {
        proxy_pass http://test_servers;
        proxy_set_header Host \$host;
    }
}
EOF'

# 3. Enable site configuration
sudo ln -sf /etc/nginx/sites-available/load-balancer /etc/nginx/sites-enabled/

# 4. Reload Nginx
sudo nginx -t
sudo systemctl reload nginx

# 5. Add local domain mapping inside hosts file:
# Add: 127.0.0.1 lb.test.local

# 6. Test routing traffic (should balance request distributions sequentially)
curl http://lb.test.local
```
