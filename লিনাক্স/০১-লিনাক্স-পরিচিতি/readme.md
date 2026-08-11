# লিনাক্স শেখা

## লিনাক্সের প্রাথমিক CLI কমান্ড

- লিনাক্সের OS-এর নাম ও ভার্সন দেখুন: `hostnamectl`
- ডিরেক্টরির তালিকা দেখুন: `ls -l`
- সিস্টেমের root directory-তে যান: `cd /` অথবা `cd ~`
- একটি ফোল্ডারের ভেতরের সবকিছু কপি করুন: `cp -r /copy/from/* /destination/path/`
- hidden (`.filename`) ফাইলসহ একটি ফোল্ডারের সবকিছু কপি করুন: `cp -r /copy/from/. /destination/path/`
- Static hostname সেট করুন: `hostnamectl set-hostname your-new-hostname`
- Container বা Linux distribution-এর ভেতরে প্রয়োজন হলে basic editor `nano` install করুন: `apt update && apt install nano -y`
- CPU-এর তথ্য দেখুন: `cat /proc/cpuinfo`

## Linux Distribution-এর Ecosystem অনুযায়ী শ্রেণিবিন্যাস

| Distribution | Ecosystem | ধরন | ব্যবহার |
| ------ | --------- | ---- | ---- |
| CentOS Linux | RHEL | Stable | এখন এড়িয়ে চলা ভালো |
| CentOS Stream | RHEL | Rolling preview | Testing |
| Rocky Linux | RHEL | Stable | Production |
| AlmaLinux | RHEL | Stable | Production |
| Kali Linux | Debian | Specialized | Security কাজ |
| Parrot OS | Debian | Specialized | Security/Development |
| openSUSE | Independent | Stable/Rolling | DevOps/Server |

## SSH দিয়ে Linux Machine-এ প্রবেশ | Linux Filesystem Hierarchy

SSH দিয়ে login করার পর সাধারণত authenticated user-এর home directory-তে থাকবেন। উদাহরণ:

```text
root@Ubuntu:~#
```

এখানে:

- `root` = username
- `Ubuntu` = hostname
- `~` = বর্তমান user-এর home directory
- `#` = root user হিসেবে login করা হয়েছে

এখন system-এর root directory-তে যেতে:

```bash
root@Ubuntu:~# cd /
```

Linux-এর root directory-র গুরুত্বপূর্ণ অংশগুলো:

```text
/bin        - Linux command চালানোর binary/program/executable ফাইল।
/boot       - Bootloader-এর প্রয়োজনীয় static file এবং kernel boot করার ফাইল।
/cdrom      - CD/DVD media mount করার জন্য ব্যবহৃত directory (যদি থাকে)।
/dev        - Device সম্পর্কিত special file; যেমন keyboard, mouse, disk ইত্যাদি।
/etc        - System-wide configuration file রাখার প্রধান directory।
/home       - সাধারণ user-দের home directory।
/lib        - System-এর প্রয়োজনীয় shared library।
/lib32      - 32-bit library (যদি system-এ থাকে)।
/lib64      - 64-bit library।
/lost+found - Filesystem recovery-এর সময় পাওয়া orphaned file রাখার জায়গা (সাধারণত ext filesystem-এ)।
/media      - Removable media, যেমন USB/SD card-এর mount point।
/mnt        - Temporary/manual mount করার জন্য ব্যবহৃত directory।
/opt        - Optional বা third-party software রাখার জন্য।
/proc       - Running process ও kernel-এর virtual information filesystem।
/root       - root user-এর home directory।
/run        - বর্তমানে চলমান system/process-এর runtime data।
/sbin       - System administration-এর binary/command।
/snap       - Snap package-এর data ও mount point।
/srv        - System যে service-related data পরিবেশন করে তার জন্য।
/sys        - Kernel ও hardware সম্পর্কিত virtual information filesystem।
/tmp        - Temporary file রাখার directory।
/usr        - User-space applications, libraries, documentation ইত্যাদি।
/var        - পরিবর্তনশীল data; যেমন log, cache, spool ইত্যাদি।
```

> মনে রাখুন: `/` হলো system root directory, আর `/root` হলো root user-এর home directory।

## Linux-এ Help নেওয়া ও Man Page

`man` হলো system reference manual পড়ার interface।

```bash
man man
man lsblk
man sshd
man mandb
whereis mandb
whatis mandb
man 5 mandb
cd --help
```

কিছু গুরুত্বপূর্ণ অর্থ:

- `man man` → `man` command-এর manual দেখায়।
- `man lsblk` → block device সম্পর্কিত manual।
- `man sshd` → OpenSSH server daemon-এর manual।
- `man mandb` → manual page index তৈরি/আপডেট করার তথ্য।
- `whereis mandb` → binary, source এবং manual-এর অবস্থান খুঁজে দেয়।
- `whatis mandb` → command-এর সংক্ষিপ্ত description দেখায়।
- `man 5 mandb` → নির্দিষ্ট manual section-এর entry দেখায়।
- `cd --help` → command-এর available option সম্পর্কে সাহায্য দেয়।

## Linux-এ Directory নিয়ে কাজ | Directory Management

Linux-এ প্রায় সবকিছুই file হিসেবে বিবেচিত হয়; directory-ও একটি বিশেষ ধরনের file structure।

### Root-এর অর্থ

- `/` = system root directory।
- `/root` = root user-এর home directory।
- `~` = বর্তমানে login করা user-এর home directory।

উদাহরণ:

```text
root@hostname:~#
```

এখানে `~` root user-এর home directory বোঝায়। আবার:

```text
sakil@hostname:~$
```

এখানে `~` Sakil user-এর home directory বোঝায়।

### গুরুত্বপূর্ণ Directory Command

```text
pwd pathname              = বর্তমান working directory দেখায়।
cd pathname               = relative path ব্যবহার করে directory পরিবর্তন করে।
cd /pathname              = absolute path ব্যবহার করে directory পরিবর্তন করে।
cd ~                      = বর্তমান user-এর home directory-তে যায়।
cd -                      = আগের directory-তে ফিরে যায়।
ls                       = বর্তমান directory-এর item দেখায়।
ls /var/log               = নির্দিষ্ট directory-এর item দেখায়।
ls -l                     = বিস্তারিত list দেখায়।
ls -la                    = hidden file-সহ বিস্তারিত list দেখায়।
ls -lah                   = hidden file-সহ human-readable size দেখায়।
ls -ldh                   = directory-এর নিজস্ব তথ্য human-readable format-এ দেখায়।
ls -li                    = inode number-সহ list দেখায়।
ll                       = সাধারণত `ls -l`-এর alias।
ll sakil/os/fedora sakil/os/debian = একাধিক directory-এর list দেখায়।
mkdir dirName             = একটি খালি directory তৈরি করে।
mkdir -p parent/child     = parent ও child directory তৈরি করে।
mkdir -p sakil/os/fedora sakil/os/debian = একাধিক nested directory তৈরি করে।
rmdir pathOfDir           = খালি directory মুছে দেয়।
rmdir dir1 dir2 dir3      = একাধিক খালি directory মুছে দেয়।
rm -rf parent/child       = directory ও তার contents forcefully মুছে দেয়।
```

> `rm -rf` অত্যন্ত সতর্কতার সঙ্গে ব্যবহার করুন। ভুল path দিলে গুরুত্বপূর্ণ data মুছে যেতে পারে।

### Dot (`.` এবং `..`)-এর অর্থ

```text
.  = বর্তমান directory-এর reference
.. = parent directory-এর reference
```

## File Management

### `touch` command

```bash
touch filename
```

একটি খালি file তৈরি করে।

```bash
touch {1..100}.txt
```

`1.txt` থেকে `100.txt` পর্যন্ত একাধিক file তৈরি করে।

```bash
touch filename
```

File-এর timestamp update করে।

```bash
touch -t 202601011030 filename
```

নির্দিষ্ট timestamp সেট করে।

### `cat` ও `tac`

```bash
cat fileName
```

File-এর content দেখায়।

```bash
cat > fileName
```

নতুন content লিখে file তৈরি/overwrite করা যায়। Existing content overwrite হয়ে যাবে। `Ctrl+C` দিয়ে prompt থেকে বের হওয়া যায়।

```bash
cat > file.txt << EOF
content
EOF
```

এখানে `EOF` একটি নির্ধারিত termination marker হিসেবে কাজ করে।

```bash
cat >> fileName
```

Existing content রেখে নতুন content append করে।

```bash
cat >> file.txt << EOF
new content
EOF
```

নির্দিষ্ট marker ব্যবহার করে content append করে।

```bash
tac fileName
```

File-এর content উল্টো ক্রমে দেখায়।

### `head` ও `tail`

```bash
head fileName
```

সাধারণত প্রথম ১০টি line দেখায়।

```bash
head -5 fileName
```

প্রথম ৫টি line দেখায়।

```bash
head -5 fileName | tac
```

প্রথম ৫টি line উল্টো ক্রমে দেখায়।

```bash
tail fileName
```

সাধারণত শেষ ১০টি line দেখায়।

```bash
tail -5 fileName
```

শেষ ৫টি line দেখায়।

```bash
tail -5 fileName | tac
```

শেষ ৫টি line উল্টো ক্রমে দেখায়।

### অন্যান্য দরকারি command

```bash
file filename
```

File-এর ধরন শনাক্ত করে।

```bash
mv sourceDirOrFile newDirOrFile
```

File বা directory rename অথবা move করে।

## File Permission / Access Mode

প্রতিটি file/directory-এর permission তিন ধরনের user-এর জন্য নির্ধারিত হয়:

- **Owner (`u`)** → file-এর owner।
- **Group (`g`)** → সংশ্লিষ্ট group-এর user।
- **Others (`o`)** → অন্য সবাই।

### Permission-এর ধরন

| Permission | File-এর ক্ষেত্রে | Directory-এর ক্ষেত্রে |
| ------ | --------- | ---- |
| `r` | File পড়া | File-এর তালিকা দেখা (`ls`) |
| `w` | File পরিবর্তন করা | File তৈরি/মুছে ফেলা |
| `x` | File execute করা | Directory-তে প্রবেশ করা (`cd`) |

### Permission Format

উদাহরণ:

```text
-rwxr-xr--
drwxr-xr-x
lrwxrwxrwx
```

এখানে মোট ১০টি character থাকে।

#### ১. প্রথম character — File Type

- `-` = regular file
- `d` = directory
- `l` = symbolic link

#### ২. পরের ৯টি character — Permission

```text
rwx r-x r--
│   │   │
│   │   └── Others
│   └────── Group
└────────── Owner
```

উদাহরণ:

- Owner → `rwx` → read, write, execute করতে পারে।
- Group → `r-x` → read ও execute করতে পারে।
- Others → `r--` → শুধু read করতে পারে।

আরও উদাহরণ:

- `drwxr-xr-x` → directory; owner full access, group/others read ও enter করতে পারে।
- `lrwxrwxrwx` → symbolic link।

### `chmod` — Symbolic Mode

| Symbol | অর্থ |
| --- | --- |
| `+` | permission যোগ করে |
| `-` | permission সরিয়ে দেয় |
| `=` | নির্দিষ্ট permission সেট করে |

উদাহরণ:

```bash
ls -lh devops.txt
-rw-r--r-- 1 root root 0 May  2 02:52 devops.txt

chmod o+wx devops.txt
ls -lh devops.txt
-rw-r--rwx 1 root root 0 May  2 02:52 devops.txt

chmod u+x devops.txt
ls -lh devops.txt
-rwxr--rwx 1 root root 0 May  2 02:52 devops.txt

chmod u-w devops.txt
ls -lh devops.txt
-r-xr--rwx 1 root root 0 May  2 02:52 devops.txt

chmod g=rw devops.txt
ls -l devops.txt
-r-xrw-rwx 1 root root 0 May  2 02:52 devops.txt
```

একাধিক permission একসাথে:

```bash
chmod o-wx,u+w,g=rx devops.txt
ls -lh devops.txt
-rwxr-xr-- 1 root root 0 May  2 02:52 devops.txt
```

### `chmod` — Numeric / Octal Mode

| Permission | Value | অর্থ |
| --- | ---: | --- |
| `---` | 0 | কোনো permission নেই |
| `--x` | 1 | Execute |
| `-w-` | 2 | Write |
| `-wx` | 3 | Write + Execute |
| `r--` | 4 | Read |
| `r-x` | 5 | Read + Execute |
| `rw-` | 6 | Read + Write |
| `rwx` | 7 | Read + Write + Execute |

উদাহরণ:

```text
rwx = 4 + 2 + 1 = 7
r-x = 4 + 0 + 1 = 5
r-- = 4 + 0 + 0 = 4
```

তাই:

```bash
chmod 754 file.txt
```

এর অর্থ:

```text
Owner  → 7 → rwx
Group  → 5 → r-x
Others → 4 → r--
```

আরও উদাহরণ:

```bash
chmod 777 devops.txt
chmod 755 devops.txt
chmod 744 devops.txt
chmod 700 devops.txt
```

### সহজভাবে মনে রাখুন

> **কে (`owner/group/others`) কোন file-এর ওপর কী করতে পারবে (`read/write/execute`) — সেটিই file permission।**

> 🏠 [সূচিপত্রে ফিরে যান](../) ➡️ পরবর্তী অধ্যায়: [০২। শেল এক্সপ্যানশন](../০২-শেল-এক্সপ্যানশন)

