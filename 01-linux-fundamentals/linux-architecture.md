
# Linux Architecture

## 1. Introduction to Linux Architecture

Linux architecture describes how the different components of a Linux operating system work together to manage hardware, run applications, and provide services to users.

Linux follows a layered architecture. Each layer has a specific responsibility and communicates with other layers.

The major components are:

1. Hardware
2. Kernel
3. System Call Interface
4. System Libraries
5. Shell
6. System Utilities and Applications
7. Users

## 2. Linux Architecture Diagram

```text
+--------------------------------------+
|                Users                 |
+--------------------------------------+
                    |
+--------------------------------------+
|       Applications and Utilities     |
|  Editors | Browsers | Commands       |
+--------------------------------------+
                    |
+--------------------------------------+
|                 Shell                |
|       Bash | Zsh | Sh                 |
+--------------------------------------+
                    |
+--------------------------------------+
|            System Libraries          |
|       glibc and other libraries      |
+--------------------------------------+
                    |
+--------------------------------------+
|         System Call Interface        |
+--------------------------------------+
                    |
+--------------------------------------+
|                Kernel                |
| Process | Memory | Device | Network  |
| File System | Security Management    |
+--------------------------------------+
                    |
+--------------------------------------+
|                Hardware              |
| CPU | RAM | Disk | Network Devices   |
+--------------------------------------+
```

## 3. Hardware

### Definition

Hardware refers to the physical components of a computer system that can be seen and touched.

The Linux operating system interacts with hardware through the kernel.

### Examples

- CPU: Executes instructions and processes data.
- RAM: Temporarily stores data and programs currently in use.
- Hard Disk / SSD: Stores files, applications, and operating system data.
- Network Interface Card (NIC): Enables network communication.
- Keyboard and Mouse: Input devices.

### Example

When you execute a Linux command, the CPU processes the instructions, RAM holds the required data, and the storage device may provide the program or files.

### DevOps relevance

A DevOps engineer monitors CPU, memory, disk, and network utilization to identify server performance problems.

Useful commands:

```bash
lscpu
free -h
lsblk
df -h
ip addr
```

## 4. Kernel

### Definition

The kernel is the core component of the Linux operating system. It acts as an interface between software and hardware and manages system resources.

It is responsible for controlling processes, memory, devices, filesystems, and networking.

### Major responsibilities of the kernel

1. Process Management
2. Memory Management
3. Device Management
4. File System Management
5. Network Management
6. Security and Access Control

### 4.1 Process Management

The kernel manages running processes and allocates CPU time to them.

It handles:

- Process creation
- Process scheduling
- Process termination
- Process communication

Example:

```bash
ps aux
top
```

### 4.2 Memory Management

The kernel manages RAM and virtual memory.

It handles:

- Memory allocation
- Memory deallocation
- Virtual memory
- Memory protection

Example:

```bash
free -h
```

### 4.3 Device Management

The kernel communicates with hardware devices through device drivers.

Examples include:

- Disk devices
- USB devices
- Network cards
- Printers

Useful commands:

```bash
lsblk
lspci
lsusb
```

### 4.4 File System Management

The kernel provides access to files and directories through supported filesystems.

Examples:

- ext4
- XFS
- Btrfs

Useful command:

```bash
df -Th
```

### 4.5 Network Management

The kernel manages network communication, network interfaces, and protocol operations.

Useful commands:

```bash
ip addr
ip route
ss -tuln
```

### 4.6 Security Management

The kernel enforces security mechanisms such as permissions, user identities, and process isolation.

Example:

```bash
ls -l
id
```

## 5. System Call Interface

### Definition

A system call is a mechanism through which a user-space program requests a service from the kernel.

Applications cannot directly perform privileged kernel operations. They use system calls to request the required services.

### Examples of system calls

| System call | Purpose |
|---|---|
| `open()` | Opens a file |
| `read()` | Reads data |
| `write()` | Writes data |
| `close()` | Closes a file descriptor |
| `fork()` | Creates a child process |
| `execve()` | Executes a program |
| `wait()` | Waits for a child process |
| `getpid()` | Returns the process ID |

### Example

When a program reads a file, it requests the kernel to access the file using system calls such as `open()` and `read()`.

### Important point

System calls provide a controlled boundary between user space and kernel space.

## 6. System Libraries

### Definition

System libraries are collections of reusable functions that applications use to perform common operations and access operating system services.

They provide convenient interfaces to functionality that may involve system calls.

### Examples

- glibc (GNU C Library)
- libpthread functionality
- libm (mathematical functions)

### Example

A program written in C may use a library function to open or read a file. The library can invoke the appropriate system call.

### Difference between System Calls and System Libraries

| System Calls | System Libraries |
|---|---|
| Request services from the kernel | Provide reusable functions for applications |
| Form the user-kernel interface | Often provide higher-level interfaces |
| Example: `read()` | Example: C library functions |

## 7. Shell

### Definition

A shell is a command-line interpreter that accepts commands from users, interprets them, and starts the requested programs.

It provides an interface between the user and the operating system.

### Popular Linux shells

| Shell | Description |
|---|---|
| Bash | Bourne Again Shell; widely used |
| Sh | Traditional Unix shell interface |
| Zsh | Interactive shell with customization features |
| Fish | User-friendly interactive shell |
| Ksh | KornShell |

### Common shell commands

```bash
pwd
ls
cd
mkdir
touch
cat
```

### Check the current shell

```bash
echo $SHELL
```

### Check available shells

```bash
cat /etc/shells
```

### Important point

The shell is not the kernel. The shell accepts user commands, while the kernel manages system resources.

## 8. System Utilities and Applications

### Definition

System utilities are programs that help users and administrators perform operating system tasks.

Applications are programs used to perform specific activities.

### Examples of system utilities

| Utility | Purpose |
|---|---|
| `ls` | Lists directory contents |
| `cp` | Copies files |
| `mv` | Moves or renames files |
| `grep` | Searches text |
| `find` | Searches for files |
| `ps` | Displays process information |
| `systemctl` | Manages systemd services |
| `df` | Displays filesystem disk usage |

### Examples of applications

- Web browsers
- Text editors
- Database servers
- Web servers
- Monitoring tools

## 9. User Space and Kernel Space

Linux separates execution into two important areas.

### 9.1 User Space

User space is the area where normal applications and user programs execute.

Examples:

- Shell
- Text editors
- Web browsers
- Application processes
- System utilities

Applications generally have restricted access to hardware and kernel memory.

### 9.2 Kernel Space

Kernel space is the privileged area where the kernel executes.

It manages:

- CPU scheduling
- Memory
- Device drivers
- Filesystems
- Networking

### Difference between User Space and Kernel Space

| User Space | Kernel Space |
|---|---|
| Runs normal applications | Runs the kernel |
| Has restricted privileges | Has privileged access |
| Applications execute here | Core operating system operations execute here |
| Application failure is generally isolated | Kernel failure can affect the entire system |

## 10. How a Linux Command Works

Consider the following command:

```bash
ls -l /home
```

Execution flow:

1. The user enters the command in the terminal.
2. The shell interprets the command.
3. The shell locates and starts the `ls` program.
4. The program requests filesystem information through operating system interfaces.
5. The kernel accesses the required filesystem data.
6. The results are returned to the program.
7. The program displays the output in the terminal.

Important: The shell does not itself perform every operation. It starts programs, and those programs interact with the operating system.

## 11. Linux Architecture and DevOps

Understanding Linux architecture helps DevOps engineers troubleshoot servers and applications.

### Scenario 1: High CPU utilization

Problem: A server is running slowly because CPU usage is high.

Useful commands:

```bash
top
htop
ps aux --sort=-%cpu | head
```

### Scenario 2: High memory utilization

Problem: An application is consuming too much memory.

Useful commands:

```bash
free -h
top
ps aux --sort=-%mem | head
```

### Scenario 3: Disk space is full

Problem: A server cannot write new files because disk space is exhausted.

Useful commands:

```bash
df -h
du -sh /var/log
```

### Scenario 4: Network connectivity problem

Problem: An application cannot communicate with another server.

Useful commands:

```bash
ip addr
ip route
ping -c 4 8.8.8.8
ss -tuln
```

### Scenario 5: Service is not running

Problem: An application service is unavailable.

Useful commands:

```bash
systemctl status nginx
systemctl restart nginx
journalctl -u nginx
```

## 12. Important Linux Architecture Terms

| Term | Meaning |
|---|---|
| Kernel | Core of the operating system |
| Shell | Command interpreter |
| Hardware | Physical computer components |
| System Call | Request made by a program to the kernel |
| System Library | Reusable functions for applications |
| User Space | Area where normal applications execute |
| Kernel Space | Privileged area where the kernel executes |
| Process | A running instance of a program |
| Device Driver | Software that enables the kernel to communicate with hardware |
| Filesystem | Method of organizing and storing files |

## 13. Interview Questions and Answers

### Q1. What is Linux architecture?

Linux architecture is the layered structure of the Linux operating system. It includes hardware, the kernel, system call interface, system libraries, shell, utilities, and applications.

### Q2. What is a kernel?

The kernel is the core of Linux. It manages hardware resources, processes, memory, devices, filesystems, and networking.

### Q3. What are the main responsibilities of the kernel?

The kernel manages process scheduling, memory allocation, device communication, filesystems, networking, and security.

### Q4. What is a shell?

A shell is a command-line interpreter that accepts user commands and starts the requested programs.

### Q5. What is the difference between a shell and a kernel?

The shell interprets user commands and launches programs. The kernel manages hardware and system resources.

### Q6. What is a system call?

A system call is a mechanism through which a user-space program requests a service from the kernel.

### Q7. What are system libraries?

System libraries provide reusable functions that applications use to access operating system functionality.

### Q8. What is user space?

User space is the memory and execution area where normal applications and user programs run with restricted privileges.

### Q9. What is kernel space?

Kernel space is the privileged execution area where the Linux kernel performs core operating system operations.

### Q10. What is the difference between user space and kernel space?

User space runs normal applications with restricted privileges, while kernel space runs the kernel with privileged access to system resources.

### Q11. What is a device driver?

A device driver is software that allows the operating system kernel to communicate with a hardware device.

### Q12. What happens when you execute a Linux command?

The shell interprets the command and starts the required program. The program interacts with the kernel when it needs operating system services, and the output is displayed in the terminal.

### Q13. What is Bash?

Bash stands for Bourne Again Shell. It is a popular command-line interpreter used in Linux.

### Q14. What is process management?

Process management is the kernel's responsibility for creating, scheduling, controlling, and terminating processes.

### Q15. Why is Linux architecture important for DevOps?

It helps DevOps engineers understand server resources, troubleshoot CPU and memory problems, manage services, investigate networking issues, and diagnose application failures.

## 14. Practical Exercises

1. Display your Linux kernel version.

```bash
uname -r
```

2. Display detailed system information.

```bash
uname -a
```

3. Identify your current shell.

```bash
echo $SHELL
```

4. Display CPU information.

```bash
lscpu
```

5. Display memory usage.

```bash
free -h
```

6. Display block storage devices.

```bash
lsblk
```

7. Display filesystem disk usage.

```bash
df -h
```

8. Display current running processes.

```bash
ps aux
```

9. Display network interfaces.

```bash
ip addr
```

10. Display active listening ports.

```bash
ss -tuln
```

## 15. Quick Revision

- Linux architecture explains how Linux components work together.
- Hardware contains physical computer components.
- Kernel is the core of the Linux operating system.
- System calls allow programs to request kernel services.
- System libraries provide reusable functions.
- Shell interprets commands and launches programs.
- System utilities help users perform operating system tasks.
- User space runs normal applications.
- Kernel space runs privileged kernel operations.
- Device drivers allow the kernel to communicate with hardware.

## 16. Final Takeaway

Linux architecture is important for understanding how commands, applications, hardware, and system resources interact.

For a DevOps engineer, understanding the kernel, shell, processes, memory, filesystems, and networking makes Linux server administration and troubleshooting easier.
