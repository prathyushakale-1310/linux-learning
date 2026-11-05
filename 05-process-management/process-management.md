# Linux Process Management

## 1. Introduction to Process Management

Linux is a multitasking operating system. It can run multiple programs and tasks at the same time.

Whenever a program starts executing, the operating system creates a process to manage its execution.

**Definition:**

A process is an instance of a running program. It has its own Process ID (PID), memory space, execution state, and other resources managed by the Linux kernel.

### Why is process management important?

- Monitor running applications.
- Identify applications consuming high CPU or memory.
- Stop unwanted or unresponsive processes.
- Manage background tasks.
- Troubleshoot application failures.
- Control application resource usage.

**DevOps example:**

Suppose an application running on a Linux server is consuming 100% CPU. A DevOps engineer needs to identify the process, investigate the issue, and take appropriate action.

Linux process-management commands help perform these tasks.

---

## 2. Difference Between Program and Process

### Program

A program is a set of instructions stored in a file on disk.

Example:

```bash
/usr/bin/python3
```

### Process

A process is a running instance of a program.

For example, when you execute:

```bash
python3 app.py
```

Linux starts a process to execute the Python program.

| Program | Process |
|---|---|
| Set of instructions stored on disk | Running instance of a program |
| Passive | Active |
| Does not need a PID | Has a PID |
| Example: `app.py` | Running `python3 app.py` |

**Interview point:** One program can have multiple running processes.

---

## 3. Understanding Process ID (PID)

PID stands for **Process ID**.

**Definition:**

A PID is a unique numerical identifier assigned to a process by the operating system while that process exists.

Example:

```bash
ps
```

Sample output:

```text
PID TTY          TIME CMD
1250 pts/0    00:00:00 bash
1280 pts/0    00:00:00 ps
```

Here:
- `1250` is the PID of the Bash process.
- `1280` is the PID of the `ps` command.

### Find the current shell PID

```bash
echo $$
```

This displays the PID of the current shell.

### Find the PID of a process by name

```bash
pgrep nginx
```

This displays matching process IDs for `nginx`, if any are running.

### Find the PID of a running command

```bash
pidof nginx
```

This can display the PIDs associated with the specified program name.

---

## 4. Understanding PPID – Parent Process ID

PPID stands for **Parent Process ID**.

**Definition:**

PPID is the process ID of the process that created another process.

A process can create another process. The original process is called the parent, and the newly created process is called the child.

Example:

```bash
ps -ef
```

Sample output:

```text
UID      PID   PPID  C STIME TTY          TIME CMD
root       1      0  0 Nov04 ?        00:00:02 systemd
user    1250      1  0 Nov05 pts/0    00:00:00 bash
user    1280   1250  0 Nov05 pts/0    00:00:00 sleep 100
```

In this example:
- PID `1250` is the parent of PID `1280`.
- PID `1280` is the child process.
- PPID `1250` identifies the parent.

**Important:** Process ID `1` is generally the system's initial process, commonly `systemd` on modern Linux distributions. The exact process hierarchy depends on the system.

---

## 5. Types of Processes

Linux processes can be understood in different ways based on how they execute.

### 5.1 Foreground Process

**Definition:**

A foreground process runs attached to the terminal and normally receives keyboard input from that terminal.

Example:

```bash
sleep 30
```

The terminal waits while the command runs.

You can usually stop a foreground command by pressing:

```text
Ctrl + C
```

This sends an interrupt signal, usually SIGINT, to the foreground process group.

### 5.2 Background Process

**Definition:**

A background process runs without keeping the terminal occupied by that command.

To start a command in the background, add `&` at the end.

Example:

```bash
sleep 100 &
```

The shell starts the command in the background and normally displays its PID and job number.

Example output:

```text
[1] 1450
```

Here:
- `1` is the job number.
- `1450` is the process ID.

You can continue using the terminal while the command runs.

### 5.3 Daemon Process

**Definition:**

A daemon is a background service process that performs tasks without direct interaction with a user terminal.

Examples:

- `sshd` – Provides SSH access.
- `cron` or `crond` – Runs scheduled tasks.
- `systemd` – System and service manager.

Check a daemon:

```bash
ps -ef | grep sshd
```

**DevOps example:**

The SSH daemon allows administrators to connect to remote Linux servers.

### 5.4 Orphan Process

**Definition:**

An orphan process is a process whose parent process has terminated while the child process is still running.

On Linux, orphan processes are normally adopted by PID 1 or an appropriate subreaper.

### 5.5 Zombie Process

**Definition:**

A zombie process is a process that has finished execution but still has an entry in the process table because its parent has not yet collected its exit status.

A zombie process:
- Has completed execution.
- Does not continue running application code.
- Retains a process-table entry.
- Usually appears with state `Z`.

Check for zombies:

```bash
ps aux | grep ' Z'
```

A more reliable state-focused check is:

```bash
ps -eo pid,ppid,state,cmd | awk '$3 == "Z"'
```

**Important:** A zombie cannot be fixed by killing it because it has already exited. The parent process normally needs to collect its exit status. Investigate or restart the parent only when appropriate.

---

## 6. Checking Running Processes Using ps

`ps` stands for **Process Status**.

**Definition:**

The `ps` command displays information about running processes.

### 6.1 Basic command

```bash
ps
```

Displays processes associated with the current terminal.

### 6.2 Display all processes

```bash
ps -ef
```

Displays a detailed list of processes on the system.

Important columns:

| Column | Meaning |
|---|---|
| UID | User who owns the process |
| PID | Process ID |
| PPID | Parent Process ID |
| C | CPU utilization information |
| STIME | Start time |
| TTY | Controlling terminal |
| TIME | CPU time used |
| CMD | Command being executed |

### 6.3 Display processes in BSD-style format

```bash
ps aux
```

This displays a detailed process list, including processes without a controlling terminal.

Important columns:

| Column | Meaning |
|---|---|
| USER | Process owner |
| PID | Process ID |
| %CPU | CPU usage |
| %MEM | Memory usage |
| VSZ | Virtual memory size |
| RSS | Resident memory size |
| STAT | Process state |
| COMMAND | Executed command |

### 6.4 Find a specific process

```bash
ps -ef | grep nginx
```

This searches the process listing for `nginx`.

Note: `grep` may also match its own command. For a cleaner search, use:

```bash
pgrep -a nginx
```

### 6.5 Display process tree

```bash
ps -ef --forest
```

This displays processes in a tree-like hierarchy on systems supporting this option.

Another useful command:

```bash
pstree
```

It displays the process hierarchy. The `pstree` utility may need to be installed.

---

## 7. Monitoring Processes Using top

`top` is a real-time process monitoring command.

**Definition:**

The `top` command displays running processes and continuously updates system resource usage, including CPU and memory.

Command:

```bash
top
```

It displays information such as:
- System uptime.
- Number of users.
- Load average.
- CPU usage.
- Memory usage.
- Swap usage.
- Running processes.
- Process IDs.
- CPU and memory consumption by process.

### Useful keys inside top

| Key | Purpose |
|---|---|
| `q` | Quit top |
| `P` | Sort by CPU usage |
| `M` | Sort by memory usage |
| `k` | Send a signal to a process |
| `1` | Show CPU usage per CPU |
| `h` | Display help |

**DevOps example:**

If a Linux server becomes slow, run:

```bash
top
```

Check whether any process is consuming unusually high CPU or memory.

---

## 8. htop – Interactive Process Monitoring

`htop` is an interactive process monitoring tool with a user-friendly interface.

Command:

```bash
htop
```

Features:
- Displays CPU and memory usage.
- Provides an interactive process list.
- Allows process searching and filtering.
- Supports sorting processes.
- Allows users to send signals to processes.

**Note:** `htop` may not be installed by default.

Install on Ubuntu or Debian:

```bash
sudo apt update
sudo apt install htop
```

Install on RHEL-based systems:

```bash
sudo dnf install htop
```

### Difference between top and htop

| top | htop |
|---|---|
| Usually available by default | May need installation |
| Traditional terminal interface | More user-friendly interface |
| Keyboard-based interaction | Interactive navigation and process management |
| Displays process information | Displays process information with additional visual features |

---

## 9. Managing Background Jobs

Linux shells allow commands to run as background jobs.

Important commands:

- `jobs`
- `bg`
- `fg`

### 9.1 jobs

**Definition:**

The `jobs` command displays background and stopped jobs associated with the current shell.

Example:

```bash
sleep 200 &
jobs
```

Sample output:

```text
[1]+ Running sleep 200 &
```

### 9.2 bg

**Definition:**

The `bg` command resumes a stopped job in the background.

Example:

Start a foreground command:

```bash
sleep 200
```

Press:

```text
Ctrl + Z
```

This suspends the foreground job.

Now run:

```bash
bg
```

The suspended job continues running in the background.

### 9.3 fg

**Definition:**

The `fg` command brings a background or stopped job to the foreground.

Example:

```bash
fg
```

To bring a specific job to the foreground:

```bash
fg %1
```

Here `%1` represents job number 1.

### Difference between jobs, bg, and fg

| Command | Purpose |
|---|---|
| `jobs` | Lists shell jobs |
| `bg` | Resumes a stopped job in the background |
| `fg` | Brings a job to the foreground |

**Important:** Shell job numbers are not the same as process IDs. The commands operate on jobs known to the current shell.

---

## 10. Managing Processes Using kill

`kill` is used to send signals to processes.

**Definition:**

The `kill` command sends a specified signal to a process identified by its PID.

Despite its name, `kill` does not always terminate a process. It can send different signals.

### Syntax

```bash
kill PID
```

By default, `kill` sends SIGTERM (signal 15).

### 10.1 SIGTERM – Signal 15

SIGTERM requests a process to terminate gracefully.

Example:

```bash
kill 1450
```

The process can handle this signal and perform cleanup before exiting.

### 10.2 SIGKILL – Signal 9

SIGKILL forcibly terminates a process.

Example:

```bash
kill -9 1450
```

Important:
- The process cannot catch or ignore SIGKILL.
- It cannot perform its own graceful cleanup.
- Use it only when normal termination fails and it is safe to do so.

### 10.3 SIGINT – Signal 2

SIGINT is an interrupt signal, commonly sent when pressing `Ctrl + C` in a terminal.

Example:

```bash
kill -2 1450
```

### 10.4 List available signals

```bash
kill -l
```

### Common Linux signals

| Signal | Number | Purpose |
|---|---:|---|
| SIGHUP | 1 | Hangup; often used by applications to reload configuration |
| SIGINT | 2 | Interrupt |
| SIGQUIT | 3 | Quit and may produce a core dump |
### 11.1 killall


**Definition:**

`killall` sends a signal to processes matching a specified process name.

Example:

```bash
killall nginx

```


This sends the default termination signal to matching processes named `nginx`.


To force termination:


```bash

killall -9 nginx
```


Be careful: it may affect multiple matching processes.


### 11.2 pkill


**Definition:**


`pkill` sends a signal to processes selected by their name or other matching criteria.


Example:


```bash
pkill nginx
```

Send SIGTERM to matching processes.

Send SIGKILL:

```bash
pkill -9 nginx
```

Match a full command line instead of only the process name:

```bash
pkill -f "python3 app.py"
```

**Important:** Always verify which processes match before terminating them, especially on production servers.

### Difference between kill, killall, and pkill


| Command | Purpose |

|---|---|

| `kill PID` | Sends a signal to a process by PID |
| `killall name` | Sends a signal to matching process names |

| `pkill name` | Sends a signal to processes matching a pattern |


---

## 12. Process States in Linux

A process can exist in different states during its lifecycle.

Common process states:

| State | Meaning |
|---|---|
| R | Running or runnable |
| S | Interruptible sleep |
| D | Uninterruptible sleep, often waiting for I/O |
| T | Stopped or traced |
| Z | Zombie |
| I | Idle kernel thread |


### Check process states


```bash

ps -eo pid,ppid,state,cmd

```


Or:


```bash
ps aux
```

Look at the `STAT` column.

### 12.1 Running or Runnable (R)

The process is currently executing or ready to execute on the CPU.

### 12.2 Sleeping (S)

The process is waiting for an event, such as input or a timer. It can usually be interrupted by signals.

### 12.3 Uninterruptible Sleep (D)

The process is waiting for certain kernel operations, commonly I/O.

A process in this state may not respond immediately to termination signals.

If a process remains in D state, investigate the underlying I/O or storage issue.

### 12.4 Stopped (T)

The process has been stopped, for example by SIGSTOP or a job-control action.

### 12.5 Zombie (Z)

The process has finished but its parent has not collected its exit status.

**Interview point:** A zombie process is not actively executing.

---

## 13. nice and renice – Process Priority

Linux uses scheduling priorities to decide how processes receive CPU time.

`nice` and `renice` help influence the scheduling priority of normal processes.

### 13.1 nice

**Definition:**

The `nice` command starts a process with a specified nice value.

Nice values normally range from:

```text
-20 to 19
```

| Nice value | Meaning |
|---|---|
| -20 | Highest scheduling priority for normal nice values |
| 0 | Default nice value |
| 19 | Lowest scheduling priority for normal nice values |

Example:

```bash
nice -n 10 sleep 100
```

This starts the process with nice value 10.

Check:

```bash
ps -o pid,ni,pri,cmd -C sleep
```

### 13.2 renice

**Definition:**

The `renice` command changes the nice value of an already running process.

Syntax:

```bash
renice value -p PID
```

Example:

```bash
renice 10 -p 1450
```

This changes the nice value of PID 1450 to 10.

Changing a process to a more favorable priority, such as a negative nice value, generally requires elevated privileges.

**Important:** Nice values influence CPU scheduling for normal processes. They do not guarantee a specific amount of CPU time.

### Difference between nice and renice

| nice | renice |
|---|---|
| Starts a new process with a selected nice value | Changes the nice value of an existing process |
| Used before or during process startup | Used after the process has started |

---

## 14. Understanding /proc in Linux

`/proc` is a virtual filesystem provided by the Linux kernel.

It exposes information about running processes and system resources.

Each running process usually has a directory named after its PID.

Example:

```bash
ls /proc
```

Check a process:

```bash
ls /proc/1450
```

Some important files:

| File | Information |
|---|---|
| `/proc/PID/status` | Process status and details |
| `/proc/PID/cmdline` | Command-line arguments |
| `/proc/PID/cwd` | Symbolic link to process working directory |
| `/proc/PID/exe` | Symbolic link to executed program |
| `/proc/cpuinfo` | CPU information |
| `/proc/meminfo` | Memory information |

Example:

```bash
cat /proc/1450/status
```

**DevOps use:** The `/proc` filesystem is useful for troubleshooting process behavior and inspecting resource information.

---

## 15. Process Monitoring and Troubleshooting Commands

### Find processes consuming CPU

```bash
ps aux --sort=-%cpu | head
```

Displays processes sorted by CPU usage, highest first.

### Find processes consuming memory

```bash
ps aux --sort=-%mem | head
```

Displays processes sorted by memory usage, highest first.

### Find a process by name

```bash
pgrep -a nginx
```

### Display a process tree

```bash
pstree -p
```

### Display process details

```bash
ps -p PID -o pid,ppid,user,stat,%cpu,%mem,cmd
```

Replace `PID` with the actual process ID.

### Monitor a process continuously

```bash
top -p 1450
```

This monitors PID 1450.

### Display open files used by a process

```bash
sudo lsof -p 1450
```

The `lsof` utility may need to be installed.

### Find processes listening on ports

```bash
sudo ss -lntup
```

This displays listening TCP and UDP sockets and, where permissions allow, their associated processes.

---

## 16. Practical DevOps Scenarios

### Scenario 1: Application is consuming high CPU

Problem:

A Linux server is slow because an application is consuming too much CPU.

Step 1: Check running processes.

```bash
top
```

Step 2: Find processes sorted by CPU usage.

```bash
ps aux --sort=-%cpu | head
```

Step 3: Identify the process and investigate its behavior.

Step 4: If the process needs to be stopped, try graceful termination:

```bash
kill PID
```

Step 5: Verify whether it has stopped.

```bash
ps -p PID
```

**Explanation:** First identify the process and understand why it is consuming CPU. Do not terminate critical production processes without checking their impact.

### Scenario 2: Application is consuming high memory

Step 1:

```bash
top
```

Step 2:

```bash
ps aux --sort=-%mem | head
```

Step 3: Identify the process consuming excessive memory.

Step 4: Check application logs and investigate possible memory leaks or abnormal workload.

**Explanation:** High memory usage may be expected for some applications. Check the application requirements before taking action.

### Scenario 3: Application is not responding

Step 1: Find the application PID.

```bash
pgrep -a python
```

Step 2: Check the process.

```bash
ps -p PID -o pid,stat,cmd
```

Step 3: Try graceful termination if appropriate.

```bash
kill PID
```

Step 4: If the process does not terminate and it is safe to force termination:

```bash
kill -9 PID
```

**Explanation:** SIGTERM gives the application an opportunity to clean up. SIGKILL should be a last resort.

### Scenario 4: Run a script in the background

Suppose a long-running script should not keep the terminal occupied.

```bash
python3 backup.py &
```

Check shell jobs:

```bash
jobs
```

Find the process:

```bash
pgrep -af backup.py
```

**Important:** A background job is still associated with its shell session. It may receive a hangup signal when the terminal closes. For longer jobs, consider `nohup`, `tmux`, `screen`, or a proper systemd service.

### Scenario 5: Run a command after logout

Use:

```bash
nohup python3 backup.py > backup.log 2>&1 &
```

Explanation:

- `nohup` makes the command ignore the hangup signal.
- `>` redirects standard output to `backup.log`.
- `2>&1` redirects standard error to the same file.
- `&` starts the command in the background.

Check:

```bash
pgrep -af backup.py
```

Read the output:

```bash
tail -f backup.log
```

**Note:** `nohup` does not provide service supervision or automatic restart. For production applications, a systemd service or another process supervisor is usually more suitable.

### Scenario 6: Too many processes are running

Check the process count:

```bash
ps -e --no-headers | wc -l
```

Inspect processes:

```bash
ps -ef --forest
```

Check CPU and memory:

```bash
top
```

**Explanation:** Investigate which applications are creating the processes. Do not randomly kill processes.

### Scenario 7: Find a zombie process

Run:

```bash
ps -eo pid,ppid,state,cmd | awk '$3 == "Z"'
```

Identify its parent process using the PPID column.

Inspect the parent:

```bash
ps -p PPID -o pid,ppid,stat,cmd
```

**Explanation:** A zombie has already exited. Investigate the parent process, which is responsible for collecting the child's exit status.

---

## 17. Hands-on Practice Exercises

Perform these exercises in your Linux terminal.

### Exercise 1: Display running processes

```bash
ps
```

Question: What information does the command display?

### Exercise 2: Display all processes

```bash
ps -ef
```

Question: What is the difference between `ps` and `ps -ef`?

### Exercise 3: Start a background process

```bash
sleep 100 &
```

Question: What is the PID displayed by the terminal?

### Exercise 4: List background jobs

```bash
jobs
```

Question: What does the job number represent?

### Exercise 5: Stop a foreground process

```bash
sleep 200
```

Press `Ctrl + C`.

Question: Which signal is commonly sent by `Ctrl + C`?

### Exercise 6: Suspend and resume a process

Start:

```bash
sleep 200
```

Press `Ctrl + Z`.

Then run:

```bash
jobs
bg
```

Question: What is the difference between `Ctrl + Z` and `Ctrl + C`?

### Exercise 7: Bring a job to the foreground

```bash
fg
```

Question: What happens when you execute this command?

### Exercise 8: Monitor CPU and memory

```bash
top
```

Question: Which keys sort processes by CPU and memory usage?

### Exercise 9: Find a process by name

```bash
pgrep -a sleep
```

Question: What information does this command display?

### Exercise 10: Terminate a process

Start:

```bash
sleep 300 &
```

Find its PID and terminate it gracefully:

```bash
kill PID
```

Verify:

```bash
ps -p PID
```

Question: Which signal does `kill PID` send by default?

### Exercise 11: Check process hierarchy

```bash
ps -ef --forest
```

Question: What is the relationship between parent and child processes?

### Exercise 12: Check process information

```bash
ps -eo pid,ppid,state,cmd
```

Question: What does the state `Z` mean?

---

## 18. Linux Process Management – Interview Questions and Answers

### Q1. What is a process in Linux?

A process is an instance of a running program. It has a PID and uses system resources such as CPU and memory.

### Q2. What is the difference between a program and a process?

A program is a set of instructions stored on disk, whereas a process is an executing instance of that program.

### Q3. What is PID?

PID stands for Process ID. It is the numerical identifier assigned to a process.

### Q4. What is PPID?

PPID stands for Parent Process ID. It identifies the parent process that created a child process.

### Q5. What is a foreground process?

A foreground process runs attached to the terminal and normally receives keyboard input from it.

### Q6. What is a background process?

A background process runs without keeping the terminal occupied by that command. We can start one using `&`.

### Q7. What is a daemon process?

A daemon is a background service process that performs tasks without direct interaction with a user terminal.

### Q8. What is the ps command?

`ps` displays information about running processes.

### Q9. What is the difference between ps and top?

`ps` provides a snapshot of process information, while `top` continuously updates process and system resource information.

### Q10. What is htop?

`htop` is an interactive process-monitoring tool that displays CPU, memory, and process information.

### Q11. What is the kill command?

`kill` sends a signal to a process using its PID.

### Q12. What is the difference between SIGTERM and SIGKILL?

- SIGTERM requests graceful termination and can be handled by the process.
- SIGKILL forcibly terminates a process and cannot be caught or ignored.

### Q13. What is the difference between kill, killall, and pkill?

- `kill` sends a signal using a PID.
- `killall` targets processes by name.
- `pkill` targets processes using name or other matching criteria.

### Q14. What is a zombie process?

A zombie process has completed execution but remains in the process table until its parent collects its exit status.

### Q15. What is an orphan process?

An orphan process is a child process whose parent has terminated while the child is still running.

### Q16. What is the difference between zombie and orphan processes?

A zombie has already exited but has not been reaped by its parent. An orphan is still running after its parent has terminated.

### Q17. What is the jobs command?

`jobs` displays background and stopped jobs associated with the current shell.

### Q18. What is the difference between bg and fg?

`bg` resumes a stopped job in the background, whereas `fg` brings a job to the foreground.

### Q19. What is the nice command?

`nice` starts a process with a specified scheduling nice value.

### Q20. What is the renice command?

`renice` changes the nice value of an already running process.

### Q21. What is the range of nice values?

The normal range is from `-20` to `19`. A lower value means a more favorable scheduling priority.

### Q22. What is the /proc directory?

`/proc` is a virtual filesystem that exposes process and kernel information.

### Q23. How do you find a process consuming high CPU?

```bash
ps aux --sort=-%cpu | head
```

Or:

```bash
top
```

### Q24. How do you find a process consuming high memory?

```bash
ps aux --sort=-%mem | head
```

### Q25. How do you run a process in the background?

Add `&` at the end of the command.

Example:

```bash
sleep 100 &
```

### Q26. How do you check the PID of a process?

```bash
pgrep -a process_name
```

You can also use:

```bash
pidof process_name
```

### Q27. What is an uninterruptible sleep state?

It is a process state, shown as `D`, in which the process is waiting for certain kernel operations, commonly I/O.

### Q28. How do you check the process tree?

```bash
pstree
```

Or:

```bash
ps -ef --forest
```

### Q29. How would you troubleshoot a Linux server with high CPU usage?

I would run `top` or `ps aux --sort=-%cpu | head` to identify CPU-intensive processes. Then I would investigate the application, logs, and workload before deciding whether to restart or terminate anything.

### Q30. What would you do if an application process is not responding?

I would identify its PID, inspect its state and logs, and try SIGTERM for graceful termination. If it does not respond and force termination is justified, I would use SIGKILL as a last resort.

---

## 19. Important Points to Remember

- A process is an executing instance of a program.
- PID identifies a process.
- PPID identifies a process's parent.
- `ps` displays process information.
- `top` monitors processes and system resources in real time.
- `htop` provides an interactive process-monitoring interface.
- `&` starts a command in the background.
- `jobs` displays jobs belonging to the current shell.
- `bg` resumes a stopped job in the background.
- `fg` brings a job to the foreground.
- `kill` sends signals to processes.
- SIGTERM requests graceful termination.
- SIGKILL forces termination.
- `nice` starts a process with a selected nice value.
- `renice` changes an existing process's nice value.
- Zombie processes have exited but have not been reaped.
- Orphan processes continue running after their parent exits.
- `/proc` provides process and kernel information.
- Always investigate before terminating important production processes.

---

## 20. Final DevOps Interview Tip

For a fresher DevOps interview, be confident explaining these practical situations:

1. How to identify a process consuming high CPU.
2. How to identify a process consuming high memory.
3. How to stop an unresponsive process safely.
4. How to run a script in the background.
5. How to monitor processes in real time.
6. How to identify zombie processes.
7. How to understand parent and child processes.
8. How to change process scheduling priority.

**Remember:** Process management is an important Linux skill for monitoring applications, troubleshooting server performance, managing background tasks, and maintaining reliable DevOps environments.
