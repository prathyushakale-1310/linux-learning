
# Linux Filesystem Hierarchy (FHS)

## 1. Introduction

The Linux filesystem is organized in a hierarchical tree structure.

The Filesystem Hierarchy Standard (FHS) defines the common directory structure used by Linux distributions.

The root directory `/` is the starting point of the entire filesystem. All files and directories are organized under it.

### Important Definitions

- **Filesystem:** A method used by an operating system to organize and store files.
- **Directory:** A location used to organize files and other directories.
- **Root Directory (/):** The top-level directory of Linux.
- **Root User:** The administrator account with UID 0.
- **Mount Point:** A directory where another filesystem is attached.

---

## 2. Linux Filesystem Structure

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

---

## 3. Important Linux Directories

### 3.1 / - Root Directory

**Definition:**

The root directory is the top-level directory of the Linux filesystem. All other directories and files are organized under it.

**Examples:**

```bash
cd /
ls /
pwd
```

### 3.2 /home - User Home Directories

**Definition:**

Stores personal directories and files belonging to regular users.

**Example path:**

`/home/prathyusha`

**Commands:**

```bash
cd /home
ls
```

### 3.3 /root - Root User Home Directory

**Definition:**

The home directory of the root or administrator user.

**Important:**

`/root` and `/` are different directories.

**Command:**

```bash
sudo ls /root
```

### 3.4 /etc - Configuration Files

**Definition:**

Stores system-wide configuration files.

**Important files:**

- `/etc/passwd` - User account information.
- `/etc/shadow` - Password hashes and password aging information.
- `/etc/group` - Group information.
- `/etc/hostname` - System hostname.
- `/etc/hosts` - Local hostname-to-IP mappings.
- `/etc/fstab` - Filesystem mount configuration.
- `/etc/ssh/sshd_config` - SSH server configuration.

**Commands:**

```bash
ls /etc
cat /etc/hostname
cat /etc/hosts
```

### 3.5 /var - Variable Data

**Definition:**

Stores data that changes frequently while the system is running.

**Important subdirectories:**

- `/var/log` - System and application logs.
- `/var/cache` - Cached data.
- `/var/tmp` - Temporary files.
- `/var/lib` - Application and service state data.

**Commands:**

```bash
ls /var
ls /var/log
```

### 3.6 /tmp - Temporary Files

**Definition:**

Stores temporary files created by applications and users.

**Important:**

Files in `/tmp` may be deleted automatically during reboot or by system cleanup policies.

**Commands:**

```bash
cd /tmp
touch test.txt
ls
```

### 3.7 /bin - Essential User Commands

**Definition:**

Contains essential commands needed for basic system operation.

**Examples:**

- `ls`
- `cp`
- `mv`
- `cat`

**Command:**

```bash
ls /bin
```

**Note:** On many modern Linux distributions, `/bin` is a symbolic link to `/usr/bin`.

### 3.8 /sbin - System Administration Commands

**Definition:**

Contains system administration utilities.

**Examples:**

- `fsck`
- `reboot`
- `shutdown`

**Command:**

```bash
ls /sbin
```

### 3.9 /usr - User Programs and Shared Data

**Definition:**

Contains applications, libraries, documentation, and other shared resources.

**Important subdirectories:**

- `/usr/bin` - User commands and applications.
- `/usr/sbin` - System administration programs.
- `/usr/lib` - Libraries.
- `/usr/share` - Shared architecture-independent data.

**Commands:**

```bash
ls /usr
ls /usr/bin
```

### 3.10 /boot - Boot Files

**Definition:**

Contains files required to boot the Linux operating system.

**Examples:**

- Linux kernel files.
- Initial RAM filesystem (initramfs).
- Bootloader-related files.

**Command:**

```bash
ls /boot
```

### 3.11 /dev - Device Files

**Definition:**

Contains special files that represent hardware and virtual devices.

**Examples:**

- `/dev/sda` - Common name for a disk device.
- `/dev/null` - Discards data written to it.
- `/dev/zero` - Produces zero bytes.
- `/dev/tty` - Represents a terminal.

**Command:**

```bash
ls /dev
```

### 3.12 /proc - Process and Kernel Information

**Definition:**

A virtual filesystem that provides information about running processes and kernel details.

**Important files:**

- `/proc/cpuinfo` - CPU information.
- `/proc/meminfo` - Memory information.
- `/proc/uptime` - System uptime.
- `/proc/<PID>` - Information about a process.

**Commands:**

```bash
cat /proc/cpuinfo
cat /proc/meminfo
```

### 3.13 /sys - System and Hardware Information

**Definition:**

A virtual filesystem that exposes information about devices, drivers, and the Linux kernel.

**Commands:**

```bash
ls /sys
ls /sys/class
```

### 3.14 /mnt - Temporary Mount Point

**Definition:**

Used by administrators to temporarily mount filesystems.

**Example:**

```bash
sudo mount /dev/sdb1 /mnt
```

**Note:** This command works only when the device exists and the filesystem is suitable.

### 3.15 /media - Removable Media

**Definition:**

Commonly used as a mount location for removable devices such as USB drives and CDs.

**Command:**

```bash
ls /media
```

### 3.16 /opt - Optional Software

**Definition:**

Used for additional or third-party application software.

**Command:**

```bash
ls /opt
```

### 3.17 /run - Runtime Data

**Definition:**

Stores temporary runtime information about the system and running services. It is usually cleared during boot.

**Command:**

```bash
ls /run
```

### 3.18 /srv - Service Data

**Definition:**

Stores data served by system services, such as web or file services.

**Command:**

```bash
ls /srv
```

---

## 4. Important Linux Configuration and Log Files

| File | Purpose |
|---|---|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Password hashes and aging information |
pwd
```

Displays the current working directory.

### List Root Directory

```bash
ls /
```

Lists directories and files under the root directory.

### Display Detailed Directory Information

```bash
ls -l /
```

Displays permissions, ownership, size and modification time.

### Display Hidden Files

```bash
ls -a /
```

Displays hidden files and directories.

### Display Directory Structure

```bash
tree /
```

Displays directories in a tree format.

**Note:** Install the `tree` package if the command is unavailable.

### Find a File

```bash
find /etc -name "hostname"
```

Searches for a file named hostname inside `/etc`.

### Check Filesystem Disk Usage

```bash
df -h
```

Displays filesystem disk usage in human-readable format.

### Check Directory Size

```bash
du -sh /var/log
```

Displays the total size of `/var/log`.

### Display Mounted Filesystems

```bash
findmnt
```

Displays mounted filesystems and their mount points.

---

## 6. Absolute Path vs Relative Path

### Absolute Path

**Definition:**

An absolute path starts from the root directory `/`.

**Example:**

```bash
cd /home/prathyusha
```

### Relative Path

**Definition:**

A relative path starts from the current working directory.

**Example:**

```bash
cd Documents
```

### Special Directory Symbols

| Symbol | Meaning |
|---|---|
| `/` | Root directory |
| `.` | Current directory |

| `..` | Parent directory |

| `~` | Current user's home directory |

| `-` | Previous working directory |


**Examples:**


```bash
pwd
cd .
cd ..
cd ~

cd -
```

---

## 7. Practical DevOps Scenarios


### Scenario 1: Checking Application Logs

If an application is not working, check its logs.

```bash
cd /var/log
ls -l
```

For systemd-based systems:

```bash
journalctl -u nginx
```

### Scenario 2: Checking Server Disk Space

If a server runs out of disk space:

```bash
df -h
du -sh /var/log
```

### Scenario 3: Checking System Configuration

To check the system hostname:

```bash
cat /etc/hostname
```

### Scenario 4: Checking System Memory

```bash
free -h
cat /proc/meminfo
```

### Scenario 5: Checking Running Processes

```bash
ps aux
ls /proc
```

---

## 8. Linux Filesystem Hierarchy Interview Questions

### Q1. What is the Linux filesystem hierarchy?

The Linux filesystem hierarchy is a tree-like structure used to organize files and directories. It starts from the root directory `/`.

### Q2. What is the root directory?

The root directory `/` is the top-level directory of Linux. All other directories are organized under it.

### Q3. What is the difference between `/` and `/root`?

`/` is the root directory of the entire filesystem, whereas `/root` is the home directory of the root user.

### Q4. What is the purpose of `/etc`?

It stores system-wide configuration files.

### Q5. Where are Linux logs stored?

Many traditional Linux log files are stored in `/var/log`. Systemd-based systems also provide logs through `journalctl`.

### Q6. What is the purpose of `/tmp`?

It stores temporary files created by users and applications.

### Q7. What is `/proc`?

It is a virtual filesystem that provides information about running processes and kernel details.

### Q8. What is `/dev`?

It contains special device files that represent hardware and virtual devices.

### Q9. What is the difference between `/home` and `/root`?

`/home` contains home directories for regular users, while `/root` is the home directory of the root administrator.

### Q10. What is the purpose of `/boot`?

It stores files required to boot the Linux system, including kernel and bootloader-related files.

### Q11. What is the difference between `/mnt` and `/media`?

`/mnt` is commonly used for manually mounted filesystems, while `/media` is commonly used for removable media.

### Q12. What is the purpose of `/usr`?

It contains user applications, commands, libraries and shared data.

### Q13. What is a mount point?

A mount point is a directory where a filesystem is attached so its contents can be accessed.

### Q14. What is the difference between an absolute and relative path?

An absolute path starts from `/`, while a relative path starts from the current working directory.

### Q15. Where can you find CPU and memory information?

CPU information is available in `/proc/cpuinfo`, and memory information is available in `/proc/meminfo`.

---

## 9. Practice Exercises

Try these commands yourself:

1. Display the root directory contents.
2. Navigate to `/etc`.
3. Display the hostname of your system.
4. Find the `hosts` file inside `/etc`.
5. Display CPU information.
6. Display memory information.
7. Check disk usage using `df -h`.
8. Check the size of `/var/log`.
9. Navigate to your home directory.
10. Display the current working directory.
11. Display mounted filesystems using `findmnt`.
12. Find the SSH server configuration file.
13. Display the contents of `/usr/bin`.
14. Check the current system uptime.
15. Explain the difference between `/tmp` and `/var/tmp`.

---

## 10. Quick Revision Notes

| Directory | Purpose |
|---|---|
| `/` | Root directory |
| `/home` | Regular user home directories |
| `/root` | Root user's home directory |
| `/etc` | Configuration files |
| `/var` | Variable data and logs |
| `/tmp` | Temporary files |
| `/bin` | Essential user commands |
| `/sbin` | System administration commands |
| `/usr` | Applications and shared resources |
| `/boot` | Boot files |
| `/dev` | Device files |
| `/proc` | Process and kernel information |
| `/sys` | Kernel and hardware information |
| `/mnt` | Temporary mount point |
| `/media` | Removable media |
| `/opt` | Optional software |
| `/run` | Runtime data |
| `/srv` | Service data |

---

## 11. Key Takeaways

- Linux follows a hierarchical filesystem structure.
- The root directory `/` is the starting point.
- `/etc` contains system configuration files.
- `/var/log` commonly contains system and application logs.
- `/home` stores regular user data.
- `/root` is the home directory of the root user.
- `/proc` provides process and kernel information.
- `/dev` contains device files.
- `/boot` stores boot-related files.
- `df -h` checks filesystem disk usage.
- `du -sh` checks directory size.
- `findmnt` displays mounted filesystems.

---

**End of File 3: Linux Filesystem Hierarchy**

