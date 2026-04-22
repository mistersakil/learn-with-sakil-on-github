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

## Access Linux Machine Using SSH | Filesystem Hierarchy in Linux

After login normally you will be inside authenticated user home directory like this (in my case, you will see something different but in similar pattern) `root@Ubuntu:~#.`

Here root is username, Ubuntu is host name, ~ means home directory and # means you are logged in as system root user.

Now go to system root directory.
`root@Ubuntu:~# cd /`

Here we can see almost 19 directory. We are going to explain them:

```rootDirectories
* bin - binary/program/executable files which through we executes linux commands
* boot - The boot directory holds the static files used by the bootloader to initialize the system and load the kernel into memory.
* cdrom - 
* dev - device related program files to perform I/O (ex: mouse, printer, keyboard etc)
* etc -
* home - users home directory. all individual users will be included here
* lib - os library related program files
* lib32 - 32 bit os library related program files
* lib64 - 64 bit os library related program files
* libx64 - 
* lost+found - 
* media - 
* mnt - Initially empty directory. mounting or unmounting portable devices (ex: usb devices)
* opt - 
* proc - OS Process related executable files. 
* root - Root directory of OS
* run - Removable devices can be see here
* sbin - Super user binary files
* snap - 
* srv - Serving tmp data (ex: ftp)
* swapfile - 
* sys - 
* tmp - To store temporary data
* usr - 
* var - var directory (short for variable) is used to store data that changes (logs) frequently during system operation

```

## Getting Help & Man Pages in Linux | Command Details & Help

'man' an interface to the system reference manuals.

```manPages
* man man =  an interface to the system reference manuals
* man lsblk = list of block devices.
* man sshd = OpenSSH ssh demon
* man mandb = create or update the manual page index caches
* whereis mandb  = route: location/of/binary location/of/manual (to find out location of process and manual)
* whatis mandb = create or update the manual page index caches
* man 5 mandb = manual of perticular index of a process
* cd --help = to get helper/options manual of a process
```
