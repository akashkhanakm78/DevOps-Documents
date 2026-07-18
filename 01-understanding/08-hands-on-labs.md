# Chapter 08: DevOps and Linux Hands-On Labs

Follow these lab exercises step-by-step to practice file manipulation, permission management, and automation.

---

## Lab 1: Linux Directory Navigation and Stream Manipulation

**Goal**: Navigate directories, write data to files using redirection, and filter strings using piping.

### Steps
```bash
# 1. Navigate to the temporary directory
cd /tmp

# 2. Create a lab directory
mkdir devops-lab
cd devops-lab

# 3. Write a multi-line file using echo and redirection operators
echo "system_status=OK" > config.cfg
echo "port=8080" >> config.cfg
echo "db_connection=FAILED" >> config.cfg
echo "debug_mode=true" >> config.cfg

# 4. View file contents
cat config.cfg

# 5. Filter for lines containing "FAILED"
grep "FAILED" config.cfg

# 6. Chain commands: list files, filter for "config", and count matching lines
ls | grep "config" | wc -l
```

---

## Lab 2: File Permission Management

**Goal**: Create user-restricted scripts and translate permission weights.

### Steps
```bash
# 1. Create a script file
echo "echo 'Running deploy script...'" > deploy.sh

# 2. Check default file permissions
ls -l deploy.sh
# Expected output similar to: -rw-r--r-- (Owner can read/write, others can only read)

# 3. Try running the script (should fail)
./deploy.sh
# Console error: bash: ./deploy.sh: Permission denied

# 4. Grant execute permissions to the Owner only using Octal notation (744)
chmod 744 deploy.sh

# 5. Run the script again (should execute successfully)
./deploy.sh
```

---

## Lab 3: Automation with Cron Jobs

**Goal**: Schedule a task to execute automatically at regular intervals.

### Steps
Cron is a time-based job scheduler in Linux. You configure jobs inside the `crontab` file.

```bash
# 1. Edit the crontab file for your current user
crontab -e

# 2. Add the following line at the bottom to write system uptime to a log every minute:
# * * * * * uptime >> /tmp/uptime.log
# (The 5 stars represent: Minute, Hour, Day of Month, Month, Day of Week)

# 3. Save and close the editor. You should see:
# crontab: installing new crontab

# 4. Wait 2 minutes and check if the logs are being generated automatically:
cat /tmp/uptime.log
```
* **Uptime Log Output**: Shows system run status and load averages updated every 60 seconds.
