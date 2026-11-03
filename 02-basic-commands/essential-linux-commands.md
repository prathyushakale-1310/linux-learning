# Essential Linux Commands

Linux commands are instructions executed through the Linux
terminal to perform operations such as file management,
system administration, networking and process management.

# 1. Basic Navigation Commands

## 1. pwd

Definition:
Displays the present working directory.

Syntax:
pwd

Example:
pwd

## 2. ls

Definition:
Lists files and directories.

Syntax:
ls [options] [directory]

Examples:
ls
ls -l
ls -a
ls -lh
ls -lah
ls -lt

Options:
-l : Long listing format
-a : Show hidden files
-h : Human-readable file sizes
-t : Sort by modification time
-R : List recursively

## 3. cd

Definition:
Changes the current working directory.

Syntax:
cd [directory]

Examples:
cd /home
cd ..
cd ~
cd /
cd -

## 4. mkdir

Definition:
Creates directories.

Syntax:
mkdir [options] directory

Examples:
mkdir projects
mkdir -p projects/linux/commands
mkdir dir1 dir2

## 5. rmdir

Definition:
Removes empty directories.

Syntax:
rmdir directory

Example:
rmdir old_folder

# 2. File Management Commands

## 6. touch

Definition:
Creates an empty file or updates file timestamps.

Syntax:
touch filename

Examples:
touch file.txt
touch file1.txt file2.txt

## 7. cp

Definition:
Copies files and directories.

Syntax:
cp [options] source destination

Examples:
cp file.txt backup.txt
cp file.txt /home/user/
cp -r folder1 folder2
cp -i file.txt backup.txt

Options:
-r : Copy directories recursively
-i : Ask before overwriting
-v : Show copied files

## 8. mv

Definition:
Moves or renames files and directories.

Syntax:
mv source destination

Examples:
mv old.txt new.txt
mv file.txt /home/user/
mv -i file.txt backup/

## 9. rm

Definition:
Removes files and directories.

Syntax:
rm [options] filename

Examples:
rm file.txt
rm -r folder
rm -i file.txt
rm -f file.txt

Options:
-r : Remove directories recursively
-f : Force removal
-i : Ask before removal

Warning:
Be extremely careful with rm -rf.

## 10. ln

Definition:
Creates hard links and symbolic links.

Syntax:
ln source linkname
ln -s source linkname

Examples:
ln file.txt hardlink.txt
ln -s file.txt symlink.txt

# 3. File Viewing Commands

## 11. cat

Definition:
Displays and combines file contents.

Syntax:
cat filename

Examples:
cat file.txt
cat file1.txt file2.txt
cat -n file.txt

## 12. less

Definition:
Displays file contents one page at a time.

Syntax:
less filename

Example:
less application.log

Press q to exit.

## 13. head

Definition:
Displays the first lines of a file.

Syntax:
head [options] filename

Examples:
head file.txt
head -n 5 file.txt

## 14. tail

Definition:
Displays the last lines of a file.

Syntax:
tail [options] filename

Examples:
tail file.txt
tail -n 20 file.txt
tail -f application.log

## 15. echo

Definition:
Prints text or variable values.

Syntax:
echo [text]

Examples:
echo "Hello Linux"
echo $HOME
echo "Linux Commands" > file.txt
echo "New line" >> file.txt

# 4. Search and Text Processing Commands

## 16. grep

Definition:
Searches for matching text or patterns.

Syntax:
grep [options] pattern filename

Examples:
grep "error" application.log
grep -i "error" application.log
grep -n "error" application.log
grep -r "error" /var/log

Options:
-i : Ignore case
-n : Show line numbers
-r : Search recursively
-v : Show non-matching lines
-c : Count matching lines

## 17. find

Definition:
Searches for files and directories.

Syntax:
find path [conditions]

Examples:
find . -name "file.txt"
find /home -type f
find . -type d
find . -name "*.log"

## 18. wc

Definition:
Counts lines, words and bytes or characters.

Examples:
wc file.txt
wc -l file.txt
wc -w file.txt
wc -c file.txt

## 19. sort

Definition:
Sorts lines of text.

Examples:
sort names.txt
sort -r names.txt
sort -n numbers.txt

## 20. uniq

Definition:
Removes or identifies adjacent duplicate lines.

Examples:
sort names.txt | uniq
sort names.txt | uniq -c

# 5. System Information Commands

## 21. uname

Definition:
Displays kernel and system information.

Examples:
uname
uname -a
uname -r
uname -m

## 22. whoami

Definition:
Displays the current effective username.

Example:
whoami

## 23. who

Definition:
Displays users currently logged in.

Example:
who

## 24. hostname

Definition:
Displays the system hostname.

Examples:
hostname
hostname -I

## 25. date

Definition:
Displays the current system date and time.

Examples:
date
date "+%d-%m-%Y"

# 6. User and Permission Commands

## 26. sudo

Definition:
Executes commands with elevated privileges.

Example:
sudo apt update

## 27. chmod

Definition:
Changes file permissions.

Examples:
chmod 755 script.sh
chmod 644 file.txt
chmod +x script.sh

## 28. chown

Definition:
Changes file ownership.

Examples:
sudo chown user file.txt
sudo chown user:group file.txt

## 29. id

Definition:
Displays user and group IDs.

Example:
id

## 30. groups

Definition:
Displays the groups of a user.

Example:
groups

# 7. Process Management Commands

## 31. ps

Definition:
Displays process information.

Examples:
ps
ps aux
ps -ef
ps aux | grep nginx

## 32. top

Definition:
Displays live process and resource usage.

Example:
top

## 33. kill

Definition:
Sends a signal to a process using its PID.

Examples:
kill 1234
kill -15 1234
kill -9 1234

## 34. pgrep

Definition:
Finds process IDs by process name or pattern.

Example:
pgrep nginx

## 35. jobs

Definition:
Displays jobs running in the current shell.

Example:
jobs

# 8. Disk and Memory Commands

## 36. df

Definition:
Displays filesystem disk space usage.

Examples:
df -h
df -Th

## 37. du

Definition:
Displays file and directory disk usage.

Examples:
du -sh folder/
du -h --max-depth=1 /var

## 38. free

Definition:
Displays memory and swap usage.

Examples:
free -h
free -m

## 39. uptime

Definition:
Displays system uptime and load averages.

Example:
uptime

# 9. Networking Commands

## 40. ip

Definition:
Displays and manages network interfaces and routes.

Examples:
ip addr
ip route
ip link

## 41. ping

Definition:
Tests network connectivity.

Example:
ping -c 4 google.com

## 42. ssh

Definition:
Connects securely to a remote machine.

Syntax:
ssh username@hostname

Example:
ssh user@192.168.1.10

## 43. scp

Definition:
Copies files securely between systems.

Example:
scp file.txt user@server:/home/user/

## 44. curl

Definition:
Transfers data using URLs and supports HTTP requests.

Examples:
curl https://example.com
curl -I https://example.com

## 45. wget

Definition:
Downloads files from URLs.

Example:
wget https://example.com/file.zip

## 46. ss

Definition:
Displays socket and network connection information.

Examples:
ss -tuln
ss -tulnp

# 10. Archive and Compression Commands

## 47. tar

Definition:
Creates and extracts archive files.

Examples:
tar -cvf backup.tar folder/
tar -xvf backup.tar
tar -czvf backup.tar.gz folder/
tar -xzvf backup.tar.gz

## 48. zip

Definition:
Compresses files into ZIP archives.

Examples:
zip files.zip file1.txt file2.txt
zip -r backup.zip folder/

## 49. unzip

Definition:
Extracts ZIP archives.

Examples:
unzip backup.zip
unzip -l backup.zip

# 11. Package Management Commands

## 50. apt

Definition:
Manages software packages on Debian-based systems.

Examples:
sudo apt update
sudo apt upgrade
sudo apt install nginx
sudo apt remove nginx

## 51. dnf

Definition:
Manages packages on Fedora and modern RHEL-based systems.

Example:
sudo dnf install nginx

## 52. yum

Definition:
Manages packages on older RHEL-based systems.

Example:
sudo yum install nginx

## 53. pacman

Definition:
Manages packages on Arch Linux.

Example:
sudo pacman -S nginx

# 12. Service Management Commands

## 54. systemctl

Definition:
Controls systemd services and units.

Examples:
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl status nginx
sudo systemctl enable nginx
sudo systemctl disable nginx

## 55. service

Definition:
Provides a service-management interface on systems
that support it.

Examples:
sudo service nginx start
sudo service nginx stop
sudo service nginx status

# 13. Environment and History Commands

## 56. export

Definition:
Exports shell variables to child processes.

Example:
export APP_ENV=production

## 57. alias

Definition:
Creates shortcuts for commands.

Examples:
alias ll='ls -lah'
unalias ll

## 58. history

Definition:
Displays commands entered in the shell history.

Examples:
history
history 10
history -c

## 59. man

Definition:
Displays the manual page of a command.

Examples:
man ls
man grep
man chmod

## 60. clear

Definition:
Clears the terminal display.

Example:
clear

---

# Important DevOps Practice Commands

Check disk usage:
df -h

Check memory:
free -h

Check running processes:
ps aux

Monitor processes:
top

Check listening ports:
ss -tulnp

Monitor logs:
tail -f application.log

Check system uptime:
uptime

Check network connectivity:
ping -c 4 google.com

Check service status:
systemctl status nginx

Find large files:
find /var -type f -size +100M

---

# Important Notes

1. Linux commands are case-sensitive.
2. Use man command to learn command options.
3. Use sudo only when elevated privileges are needed.
4. Be careful when deleting files.
5. Practice commands in a safe Linux environment.
6. Some commands require installation or administrator privileges.
7. Command availability and options can differ between Linux distributions.
