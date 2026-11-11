# Linux File Permissions and Ownership

## 1. Introduction to Linux File Permissions

Linux is a multi-user operating system. Multiple users can access the same system, so Linux uses file permissions to control who can read, modify, or execute files and directories.

**Definition:**

Linux file permissions are security rules that determine what actions a file owner, group members, and other users can perform on files and directories.

### Why are file permissions important?

- Protect sensitive files from unauthorized access.
- Prevent users from modifying important system files.
- Control access to application files and deployment scripts.
- Secure configuration files and application logs.
- Allow teams to share directories safely.

**DevOps example:**

Suppose a deployment script is used to deploy an application. Only authorized users should be able to modify it, while the deployment user should be able to execute it.

Linux permissions help control this access.

---

## 2. Understanding Linux Permission Types

Linux provides three basic permissions:

| Permission | Symbol | Numeric value | Meaning |
|---|---|---:|---|
| Read | `r` | 4 | View file contents |
| Write | `w` | 2 | Modify file contents |
| Execute | `x` | 1 | Execute a file or search a directory |
| No permission | `-` | 0 | No access |

### 2.1 Read Permission (`r`)

For files:
- Allows users to view file contents.
- Example: Reading a text file using `cat`.

For directories:
- Allows users to list directory entries.
- Example: Running `ls` on a directory.

### 2.2 Write Permission (`w`)

For files:
- Allows users to modify file contents.

For directories:
- Allows users to create, delete, and rename entries, subject to other permission checks such as the sticky bit.

### 2.3 Execute Permission (`x`)

For files:
- Allows users to execute a program or script, provided it is otherwise executable and the system permits it.

For directories:
- Allows users to search or traverse the directory.
- It is needed to access entries inside a directory when their names are known.

**Important:** Directory permissions work differently from file permissions. For example, execute permission on a directory allows traversal, not execution of the directory itself.

---

## 3. Understanding Owner, Group, and Others

Linux divides permissions into three categories:

1. **User (Owner):** The user who owns the file.
2. **Group:** Users who belong to the file's associated group.
3. **Others:** All users who are neither the owner nor members of the associated group.

### Example: Understanding `ls -l`

Command:

```bash
ls -l
```

Example output:

```text
-rwxr-xr-- 1 prathyusha developers 1024 Nov 5 10:30 deploy.sh
```

The permission string is:

```text
-rwxr-xr--
```

Breakdown:

```text
- | rwx | r-x | r--
    User  Group Others
```

| Part | Meaning | Permission |
|---|---|---|
| `-` | File type | Regular file |
| `rwx` | Owner | Read, write, execute |
| `r-x` | Group | Read, execute |
| `r--` | Others | Read only |

In this example:
- Owner `prathyusha` can read, write, and execute.
- Members of group `developers` can read and execute.
- Other users can only read.

**Note:** The first character identifies the file type. A `-` means a regular file, while `d` means a directory.

---

## 4. Checking File Permissions

### 4.1 `ls -l`

Displays file permissions, ownership, size, and modification time.

Syntax:

```bash
ls -l filename
```

Example:

```bash
ls -l deploy.sh
```

### 4.2 `stat`

Displays detailed information about a file, including permissions and ownership.

Syntax:

```bash
stat filename
```

Example:

```bash
stat deploy.sh
```

### 4.3 `namei -l`

Displays the permissions and ownership of each directory component in a path.

Example:

```bash
namei -l /var/www/html/index.html
```

```text

4 + 2 + 1 = 7
```


Permission: `rwx`


**Example 2: Read + Execute**


```text
4 + 1 = 5
```


Permission: `r-x`

**Example 3: Read only**

```text
4 = 4
```

Permission: `r--`

**Example 4: Read + Write**

```text
4 + 2 = 6
```

Permission: `rw-`

### Common permission combinations

| Numeric | Symbolic | Meaning |
|---:|---|---|
| 777 | `rwxrwxrwx` | Everyone has full permissions |
| 755 | `rwxr-xr-x` | Owner has full access; others can read and execute |
| 700 | `rwx------` | Only owner has access |
| 644 | `rw-r--r--` | Owner can read/write; others can read |
| 600 | `rw-------` | Only owner can read/write |
| 750 | `rwxr-x---` | Owner has full access; group can read/execute |

**Security note:** Avoid using `777` as a routine solution. It gives every local user write access and can create security risks.

---

## 6. chmod – Changing File Permissions

`chmod` stands for **Change Mode**.


**Definition:**


The `chmod` command is used to change the permissions of files and directories in Linux.


### Syntax

```bash
chmod permissions filename
```

There are two methods:

1. Numeric method
2. Symbolic method

### 6.1 Numeric Method

Example:

```bash
chmod 755 deploy.sh
```

Meaning:

- Owner: Read, write, execute.
- Group: Read and execute.
- Others: Read and execute.

Another example:

```bash
chmod 644 config.txt
```

Meaning:

- Owner: Read and write.
- Group: Read only.
- Others: Read only.

### 6.2 Symbolic Method

Symbolic permissions use letters:

| Symbol | Meaning |
|---|---|
| `u` | User (owner) |
| `g` | Group |
| `o` | Others |
| `a` | All |
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exact permission |

Examples:

**Add execute permission to owner:**

```bash
chmod u+x deploy.sh
```

**Remove write permission from group:**

```bash
chmod g-w config.txt
```

**Give others read-only permission:**

```bash
chmod o=r config.txt
```

**Add read permission to everyone:**

```bash
chmod a+r notes.txt
```

**Remove all permissions from others:**

```bash
chmod o= file.txt
```

### 6.3 Recursive Permission Changes

The `-R` option applies permission changes recursively to a directory and its contents.

Example:

```bash
chmod -R 755 /var/www/html
```

This applies `755` to all files and directories under that path.

**Important:** Applying the same permission recursively to files and directories is often inappropriate. Regular files usually do not need execute permission. Be careful with recursive changes, especially in production.

---

## 7. chown – Changing File Ownership

`chown` stands for **Change Owner**.

**Definition:**

The `chown` command is used to change the owner of a file or directory. It can also change the associated group.

### Syntax

```bash
chown username filename
```

### Examples

**Change file owner:**

```bash
sudo chown prathyusha deploy.sh
```

**Change both owner and group:**

```bash
sudo chown prathyusha:developers deploy.sh
```

**Change only group using chown:**

```bash
sudo chown :developers deploy.sh
```

**Change ownership recursively:**

```bash
sudo chown -R prathyusha:developers /var/www/html
```

**Verify ownership:**

```bash
ls -l deploy.sh
```

**Important:** Changing ownership usually requires root privileges or appropriate capabilities. Use `sudo` when necessary.

---

## 8. chgrp – Changing Group Ownership

`chgrp` stands for **Change Group**.

**Definition:**

The `chgrp` command is used to change the group ownership of a file or directory.

### Syntax

```bash
chgrp groupname filename
```

### Examples

**Change group ownership:**

```bash
sudo chgrp developers deploy.sh
```

**Change group ownership recursively:**

```bash
sudo chgrp -R developers /var/www/html
```

**Verify:**

```bash
ls -l deploy.sh
```

### Difference between chmod, chown, and chgrp

| Command | Purpose |
|---|---|
| `chmod` | Changes file permissions |
| `chown` | Changes owner and optionally group |
| `chgrp` | Changes group ownership |

---

## 9. umask – Default Permission Settings

`umask` stands for **User File Creation Mode Mask**.

**Definition:**

The `umask` command controls which permission bits are removed from the default permissions when new files and directories are created.

It does not change permissions on existing files.

### Check the current umask

```bash
umask
```

Example output:

```text
0022
```

### How umask works

Linux uses different base permissions when creating files and directories:

| Type | Base permissions |
|---|---|
| Regular files | 666 (`rw-rw-rw-`) |
| Directories | 777 (`rwxrwxrwx`) |

The umask removes permission bits from these base permissions.

Example: `umask 022`

For a regular file:

```text
Base permissions: 666
Umask:            022
Result:           644
```

For a directory:

```text
Base permissions: 777
Umask:            022
Result:           755
```

Therefore:
- New files usually get `644`.
- New directories usually get `755`.

**Important:** This is a bitmask operation, not ordinary decimal subtraction. The application creating a file can also request more restrictive permissions, so the final result may differ.

### Change umask

```bash
umask 027
```

With this mask, the typical results are:

- Files: `640`
- Directories: `750`

### Why is umask important in DevOps?

It helps prevent newly created files from receiving unnecessarily broad permissions.

For example, a service creating configuration files may use a restrictive umask so other users cannot read sensitive information.

---

## 10. Special Permissions in Linux

Linux provides three special permission bits:

1. SUID – Set User ID
2. SGID – Set Group ID
3. Sticky Bit

These permissions provide additional control over how executable files and directories behave.

### 10.1 SUID – Set User ID

**Definition:**

SUID is a special permission that causes an executable program to run with the effective user ID of the file owner, rather than the user who started it.

It is commonly used by certain system programs that need controlled elevated privileges.

Numeric value:

```text
4
```

Example:

```bash
chmod u+s filename
```

Numeric example:

```bash
chmod 4755 filename
```

Check permissions:

```bash
ls -l filename
```

You may see:

```text
-rwsr-xr-x
```

The `s` in the owner's execute position indicates SUID is set and the execute bit is also set.

**Example:**

The `passwd` command commonly uses SUID so an ordinary user can securely request a password change through a privileged program.

Check it with:

```bash
ls -l /usr/bin/passwd
```

**Security note:** SUID programs must be carefully controlled. A vulnerable SUID executable can create a serious privilege-escalation risk. SUID behavior can also be restricted by the filesystem or system configuration.

### 10.2 SGID – Set Group ID

**Definition:**

SGID is a special permission that has two important behaviors:

- On executable files, it can cause the program to run with the effective group ID of the file's group.
- On directories, newly created files and subdirectories inherit the directory's group ownership.

Numeric value:

```text
2
```

**Set SGID on a file:**

```bash
chmod g+s filename
```

**Set SGID on a directory:**

```bash
chmod g+s shared
```

Numeric example:

```bash
chmod 2775 shared
```

Check:

```bash
ls -ld shared
```

Example output:

```text
drwxrwsr-x
```

The `s` in the group's execute position indicates SGID is set and the execute bit is also set.

**DevOps example:**

Suppose multiple developers need to work in a shared deployment directory. Setting SGID on that directory helps new files inherit the shared group.

This makes team collaboration easier.

### 10.3 Sticky Bit

**Definition:**

The sticky bit is a special directory permission that restricts who can delete or rename entries inside that directory.

In a sticky directory, deletion or renaming is generally allowed only for:
- The owner of the file.
- The owner of the directory.
- A privileged user.

Numeric value:

```text
1
```

**Set sticky bit:**

```bash
chmod +t directory
```

Numeric example:

```bash
chmod 1777 shared
```

Check:

```bash
ls -ld shared
```

Example output:

```text
drwxrwxrwt
```

The `t` in the others' execute position indicates the sticky bit is set and the execute bit is also set.

**Real-world example:**

The `/tmp` directory commonly uses permissions `1777`.

Many users can create temporary files there, but one ordinary user cannot normally delete another user's files just because the directory is writable.

### Special permission summary

| Permission | Numeric value | Main purpose |
|---|---:|---|
| SUID | 4 | Executable uses file owner's effective UID |
| SGID | 2 | Executable uses file group's effective GID; directories inherit group |
| Sticky bit | 1 | Restricts deletion/renaming in shared directories |

**Important:** If the special bit is set but the corresponding execute/search bit is not, the permission may appear as uppercase `S` or `T` instead of lowercase `s` or `t`.

---

## 11. ACL – Access Control Lists

ACL stands for **Access Control List**.

**Definition:**

ACL provides more detailed permission control by allowing permissions to be assigned to specific users and groups beyond the basic owner, group, and others permission model.

### Why do we need ACL?

Suppose a file is owned by `prathyusha` and belongs to the `developers` group.

You want one additional user, `tester`, to read the file without changing the owner or the group.

ACL can provide this access.

### Important ACL commands

**1. Check ACL permissions**

```bash
getfacl filename
```

**2. Give a specific user read permission**

```bash
setfacl -m u:tester:r filename
```

**3. Give a specific user read and write permission**

```bash
setfacl -m u:tester:rw filename
```

**4. Give a group read and execute permission**

```bash
setfacl -m g:developers:rx filename
```

**5. Remove a specific user's ACL entry**

```bash
setfacl -x u:tester filename
```

**6. Remove all extended ACL entries**

```bash
setfacl -b filename
```

**7. Set a default ACL on a directory**

```bash
setfacl -d -m g:developers:rwx shared
```

A default ACL on a directory can be inherited by newly created entries, subject to the creating application's requested permissions.

### ACL mask

ACL has a mask that can limit the effective permissions of named users, named groups, and the owning group.

Use:

```bash
getfacl filename
```

Look for entries such as:

```text
mask::r--
```

An ACL entry may request more permissions than the mask allows. The effective permissions are limited by that mask.

### DevOps example

A deployment team has three users:
- Developer
- Tester
- Deployment user

Instead of changing file ownership every time, ACL can grant the tester read access while keeping the existing owner and group.

**Note:** ACL support depends on the filesystem and system configuration. The `getfacl` and `setfacl` utilities may need to be installed.

---

## 12. Common Linux Permission Commands – Quick Reference

| Command | Purpose |
|---|---|
| `ls -l` | View permissions and ownership |
| `ls -ld directory` | View directory's own permissions |
| `stat file` | Display detailed file information |
| `namei -l path` | Check permissions along a path |
| `chmod 755 file` | Set numeric permissions |
| `chmod u+x file` | Add execute permission to owner |
| `chmod g-w file` | Remove group write permission |
| `chmod -R 755 dir` | Recursively change permissions |
| `chown user file` | Change file owner |
| `chown user:group file` | Change owner and group |
| `chgrp group file` | Change group ownership |
| `umask` | Display current umask |
| `umask 027` | Set umask |
| `getfacl file` | View ACL |
| `setfacl -m u:user:r file` | Add user ACL entry |
| `setfacl -b file` | Remove extended ACL entries |

---

## 13. Practical DevOps Scenarios

### Scenario 1: Deployment script is not executing

Problem:

```text
Permission denied
```

Check permissions:

```bash
ls -l deploy.sh
```

Give the owner execute permission:

```bash
chmod u+x deploy.sh
```

Execute:

```bash
./deploy.sh
```

**Explanation:** The script needs execute permission for the user running it. The script's interpreter and path permissions must also be valid.

### Scenario 2: Web server cannot read application files

Check ownership and permissions:

```bash
ls -l /var/www/html/
```

Check which user the web server runs as, then ensure it has the required read permissions and directory traversal permissions.

Example of setting a suitable owner and group:

```bash
sudo chown -R deploy:www-data /var/www/html
```

Do not blindly apply permissions to the whole directory. Choose permissions based on which files need to be readable, writable, or executable.

### Scenario 3: Multiple developers need a shared directory

Create a shared directory:

```bash
sudo mkdir /shared
```

Assign the group:

```bash
sudo chgrp developers /shared
```

Set group permissions and SGID:

```bash
sudo chmod 2775 /shared
```

**Explanation:**
- Developers in the group can work in the directory.
- SGID helps new entries inherit the directory's group.
- The owner and group have read/write/search access.
- Others have read/search access.

Ensure the intended developers are members of the `developers` group.

### Scenario 4: Sensitive configuration file

Example:

```bash
chmod 600 database.conf
```

**Explanation:**

Only the file owner can read and write the file. Other users cannot access its contents through ordinary permission checks.

In production, also ensure the correct owner, group, secret-management approach, and application access are configured.

### Scenario 5: Shared temporary directory

Example:

```bash
sudo chmod 1777 /tmp
```

**Explanation:**

The sticky bit helps prevent ordinary users from deleting or renaming files owned by other users in the shared writable directory.

Do not change system directory permissions unless you understand the impact.

### Scenario 6: Application cannot access a file

Troubleshooting steps:

```bash
ls -l /path/to/file
namei -l /path/to/file
getfacl /path/to/file
```

Check:
1. Is the application running as the expected user?
2. Does the user have permission to read or execute the file?
3. Does the user have execute/search permission on every parent directory?
4. Are ACL entries or ACL masks restricting access?
5. Is the filesystem mounted read-only?
6. Are additional security controls such as SELinux or AppArmor denying access?

**Important:** Linux file permissions are only one layer of access control. A correct `chmod` command does not automatically resolve SELinux, AppArmor, or read-only filesystem restrictions.

---

## 14. Common Permission Errors and Mistakes

### Mistake 1: Using `chmod 777` to fix every problem

Why it is wrong:
- Gives all users read, write, and execute permissions.
- Can expose sensitive files.
- Can allow unauthorized modification.

Better approach:

```bash
chmod 755 deploy.sh
```

Or select permissions based on the actual requirement.

### Mistake 2: Forgetting directory execute permission

A user may have read permission on a directory but still be unable to access entries inside it if execute/search permission is missing.

Check:

```bash
ls -ld directory
```

### Mistake 3: Changing permissions but ignoring ownership

Even if the permission bits look correct, the file may belong to a different user or group.

Check:

```bash
ls -l filename
```

### Mistake 4: Applying recursive permissions without checking file types

This command:

```bash
chmod -R 755 /var/www/html
```

also gives execute permission to regular files.

Use recursive permission changes carefully. Select appropriate permissions separately for files and directories when needed.

### Mistake 5: Confusing chmod and chown

- `chmod` changes permissions.
- `chown` changes ownership.
- `chgrp` changes group ownership.

---

## 15. Hands-on Practice Exercises

Perform these exercises inside a safe practice directory. Do not experiment on important system directories.

### Exercise 1: Create practice files

```bash
mkdir permissions-practice
cd permissions-practice
touch file1.txt
touch deploy.sh
mkdir shared
```

### Exercise 2: Check permissions

```bash
ls -l
```

Question: What permissions are assigned to the new files and directory?

### Exercise 3: Change permissions

Give the owner read and write permissions, and give group and others read-only permissions:

```bash
chmod 644 file1.txt
```

Verify:

```bash
ls -l file1.txt
```

### Exercise 4: Make a script executable

```bash
chmod u+x deploy.sh
```

Check:

```bash
ls -l deploy.sh
```

### Exercise 5: Change group ownership

If a group named `developers` exists:

```bash
sudo chgrp developers file1.txt
```

### Exercise 6: Change owner and group

If the user and group exist:

```bash
sudo chown username:developers file1.txt
```

Replace `username` with an actual user on your system.

### Exercise 7: Understand umask

```bash
umask 027
touch newfile.txt
mkdir newdir
ls -ld newdir
ls -l newfile.txt
```

Observe the resulting permissions.

### Exercise 8: Practice SGID

```bash
chmod 2775 shared
ls -ld shared
```

Observe the `s` in the group permission position.

### Exercise 9: Practice sticky bit

Create a practice directory and set the sticky bit:

```bash
mkdir sticky-practice
chmod 1777 sticky-practice
ls -ld sticky-practice
```

Observe the `t` in the others permission position.

### Exercise 10: Practice ACL

If ACL tools are installed:

```bash
setfacl -m u:username:r file1.txt
getfacl file1.txt
```

Replace `username` with a real user on your system.

---

## 16. Linux File Permissions – Interview Questions and Answers

### Q1. What are Linux file permissions?

Linux file permissions control who can read, write, and execute files and directories.

### Q2. What are the three basic permission types?

Read (`r`), write (`w`), and execute (`x`).

### Q3. What are owner, group, and others?

- Owner: User who owns the file.
- Group: Users belonging to the file's associated group.
- Others: All remaining users.

### Q4. What is chmod?

`chmod` changes file and directory permissions.

### Q5. What is chown?

`chown` changes the owner of a file or directory and can also change its group.

### Q6. What is chgrp?

`chgrp` changes the group ownership of a file or directory.

### Q7. What is the difference between chmod and chown?

`chmod` changes permissions, whereas `chown` changes ownership.

### Q8. What does chmod 755 mean?

- Owner: Read, write, execute.
- Group: Read, execute.
- Others: Read, execute.

### Q9. What does chmod 644 mean?

- Owner: Read and write.
- Group: Read only.
- Others: Read only.

### Q10. What does chmod 777 mean?

Everyone has read, write, and execute permissions. It is generally unsafe for sensitive files and directories.

### Q11. What is umask?

Umask controls which permission bits are removed from the default permissions when new files and directories are created.

### Q12. What permissions are normally used as the base for new files and directories?

- Files: `666`
- Directories: `777`

The final permissions depend on the umask and the permissions requested by the creating application.

### Q13. What is SUID?

SUID causes an executable program to run with the effective user ID of the file owner, subject to system restrictions.

### Q14. What is SGID?

SGID can make an executable run with the effective group ID of its group. On directories, it helps new entries inherit the directory's group.

### Q15. What is the sticky bit?

The sticky bit restricts deletion and renaming of entries in a shared writable directory to the file owner, directory owner, or a privileged user.

### Q16. What is the difference between SUID, SGID, and sticky bit?

- SUID: Controls the effective user ID of an executable.
- SGID: Controls the effective group ID of an executable and group inheritance in directories.
- Sticky bit: Restricts deletion and renaming in shared directories.

### Q17. What is ACL in Linux?

ACL stands for Access Control List. It provides more detailed permission management for specific users and groups.

### Q18. Which commands are used to manage ACL?

- `getfacl`: Displays ACL entries.
- `setfacl`: Modifies ACL entries.

### Q19. How do you give execute permission to a script?

```bash
chmod +x script.sh
```

### Q20. How do you check file permissions?

```bash
ls -l filename
```

You can also use:

```bash
stat filename
```

### Q21. Why should we avoid chmod 777?

It grants all users full permissions and can allow unauthorized access or modification.

### Q22. What is the difference between file and directory permissions?

For files, read means viewing contents, write means modifying contents, and execute means running the file.

For directories, read means listing entries, write means modifying directory entries, and execute means searching or traversing the directory.

### Q23. How do you change ownership recursively?

```bash
sudo chown -R user:group directory
```

### Q24. What is the purpose of the SGID bit on a shared directory?

It helps newly created files and subdirectories inherit the shared directory's group, supporting team collaboration.

### Q25. How would you troubleshoot a Permission denied error?

I would check the file's permissions and ownership using `ls -l`, inspect parent directory permissions using `namei -l`, check ACLs using `getfacl`, and verify which user the application is running as. I would also check read-only mounts and additional security controls if necessary.

---

## 17. Important Points to Remember

- `r` means read, `w` means write, and `x` means execute/search.
- Linux permissions are divided into owner, group, and others.
- `chmod` changes permissions.
- `chown` changes ownership.
- `chgrp` changes group ownership.
- Numeric permissions use `4` for read, `2` for write, and `1` for execute.
- `755` is a common permission for executable scripts and directories.
- `644` is a common permission for ordinary readable files.
- `600` is useful for files that should be accessible only to their owner.
- `umask` controls default permission restrictions for newly created files and directories.
- SUID, SGID, and sticky bit are special permissions.
- ACL provides more granular access control.
- Avoid using `chmod 777` as a general troubleshooting solution.
- Always check ownership and parent directory permissions when troubleshooting access problems.

## 18. Final DevOps Interview Tip

For a fresher DevOps interview, you should be able to explain these practical situations confidently:

1. A deployment script is not executing because it lacks execute permission.
2. A web server cannot read application files because of incorrect ownership or permissions.
3. Multiple developers need access to a shared directory, so SGID can help maintain group ownership.
4. A configuration file contains sensitive information, so restrictive permissions such as `600` may be appropriate.
5. A shared temporary directory uses the sticky bit to protect users' files.
6. An application reports `Permission denied`, so you check ownership, permissions, ACLs, and parent directory traversal.

**Remember:** In DevOps, file permissions are important for application security, deployment automation, server administration, and troubleshooting production issues.
