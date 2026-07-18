# Linux Commands Cheat Sheet

Quick command reference sheet for common Linux Administration tasks.

---

## 1. Directory Navigation & Search
```bash
pwd                                       # Print current working directory
cd /var/log                               # Change directory to /var/log
cd ..                                     # Move up one directory level
ls -la                                    # Long listing of all files (including hidden)
find / -name "*.conf" 2>/dev/null        # Search for files ending in .conf from root
```

## 2. File Operations
```bash
touch app.log                             # Create empty file
mkdir -p project/src                      # Create directories recursively
cp source.txt backup.txt                  # Copy file
cp -r src_dir/ dest_dir/                  # Copy directory recursively
mv old.txt new.txt                        # Move/rename file
rm file.txt                               # Delete file
rm -rf folder/                            # Force delete folder and its contents
```

## 3. Redirection & Pipes
```bash
echo "Server Running" > status.log        # Overwrite file with text
echo "Log Entry" >> status.log            # Append text to file
cat status.log | grep "Running"           # Pipe file contents to search filter
uptime > log.txt 2>&1                     # Redirect stdout and stderr to log.txt
```

## 4. Permissions & Owners
```bash
chmod 755 script.sh                       # User (rwx), Group (r-x), Others (r-x)
chmod 644 config.cfg                      # User (rw-), Group (r--), Others (r--)
chmod +x script.sh                        # Make file executable
chown user:group file.txt                 # Change owner and group
chown -R user:group folder/               # Change owner and group recursively
```

## 5. Processes & Services
```bash
ps aux | grep nginx                       # Find PID of running nginx processes
top                                       # Interactive real-time process list
kill -9 1234                              # Force kill process with PID 1234
systemctl start nginx                     # Start service
systemctl stop nginx                      # Stop service
systemctl restart nginx                   # Restart service
systemctl status nginx                    # View status & logs of service
systemctl enable nginx                    # Enable service to start on boot
```

## 6. Networking & Troubleshooting
```bash
ping -c 4 8.8.8.8                         # Ping host 4 times
curl -I https://google.com                # Fetch HTTP response headers
ss -tulpn                                 # List all listening TCP/UDP ports with PIDs
nslookup google.com                       # Get IP address of domain
ip addr show                              # Show all network adapters and IPs
ssh -i key.pem user@192.168.1.50          # Connect using private key
```
