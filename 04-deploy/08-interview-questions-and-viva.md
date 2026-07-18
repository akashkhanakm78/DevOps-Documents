# Chapter 08: Web Deployments Interview Prep & Viva Bank

A compilation of the most frequently asked web server administration and app deployment interview questions.

---

## Section 1: Core Web Server Architectures

### Q1: What is the difference between Nginx and Apache?
**Answer**:
* **Architecture**: Nginx uses an asynchronous, non-blocking, event-driven architecture, handling thousands of concurrent connections within a single worker process. Apache traditionally uses a process-based model (creating one process/thread per connection).
* **Performance**: Nginx is significantly faster and uses less memory (RAM) when serving static files or acting as a reverse proxy under heavy traffic. Apache is highly extensible and supports configuration overrides via `.htaccess` files.
* **Usage**: Nginx is commonly deployed as a reverse proxy and load balancer in front of application servers, while Apache is often used for serving legacy LAMP stack applications.

### Q2: What is a reverse proxy?
**Answer**: A reverse proxy is a server that sits in front of backend web application servers. It intercepts incoming client requests, forwards them to the appropriate backend application, and returns the response to the client. This hides backend IP addresses, manages SSL decryption, and provides load balancing.

---

## Section 2: Configurations and Deployments

### Q3: What is the difference between `try_files` in Nginx and `.htaccess` in Apache?
**Answer**:
* `try_files` is an Nginx directive that checks for the existence of files or directories in the specified order, falling back to a default file or status code if none are found. It runs inside the compiled server configuration blocks, ensuring high performance.
* `.htaccess` is a directory-level configuration file processed by Apache at runtime. It allows developers to override global settings (like URL rewrites) without modifying global configuration files. Because Apache must scan folders recursively for `.htaccess` files on every request, it degrades performance slightly.

### Q4: Explain the difference between `reload` and `restart` on Nginx/Apache services.
**Answer**:
* **`reload`** (e.g., `systemctl reload nginx`) performs a graceful reload of configuration files. It instructs the master process to parse the new configuration, start new worker processes, and slowly shut down old worker processes once they finish handling active connections. There is **zero downtime**.
* **`restart`** (e.g., `systemctl restart nginx`) terminates the entire web server process, killing all active connections, and starts a fresh process. This causes a brief period of **downtime**.

---

## Section 3: Diagnostic Scenarios

### Q5: How do you troubleshoot a "502 Bad Gateway" error on Nginx?
**Answer**:
1. Check the Nginx error logs: `tail -n 20 /var/log/nginx/error.log`.
2. Verify if the backend application (Node.js/Python server) is running: `pm2 status` or `systemctl status gunicorn`.
3. Check what port/socket the backend is listening on: `sudo ss -tulpn`.
4. Ensure the Nginx `proxy_pass` port matches the backend's active listening port.

### Q6: How do you resolve a "403 Forbidden" error on an Nginx static site?
**Answer**:
1. Verify directory and file permissions: Ensure the web server user (`www-data`) has read (`r`) permission on the files and execute (`x`) permission on all parent directories: `chmod 755 /var/www/site` and `chmod 644 /var/www/site/index.html`.
2. Check if the index file name matches the Nginx `index` directive.
3. Ensure the web server user has ownership: `sudo chown -R www-data:www-data /var/www/site`.
