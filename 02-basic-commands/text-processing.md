
A commonly used Bash shortcut is:

This stores both normal output and error messages in `result.log`.
```
```bash

command &> output.log
```
ls /home /wrong-path > result.log 2>&1

This redirects both standard output and standard error to the same file in Bash.


### 3.6 Discarding Output

command > output.log 2>&1
`/dev/null` is a special device that discards anything written to it.

Example:
```

```bash

command > /dev/null
```

Example:
To discard both normal output and errors:

```bash

command > /dev/null 2>&1
```
```bash
```bash


To redirect standard output and standard error into the same file:
This is useful when running commands in scripts where output is unnecessary.


### 3.5 Redirecting Both Output and Errors

---

The error message is stored in `error.log`.
```
ls /wrong-path 2> error.log
```bash
## 4. Pipes (`|`)


Example:

A pipe is used to send the standard output of one command as the standard input of another command.

# Linux Text Processing Commands

Syntax:

## 1. Introduction to Text Processing

```bash
command1 | command2
Text processing in Linux refers to the process of reading, searching, filtering, extracting, modifying, sorting, and analyzing text data using command-line utilities.
```


Example:

Linux stores a large amount of information in text files, including:
```bash
ls | wc -l
```


This counts the number of lines produced by `ls`.

Another example:

```bash
ps aux | grep nginx
```

This searches the process list for entries containing `nginx`.

- System logs
- Configuration files
- Application logs
- User information
- Network information
- Server monitoring data
- Error messages

Text-processing commands help administrators and DevOps engineers analyze this information efficiently without manually opening every file.

### Why is text processing important in DevOps?

Text processing is commonly used for:

- Analyzing application logs.
- Finding errors in server logs.
- Searching for specific information in configuration files.
- Extracting required fields from files.
- Counting errors and warnings.
### Multiple Pipes
- Filtering unnecessary information.
- Automating tasks through shell scripts.
- Monitoring application and server health.

Example:


```bash
grep "ERROR" application.log
```
We can connect multiple commands using pipes.

This command searches for lines containing the word `ERROR` in `application.log`.
The `2>` operator redirects standard error into a file.


---

Example:
## 2. Standard Input, Standard Output and Standard Error


Linux uses three standard data streams for communication between commands and processes.

```bash
### 2.1 Standard Input (stdin)

cat application.log | grep "ERROR" | wc -l
Standard input is the data provided to a command or program.
```


This performs three operations:
- File descriptor: `0`

- Default source: Keyboard
1. Reads the application log.

2. Filters lines containing `ERROR`.
Example:

3. Counts the matching lines.
```bash
cat

```

Type some text and press `Ctrl + D` to finish.
A more efficient version is:

```bash

### 2.2 Standard Output (stdout)

Standard output is the normal output produced by a command.
grep "ERROR" application.log | wc -l
```

- File descriptor: `1`
- Default destination: Terminal

### Important Points


- A pipe is represented by `|`.
Example:

- The output of one command becomes the input of another.
- Pipes help combine simple commands into powerful operations.
```bash
- By default, pipes transfer standard output, not standard error.

ls
---

## 5. cat Command

```
The `cat` command is used to display, combine, and create text-file contents.

Syntax:

```bash
cat [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-n` | Display line numbers for all lines |
| `-b` | Number non-empty lines |
| `-s` | Suppress repeated empty lines |
| `-A` | Display non-printing characters and line endings |

### Examples

Display file contents:

```bash
cat file.txt
```

Display line numbers:

```bash
cat -n file.txt
```

Number only non-empty lines:

```bash
cat -b file.txt
```

Combine two files:

```bash
cat file1.txt file2.txt
```

Combine files and save the result:

```bash
cat file1.txt file2.txt > combined.txt
```

Create a file using standard input:

```bash
cat > newfile.txt
```

Type the contents and press `Ctrl + D` to save and finish.

### Practical Use

```bash
cat /etc/os-release
```

Displays Linux distribution identification information.

**Note:** `cat` is suitable for displaying small files. For large files, use `less`.

---

## 6. less and more Commands

These commands are used to view large text files page by page.

### 6.1 less Command

The `less` command allows users to navigate through a file without loading the entire output into the terminal at once.

Syntax:

```bash
less filename
```

Example:

```bash
less /var/log/syslog
```

Useful keyboard shortcuts:

| Key | Function |
|---|---|
| `Space` | Move forward one screen |
| `b` | Move backward one screen |
| `Enter` | Move down one line |
| `g` | Go to the beginning |
| `G` | Go to the end |
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Find next match |
| `N` | Find previous match |
| `q` | Quit |

### 6.2 more Command

The `more` command displays text one screen at a time.

Syntax:

```bash
more filename
```

Example:

```bash
more largefile.txt
```

Common keyboard shortcuts:

| Key | Function |
|---|---|
| `Space` | Next screen |
| `Enter` | Next line |
| `q` | Quit |

### Difference Between less and more

| Feature | less | more |
|---|---|---|
| Purpose | View text page by page | View text page by page |
| Backward navigation | Supported | Limited |
| Searching | Supported | Supported |
| Large files | Suitable | Suitable |
| Navigation | More flexible | More limited |

---

## 7. head Command

The `head` command displays the first lines of a file.

By default, it displays the first 10 lines.

Syntax:

```bash
head [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-n` | Specify the number of lines |
| `-c` | Display a specified number of bytes |
| `-q` | Do not display filenames in multi-file output |
| `-v` | Always display filenames |

### Examples

Display the first 10 lines:

```bash
head file.txt
```

Display the first 5 lines:

```bash
head -n 5 file.txt
```

Display the first 20 lines:

```bash
head -n 20 file.txt
```

Display the first 50 bytes:

```bash
head -c 50 file.txt
```

### Practical Scenario

To inspect the beginning of an application log:

```bash
head -n 10 application.log
```

---

## 8. tail Command

The `tail` command displays the last lines of a file.

By default, it displays the last 10 lines.

Syntax:

```bash
tail [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-n` | Specify the number of lines |
| `-c` | Display a specified number of bytes |
| `-f` | Continuously monitor file updates |
| `-F` | Follow a file by name and retry if it is replaced |
| `-q` | Suppress filenames in multi-file output |
| `-v` | Always display filenames |

### Examples

Display the last 10 lines:

```bash
tail file.txt
```

Display the last 5 lines:

```bash
tail -n 5 file.txt
```

Display the last 20 lines:

```bash
tail -n 20 application.log
```

### Live Log Monitoring

```bash
tail -f application.log
```

This continuously displays new lines added to the file.

Press `Ctrl + C` to stop monitoring.

### Difference Between tail -f and tail -F

- `tail -f` follows the file descriptor. If the file is replaced during log rotation, it may continue following the old file.
- `tail -F` follows the filename and retries if the file is replaced or temporarily unavailable.

### Practical DevOps Scenario

When an application is running on a server, developers and DevOps engineers can monitor logs in real time:

```bash
tail -f /var/log/nginx/access.log
```

This helps observe incoming HTTP requests.

For Nginx, log paths can vary depending on the distribution and configuration.

---

## 9. grep Command

The `grep` command is used to search for specific words, patterns, or strings inside files or command output.

The name comes from Global Regular Expression Print.

Syntax:

```bash
grep [options] "pattern" filename
```

### Common Options

| Option | Description |
|---|---|
| `-i` | Ignore uppercase and lowercase differences |
| `-n` | Display matching line numbers |
| `-v` | Display lines that do not match |
| `-r` | Search recursively in directories |
| `-R` | Search recursively and follow symbolic links |
| `-c` | Count matching lines |
| `-l` | Display only filenames containing matches |
| `-w` | Match complete words |
| `-o` | Display only matching portions |
| `-E` | Use extended regular expressions |
| `-A n` | Display n lines after a match |
| `-B n` | Display n lines before a match |
| `-C n` | Display n lines before and after a match |

### Examples

Search for a word:

```bash
grep "error" application.log
```

Case-insensitive search:

```bash
grep -i "error" application.log
```

Display line numbers:

```bash
grep -n "error" application.log
```

Display lines that do not contain a word:

```bash
grep -v "success" application.log
```

Count matching lines:

```bash
grep -c "ERROR" application.log
```

Search recursively:

```bash
grep -r "password" /etc
```

Display filenames containing a match:

```bash
grep -l "ERROR" *.log
```

Search for a complete word:

```bash
grep -w "root" file.txt
```

Display three lines after a matching line:

```bash
grep -A 3 "ERROR" application.log
```

Display three lines before a matching line:

```bash
grep -B 3 "ERROR" application.log
```

Display three lines before and after a matching line:

```bash
grep -C 3 "ERROR" application.log
```

### grep with Pipes

Search for Nginx processes:

```bash
ps aux | grep nginx
```

Search for failed SSH login messages:

```bash
grep -i "failed" /var/log/auth.log
```

The SSH authentication log location differs between distributions. For example, Debian and Ubuntu commonly use `/var/log/auth.log`, while RHEL-family systems commonly use `/var/log/secure`. Some systems use the systemd journal instead.

### Basic Regular Expressions

Regular expressions are patterns used to identify matching text.

| Pattern | Meaning |
|---|---|
| `^word` | Lines beginning with word |
| `word$` | Lines ending with word |
| `.` | Any single character |
| `*` | Zero or more occurrences of the preceding expression |
| `[abc]` | Any one character among a, b, or c |
| `[0-9]` | Any digit |
| `[^0-9]` | Any non-digit |
| `^$` | Empty line |

Examples:

Find lines starting with ERROR:

```bash
grep "^ERROR" application.log
```

Find lines ending with failed:

```bash
grep "failed$" application.log
```

Find lines containing digits:

```bash
grep "[0-9]" file.txt
```

Find empty lines:

```bash
grep "^$" file.txt
```

**Important:** Basic `grep` uses basic regular expressions. Options such as `-E` enable extended regular expressions, which support operators such as `+`, `?`, and `|` without requiring backslashes.

---

## 10. cut Command

The `cut` command extracts specific characters, bytes, or fields from each line of a file.

It is commonly used to extract columns from structured text.

Syntax:

```bash
cut [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-c` | Select character positions |
| `-b` | Select byte positions |
| `-d` | Specify a field delimiter |
| `-f` | Select field numbers |
| `--complement` | Select everything except the specified positions or fields |

### Examples

Extract the first five characters:

```bash
cut -c 1-5 file.txt
```

Extract the first field:

```bash
cut -d ':' -f 1 /etc/passwd
```

Here:

- `-d ':'` specifies the colon as the delimiter.
- `-f 1` selects the first field.

Extract the first and third fields:

```bash
cut -d ':' -f 1,3 /etc/passwd
```

Extract fields from a CSV file:

```bash
cut -d ',' -f 1,2 employees.csv
```

Extract everything except the first field:

```bash
cut -d ':' -f 1 --complement /etc/passwd
```

**Important:** `cut` works with a specified delimiter. It does not automatically parse every CSV format correctly, especially files containing quoted delimiters or complex escaping.

---

## 11. paste Command

The `paste` command combines lines from multiple files side by side.

Syntax:

```bash
paste [options] file1 file2
```

### Common Options

| Option | Description |
|---|---|
| `-d` | Specify a delimiter |
| `-s` | Combine all lines of each file serially |

### Examples

Suppose `names.txt` contains:

```text
Prathyusha
Rahul
Kiran
```

And `departments.txt` contains:

```text
DevOps
Testing
Development
```

Combine both files:

```bash
paste names.txt departments.txt
```

Output:

```text
Prathyusha    DevOps
Rahul         Testing
Kiran         Development
```

Use a comma as the delimiter:

```bash
paste -d ',' names.txt departments.txt
```

Combine all lines into one line:

```bash
paste -s names.txt
```

### Practical Use

Combining separate lists of employee names and departments into a tab-separated or comma-separated output.

---

## 12. sort Command

The `sort` command sorts lines in a text file.

By default, it sorts alphabetically according to the current locale.

Syntax:

```bash
sort [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-r` | Reverse sorting order |
| `-n` | Sort numerically |
| `-h` | Sort human-readable numbers |
| `-u` | Sort and remove duplicate lines |
| `-k` | Sort based on a specified field or key |
| `-t` | Specify a field delimiter |
| `-f` | Ignore case differences |
| `-o` | Write output to a specified file |

### Examples

Sort alphabetically:

```bash
sort names.txt
```

Sort in reverse order:

```bash
sort -r names.txt
```

Sort numbers numerically:

```bash
sort -n numbers.txt
```

Sort and remove duplicate lines:

```bash
sort -u names.txt
```

Sort without case sensitivity:

```bash
sort -f names.txt
```

Save sorted output:

```bash
sort names.txt -o sorted-names.txt
```

### Sorting by a Specific Field

Suppose `employees.txt` contains:

```text
101:Prathyusha:50000
102:Rahul:40000
103:Kiran:60000
```

Sort according to the third field:

```bash
sort -t ':' -k 3,3n employees.txt
```

Explanation:

- `-t ':'` sets the delimiter.
- `-k 3,3` selects the third field.
- `n` performs numerical sorting.

### Practical Use

Sorting log records, employee information, IP addresses, and numerical reports.

---

## 13. uniq Command

The `uniq` command identifies or removes adjacent duplicate lines.

Syntax:

```bash
uniq [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-c` | Count repeated adjacent lines |
| `-d` | Display only duplicate lines |
| `-u` | Display only unique lines |
| `-i` | Ignore case differences |

### Examples

Remove adjacent duplicates:

```bash
uniq names.txt
```

Count duplicate occurrences:

```bash
uniq -c names.txt
```

Display only duplicate lines:

```bash
uniq -d names.txt
```

Display only non-repeated lines:

```bash
uniq -u names.txt
```

### Important: sort and uniq Together

`uniq` only compares adjacent lines. Therefore, it is common to combine `sort` and `uniq`.

Example:

```bash
sort names.txt | uniq
```

Count occurrences of each line:

```bash
sort names.txt | uniq -c
```

### Practical DevOps Scenario

Find the most frequently occurring IP addresses in an Nginx access log:

```bash
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -10
```

This command:

1. Extracts the first field, usually the client IP address in a standard Nginx access-log format.
2. Sorts the IP addresses.
3. Counts repeated IP addresses.
4. Sorts the counts numerically in descending order.
5. Displays the top 10 entries.

---

## 14. wc Command

The `wc` command counts lines, words, bytes, and characters.

Syntax:

```bash
wc [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |
| `-L` | Display the maximum line length |

### Examples

Count lines:

```bash
wc -l file.txt
```

Count words:

```bash
wc -w file.txt
```

Count bytes:

```bash
wc -c file.txt
```

Count characters:

```bash
wc -m file.txt
```

Find the maximum line length:

```bash
wc -L file.txt
```

Count the number of files in a directory:

```bash
ls -1 | wc -l
```

**Note:** `wc -l` counts newline characters. A final line without a terminating newline may not be counted as an additional line.

### Practical Use

Count how many error messages exist in a log file:

```bash
grep -i "error" application.log | wc -l
```

---

## 15. tr Command

The `tr` command translates, deletes, or squeezes characters from standard input.

Syntax:

```bash
tr [options] SET1 [SET2]
```

### Common Options

| Option | Description |
|---|---|
| `-d` | Delete specified characters |
| `-s` | Squeeze repeated characters |
| `-c` | Use the complement of SET1 |
| `-t` | Truncate SET1 to the length of SET2 |

### Examples

Convert lowercase to uppercase:

```bash
echo "hello linux" | tr 'a-z' 'A-Z'
```

Output:

```text
HELLO LINUX
```

Convert uppercase to lowercase:

```bash
echo "LINUX" | tr 'A-Z' 'a-z'
```

Delete digits:

```bash
echo "Linux123" | tr -d '0-9'
```

Output:

```text
Linux
```

Remove repeated spaces:

```bash
echo "Linux    DevOps" | tr -s ' '
```

Replace spaces with underscores:

```bash
echo "Linux DevOps" | tr ' ' '_'
```

### Practical Use

Convert a list of names to uppercase:

```bash
cat names.txt | tr 'a-z' 'A-Z'
```

---

## 16. sed Command

The `sed` command stands for Stream Editor.

It is used to search, replace, delete, and modify text in a stream or file.

Syntax:

```bash
sed [options] 'command' filename
```

### Common Operations

| Operation | Description |
|---|---|
| `s` | Substitute text |
| `d` | Delete matching lines |
| `p` | Print selected lines |
| `-n` | Suppress automatic printing |
| `-i` | Edit a file in place |

### 16.1 Replace a Word

Example:

```bash
sed 's/Linux/Unix/' file.txt
```

This replaces the first occurrence of `Linux` with `Unix` on each line.

### 16.2 Replace All Occurrences in Each Line

```bash
sed 's/Linux/Unix/g' file.txt
```

The `g` flag replaces all occurrences on each line.

### 16.3 Ignore Case While Replacing

```bash
sed 's/linux/unix/gi' file.txt
```

The `i` flag makes the match case-insensitive.

### 16.4 Delete a Specific Line

```bash
sed '3d' file.txt
```

Deletes the third line from the output.

### 16.5 Delete Empty Lines

```bash
sed '/^$/d' file.txt
```

### 16.6 Display a Specific Line

```bash
sed -n '5p' file.txt
```

Displays only the fifth line.

### 16.7 Display a Range of Lines

```bash
sed -n '2,5p' file.txt
```

Displays lines 2 through 5.

### 16.8 Modify a File Directly

```bash
sed -i 's/Linux/Unix/g' file.txt
```

This directly modifies the file.

**Important:** The `-i` option changes the original file. Always keep a backup when modifying important configuration files.

Create a backup before editing:

```bash
sed -i.bak 's/Linux/Unix/g' file.txt
```

This creates `file.txt.bak` before modifying the original file.

### Practical DevOps Scenario

Suppose an application configuration contains:

```text
APP_ENV=development
```

Change the environment value:

```bash
sed -i 's/APP_ENV=development/APP_ENV=production/' config.txt
```

This is a simple example of modifying configuration text. In production, verify the target file and configuration format before making changes.

---

## 17. awk Command

The `awk` command is a powerful text-processing utility used for pattern scanning, field extraction, calculations, and report generation.

It is particularly useful for processing structured text such as logs and delimiter-separated files.

Syntax:

```bash
awk 'pattern { action }' filename
```

### Important Concepts

- `$0` represents the entire current line.
- `$1` represents the first field.
- `$2` represents the second field.
- `$3` represents the third field.
- `NF` represents the number of fields in the current record.
- `NR` represents the current record number.
- `FS` represents the input field separator.
- `OFS` represents the output field separator.

By default, `awk` separates fields using whitespace.

### 17.1 Print the Entire Line

```bash
awk '{print $0}' file.txt
```

### 17.2 Print the First Column

```bash
awk '{print $1}' file.txt
```

### 17.3 Print the Second Column

```bash
awk '{print $2}' file.txt
```

### 17.4 Print Multiple Columns

```bash
awk '{print $1, $3}' file.txt
```

### 17.5 Print Line Numbers

```bash
awk '{print NR, $0}' file.txt
```

### 17.6 Print the Last Column

```bash
awk '{print $NF}' file.txt
```

### 17.7 Print Lines Based on a Condition

Display lines where the third field is greater than 5000:

```bash
awk '$3 > 5000 {print $0}' employees.txt
```

### 17.8 Change the Field Separator

Suppose a file contains:

```text
101:Prathyusha:DevOps
102:Rahul:Testing
103:Kiran:Development
```

Extract the first and third fields:

```bash
awk -F ':' '{print $1, $3}' employees.txt
```

### 17.9 Print a Specific Line

```bash
awk 'NR == 3 {print}' file.txt
```

### 17.10 Count the Number of Lines

```bash
awk 'END {print NR}' file.txt
```

### 17.11 Calculate the Sum of a Column

Suppose `salary.txt` contains:

```text
50000
40000
60000
```

Calculate the total:

```bash
awk '{sum += $1} END {print sum}' salary.txt
```

### 17.12 Calculate the Average

```bash
awk '{sum += $1} END {if (NR > 0) print sum/NR}' salary.txt
```

### 17.13 Print Lines Containing a Word

```bash
awk '/ERROR/ {print}' application.log
```

### Practical DevOps Scenario

Extract the HTTP status codes from a standard Nginx access log:

```bash
awk '{print $9}' /var/log/nginx/access.log
```

Count requests by HTTP status code:

```bash
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr
```

**Note:** The field positions depend on the actual log format. The above example assumes a conventional Nginx combined access-log format.

---

## 18. tee Command

The `tee` command reads data from standard input and writes it to both standard output and one or more files.

Syntax:

```bash
command | tee filename
```

### Examples

Save command output into a file while displaying it:

```bash
ls | tee output.txt
```

Append output to an existing file:

```bash
date | tee -a output.txt
```

Save output from a command:

```bash
ps aux | tee process-list.txt
```

Use `tee` with elevated privileges to write to a protected file:

```bash
echo "Test entry" | sudo tee -a /var/log/test.log
```

This is useful because shell redirection such as `sudo echo "text" >> file` does not normally elevate the permissions of the shell performing the redirection.

### Practical Use

Capture application command output for troubleshooting while also displaying it in the terminal.

---

## 19. xargs Command

The `xargs` command reads items from standard input and passes them as arguments to another command.

Syntax:

```bash
command1 | xargs command2
```

### Examples

Pass filenames to `echo`:

```bash
cat files.txt | xargs echo
```

Find all `.log` files and pass them to `wc`:

```bash
find . -name "*.log" -print0 | xargs -0 wc -l
```

Delete temporary files safely using null-delimited filenames:

```bash
find /tmp -type f -name "*.tmp" -print0 | xargs -0 -r rm --
```

Explanation:

- `find` searches for matching files.
- `-print0` separates filenames using null characters.
- `xargs -0` reads null-delimited filenames.
- `-r` prevents running the command if no input is received on GNU `xargs`.
- `rm --` treats following arguments as filenames, including names beginning with a hyphen.

### Important Points

- `xargs` is useful for passing input to commands that do not read filenames from standard input.
- `-0` is recommended when processing filenames that may contain spaces or special characters.
- Always verify the files selected before running destructive commands.

---

## 20. diff Command

The `diff` command compares two files and displays their differences.

Syntax:

```bash
diff [options] file1 file2
```

### Common Options

| Option | Description |
|---|---|
| `-u` | Display unified format |
| `-c` | Display context format |
| `-q` | Report only whether files differ |
| `-y` | Display differences side by side |
| `-r` | Compare directories recursively |

### Examples

Compare two files:

```bash
diff file1.txt file2.txt
```

Display differences in unified format:

```bash
diff -u file1.txt file2.txt
```

Check whether files differ:

```bash
diff -q file1.txt file2.txt
```

Compare files side by side:

```bash
diff -y file1.txt file2.txt
```

### Practical DevOps Scenario

Compare two configuration files:

```bash
diff -u nginx.conf nginx.conf.backup
```

This helps identify changes made to configuration files.

---

## 21. Combining Text-Processing Commands

Linux commands become especially powerful when combined using pipes and redirection.

The following examples are useful for DevOps interviews and practical server administration.

### Scenario 1: Find the Number of Errors

```bash
grep -i "error" application.log | wc -l
```

Counts matching error lines.

### Scenario 2: Find the Number of Failed Login Messages

```bash
grep -i "failed" /var/log/auth.log | wc -l
```

Counts matching failed-login log lines.

The log path may differ between Linux distributions.

### Scenario 3: Find the Top 10 IP Addresses

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```

Finds the most frequently occurring client IP addresses in a standard access-log format.

### Scenario 4: Find HTTP 404 Requests

```bash
awk '$9 == 404 {print $0}' access.log
```

Displays requests with HTTP status code 404 in a conventional Nginx combined log format.

### Scenario 5: Find HTTP 500 Errors

```bash
awk '$9 == 500 {print $0}' access.log
```

Displays server error responses with status code 500.

### Scenario 6: Search for a Specific Application Error

```bash
grep -in "connection refused" application.log
```

Finds connection-refused messages without considering letter case and displays line numbers.

### Scenario 7: Monitor Application Errors in Real Time

```bash
tail -f application.log | grep --line-buffered -i "error"
```

Displays new log lines containing errors as they are written.

### Scenario 8: Extract Usernames from the passwd File

```bash
cut -d ':' -f 1 /etc/passwd
```

Extracts the first field, which contains usernames.

### Scenario 9: Count Unique Error Messages

```bash
grep -i "error" application.log | sort | uniq -c
```

Counts repeated, identical matching lines.

### Scenario 10: Save Command Output and Display It

```bash
df -h | tee disk-usage.txt
```

Displays disk usage and saves the output.

---

## 22. Important Linux Log Files

Linux systems store system and application information in log files.

Some commonly used locations include:

| Log File | Purpose |
|---|---|
| `/var/log/syslog` | General system messages on many Debian/Ubuntu systems |
| `/var/log/messages` | General system messages on many RHEL-family systems |
| `/var/log/auth.log` | Authentication-related logs on Debian/Ubuntu |
| `/var/log/secure` | Authentication-related logs on RHEL-family systems |
| `/var/log/kern.log` | Kernel-related messages on systems that maintain this file |
| `/var/log/boot.log` | Boot-related messages where available |
| `/var/log/nginx/access.log` | Nginx access logs, when configured with this path |
| `/var/log/nginx/error.log` | Nginx error logs, when configured with this path |

**Important:** Log locations depend on the distribution and service configuration. Some modern systems primarily use systemd's journal instead of storing every log in traditional text files.

Useful command:

```bash
journalctl
```

Display recent system logs:

```bash
journalctl -n 50
```

Follow logs in real time:

```bash
journalctl -f
```

Display logs for a particular service:

```bash
journalctl -u nginx
```

These commands are covered in more detail in the later Logs and Monitoring section of this repository.

---

## 23. Common Command Comparison

| Command | Main Purpose |
|---|---|
| `cat` | Display and combine file contents |
| `less` | View files interactively |
| `more` | View files page by page |
| `head` | Display the beginning of a file |
| `tail` | Display the end of a file |
| `grep` | Search for patterns |
| `cut` | Extract characters or fields |
| `paste` | Combine lines from multiple files |
| `sort` | Sort lines |
| `uniq` | Identify adjacent duplicate lines |
| `wc` | Count lines, words, bytes and characters |
| `tr` | Translate, delete or squeeze characters |
| `sed` | Edit and transform text streams |
| `awk` | Process fields and generate reports |
| `tee` | Display and save command output |
| `xargs` | Convert standard input into command arguments |
| `diff` | Compare files |

---

## 24. Practical Exercises

Practice the following tasks in your Linux terminal.

Create a practice directory:

```bash
mkdir text-processing-practice
cd text-processing-practice
```

Create a sample log file:

```bash
cat > application.log <<'EOF'
INFO Application started
ERROR Database connection failed
INFO User logged in
WARNING High memory usage
ERROR Connection timeout
INFO Application running
ERROR Database connection failed
WARNING Disk usage high
INFO User logged out
ERROR Service unavailable
EOF
```

Create a sample employee file:

```bash
cat > employees.txt <<'EOF'
101:Prathyusha:DevOps:50000
102:Rahul:Testing:40000
103:Kiran:Development:60000
104:Anil:DevOps:55000
105:Priya:Testing:45000
EOF
```

### Beginner-Level Exercises

1. Display the complete application log.
2. Display the first five lines.
3. Display the last three lines.
4. Count the total number of lines.
5. Count the total number of words.
6. Search for all ERROR messages.
7. Display line numbers for ERROR messages.
8. Search for WARNING messages without considering case.
9. Extract employee IDs from `employees.txt`.
10. Extract employee names and departments.

### Intermediate-Level Exercises

11. Count the total number of ERROR lines.
12. Count the total number of WARNING lines.
13. Display all lines that do not contain INFO.
14. Sort employee records alphabetically.
15. Sort employee records according to salary in descending order.
16. Extract only the department names.
17. Convert all employee names to uppercase.
18. Replace ERROR with CRITICAL in the command output.
19. Display only the third line of the employee file.
20. Find the longest line in the employee file.

### Advanced-Level Exercises

21. Count the number of occurrences of each log message.
22. Extract all error messages and save them in `errors.txt`.
23. Monitor the application log continuously.
24. Display only employees earning more than 45000.
25. Calculate the total employee salary.
26. Calculate the average employee salary.
27. Compare two configuration files.
28. Find the most frequently occurring error message.
29. Save command output to a file while displaying it.
30. Combine `grep`, `sort`, `uniq`, and `wc` to analyze application logs.

---

## 25. Important Interview Questions and Answers

### Q1. What is text processing in Linux?

Text processing is the process of searching, filtering, extracting, modifying, sorting, and analyzing text data using Linux commands.

Examples include `grep`, `sed`, `awk`, `cut`, and `sort`.

### Q2. What is the difference between grep, sed, and awk?

- `grep` searches for matching text or patterns.
- `sed` modifies and transforms text streams.
- `awk` processes structured text, extracts fields, performs calculations, and generates reports.

### Q3. What is a pipe in Linux?

A pipe connects two or more commands by passing the standard output of one command as the standard input of another command.

Example:

```bash
ls | wc -l
```

### Q4. What is the difference between > and >>?

- `>` redirects output and overwrites existing file contents.
- `>>` appends output to an existing file without overwriting its contents.

### Q5. What is the difference between stdin, stdout, and stderr?

- stdin is standard input and uses file descriptor 0.
- stdout is standard output and uses file descriptor 1.
- stderr is standard error and uses file descriptor 2.

### Q6. What is the difference between head and tail?

- `head` displays the beginning of a file.
- `tail` displays the end of a file.

Both display 10 lines by default.

### Q7. How do you monitor logs in real time?

Use:

```bash
tail -f application.log
```

It continuously displays new lines added to the file.

### Q8. What is the difference between tail -f and tail -F?

`tail -f` follows the file descriptor, while `tail -F` follows the filename and retries when the file is replaced or becomes unavailable.

### Q9. What is the purpose of grep -i?

The `-i` option performs a case-insensitive search.

Example:

```bash
grep -i "error" application.log
```

It matches ERROR, Error, error, and other case variations.

### Q10. What is the difference between grep -r and grep -R?

Both search recursively through directories.

- `-r` recursively searches directories.
- `-R` recursively searches and follows symbolic links.

### Q11. What is the purpose of cut?

The `cut` command extracts selected characters, bytes, or fields from text.

Example:

```bash
cut -d ':' -f 1 /etc/passwd
```

### Q12. What is the difference between sort and uniq?

- `sort` arranges lines in a specified order.
- `uniq` identifies or removes adjacent duplicate lines.

They are commonly combined:

```bash
sort file.txt | uniq
```

### Q13. What is the purpose of wc?

The `wc` command counts lines, words, bytes, and characters.

Example:

```bash
wc -l file.txt
```

### Q14. What is the difference between sed and awk?

`sed` is mainly used for stream editing and text substitution, while `awk` is designed for pattern scanning, field extraction, calculations, and report generation.

### Q15. What is the purpose of sed -i?

It modifies a file directly instead of displaying only the modified output.

Example:

```bash
sed -i 's/old/new/g' file.txt
```

### Q16. What is the difference between awk $0 and awk $1?

- `$0` represents the entire current record or line.
- `$1` represents the first field.

### Q17. What is NF in awk?

`NF` represents the number of fields in the current record.

Example:

```bash
awk '{print NF}' file.txt
```

### Q18. What is NR in awk?

`NR` represents the current record number.

Example:

```bash
awk '{print NR, $0}' file.txt
```

### Q19. What is the purpose of tee?

The `tee` command displays command output on the terminal while also writing it to a file.

Example:

```bash
ls | tee output.txt
```

### Q20. What is the purpose of xargs?

`xargs` reads items from standard input and passes them as arguments to another command.

Example:

```bash
find . -name "*.log" -print0 | xargs -0 wc -l
```

### Q21. How do you find all ERROR messages in a log file?

```bash
grep -i "error" application.log
```

### Q22. How do you count the number of failed login messages?

```bash
grep -i "failed" /var/log/auth.log | wc -l
```

The log location depends on the distribution.

### Q23. How do you find the top 10 IP addresses accessing a web server?

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```

This assumes the first field in the access log contains the client IP address.

### Q24. How do you find HTTP 500 errors in an Nginx access log?

```bash
awk '$9 == 500 {print $0}' /var/log/nginx/access.log
```

This assumes a conventional Nginx combined access-log format.

### Q25. How do you save command output into a file without overwriting the existing content?

Use append redirection:

```bash
command >> output.txt
```

Alternatively:

```bash
command | tee -a output.txt
```

---

## 26. Important Points to Remember for DevOps Interviews

1. `grep` is used for searching text and patterns.
2. `sed` is used for stream editing and substitution.
3. `awk` is used for field extraction, calculations, and report generation.
4. `cut` extracts selected fields or characters.
5. `sort` arranges data in order.
6. `uniq` identifies adjacent duplicate lines.
7. `wc -l` counts newline characters.
8. `head` displays the first lines of a file.
9. `tail -f` monitors log updates continuously.
10. `tail -F` follows a filename across file replacement.
11. `>` overwrites output files.
12. `>>` appends output to files.
13. `|` connects commands through standard output and input.
14. `2>` redirects error messages.
15. `tee` displays and saves output simultaneously.
16. `xargs` passes standard input as command arguments.
17. `diff` compares two files.
18. `journalctl` reads systemd journal logs.

### Most Important Commands to Practice

```bash
grep -i "error" application.log
grep -n "ERROR" application.log
grep -v "INFO" application.log
head -n 10 application.log
tail -n 10 application.log
tail -f application.log
cut -d ':' -f 1 /etc/passwd
sort names.txt
sort names.txt | uniq -c
wc -l application.log
tr 'a-z' 'A-Z'
sed 's/old/new/g' file.txt
awk '{print $1}' file.txt
awk 'END {print NR}' file.txt
tee output.txt
find . -name "*.log" -print0 | xargs -0 wc -l
diff -u file1.txt file2.txt
```

---

## Conclusion

Linux text-processing commands are essential for system administration, server troubleshooting, log analysis, configuration management, and DevOps automation.

A DevOps engineer should understand how to combine commands using pipes, redirection, and shell utilities to analyze large amounts of information efficiently.

The most important commands to master are:

- `grep`
- `sed`
- `awk`
- `cut`
- `sort`
- `uniq`
- `wc`
- `head`
- `tail`
- `tee`
- `xargs`

Practice these commands regularly using real log files and configuration files to develop practical Linux troubleshooting skills.
The list of files and directories is displayed on the terminal.

### 2.3 Standard Error (stderr)

Standard error is used to display error messages generated by commands.

- File descriptor: `2`
- Default destination: Terminal

Example:

```bash
ls /nonexistent-directory
```

Linux displays an error because the directory does not exist.

### Summary

| Stream | File Descriptor | Purpose |
|---|---:|---|
| stdin | 0 | Input |
| stdout | 1 | Normal output |
| stderr | 2 | Error output |

---

## 3. Input and Output Redirection

Redirection is used to change the default input or output destination of a command.

### 3.1 Output Redirection (`>`)

The `>` operator redirects standard output into a file.

Syntax:

```bash

command > filename
```

Example:

```bash
ls > files.txt
```

This stores the output of `ls` in `files.txt`.

**Important:** If the file already exists, its previous contents are overwritten.

### 3.2 Append Redirection (`>>`)

The `>>` operator appends output to the end of a file without removing existing content.

Syntax:

```bash
command >> filename
```

Example:

```bash
date >> system-info.txt
```

This appends the current date and time to the file.

### 3.3 Input Redirection (`<`)

The `<` operator takes input from a file instead of the keyboard.

Syntax:

```bash
command < filename
```

Example:

```bash
wc -l < file.txt
```

This counts the number of lines in the file using standard input.

### 3.4 Error Redirection (`2>`)

The `2>` operator redirects standard error into a file.

Example:

```bash
ls /wrong-path 2> error.log
```

The error message is stored in `error.log`.

### 3.5 Redirecting Both Output and Errors

To redirect standard output and standard error into the same file:

```bash
command > output.log 2>&1
```

Example:

```bash
ls /home /wrong-path > result.log 2>&1
```

This stores both normal output and error messages in `result.log`.

A commonly used Bash shortcut is:

```bash
command &> output.log
```

This redirects both standard output and standard error to the same file in Bash.

### 3.6 Discarding Output

`/dev/null` is a special device that discards anything written to it.

Example:

```bash
command > /dev/null
```

To discard both normal output and errors:

```bash
command > /dev/null 2>&1
```

This is useful when running commands in scripts where output is unnecessary.

---

## 4. Pipes (`|`)

A pipe is used to send the standard output of one command as the standard input of another command.

Syntax:

```bash
command1 | command2
```

Example:

```bash
ls | wc -l
```

This counts the number of lines produced by `ls`.

Another example:

```bash
ps aux | grep nginx
```

This searches the process list for entries containing `nginx`.

### Multiple Pipes

We can connect multiple commands using pipes.

Example:

```bash
cat application.log | grep "ERROR" | wc -l
```

This performs three operations:

1. Reads the application log.
2. Filters lines containing `ERROR`.
3. Counts the matching lines.

A more efficient version is:

```bash
grep "ERROR" application.log | wc -l
```

### Important Points

- A pipe is represented by `|`.
- The output of one command becomes the input of another.
- Pipes help combine simple commands into powerful operations.
- By default, pipes transfer standard output, not standard error.

---

## 5. cat Command

The `cat` command is used to display, combine, and create text-file contents.

Syntax:

```bash
cat [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-n` | Display line numbers for all lines |
| `-b` | Number non-empty lines |
| `-s` | Suppress repeated empty lines |
| `-A` | Display non-printing characters and line endings |

### Examples

Display file contents:

```bash
cat file.txt
```

Display line numbers:

```bash
cat -n file.txt
```

Number only non-empty lines:

```bash
cat -b file.txt
```

Combine two files:

```bash
cat file1.txt file2.txt
```

Combine files and save the result:

```bash
cat file1.txt file2.txt > combined.txt
```

Create a file using standard input:

```bash
cat > newfile.txt
```

Type the contents and press `Ctrl + D` to save and finish.

### Practical Use

```bash
cat /etc/os-release
```

Displays Linux distribution identification information.

**Note:** `cat` is suitable for displaying small files. For large files, use `less`.

---

## 6. less and more Commands

These commands are used to view large text files page by page.

### 6.1 less Command

The `less` command allows users to navigate through a file without loading the entire output into the terminal at once.

Syntax:

```bash
less filename
```

Example:

```bash
less /var/log/syslog
```

Useful keyboard shortcuts:
### 3.4 Error Redirection (`2>`)

| Key | Function |
|---|---|
| `Space` | Move forward one screen |
| `b` | Move backward one screen |
| `Enter` | Move down one line |
| `g` | Go to the beginning |
| `G` | Go to the end |
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Find next match |
| `N` | Find previous match |
| `q` | Quit |

### 6.2 more Command

The `more` command displays text one screen at a time.

Syntax:

```bash
more filename
```

Example:

```bash
more largefile.txt
```

Common keyboard shortcuts:

| Key | Function |
|---|---|
| `Space` | Next screen |
| `Enter` | Next line |
| `q` | Quit |

### Difference Between less and more

| Feature | less | more |
|---|---|---|
| Purpose | View text page by page | View text page by page |
| Backward navigation | Supported | Limited |
| Searching | Supported | Supported |
| Large files | Suitable | Suitable |
| Navigation | More flexible | More limited |

---

## 7. head Command

The `head` command displays the first lines of a file.

By default, it displays the first 10 lines.

Syntax:

```bash
head [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-n` | Specify the number of lines |
| `-c` | Display a specified number of bytes |
| `-q` | Do not display filenames in multi-file output |
| `-v` | Always display filenames |

### Examples

Display the first 10 lines:

```bash
head file.txt
```

Display the first 5 lines:

```bash
head -n 5 file.txt
```

Display the first 20 lines:

```bash
head -n 20 file.txt
```

Display the first 50 bytes:

```bash
head -c 50 file.txt
```

### Practical Scenario

To inspect the beginning of an application log:

```bash
head -n 10 application.log
```

---

## 8. tail Command

The `tail` command displays the last lines of a file.

By default, it displays the last 10 lines.

Syntax:

```bash
tail [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-n` | Specify the number of lines |
| `-c` | Display a specified number of bytes |
| `-f` | Continuously monitor file updates |
| `-F` | Follow a file by name and retry if it is replaced |
| `-q` | Suppress filenames in multi-file output |
| `-v` | Always display filenames |

### Examples

Display the last 10 lines:

```bash
tail file.txt
```

Display the last 5 lines:

```bash
tail -n 5 file.txt
```

Display the last 20 lines:

```bash
tail -n 20 application.log
```

### Live Log Monitoring

```bash
tail -f application.log
```

This continuously displays new lines added to the file.

Press `Ctrl + C` to stop monitoring.

### Difference Between tail -f and tail -F

- `tail -f` follows the file descriptor. If the file is replaced during log rotation, it may continue following the old file.
- `tail -F` follows the filename and retries if the file is replaced or temporarily unavailable.

### Practical DevOps Scenario

When an application is running on a server, developers and DevOps engineers can monitor logs in real time:

```bash
tail -f /var/log/nginx/access.log
```

This helps observe incoming HTTP requests.

For Nginx, log paths can vary depending on the distribution and configuration.

---

## 9. grep Command

The `grep` command is used to search for specific words, patterns, or strings inside files or command output.

The name comes from Global Regular Expression Print.

Syntax:

```bash
grep [options] "pattern" filename
```

### Common Options

| Option | Description |
|---|---|
| `-i` | Ignore uppercase and lowercase differences |
| `-n` | Display matching line numbers |
| `-v` | Display lines that do not match |
| `-r` | Search recursively in directories |
| `-R` | Search recursively and follow symbolic links |
| `-c` | Count matching lines |
| `-l` | Display only filenames containing matches |
| `-w` | Match complete words |
| `-o` | Display only matching portions |
| `-E` | Use extended regular expressions |
| `-A n` | Display n lines after a match |
| `-B n` | Display n lines before a match |
| `-C n` | Display n lines before and after a match |

### Examples

Search for a word:

```bash
grep "error" application.log
```

Case-insensitive search:

```bash
grep -i "error" application.log
```

Display line numbers:

```bash
grep -n "error" application.log
```

Display lines that do not contain a word:

```bash
grep -v "success" application.log
```

Count matching lines:

```bash
grep -c "ERROR" application.log
```

Search recursively:

```bash
grep -r "password" /etc
```

Display filenames containing a match:

```bash
grep -l "ERROR" *.log
```

Search for a complete word:

```bash
grep -w "root" file.txt
```

Display three lines after a matching line:

```bash
grep -A 3 "ERROR" application.log
```

Display three lines before a matching line:

```bash
grep -B 3 "ERROR" application.log
```

Display three lines before and after a matching line:

```bash
grep -C 3 "ERROR" application.log
```

### grep with Pipes

Search for Nginx processes:

```bash
ps aux | grep nginx
```

Search for failed SSH login messages:

```bash
grep -i "failed" /var/log/auth.log
```


The SSH authentication log location differs between distributions. For example, Debian and Ubuntu commonly use `/var/log/auth.log`, while RHEL-family systems commonly use `/var/log/secure`. Some systems use the systemd journal instead.

### Basic Regular Expressions

Regular expressions are patterns used to identify matching text.

| Pattern | Meaning |
|---|---|
| `^word` | Lines beginning with word |
| `word$` | Lines ending with word |
| `.` | Any single character |
| `*` | Zero or more occurrences of the preceding expression |
| `[abc]` | Any one character among a, b, or c |
| `[0-9]` | Any digit |
| `[^0-9]` | Any non-digit |
| `^$` | Empty line |

Examples:

Find lines starting with ERROR:

```bash
grep "^ERROR" application.log
```

Find lines ending with failed:

```bash
grep "failed$" application.log
```

Find lines containing digits:

```bash
grep "[0-9]" file.txt
```

Find empty lines:

```bash
grep "^$" file.txt
```

**Important:** Basic `grep` uses basic regular expressions. Options such as `-E` enable extended regular expressions, which support operators such as `+`, `?`, and `|` without requiring backslashes.

---

## 10. cut Command

The `cut` command extracts specific characters, bytes, or fields from each line of a file.

It is commonly used to extract columns from structured text.

Syntax:

```bash
cut [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-c` | Select character positions |
| `-b` | Select byte positions |
| `-d` | Specify a field delimiter |
| `-f` | Select field numbers |
| `--complement` | Select everything except the specified positions or fields |

### Examples

Extract the first five characters:

```bash
cut -c 1-5 file.txt
```

Extract the first field:

```bash
cut -d ':' -f 1 /etc/passwd
```

Here:

- `-d ':'` specifies the colon as the delimiter.
- `-f 1` selects the first field.

Extract the first and third fields:

```bash
cut -d ':' -f 1,3 /etc/passwd
```

Extract fields from a CSV file:

```bash
cut -d ',' -f 1,2 employees.csv
```

Extract everything except the first field:

```bash
cut -d ':' -f 1 --complement /etc/passwd
```

**Important:** `cut` works with a specified delimiter. It does not automatically parse every CSV format correctly, especially files containing quoted delimiters or complex escaping.

---

## 11. paste Command

The `paste` command combines lines from multiple files side by side.

Syntax:

```bash
paste [options] file1 file2
```

### Common Options

| Option | Description |
|---|---|
| `-d` | Specify a delimiter |
| `-s` | Combine all lines of each file serially |

### Examples

Suppose `names.txt` contains:

```text
Prathyusha
Rahul
Kiran
```

And `departments.txt` contains:

```text
DevOps
Testing
Development
```

Combine both files:

```bash
paste names.txt departments.txt
```

Output:

```text
Prathyusha    DevOps
Rahul         Testing
Kiran         Development
```
This counts the number of lines in the file using standard input.

Use a comma as the delimiter:

```bash
paste -d ',' names.txt departments.txt
```

Combine all lines into one line:

```bash
paste -s names.txt
```

### Practical Use

Combining separate lists of employee names and departments into a tab-separated or comma-separated output.

---

## 12. sort Command

The `sort` command sorts lines in a text file.

By default, it sorts alphabetically according to the current locale.

Syntax:

```bash
sort [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-r` | Reverse sorting order |
| `-n` | Sort numerically |
| `-h` | Sort human-readable numbers |
| `-u` | Sort and remove duplicate lines |
| `-k` | Sort based on a specified field or key |
| `-t` | Specify a field delimiter |
| `-f` | Ignore case differences |
| `-o` | Write output to a specified file |

### Examples

Sort alphabetically:

```bash
sort names.txt
```

Sort in reverse order:

```bash
sort -r names.txt
```

Sort numbers numerically:

```bash
sort -n numbers.txt
```

Sort and remove duplicate lines:

```bash
sort -u names.txt
```

Sort without case sensitivity:

```bash
sort -f names.txt
```

Save sorted output:

```bash
sort names.txt -o sorted-names.txt
```

### Sorting by a Specific Field

Suppose `employees.txt` contains:

```text
101:Prathyusha:50000
102:Rahul:40000
103:Kiran:60000
```

Sort according to the third field:

```bash
sort -t ':' -k 3,3n employees.txt
```

Explanation:

- `-t ':'` sets the delimiter.
- `-k 3,3` selects the third field.
- `n` performs numerical sorting.

### Practical Use

Sorting log records, employee information, IP addresses, and numerical reports.

---

## 13. uniq Command

The `uniq` command identifies or removes adjacent duplicate lines.

Syntax:

```bash
uniq [options] filename
```

### Common Options

| Option | Description |
|---|---|
| `-c` | Count repeated adjacent lines |
| `-d` | Display only duplicate lines |
| `-u` | Display only unique lines |
| `-i` | Ignore case differences |

### Examples

Remove adjacent duplicates:

```bash
uniq names.txt
```

Count duplicate occurrences:

```bash
uniq -c names.txt
```

Display only duplicate lines:

```bash
uniq -d names.txt
```

Display only non-repeated lines:

```bash
uniq -u names.txt
```

### Important: sort and uniq Together

`uniq` only compares adjacent lines. Therefore, it is common to combine `sort` and `uniq`.

Example:

```bash
sort names.txt | uniq
```

Count occurrences of each line:

```bash
sort names.txt | uniq -c
```

### Practical DevOps Scenario

Find the most frequently occurring IP addresses in an Nginx access log:

```bash
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -10
```

This command:

1. Extracts the first field, usually the client IP address in a standard Nginx access-log format.
2. Sorts the IP addresses.
3. Counts repeated IP addresses.
4. Sorts the counts numerically in descending order.
5. Displays the top 10 entries.
```


---

## 14. wc Command

The `wc` command counts lines, words, bytes, and characters.

Syntax:
```bash
wc -l < file.txt

```bash
wc [options] filename
```


### Common Options

| Option | Description |
|---|---|
| `-l` | Count lines |
| `-w` | Count words |
| `-c` | Count bytes |
| `-m` | Count characters |
Example:
| `-L` | Display the maximum line length |

### Examples

Count lines:

```bash
wc -l file.txt
```

Count words:

```bash
wc -w file.txt
```

Count bytes:

```bash
wc -c file.txt
```


Count characters:

```bash
wc -m file.txt
```

Find the maximum line length:

```bash
wc -L file.txt
```

Count the number of files in a directory:

```bash
ls -1 | wc -l
```

**Note:** `wc -l` counts newline characters. A final line without a terminating newline may not be counted as an additional line.

### Practical Use

Count how many error messages exist in a log file:

```bash
grep -i "error" application.log | wc -l
```

---

## 15. tr Command

The `tr` command translates, deletes, or squeezes characters from standard input.

Syntax:

```bash
tr [options] SET1 [SET2]
```

### Common Options

| Option | Description |
|---|---|
| `-d` | Delete specified characters |
```
| `-s` | Squeeze repeated characters |
| `-c` | Use the complement of SET1 |
| `-t` | Truncate SET1 to the length of SET2 |

### Examples

Convert lowercase to uppercase:

```bash
echo "hello linux" | tr 'a-z' 'A-Z'
```

Output:

```text
HELLO LINUX
```

Convert uppercase to lowercase:

```bash
echo "LINUX" | tr 'A-Z' 'a-z'
```

Delete digits:

```bash
echo "Linux123" | tr -d '0-9'
```

Output:

```text
Linux
```

Remove repeated spaces:

```bash
echo "Linux    DevOps" | tr -s ' '
```

Replace spaces with underscores:

```bash
echo "Linux DevOps" | tr ' ' '_'
```

### Practical Use

Convert a list of names to uppercase:

```bash
cat names.txt | tr 'a-z' 'A-Z'
```

---

## 16. sed Command

The `sed` command stands for Stream Editor.

It is used to search, replace, delete, and modify text in a stream or file.

Syntax:

```bash
sed [options] 'command' filename
```

### Common Operations

| Operation | Description |
|---|---|
| `s` | Substitute text |
| `d` | Delete matching lines |
| `p` | Print selected lines |
| `-n` | Suppress automatic printing |
| `-i` | Edit a file in place |

### 16.1 Replace a Word

Example:

```bash
sed 's/Linux/Unix/' file.txt
```
command < filename

This replaces the first occurrence of `Linux` with `Unix` on each line.

### 16.2 Replace All Occurrences in Each Line

```bash
sed 's/Linux/Unix/g' file.txt
```

The `g` flag replaces all occurrences on each line.

### 16.3 Ignore Case While Replacing

```bash
sed 's/linux/unix/gi' file.txt
```

The `i` flag makes the match case-insensitive.

### 16.4 Delete a Specific Line

```bash
sed '3d' file.txt
```

Deletes the third line from the output.

### 16.5 Delete Empty Lines

```bash
sed '/^$/d' file.txt
```

### 16.6 Display a Specific Line

```bash
sed -n '5p' file.txt
```

Displays only the fifth line.

### 16.7 Display a Range of Lines

```bash
sed -n '2,5p' file.txt
```

Displays lines 2 through 5.

### 16.8 Modify a File Directly

```bash
sed -i 's/Linux/Unix/g' file.txt
```

This directly modifies the file.

**Important:** The `-i` option changes the original file. Always keep a backup when modifying important configuration files.

Create a backup before editing:

```bash
sed -i.bak 's/Linux/Unix/g' file.txt
```

This creates `file.txt.bak` before modifying the original file.

### Practical DevOps Scenario

Suppose an application configuration contains:

```text
APP_ENV=development
```

Change the environment value:

```bash
sed -i 's/APP_ENV=development/APP_ENV=production/' config.txt
```

This is a simple example of modifying configuration text. In production, verify the target file and configuration format before making changes.

---

## 17. awk Command

The `awk` command is a powerful text-processing utility used for pattern scanning, field extraction, calculations, and report generation.

It is particularly useful for processing structured text such as logs and delimiter-separated files.

Syntax:

```bash
awk 'pattern { action }' filename
```

### Important Concepts

- `$0` represents the entire current line.
- `$1` represents the first field.
- `$2` represents the second field.
- `$3` represents the third field.
- `NF` represents the number of fields in the current record.
- `NR` represents the current record number.
- `FS` represents the input field separator.
- `OFS` represents the output field separator.

By default, `awk` separates fields using whitespace.

### 17.1 Print the Entire Line

```bash
awk '{print $0}' file.txt
```

### 17.2 Print the First Column

```bash
```bash
awk '{print $1}' file.txt
```

### 17.3 Print the Second Column

```bash
awk '{print $2}' file.txt
```

### 17.4 Print Multiple Columns

```bash
awk '{print $1, $3}' file.txt
```

### 17.5 Print Line Numbers

```bash
awk '{print NR, $0}' file.txt
```

### 17.6 Print the Last Column

```bash
awk '{print $NF}' file.txt
```

### 17.7 Print Lines Based on a Condition

Display lines where the third field is greater than 5000:

```bash
awk '$3 > 5000 {print $0}' employees.txt
```

### 17.8 Change the Field Separator

Suppose a file contains:

```text
101:Prathyusha:DevOps
102:Rahul:Testing
103:Kiran:Development
```

Extract the first and third fields:

```bash
awk -F ':' '{print $1, $3}' employees.txt
```

### 17.9 Print a Specific Line

```bash
awk 'NR == 3 {print}' file.txt
```

### 17.10 Count the Number of Lines

```bash
awk 'END {print NR}' file.txt
```

### 17.11 Calculate the Sum of a Column

Suppose `salary.txt` contains:

```text
50000
40000
60000
```

Calculate the total:

```bash
awk '{sum += $1} END {print sum}' salary.txt
```

### 17.12 Calculate the Average

```bash
awk '{sum += $1} END {if (NR > 0) print sum/NR}' salary.txt
```

### 17.13 Print Lines Containing a Word

```bash
awk '/ERROR/ {print}' application.log
```

### Practical DevOps Scenario

Extract the HTTP status codes from a standard Nginx access log:

```bash
awk '{print $9}' /var/log/nginx/access.log
```

Count requests by HTTP status code:


```bash
awk '{print $9}' /var/log/nginx/access.log | sort | uniq -c | sort -nr
```

**Note:** The field positions depend on the actual log format. The above example assumes a conventional Nginx combined access-log format.

---

## 18. tee Command

The `tee` command reads data from standard input and writes it to both standard output and one or more files.

Syntax:

```bash
command | tee filename
```

### Examples

Save command output into a file while displaying it:

```bash
ls | tee output.txt
```

Append output to an existing file:

```bash
date | tee -a output.txt
```

Save output from a command:

```bash
ps aux | tee process-list.txt
```

Use `tee` with elevated privileges to write to a protected file:

```bash
echo "Test entry" | sudo tee -a /var/log/test.log
```

This is useful because shell redirection such as `sudo echo "text" >> file` does not normally elevate the permissions of the shell performing the redirection.

### Practical Use

Capture application command output for troubleshooting while also displaying it in the terminal.

---

## 19. xargs Command

The `xargs` command reads items from standard input and passes them as arguments to another command.

Syntax:


Syntax:
```bash
command1 | xargs command2
```

### Examples

Pass filenames to `echo`:

```bash
cat files.txt | xargs echo
```

Find all `.log` files and pass them to `wc`:

```bash
find . -name "*.log" -print0 | xargs -0 wc -l
```

Delete temporary files safely using null-delimited filenames:

```bash
find /tmp -type f -name "*.tmp" -print0 | xargs -0 -r rm --
```

Explanation:

- `find` searches for matching files.
- `-print0` separates filenames using null characters.
- `xargs -0` reads null-delimited filenames.
- `-r` prevents running the command if no input is received on GNU `xargs`.
- `rm --` treats following arguments as filenames, including names beginning with a hyphen.

### Important Points

- `xargs` is useful for passing input to commands that do not read filenames from standard input.
- `-0` is recommended when processing filenames that may contain spaces or special characters.
- Always verify the files selected before running destructive commands.

---

## 20. diff Command

The `diff` command compares two files and displays their differences.

Syntax:

```bash
diff [options] file1 file2
```

### Common Options

| Option | Description |
|---|---|
| `-u` | Display unified format |
| `-c` | Display context format |
| `-q` | Report only whether files differ |
| `-y` | Display differences side by side |
| `-r` | Compare directories recursively |

### Examples

Compare two files:

```bash
diff file1.txt file2.txt
```

Display differences in unified format:

```bash
diff -u file1.txt file2.txt
```

Check whether files differ:

```bash
diff -q file1.txt file2.txt
```

Compare files side by side:

```bash
diff -y file1.txt file2.txt
```

### Practical DevOps Scenario

Compare two configuration files:

```bash
diff -u nginx.conf nginx.conf.backup
```

This helps identify changes made to configuration files.

---

## 21. Combining Text-Processing Commands

Linux commands become especially powerful when combined using pipes and redirection.

The following examples are useful for DevOps interviews and practical server administration.

### Scenario 1: Find the Number of Errors

```bash
grep -i "error" application.log | wc -l
```

Counts matching error lines.

### Scenario 2: Find the Number of Failed Login Messages

```bash
grep -i "failed" /var/log/auth.log | wc -l
```

Counts matching failed-login log lines.

The log path may differ between Linux distributions.

### Scenario 3: Find the Top 10 IP Addresses

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -10
```

Finds the most frequently occurring client IP addresses in a standard access-log format.
The `<` operator takes input from a file instead of the keyboard.

### Scenario 4: Find HTTP 404 Requests

```bash
awk '$9 == 404 {print $0}' access.log
```

Displays requests with HTTP status code 404 in a conventional Nginx combined log format.

### Scenario 5: Find HTTP 500 Errors

```bash
awk '$9 == 500 {print $0}' access.log
```

Displays server error responses with status code 500.

### Scenario 6: Search for a Specific Application Error

```bash
grep -in "connection refused" application.log
```

Finds connection-refused messages without considering letter case and displays line numbers.


### Scenario 7: Monitor Application Errors in Real Time

```bash
tail -f application.log | grep --line-buffered -i "error"
```

```bash
ls > files.txt
```

This stores the output of `ls` in `files.txt`.

**Important:** If the file already exists, its previous contents are overwritten.

### 3.2 Append Redirection (`>>`)

The `>>` operator appends output to the end of a file without removing existing content.

Syntax:

```bash
command >> filename
```

Example:

```bash
date >> system-info.txt
```

This appends the current date and time to the file.

### 3.3 Input Redirection (`<`)
Displays new log lines containing errors as they are written.

### Scenario 8: Extract Usernames from the passwd File

```bash
cut -d ':' -f 1 /etc/passwd
```

Extracts the first field, which contains usernames.

### Scenario 9: Count Unique Error Messages

```bash
grep -i "error" application.log | sort | uniq -c
```

Counts repeated, identical matching lines.

### Scenario 10: Save Command Output and Display It

```bash
df -h | tee disk-usage.txt
```

Displays disk usage and saves the output.

---

## 22. Important Linux Log Files
Example:


Linux systems store system and application information in log files.

Some commonly used locations include:

| Log File | Purpose |
|---|---|
| `/var/log/syslog` | General system messages on many Debian/Ubuntu systems |
| `/var/log/messages` | General system messages on many RHEL-family systems |
| `/var/log/auth.log` | Authentication-related logs on Debian/Ubuntu |
| `/var/log/secure` | Authentication-related logs on RHEL-family systems |
| `/var/log/kern.log` | Kernel-related messages on systems that maintain this file |
| `/var/log/boot.log` | Boot-related messages where available |
| `/var/log/nginx/access.log` | Nginx access logs, when configured with this path |
| `/var/log/nginx/error.log` | Nginx error logs, when configured with this path |
```


**Important:** Log locations depend on the distribution and service configuration. Some modern systems primarily use systemd's journal instead of storing every log in traditional text files.

Useful command:

```bash
journalctl
```

Display recent system logs:

```bash
journalctl -n 50
```

Follow logs in real time:

```bash
journalctl -f
```bash
command > filename
```

Display logs for a particular service:

```bash
journalctl -u nginx
```

These commands are covered in more detail in the later Logs and Monitoring section of this repository.

---

## 23. Common Command Comparison

| Command | Main Purpose |
|---|---|
| `cat` | Display and combine file contents |
| `less` | View files interactively |
| `more` | View files page by page |
| `head` | Display the beginning of a file |
| `tail` | Display the end of a file |
| `grep` | Search for patterns |
| `cut` | Extract characters or fields |
| `paste` | Combine lines from multiple files |
| `sort` | Sort lines |
| `uniq` | Identify adjacent duplicate lines |
| `wc` | Count lines, words, bytes and characters |
| `tr` | Translate, delete or squeeze characters |
| `sed` | Edit and transform text streams |
| `awk` | Process fields and generate reports |
Syntax:

| `tee` | Display and save command output |
| `xargs` | Convert standard input into command arguments |
| `diff` | Compare files |

---

## 24. Practical Exercises

Practice the following tasks in your Linux terminal.

Create a practice directory:

```bash
mkdir text-processing-practice
cd text-processing-practice

```

Create a sample log file:

```bash
cat > application.log <<'EOF'
The `>` operator redirects standard output into a file.
INFO Application started
ERROR Database connection failed
INFO User logged in
WARNING High memory usage
ERROR Connection timeout
INFO Application running
ERROR Database connection failed
WARNING Disk usage high
INFO User logged out
# Linux Text Processing Commands

## 1. Introduction to Text Processing

Text processing in Linux refers to the process of reading, searching, filtering, extracting, modifying, sorting, and analyzing text data using command-line utilities.

Linux stores a large amount of information in text files, including:

- System logs
- Configuration files
- Application logs
- User information
- Network information
- Server monitoring data
- Error messages

Text-processing commands help administrators and DevOps engineers analyze this information efficiently without manually opening every file.

### Why is text processing important in DevOps?

Text processing is commonly used for:






```bash
ls
```

### 2.3 Standard Error (stderr)

Standard error is used to display error messages generated by commands.

- File descriptor: `2`
- Default destination: Terminal

Example:

```bash
ls /nonexistent-directory
```
### 3.1 Output Redirection (`>`)

Linux displays an error because the directory does not exist.

### Summary

| Stream | File Descriptor | Purpose |
|---|---:|---|
| stdin | 0 | Input |
| stdout | 1 | Normal output |
| stderr | 2 | Error output |

---

## 3. Input and Output Redirection

Redirection is used to change the default input or output destination of a command.


The list of files and directories is displayed on the terminal.
Example:
- Default destination: Terminal

- File descriptor: `1`
### 2.2 Standard Output (stdout)

Standard output is the normal output produced by a command.
Type some text and press `Ctrl + D` to finish.
Example:

```bash
cat
```
- Analyzing application logs.
- Finding errors in server logs.
- Searching for specific information in configuration files.
- Extracting required fields from files.
- File descriptor: `0`
- Default source: Keyboard

- Counting errors and warnings.
- Filtering unnecessary information.
- Automating tasks through shell scripts.
- Monitoring application and server health.

Example:

```bash

grep "ERROR" application.log
```
Standard input is the data provided to a command or program.
## 2. Standard Input, Standard Output and Standard Error

### 2.1 Standard Input (stdin)
Linux uses three standard data streams for communication between commands and processes.



This command searches for lines containing the word `ERROR` in `application.log`.

