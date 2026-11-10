# Linux Services and Systemd

## 1. Introduction to Linux Services

### What is a Linux Service?

A Linux service is a program that runs in the background to perform specific tasks without requiring continuous user interaction.

Services are generally used to provide functionality such as:
- Web hosting.
- Database management.
- Network connectivity.
- SSH remote access.
- Monitoring.
- Application execution.
- Scheduling automated tasks.

### Examples of Linux Services

| Service | Purpose |
|---|---|
| ssh / sshd | Provides SSH remote access |
| nginx | Web server |
| apache2 / httpd | Web server |
| cron / crond | Executes scheduled tasks |
| docker | Manages containers |
| mariadb | Database server |
| rsyslog | System logging |

### Real-Time DevOps Example

Suppose you deploy an application on an Ubuntu server.

You install Nginx:

```bash
sudo apt install nginx
```

After installation, you need to start the Nginx service:

```bash
sudo systemctl start nginx
```


Now Nginx can run in the background and serve web requests.


---


## 2. What is a Daemon?


A daemon is a background process that provides services or performs system tasks.


Daemons generally run without direct user interaction.

Many Linux daemon processes have names ending in the letter `d`.

Examples:

- `sshd` – SSH daemon.
- `crond` – Cron daemon.
- `systemd` – System and service manager.
- `rsyslogd` – System logging daemon.

### Difference Between a Process, Service, and Daemon

| Term | Meaning |
|---|---|
| Process | A running instance of a program |
| Daemon | A background process that performs tasks or provides services |
| Service | A system-managed application or functionality |

**Interview point:** Every daemon is a process, but not every process is a daemon.

---

## 3. What is Init System?

An init system is the first userspace process started by the Linux kernel during boot.

Its primary responsibilities include:

- Starting system services.
- Managing system initialization.
- Handling service dependencies.
- Managing system shutdown.
- Controlling the startup process.

The init system normally runs as PID 1.

Check the process with PID 1:

```bash
ps -p 1 -o pid,comm,args
```

### Common Linux Init Systems

1. SysVinit – Traditional initialization system.
2. Upstart – Event-based init system historically used by Ubuntu.
3. systemd – Modern system and service manager.

Most modern Ubuntu, Debian, Fedora, RHEL, Rocky Linux, and AlmaLinux installations use systemd by default.

**Important:** Not every Linux distribution uses systemd. Some distributions use alternatives such as OpenRC or runit.

---

## 4. What is systemd?

### Definition

systemd is a system and service manager used by many modern Linux distributions.

It is responsible for initializing the system, managing services, handling dependencies, and controlling system startup and shutdown.

systemd starts as PID 1 during the normal system boot process.

### Responsibilities of systemd

- Starts services during system boot.
- Stops and restarts services.
- Manages service dependencies.
- Monitors service states.
- Manages system targets.
- Collects system logs through journald.
- Supports scheduled tasks using timer units.
- Manages mount points and sockets.

### Check Whether systemd Is Running

```bash
ps -p 1 -o comm=
```

Expected output on a system using systemd:

```text
systemd
```

Check the systemd version:

```bash
systemctl --version
```

Check the default boot target:

```bash
systemctl get-default
```

---

## 5. What is systemctl?

`systemctl` is the command-line utility used to communicate with systemd and manage system services and other systemd units.

It is one of the most important commands for Linux administration and DevOps interviews.

### Basic Syntax

```bash
systemctl [OPTIONS] COMMAND UNIT
```

Example:

```bash
sudo systemctl start nginx
```

Here:
- `systemctl` – systemd management command.
- `start` – operation.
- `nginx` – service name.

---

## 6. Managing Linux Services Using systemctl

For the following examples, assume that Nginx is installed.

### 6.1 Check Service Status

Syntax:

```bash
systemctl status service_name
```

Example:

```bash
systemctl status nginx
```

Displays:
- Whether the service is loaded.
- Whether it is enabled at boot.
- Whether it is currently active.
- Main process ID.
- Recent service log messages.

Common service states:

| State | Meaning |
|---|---|
| active (running) | Service is running |
| active (exited) | Service completed successfully but has no continuously running main process |
| inactive (dead) | Service is not currently running |
| failed | Service encountered an error |
| activating | Service is starting |
| deactivating | Service is stopping |

### 6.2 Start a Service

Starts a service immediately.

```bash
sudo systemctl start nginx
```

Verify:

```bash
systemctl status nginx
```

### 6.3 Stop a Service

Stops a running service.

```bash
sudo systemctl stop nginx
```

### 6.4 Restart a Service

Stops and starts a service again.

```bash
sudo systemctl restart nginx
```

Use this when configuration changes require a complete restart or when a service needs restarting.

### 6.5 Reload a Service

Requests a service to reload its configuration without completely restarting the service.

```bash
sudo systemctl reload nginx
```

This works only when the service supports reload operations.

For Nginx, you can also test the configuration first:

```bash
sudo nginx -t
```

Then reload:

```bash
sudo systemctl reload nginx
```

### 6.6 Enable a Service

Configures a service to start automatically during applicable system boot targets.


```bash

sudo systemctl enable nginx

```


### 6.7 Disable a Service


Prevents a service from being automatically started through its normal enablement configuration.


```bash
sudo systemctl disable nginx
```


Disabling a service does not necessarily stop it if it is already running.

### 6.8 Enable and Start a Service Together


```bash

sudo systemctl enable --now nginx
```

This enables the service and starts it immediately.

### 6.9 Disable and Stop a Service

```bash
sudo systemctl disable --now nginx
```

Disables normal boot enablement and stops the currently running service.

### 6.10 Check Whether a Service Is Active

```bash
systemctl is-active nginx
```

Possible output:

```text
active
```

### 6.11 Check Whether a Service Is Enabled

```bash
systemctl is-enabled nginx
```

Possible output:

```text
enabled
```

### 6.12 List Running Services

```bash
systemctl list-units --type=service
```

List all loaded service units, including inactive ones:

```bash
systemctl list-units --type=service --all
```

### 6.13 List Installed Service Unit Files

```bash
systemctl list-unit-files --type=service
```

This shows service unit files and their enablement states.

### 6.14 View Service Configuration

```bash
systemctl cat nginx
```

Displays the main service unit file and applicable drop-in configuration files.

### 6.15 View Detailed Service Properties

```bash
systemctl show nginx
```

Displays detailed properties of the service unit.

---

## 7. Difference Between Start, Enable, Restart, and Reload

This is an important DevOps interview topic.

| Command | Purpose |
|---|---|
| `systemctl start nginx` | Starts the service now |
| `systemctl stop nginx` | Stops the service now |
| `systemctl restart nginx` | Restarts the service |
| `systemctl reload nginx` | Reloads service configuration |
| `systemctl enable nginx` | Enables automatic startup |
| `systemctl disable nginx` | Disables normal automatic startup |
| `systemctl enable --now nginx` | Enables and starts the service |
| `systemctl disable --now nginx` | Disables and stops the service |

**Important interview point:**

Starting a service does not automatically enable it at boot.

Enabling a service does not necessarily start it immediately.

---

## 8. What are systemd Unit Files?

A unit file is a configuration file that describes how systemd should manage a particular resource or task.

Unit files define:
- How a service starts.
- Which command should execute.
- Dependencies.
- Startup behavior.
- Restart policies.
- Configuration settings.

### Common Unit File Types

| Unit extension | Purpose |
|---|---|
| `.service` | Manages services |
| `.socket` | Manages sockets |
| `.target` | Groups units and defines system states |
| `.timer` | Schedules tasks |
| `.mount` | Manages mount points |
| `.automount` | Manages automatic mounting |
| `.path` | Monitors filesystem paths |
| `.device` | Represents devices |
| `.swap` | Manages swap devices or files |

### Where Are Unit Files Stored?

Common system unit file locations:

**Ubuntu/Debian:**

```text
/etc/systemd/system/
/lib/systemd/system/
/usr/lib/systemd/system/
```

**RHEL/Fedora/Rocky Linux:**

```text
/etc/systemd/system/
/usr/lib/systemd/system/
```

Some distributions use `/lib/systemd/system` as a compatibility path or symlink to `/usr/lib/systemd/system`.

### Important Difference

- `/etc/systemd/system/` – Administrator-managed configuration and custom unit files.
- `/usr/lib/systemd/system/` – Vendor-provided unit files on many modern distributions.
- `/lib/systemd/system/` – Common vendor unit location on Debian-based systems.

Avoid editing vendor-provided unit files directly. Use administrator-managed overrides or drop-in files.

---

## 9. Structure of a systemd Service Unit File

A systemd service unit commonly contains three main sections.

### 9.1 [Unit] Section

Defines service information and dependencies.

Common directives:

- `Description` – Describes the service.
- `After` – Defines ordering after another unit.
- `Wants` – Expresses a weaker dependency.
- `Requires` – Defines a stronger dependency relationship.

### 9.2 [Service] Section

Defines how the service runs.

Common directives:

- `Type` – Defines the service startup behavior.
- `ExecStart` – Command used to start the service.
- `ExecStop` – Optional command used to stop the service.
- `ExecReload` – Optional command used to reload configuration.
- `Restart` – Defines restart behavior.
- `User` – Defines the account used to run the service.
- `WorkingDirectory` – Defines the working directory.
- `Environment` – Defines environment variables.

### 9.3 [Install] Section

Defines installation and enablement relationships.

Common directives:

- `WantedBy` – Defines the target that should pull in the service when enabled.

### Example Service Unit File

```ini
[Unit]
Description=My Application Service
After=network.target

[Service]
Type=simple
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/python3 /opt/myapp/app.py
Restart=on-failure
RestartSec=5
User=myapp

[Install]
WantedBy=multi-user.target
```

Explanation:

- `Description` – Describes the application.
- `After=network.target` – Starts after the network target has been reached.
- `Type=simple` – Treats the process started by `ExecStart` as the main service process.
- `WorkingDirectory` – Sets the application's working directory.
- `ExecStart` – Runs the application.
- `Restart=on-failure` – Restarts the service when it exits unsuccessfully.
- `RestartSec=5` – Waits five seconds before a restart attempt.
- `User=myapp` – Runs the application as the `myapp` account.
- `WantedBy=multi-user.target` – Allows the service to be enabled for the multi-user target.

The `myapp` account and application files must exist before this example can work.

---

## 10. Types of systemd Services

The `Type` directive controls how systemd understands service startup.

### 10.1 Type=simple

The process started by `ExecStart` is considered the main service process.

```ini
[Service]
Type=simple
ExecStart=/usr/bin/python3 /opt/myapp/app.py
```

### 10.2 Type=exec

systemd waits until the configured executable has been successfully executed.

```ini
[Service]
Type=exec
ExecStart=/usr/bin/python3 /opt/myapp/app.py
```

### 10.3 Type=forking

Used for traditional services that start a process and then fork into the background.

```ini
[Service]
Type=forking
ExecStart=/usr/sbin/example-service
```

For reliable tracking, the service may also require a `PIDFile` directive, depending on how the application works.

### 10.4 Type=oneshot

Used for tasks that run once and then finish.

```ini
[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
```

Example use cases:
- Backup scripts.
- Cleanup tasks.
- One-time configuration tasks.

### 10.5 Type=notify

Used by applications that notify systemd when startup is complete through the systemd notification mechanism.

```ini
[Service]
Type=notify
ExecStart=/usr/bin/example-service
```

The application must support systemd notifications.

---

## 11. What are systemd Targets?

A target is a systemd unit used to group other units and define a particular system state.

Targets help systemd organize the boot process and service dependencies.

### Common systemd Targets

| Target | Purpose |
|---|---|
| `poweroff.target` | Shuts down the system |
| `rescue.target` | Starts a rescue environment |
| `multi-user.target` | Provides a multi-user non-graphical system |
| `graphical.target` | Provides a graphical login environment |
| `reboot.target` | Reboots the system |

### Check Default Target

```bash
systemctl get-default
```

### Change Default Target

Set multi-user mode as the default:

```bash
sudo systemctl set-default multi-user.target
```

Set graphical mode as the default:

```bash
sudo systemctl set-default graphical.target
```

### Switch Target Immediately

```bash
sudo systemctl isolate multi-user.target
```

**Warning:** Isolating a target can stop services that are not required by the selected target. Be especially careful when working through SSH or on production servers.

---

## 12. Difference Between Runlevels and systemd Targets

Traditional SysVinit systems use runlevels to define system operating states.

systemd uses targets for similar purposes.

| SysVinit runlevel | Common systemd equivalent |
|---|---|
| 0 | poweroff.target |
| 1 | rescue.target |
| 2 | multi-user.target or distribution-specific configuration |
| 3 | multi-user.target |
| 4 | Usually unused or custom |
| 5 | graphical.target |
| 6 | reboot.target |

**Important:** The mapping is approximate. Runlevel meanings can differ between distributions, especially for runlevels 2, 3, and 4.

---

## 13. What is systemd Journal?

systemd includes a logging service called journald.

`systemd-journald` collects and stores system and service logs.

These logs can be accessed using the `journalctl` command.

### What is journalctl?

`journalctl` is a command-line tool used to view and analyze logs collected by systemd's journal.

It is very useful for troubleshooting failed services and system problems.

### 13.1 View All Logs

```bash
journalctl
```

### 13.2 View Logs for a Particular Service

```bash
journalctl -u nginx
```

### 13.3 View Recent Logs

```bash
journalctl -n 50
```

Displays the latest 50 log entries.

### 13.4 Follow Logs in Real Time

```bash
journalctl -f
```

Similar to:

```bash
tail -f /var/log/syslog
```

However, journalctl reads the systemd journal, whereas tail reads a file.

### 13.5 Follow Logs for a Particular Service

```bash
journalctl -u nginx -f
```

Useful for monitoring application or web server logs.

### 13.6 View Logs from Current Boot

```bash
journalctl -b
```

### 13.7 View Logs from Previous Boot

```bash
journalctl -b -1
```

This requires the previous boot's journal to be available and retained.

### 13.8 View Logs Since a Particular Time

```bash
journalctl --since "1 hour ago"
```

Another example:

```bash
journalctl --since "2026-11-10 10:00:00"
```

### 13.9 View Logs Within a Time Range

```bash
journalctl --since "2026-11-10 10:00:00" --until "2026-11-10 11:00:00"
```

### 13.10 View Only Error Messages

```bash
journalctl -p err
```

### 13.11 View Logs Without a Pager

```bash
journalctl --no-pager
```

### 13.12 Check Journal Disk Usage

```bash
journalctl --disk-usage
```

---

## 14. What is daemon-reload?

When you create or modify a systemd unit file, systemd may not automatically recognize the changes.

You must reload its unit configuration.

Command:

```bash
sudo systemctl daemon-reload
```

Purpose:

- Reloads systemd unit configuration.
- Makes newly created unit files available.
- Recognizes changes made to unit files.

Important:

`daemon-reload` does not restart running services.

If a running service needs to use the changed configuration, you may also need to reload or restart that service.

### Example

After editing a service file:

```bash
sudo nano /etc/systemd/system/myapp.service
```

Execute:

```bash
sudo systemctl daemon-reload
```

Then:

```bash
sudo systemctl restart myapp
```

---

## 15. Creating a Custom systemd Service

This is an important practical DevOps exercise.

Suppose you have a shell script that should run continuously in the background.

### Step 1: Create a Script

```bash
sudo nano /usr/local/bin/myapp.sh
```

Add:

```bash
#!/bin/bash

while true
do
    echo "My application is running"
    sleep 30
done
```

Save the script.

### Step 2: Make the Script Executable

```bash
sudo chmod +x /usr/local/bin/myapp.sh
```

### Step 3: Create a systemd Service File

```bash
sudo nano /etc/systemd/system/myapp.service
```

Add:

```ini
[Unit]
Description=My Custom Application Service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/myapp.sh
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### Step 4: Reload systemd

```bash
sudo systemctl daemon-reload
```

### Step 5: Start the Service

```bash
sudo systemctl start myapp
```

### Step 6: Check Service Status

```bash
sudo systemctl status myapp
```

### Step 7: Enable Automatic Startup

```bash
sudo systemctl enable myapp
```

Or perform both operations together:

```bash
sudo systemctl enable --now myapp
```

### Step 8: View Service Logs

```bash
sudo journalctl -u myapp -f
```

Expected log message:

```text
My application is running
```

### Step 9: Stop the Service

```bash
sudo systemctl stop myapp
```

### Step 10: Disable the Service

```bash
sudo systemctl disable myapp
```

This is a simple demonstration service. In production, use an appropriate dedicated service account, proper application logging, security restrictions, and a meaningful health-check strategy.

---

## 16. Masking and Unmasking Services

### What is Masking?

Masking prevents a service from being started manually or automatically through systemd while it remains masked.

Command:

```bash
sudo systemctl mask nginx
```

A masked service is linked to `/dev/null`.

Even attempting to start it normally will fail.

### What is Unmasking?

Unmasking removes the mask and allows the service to be started again.

```bash
sudo systemctl unmask nginx
```

### Difference Between Disable and Mask

| Disable | Mask |
|---|---|
| Prevents normal automatic startup through enablement | Prevents the unit from being started |
| Service can still be started manually | Normal manual startup is blocked |
| Removes enablement links | Creates a mask |

Use masking carefully because it can interfere with other services that depend on the masked unit.

---

## 17. Service Dependencies and Ordering

systemd manages relationships between services using dependency and ordering directives.

### Common Directives

**After**

```ini
After=network.target
```

Defines ordering. It does not automatically require or start the referenced unit.

**Before**

```ini
Before=nginx.service
```

Defines that the current unit should be started before Nginx.

**Requires**

```ini
Requires=network.service
```

Creates a stronger dependency. If the required unit is started, systemd also attempts to start the dependency. Failure handling depends on the dependency relationship.

**Wants**

```ini
Wants=network.target
```

Creates a weaker dependency. systemd attempts to start the wanted unit, but failure does not normally cause the requesting unit to fail.

**Important interview point:**

- `After` and `Before` control ordering.
- `Requires` and `Wants` express dependencies.
- Ordering and dependency are different concepts.

---

## 18. Service Troubleshooting

A DevOps engineer should know how to identify and fix service failures.

### Scenario 1: Nginx Service Is Not Starting

Check status:

```bash
sudo systemctl status nginx
```

Check logs:

```bash
sudo journalctl -u nginx -n 50
```

Test Nginx configuration:

```bash
sudo nginx -t
```

Check whether another application is using port 80:

```bash
sudo ss -tulpn | grep ':80'
```

Fix the identified problem and retry:

```bash
sudo systemctl restart nginx
```

### Scenario 2: Service Fails After Reboot

Check whether the service is enabled:

```bash
systemctl is-enabled nginx
```

If disabled:

```bash
sudo systemctl enable nginx
```

Check failed services:

```bash
systemctl --failed
```

Check boot logs:

```bash
journalctl -b -u nginx
```

### Scenario 3: Service Configuration Changes Are Not Applied

Check configuration syntax first, if the application supports a validation command.

Then reload systemd unit files if the unit file itself changed:

```bash
sudo systemctl daemon-reload
```

Reload the application:

```bash
sudo systemctl reload nginx
```

If reload is unsupported or insufficient:

```bash
sudo systemctl restart nginx
```

### Scenario 4: Service Keeps Restarting

Check:

```bash
systemctl status myapp
```

View logs:

```bash
journalctl -u myapp -n 100
```

Investigate:

- Application errors.
- Incorrect executable paths.
- Missing dependencies.
- Permission problems.
- Configuration errors.
- Incorrect restart policies.

### Scenario 5: Service Is Active but Application Is Not Accessible

Check service status:

```bash
systemctl status nginx
```

Check listening ports:

```bash
sudo ss -tulpn
```

Test locally:

```bash
curl -I http://localhost
```

Check firewall configuration.

Ubuntu with UFW:

```bash
sudo ufw status
```

RHEL-based systems using firewalld:

```bash
sudo firewall-cmd --list-all
```

Check the application logs for additional errors.

---

## 19. Additional Useful systemd Commands

### List Failed Units

```bash
systemctl --failed
```

### List All Active Units

```bash
systemctl list-units --state=active
```

### List Service Unit Files

```bash
systemctl list-unit-files --type=service
```

### Display Unit Dependencies

```bash
systemctl list-dependencies nginx
```

### Display Service Properties

```bash
systemctl show nginx
```

### Verify a Unit File

```bash
sudo systemd-analyze verify /etc/systemd/system/myapp.service
```

### Analyze Boot Performance

```bash
systemd-analyze
```

### Identify Slow Starting Services

```bash
systemd-analyze blame
```

### Display Critical Boot Chain

```bash
systemd-analyze critical-chain
```

These commands are useful for investigating service configuration and boot performance.

---

## 20. systemd vs SysVinit

| Feature | systemd | SysVinit |
|---|---|---|
| Initialization | Modern system manager | Traditional init system |
| Main PID | PID 1 | PID 1 |
| Service management | systemctl | service / init scripts |
| Configuration | Unit files | Shell scripts |
| Dependencies | Explicit unit relationships | Primarily runlevel and script ordering |
| Logging integration | journald | Traditionally syslog-based |
| Startup | Supports parallel startup | Traditionally sequential runlevel scripts |

### Common SysVinit-Compatible Commands

```bash
sudo service nginx start
```

```bash
sudo service nginx stop
```

```bash
sudo service nginx restart
```

On systemd-based distributions, the `service` command may act as a compatibility interface to systemd. On older systems, it manages traditional init scripts.

---

## 21. Linux Services in DevOps

Services and systemd are important for managing production applications.

### Real-Time DevOps Use Cases

**1. Managing Web Servers**

Start Nginx:

```bash
sudo systemctl start nginx
```

Check status:

```bash
systemctl status nginx
```

**2. Managing Application Services**

After deploying an application, restart its service:

```bash
sudo systemctl restart myapp
```

**3. Automatic Service Startup**

Ensure an application starts after server reboot:

```bash
sudo systemctl enable myapp
```

**4. Monitoring Application Logs**

```bash
journalctl -u myapp -f
```

**5. Troubleshooting Production Failures**

```bash
systemctl --failed
```

Then investigate the relevant service:

```bash
journalctl -u myapp -n 100
```

**6. Automating Service Management**

DevOps engineers can use:

- Ansible.
- Shell scripts.
- CI/CD pipelines.
- Configuration management tools.
- Infrastructure automation.

### Example: Ansible Service Management

A simple Ansible task:

```yaml
- name: Ensure Nginx is running and enabled
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true
```

This ensures Nginx is running and configured to start at boot.

---

## 22. Best Practices for Managing Linux Services

1. Check service status after installation.
2. Enable required services to start automatically.
3. Avoid enabling unnecessary services.
4. Monitor service logs regularly.
5. Validate configuration before restarting production services.
6. Prefer reload over restart when supported and appropriate.
7. Use dedicated non-root service accounts when possible.
8. Avoid editing vendor-provided systemd unit files directly.
9. Use drop-in overrides for service customization.
10. Investigate failed services instead of repeatedly restarting them.
11. Use monitoring and alerting for important services.
12. Apply service changes carefully in production environments.

---

## 23. Important Commands Revision

| Command | Purpose |
|---|---|
| `systemctl status nginx` | Check service status |
| `systemctl start nginx` | Start service |
| `systemctl stop nginx` | Stop service |
| `systemctl restart nginx` | Restart service |
| `systemctl reload nginx` | Reload configuration |
| `systemctl enable nginx` | Enable startup at boot |
| `systemctl disable nginx` | Disable normal boot enablement |
| `systemctl enable --now nginx` | Enable and start |
| `systemctl disable --now nginx` | Disable and stop |
| `systemctl is-active nginx` | Check whether active |
| `systemctl is-enabled nginx` | Check whether enabled |
| `systemctl list-units --type=service` | List service units |
| `systemctl --failed` | List failed units |
| `systemctl daemon-reload` | Reload unit configurations |
| `systemctl cat nginx` | Display unit configuration |
| `systemctl mask nginx` | Mask a service |
| `systemctl unmask nginx` | Unmask a service |
| `journalctl -u nginx` | View service logs |
| `journalctl -f` | Follow logs |
| `journalctl -b` | View current boot logs |
| `systemd-analyze blame` | Identify slow boot units |

---

## 24. Practical Exercises

Complete these exercises using a Linux virtual machine or another Linux environment.

### Beginner Level

1. Check whether your Linux system uses systemd.
2. Display the systemd version.
3. Check the status of the SSH service.
4. List all active service units.
5. Find the default systemd target.
6. Check whether Nginx is installed.
7. Start the SSH service.
8. Check whether SSH is active.
9. Display all failed systemd units.
10. Check whether Nginx is enabled.

### Intermediate Level

11. Install Nginx and start the service.
12. Enable Nginx to start automatically during boot.
13. Restart Nginx.
14. Reload Nginx without a full restart.
15. Display Nginx service logs.
16. Follow service logs in real time.
17. Find the Nginx unit file location.
18. Display the Nginx unit configuration.
19. Explain the difference between `start` and `enable`.
20. Explain the difference between `restart` and `reload`.

### DevOps Scenario-Based Exercises

21. Your Nginx service fails to start. Identify the possible causes and troubleshoot it.

22. Your application service works manually but does not start after reboot. How will you fix it?

23. You modified a systemd service unit file. What command should you execute before restarting the service?

24. Your application keeps restarting every five seconds. How will you investigate the issue?

25. Your application is running, but users cannot access it through the browser. What will you check?

26. You need to create a custom systemd service for a Python application. Explain the steps.

27. You need to monitor application logs continuously. Which command will you use?

28. A service must not be started manually or automatically. Which systemd command will you use?

---

## 25. Linux Services and Systemd Interview Questions

### Basic Questions

**Q1. What is a Linux service?**

A Linux service is a program or system functionality that performs tasks in the background, usually without direct user interaction.

**Q2. What is a daemon?**

A daemon is a background process that provides services or performs system tasks.

**Q3. What is an init system?**

An init system is the first userspace process started by the Linux kernel. It initializes the system and manages service startup and shutdown.

**Q4. What is systemd?**

systemd is a system and service manager responsible for initializing the system, managing services, dependencies, and system states.

**Q5. What is systemctl?**

systemctl is the command-line utility used to manage systemd services and other units.

**Q6. What is PID 1?**

PID 1 is the first userspace process started by the kernel. On a typical systemd-based Linux distribution, it is systemd.

### Intermediate Questions

**Q7. What is the difference between start and enable?**

Start runs a service immediately, whereas enable configures the service for automatic startup during applicable boot targets.

**Q8. What is the difference between restart and reload?**

Restart stops and starts the service again. Reload asks the service to apply configuration changes without a complete restart, if supported.

**Q9. What is a systemd unit file?**

A unit file is a configuration file that tells systemd how to manage a service or another system resource.

**Q10. What is daemon-reload?**

`systemctl daemon-reload` makes systemd reread unit files and recognize configuration changes.

**Q11. What are systemd targets?**

Targets are units that group other units and represent particular system states, such as multi-user or graphical mode.

**Q12. What is journalctl?**

journalctl is a command-line tool used to view and analyze logs collected by systemd-journald.

**Q13. What is the difference between disable and mask?**

Disable removes normal boot enablement. Mask prevents the service from being started through systemd while it remains masked.

**Q14. What is a oneshot service?**

A oneshot service executes a task and then exits. It is useful for scripts and one-time administrative operations.

**Q15. Where are systemd service files stored?**

Common locations include:

```text
/etc/systemd/system/
/usr/lib/systemd/system/
/lib/systemd/system/
```

The exact vendor unit directory depends on the distribution.

### DevOps Scenario-Based Questions

**Q16. How do you check whether Nginx is running?**

```bash
systemctl status nginx
```

**Q17. How do you make Nginx start automatically after reboot?**

```bash
sudo systemctl enable nginx
```

**Q18. Your service is failing. How do you troubleshoot it?**

I first check its status using `systemctl status`, then inspect its logs using `journalctl -u`. I investigate configuration errors, permissions, dependencies, port conflicts, and application failures before restarting the service.

**Q19. How do you create a custom systemd service?**

I create an application script, create a service unit file under `/etc/systemd/system/`, configure the appropriate service directives, run `systemctl daemon-reload`, and then start and enable the service.

**Q20. Your application is working but is not accessible through the browser. What will you check?**

I check service status, application logs, listening ports, firewall rules, network connectivity, and application configuration.

**Q21. How do you monitor service logs continuously?**

```bash
sudo journalctl -u nginx -f
```

**Q22. How do you identify failed services on a Linux server?**

```bash
systemctl --failed
```

**Q23. What is the difference between systemd and SysVinit?**

systemd uses unit files and provides integrated service management, dependency handling, and journald logging. SysVinit uses traditional initialization scripts and runlevels.

**Q24. How can you manage services across multiple Linux servers?**

I can use Ansible or other configuration management tools to start, stop, restart, and enable services consistently across multiple servers.

---

## 26. Final Interview Preparation Checklist

Before completing this topic, make sure you can:

- Explain Linux services and daemons.
- Explain the role of an init system.
- Explain systemd and systemctl.
- Understand PID 1.
- Start, stop, restart, and reload services.
- Enable and disable services.
- Understand service states.
- Explain systemd unit files.
- Understand unit file sections.
- Explain systemd service types.
- Understand targets and runlevels.
- View and troubleshoot logs using journalctl.
- Explain daemon-reload.
- Create a custom systemd service.
- Understand masking and unmasking.
- Troubleshoot failed services.
- Explain how systemd is used in DevOps.

### Key Takeaway

Linux services and systemd are essential for managing applications and background processes.

A DevOps engineer should be comfortable managing services, configuring automatic startup, monitoring logs, troubleshooting failures, and automating service management across Linux servers.
