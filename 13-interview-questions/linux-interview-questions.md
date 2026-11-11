# Linux Interview Questions and Answers for DevOps Freshers

## 1. Introduction

Linux is one of the most important operating systems used in the DevOps industry.

DevOps engineers use Linux to:

- Manage servers.
- Deploy applications.
- Monitor system resources.
- Manage users and permissions.
- Troubleshoot application failures.
- Automate repetitive tasks.
- Manage services and processes.
- Configure networking.
- Work with cloud infrastructure.

This document contains frequently asked Linux interview questions for freshers preparing for entry-level DevOps roles.

---
- System calls.

- Network management.


**Interview answer:**

The Linux kernel is the core of the operating system. It manages hardware resources such as CPU, memory, storage, and devices and provides services to applications.


## Q3. What is the difference between Linux and Unix?


| Linux | Unix |

|---|---|
| Open-source kernel and operating-system ecosystem | Family of operating systems, many with proprietary implementations |
| Used in servers, desktops, and cloud environments | Common in enterprise and specialized environments |

| Many distributions are freely available | Licensing varies by implementation |

| Examples include Ubuntu and Debian | Examples include AIX and HP-UX |


## Q4. What is a Linux distribution?

A Linux distribution is an operating system built using the Linux kernel along with system utilities, software packages, libraries, and applications.


**Examples:**

- Ubuntu
- Debian
- Fedora
- Red Hat Enterprise Linux
- Rocky Linux
- Amazon Linux

## Q5. What is the difference between CLI and GUI?

**CLI:** Command Line Interface allows users to interact with the system using commands.

**GUI:** Graphical User Interface allows users to interact using graphical elements such as windows, icons, and menus.

**Interview answer:**

CLI is commonly used by DevOps engineers because it supports automation, scripting, and remote server administration. GUI is easier for some visual tasks but is not always available on production servers.

## Q6. What is the root user?

The root user is the superuser account in Linux.


It has extensive privileges to manage the operating system, users, files, permissions, and services.


**Check the current user:**


```bash
whoami

```


## Q7. What is the difference between root and a normal user?


| Root user | Normal user |
|---|---|

| Has extensive administrative privileges | Has limited permissions |
| Can manage system-wide configuration | Usually manages permitted resources |

| UID is normally 0 | Normally has a different UID |

| Can access protected system resources | Requires permission or elevated privileges |


## Q8. What is the purpose of the `sudo` command?


`sudo` allows an authorized user to execute a command with elevated privileges.


Example:


```bash
sudo apt update

```


**Interview answer:**


`sudo` is used to execute administrative commands without logging in directly as the root user. It supports controlled privilege elevation.


---

# PART 2: Basic Linux Commands


## Q9. How do you display the current working directory?


```bash
pwd
```

`pwd` stands for Print Working Directory.

## Q10. How do you list files and directories?

```bash
ls
```

Useful options:

```bash
ls -l
ls -a
ls -lh
ls -la
```

- `-l`: Long listing format.
- `-a`: Includes hidden files.
- `-h`: Human-readable file sizes.

## Q11. How do you create a directory?

```bash
mkdir devops
```

Create nested directories:

```bash
mkdir -p project/backend/logs
```

## Q12. How do you create a file?

```bash
touch file.txt
```

## Q13. How do you copy a file?

```bash
cp source.txt destination.txt
```

Copy a directory recursively:

```bash
cp -r source_dir backup_dir
```

## Q14. How do you move or rename a file?

```bash
mv old.txt new.txt
```

Move a file to another directory:

```bash
mv file.txt /tmp/
```

## Q15. How do you delete files and directories?

Delete a file:

```bash
rm file.txt
```

Delete an empty directory:

```bash
rmdir directory_name
```

Delete a directory and its contents:

```bash
rm -r directory_name
```

**Important:** Be careful with recursive deletion commands. Always verify the target before executing them.

## Q16. How do you view file contents?

```bash
cat file.txt
less file.txt
more file.txt
```

Display the beginning of a file:

```bash
head file.txt
```

Display the end of a file:

```bash
tail file.txt
```

## Q17. What is the difference between `cat`, `less`, and `more`?

| Command | Purpose |
|---|---|
| `cat` | Displays file contents |
| `less` | Allows flexible page-by-page navigation |
| `more` | Displays content one screen at a time |

## Q18. How do you search for a word inside a file?

```bash
grep "ERROR" app.log
```

Case-insensitive search:

```bash
grep -i "error" app.log
```

Search recursively:

```bash
grep -r "ERROR" /var/log/
```

## Q19. What is a pipe in Linux?

A pipe (`|`) sends the output of one command as the input to another command.

Example:

```bash
ps aux | grep nginx
```

Here, the output of `ps aux` is passed to `grep`.

## Q20. What is redirection in Linux?

Redirection sends command input or output to a file or another destination.

| Operator | Purpose |
|---|---|
| `>` | Redirect output and overwrite a file |
| `>>` | Append output to a file |
| `<` | Read input from a file |
| `2>` | Redirect standard error |
| `2>&1` | Redirect standard error to standard output |

Examples:

```bash
echo "Hello Linux" > output.txt
echo "New line" >> output.txt
ls /missing 2> error.log
```

---

# PART 3: Users and Permissions

## Q21. How do you create a user in Linux?

```bash
sudo useradd -m devopsuser
```

Set the user's password:

```bash
sudo passwd devopsuser
```

## Q22. How do you create a group?

```bash
sudo groupadd developers
```

## Q23. How do you add a user to a group?

```bash
sudo usermod -aG developers devopsuser
```

The `-aG` options append the user to the supplementary group without replacing existing supplementary group memberships.

## Q24. How do you check user information?

```bash
id devopsuser
```

## Q25. What are Linux file permissions?

Linux file permissions control which users can read, write, or execute files and directories.

There are three permission categories:

- User (owner)
- Group
- Others

There are three basic permissions:

| Permission | Symbol | Numeric value |
|---|---|---|
| Read | r | 4 |
| Write | w | 2 |
| Execute | x | 1 |

## Q26. Explain `chmod 755`.

```bash
chmod 755 deploy.sh
```

Explanation:

- Owner: 7 = read + write + execute.
- Group: 5 = read + execute.
- Others: 5 = read + execute.

## Q27. What is the difference between `chmod` and `chown`?

| chmod | chown |
|---|---|
| Changes permissions | Changes ownership |
| Controls read, write, and execute access | Changes file owner or group |
| Example: `chmod 755 file` | Example: `chown user file` |

## Q28. What is the difference between `chown` and `chgrp`?

- `chown` changes the owner and can also change the group.
- `chgrp` changes the group ownership.

Examples:

```bash
sudo chown devopsuser file.txt
sudo chgrp developers file.txt
```

## Q29. What is umask?

`umask` controls which permission bits are removed from the default permissions when new files and directories are created.

Check the current value:

```bash
umask
```

A common umask value is `022`.

For ordinary creation defaults:

- Files: 666 - 022 = 644.
- Directories: 777 - 022 = 755.

Actual permissions can also be affected by applications and filesystem behavior.

---

# PART 4: Process Management

## Q30. What is a process in Linux?

A process is a running instance of a program.

Each process normally has a unique Process ID (PID).

## Q31. How do you view running processes?

```bash
ps aux
```

For real-time monitoring:

```bash
top
```

## Q32. What is a PID?

PID stands for Process ID.

It is a unique identifier assigned to a process by the operating system.

## Q33. How do you find the PID of a process?

```bash
pgrep nginx
```

Or:

```bash
ps aux | grep nginx
```

## Q34. How do you terminate a process?

Graceful termination:

```bash
kill PID
```

Forceful termination:

```bash
kill -9 PID
```

**Interview answer:**

I first try to terminate a process gracefully using `kill`. I use `kill -9` only when normal termination fails because it does not allow the process to handle cleanup.

## Q35. What is the difference between `top` and `ps`?

| top | ps |
|---|---|
| Displays processes in real time | Displays a process snapshot |
| Useful for continuous monitoring | Useful for inspecting process details |
| Updates periodically | Does not continuously update by default |

## Q36. What is a daemon?

A daemon is a background process that provides services or performs tasks without direct user interaction.

Examples include:

- `sshd`
- `cron`
- `systemd-journald`

---

# PART 5: Disk and Package Management

## Q37. How do you check disk usage?

```bash
df -h
```

It displays filesystem space usage in a human-readable format.

## Q38. What is the difference between `df` and `du`?

| df | du |
|---|---|
| Displays filesystem-level disk usage | Displays file or directory disk usage |
| Useful for checking mounted filesystem capacity | Useful for finding which directories consume space |
| Example: `df -h` | Example: `du -sh /var/log` |

## Q39. How do you find large directories?

```bash
sudo du -xhd1 /var
```

This displays the sizes of directories one level below `/var`, without crossing filesystem boundaries.

## Q40. How do you check memory usage?

```bash
free -h
```

## Q41. What is a package manager?

A package manager is a tool used to install, update, configure, and remove software packages.

Examples:

| Distribution family | Package manager |
|---|---|
| Ubuntu and Debian | APT |
| Fedora | DNF |
| RHEL | DNF or YUM, depending on version |
| Arch Linux | Pacman |

## Q42. How do you install a package in Ubuntu?

```bash
sudo apt update
sudo apt install nginx
```

## Q43. How do you remove a package?

```bash
sudo apt remove nginx
```

---

# PART 6: Networking

## Q44. What is an IP address?

An IP address is a logical address assigned to a network interface so that devices can communicate over an IP network.

There are two main versions:

- IPv4
- IPv6

## Q45. What is the difference between public and private IP addresses?

- A public IP address is globally routable on the internet.
- A private IP address is used inside private networks and is not directly routable across the public internet.

## Q46. What is DNS?

DNS stands for Domain Name System.

It translates domain names into IP addresses so clients can locate servers.

Example:

```bash
nslookup google.com
```

## Q47. What is the difference between TCP and UDP?

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Provides reliable, ordered delivery | Does not guarantee delivery or ordering |
| Uses acknowledgments and retransmissions | Has lower protocol overhead |
| Examples: HTTPS and SSH | Examples: DNS queries and real-time media traffic |

## Q48. What is a port number?

A port number identifies a particular application or service endpoint on a host.

Common ports:

| Port | Service |
|---|---|
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |

## Q49. How do you check IP addresses in Linux?

```bash
ip addr
```

## Q50. How do you check network connectivity?

```bash
ping -c 4 8.8.8.8
```

## Q51. How do you check listening ports?

```bash
sudo ss -tulnp
```

## Q52. What is SSH?

SSH stands for Secure Shell.

It is a network protocol used to securely access and manage remote systems.

Example:

```bash
ssh username@server-ip
```

---

# PART 7: Services and systemd

## Q53. What is systemd?

systemd is a system and service manager used by many modern Linux distributions.

It manages system startup, services, and other system resources.

## Q54. What is systemctl?

`systemctl` is the command-line utility used to manage systemd services and units.

## Q55. How do you check service status?

```bash
sudo systemctl status nginx
```

## Q56. How do you start, stop, and restart a service?

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

## Q57. What is the difference between `start` and `enable`?

- `start` starts the service immediately.
- `enable` configures the service to start automatically during boot.

## Q58. How do you enable a service at startup?

```bash
sudo systemctl enable nginx
```

## Q59. How do you check whether a service is active?

```bash
systemctl is-active nginx
```

---

# PART 8: Logs and Monitoring

## Q60. What are Linux logs?

Logs are records of system events, application activity, errors, warnings, and other operational information.

They help administrators investigate and troubleshoot problems.

## Q61. Where are Linux log files commonly stored?

```bash
/var/log/
```

Examples:

- `/var/log/syslog` on many Debian/Ubuntu systems.
- `/var/log/messages` on many RHEL-family systems.
- `/var/log/auth.log` on many Debian/Ubuntu systems.
- `/var/log/secure` on many RHEL-family systems.

The exact files depend on the distribution and logging configuration.

## Q62. How do you view the latest log entries?

```bash
tail -n 20 /var/log/syslog
```

## Q63. How do you monitor logs continuously?

```bash
tail -f /var/log/app.log
```

## Q64. How do you search for errors in logs?

```bash
grep -i "error" /var/log/app.log
```

## Q65. What is journalctl?

`journalctl` is a command used to query logs collected by systemd-journald.

Example:

```bash
sudo journalctl -u nginx
```

Display the latest 50 entries:

```bash
sudo journalctl -u nginx -n 50
```

Follow logs continuously:

```bash
sudo journalctl -u nginx -f
```

## Q66. What is log rotation?

Log rotation manages log files by rotating, compressing, and removing old logs according to configured policies.

It helps prevent logs from consuming excessive disk space.

A common Linux utility for this is `logrotate`.

---

# PART 9: Shell Scripting

## Q67. What is a shell script?

A shell script is a file containing a sequence of shell commands that can be executed together to automate tasks.

Example:

```bash
#!/bin/bash

echo "Hello DevOps"
date
hostname
```

## Q68. What is a shebang?

A shebang is the first line of a script that specifies which interpreter should execute it.

Example:

```bash
#!/bin/bash
```

## Q69. How do you execute a shell script?

First, give it execute permission:

```bash
chmod +x script.sh
```

Then execute it:

```bash
./script.sh
```

Alternatively:

```bash
bash script.sh
```

## Q70. What are variables in shell scripting?

Variables store values that can be used later in a script.

Example:

```bash
name="DevOps"
echo "$name"
```

## Q71. What is the difference between `$1` and `$@`?

- `$1` represents the first positional argument passed to a script.
- `"$@"` represents all positional arguments as separate quoted arguments.

## Q72. What is an exit status?

An exit status is a numeric value returned by a command or script to indicate its result.

- `0` generally means success.
- A non-zero value indicates an error or another unsuccessful result.

Check the previous command's status:

```bash
echo $?
```

## Q73. What is cron?

Cron is a time-based job scheduler used to run commands and scripts automatically.

Edit cron jobs:

```bash
crontab -e
```

Example: Run a backup every day at 2 AM.

```bash
0 2 * * * /home/user/backup.sh
```

---

# PART 10: Real-World DevOps Troubleshooting

## Q74. A Linux server is running slowly. How would you troubleshoot it?

**Answer:**

I would investigate CPU, memory, disk, and process usage.

Commands:

```bash
top
free -h
df -h
uptime
ps aux --sort=-%cpu | head
```

Then I would inspect logs, identify the resource bottleneck, and take corrective action after understanding the root cause.

## Q75. An application is not responding. What would you check?

**Answer:**

I would:

1. Check whether the application process is running.
2. Check the service status.
3. Inspect application logs.
4. Verify the listening port.
5. Check network connectivity.
6. Review configuration and permissions.

Commands:

```bash
ps aux | grep application
sudo systemctl status application
sudo journalctl -u application -n 50
sudo ss -tulnp
```

## Q76. A server disk is full. What would you do?

**Answer:**

I would first identify the affected filesystem and then locate directories consuming space.

Commands:

```bash
df -h
sudo du -xhd1 /var
sudo du -sh /var/log/*
```

I would investigate large files, review log retention, and use an approved cleanup or log-rotation process.

I would not delete files blindly.

## Q77. A deployment script shows Permission Denied. How would you fix it?

**Answer:**

I would inspect the file permissions and ownership.

```bash
ls -l deploy.sh
```

If the owner needs execute permission:

```bash
chmod u+x deploy.sh
```

I would also verify the script's interpreter, file ownership, and directory permissions.

## Q78. An application cannot connect to a database. What would you check?

**Answer:**

I would verify:

1. Whether the database service is running.
2. Whether the database is listening on the expected port.
3. Whether DNS resolution works.
4. Whether network connectivity is available.
5. Whether firewall rules permit the connection.
6. Whether application credentials and database configuration are correct.

Useful commands:

```bash
sudo systemctl status mysql
sudo ss -tulnp
getent hosts database-host
nc -vz database-host 3306
```

The database service name and port depend on the actual database configuration.

## Q79. A website is not accessible. How would you troubleshoot it?

**Answer:**

I would check:

1. Web server service status.
2. Web server configuration.
3. Listening ports.
4. Local HTTP response.
5. DNS resolution.
6. Firewall and network connectivity.
7. Application and web server logs.

Commands:

```bash
sudo systemctl status nginx
sudo nginx -t
sudo ss -tulnp
curl -I http://localhost
getent hosts example.com
```

## Q80. How would you investigate repeated application errors?

**Answer:**

I would inspect application logs and system logs, identify the error pattern, determine when it started, and correlate it with recent deployments or configuration changes.

Commands:

```bash
grep -i "error" /var/log/app.log
tail -f /var/log/app.log
sudo journalctl -u application -n 100
```

---

# PART 11: Scenario-Based Interview Questions

## Scenario 1: A server has high CPU usage. What commands would you use?

**Expected answer:**

```bash
top
ps aux --sort=-%cpu | head
uptime
```

I would identify the process consuming high CPU and investigate why it is using excessive resources.

## Scenario 2: A server has low available memory. How would you investigate?

**Expected answer:**

```bash
free -h
top
ps aux --sort=-%mem | head
```

I would identify memory-intensive processes and investigate whether memory usage is expected or abnormal.

## Scenario 3: You need to find all `.log` files under `/var/log`.

**Expected answer:**

```bash
sudo find /var/log -type f -name "*.log"
```

## Scenario 4: You need to find files larger than 100 MB.

**Expected answer:**

```bash
sudo find /var -type f -size +100M
```

## Scenario 5: You need to check whether a process is running.

**Expected answer:**

```bash
pgrep nginx
```

Or:

```bash
ps aux | grep nginx
```

## Scenario 6: You need to check the last 100 lines of an application log.

**Expected answer:**

```bash
tail -n 100 /var/log/app.log
```

## Scenario 7: You need to archive an application directory.

**Expected answer:**

```bash
tar -czvf application-backup.tar.gz application/
```

## Scenario 8: You need to find all files owned by a particular user.

**Expected answer:**

```bash
sudo find / -user devopsuser -type f 2>/dev/null
```

## Scenario 9: You need to check the current listening ports.

**Expected answer:**

```bash
sudo ss -tulnp
```

## Scenario 10: You need to check the last system boot time.

**Expected answer:**

```bash
who -b
```

---

# PART 12: Practical Interview Exercises

Try to solve these tasks without looking at the answers.

### Beginner Level

1. Display the current Linux username.
2. Display the current working directory.
3. List all files, including hidden files.
4. Create a directory called `interview-practice`.
5. Create a file called `notes.txt`.
6. Display the contents of the file.
7. Find the current kernel version.
8. Display system uptime.

### Intermediate Level

9. Create a user named `interviewuser`.
10. Create a group named `devops`.
11. Add the user to the group.
12. Set file permissions to `755`.
13. Find a process by name.
14. Check filesystem disk usage.
15. Find large files in a directory.
16. Search for the word `ERROR` in a log file.
17. Check the status of Nginx.
18. Display listening ports.

### DevOps Scenario Level

19. Troubleshoot a server with high CPU usage.
20. Troubleshoot an application that is not starting.
21. Investigate a full disk.
22. Identify why a deployment script cannot execute.
23. Check whether a database port is reachable.
24. Create and verify a compressed backup.
25. Write a script that displays CPU, memory, and disk information.
26. Schedule a backup script using cron.
27. Explain how you would troubleshoot an HTTP 500 error.
28. Explain how you would investigate a DNS resolution failure.

---

# PART 13: Important Linux Commands for Quick Revision

| Category | Command | Purpose |
|---|---|---|
| Navigation | `pwd` | Current directory |
| Navigation | `ls -la` | List all files |
| Navigation | `cd` | Change directory |
| Files | `touch` | Create a file |
| Files | `mkdir -p` | Create directories |
| Files | `cp` | Copy files |
| Files | `mv` | Move or rename files |
| Files | `rm` | Remove files |
| Files | `find` | Search for files |
| Text | `cat` | Display file contents |
| Text | `grep` | Search text |
| Text | `sed` | Stream editing |
| Text | `awk` | Text processing |
| Users | `whoami` | Current username |
| Users | `id` | User and group IDs |
| Permissions | `chmod` | Change permissions |
| Permissions | `chown` | Change ownership |
| Processes | `ps aux` | List processes |
| Processes | `top` | Monitor processes |
| Processes | `kill` | Send a signal to a process |
| Disk | `df -h` | Filesystem usage |
| Disk | `du -sh` | Directory size |
| Memory | `free -h` | Memory usage |
| Networking | `ip addr` | IP addresses |
| Networking | `ping` | Test connectivity |
| Networking | `ss -tulnp` | Listening sockets |
| Services | `systemctl` | Manage services |
| Logs | `journalctl` | Query systemd logs |
| Logs | `tail -f` | Follow log output |
| Archive | `tar` | Archive and extract files |
| Scheduling | `crontab` | Manage cron jobs |

---

# PART 14: Tips for Linux DevOps Interviews

1. Understand concepts instead of memorizing commands.
2. Practice Linux commands in a real terminal.
3. Explain what a command does before giving the syntax.
4. Understand file permissions and ownership thoroughly.
5. Practice process and service troubleshooting.
6. Learn how to read and investigate logs.
7. Understand networking fundamentals and common ports.
8. Practice basic Bash scripting.
9. Explain your troubleshooting approach step by step.
10. Never recommend destructive commands without checking their impact.
11. Use practical examples from your Linux learning repository.
12. Be honest about your experience and distinguish practice projects from professional experience.

### A Good Structure for Answering Scenario Questions

When an interviewer gives you a troubleshooting problem, follow this sequence:

1. **Understand:** Identify the reported problem.
2. **Inspect:** Collect information using suitable commands.
3. **Analyze:** Identify the likely root cause.
4. **Resolve:** Apply the safest appropriate fix.
5. **Verify:** Confirm that the issue is resolved.
6. **Prevent:** Recommend monitoring or automation to reduce recurrence.

---


