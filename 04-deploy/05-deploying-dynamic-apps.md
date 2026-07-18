# Chapter 05: Deploying Dynamic Applications

This manual guides you through deploying dynamic backend applications (Node.js and Python) on Virtual Machines behind an Nginx reverse proxy.

---

## 1. Deploying a Node.js Application with PM2

When running node applications in production, starting them directly with `node app.js` in a shell is unsafe because:
* If the server crashes or restarts, the process dies.
* If the application encounters an unhandled exception, the process crashes.
* We use **PM2** (Process Manager 2) to manage node applications in production, keeping them running continuously in the background and auto-restarting them on crashes.

### Step 1: Install Node.js and PM2
```bash
# Install Node.js runtime environment (via NodeSource repo or system apt)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install PM2 globally using npm
sudo npm install -g pm2
```

### Step 2: Configure and Start Node application
Navigate to your application directory `/var/www/node-app`:
```bash
# Install dependencies
npm install

# Start the application with PM2, giving it a name
pm2 start app.js --name "node-api"

# Configure PM2 to start automatically during server reboot
pm2 startup systemd
# (Run the environment script command printed on your console to complete setup)

# Save the current process list configuration
pm2 save
```

### Step 3: Nginx Proxy Configuration
Update your server configuration block to proxy traffic to the PM2 port (default `3000`):
```nginx
server {
    listen 80;
    server_name api.myapp.local;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## 2. Deploying a Python Web App with Gunicorn

Python web applications (built using Flask, Django, or FastAPI) cannot be run directly by standard web servers. They require a **WSGI** (Web Server Gateway Interface) server to run in production. We use **Gunicorn** to run the app, and proxy traffic to it using Nginx.

```
  [ Client Browser ] ===> [ Nginx (Reverse Proxy) ] ===> [ Gunicorn (WSGI Server) ] ===> [ Python Flask/Django App ]
```

### Step 1: Prepare Virtual Environment and Gunicorn
Navigate to your Python directory `/var/www/py-app`:
```bash
# Install python-pip and virtual environment tools
sudo apt install -y python3-pip python3-venv

# Create a virtual environment
python3 -m venv venv

# Activate virtual environment
source venv/bin/activate

# Install application dependencies and Gunicorn
pip install -r requirements.txt
pip install gunicorn
```

### Step 2: Start Gunicorn
Start the Gunicorn process, binding it to a local port or UNIX socket:
```bash
# Start Gunicorn, specifying entry app instance 'wsgi:app'
gunicorn --workers 3 --bind 127.0.0.1:8000 wsgi:app &
```
* **Best Practice**: Create a systemd service file `/etc/systemd/system/gunicorn.service` to manage the Gunicorn process automatically.

### Step 3: Nginx Proxy Configuration
Update your server configuration block to proxy traffic to Gunicorn (port `8000`):
```nginx
server {
    listen 80;
    server_name pyapp.local;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
* **Verification**: Reload Nginx (`sudo systemctl reload nginx`) to complete setup.
