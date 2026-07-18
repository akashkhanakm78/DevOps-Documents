# Chapter 07: Server Troubleshooting and Log Management

This manual explains how to locate, read, and analyze web server log paths, and resolve common HTTP status code errors.

---

## 1. Web Server Log Locations

When troubleshooting server issues, logs are your first line of defense.

### 1.1 Nginx Log Paths
* **Access Log**: Records details of all incoming client requests.
  * `/var/log/nginx/access.log`
* **Error Log**: Records issues Nginx encountered (like configuration errors, socket failures, and permission denials).
  * `/var/log/nginx/error.log`

### 1.2 Apache Log Paths
* **Access Log**:
  * `/var/log/apache2/access.log` (on Ubuntu/Debian)
  * `/var/log/httpd/access_log` (on CentOS/RHEL)
* **Error Log**:
  * `/var/log/apache2/error.log` (on Ubuntu/Debian)
  * `/var/log/httpd/error_log` (on CentOS/RHEL)

### 1.3 Helpful Log Reading Commands
```bash
# Monitor logs in real-time as requests come in
tail -f /var/log/nginx/access.log

# View last 50 entries of error log
tail -n 50 /var/log/nginx/error.log

# Filter log entries containing "denied"
grep -i "denied" /var/log/nginx/error.log
```

---

## 2. Resolving Common HTTP Status Code Errors

### 2.1 HTTP 403 Forbidden (Permission Denied)
* **Causes**:
  * The web server process (`www-data` or `nginx`) does not have read permissions on the files inside `/var/www/`, or execute permissions on parent directories.
  * The file requested does not exist and directory indexing (`autoindex`) is turned off.
* **Fix**:
  * Adjust directory permissions: `chmod 755 /var/www/site` and `chmod 644 /var/www/site/index.html`.
  * Ensure the directory owner is corrected: `chown -R www-data:www-data /var/www/site`.

### 2.2 HTTP 404 Not Found
* **Causes**:
  * The requested URL does not match any file in the document root directory.
  * The `root` path specified inside the server block configuration is incorrect.
* **Fix**:
  * Verify the physical path exists: `ls /var/www/site/my-file.html`.
  * Crosscheck the `root` path in Nginx site config.

### 2.3 HTTP 502 Bad Gateway
* **Causes**:
  * Nginx acted as a proxy but could not connect to the backend application (Node.js/Python server). The backend app is crashed, offline, or listening on a different port.
* **Fix**:
  * Check backend app status: `pm2 status` or `systemctl status gunicorn`.
  * Verify what port the backend is listening on: `sudo ss -tulpn | grep node`.
  * Ensure the Nginx `proxy_pass` port matches the backend application port exactly.

### 2.4 HTTP 504 Gateway Timeout
* **Causes**:
  * The backend application (Node.js/Python server) accepted the request, but took too long to respond. The connection was closed by Nginx after a timeout period.
* **Fix**:
  * Optimize slow database queries or long loops in the backend code.
  * Increase proxy timeout values inside the Nginx server block:
    ```nginx
    proxy_read_timeout 300;
    proxy_connect_timeout 300;
    proxy_send_timeout 300;
    ```
* **Explanation**: Extends the Nginx waiting period to 300 seconds before aborting the request.
