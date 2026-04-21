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
bin - binary/program/executable files which through we executes linux commands
boot - 
cdrom - 
dev -
etc -
home - 
lib - 
lib32 - 
lib64 - 
libx64 - 
lost+found - 
media - 
mnt - 
opt - 
proc - 
root - 
run - 
sbin - 
snap - 
srv - 
swapfile - 
sys - 
tmp - 
usr - 
var - 

```

