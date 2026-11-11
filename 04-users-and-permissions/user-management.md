# Linux User Management

## 1. Introduction to Linux User Management

### Definition

Linux User Management is the process of creating, modifying, deleting, and managing user accounts on a Linux operating system.

Linux is a multi-user operating system, which means multiple users can access and use the same system with separate accounts.

Each user has an individual identity, permissions, groups, and access privileges.

### Why is user management important in DevOps?

- To create accounts for developers and system administrators.
- To provide secure access to Linux servers.
- To manage permissions for different users.
- To prevent unauthorized access.
- To assign users to appropriate groups.
- To manage application and service accounts.
- To maintain security in production environments.

---

## 2. Types of Users in Linux

Linux generally has three categories of user accounts.

### 2.1 Root User

**Definition:** The root user is the superuser account in Linux. It has administrative privileges and can perform almost all system operations.

The root user has UID 0.

Examples of root operations:

- Installing software.
- Creating and deleting user accounts.
- Modifying system configuration files.
- Managing services.
- Changing file ownership.
- Managing system security.

Check the current user:

```bash
whoami
```

Switch to the root account:

```bash
su -
```

If the system uses sudo, you can obtain a root shell with:

```bash
sudo -i
```

**Important:** Avoid using the root account for everyday activities. Use a regular account with `sudo` when administrative privileges are needed.

### 2.2 Regular User

**Definition:** A regular user is an account created for normal system usage. It has limited privileges and generally cannot modify protected system resources without administrative authorization.

Examples:

- Developer account.
- Administrator account with limited privileges.
- Employee account.

Create a regular user:

```bash
sudo useradd prathyusha
```

Set the user's password:

```bash
sudo passwd prathyusha
```

### 2.3 System User

**Definition:** A system user is an account generally created to run system services and applications rather than for interactive human login.

Examples:

- Nginx service user.
- Database service user.
- Application service user.

Create a system user:

```bash
sudo useradd --system nginxuser
```

System users commonly have a lower UID range and may not have an interactive login shell. Exact UID ranges and defaults depend on the Linux distribution.

### Difference Between Root, Regular, and System Users

| User type | Purpose | UID |
|---|---|---|
| Root user | Full system administration | 0 |
| Regular user | Normal system usage | Usually 1000 or higher on many distributions |
| System user | Runs services and applications | Usually assigned from a system UID range |

---

## 3. Understanding UID and GID

### 3.1 UID (User ID)

**Definition:** UID stands for User Identifier. It is a numerical value used by Linux to identify each user account.

Every Linux user has a UID.

Example:

```bash
id prathyusha
```

Sample output:

```text
uid=1001(prathyusha) gid=1001(prathyusha) groups=1001(prathyusha)
```

Here:

- `uid=1001`: User ID.
- `gid=1001`: Primary Group ID.
- `groups=1001`: Group membership.

### 3.2 GID (Group ID)

**Definition:** GID stands for Group Identifier. It is a numerical value used by Linux to identify a group.

Groups allow multiple users to share access to resources.

Check a user's UID and GID:

```bash
id username
```

Example:

```bash
id prathyusha
```

### Difference Between UID and GID

| UID | GID |
|---|---|
| Identifies a user | Identifies a group |
| Stands for User ID | Stands for Group ID |
| Assigned to every user account | Assigned to every group |

---

## 4. Important Linux User Management Configuration Files

Linux stores user and group information in specific system files.

The most important files are:

1. `/etc/passwd`
2. `/etc/shadow`
3. `/etc/group`
4. `/etc/gshadow`

### 4.1 /etc/passwd

**Definition:** The `/etc/passwd` file stores basic information about Linux user accounts.

View its contents:

```bash
cat /etc/passwd
```

Example entry:

```text
prathyusha:x:1001:1001:Prathyusha:/home/prathyusha:/bin/bash
```

The file contains seven colon-separated fields.

| Field | Value | Description |
|---|---|---|
| 1 | prathyusha | Username |
| 2 | x | Password placeholder |
| 3 | 1001 | UID |
| 4 | 1001 | Primary GID |
| 5 | Prathyusha | User information or GECOS field |
| 6 | /home/prathyusha | Home directory |
| 7 | /bin/bash | Login shell |

**Important:** The `x` indicates that password information is stored separately, generally in `/etc/shadow`.

### 4.2 /etc/shadow

**Definition:** The `/etc/shadow` file stores password hashes and password-aging information for Linux user accounts.

View its contents with administrative privileges:

```bash
sudo cat /etc/shadow
```

Example entry:

```text
prathyusha:$y$j9T$examplehash:20000:0:99999:7:::
```

Important fields:

| Field | Description |
|---|---|
| 1 | Username |
| 2 | Encrypted password hash or password status marker |
| 3 | Last password change |
| 4 | Minimum password age |
| 5 | Maximum password age |
| 6 | Password warning period |
| 7 | Password inactivity period |
| 8 | Account expiration date |
| 9 | Reserved field |

**Important security point:** Password hashes are not stored as plain-text passwords. Access to `/etc/shadow` is restricted because it contains sensitive authentication information.

### 4.3 /etc/group

**Definition:** The `/etc/group` file stores information about Linux groups and their members.

View its contents:

```bash
cat /etc/group
```

Example entry:

```text
developers:x:1002:prathyusha,admin
```

It contains four fields:

| Field | Description |
|---|---|
| 1 | Group name |
| 2 | Group password placeholder |
| 3 | GID |
| 4 | Supplementary group members |

### 4.4 /etc/gshadow

**Definition:** The `/etc/gshadow` file stores secure group information, including group passwords and group administrator details.

View it:

```bash
sudo cat /etc/gshadow
```

It is protected because it contains sensitive group authentication information.

### Difference Between passwd, shadow, group, and gshadow

| File | Purpose |
|---|---|
| `/etc/passwd` | Basic user account information |
| `/etc/shadow` | Password hashes and password aging |
| `/etc/group` | Group information and members |
| `/etc/gshadow` | Secure group information |

**Interview point:** `/etc/passwd` and `/etc/group` contain account and group information, while `/etc/shadow` and `/etc/gshadow` contain sensitive authentication-related information.

---

## 5. Creating Users in Linux

Linux provides commands to create user accounts.

### 5.1 useradd

**Definition:** The `useradd` command creates a new user account in Linux.

Syntax:

```bash
sudo useradd username
```

Example:

```bash
sudo useradd prathyusha
```

This creates the user account using the system's default settings.

### 5.2 Create a user with a home directory

```bash
sudo useradd -m prathyusha
```

Explanation:

- `useradd`: Creates a user.
- `-m`: Creates the user's home directory.
- `prathyusha`: Username.

The home directory is generally created under `/home`.

Example:

```text
/home/prathyusha
```

### 5.3 Create a user with a specific home directory

```bash
sudo useradd -m -d /home/devops prathyusha
```

Here:

- `-m`: Creates the home directory.
- `-d`: Specifies the home directory path.

### 5.4 Create a user with a specific UID

```bash
sudo useradd -m -u 1500 developer
```

The `-u` option specifies the UID.

### 5.5 Create a user with a specific shell

```bash
sudo useradd -m -s /bin/bash developer
```

The `-s` option specifies the user's login shell.

### 5.6 Create a user with a specific primary group

```bash
sudo useradd -m -g developers developer
```

The `-g` option specifies the primary group. The group must already exist.

### 5.7 Create a user with supplementary groups

```bash
sudo useradd -m -G docker,sudo developer
```

The `-G` option assigns supplementary groups.

The specified groups must already exist.

**Note:** The administrative group may be `sudo` on Ubuntu or `wheel` on some other distributions.

---

## 6. Setting and Changing User Passwords

### passwd

**Definition:** The `passwd` command is used to set or change a user's password.

### 6.1 Set a password for a user

```bash
sudo passwd prathyusha
```

The system prompts you to enter and confirm a new password.

### 6.2 Change your own password

```bash
passwd
```

The system asks for the current password and then the new password.

### 6.3 Change another user's password

```bash
sudo passwd username
```

### 6.4 Lock a user account password

```bash
sudo passwd -l username
```

Locks the password authentication for the account.

### 6.5 Unlock a user account password

```bash
sudo passwd -u username
```

Unlocks a password-locked account.

### 6.6 Check password status

```bash
sudo passwd -S username
```

Displays the password status.

**Important:** Locking a password does not necessarily disable every possible way of accessing an account. SSH keys, other authentication methods, and active sessions may need separate attention.

---

## 7. Modifying Users Using usermod

### Definition

The `usermod` command modifies an existing Linux user account.

Syntax:

```bash
sudo usermod [options] username
```

### 7.1 Change a username

```bash
sudo usermod -l newname oldname
```

Changes the login name from `oldname` to `newname`.

### 7.2 Change a user's home directory

```bash
sudo usermod -d /home/newhome -m username
```

- `-d`: Specifies the new home directory.
- `-m`: Moves the contents of the existing home directory to the new location.

### 7.3 Change the user's login shell

```bash
sudo usermod -s /bin/bash username
```

Changes the login shell to Bash.

### 7.4 Change the user's primary group

```bash
sudo usermod -g developers username
```

Changes the user's primary group to `developers`.

### 7.5 Add a user to supplementary groups

```bash
sudo usermod -aG docker username
```

Explanation:

- `-a`: Append the group instead of replacing the existing supplementary groups.
- `-G`: Specifies supplementary groups.

**Very important:** Always use `-aG` when adding a user to supplementary groups unless you intentionally want to replace their existing supplementary group list.

For example:

```bash
sudo usermod -aG docker,sudo username
```

### 7.6 Lock a user account

```bash
sudo usermod -L username
```

### 7.7 Unlock a user account

```bash
sudo usermod -U username
```

---

## 8. Deleting Users in Linux

### userdel

**Definition:** The `userdel` command deletes a user account from Linux.

Syntax:

```bash
sudo userdel username
```

Example:

```bash
sudo userdel prathyusha
```

This removes the user account but does not normally remove the user's home directory.

### Delete a user with the home directory
```bash
sudo userdel -r prathyusha
```

The `-r` option removes the user's home directory and mail spool where applicable.

**Important:** Deleting a user does not automatically remove every file owned by that user elsewhere on the system. Such files may remain with the former UID as their owner.

### Before deleting a user

Check the account:

```bash
id username
```

Check whether the user has active sessions:

```bash
who
```

Review files and services owned by the user before deleting the account.

---

## 9. Linux Group Management

### Definition

A group is a collection of Linux user accounts. Groups make it easier to manage access for multiple users.

Instead of assigning access separately to each user, administrators can manage access through groups.

Example:

A DevOps team may have a group called `devops`, containing multiple team members.

### 9.1 groupadd

**Definition:** Creates a new group.

Syntax:

```bash
sudo groupadd groupname
```

Example:

```bash
sudo groupadd developers
```

### 9.2 groupdel

**Definition:** Deletes an existing group.

Syntax:

```bash
sudo groupdel groupname
```

Example:

```bash
sudo groupdel developers
```

A primary group cannot normally be deleted while it is still the primary group of an existing user.

### 9.3 groupmod

**Definition:** Modifies an existing group.

Change the group name:

```bash
sudo groupmod -n newgroup oldgroup
```

Change the group GID:

```bash
sudo groupmod -g 2000 developers
```

### 9.4 Add a user to a group

```bash
sudo usermod -aG developers username
```

### 9.5 Check group membership

```bash
groups username
```

Another command:

```bash
id username
```

### 9.6 Display all groups

```bash
cat /etc/group
```

### Difference Between Primary and Supplementary Groups

**Primary group:**

The main group assigned to a user account. New files created by the user normally inherit this group, subject to directory settings and other filesystem behavior.

**Supplementary group:**

Additional groups that provide the user with membership-based access to resources.

Example:

```bash
sudo usermod -g developers -aG docker,sudo prathyusha
```

Here:

- `developers` is the primary group.
- `docker` and `sudo` are supplementary groups.

---

## 10. Checking User Information

Linux provides several commands to identify users and inspect account information.

### 10.1 whoami

**Definition:** Displays the username of the currently effective user.

```bash
whoami
```

Example output:

```text
prathyusha
```

### 10.2 who

**Definition:** Displays users currently logged into the system.

```bash
who
```

### 10.3 w

**Definition:** Displays logged-in users and their current activities.

```bash
w
```

It can show login information, idle time, and current processes.

### 10.4 id

**Definition:** Displays the UID, GID, and group memberships of a user.

```bash
id
```

For a specific user:

```bash
id prathyusha
```

### 10.5 groups

**Definition:** Displays the groups to which a user belongs.

```bash
groups
```

For a specific user:

```bash
groups prathyusha
```

### 10.6 getent

**Definition:** Retrieves entries from system databases such as users and groups.

Display a user account:

```bash
getent passwd prathyusha
```

Display a group:

```bash
getent group developers
```

**DevOps point:** `getent` is useful because it can retrieve account information from configured identity sources, not only local files.

### 10.7 last

**Definition:** Displays previous login sessions using system login records.

```bash
last
```

This is useful for auditing and troubleshooting user login activity.

### 10.8 lastlog

**Definition:** Displays the most recent login information for user accounts on systems that provide the command and its supporting database.

```bash
lastlog
```

Availability can vary across Linux distributions.

---

## 11. Managing User Password Expiration Using chage

### Definition

The `chage` command manages password aging and expiration settings for user accounts.

It helps administrators enforce password-expiration policies.

### 11.1 View password aging information

```bash
sudo chage -l username
```

Displays password aging information.

### 11.2 Set maximum password age

```bash
sudo chage -M 90 username
```

Sets the maximum password age to 90 days.

### 11.3 Set minimum password age

```bash
sudo chage -m 7 username
```

Sets the minimum password age to 7 days.

### 11.4 Set password warning period

```bash
sudo chage -W 7 username
```

Warns the user 7 days before password expiration.

### 11.5 Set account expiration date

```bash
sudo chage -E 2027-12-31 username
```

Sets the account expiration date.

**Important:** Password expiration and account expiration are different. Password expiration requires a password change, while account expiration can prevent account access after the configured date.

---

## 12. Switching Between Users

Linux provides commands to switch between user accounts.

### 12.1 su

**Definition:** The `su` command allows a user to switch to another user account.

Syntax:

```bash
su username
```

Example:

```bash
su prathyusha
```

To switch to root with a login environment:

```bash
su -
```

The command normally requires the target account's password, depending on the system configuration.

### 12.2 sudo

**Definition:** The `sudo` command allows an authorized user to execute commands with another user's privileges, usually root privileges.

Example:

```bash
sudo apt update
```

Run a command as another user:

```bash
sudo -u username whoami
```

### Difference Between su and sudo

| su | sudo |
|---|---|
| Switches to another user account | Executes commands with another user's privileges |
| Commonly requires the target user's password | Commonly authenticates using the invoking user's password |
| Can open a shell as another user | Can execute one command or open a privileged shell |
| Uses the target account's environment depending on options | Uses configured sudo environment and policy |

**Interview point:** `sudo` is commonly preferred for controlled administrative access because permissions can be granted to specific users and commands.

---

## 13. Understanding User Login Shells

### Definition

A login shell is the command-line environment started when a user logs in to a Linux system.

It allows users to execute commands and interact with the operating system.

Common shells:

| Shell | Description |
|---|---|
| `/bin/bash` | Bourne Again Shell |
| `/bin/sh` | Standard shell interface, often linked to an implementation |
| `/bin/zsh` | Z Shell |
| `/bin/fish` | Friendly Interactive Shell |
| `/usr/sbin/nologin` | Prevents normal interactive login |

Check the current shell:

```bash
echo $SHELL
```

Check available shells:

```bash
cat /etc/shells
```

Change a user's shell:

```bash
sudo usermod -s /bin/bash username
```

Set a non-interactive login shell:

```bash
sudo usermod -s /usr/sbin/nologin serviceuser
```

**DevOps use case:** Service accounts often use non-interactive shells to reduce unnecessary interactive access.

---

## 14. Understanding sudoers

### Definition

The sudoers file defines which users and groups are authorized to execute commands using `sudo`.

The main configuration file is:

```text
/etc/sudoers
```

View the configuration:

```bash
sudo cat /etc/sudoers
```

### Edit sudoers safely

Use:

```bash
sudo visudo
```

`visudo` checks the syntax before saving changes, helping prevent configuration errors that could break sudo access.

### Example sudoers entry

```text
prathyusha ALL=(ALL:ALL) ALL
```

This generally allows `prathyusha` to run commands as any user and group on any host where the configuration applies.

### Give sudo access through a group

On Ubuntu, authorized administrators are commonly members of the `sudo` group.

Example:

```bash
sudo usermod -aG sudo username
```

On many RHEL-based distributions, the `wheel` group is commonly used.

Example:

```bash
sudo usermod -aG wheel username
```

The exact configuration depends on the distribution and sudoers policy.

**Important:** Never edit `/etc/sudoers` directly with an ordinary text editor. Use `visudo`.

---

## 15. Practical DevOps User Management Scenarios

### Scenario 1: Create a new developer account

Requirement: Create a user called `developer` with a home directory.

```bash
sudo useradd -m developer
sudo passwd developer
```

Explanation:

- Creates the user.
- Creates the home directory.
- Sets the password.

### Scenario 2: Create a DevOps group

Requirement: Create a group called `devops`.

```bash
sudo groupadd devops
```

### Scenario 3: Add a developer to the DevOps group

```bash
sudo usermod -aG devops developer
```

Verify:

```bash
groups developer
```

### Scenario 4: Give a user Docker group membership

Requirement: Allow an authorized user to access the Docker group.

```bash
sudo usermod -aG docker developer
```

The user may need to log out and log in again for the new group membership to appear in a new session.

**Security note:** Membership in the Docker group can provide root-equivalent control over many Linux systems. Grant it only to trusted users.

### Scenario 5: Lock a user account

Requirement: Temporarily lock password authentication for a user.

```bash
sudo passwd -l developer
```

Check the status:

```bash
sudo passwd -S developer
```

Remember that this does not necessarily terminate existing sessions or disable SSH-key authentication.

### Scenario 6: Remove a user from a supplementary group

On systems using GNU `gpasswd`:

```bash
sudo gpasswd -d developer devops
```

Verify:

```bash
groups developer
```

### Scenario 7: Delete a user and their home directory

```bash
sudo userdel -r developer
```

Review the user's files and running processes before performing this operation.

### Scenario 8: Find users currently logged in

```bash
who
```

For more information about their activity:

This example searches local `/etc/passwd` entries for accounts using `/usr/sbin/nologin`.


---


## 16. Important Linux User Management Commands – Quick Revision


| Command | Purpose |
|---|---|

| `useradd` | Create a user |
| `usermod` | Modify a user |
| `userdel` | Delete a user |

| `passwd` | Set or change a password |
| `groupadd` | Create a group |

| `groupmod` | Modify a group |

| `groupdel` | Delete a group |

| `groups` | Display group membership |

| `id` | Display UID, GID, and groups |

| `whoami` | Display current effective username |

| `who` | Display logged-in users |

| `w` | Display logged-in users and activity |
| `getent` | Retrieve user and group database entries |

| `su` | Switch user |
| `sudo` | Execute commands with authorized privileges |

| `chage` | Manage password aging |
| `last` | Display previous login sessions |

| `visudo` | Safely edit sudoers configuration |


---


## 17. Practical Exercises


Perform these exercises on a Linux virtual machine or another test environment where you have administrative access.


1. Check your current username using `whoami`.
2. Display your UID and GID using `id`.

3. Display your group membership using `groups`.
4. View the `/etc/passwd` file.

5. View the `/etc/group` file.
6. Create a user named `testuser`.

7. Create a home directory for `testuser`.
8. Set a password for `testuser`.

9. Create a group named `developers`.
10. Add `testuser` to the `developers` group.
11. Verify the group membership.
12. Change the user's login shell to `/bin/bash`.
13. Display the user's password aging information using `chage`.
14. Lock and unlock the user's password.

15. Display currently logged-in users.
16. Check the previous login records using `last`.
17. Delete the test user after completing the exercises.
18. Delete the test group if it is no longer needed.

**Note:** Perform user deletion and group deletion only on test accounts and groups that you created.

---

## 18. Linux User Management Interview Questions and Answers

### Q1. What is user management in Linux?

User management is the process of creating, modifying, deleting, and managing user accounts and their access privileges in Linux.


### Q2. What are the different types of users in Linux?


The commonly discussed categories are root users, regular users, and system users.


### Q3. What is the root user?


The root user is the superuser account with UID 0 and administrative privileges.


### Q4. What is UID?


UID stands for User Identifier. It is a numerical value used to identify a Linux user.


### Q5. What is GID?

### Q6. What is the difference between UID and GID?

GID stands for Group Identifier. It is a numerical value used to identify a Linux group.


UID identifies a user, while GID identifies a group.


### Q7. What is the purpose of /etc/passwd?

It stores basic Linux user account information, including username, UID, primary GID, home directory, and login shell.

### Q8. What is the purpose of /etc/shadow?

It stores password hashes and password-aging information. Access is restricted because the information is sensitive.

### Q9. What is the difference between /etc/passwd and /etc/shadow?
`/etc/passwd` stores basic user account details, whereas `/etc/shadow` stores password hashes and password-aging information.

### Q10. How do you create a new user?

```bash
sudo useradd username
```


```bash
sudo useradd -m username

### Q11. How do you set a user's password?

```bash
```

### Q12. What is the difference between useradd and usermod?


### Q13. How do you delete a user?

```bash
sudo userdel username
```

### Q14. How do you delete a user along with the home directory?

```bash
sudo userdel -r username
```

### Q15. What is the purpose of groupadd?

`useradd` creates a new user account, whereas `usermod` modifies an existing user account.
sudo passwd username
```
The `groupadd` command creates a new Linux group.
To create the user with a home directory:



### Q16. What is the difference between primary and supplementary groups?


A primary group is the main group assigned to a user. Supplementary groups provide additional group memberships and access.


### Q17. How do you add a user to a group?


```bash
sudo usermod -aG groupname username

```


### Q18. Why is -aG important in usermod?


`-aG` appends the specified supplementary group without replacing the user's existing supplementary group memberships.


### Q19. How do you check a user's groups?


```bash
groups username

```


### Q20. How do you check a user's UID and GID?


```bash
id username

```


### Q21. What is the difference between su and sudo?


`su` switches to another user account, while `sudo` executes commands with authorized privileges, commonly root privileges.


### Q22. What is the purpose of the passwd command?

The `passwd` command sets or changes user passwords and can also lock or unlock password authentication.

### Q23. How do you lock a user's password?

```bash
sudo passwd -l username
```

### Q24. How do you unlock a user's password?
who

### Q28. What is a system user?


### Q29. What is the purpose of /etc/group?

It stores group information, including group names, GIDs, and supplementary group members.

### Q30. What is the purpose of visudo?

`visudo` safely edits the sudoers configuration and checks its syntax before saving.

### Q31. How do you give a user sudo access on Ubuntu?

```bash
sudo usermod -aG sudo username
```

The user must have permission to run the administrative command that modifies group membership.

### Q32. What happens when you add a user to a new group?

The user receives supplementary group membership. A new login session is generally needed for the updated membership to apply to newly started processes.

### Q33. How do you check whether a user exists?

```bash
id username
```

Another method:

```bash
getent passwd username
```

### Q34. How do you change a user's login shell?

```bash
sudo usermod -s /bin/bash username
```

### Q35. Why should we avoid using the root account for everyday tasks?

Using root for routine tasks increases the risk of accidental system damage and security incidents. Using a regular account with controlled `sudo` access is safer.

---

## 19. Key Takeaways

- Linux is a multi-user operating system.
- Root is the superuser account with UID 0.
- UID identifies users, while GID identifies groups.
- `/etc/passwd` stores basic user account information.
- `/etc/shadow` stores password hashes and aging information.
- `/etc/group` stores group information.
- `/etc/gshadow` stores secure group information.
- `useradd` creates users.
- `usermod` modifies users.
- `userdel` deletes users.
- `groupadd` creates groups.
- `passwd` manages passwords.
- `id` and `groups` display user information.
- `sudo` provides controlled administrative access.
- `chage` manages password aging.
- `visudo` safely manages sudoers configuration.

Linux user management is an essential skill for DevOps engineers because it helps maintain secure server access, application accounts, and team-based access control.A system user is generally an account used to run services and applications rather than for normal interactive human login.

`who` displays logged-in users, while `w` also provides information about their current activity and system usage.
### Q27. What is the difference between who and w?

```
```bash


### Q26. How do you check the current logged-in users?

The `chage` command manages password aging and expiration settings.

### Q25. What is the purpose of chage?

```bash
```
sudo passwd -u username

