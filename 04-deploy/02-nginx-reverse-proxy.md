# Chapter 02: Nginx as a Reverse Proxy & Load Balancer

This manual explains how to configure Nginx to route external traffic to backend applications and distribute load across multiple servers.

---

## 1. What is a Reverse Proxy?

* **Forward Proxy**: Sits in front of client machines (browsers) to control, filter, or hide outgoing internet requests (e.g., corporate office internet filters).
* **Reverse Proxy**: Sits in front of backend web application servers to receive incoming requests from the internet, routing them to the appropriate application server. Clients do not communicate with the backend application directly; they only see the reverse proxy.

```
  [ Client Browser ] ===> [ Nginx Reverse Proxy (Port 80) ] ===> [ Node.js/Python App (Port 3000) ]
```

### Benefits of a Reverse Proxy:
* **Security**: Hides backend system IPs and ports from direct public access.
* **SSL Termination**: Decrypts HTTPS requests at Nginx, forwarding unencrypted HTTP requests to backend apps inside the local network (reduces CPU load on apps).
* **Caching**: Caches static assets to reduce backend database requests.
* **Load Balancing**: Distributes incoming traffic across multiple instances of a backend service.

---

## 2. Reverse Proxy Configuration

To forward web traffic to a backend app running on `localhost:3000`:

```nginx
server {
    listen 80;
    server_name myapp.com;

    location / {
        proxy_pass http://127.0.0.1:3000;      # Forward traffic to local port 3000
        
        # Forward original client headers to the backend application
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

## 3. Configuring Nginx Load Balancing

You can define a pool of backend servers using the `upstream` directive inside the `http` context of `/etc/nginx/nginx.conf` (or inside `/etc/nginx/conf.d/` custom configurations):

```nginx
# 1. Define the pool of backend application instances
upstream my_backend_pool {
    # Method 1: Round Robin (default) - cycles requests sequentially
    server 192.168.1.10:3000;
    server 192.168.1.11:3000;
    server 192.168.1.12:3000 backup;          # Runs only if others are down
}

server {
    listen 80;
    server_name myapp.com;

    location / {
        proxy_pass http://my_backend_pool;     # Reference the upstream pool
        proxy_set_header Host $host;
    }
}
```

### 3.1 Load Balancing Algorithms
1. **Round Robin (Default)**: Distributes requests sequentially down the list of servers.
2. **Least Connections (`least_conn`)**: Routes requests to the server with the lowest number of active client connections. Useful for long-running computation requests.
   ```nginx
   upstream my_backend_pool {
       least_conn;
       server 192.168.1.10:3000;
       server 192.168.1.11:3000;
   }
   ```
3. **IP Hash (`ip_hash`)**: Uses the client's IP address to compute a hash. The client will always be routed to the same backend server (Sticky Sessions). Useful for stateful applications storing local user sessions.
   ```nginx
   upstream my_backend_pool {
       ip_hash;
       server 192.168.1.10:3000;
       server 192.168.1.11:3000;
   }
   ```
