
# Linux Navigation Commands

## 1. Introduction

Linux navigation commands are used to move between directories, identify the current working directory, list files and directories, and locate files in the Linux filesystem.

These commands are among the most frequently used commands by Linux administrators and DevOps engineers.

### Important Navigation Commands

| Command | Purpose |
|---|---|
| `pwd` | Displays the current working directory |
| `ls` | Lists files and directories |
| `cd` | Changes the current directory |
| `tree` | Displays directories in tree format |
| `find` | Searches for files and directories |
| `locate` | Quickly searches for files using a database |
| `which` | Displays the location of an executable |
| `whereis` | Locates binary, source, and manual files |
| `realpath` | Displays the absolute path |
| `basename` | Displays the final component of a path |
| `dirname` | Displays the directory component of a path |

---

## 2. pwd – Print Working Directory

### Definition

The `pwd` command displays the absolute path of the current working directory.

### Syntax

```bash
pwd [OPTION]
```

### Examples

Display the current directory:

```bash
pwd
```

Example output:

```text
/home/user
```

Display the physical path without resolving symbolic links:

```bash
pwd -P
```

Display the logical path, including symbolic links:

```bash
pwd -L
```

### Important Options

| Option | Description |
|---|---|
| `-L` | Displays the logical path |
| `-P` | Displays the physical path |

### Practical Use

Before creating or modifying files, use `pwd` to confirm your current location.

---

## 3. ls – List Directory Contents

### Definition

The `ls` command displays files and directories in a specified location.

### Syntax

```bash
ls [OPTIONS] [PATH]
```

### Basic Examples

List files and directories:

```bash
ls
```

List the contents of a specific directory:

```bash
ls /home
```

Display files in long format:

```bash
ls -l
```

Display hidden files:

```bash
ls -a
```

Display hidden files in long format:

```bash
ls -la
```

Display human-readable file sizes:

```bash
ls -lh
```

List directories only:

```bash
ls -d */
```

List files in reverse order:

```bash
ls -r
```

List files by modification time:

```bash
ls -lt
```

List files by size:

```bash
ls -lS
```

### Important Options

| Option | Description |
|---|---|
| `-l` | Long listing format |
| `-a` | Displays hidden files, including `.` and `..` |
| `-A` | Displays hidden files except `.` and `..` |
| `-h` | Human-readable file sizes |
| `-R` | Lists contents recursively |
| `-t` | Sorts by modification time |
| `-r` | Reverses the sorting order |
| `-S` | Sorts by file size |
| `-d` | Lists directory entries themselves |
| `-i` | Displays inode numbers |

### Understanding ls -l Output

Example:

```bash
ls -l
```

Output:

```text
-rw-r--r-- 1 user user 1024 Nov 3 10:30 notes.txt
drwxr-xr-x 2 user user 4096 Nov 3 10:35 documents
```

Explanation:

| Field | Meaning |
|---|---|
| `-rw-r--r--` | File type and permissions |
| `1` | Number of hard links |
| `user` | Owner |
| `user` | Group |
| `1024` | File size in bytes |
| `Nov 3 10:30` | Modification date and time |
| `notes.txt` | Filename |

### Important Point

Linux hidden files usually begin with a dot (`.`).

Examples:

```text
.bashrc
.profile
.ssh
```

---

## 4. cd – Change Directory

### Definition

The `cd` command changes the current working directory.

### Syntax

```bash
cd [DIRECTORY]
```

### Examples

Move into a directory:

```bash
cd Documents
```

Move to the home directory:

```bash
cd
```

Another way to move to the home directory:

```bash
cd ~
```

Move to the root directory:

```bash
cd /
```

Move one directory backward:

```bash
cd ..
```

Move two directories upward:

```bash
cd ../..
```

Move into a directory using an absolute path:

```bash
cd /var/log
```

Move to the previous working directory:

```bash
cd -
```

### Important Navigation Symbols

| Symbol | Meaning |
|---|---|
| `/` | Root directory |
| `~` | Current user's home directory |
| `.` | Current directory |
| `..` | Parent directory |
| `-` | Previous working directory |

### Practical Example

Suppose your current location is:

```text
/home/user/Documents/projects
```

Run:

```bash
cd ..
```

New location:

```text
/home/user/Documents
```

Run:

```bash
cd ../..
```

New location:

```text
/home/user
```

---

## 5. Absolute Path and Relative Path

### 5.1 Absolute Path

An absolute path specifies the complete location of a file or directory starting from the root directory `/`.

Example:

```bash
cd /home/user/Documents
```

Characteristics:

- Starts from `/`.
- Specifies the complete path.
- Does not depend on the current working directory.

### 5.2 Relative Path

A relative path specifies a location based on the current working directory.

Example:

```bash
cd Documents
```

If your current directory is `/home/user`, the command moves to `/home/user/Documents`.

Characteristics:

- Does not start with `/`.
- Depends on the current working directory.
- Often shorter than an absolute path.

### Difference

| Absolute Path | Relative Path |
|---|---|
| Starts from `/` | Starts from the current directory |
| Complete path | Path relative to current location |
| Independent of current directory | Depends on current directory |
| Example: `/home/user/docs` | Example: `docs` |

---

## 6. mkdir – Make Directory

### Definition

The `mkdir` command creates new directories.

### Syntax

```bash
mkdir [OPTIONS] DIRECTORY
```

### Examples

Create a directory:

```bash
mkdir projects
```

Create multiple directories:

```bash
mkdir dev test production
```

Create parent directories when they do not exist:

```bash
mkdir -p projects/devops/linux
```

Create a directory with specific permissions:

```bash
mkdir -m 755 secure-folder
```

### Important Options

| Option | Description |
|---|---|
| `-p` | Creates parent directories as needed |
| `-m` | Sets directory permissions |
| `-v` | Displays a message for each created directory |

### Practical Example

Create a directory structure for a DevOps project:

```bash
mkdir -p devops/linux/commands
```

This creates all three directory levels if they do not already exist.

---

## 7. rmdir – Remove Empty Directory

### Definition

The `rmdir` command removes empty directories.

### Syntax

```bash
rmdir [OPTIONS] DIRECTORY
```

### Examples

Remove an empty directory:

```bash
rmdir old-folder
```

Remove empty parent directories:

```bash
rmdir -p projects/devops/linux
```

### Important Options

| Option | Description |
|---|---|
| `-p` | Removes parent directories if they are empty |
| `-v` | Displays messages about removed directories |

### Important Point

`rmdir` cannot remove a directory containing files or other non-empty directories.

---

## 8. tree – Display Directory Structure

### Definition

The `tree` command displays files and directories in a hierarchical tree format.

### Examples

Display the current directory structure:

```bash
tree
```

Display a specific directory:

```bash
tree /home
```

Display only two directory levels:

```bash
tree -L 2
```

Display directories only:

```bash
tree -d
```

Display hidden files:

```bash
tree -a
```

Display directories and files without indentation lines:

```bash
tree -i
```

### Important Options

| Option | Description |
|---|---|
| `-L` | Limits the depth |
| `-d` | Displays directories only |
| `-a` | Includes hidden files |
| `-f` | Displays full paths |
| `-h` | Displays file sizes in human-readable format |
| `-i` | Removes indentation lines |

### Installation

Ubuntu/Debian:

```bash
sudo apt update
sudo apt install tree
```

Fedora/RHEL-family:

```bash
sudo dnf install tree
```

### Practical Use

To understand a project directory:

```bash
tree linux-learning
```

---

## 9. find – Search for Files and Directories

### Definition

The `find` command searches for files and directories within a specified location based on conditions such as name, type, size, and modification time.

### Syntax

```bash
find [PATH] [OPTIONS] [EXPRESSION]
```

### Examples

Find a file by name:

```bash
find /home -name "notes.txt"
```

Search without considering uppercase or lowercase:

```bash
find /home -iname "notes.txt"
```

Find directories:

```bash
find /home -type d
```

Find regular files:

```bash
find /home -type f
```

Find files with a particular extension:

```bash
find /home -type f -name "*.log"
```

Find files larger than 100 MB:

```bash
find /var -type f -size +100M
```

Find files modified within the last 24 hours:

```bash
find /home -type f -mtime -1
```

Find empty files:

```bash
find /home -type f -empty
```

Find empty directories:

```bash
find /home -type d -empty
```

### Important Options

| Option | Description |
|---|---|
| `-name` | Searches by exact name pattern |
| `-iname` | Searches by name without case sensitivity |
| `-type f` | Searches for regular files |
| `-type d` | Searches for directories |
| `-size` | Searches by file size |
| `-mtime` | Searches by modification time in days |
| `-empty` | Finds empty files or directories |
| `-user` | Searches for files owned by a user |
| `-perm` | Searches by permissions |
| `-maxdepth` | Limits directory traversal depth |

### Practical DevOps Examples

Find log files:

```bash
find /var/log -type f -name "*.log"
```

Find files larger than 500 MB:

```bash
find / -type f -size +500M 2>/dev/null
```

Find files owned by a particular user:

```bash
find /home -type f -user ubuntu
```

Find files modified in the last seven days:

```bash
find /var/log -type f -mtime -7
```

### Important Point

`2>/dev/null` redirects error messages to `/dev/null`. It is useful when a search encounters directories that the current user cannot access.

---

## 10. locate – Quickly Search for Files

### Definition

The `locate` command searches for files and directories using a previously created database of filesystem paths.

### Examples

Search for a file:

```bash
locate notes.txt
```

Search without case sensitivity:

```bash
locate -i notes.txt
```

Display the number of matching results:

```bash
locate -c notes.txt
```

Limit the number of results:

```bash
locate -n 10 notes.txt
```

### Important Options

| Option | Description |
|---|---|
| `-i` | Ignores uppercase/lowercase differences |
| `-c` | Displays the number of matches |
| `-n` | Limits the number of results |
| `-r` | Uses a regular expression |

### Update the Database

On systems using the `plocate` or `mlocate` database tools:

```bash
sudo updatedb
```

### Difference Between find and locate

| find | locate |
|---|---|
| Searches the filesystem directly | Searches a database |
| Can find recently created files immediately | May not show recently created files until database update |
| Can search by size, owner, type, and time | Primarily searches indexed paths |
| Can be slower for large searches | Usually very fast |

---

## 11. which – Locate an Executable

### Definition

The `which` command displays the location of an executable command by searching directories listed in the PATH environment variable.

### Examples

Find the location of Python:

```bash
which python
```

Find the location of Git:

```bash
which git
```

Find the location of Bash:

```bash
which bash
```

### Practical Use

If you want to know which executable will run when you type a command, use `which`.

### Important Point

`which` is not guaranteed to find every command, especially shell built-ins. The Bash `type` command is more useful for identifying aliases, functions, built-ins, and executable commands.

Example:

```bash
type cd
type ls
```

---

## 12. whereis – Locate Binary, Source, and Manual Files

### Definition

The `whereis` command searches for the binary, source, and manual page files associated with a command.

### Examples

```bash
whereis ls
whereis python3
whereis nginx
```

### Important Options

| Option | Description |
|---|---|
| `-b` | Searches for binary files |
| `-m` | Searches for manual pages |
| `-s` | Searches for source files |

Examples:

```bash
whereis -b ls
whereis -m ls
```

---

## 13. realpath – Display the Absolute Path

### Definition

The `realpath` command displays the resolved absolute path of a file or directory.

### Examples

```bash
realpath notes.txt
```

Resolve a directory path:

```bash
realpath Documents
```

### Practical Use

Useful in shell scripts when you need to work with a complete path instead of a relative path.

---

## 14. basename – Extract the Filename

### Definition

The `basename` command displays the final component of a path.

### Examples

```bash
basename /home/user/Documents/notes.txt
```

Output:

```text
notes.txt
```

Remove a suffix:

```bash
basename report.txt .txt
```

Output:

```text
report
```

---

## 15. dirname – Extract the Directory Path

### Definition

The `dirname` command displays the directory portion of a path.

### Example

```bash
dirname /home/user/Documents/notes.txt
```

Output:

```text
/home/user/Documents
```

### Difference Between basename and dirname

| basename | dirname |
|---|---|
| Displays the final path component | Displays the directory portion |
| Example: `notes.txt` | Example: `/home/user/Documents` |

---

## 16. Important Navigation Concepts

### 16.1 Root Directory (/)

The root directory is the top-level directory in Linux.

All files and directories are organized under `/`.

Example:

```bash
cd /
ls
```

### 16.2 Home Directory (~)

The home directory is the personal directory assigned to a user.

Example:

```bash
cd ~
pwd
```

A normal user might have `/home/username` as their home directory. The root user's home directory is usually `/root`.

### 16.3 Hidden Files

Files whose names begin with a dot (`.`) are normally hidden in standard directory listings.

Examples:

```text
.bashrc
.profile
.ssh
```

Display them using:

```bash
ls -a
```

### 16.4 Parent Directory

The `..` symbol represents the parent directory.

Example:

```bash
cd ..
```

### 16.5 Current Directory

The `.` symbol represents the current directory.

Example:

```bash
ls .
```

---

## 17. Practical DevOps Scenarios

### Scenario 1: Find the Nginx configuration directory

```bash
find /etc -type d -name "nginx"
```

### Scenario 2: Find application log files

```bash
find /var/log -type f -name "*.log"
```

### Scenario 3: Find files larger than 1 GB

```bash
find / -type f -size +1G 2>/dev/null
```

### Scenario 4: Check the current working directory before deployment

```bash
pwd
```

### Scenario 5: Explore the application directory

```bash
cd /var/www
ls -lah
```

### Scenario 6: Locate the Git executable

```bash
which git
```

### Scenario 7: Check the full path of a configuration file

```bash
realpath /etc/hosts
```

### Scenario 8: Find recently modified log files

```bash
find /var/log -type f -mtime -1
```

---

## 18. Interview Questions and Answers

### Q1. What is the purpose of the pwd command?

`pwd` displays the absolute path of the current working directory.

### Q2. What is the difference between ls and ls -l?

`ls` displays filenames and directory names. `ls -l` displays detailed information such as permissions, ownership, size, and modification time.

### Q3. How do you display hidden files in Linux?

Use:

```bash
ls -a
```

### Q4. What is the purpose of the cd command?

`cd` changes the current working directory.

### Q5. What is the difference between / and ~?

`/` represents the root directory of the entire filesystem. `~` represents the current user's home directory.

### Q6. What does cd .. do?

It moves the current working directory to its parent directory.

### Q7. What is the difference between absolute and relative paths?

An absolute path starts from `/` and specifies a complete location. A relative path specifies a location based on the current working directory.

### Q8. What is the purpose of mkdir?

`mkdir` creates directories.

### Q9. What is the difference between mkdir and mkdir -p?

`mkdir` creates a directory. `mkdir -p` also creates missing parent directories.

### Q10. What is the difference between rmdir and rm -r?

`rmdir` removes empty directories. `rm -r` recursively removes directories and their contents.

### Q11. What is the purpose of the find command?

`find` searches for files and directories based on conditions such as name, type, size, owner, and modification time.

### Q12. What is the difference between find and locate?

`find` searches the filesystem directly, while `locate` searches an indexed database of paths.

### Q13. How do you find all .log files in Linux?

```bash
find / -type f -name "*.log" 2>/dev/null
```

### Q14. How do you find files larger than 100 MB?

```bash
find / -type f -size +100M 2>/dev/null
```

### Q15. What is the purpose of the tree command?

`tree` displays files and directories in a hierarchical structure.

### Q16. What is the difference between which and whereis?

`which` searches PATH directories for an executable command. `whereis` searches standard locations for binary, source, and manual files.

### Q17. How do you find the location of the Git command?

```bash
which git
```

### Q18. What is the purpose of realpath?

`realpath` displays the resolved absolute path of a file or directory.

### Q19. What is the difference between basename and dirname?

`basename` extracts the final component of a path, while `dirname` extracts its directory portion.

### Q20. How do you move to the previous working directory?

```bash
cd -
```

---

## 19. Practical Exercises

Perform these exercises in your Linux environment.

1. Display your current working directory.

```bash
pwd
```

2. List all files, including hidden files.

```bash
ls -la
```

3. Move to the root directory.

```bash
cd /
```

4. Move to your home directory.

```bash
cd ~
```

5. Create a directory named `devops`.

```bash
mkdir devops
```

6. Create nested directories.

```bash
mkdir -p devops/linux/commands
```

7. Move into the commands directory.

```bash
cd devops/linux/commands
```

8. Move two levels backward.

```bash
cd ../..
```

9. Display the directory structure.

```bash
tree devops
```

10. Find all `.log` files under `/var/log`.

```bash
find /var/log -type f -name "*.log"
```

11. Display the full path of `/etc/hosts`.

```bash
realpath /etc/hosts
```

12. Find the location of the Git executable.

```bash
which git
```

13. Display only directories in the current location.

```bash
ls -d */
```

14. Display the ten most recently modified entries.

```bash
ls -lt | head -10
```

15. Remove an empty test directory.

```bash
mkdir test-folder
rmdir test-folder
```

---

## 20. Quick Revision Table

| Command | Purpose |
|---|---|
| `pwd` | Current directory |
| `ls` | List directory contents |
| `cd` | Change directory |
| `mkdir` | Create directory |
| `rmdir` | Remove empty directory |
| `tree` | Display directory tree |
| `find` | Search filesystem |
| `locate` | Search indexed paths |
| `which` | Locate executable in PATH |
| `whereis` | Locate binary, source, and manual files |
| `realpath` | Display resolved absolute path |
| `basename` | Extract final path component |
| `dirname` | Extract directory component |

## 21. Final Takeaway

Linux navigation commands are essential for working with files, directories, servers, and application environments.

A DevOps engineer frequently uses `pwd`, `ls`, `cd`, `find`, and `realpath` while deploying applications, inspecting server directories, locating configuration files, and troubleshooting production systems.

Practice these commands regularly until you can use them without referring to notes.
