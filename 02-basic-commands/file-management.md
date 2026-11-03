
# Linux File Management Commands

## 1. Introduction

Linux file management commands are used to create, copy, move, rename, delete, view, compare, and manage files and directories.

These commands are important for Linux administration, application deployment, log management, and DevOps operations.

## 2. Important File Management Commands

| Command | Purpose |
|---|---|
| `touch` | Creates an empty file or updates timestamps |
| `cat` | Displays and combines file contents |
| `cp` | Copies files and directories |
| `mv` | Moves or renames files and directories |
| `rm` | Removes files and directories |
| `file` | Identifies file types |
| `stat` | Displays detailed file information |
| `ln` | Creates links |
| `head` | Displays the beginning of a file |
| `tail` | Displays the end of a file |
| `less` | Views files page by page |
| `more` | Displays file contents one screen at a time |
| `nano` | Opens a terminal text editor |
| `vim` | Opens the Vim text editor |
| `diff` | Compares files line by line |
| `cmp` | Compares files byte by byte |
| `truncate` | Changes the size of a file |
| `split` | Splits a file into smaller parts |
| `shred` | Attempts to overwrite file contents before removal |

---

## 3. touch – Create Files

### Definition

The `touch` command creates an empty file if it does not exist. If the file already exists, it updates its access and modification timestamps by default.

### Syntax

```bash
touch [OPTIONS] FILE
```

### Examples

Create an empty file:

```bash
touch notes.txt
```

Create multiple files:

```bash
touch file1.txt file2.txt file3.txt
```

Create a file with a specific name:

```bash
touch linux-commands.md
```

Create a file inside a directory:

```bash
touch /home/user/Documents/report.txt
```

Update the timestamp of an existing file:

```bash
touch notes.txt
```

Update only the access time:

```bash
touch -a notes.txt
```

Update only the modification time:

```bash
touch -m notes.txt
```

### Important Options

| Option | Description |
|---|---|
| `-a` | Updates access time |
| `-m` | Updates modification time |
| `-c` | Does not create a file if it does not exist |
| `-t` | Sets a timestamp |
| `-r` | Uses another file's timestamps |

### Practical Use

Create configuration or log files:

```bash
touch application.log
touch config.txt
```

---

## 4. cat – Display File Contents

### Definition

The `cat` command displays, combines, and writes file contents.

### Syntax

```bash
cat [OPTIONS] FILE
```

### Examples

Display a file:

```bash
cat notes.txt
```

Display multiple files:

```bash
cat file1.txt file2.txt
```

Combine two files into a third file:

```bash
cat file1.txt file2.txt > combined.txt
```

Append one file's contents to another:

```bash
cat file2.txt >> combined.txt
```

Display line numbers:

```bash
cat -n notes.txt
```

Display line numbers only for non-empty lines:

```bash
cat -b notes.txt
```

Display non-printing characters:

```bash
cat -A notes.txt
```

### Important Options

| Option | Description |
|---|---|
| `-n` | Numbers all output lines |
| `-b` | Numbers non-empty lines |
| `-s` | Suppresses repeated empty lines |
| `-A` | Displays non-printing characters |

### Important Redirection Operators

| Operator | Purpose |
|---|---|
| `>` | Redirects output and overwrites the destination |
| `>>` | Redirects output and appends to the destination |
| `<` | Uses a file as command input |

Example:

```bash
echo "Linux Learning" > notes.txt
echo "DevOps Preparation" >> notes.txt
cat notes.txt
```

Output:

```text
Linux Learning
DevOps Preparation
```

Important: Be careful with `>` because it replaces the existing contents of a file.

---

## 5. cp – Copy Files and Directories

### Definition

The `cp` command copies files and directories from one location to another.

### Syntax

```bash
cp [OPTIONS] SOURCE DESTINATION
```

### Examples

Copy a file:

```bash
cp notes.txt backup.txt
```

Copy a file into another directory:

```bash
cp notes.txt Documents/
```

Copy multiple files into a directory:

```bash
cp file1.txt file2.txt Documents/
```

Copy a directory recursively:

```bash
cp -r project backup-project
```

Preserve file attributes where possible:

```bash
cp -p notes.txt backup.txt
```

Copy a directory while preserving attributes:

```bash
cp -a project project-backup
```

Ask before overwriting an existing file:

```bash
cp -i notes.txt backup.txt
```

Display messages about copied files:

```bash
cp -v notes.txt backup.txt
```

### Important Options

| Option | Description |
|---|---|
| `-r` | Copies directories recursively |
| `-R` | Copies directories recursively |
| `-i` | Asks before overwriting |
| `-f` | Forces copying by removing the destination if needed |
| `-v` | Displays copied files |
| `-p` | Preserves mode, ownership where permitted, and timestamps |
| `-a` | Archive mode; preserves attributes and copies recursively |

### Practical DevOps Example

Create a backup of an application configuration file:

```bash
cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
```

Important: Copying configuration files may require administrator privileges.

---

## 6. mv – Move and Rename Files

### Definition

The `mv` command moves files or directories from one location to another. It can also rename files and directories.

### Syntax

```bash
mv [OPTIONS] SOURCE DESTINATION
```

### Examples

Rename a file:

```bash
mv old.txt new.txt
```

Move a file into another directory:

```bash
mv notes.txt Documents/
```

Move multiple files into a directory:

```bash
mv file1.txt file2.txt Documents/
```

Move a directory:

```bash
mv project /home/user/
```

Ask before overwriting:

```bash
mv -i notes.txt Documents/
```

Display the movement:

```bash
mv -v notes.txt Documents/
```

Prevent overwriting an existing destination:

```bash
mv -n notes.txt Documents/
```

### Important Options

| Option | Description |
|---|---|
| `-i` | Asks before overwriting |
| `-f` | Does not prompt before overwriting |
| `-n` | Does not overwrite an existing destination |
| `-v` | Displays the operation |
| `-u` | Moves only when the source is newer or the destination is missing |

### Practical DevOps Example

Rename an old application configuration file:

```bash
mv application.conf application.conf.old
```

This can be useful when preparing a new configuration.

---

## 7. rm – Remove Files and Directories

### Definition

The `rm` command removes files and directories.

### Syntax

```bash
rm [OPTIONS] FILE
```

### Examples

Remove a file:

```bash
rm notes.txt
```

Remove multiple files:

```bash
rm file1.txt file2.txt
```

Ask before deleting:

```bash
rm -i notes.txt
```

Display deleted files:

```bash
rm -v notes.txt
```

Remove a directory and its contents recursively:

```bash
rm -r project
```

Forcefully remove files without prompting:

```bash
rm -f notes.txt
```

Forcefully remove a directory and its contents:

```bash
rm -rf temporary-folder
```

### Important Options

| Option | Description |
|---|---|
| `-r` | Removes directories and their contents recursively |
| `-f` | Forces removal without prompting |
| `-i` | Prompts before every removal |
| `-I` | Prompts once before removing multiple files or recursively removing |
| `-v` | Displays removed files |

### Important Safety Warning

Be extremely careful with:

```bash
rm -rf
```

This command can permanently remove large amounts of data.

Never execute an unfamiliar deletion command on a production server.

Before deleting, verify the target:

```bash
pwd
ls -lah
```

Safer interactive deletion:

```bash
rm -ri project
```

---

## 8. file – Identify File Types

### Definition

The `file` command identifies a file's type by examining its contents and other information.

A file's type is not determined only by its extension.

### Examples

Check a text file:

```bash
file notes.txt
```

Check a script:

```bash
file script.sh
```

Check a binary file:

```bash
file /bin/ls
```

Check multiple files:

```bash
file *
```

### Important Options

| Option | Description |
|---|---|
| `-b` | Displays the result without the filename |
| `-i` | Displays MIME type information |
| `-L` | Follows symbolic links |

### Practical Use

Identify an unknown downloaded file before attempting to use it:

```bash
file downloaded-file
```

---

## 9. stat – Display File Information

### Definition

The `stat` command displays detailed information about a file or directory.

### Syntax

```bash
stat FILE
```

### Example

```bash
stat notes.txt
```

It may display:

- File size
- File type
- Permissions
- Inode number
- Number of links
- Owner and group
- Access time
- Modification time
- Change time

Display only the file size in bytes:

```bash
stat -c "%s" notes.txt
```

Display permissions:

```bash
stat -c "%a" notes.txt
```

Display owner and group:

```bash
stat -c "%U %G" notes.txt
```

### Important Options

| Option | Description |
|---|---|
| `-c` | Uses a custom output format |
| `-f` | Displays filesystem information |
| `-L` | Follows symbolic links |

---

## 10. ln – Create Links

### Definition

The `ln` command creates links between files.

There are two major types:

1. Hard links
2. Symbolic links (soft links)

### 10.1 Hard Link

A hard link is another directory entry referring to the same inode as the original file.

Example:

```bash
ln original.txt hardlink.txt
```

Both names refer to the same underlying file data.

Important points:

- Hard links usually cannot cross filesystem boundaries.
- Hard links to directories are normally not allowed for ordinary users.
- Removing one filename does not remove the data while another hard link still exists.

### 10.2 Symbolic Link

A symbolic link is a separate file that stores a path referring to another file or directory.

Example:

```bash
ln -s original.txt softlink.txt
```

Create a symbolic link to a directory:

```bash
ln -s /var/log logs-link
```

### Difference Between Hard Link and Soft Link

| Hard Link | Symbolic Link |
|---|---|
| Refers to the same inode | Refers to a pathname |
| Normally cannot cross filesystems | Can point across filesystems |
| Usually remains accessible if another hard link exists | Can become broken if its target is removed |
| Cannot normally link directories | Can link directories |
| Created using `ln` | Created using `ln -s` |

### Check Links

```bash
ls -li
```

The inode number helps identify hard links referring to the same inode.

---

## 11. head – Display the Beginning of a File

### Definition

The `head` command displays the first lines of a file.

By default, it displays the first 10 lines.

### Examples

Display the first 10 lines:

```bash
head notes.txt
```

Display the first 5 lines:

```bash
head -n 5 notes.txt
```

Display the first 20 lines:

```bash
head -n 20 notes.txt
```

Display the first 100 bytes:

```bash
head -c 100 notes.txt
```

### Important Options

| Option | Description |
|---|---|
| `-n` | Specifies the number of lines |
| `-c` | Specifies the number of bytes |
| `-q` | Suppresses filename headers when displaying multiple files |
| `-v` | Always displays filename headers |

---

## 12. tail – Display the End of a File

### Definition

The `tail` command displays the last lines of a file.

By default, it displays the last 10 lines.

### Examples

Display the last 10 lines:

```bash
tail notes.txt
```

Display the last 5 lines:

```bash
tail -n 5 notes.txt
```

Display the last 50 lines:

```bash
tail -n 50 notes.txt
```

Display the last 100 bytes:

```bash
tail -c 100 notes.txt
```

### Follow a Log File

```bash
tail -f application.log
```

This continuously displays new lines added to the file.

Another option:

```bash
tail -F application.log
```

`-F` retries if the file is rotated or recreated, which can be useful for monitoring logs.

### Important Options

| Option | Description |
|---|---|
| `-n` | Displays the specified number of lines |
| `-c` | Displays the specified number of bytes |
| `-f` | Continuously follows a file |
| `-F` | Follows a file by name and retries if it is replaced |
| `-q` | Suppresses filename headers |

### DevOps Example

Monitor an application log in real time:

```bash
tail -f /var/log/nginx/access.log
```

Stop monitoring using:

```text
Ctrl + C
```

---

## 13. less – View Files Page by Page

### Definition

The `less` command allows users to view large files one screen at a time without loading the entire file into a terminal display.

### Example

```bash
less application.log
```

### Useful Keyboard Shortcuts

| Key | Purpose |
|---|---|
| `Space` | Move forward one screen |
| `b` | Move backward one screen |
| `Enter` | Move down one line |
| `k` | Move up one line |
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Find next match |
| `N` | Find previous match |
| `g` | Go to beginning |
| `G` | Go to end |
| `q` | Quit |

### Practical Use

View a large log file:

```bash
less /var/log/syslog
```

The exact log filename varies by distribution.

---

## 14. more – View Files One Screen at a Time

### Definition

The `more` command displays file contents one screen at a time.

### Example

```bash
more notes.txt
```

### Common Controls

| Key | Purpose |
|---|---|
| Space | Next screen |
| Enter | Next line |
| q | Quit |

### Difference Between more and less

| more | less |
|---|---|
| Basic pager | More feature-rich pager |
| Primarily moves forward | Supports forward and backward navigation |
| Limited navigation features | Supports searching and flexible navigation |

---

## 15. nano – Terminal Text Editor

### Definition

Nano is a beginner-friendly terminal-based text editor used to create and edit text files.

### Open a File

```bash
nano notes.txt
```

### Common Keyboard Shortcuts

| Shortcut | Purpose |
|---|---|
| Ctrl + O | Save file |
| Enter | Confirm filename |
| Ctrl + X | Exit Nano |
| Ctrl + K | Cut current line |
| Ctrl + U | Paste cut text |
| Ctrl + W | Search text |
| Ctrl + \ | Search and replace |
| Ctrl + G | Open help |
| Ctrl + C | Display cursor position |

### Create a Configuration File

```bash
nano config.txt
```

Type the required configuration and save it using Ctrl + O.

---

## 16. vim – Advanced Terminal Text Editor

### Definition

Vim is a powerful terminal-based text editor commonly available on Linux systems.

It supports multiple editing modes.

### Open a File

```bash
vim notes.txt
```

### Important Vim Modes

1. Normal Mode
2. Insert Mode
3. Command-Line Mode
4. Visual Mode

### Important Commands

| Command | Purpose |
|---|---|
| `i` | Enter Insert Mode |
| `Esc` | Return to Normal Mode |
| `:w` | Save changes |
| `:q` | Quit |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |
| `dd` | Delete current line |
| `yy` | Copy current line |
| `p` | Paste |
| `/word` | Search for a word |

### Example

Open a file:

```bash
vim notes.txt
```

Press `i` to insert text.

Press `Esc` and type:

```vim
:wq
```

Press Enter to save and exit.

---

## 17. diff – Compare Files

### Definition

The `diff` command compares two files line by line and displays their differences.

### Syntax

```bash
diff [OPTIONS] FILE1 FILE2
```

### Examples

Compare two files:

```bash
diff file1.txt file2.txt
```

Display differences in a more readable format:

```bash
diff -u file1.txt file2.txt
```

Compare directories:

```bash
diff -r directory1 directory2
```

Ignore differences in uppercase and lowercase:

```bash
diff -i file1.txt file2.txt
```

Ignore whitespace differences:

```bash
diff -w file1.txt file2.txt
```

### Important Options

| Option | Description |
|---|---|
| `-u` | Unified output format |
| `-r` | Compares directories recursively |
| `-i` | Ignores case differences |
| `-w` | Ignores all whitespace differences |
| `-q` | Reports only whether files differ |

### DevOps Example

Compare an old and new configuration file before deployment:

```bash
diff -u nginx.conf.old nginx.conf.new
```

---

## 18. cmp – Compare Files Byte by Byte

### Definition

The `cmp` command compares two files byte by byte and reports where they differ.

### Examples

```bash
cmp file1.txt file2.txt
```

Display the byte and line number of the first difference:

```bash
cmp -l file1.txt file2.txt
```

### Difference Between diff and cmp

| diff | cmp |
|---|---|
| Compares text line by line | Compares byte by byte |
| Displays line-based differences | Reports byte positions |
| Useful for configuration comparison | Useful for exact file comparison |

---

## 19. truncate – Change File Size

### Definition

The `truncate` command reduces or increases a file's size to a specified length.

### Examples

Create an empty file if needed and set its size to zero:

```bash
truncate -s 0 application.log
```

Set a file size to 1 MB:

```bash
truncate -s 1M test.txt
```

Set a file size to 100 bytes:

```bash
truncate -s 100 test.txt
```

### Important Options

| Option | Description |
|---|---|
| `-s` | Specifies the desired file size |
| `-c` | Does not create missing files |

### DevOps Example

Clear an application log without deleting the file:

```bash
sudo truncate -s 0 /var/log/application.log
```

Important: Truncating a log destroys its existing contents. Preserve or rotate logs according to your organization's retention policy before clearing them.

---

## 20. split – Split Large Files

### Definition

The `split` command divides a large file into smaller files.

### Examples

Split a file into parts of 1000 lines:

```bash
split -l 1000 large.txt part_
```

Split a file into parts of 10 MB:

```bash
split -b 10M large.txt part_
```

Split into files with numeric suffixes:

```bash
split -d large.txt part_
```

### Important Options

| Option | Description |
|---|---|
| `-l` | Splits by number of lines |
| `-b` | Splits by byte size |
| `-d` | Uses numeric suffixes |
| `-a` | Sets suffix length |

### Practical Use

Large log files can be split into smaller files for easier analysis and transfer.

---

## 21. shred – Secure File Removal Attempt

### Definition

The `shred` command attempts to overwrite file contents so that recovering the original data becomes more difficult.

### Example

```bash
shred -u sensitive.txt
```

### Important Options

| Option | Description |
|---|---|
| `-n` | Specifies the number of overwrite passes |
| `-u` | Removes the file after overwriting |
| `-v` | Displays progress |
| `-z` | Adds a final overwrite pass with zeros |

Example:

```bash
shred -n 3 -u sensitive.txt
```

### Important Warning

`shred` is not guaranteed to securely erase data on every storage device or filesystem. SSD wear-leveling, journaling, snapshots, backups, and copy-on-write filesystems can prevent reliable overwriting.

Use appropriate storage encryption and device-specific secure-erase procedures when secure data disposal is required.

---

## 22. File Timestamps

Linux files commonly have three important timestamps.

| Timestamp | Meaning |
|---|---|
| atime | Last access time |
| mtime | Last content modification time |
| ctime | Last metadata or inode status change time |

### Check timestamps

```bash
stat notes.txt
```

### Update access time

```bash
touch -a notes.txt
```

### Update modification time

```bash
touch -m notes.txt
```

Important: ctime is not the creation time. It represents the last change to inode metadata or file status.

---

## 23. Important File Management Concepts

### 23.1 File

A file is a named object used to store data.

Examples:

```text
notes.txt
script.sh
application.log
config.conf
```

### 23.2 Directory

A directory is a filesystem object that stores references to files and other directories.

Example:

```text
/home/user/Documents
```

### 23.3 Inode

An inode is a filesystem data structure that stores metadata about a file, such as permissions, ownership, size, timestamps, and data block references.

The filename is stored in a directory entry, not inside the inode itself.

Check inode numbers:

```bash
ls -i
```

### 23.4 Symbolic Link

A symbolic link is a separate filesystem object that points to another path.

Create one:

```bash
ln -s original.txt shortcut.txt
```

### 23.5 File Extension

A file extension is the suffix in a filename that commonly indicates the file's format or purpose.

Examples:

- `.txt`
- `.sh`
- `.log`
- `.conf`
- `.md`

Linux does not require file extensions to identify every file type.

---

## 24. Practical DevOps Scenarios

### Scenario 1: Create an application configuration file

```bash
touch application.conf
nano application.conf
```

### Scenario 2: Back up an existing configuration file

```bash
cp nginx.conf nginx.conf.backup
```

### Scenario 3: Rename an old deployment file

```bash
mv application-old.conf application.conf
```

### Scenario 4: Monitor application logs

```bash
tail -f application.log
```

### Scenario 5: Display the last 50 lines of an error log

```bash
tail -n 50 application.log
```

### Scenario 6: Compare two application configurations

```bash
diff -u config-old.conf config-new.conf
```

### Scenario 7: Identify an unknown downloaded file

```bash
file downloaded-file
```

### Scenario 8: Check file permissions and ownership

```bash
stat application.conf
```

### Scenario 9: Create a symbolic link to application logs

```bash
ln -s /var/log/nginx nginx-logs
```

### Scenario 10: Clear a test log file

```bash
truncate -s 0 test.log
```

Do this only after confirming that the existing log contents are no longer needed.

---

## 25. Interview Questions and Answers

### Q1. What is the purpose of the touch command?

The `touch` command creates an empty file if it does not exist or updates its timestamps if it already exists.

### Q2. What is the difference between cat and less?

`cat` displays file contents directly in the terminal. `less` allows users to navigate large files page by page.

### Q3. What is the difference between cp and mv?

`cp` copies files or directories, while `mv` moves or renames them.

### Q4. How do you copy a directory recursively?

```bash
cp -r source destination
```

### Q5. What is the difference between rm and rmdir?

`rm` removes files and can recursively remove directories. `rmdir` removes only empty directories.

### Q6. What does rm -rf do?

It forcefully removes files and directories recursively without interactive confirmation. It should be used carefully because it can permanently destroy data.

### Q7. What is the difference between > and >>?

`>` overwrites the destination file. `>>` appends output to the destination file.

### Q8. How do you monitor a log file in real time?

```bash
tail -f application.log
```

### Q9. What is the difference between head and tail?

`head` displays the beginning of a file, while `tail` displays the end.

### Q10. What is the purpose of the file command?

It identifies a file's type by examining its contents and other information.

### Q11. What is the purpose of stat?

`stat` displays detailed file information, including size, permissions, ownership, inode, and timestamps.

### Q12. What is an inode?

An inode is a filesystem data structure containing metadata about a file and references to its stored data.

### Q13. What is the difference between hard links and symbolic links?

A hard link refers to the same inode as the original file. A symbolic link stores a path pointing to another file or directory.

### Q14. What is the purpose of the diff command?

`diff` compares files line by line and displays their differences.

### Q15. What is the difference between diff and cmp?

`diff` compares files line by line, whereas `cmp` compares them byte by byte.

### Q16. How do you clear a log file without deleting it?

```bash
truncate -s 0 application.log
```

### Q17. How do you create a symbolic link?

```bash
ln -s original.txt link.txt
```

### Q18. How do you display file permissions and ownership?

```bash
stat filename
```

Or:

```bash
ls -l filename
```

### Q19. What is the difference between mtime and ctime?

mtime indicates when file contents were last modified. ctime indicates when inode metadata or file status was last changed.

### Q20. How do you compare old and new configuration files before deployment?

```bash
diff -u old.conf new.conf
```

---

## 26. Practical Exercises

Perform these exercises in your Linux environment.

### Exercise 1: Create a file

```bash
touch notes.txt
```

### Exercise 2: Create multiple files

```bash
touch file1.txt file2.txt file3.txt
```

### Exercise 3: Write text into a file

```bash
echo "Learning Linux" > notes.txt
```

### Exercise 4: Append more text

```bash
echo "Preparing for DevOps" >> notes.txt
```

### Exercise 5: Display file contents

```bash
cat notes.txt
```

### Exercise 6: Copy a file

```bash
cp notes.txt backup.txt
```

### Exercise 7: Rename a file

```bash
mv backup.txt notes-backup.txt
```

### Exercise 8: Create a directory and copy a file into it

```bash
mkdir documents
cp notes.txt documents/
```

### Exercise 9: Display file information

```bash
stat notes.txt
```

### Exercise 10: Identify the file type

```bash
file notes.txt
```

### Exercise 11: Create a symbolic link

```bash
ln -s notes.txt notes-link.txt
```

### Exercise 12: Compare two files

```bash
echo "Linux" > file1.txt
echo "DevOps" > file2.txt
diff file1.txt file2.txt
```

### Exercise 13: Display the first three lines

```bash
head -n 3 notes.txt
```

### Exercise 14: Display the last three lines

```bash
tail -n 3 notes.txt
```

### Exercise 15: Clear a test file

```bash
truncate -s 0 file2.txt
```

### Exercise 16: Remove test files

```bash
rm file1.txt file2.txt
```

### Exercise 17: Remove the test directory after checking its contents

```bash
ls -la documents
rm documents/notes.txt
rmdir documents
```

---

## 27. Quick Revision Table

| Command | Purpose |
|---|---|
| `touch` | Create files or update timestamps |
| `cat` | Display or combine file contents |
| `cp` | Copy files and directories |
| `mv` | Move or rename files |
| `rm` | Remove files and directories |
| `file` | Identify file type |
| `stat` | Display detailed file information |
| `ln` | Create hard links |
| `ln -s` | Create symbolic links |
| `head` | Display beginning of file |
| `tail` | Display end of file |
| `less` | View files page by page |
| `more` | View files one screen at a time |
| `nano` | Edit files using Nano |
| `vim` | Edit files using Vim |
| `diff` | Compare files line by line |
| `cmp` | Compare files byte by byte |
| `truncate` | Change file size |
| `split` | Split large files |
| `shred` | Attempt to overwrite file contents |

## 28. Final Takeaway

File management is one of the most important Linux skills for a DevOps engineer.

You will regularly use commands such as `touch`, `cat`, `cp`, `mv`, `rm`, `tail`, `less`, and `diff` while creating configuration files, backing up deployments, monitoring logs, and troubleshooting production systems.

Practice these commands until you can use them confidently without referring to notes.
