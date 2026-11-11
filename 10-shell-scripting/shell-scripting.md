# Linux Shell Scripting

## 1. Introduction to Shell Scripting

Shell scripting is the process of writing a sequence of Linux commands in a file and executing them together using a shell interpreter.

A shell script helps automate repetitive tasks and reduces manual work.

### Why is Shell Scripting important?

Shell scripting is useful for:

1. Automating repetitive tasks.
2. Managing files and directories.
3. Monitoring system resources.
4. Managing Linux services.
5. Taking backups.
---

## 4. Shebang (#!)

The shebang is the first line of a script. It specifies which interpreter should execute the script when it is run directly.

Example:

```bash
#!/bin/bash
```

Other examples:

```bash
#!/bin/sh
```

```bash
#!/usr/bin/env bash
```

### Difference

- `#!/bin/bash` uses Bash from the specified path.
- `#!/bin/sh` uses the system's `sh` interpreter.
- `#!/usr/bin/env bash` searches for Bash using the environment's PATH.

**Important:** Bash-specific features may not work in other shells such as `sh`. Always use the appropriate interpreter for the script.

---

## 5. Creating and Executing a Shell Script

### Step 1: Create a script

```bash
nano hello.sh
```

### Step 2: Write the script

```bash
#!/bin/bash

echo "Hello World"
echo "My first shell script"
```

### Step 3: Save the file

Inside Nano:

- Press `Ctrl + O`.
- Press `Enter`.
- Press `Ctrl + X`.

### Step 4: Give execute permission

```bash
chmod +x hello.sh
```

### Step 5: Execute the script

```bash
./hello.sh
```

### Alternative execution

```bash
bash hello.sh
```

You can also use:

```bash
sh hello.sh
```

However, use `sh` only when the script is compatible with the system's `sh` interpreter.

### Difference between execution methods

| Command | Explanation |
|---|---|
| `./hello.sh` | Executes using the script's shebang |
| `bash hello.sh` | Executes using Bash explicitly |
| `sh hello.sh` | Executes using the system's sh interpreter |

**Important:** When using `bash hello.sh`, execute permission is not required because Bash reads the file directly. The file still needs read permission.

---

## 6. Comments in Shell Scripting

Comments are used to explain code. They are ignored by the shell during execution.

### Single-line comment

```bash
#!/bin/bash

# This is a comment
echo "Hello"
```

### Multiple-line comments

Bash does not have a dedicated multiline comment syntax. You can place `#` at the beginning of each comment line.

```bash
#!/bin/bash

# This script displays
# a welcome message
# to the user

echo "Welcome"
```

**Best practice:** Write comments to explain the purpose of important or complex commands.

---

## 7. Variables in Shell Scripting

Variables are used to store values that can be accessed and modified during script execution.

### Creating variables

```bash
#!/bin/bash

name="Prathyusha"
age=22

echo "$name"
echo "$age"
```

### Important variable rules

1. Do not add spaces around `=` during assignment.
2. Variable names should not contain spaces.
3. Variable names are case-sensitive.
4. Use `$` to access a variable's value.
5. Variable names can contain letters, numbers, and underscores, but cannot start with a number.

Correct:

```bash
name="Linux"
```

Incorrect:

```bash
name = "Linux"
```

### Display variables

```bash
name="DevOps"

echo $name
echo "$name"
```

Prefer double quotes around variable expansions to prevent unwanted word splitting and pathname expansion.

### Read-only variables

```bash
readonly company="OpenAI"

echo "$company"
```

A read-only variable cannot be reassigned.

### Remove a variable

```bash
unset name
```

---

## 8. Types of Variable Expansion

### 8.1 Normal variable expansion

```bash
name="Linux"
echo "$name"
```

Output:

```text
Linux
```

### 8.2 Command substitution

Command substitution stores the output of a command in a variable.

```bash
current_date=$(date)
echo "$current_date"
```

Another example:

```bash
files=$(ls)
echo "$files"
```

Modern Bash scripts should generally use `$(command)` instead of the older backtick syntax.

### 8.3 Environment variables

Environment variables provide information to processes and applications.

Common environment variables:

| Variable | Purpose |
|---|---|
| `$HOME` | Current user's home directory |
| `$USER` | Current username |
| `$PATH` | Directories searched for executable commands |
| `$PWD` | Current working directory |
| `$SHELL` | User's configured login shell |
| `$HOSTNAME` | System hostname |

Example:

```bash
echo "$HOME"
echo "$USER"
echo "$PATH"
echo "$PWD"
```

Display all environment variables:

```bash
printenv
```

Create an environment variable:

```bash
export APP_ENV="development"
```

---

## 9. User Input Using read

The `read` command accepts input from the user.

Example:

```bash
#!/bin/bash

echo "Enter your name:"
read name

echo "Welcome $name"
```

### Shorter method

```bash
#!/bin/bash

read -p "Enter your name: " name

echo "Hello $name"
```

### Read multiple values

```bash
#!/bin/bash

read -p "Enter your first and last name: " first last

echo "First name: $first"
echo "Last name: $last"
```

### Hide sensitive input

```bash
#!/bin/bash

read -s -p "Enter password: " password
echo
echo "Password received"
```

`-s` prevents the typed input from being displayed.

**Security note:** Do not print passwords or store them in plain-text logs.

---

## 10. Command-Line Arguments

Command-line arguments allow users to pass values to a script when executing it.

Example:

```bash
#!/bin/bash

echo "First argument: $1"
echo "Second argument: $2"
```

Execute:

```bash
bash script.sh Linux DevOps
```

Output:

```text
First argument: Linux
Second argument: DevOps
```

### Important special variables

| Variable | Meaning |
|---|---|
| `$0` | Script name or invocation path |
| `$1` | First positional argument |
| `$2` | Second positional argument |
| `$#` | Number of positional arguments |
| `$@` | All positional arguments |
| `$*` | All positional arguments, expanded differently when quoted |
| `$$` | Process ID of the current shell |
| `$?` | Exit status of the previous command |
| `$!` | Process ID of the most recently started background job |

Example:

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Number of arguments: $#"
echo "All arguments: $@"
```

Execute:

```bash
bash script.sh Linux DevOps AWS
```

### Difference between "$@" and "$*"

- `"$@"` expands each argument as a separate quoted word.
- `"$*"` expands all arguments together as one word, separated by the first character of `IFS`.

**Interview point:** Prefer `"$@"` when passing all script arguments to another command or function.

---

## 11. Operators in Shell Scripting

Operators are used to perform arithmetic, comparison, and logical operations.

### 11.1 Arithmetic operators

Common arithmetic operators:

| Operator | Meaning |
|---|---|
| `+` | Addition |
| `-` | Subtraction |
| `*` | Multiplication |
| `/` | Division |
| `%` | Modulus (remainder) |

Bash arithmetic example:

```bash
#!/bin/bash

a=20
b=10

echo $((a + b))
echo $((a - b))
echo $((a * b))
echo $((a / b))
echo $((a % b))
```

### 11.2 Comparison operators

For integer comparisons inside `[` or `[[`:

| Operator | Meaning |
|---|---|
| `-eq` | Equal to |
| `-ne` | Not equal to |
| `-gt` | Greater than |
| `-lt` | Less than |
| `-ge` | Greater than or equal to |
| `-le` | Less than or equal to |

Example:

```bash
#!/bin/bash

a=20
b=10

if [ "$a" -gt "$b" ]; then
    echo "a is greater than b"
fi
```

Bash also supports operators such as `>` and `<` inside `(( ))` for arithmetic comparisons.

### 11.3 String comparison operators

| Operator | Meaning |
|---|---|
| `=` | Strings are equal |
| `!=` | Strings are not equal |
| `-z` | String is empty |
| `-n` | String is not empty |

Example:

```bash
#!/bin/bash

name="Linux"

if [ "$name" = "Linux" ]; then
    echo "Correct name"
fi
```

### 11.4 Logical operators

| Operator | Meaning |
|---|---|
| `&&` | Logical AND |
| `||` | Logical OR |
| `!` | Logical NOT |

Example:

```bash
#!/bin/bash

age=25

if [ "$age" -ge 18 ] && [ "$age" -le 60 ]; then
    echo "Age is within the range"
fi
```

---

## 12. Conditional Statements

Conditional statements execute commands based on whether a condition is true or false.

### 12.1 if statement

Syntax:

```bash
if [ condition ]; then
    commands
fi
```

Example:

```bash
#!/bin/bash

number=10

if [ "$number" -gt 5 ]; then
    echo "Number is greater than 5"
fi
```

### 12.2 if-else statement

```bash
#!/bin/bash

number=5

if [ "$number" -gt 10 ]; then
    echo "Number is greater than 10"
else
    echo "Number is not greater than 10"
fi
```

### 12.3 if-elif-else statement

```bash
#!/bin/bash

marks=75

if [ "$marks" -ge 90 ]; then
    echo "Grade A"
elif [ "$marks" -ge 75 ]; then
    echo "Grade B"
elif [ "$marks" -ge 50 ]; then
    echo "Grade C"
else
    echo "Fail"
fi
```

### 12.4 Nested if statement

An if statement inside another if statement is called a nested if statement.

```bash
#!/bin/bash

number=20

if [ "$number" -gt 0 ]; then
    if [ "$number" -gt 10 ]; then
        echo "Positive number greater than 10"
    fi
fi
```

### 12.5 case statement

The `case` statement is useful when checking multiple possible values.

Example:

```bash
#!/bin/bash

read -p "Enter an option: " choice

case "$choice" in
    start)
        echo "Starting service"
        ;;
    stop)
        echo "Stopping service"
        ;;
    restart)
        echo "Restarting service"
        ;;
    *)
        echo "Invalid option"
        ;;
esac
```

**Important syntax:**
- `case` starts the statement.
- `in` begins the matching patterns.
- `)` separates a pattern from its commands.
- `;;` ends a pattern's commands.
- `*)` handles unmatched values.
- `esac` ends the statement.

---

## 13. Loops in Shell Scripting

Loops execute a set of commands repeatedly.

Bash supports `for`, `while`, and `until` loops.

### 13.1 for loop

A `for` loop executes commands for each item in a list.

Syntax:

```bash
for variable in list
do
    commands
done
```

Example:

```bash
#!/bin/bash

for name in Linux AWS Docker
do
    echo "$name"
done
```

### Using a range

```bash
#!/bin/bash

for i in {1..5}
do
    echo "$i"
done
```

### C-style for loop

```bash
#!/bin/bash

for ((i=1; i<=5; i++))
do
    echo "$i"
done
```

### 13.2 while loop

A `while` loop executes commands as long as the condition is true.

Example:

```bash
#!/bin/bash

i=1

while [ "$i" -le 5 ]
do
    echo "$i"
    i=$((i + 1))
done
```

### 13.3 until loop

An `until` loop executes commands until its condition becomes true.

Example:

```bash
#!/bin/bash

i=1

until [ "$i" -gt 5 ]
do
    echo "$i"
    i=$((i + 1))
done
```

### 13.4 break statement

The `break` statement terminates the loop.

```bash
#!/bin/bash

for i in {1..10}
do
    if [ "$i" -eq 5 ]; then
        break
    fi

    echo "$i"
done
```

### 13.5 continue statement

The `continue` statement skips the current iteration and moves to the next iteration.

```bash
#!/bin/bash

for i in {1..5}
do
    if [ "$i" -eq 3 ]; then
        continue
    fi

    echo "$i"
done
```

---

## 14. Functions in Shell Scripting

A function is a reusable block of commands that performs a specific task.

Functions help avoid repeating the same code.

### Function syntax

```bash
function_name() {
    commands
}
```

Example:

```bash
#!/bin/bash

greet() {
    echo "Welcome to Linux"
}

greet
```

### Function with arguments

```bash
#!/bin/bash

greet() {
    echo "Hello $1"
}

greet "Prathyusha"
```

### Function returning an exit status

```bash
#!/bin/bash

check() {
    return 0
}

check

if [ "$?" -eq 0 ]; then
    echo "Function successful"
fi
```

**Important:** In Bash, `return` sets a function's exit status, usually an integer from 0 to 255. It does not return arbitrary text. Use `echo` or another output method when a function needs to produce text.

Example:

```bash
#!/bin/bash

get_name() {
    echo "Linux"
}

name=$(get_name)

echo "$name"
```

---

## 15. Arrays in Bash

An array stores multiple values in a single variable.

Example:

```bash
#!/bin/bash

languages=("Linux" "Python" "Shell" "SQL")

echo "${languages[0]}"
echo "${languages[1]}"
```

Array indexes start from zero.

### Display all elements

```bash
echo "${languages[@]}"
```

### Display array length

```bash
echo "${#languages[@]}"
```

### Add an element

```bash
languages+=("AWS")
```

### Loop through an array

```bash
for language in "${languages[@]}"
do
    echo "$language"
done
```

**Important:** Arrays shown here are Bash arrays. They are not supported in the same way by every shell, including traditional POSIX `sh`.

---

## 16. File and Directory Conditions

Bash provides test operators to check files and directories.

| Operator | Meaning |
|---|---|
| `-e` | Path exists |
| `-f` | Regular file exists |
| `-d` | Directory exists |
| `-r` | File is readable |
| `-w` | File is writable |
| `-x` | File is executable |
| `-s` | File exists and has a size greater than zero |

### Check whether a file exists

```bash
#!/bin/bash

if [ -f "/tmp/test.txt" ]; then
    echo "File exists"
else
    echo "File does not exist"
fi
```

### Check whether a directory exists

```bash
#!/bin/bash

if [ -d "/tmp/backup" ]; then
    echo "Directory exists"
else
    echo "Directory does not exist"
fi
```

### Check whether a file is readable

```bash
#!/bin/bash

if [ -r "/tmp/test.txt" ]; then
    echo "File is readable"
fi
```

### Check whether a file is empty

```bash
#!/bin/bash

if [ -s "/tmp/test.txt" ]; then
    echo "File is not empty"
else
    echo "File is empty or does not exist"
fi
```

---

## 17. Exit Status in Shell Scripting

An exit status is a numeric value that indicates whether a command completed successfully.

Common convention:

- `0` means success.
- A non-zero value indicates failure or another condition.

### Check exit status

```bash
ls
echo $?
```

If `ls` succeeds, the exit status is normally `0`.

Example of failure:

```bash
ls /directory_that_does_not_exist
echo $?
```

This normally returns a non-zero status.

### Exit command

The `exit` command terminates a script and returns an exit status.

```bash
#!/bin/bash

echo "Script completed"

exit 0
```

Failure example:

```bash
#!/bin/bash

echo "Something went wrong"

exit 1
```

### Why is exit status important in DevOps?

Exit statuses are used by:
- CI/CD pipelines.
- Automation scripts.
- Monitoring systems.
- Scheduled jobs.
- Other scripts that call a script.

A pipeline can use the exit status to determine whether a task succeeded or failed.

---

## 18. Error Handling in Bash

Error handling helps scripts respond safely when commands fail.

### 18.1 set -e

```bash
#!/bin/bash

set -e

mkdir /tmp/devops-test
echo "Directory created"
```

`set -e` generally causes a script to exit when a simple command fails, subject to Bash's conditional and command-list exceptions.

### 18.2 set -u

```bash
#!/bin/bash

set -u

echo "$username"
```

`set -u` treats an unset variable expansion as an error.

### 18.3 set -o pipefail

```bash
#!/bin/bash

set -o pipefail

cat missing.txt | grep "Linux"
```

Normally, a pipeline returns the status of its last command. `pipefail` makes the pipeline return a failure if a command in the pipeline fails, subject to Bash's pipeline status rules.

### 18.4 Common strict mode

```bash
#!/bin/bash

set -euo pipefail

echo "Starting script"
```

This is a useful starting point for many Bash scripts, but it is not a replacement for proper error handling. Understand its behavior before using it in production.

### 18.5 Handle errors manually

```bash
#!/bin/bash

mkdir /tmp/devops-backup

if [ "$?" -eq 0 ]; then
    echo "Directory created successfully"
else
    echo "Directory creation failed"
    exit 1
fi
```

A more direct approach is:

```bash
#!/bin/bash

if mkdir /tmp/devops-backup; then
    echo "Directory created successfully"
else
    echo "Directory creation failed"
    exit 1
fi
```

---

## 19. Using grep, sed, and awk in Shell Scripts

These commands are frequently used in Linux automation.

### 19.1 grep

`grep` searches for matching text.

Example:

```bash
#!/bin/bash

grep "ERROR" application.log
```

Case-insensitive search:

```bash
grep -i "error" application.log
```

Count matching lines:

```bash
grep -c "ERROR" application.log
```

### 19.2 sed

`sed` is a stream editor used to search and transform text.

Replace a word:

```bash
sed 's/Linux/DevOps/' file.txt
```

Replace all occurrences on each line:

```bash
sed 's/Linux/DevOps/g' file.txt
```

Edit a file in place:

```bash
sed -i 's/Linux/DevOps/g' file.txt
```

**Important:** Make a backup before using in-place modifications on important files. The `-i` option has some implementation differences between GNU and BSD versions.

### 19.3 awk

`awk` is used to process structured text and columns.

Example file:

```text
John 25000
David 35000
Sam 40000
```

Display the first column:

```bash
awk '{print $1}' employee.txt
```

Display the second column:

```bash
awk '{print $2}' employee.txt
```

Display both columns:

```bash
awk '{print $1, $2}' employee.txt
```

**DevOps use case:** Use `grep` to find errors, `sed` to transform configuration text, and `awk` to extract specific fields from command output.

---

## 20. Scheduling Scripts with Cron

Cron is a time-based job scheduler in Linux. It runs commands or scripts automatically at configured times.

### Open the current user's crontab

```bash
crontab -e
```

### List scheduled cron jobs

```bash
crontab -l
```

### Remove the current user's crontab

```bash
crontab -r
```

**Warning:** `crontab -r` removes the current user's crontab. Do not run it unless you intend to delete all those scheduled jobs.

### Cron syntax

```text
* * * * * command
| | | | |
| | | | +--- Day of week
| | | +----- Month
| | +------- Day of month
| +--------- Hour
+----------- Minute
```

### Common cron examples

Run every minute:

```text
* * * * * /home/user/script.sh
```

Run every day at 9 AM:

```text
0 9 * * * /home/user/script.sh
```

Run every Sunday at midnight:

```text
0 0 * * 0 /home/user/script.sh
```

Run every 5 minutes:

```text
*/5 * * * * /home/user/script.sh
```

### Important cron best practices

1. Use absolute paths for scripts and commands.
2. Ensure scripts have appropriate permissions.
3. Redirect output to a log file when necessary.
4. Do not assume cron has the same environment as your interactive terminal.
5. Test scripts manually before scheduling them.

Example:

```text
*/5 * * * * /home/user/scripts/backup.sh >> /home/user/scripts/backup.log 2>&1
```

This runs the script every five minutes and appends standard output and errors to the log file.

---

## 21. Practical DevOps Shell Scripts

### Script 1: Display system information

Filename: `system-info.sh`

```bash
#!/bin/bash

echo "===== SYSTEM INFORMATION ====="

echo "Username: $(whoami)"
echo "Hostname: $(hostname)"
echo "Date: $(date)"
echo "Current Directory: $(pwd)"

echo "===== MEMORY USAGE ====="
free -h

echo "===== DISK USAGE ====="
df -h

echo "===== SYSTEM UPTIME ====="
uptime
```

Purpose:
- Displays system information.
- Checks memory usage.
- Checks disk usage.
- Displays system uptime.

### Script 2: Check disk usage

Filename: `disk-check.sh`

```bash
#!/bin/bash

THRESHOLD=80

USAGE=$(df -P / | awk 'NR==2 {gsub("%","",$5); print $5}')

echo "Current disk usage: ${USAGE}%"

if [ "$USAGE" -ge "$THRESHOLD" ]; then
    echo "WARNING: Disk usage is high"
else
    echo "Disk usage is normal"
fi
```

Purpose:
- Checks root filesystem usage.
- Compares disk usage with a threshold.
- Displays a warning when usage reaches or exceeds 80%.

**DevOps use case:** This type of script can be adapted for scheduled disk monitoring and alerting.

### Script 3: Create a backup

Filename: `backup.sh`

```bash
#!/bin/bash

SOURCE="/home/user/documents"
DEST="/home/user/backups"

mkdir -p "$DEST"

if [ ! -d "$SOURCE" ]; then
    echo "Source directory does not exist"
    exit 1
fi

BACKUP_NAME="backup_$(date +%Y%m%d_%H%M%S).tar.gz"

tar -czf "$DEST/$BACKUP_NAME" -C "$(dirname "$SOURCE")" "$(basename "$SOURCE")"

if [ "$?" -eq 0 ]; then
    echo "Backup created successfully"
else
    echo "Backup failed"
    exit 1
fi
```

Purpose:
- Checks whether the source directory exists.
- Creates the backup destination if needed.
- Creates a compressed archive.
- Adds a date and time to the backup filename.

**Note:** Replace the example source and destination paths with directories that exist on your system. Test backups and verify that the archive can be restored.

### Script 4: Check whether a service is running

Filename: `service-check.sh`

```bash
#!/bin/bash

SERVICE="nginx"

if systemctl is-active --quiet "$SERVICE"; then
    echo "$SERVICE is running"
else
    echo "$SERVICE is not running"
fi
```

Purpose:
- Checks the service's active state.
- Displays whether the service is running.

### Script 5: Create multiple directories

Filename: `create-directories.sh`

```bash
#!/bin/bash

for directory in Linux AWS Docker Git Python
do
    mkdir -p "$directory"
    echo "Created or verified: $directory"
done
```

Purpose:
- Creates multiple directories using a loop.
- Uses `mkdir -p` so existing directories do not cause an error.

### Script 6: Check application log errors

Filename: `error-check.sh`

```bash
#!/bin/bash

LOG_FILE="/var/log/syslog"

if [ ! -r "$LOG_FILE" ]; then
    echo "Log file does not exist or is not readable"
    exit 1
fi

echo "Searching for errors..."

grep -i "error" "$LOG_FILE"
```

Purpose:
- Checks whether a log file is readable.
- Searches for error messages.
- Displays matching lines.

**Note:** On some distributions, the system log may be `/var/log/messages` or available through `journalctl` instead.

### Script 7: User input calculator

Filename: `calculator.sh`

```bash
#!/bin/bash

read -p "Enter first number: " a
read -p "Enter second number: " b

echo "Addition: $((a + b))"
echo "Subtraction: $((a - b))"
echo "Multiplication: $((a * b))"

if [ "$b" -ne 0 ]; then
    echo "Division: $((a / b))"
else
    echo "Division is not possible by zero"
fi
```

Purpose:
- Accepts two numbers.
- Performs arithmetic operations.
- Checks for division by zero.

---

## 22. Shell Scripting Best Practices

1. Always use an appropriate shebang.
2. Use meaningful variable names.
3. Quote variables, especially when they contain paths or user input.
4. Use comments for complex operations.
5. Validate user input.
6. Check whether required files and directories exist.
7. Handle command failures.
8. Use appropriate exit codes.
9. Avoid hardcoding passwords and secret keys.
10. Use absolute paths in cron jobs.
11. Test scripts before running them in production.
12. Avoid unnecessary use of root privileges.
13. Keep scripts readable and organized.
14. Use version control such as Git to maintain scripts.
15. Use tools such as ShellCheck to detect common Bash mistakes.

### ShellCheck

ShellCheck is a static analysis tool for shell scripts.

It identifies common errors and suggests improvements.

Example:

```bash
shellcheck script.sh
```

It may need to be installed first.

Ubuntu/Debian:

```bash
sudo apt install shellcheck
```

---

## 23. Debugging Shell Scripts

Debugging helps identify mistakes in scripts.

### Syntax check

```bash
bash -n script.sh
```

Checks the script for syntax errors without executing its commands.

### Debug execution

```bash
bash -x script.sh
```

Displays commands as Bash executes them.

### Debug selected sections

```bash
set -x

echo "Debugging enabled"

set +x
```

- `set -x` enables execution tracing.
- `set +x` disables execution tracing.

**Important:** Debug output can expose secrets if they are included in commands or variable expansions. Avoid enabling tracing around sensitive operations.

---

## 24. Difference Between Shell Scripting and Python

| Shell Scripting | Python |
|---|---|
| Commonly used for command-line automation | General-purpose programming language |
| Excellent for Linux system administration | Excellent for applications and automation |
| Uses shell commands directly | Uses Python libraries and modules |
| Commonly used in DevOps scripts | Used in DevOps, backend, data, and other areas |
| Bash scripts often use `.sh` | Python scripts commonly use `.py` |

**Interview point:** Shell scripting is especially useful for automating Linux commands and system administration tasks, while Python is suitable for more complex automation and application development.

---

## 25. Practical Shell Scripting Exercises

Practice these tasks yourself before looking for solutions.

### Beginner Level

1. Write a script to display `Hello World`.
2. Write a script to display your name and current date.
3. Create two variables and display their values.
4. Accept a user's name and display a welcome message.
5. Accept two numbers and display their sum.
6. Check whether a number is positive or negative.
7. Check whether a number is even or odd.
8. Print numbers from 1 to 10 using a `for` loop.
9. Print numbers from 1 to 10 using a `while` loop.
10. Check whether a file exists.

### Intermediate Level

11. Write a script to check whether a directory exists.
12. Write a script to find the largest of three numbers.
13. Write a script to calculate the factorial of a number.
14. Write a script to display all files in a directory.
15. Write a script to count the number of files in a directory.
16. Write a script to create multiple directories.
17. Write a script to search for ERROR messages in a log file.
18. Write a function to check whether a service is running.
19. Write a script to display the top five CPU-consuming processes.
20. Write a script to check disk usage and display a warning above 80%.

### DevOps Practical Level

21. Write a script to check CPU and memory usage.
22. Write a script to create a timestamped backup.
23. Write a script to check whether Nginx is running.
24. Write a script to monitor a log file for errors.
25. Write a script to check whether a required application directory exists.
26. Write a script to verify that a website responds using `curl`.
27. Write a script to check disk space and log the result.
28. Schedule a backup script using cron.
29. Write a script that reports its success or failure using exit codes.
30. Create a script that gathers system information into a report file.

---

## 26. Linux Shell Scripting Interview Questions

### Q1. What is shell scripting?

Shell scripting is the process of writing Linux commands in a file and executing them using a shell interpreter to automate tasks.

### Q2. What is a shell?

A shell is a command-line interpreter that acts as an interface between the user and the operating system.

### Q3. What is Bash?

Bash stands for Bourne Again Shell. It is a widely used command-line shell and scripting language in Linux.

### Q4. What is a shebang?

A shebang is the first line of a script that specifies the interpreter used to execute it.

Example:

```bash
#!/bin/bash
```

### Q5. How do you execute a shell script?

First give it execute permission:

```bash
chmod +x script.sh
```

Then execute:

```bash
./script.sh
```

Alternatively:

```bash
bash script.sh
```

### Q6. What is the difference between `$` and `#`?

- `$` is used for variable expansion and other shell expansions.
- `#` begins a comment in ordinary shell script syntax.
- `#` also appears in the shebang `#!`.

### Q7. What is a variable in shell scripting?

A variable is a named location used to store a value that can be accessed and modified during script execution.

### Q8. What is the purpose of the read command?

The `read` command accepts input from the user and stores it in a variable.

### Q9. What are command-line arguments?

Command-line arguments are values passed to a script when it is executed.

Examples include `$1`, `$2`, and `$@`.

### Q10. What is the difference between `$@` and `$*`?

`"$@"` expands each argument separately, while `"$*"` combines all arguments into a single word when quoted.

### Q11. What is the difference between for and while loops?

A `for` loop commonly iterates over a list or range of values, while a `while` loop continues executing as long as its condition is true.

### Q12. What is a function in shell scripting?

A function is a reusable block of commands used to perform a specific task.

### Q13. What is the purpose of chmod +x?

It adds execute permission to a file according to the permission settings.

### Q14. What is an exit status?

An exit status is a numeric value that indicates whether a command or script completed successfully. Zero conventionally means success, while non-zero indicates failure or another condition.

### Q15. What is the purpose of cron?

Cron is used to schedule commands and scripts to run automatically at specified times.

### Q16. What is the difference between grep, sed, and awk?

- `grep` searches for matching text.
- `sed` performs text transformations.
- `awk` processes structured text and fields.

### Q17. What is the purpose of set -e?

It makes Bash exit when certain commands fail. It has exceptions, so it should not be treated as complete error handling.

### Q18. How do you debug a shell script?

Use:

```bash
bash -x script.sh
```

This displays commands as they are executed.

### Q19. How do you check disk usage using a shell script?

Use the `df` command and process its output.

Example:

```bash
df -h
```

### Q20. How can shell scripting help in DevOps?

Shell scripting helps automate deployments, backups, log analysis, system monitoring, service checks, scheduled jobs, and other repetitive infrastructure tasks.

### Q21. How do you check whether a file exists in Bash?

Use:

```bash
if [ -f "file.txt" ]; then
    echo "File exists"
fi
```

### Q22. What is the difference between source and executing a script?

Executing a script normally runs it in a separate process or shell environment. Using `source script.sh` runs it in the current shell, so variable changes and directory changes can affect the current shell session.

### Q23. What is the purpose of ShellCheck?

ShellCheck is a static analysis tool that identifies common errors and potential problems in shell scripts.

### Q24. Why should passwords not be hardcoded in shell scripts?

Hardcoded passwords can be exposed through source code, version control, logs, and other locations. Secrets should be managed through secure mechanisms.

### Q25. How would you automate a backup in Linux?

I would write a shell script that checks the source directory, creates a timestamped backup archive, verifies the operation, and logs the result. I could schedule it using cron.

---

## 27. Final Revision Checklist

Before moving to the next section, make sure you can:

- [ ] Explain what a shell and shell script are.
- [ ] Understand Bash and the shebang.
- [ ] Create and execute a shell script.
- [ ] Use comments and variables.
- [ ] Accept user input using `read`.
- [ ] Understand command-line arguments.
- [ ] Use arithmetic and comparison operators.
- [ ] Write `if`, `elif`, `else`, and `case` statements.
- [ ] Write `for`, `while`, and `until` loops.
- [ ] Use functions and arrays.
- [ ] Check files and directories.
- [ ] Understand exit statuses.
- [ ] Handle command failures.
- [ ] Use `grep`, `sed`, and `awk`.
- [ ] Schedule scripts using cron.
- [ ] Debug scripts using `bash -x`.
- [ ] Write practical DevOps automation scripts.

---

## 28. Conclusion

Shell scripting is an important skill for Linux system administration and DevOps engineering.

It helps automate repetitive tasks, manage system resources, troubleshoot issues, and support deployment workflows.

Practice writing scripts regularly instead of only memorizing commands.


