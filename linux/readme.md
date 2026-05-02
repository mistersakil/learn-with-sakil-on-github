# Learning Linux

## Linux basic cli commands

- Check Linux OS Name and Version `hostnamectl`
- Show directory listing `ls -l`
- Back to the root directory `cd /` or `cd ~`
- Copy Everything Inside a Folder `cp -r /copy/from/* /destination/path/`
- Copy Everything Inside a Folder including (.filename) files `cp -r /copy/from/. /destination/path/`
- Set os the static hostname `hostnamectl set-hostname your-new-hostname`
- Install a basic editor inside the container or linux distribution (if needed) `apt update && apt install nano -y`
- See cpu info `cat /proc/cpuinfo`

## Ecosystem-based classification

| Distro | Ecosystem | Type | Use case |
| ------ | --------- | ---- | -------- |
| CentOS Linux | RHEL | Stable | Avoid now |
| CentOS Stream | RHEL | Rolling preview | Testing |
| Rocky Linux | RHEL | Stable | Production |
| AlmaLinux | RHEL | Stable | Production |
| Kali Linux | Debian | Specialized | Security only |
| Parrot OS | Debian | Specialized | Security/dev |
| openSUSE | Independent | Stable/Rolling | DevOps/server |

## Access Linux Machine Using SSH | Filesystem Hierarchy in Linux

After login normally you will be inside authenticated user home directory like this (in my case, you will see something different but in similar pattern) `root@Ubuntu:~#.`

Here root is username, Ubuntu is host name, ~ means home directory and # means you are logged in as system root user.

Now go to system root directory.
`root@Ubuntu:~# cd /`

Here we can see almost 19 directory. We are going to explain them:

```rootDirectories
* bin - binary/program/executable files which through we executes linux commands.
* boot - The boot directory holds the static files used by the bootloader to initialize the system and load the kernel into memory.
* cdrom - 
* dev - device related program files to perform I/O (ex: mouse, printer, keyboard etc).
* etc -
* home - users home directory. all individual users will be included here.
* lib - os library related program files.
* lib32 - 32 bit os library related program files.
* lib64 - 64 bit os library related program files.
* libx64 - 
* lost+found - 
* media - 
* mnt - Initially empty directory. mounting or unmounting portable devices (ex: usb devices).
* opt - 
* proc - OS Process related executable files. 
* root - Root directory of OS.
* run - Removable devices can be see here.
* sbin - Super user binary files.
* snap - 
* srv - Serving tmp data (ex: ftp).
* swapfile - 
* sys - 
* tmp - To store temporary data.
* usr - 
* var - var directory (short for variable) is used to store data that changes (logs) frequently during system operation.

```

## Getting Help & Man Pages in Linux | Command Details & Help

*'man' an interface to the system reference manuals.*

```manPages
* man man =  an interface to the system reference manuals.
* man lsblk = list of block devices.
* man sshd = OpenSSH ssh demon.
* man mandb = create or update the manual page index caches.
* whereis mandb  = route: location/of/binary location/of/manual (to find out location of process and manual).
* whatis mandb = create or update the manual page index caches.
* man 5 mandb = manual of perticular index of a process.
* cd --help = to get helper/options manual of a process.
```

## Working With Directories In Linux | Manage Directories

*Everything in linux is files even directory is a special kinda files.*

```workingWithDirectories
* pwd = Present working directory.
* cd pathname = change directory based on relative path.
* cd /pathname = change directory based on absolute path.
* cd ~ = change to users home directory. 
* cd - = traverse to previous directory. 
* ls = list all items inside current directory.
* ls /var/log = list all items inside given directory.
* ls -l = long list of all items.
* ls -la = long list with hidden items.
* ls -lah = hidden long list with human readable memory size format.
* ls -ldh = long list of directory with human readable memory size.
* ls -li = long list with i-nodes.
* mkdir dirName = Create a empty directory (ex: mkdir /tmp/fedora).
* mkdir -p parentDirName/childDirName = Create a empty directory with parent child relationship (ex: mkdir /tmp/fedora/centos).
* rmdir pathOfDir = Remove empty directory.
* rmdir dir1 dir2 dir3 = Remove multiple empty directory.
* rm -rf parentDirName/childDirName = Remove all parent child relational directory forcefully. 

```

***Meaning of DOTS (. & ..)***

```dotsMeaning
. = A reference of current directory
.. = A reference of parent directory
```

## File Permission / Access Modes

Each file has **3 types of users**:

- **Owner (u)** → the person who created the file  
- **Group (g)** → users in the same group  
- **Others (o)** → everyone else  

***Permission Types:***

Each user can have these 3 types of permissions

| Permission | Meaning for file | Meaning for directory|
| ------ | --------- | ---- |
| r | Read file contents | List files (ls) |
| w | Modify file | Create/delete files |
| x | Execute file | Enter directory (cd) |

***Permission Format:***

Example 1: (-rwxr-xr--)
Example 2: (drwxr-xr-x)
Example 3: (lrwxrwxrwx)

This has **10 characters**:

***1. File Type (First Character)***

- `-` = regular file  
- `d` = directory  
- `l` = link  

***2. Permissions (Next 9 Characters)***

Split into 3 groups:

```splitInto3Group
rwx r-x r--
│   │  │
│   │  └── Others (u)
│   └──────── Group (g)
└────────────── Owner (o)
```

***3. Example Explanation***

- **Owner** → `rwx` → can read, write, execute  
- **Group** → `r-x` → can read, execute  
- **Others** → `r--` → can only read  

***More Examples***

- `drwxr-xr-x` → directory, everyone can read & enter
- `lrwxrwxrwx` → link, full access for all

***Using chmod in Symbolic Mode:***

| Symbol | Description |
| ------ | --------- |
| + | Adds the designated permission(s) to a file or directory |
| - | Removes the designated permission(s) from a file or directory |
| = | Sets the designated permission(s) |

Here's an example using testfile (devops.txt). Running ls -lh on the testfile shows that the file's permissions are as follows −

```SymbolicModeExample
$ls -lh devops.txt
-rw-r--r-- 1 root root 0 May  2 02:52 devops.txt

$chmod o+wx devops.txt
$ls -lh devops.txt
-rw-r--rwx 1 root root 0 May  2 02:52 devops.txt

$chmod u+x devops.txt
$ls -lh devops.txt
-rwxr--rwx 1 root root 0 May  2 02:52 devops.txt

$chmod u-w devops.txt
$ls -lh devops.txt
-r-xr--rwx 1 root root 0 May  2 02:52 devops.txt

$chmod g=rw devops.txt
$ls -l testfile
-r-xrw-rwx 1 root root 0 May  2 02:52 devops.txt
```

Here's how you can combine these commands on a single line-

```SymbolicModeExampleCombination
$chmod o-wx,u+w,g=rx devops.txt
$ls -lh devops.txt
-rwxr-xr-- 1 root root 0 May  2 02:52 devops.txt
```

***Numeric (Octal) Representations:***

Each permission has a numeric value

| Permission | Value |
| ------ | --------- |
| r | 4 |
| w | 2 |
| x | 1 |

*Add them:*

```AddThem
rwx = 4+2+1 = 7
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
```

*Example:*

```NumericExampleExplain
command : chmod 754 file.txt
--means--
> Owner → 7 (rwx)
> Group → 5 (r-x)
> Others → 4 (r--)
```

***Think of it like:***

> Who (user/group/others) can do what (read/write/execute) on which file
