# Practical DevOps Tasks Using Linux

## 1. Introduction

DevOps engineers use Linux regularly to manage servers, deploy applications, troubleshoot problems, monitor system resources, and automate repetitive tasks.

Learning Linux commands is important, but knowing how to apply them to real-world situations is equally important.

In this section, we will practice common Linux tasks performed by DevOps engineers.

### What You Will Learn

- Check system information and resource usage.
- Create and manage users and groups.
- Manage file permissions and ownership.
- Monitor processes and troubleshoot high CPU usage.
- Check disk space and memory.

**Display complete kernel and system information:**


```bash
uname -a
```


**Check the hostname:**


```bash
hostname
```


**Check system uptime:**

```bash

uptime
```


**Check CPU information:**

```bash

lscpu
```


**Check memory usage:**


```bash
free -h

```

### DevOps Scenario


You receive access to a newly provisioned Linux server. Before installing an application, you need to identify its operating system, kernel, CPU, and available memory.


These commands help you understand the server's configuration.

---

## 3. Task 2: Create and Manage Users

### Objective

Create users and groups and manage account information.

**Check the current user:**

```bash
whoami
```

**Display logged-in users:**

```bash
who
```

**Create a user:**

```bash
sudo useradd -m devopsuser
```

The `-m` option creates a home directory for the user.

**Set a password:**

```bash
sudo passwd devopsuser
```

**Create a group:**

```bash
sudo groupadd developers
```

**Add a user to a group:**

```bash
sudo usermod -aG developers devopsuser
```

**Check user information:**

```bash
id devopsuser
```

**Display group information:**

```bash
getent group developers
```

### DevOps Scenario

A new team member joins the DevOps team and requires access to a Linux server.

You create an account, assign a password, and add the user to the appropriate group.

**Important:** Use the principle of least privilege. Give users only the permissions required for their work.

---

## 4. Task 3: Manage File Permissions

### Objective

Control who can read, write, and execute files.

**Create a file:**

```bash
touch deploy.sh
```

**Check file permissions:**

```bash
ls -l deploy.sh
```

**Give the owner execute permission:**

```bash
chmod u+x deploy.sh
```

**Set permissions using numeric notation:**

```bash
chmod 755 deploy.sh
```

Permission values:

| Number | Permission |
|---|---|
| 4 | Read |
| 2 | Write |
| 1 | Execute |
| 7 | Read, write, execute |

For `755`:

- Owner: read, write, execute.
- Group: read and execute.
- Others: read and execute.

**Change file ownership:**

```bash
sudo chown devopsuser deploy.sh
```

**Change group ownership:**

```bash
sudo chgrp developers deploy.sh
```

### DevOps Scenario

A deployment script fails because it does not have execute permission.

Check its permissions and grant the minimum required permission:

```bash
chmod u+x deploy.sh
```

Avoid using `chmod 777` as a routine fix because it gives everyone full permissions.

---

## 5. Task 4: Monitor Running Processes

### Objective

Identify running processes and troubleshoot resource usage.

**Display running processes:**

```bash
ps aux
```

**Search for a particular process:**

```bash
ps aux | grep nginx
```

**Monitor processes in real time:**

```bash
top
```

If installed, you can also use:

```bash
htop
```

**Display process IDs:**

```bash
pgrep nginx
```

**Terminate a process gracefully:**

```bash
kill PID
```

Replace `PID` with the actual process ID.

**Forcefully terminate a process:**

```bash
kill -9 PID
```

Use `kill -9` only when normal termination fails because it does not allow the process to clean up.

### DevOps Scenario

An application is consuming unusually high CPU resources.

1. Run `top`.
2. Identify the process consuming excessive CPU.
3. Find its PID.
4. Investigate the application.
5. Restart or terminate it only when appropriate.

---

## 6. Task 5: Check Disk Space and Memory

### Objective

Monitor storage and memory resources.

**Check filesystem disk usage:**

```bash
df -h
```

**Check directory sizes:**

```bash
du -sh /var/log
```

**Find large directories inside a location:**

```bash
du -h --max-depth=1 /var
```

**Check memory usage:**

```bash
free -h
```

**Check block devices:**

```bash
lsblk
```

### DevOps Scenario

An application stops working because the server is running out of disk space.

Troubleshooting steps:

1. Check disk usage:

```bash
df -h
```

2. Identify large directories:

```bash
sudo du -xhd1 /var 2>/dev/null
```

3. Investigate large log files:

```bash
sudo du -sh /var/log/*
```

4. Review the relevant files and application logs.
5. Apply an approved cleanup or log-rotation procedure.

**Important:** Never delete system files or logs blindly. Identify the cause and follow the server's retention policy.

---

## 7. Task 6: Manage Services Using systemd

### Objective

Check, start, stop, restart, and enable Linux services.

**Check service status:**

```bash
sudo systemctl status nginx
```

**Start a service:**

```bash
sudo systemctl start nginx
```

**Stop a service:**

```bash
sudo systemctl stop nginx
```

**Restart a service:**

```bash
sudo systemctl restart nginx
```

**Reload service configuration:**

```bash
sudo systemctl reload nginx
```

Reload works only if the service supports it.

**Enable a service at boot:**

```bash
sudo systemctl enable nginx
```

**Disable automatic startup:**

```bash
sudo systemctl disable nginx
```

**Check whether a service is active:**

```bash
systemctl is-active nginx
```

### DevOps Scenario

An Nginx web server is not responding after a configuration change.

Troubleshooting steps:

1. Check its status.
2. Validate the Nginx configuration:

```bash
sudo nginx -t
```

3. Review error messages.
4. Reload or restart Nginx if appropriate.
5. Verify its status again.

---

## 8. Task 7: Monitor System and Application Logs

### Objective

Find errors and investigate application failures.

**View system logs:**

```bash
sudo journalctl
```

**View logs from a particular service:**

```bash
sudo journalctl -u nginx
```

**View recent service logs:**

```bash
sudo journalctl -u nginx -n 50
```

**Follow logs in real time:**

```bash
sudo journalctl -u nginx -f
```

**Search for errors in a log file:**

```bash
grep "ERROR" /var/log/app.log
```

**Display the latest 20 log lines:**

```bash
tail -n 20 /var/log/app.log
```

**Monitor a log file continuously:**

```bash
tail -f /var/log/app.log
```

### DevOps Scenario

A deployed application returns an HTTP 500 error.

Troubleshooting steps:

1. Check the application service status.
2. Review recent application logs.
3. Search for `ERROR` and `Exception`.
4. Identify the root cause.
5. Apply the fix and verify the application.

---

## 9. Task 8: Troubleshoot Network Connectivity

### Objective

Check network configuration and connectivity.

**Display IP addresses:**

```bash
ip addr
```

**Display network interfaces:**

```bash
ip link
```

**Display routing information:**

```bash
ip route
```

**Test connectivity:**

```bash
ping -c 4 8.8.8.8
```

**Check DNS resolution:**

```bash
nslookup google.com
```

If `nslookup` is unavailable, use:

```bash
getent hosts google.com
```

**Check listening ports:**

```bash
sudo ss -tulnp
```

**Test connectivity to a particular port:**

```bash
nc -vz example.com 443
```

The `nc` command may need to be installed.

### DevOps Scenario

An application cannot connect to a remote server.

Troubleshooting steps:

1. Check the local IP configuration.
2. Check the routing table.
3. Test network connectivity.
4. Verify DNS resolution.
5. Check whether the destination port is reachable.
6. Inspect firewall rules and application configuration.

---

## 10. Task 9: Create Backups and Archives

### Objective

Create compressed backups of important files and directories.

**Create a directory:**

```bash
mkdir -p ~/devops-backups
```

**Create a sample application directory:**

```bash
mkdir -p ~/app-data
```

**Create a sample file:**

```bash
echo "Application configuration" > ~/app-data/config.txt
```

**Create a compressed archive:**

```bash
tar -czvf ~/devops-backups/app-backup.tar.gz -C ~ app-data
```

Explanation:

| Option | Meaning |
|---|---|
| `c` | Create an archive |
| `z` | Compress using gzip |
| `v` | Display progress |
| `f` | Specify archive filename |

**List archive contents:**

```bash
tar -tzf ~/devops-backups/app-backup.tar.gz
```

**Extract the archive:**

```bash
mkdir -p ~/restore-test
tar -xzvf ~/devops-backups/app-backup.tar.gz -C ~/restore-test
```

### DevOps Scenario

Before changing an application's configuration, create a backup so that the previous files can be restored if the deployment fails.

**Best practice:** Test backups by restoring them. A backup is useful only if it can be recovered.

---

## 11. Task 10: Schedule Tasks Using Cron

### Objective

Automate repetitive tasks using cron.

**Open the current user's cron configuration:**

```bash
crontab -e
```

**List scheduled cron jobs:**

```bash
crontab -l
```

**Remove the current user's cron jobs:**

```bash
crontab -r
```

Be careful: this removes all cron jobs for the current user.

### Cron Syntax

```text
minute hour day-of-month month day-of-week command
```

Example:

```bash
0 2 * * * /home/devopsuser/backup.sh
```

This runs the backup script every day at 2:00 AM, using the server's configured local time.

### Common Cron Examples

| Schedule | Cron expression |
|---|---|
| Every minute | `* * * * *` |
| Every hour | `0 * * * *` |
| Every day at 2 AM | `0 2 * * *` |
| Every Sunday at midnight | `0 0 * * 0` |
| Every 15 minutes | `*/15 * * * *` |

### DevOps Scenario

A backup script needs to run automatically every day at 2 AM.

1. Create the backup script.
2. Make it executable.
3. Test it manually.
4. Add the cron entry.
5. Check cron logs and verify that the backup is created.

**Important:** Cron jobs run with a limited environment. Use absolute paths and redirect output to a log file when appropriate.

---

## 12. Task 11: Basic Application Deployment

### Objective

Understand the basic Linux steps involved in deploying a web application.

Example: Deploying static website files using Nginx.

**Step 1: Check whether Nginx is installed.**

```bash
nginx -v
```

**Step 2: Install Nginx if it is not installed.**

For Ubuntu or Debian:

```bash
sudo apt update
sudo apt install nginx
```

**Step 3: Start Nginx.**

```bash
sudo systemctl start nginx
```

**Step 4: Enable Nginx at startup.**

```bash
sudo systemctl enable nginx
```

**Step 5: Check its status.**

```bash
sudo systemctl status nginx
```

**Step 6: Identify the default web root.**

On many Ubuntu installations, it is:

```bash
/var/www/html
```

**Step 7: Create a test webpage.**

```bash
echo "<h1>My DevOps Practice Website</h1>" > index.html
```

**Step 8: Copy the webpage into the web root.**

```bash
sudo cp index.html /var/www/html/
```

**Step 9: Test the website locally.**

```bash
curl http://localhost
```

### DevOps Scenario

You need to deploy a simple static webpage on a Linux server.

The process includes installing a web server, copying website files, managing the service, and testing HTTP access.

**Note:** These are basic practice steps. Production deployments also require correct ownership, permissions, configuration validation, firewall rules, HTTPS, monitoring, and a rollback plan.

---

## 13. Task 12: Create a Basic System Monitoring Script

### Objective

Write a shell script that displays basic server resource information.

**Create the script:**

```bash
nano system-monitor.sh
```

Add the following content:

```bash
#!/bin/bash

echo "===== SYSTEM MONITOR ====="

echo "Hostname:"
hostname

echo "Current Date:"
date

echo "System Uptime:"
uptime

echo "Memory Usage:"
free -h

echo "Disk Usage:"
df -h

echo "Top CPU Processes:"
ps aux --sort=-%cpu | head -n 6

echo "===== MONITORING COMPLETED ====="
```

Save and exit Nano.

**Give execute permission:**

```bash
chmod +x system-monitor.sh
```

**Execute the script:**

```bash
./system-monitor.sh
```

### Explanation

| Command | Purpose |
|---|---|
| `hostname` | Displays hostname |
| `date` | Displays current date and time |
| `uptime` | Displays system uptime and load averages |
| `free -h` | Displays memory usage |
| `df -h` | Displays filesystem usage |
| `ps aux` | Displays running processes |
| `head -n 6` | Displays the first six lines |

### DevOps Use Case

A DevOps engineer can use a monitoring script to quickly inspect server resources during troubleshooting.

---

## 14. Common DevOps Troubleshooting Scenarios

### Scenario 1: Server Disk Is Full

**Problem:** An application cannot write files because the disk is full.

Commands:

```bash
df -h
du -sh /var/log
sudo du -xhd1 /var
```

Action: Identify the directory consuming space and follow an approved cleanup or retention procedure.

### Scenario 2: Application Service Is Down

**Problem:** The application is not responding.

Commands:

```bash
sudo systemctl status nginx
sudo journalctl -u nginx -n 50
```

Action: Investigate the error, correct the problem, and restart the service if required.

### Scenario 3: High CPU Usage

**Problem:** The server is running slowly.

Commands:

```bash
top
ps aux --sort=-%cpu | head
```

Action: Identify the resource-consuming process and investigate it before taking corrective action.

### Scenario 4: Permission Denied

**Problem:** A deployment script cannot execute.

Commands:

```bash
ls -l deploy.sh
chmod u+x deploy.sh
```

Action: Check ownership and permissions, then grant only the required access.

### Scenario 5: Application Port Is Not Listening

**Problem:** A web application cannot receive connections on its expected port.

Commands:

```bash
sudo ss -tulnp
```

Action: Verify that the application is running and listening on the expected address and port. Check firewall rules and application configuration.

### Scenario 6: DNS Resolution Failure

**Problem:** A server cannot resolve a domain name.

Commands:

```bash
getent hosts google.com
cat /etc/resolv.conf
```

Action: Investigate DNS configuration and connectivity to the configured DNS resolver.

---

## 15. Practical Exercises

Try to perform these tasks yourself in a Linux environment.

### Beginner Level

1. Display your Linux distribution and kernel version.
2. Display the current username.
3. Check the system uptime.
4. Display memory usage.
5. Display filesystem disk usage.
6. Create a directory named `devops-practice`.
7. Create a file named `deployment.txt`.
8. Give the owner execute permission on a script.

### Intermediate Level

9. Create a user named `testuser`.
10. Create a group named `developers`.
11. Add `testuser` to the `developers` group.
12. Display the top five CPU-consuming processes.
13. Find all lines containing `ERROR` in an application log.
14. Check the status of the Nginx service.
15. Display listening TCP and UDP ports.
16. Create a compressed archive of a directory.
17. Schedule a task using cron.

### DevOps Scenario Level

18. A server is running out of disk space. Explain how you would investigate it.
19. An application service is failing. Which commands would you use?
20. A deployment script cannot execute. How would you troubleshoot it?
21. A web application is not reachable. Explain your network troubleshooting steps.
22. Write a shell script that displays disk and memory usage.
23. Create a backup of an application directory and verify the archive.

**Practice rule:** Try each task yourself before checking the solution. Understand why each command is used.

---

## 16. Linux Practical DevOps Interview Questions

### Q1. How do you check disk usage in Linux?

Use:

```bash
df -h
```

It displays filesystem disk usage in a human-readable format.

### Q2. How do you check memory usage?

```bash
free -h
```

### Q3. How do you check running processes?

```bash
ps aux
```

You can also use `top` for real-time process monitoring.

### Q4. How do you check the status of a service?

```bash
sudo systemctl status service-name
```

### Q5. How do you view application logs?

```bash
tail -f /var/log/app.log
```

For services managed by systemd:

```bash
sudo journalctl -u service-name
```

### Q6. How do you find a process using a particular port?

```bash
sudo ss -tulnp
```

Look for the required port and associated process.

### Q7. How do you create a compressed backup?

```bash
tar -czvf backup.tar.gz directory/
```

### Q8. How do you schedule a script to run every day?

Use cron:

```bash
crontab -e
```

Example:

```bash
0 2 * * * /path/to/backup.sh
```

### Q9. What would you do if a Linux server became slow?

I would check CPU usage, memory usage, disk usage, running processes, and system logs. Then I would investigate the resource-consuming process and identify the root cause before applying a fix.

### Q10. What would you do if an application stopped working?

I would check the service status, review application logs, verify network connectivity, inspect resource usage, and investigate configuration or permission issues.

### Q11. How do you check whether a server can reach another server?

Use:

```bash
ping -c 4 server-ip
```

For a specific port:

```bash
nc -vz server-ip 443
```

### Q12. Why are shell scripts useful in DevOps?

Shell scripts automate repetitive tasks such as backups, monitoring, deployments, log checks, and system administration.

### Q13. What is the difference between `df` and `du`?

- `df` displays filesystem-level disk usage.
- `du` displays disk usage of files and directories.

### Q14. How do you check recent logs of a service?

```bash
sudo journalctl -u service-name -n 50
```

### Q15. What is the purpose of cron?

Cron is a Linux scheduling service used to run commands and scripts automatically at specified times or intervals.

---

