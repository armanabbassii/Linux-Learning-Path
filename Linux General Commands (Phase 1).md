**Learning path:**  
1- To better understand the meaning of each command, it is helpful to first look at what each command is an abbreviation for.
2- Each command is written in the following form:
- `command`
  3- In the **permission section**, we review the operations related to assigning permissions and user roles.
  4- In the **Extra Info** section, additional commands are explained.

---------------------
- pwd
> *print work directory

- ls
> *list

- ls -l
> *displays files and directories in long format with detailed metadata.

* ls -1
> *prints one file or directory name per line.

- man
> *manual page

```(A man page, short for **manual page**, is a form of software documentation found on Unix and Unix-like operating systems. Topics covered include programs, system libraries, system calls, and sometimes local system details.)```

*  mkdir folder_name
> *make directory (creates a new directory with the specified name)

* cd directory_name
> *change directory ( Changes the current working directory to `directory_name`.**)

- cd ..
> *Moves to the parent directory.

- cd ~
> *Changes to the user’s home directory.

* cat file.text
> *Displays the contents of a file in the terminal.***

```It stands for “**concatenate**” and is primarily used to read, display, and concatenate text files```

- touch file.text
> *Creates an empty file or updates the file’s timestamp if it already exists.
```
The `touch` command in Linux is**a fundamental tool used primarily for creating empty files and updating the timestamps of existing files
```

- echo "Hello Linux"
> *Prints text to standard output (the terminal).

- echo "Hello Linux" > file.txt
> *Writes text to a file (overwrites existing content).

* nano file.txt
>*Opens a file in the Nano text editor for interactive editing.
>	If the file does not exist:
>    Nano creates the file automatically.
```
Nano is a **command-line text editor** that comes pre-installed with most Linux distributions.
```

* mv old.txt new.txt
> *Renames a file from `old.txt` to `new.txt`.

- mv file.txt /path/to/directory/
> *Move a file to another directory


----
#### A bit Advance (Part 01)
* grep "error" syslog
> Searches for a text pattern inside a file and prints matching lines.
> 	The command above means:  "Finds lines containing the word `error`".
```
**global regular expression print**”, is a command used for searching and matching text patterns in files contained in the regular expressions. 
Furthermore, the command comes pre-installed in every Linux distribution
```
example :
- grep -i error file.text
> *Case-insensitive search.


- ps aux | grep firefox
> *This command means: Shows all running processes and filters the output to display only Firefox-related processes.

Breakdown of `ps aux`
- **`ps`**: The command to report a snapshot of current processes.
- **`a` (all)**: Shows processes for all users, not just your own.
- **`u` (user-oriented)**: Displays detailed user information (username, CPU%, MEM%, etc.).
- **`x` (no terminal)**: Includes processes that aren't attached to a terminal (like background services/daemons).
- ------
#### A bit Advance (Part 02) - Related to Permissions:
- sudo chown dotin:logreaders syslog
> *chown:
Changes the owner and group of a file or directory.
```
short for **change owner**, is a shell command for changing the owning user of Unix-based file system files – including special files such as directories. **The ownership of a file may only be altered by a super-user (such as via sudo )**.
```

- chmod 640 syslog
> chmod:
*Changes file or directory permissions (read, write, execute).
```
The chmod (short for **change mode**) command is used to manage file system access permissions on Unix and Unix-like systems.
```

- sudo usermod -aG logreaders guest5
>  *usermod:
> 	Modifies an existing user account.
```
The `usermod` command in Linux/Unix ==doesn't have a single abbreviation==; it stands for "**user modify**," used to change existing user account properties like name (`-l`), home directory (`-d`), shell (`-s`), or group memberships (`-aG`), acting as the modification counterpart to `useradd` (create) and `userdel` (delete)
```

- su - guest5
> su:
*switch user

----
## Permission Section

### Firsts Step:
* cp /var/log/syslog .
> *Copies the `syslog` file into the current directory.

### Second Step:
- sudo useradd guest
> *Creates a new user named `guest`.
- useradd guest5
> *Permission denied means you don’t have enough privileges.

- sudo passwd guest
> *Sets or changes the password for the user `guest`.

- sudo groupadd logreaders
> *create a group, because:

```
Group permissions are assigned **to files**, not **to users**.  
A user can only **be a member of one or more groups**.
```

### Third Step:
- sudo usermod -aG logreaders guest5
> *Adds the user `guest5` to the `logreaders` supplementary group.

### what is meaning of "-aG"

##### `-a` → **Append (VERY IMPORTANT)**
`-a` means:
> append → **add without removing existing groups**

##### `-G` → **Supplementary Groups**
`-G` means:
> set the **additional (secondary) groups** for a user


- sudo chown dotin:logreaders syslog
> thats mean:
> *“syslog is assigned to the logreaders group.”

## Fourth step (setting a specific permission level for a file):
- sudo chmod 640 syslog
> *Sets the permissions of the file `syslog` to `640`.

#### Permission translation:
6  4  0
│  │  │
│  │  └─ others
│  └──── group
└─────── owner

#### Table of Permission:

| permission | word | count |
| ---------- | ---- | ----- |
| read       | r    | 4     |
| write      | w    | 2     |
| execute    | x    | 1     |

🔹 **File permissions** control whether you can:
- read the file
- write to the file
- execute the file

🔹 **Directory permissions** control whether you can:
- list its contents (`ls`)
- access or open files inside it
- create or delete files within it


## Fifth step:

- su - guest5
> *change currently user to added user:
- id
> *id or whoami  for identified user

### Checking user permissions:
- $ cat syslog
> ****Result:** allowed (read permission)

- nano syslog
> ****Result:** permission denied (write permission not granted)



********************************
# Extra Info:
### How to display users of a group:

	- getent group logreaders
> *Displays information about the `logreaders` group and its members.
**respose**: logreaders:x:1008:guest5

##### Details:
logreaders:x:1008:guest5
│          │   │     └── group members
│          │   └──────── GID (Group ID)
│          └──────────── password placeholder
└─────────────────────── group name



##  How to remove a user from a group
### Before that: Group types in Linux

Linux has **two types of groups** for a user:
1- Primary group
2- Supplementary (secondary) groups

#### 1️⃣ Primary group (when user created)
The **primary group** is assigned when the user is created.
- id guest5

> **response**
> uid=1006(guest5) gid=1007(guest5) groups=1007(guest5)

### 2️⃣ Supplementary groups (extra groups)
> Supplementary groups provide **additional permissions**.

#### Now:
- sudo usermod -G logreaders guest5

```
it removes `guest5` from all supplementary groups** and keeps **only** `logreaders`.
```

> For example, if **guest5** is a member of three groups (A, B, C), and you want to remove it from **group A**, you should handle it this way:

- sudo usermod -G b,c guest5

>  **So:**  sudo usermod -aG logreaders guest5
means:
“Keep all the groups it already has, and just add this one too.”


## How to delete a user :
- sudo userdel guest5

>  **This removes the user account, but **does NOT delete**:
`/home/guest5`
files owned by that user elsewhere

>   *->**Delete user and their home directory (most common at the end)**
- sudo userdel -r guest5

>   What `-r` does:
removes `/home/guest5`
“Erase this user from the system, not just the name.”


## How to show **supplementary groups of a user**:

- groups guest5

> **response:**
guest5 : guest5 logreaders


### How to list **all users** on a Linux system:
- getent passwd
> Lists all users known to the system (regular and system users).
### Show currently logged-in users:
- who
- users
> Displays users who are currently logged in.
### Show the current user
- whoami
> Displays the username of the current session.