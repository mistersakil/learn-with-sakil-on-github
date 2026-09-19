> [📖 ০১.২। ফাইল পারমিশন — বিস্তারিত](./০১.২-ফাইল-পারমিশন)  

>  [🏠](../) [⬅️ ০০। সংক্ষিপ্ত প্রশ্নোত্তর](../০০-সংক্ষিপ্ত-প্রশ্নোত্তর) [➡️ ০১.১। ইউজার ম্যানেজমেন্ট](../০১.১-ইউজার-ম্যানেজমেন্ট)

# ০১। লিনাক্স-পরিচিতি ও ফাইল পারমিশন | Linux Overview & File Permissions

Linux শুধুমাত্র একটি Operating System নয়; এটি একটি বিশাল Open Source Ecosystem। অনেকেই Ubuntu, Fedora, Debian বা Kali Linux-কে Linux বলে থাকেন, কিন্তু প্রযুক্তিগতভাবে Linux-এর মূল অংশ হলো **Linux Kernel**।

সহজভাবে বুঝতে:

- Linux Kernel = Operating System-এর Engine
- Shell / CLI = User-এর Control Panel
- Applications = User যে Software ব্যবহার করে

একটি Linux System সাধারণত নিচের স্তরগুলো নিয়ে গঠিত:

```text
+-------------------------------------+
|        Applications (Apps)          |
+-------------------------------------+
|    CLI / GUI (User Interface)       |
+-------------------------------------+
|          LINUX KERNEL               |
+-------------------------------------+
|        Physical Hardware            |
+-------------------------------------+
```

যখন আপনি কোনো Application ব্যবহার করেন, Application সরাসরি Hardware-এর সাথে কথা বলে না। Application → Shell/GUI → Kernel → Hardware এই ধাপগুলো অনুসরণ করে কাজ সম্পন্ন হয়।

---

# Linux Kernel কী?

Kernel হলো যেকোনো Operating System-এর Core Component বা কেন্দ্রীয় অংশ।

কম্পিউটার চালু হওয়ার সময় Kernel প্রথম Memory-তে Load হয় এবং পুরো System পরিচালনার দায়িত্ব গ্রহণ করে।

Kernel-কে Operating System-এর "মস্তিষ্ক" বলা যায়।

বাস্তবে Kernel একটি মধ্যস্থতাকারী (Mediator) হিসেবে কাজ করে:

```text
Application
     │
     ▼
Linux Kernel
     │
     ▼
Hardware
```

Application Hardware-এর ভাষা বোঝে না, Hardware-ও Application-এর ভাষা বোঝে না। Kernel এই দুই পক্ষের মধ্যে অনুবাদকের মতো কাজ করে।

---

## Linux Kernel-এর প্রধান কাজ

### CPU Management

একই সময়ে Browser, VS Code, Docker, Terminal, Database ইত্যাদি অনেক Process চলতে পারে।

Kernel নির্ধারণ করে:

* কোন Process আগে চলবে
* কতক্ষণ CPU ব্যবহার করবে
* কোন Process অপেক্ষা করবে

একে Process Scheduling বলা হয়।

---

### Memory Management

RAM একটি সীমিত Resource।

Kernel নির্ধারণ করে:

* কোন Process কত RAM পাবে
* কোন Process Memory Release করবে
* Memory Shortage হলে কী হবে

উদাহরণ:

```bash
free -h 
Display amount of free and used memory in the system.
allocate and free dynamic memory
```

---

### Process Management

Linux-এ চলমান প্রতিটি Program একটি Process।

Kernel:

* Process তৈরি করে
* Process বন্ধ করে
* Process Monitoring করে

উদাহরণ:

```bash
ps aux - report a snapshot of the current processes.
```

```bash
top - display Linux processes
```

#### 📊 top কমান্ড (Table of Processes)
top কমান্ডটি লিনেক্সের একটি রিয়েল-টাইম (Real-time) সিস্টেম মনিটর। এটি উইন্ডোজের Task Manager-এর মতো কাজ করে। এটি প্রতিনিয়ত (প্রতি ৩ সেকেন্ড পর পর) আপডেট হতে থাকে এবং সিস্টেমে কতটুকু প্রসেসর ও র‍্যাম ব্যবহার হচ্ছে তা লাইভ দেখায়।
টার্মিনালে শুধু top লিখে এন্টার দিলে নিচের মতো একটি আউটপুট আসবে:

##### 🔍 top কমান্ডের প্রধান কলামগুলোর অর্থ:

* PID (Process ID): প্রতিটি রানিং প্রোগ্রামের একটি ইউনিক নম্বর বা আইডি। কোনো প্রোগ্রাম বন্ধ করতে এই আইডিটি লাগে।
* USER: কোন ইউজার বা অ্যাকাউন্ট থেকে এই প্রোগ্রামটি চালানো হচ্ছে।
* %CPU: প্রোগ্রামটি প্রসেসরের (CPU) কত শতাংশ ব্যবহার করছে।
* %MEM: প্রোগ্রামটি আপনার র‍্যামের (RAM) কত শতাংশ জায়গা নিয়েছে।
* TIME+: প্রোগ্রামটি চালু হওয়ার পর থেকে মোট কতক্ষণ সিপিইউ টাইম ব্যবহার করেছে।
* COMMAND: রানিং প্রোগ্রাম বা অ্যাপ্লিকেশনটির নাম। [1, 2, 3, 4, 5] 

##### 💡 কিছু প্রয়োজনীয় শর্টকাট (কমান্ডটি চলাকালীন কিবোর্ডে চাপুন):

* M — র‍্যাম (Memory) ব্যবহারের ওপর ভিত্তি করে তালিকা সাজাবে (Highest to Lowest)।
* P — সিপিইউ (CPU) ব্যবহারের ওপর ভিত্তি করে তালিকা সাজাবে।
* k — কোনো নির্দিষ্ট প্রসেস বন্ধ করতে চাইলে (Kill), এটি চেপে PID নম্বরটি দিতে হবে।
* q — top স্ক্রিন থেকে বের হয়ে সাধারণ টার্মিনালে ফিরে আসার জন্য।

### 📸 ps aux কমান্ড (Process Status)
ps মানে হলো Process Status। top কমান্ডের মতো এটি লাইভ আপডেট হয় না, বরং এটি কমান্ড দেওয়ার ঠিক ওই মুহূর্তের (Snapshot) সিস্টেমে চলমান সকল প্রসেসের একটি বিশাল তালিকা একবারে প্রিন্ট করে দেয়।
টার্মিনালে কমান্ডটি এভাবে লিখতে হয়:

#### 🔍 aux ফ্ল্যাগ বা অপশনগুলোর অর্থ:

* a: সিস্টেমে যত ইউজার আছে, সবার প্রসেস একসাথে দেখাবে।
* u: প্রসেসগুলোর বিস্তারিত তথ্য (যেমন- CPU, Memory ব্যবহার এবং ইউজারের নাম) সহজ ভাষায় দেখাবে।
* x: টার্মিনাল ছাড়া ব্যাকগ্রাউন্ডে স্বয়ংক্রিয়ভাবে যে প্রসেসগুলো চলছে (Daemon/Services), সেগুলোও তালিকায় যুক্ত করবে।

#### 💡 বাস্তব জীবনের ব্যবহার (Real-life Use Case):
ps aux দিয়ে সাধারণত হাজার হাজার লাইনের তালিকা আসে। তাই কোনো নির্দিষ্ট প্রোগ্রাম খুঁজে বের করতে এর সাথে grep কমান্ড ব্যবহার করা হয়। [6] 
যেমন, আপনার সিস্টেমে chrome ব্রাউজার চলছে কিনা এবং তার PID কত, তা দেখতে নিচের কমান্ডটি ব্যবহার করা হয়:

`ps aux | grep chrome`

### ⚖️ সংক্ষেপে top বনাম ps aux

| বৈশিষ্ট্য | top Command | ps aux Command |
|---|---|---|
| কাজের ধরন | লাইভ বা রিয়েল-টাইম মনিটরিং। | একটি নির্দিষ্ট মুহূর্তের স্ন্যাপশট। |
| আপডেট | স্বয়ংক্রিয়ভাবে প্রতি কয়েক সেকেন্ড পর পর রিফ্রেশ হয়। | একবারই আউটপুট দেখায়, নিজে থেকে রিফ্রেশ হয় না। |
| মূল ব্যবহার | সিস্টেম স্লো হয়ে গেলে কোন অ্যাপ বেশি লোড নিচ্ছে তা তাৎক্ষণিক দেখতে। | কোনো নির্দিষ্ট প্রসেস ব্যাকগ্রাউন্ডে চলছে কিনা তা সার্চ করে বের করতে। |

---

### Filesystem Management

Hard Disk বা SSD-তে Data কোথায় সংরক্ষিত হবে তা Kernel নিয়ন্ত্রণ করে।

Linux-এর জনপ্রিয় Filesystem:

* ext4
* xfs
* btrfs
* zfs

Kernel File Read এবং Write Operation পরিচালনা করে।

---

### Hardware Management

Kernel Keyboard, Mouse, SSD, GPU, Wi-Fi Card, Printer ইত্যাদির সাথে যোগাযোগ করে।

এ কাজ Driver-এর মাধ্যমে সম্পন্ন হয়।

---

### Security Management

Linux Permission System, User Access Control এবং Process Isolation Kernel-এর মাধ্যমেই পরিচালিত হয়।

উদাহরণ:

```bash
chmod 755 file.txt
```

```bash
chown user:user file.txt
```

---

# Linux Distribution কী?

Kernel একা একটি পূর্ণাঙ্গ Operating System নয়।

Kernel-এর সাথে Shell, Package Manager, Desktop Environment, Libraries এবং বিভিন্ন Software যুক্ত করে Linux Distribution তৈরি করা হয়।

উদাহরণ:

| Distribution | Package Manager |
| ------------ | --------------- |
| Ubuntu       | apt             |
| Debian       | apt             |
| Fedora       | dnf             |
| Rocky Linux  | dnf             |
| AlmaLinux    | dnf             |
| Arch Linux   | pacman          |
| openSUSE     | zypper          |

---

## সব Linux Distribution কি একই Kernel ব্যবহার করে?

মূল Source Code একই হলেও Distribution ভেদে Kernel Version এবং Configuration আলাদা হতে পারে।

উদাহরণ:

* Ubuntu সাধারণত Stable এবং নতুন Hardware Support-এর মধ্যে ভারসাম্য রাখে।
* Debian দীর্ঘমেয়াদী Stability-এর দিকে বেশি গুরুত্ব দেয়।
* Fedora নতুন Feature দ্রুত গ্রহণ করে।
* Arch Linux Rolling Release Model অনুসরণ করে।

Android-ও একটি Modified Linux Kernel ব্যবহার করে।

---

# CLI কী? | Command Line Interface

CLI (Command Line Interface) হলো Text-Based User Interface।

GUI-তে আমরা Mouse ব্যবহার করি।

CLI-তে আমরা Command ব্যবহার করি।

উদাহরণ:

```bash
mkdir project
cd project
ls -lah
```

CLI-এর প্রধান সুবিধা:

* দ্রুত কাজ করা যায়
* Automation করা যায়
* Remote Server পরিচালনা করা যায়
* কম Resource ব্যবহার করে

---

# Shell কী?

Shell হলো User এবং Linux Kernel-এর মধ্যবর্তী Interpreter Program।

আপনি Terminal-এ Command লিখলে Shell সেটিকে Process করে Kernel-এর কাছে পাঠায়।

Workflow:

```text
User
 │
 ▼
Shell
 │
 ▼
Kernel
 │
 ▼
Hardware
```

---

## জনপ্রিয় Linux Shell

| Shell | Description                     |
| ----- | ------------------------------- |
| Bash  | Linux-এর সবচেয়ে জনপ্রিয় Shell |
| Zsh   | Modern এবং Highly Customizable  |
| Fish  | Beginner Friendly               |
| Dash  | Lightweight                     |
| Ksh   | Korn Shell                      |

বর্তমান Shell দেখুন:

```bash
echo $SHELL
```

---

# CLI কীভাবে কাজ করে?

ধরুন আপনি লিখলেন:

```bash
ls -lah
```

তখন:

```text
User
 │
 ▼
Shell
 │
 ▼
Kernel
 │
 ▼
Filesystem
 │
 ▼
Kernel
 │
 ▼
Shell
 │
 ▼
User
```

অর্থাৎ Shell Command গ্রহণ করে, Kernel-এর মাধ্যমে Filesystem-এ Request পাঠায় এবং Result User-এর সামনে প্রদর্শন করে।

---

# CLI বনাম GUI

| CLI                                 | GUI                          |
| ----------------------------------- | ---------------------------- |
| Keyboard ভিত্তিক                    | Mouse ভিত্তিক                |
| দ্রুত                               | সহজ                          |
| Automation Friendly                 | Beginner Friendly            |
| কম Resource ব্যবহার করে             | বেশি Resource ব্যবহার করে    |
| Server Administration-এর জন্য আদর্শ | Desktop ব্যবহারের জন্য আদর্শ |

---

# Linux Kernel এবং CLI-এর সম্পর্ক

Linux শেখার সময় একটি বিষয় মনে রাখা গুরুত্বপূর্ণ:

```text
User
 │
 ▼
CLI / Shell
 │
 ▼
Linux Kernel
 │
 ▼
Hardware
```

আপনি সরাসরি Kernel-এর সাথে কাজ করেন না।

আপনি Shell বা CLI ব্যবহার করেন।

Shell Kernel-এর সাথে যোগাযোগ করে।

Kernel Hardware-এর সাথে যোগাযোগ করে।

এই Architecture বুঝে গেলে Linux-এর Filesystem, Permission, Package Management, Process Management এবং DevOps সম্পর্কিত পরবর্তী অধ্যায়গুলো বোঝা অনেক সহজ হয়ে যায়।

---

## এখন আমরা Linux-এর প্রাথমিক CLI Command শেখা শুরু করবো

- লিনাক্সের OS-এর নাম ও ভার্সন দেখুন: `hostnamectl`
- ডিরেক্টরির তালিকা দেখুন: `ls -l`
- সিস্টেমের root directory-তে যান: `cd /` 
- User home directory `cd ~`
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

---

Linux Administration এবং DevOps-এ **File Permission** অত্যন্ত গুরুত্বপূর্ণ। কোন user কোন file বা directory কীভাবে ব্যবহার করতে পারবে—তা permission এবং access-control system দ্বারা নির্ধারিত হয়।

---

## সূচিপত্র

- [১। Permission-এর মূল ধারণা](#১-permission-এর-মূল-ধারণা)
- [২। File Type ও Permission Format](#২-file-type-ও-permission-format)
- [৩। File বনাম Directory Permission](#৩-file-বনাম-directory-permission)
- [৪। Directory-এর x Permission](#৪-directory-এর-x-permission)
- [৫। ls -l Output](#৫-ls--l-output)
- [৬। chmod Symbolic Mode](#৬-chmod-symbolic-mode)
- [৭। chmod Numeric / Octal Mode](#৭-chmod-numeric--octal-mode)
- [৮। chown ও chgrp](#৮-chown-ও-chgrp)
- [৯। umask](#৯-umask)
- [১০। Special Permissions](#১০-special-permissions)
- [১১। ACL](#১১-acl)
- [১২। Permission Troubleshooting](#১২-permission-troubleshooting)
- [১৩। sudo, Root ও Least Privilege](#১৩-sudo-root-ও-least-privilege)
- [১৪। Laravel, Nginx/PHP-FPM ও Docker](#১৪-laravel-nginxphp-fpm-ও-docker)
- [১৫। SELinux/AppArmor](#১৫-selinuxapparmor)
- [১৬। Permission Audit Commands](#১৬-permission-audit-commands)
- [১৭। Best Practices](#১৭-best-practices)

---

# ১। Permission-এর মূল ধারণা

প্রতিটি Linux file/directory-এর permission তিন ধরনের user-এর জন্য নির্ধারিত হয়:

| User | Symbol | অর্থ |
| --- | --- | --- |
| Owner | `u` | File-এর owner |
| Group | `g` | File-এর group-এর সদস্য |
| Others | `o` | অন্য সবাই |

Permission:

| Permission | File | Directory |
| --- | --- | --- |
| `r` | File পড়া | Directory-এর entry/name list করা |
| `w` | File-এর content পরিবর্তন | File create/delete/rename করা |
| `x` | Program execute করা | Directory traverse/enter করা |

---

# ২। File Type ও Permission Format

উদাহরণ:

~~~text
-rwxr-xr--
drwxr-xr-x
lrwxrwxrwx
~~~

প্রথম character file type:

| Character | অর্থ |
| --- | --- |
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |
| `s` | Socket |
| `p` | Named pipe |

পরের ৯টি character:

~~~text
rwx r-x r--
│   │   │
│   │   └── Others
│   └────── Group
└────────── Owner
~~~

উদাহরণ:

~~~text
-rwxr-xr--
~~~

Owner → rwx  
Group → r-x  
Others → r--

---

# ৩। File বনাম Directory Permission

File-এর ক্ষেত্রে:

~~~text
r = content পড়া
w = content পরিবর্তন
x = executable হিসেবে চালানো
~~~

Directory-এর ক্ষেত্রে:

~~~text
r = directory-এর নাম list করা
w = entry create/delete/rename করা
x = directory traverse/enter করা
~~~

একই `rwx` permission file এবং directory-তে একই অর্থ বহন করে না।

---

# ৪। Directory-এর x Permission

Directory-এর `x` permission-এর অর্থ execute নয়; এখানে মূল অর্থ **traverse**।

ধরো:

~~~text
project/
└── secret/
    └── data.txt
~~~

`secret/`-এ `x` permission না থাকলে directory-তে প্রবেশ বা path traverse করা যাবে না।

Directory-তে সাধারণত:

~~~text
r + x → list + traverse
w + x → entry create/delete/rename
~~~

Parent directory-গুলোর `x` permission-ও file access-এর জন্য গুরুত্বপূর্ণ।

Path troubleshooting:

~~~bash
namei -l /home/sakil/project/app/file.txt
~~~

---

# ৫। ls -l Output

~~~bash
ls -l
~~~

উদাহরণ:

~~~text
-rwxr-xr-- 1 root root 1200 May 2 02:52 devops.txt
~~~

এখানে:

~~~text
-rwxr-xr-- → File type + permission
1           → Hard link count
root        → Owner
root        → Group
1200        → Size
May 2 02:52 → Modification time
devops.txt  → File name
~~~

দরকারি command:

~~~bash
ls -la
ls -lah
ls -ldh directory/
~~~

---

# ৬। chmod Symbolic Mode

`chmod` permission পরিবর্তনের জন্য ব্যবহৃত হয়।

| Symbol | অর্থ |
| --- | --- |
| `u` | Owner |
| `g` | Group |
| `o` | Others |
| `a` | All |
| `+` | Permission যোগ |
| `-` | Permission সরানো |
| `=` | Permission নির্দিষ্টভাবে সেট |

উদাহরণ:

~~~bash
chmod o+wx devops.txt
chmod u+x devops.txt
chmod u-w devops.txt
chmod g=rw devops.txt
~~~

একাধিক operation:

~~~bash
chmod o-wx,u+w,g=rx devops.txt
~~~

সম্পূর্ণ permission সেট:

~~~bash
chmod u=rw,g=r,o= file.txt
~~~

---

# ৭। chmod Numeric / Octal Mode

Permission-এর value:

| Permission | Value |
| --- | ---: |
| `---` | 0 |
| `--x` | 1 |
| `-w-` | 2 |
| `-wx` | 3 |
| `r--` | 4 |
| `r-x` | 5 |
| `rw-` | 6 |
| `rwx` | 7 |

কারণ:

~~~text
r = 4
w = 2
x = 1
~~~

তাই:

~~~text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2 + 0 = 6
r-x = 4 + 0 + 1 = 5
r-- = 4 + 0 + 0 = 4
~~~

উদাহরণ:

~~~bash
chmod 754 file.txt
~~~

এর অর্থ:

~~~text
Owner  → 7 → rwx
Group  → 5 → r-x
Others → 4 → r--
~~~

Common modes:

~~~bash
chmod 755 file
chmod 644 file
chmod 700 file
chmod 750 directory
chmod 770 directory
~~~

### 755

~~~text
Owner  → rwx
Group  → r-x
Others → r-x
~~~

### 644

~~~text
Owner  → rw-
Group  → r--
Others → r--
~~~

### 700

~~~text
Owner  → rwx
Group  → ---
Others → ---
~~~

## Recursive Permission

~~~bash
chmod -R ...
~~~

`-R` মানে recursive। তবে blindভাবে `chmod -R 755 project/` করলে regular file-ও executable হয়ে যেতে পারে।

কিছু পরিস্থিতিতে:

~~~bash
chmod -R a+X project/
~~~

ব্যবহার করা যেতে পারে। এখানে uppercase `X` directory এবং প্রয়োজনীয় executable file-এর ক্ষেত্রে execute permission নিয়ে কাজ করে।

---

# ৮। chown ও chgrp

File-এর owner/group দেখতে:

~~~bash
ls -l file.txt
~~~

Owner পরিবর্তন:

~~~bash
sudo chown john file.txt
~~~

Owner + Group:

~~~bash
sudo chown john:developers file.txt
~~~

শুধু Group:

~~~bash
sudo chown :developers file.txt
~~~

Recursive:

~~~bash
sudo chown -R john:developers project/
~~~

> `-R` খুব সাবধানে ব্যবহার করতে হবে।

Group পরিবর্তন:

~~~bash
sudo chgrp developers file.txt
sudo chgrp -R developers project/
~~~

---

# ৯। umask

নতুন file/directory তৈরি হওয়ার সময় default permission-এ `umask` গুরুত্বপূর্ণ।

দেখুন:

~~~bash
umask
~~~

সাধারণ example:

~~~text
0022
~~~

সহজ conceptual model:

~~~text
Directory base = 777
File base      = 666

777 - 022 → 755
666 - 022 → 644
~~~

তাই সাধারণভাবে:

~~~text
Directory → 755
File      → 644
~~~

Temporary পরিবর্তন:

~~~bash
umask 027
~~~

> Permission calculation-কে সহজভাবে বোঝাতে subtraction model ব্যবহার করা হয়; Linux বাস্তবে permission bitmask ব্যবহার করে।

---

# ১০। Special Permissions

Linux-এর তিনটি গুরুত্বপূর্ণ special permission:

~~~text
SUID
SGID
Sticky Bit
~~~

## ১০.১ SUID

Executable file-এ SUID থাকলে execution-এর সময় file owner-এর effective identity ব্যবহার করা যেতে পারে।

উদাহরণ দেখতে:

~~~bash
ls -l /usr/bin/passwd
~~~

SUID থাকলে owner execute position-এ `s` দেখা যেতে পারে:

~~~text
-rwsr-xr-x
~~~

Numeric value:

~~~text
SUID = 4
~~~

উদাহরণ:

~~~bash
chmod 4755 file
~~~

SUID security-sensitive।

## ১০.২ SGID

Directory-তে SGID team/shared directory-র জন্য খুব useful।

~~~bash
chmod g+s /project
~~~

অথবা:

~~~bash
chmod 2775 /project
~~~

Numeric value:

~~~text
SGID = 2
~~~

SGID directory-তে নতুন child file/directory সাধারণত parent-এর group inheritance পায়।

## ১০.৩ Sticky Bit

Shared writable directory-তে Sticky Bit ব্যবহার করা হয়।

উদাহরণ:

~~~bash
ls -ld /tmp
~~~

সাধারণত:

~~~text
drwxrwxrwt
~~~

শেষের `t` হলো Sticky Bit।

Set:

~~~bash
chmod +t shared/
~~~

Remove:

~~~bash
chmod -t shared/
~~~

Numeric value:

~~~text
Sticky Bit = 1
~~~

উদাহরণ:

~~~bash
chmod 1777 shared/
~~~

Special permission values:

| Permission | Value |
| --- | ---: |
| SUID | 4 |
| SGID | 2 |
| Sticky Bit | 1 |

---

# ১১। ACL

Traditional permission model:

~~~text
Owner
Group
Others
~~~

কিন্তু নির্দিষ্ট একাধিক user-কে আলাদা permission দিতে হলে ACL ব্যবহার করা যায়।

উদাহরণ:

~~~text
rahim      → read/write
karim      → read-only
developers → read/write
others     → no access
~~~

ACL দেখা:

~~~bash
getfacl file.txt
~~~

নির্দিষ্ট user-কে permission:

~~~bash
setfacl -m u:rahim:rw file.txt
setfacl -m u:karim:r file.txt
~~~

ACL entry remove:

~~~bash
setfacl -x u:rahim file.txt
~~~

সব ACL remove:

~~~bash
setfacl -b file.txt
~~~

## ACL Mask

ACL-এর `mask` effective permission সীমাবদ্ধ করতে পারে। তাই শুধু user entry দেখে সিদ্ধান্ত নেওয়া উচিত নয়।

## Default ACL

Directory-র future child objects-এর জন্য:

~~~bash
setfacl -d -m g:developers:rwx project/
~~~

Team-based shared directory-তে এটি useful।

---

# ১২। Permission Troubleshooting

Common error:

~~~text
Permission denied
~~~

সরাসরি `chmod 777` না দিয়ে:

### ১। Current user

~~~bash
whoami
~~~

### ২। UID/GID ও groups

~~~bash
id
groups
~~~

### ৩। File permission ও ownership

~~~bash
ls -l file.txt
~~~

### ৪। Parent path

~~~bash
namei -l /path/to/file.txt
~~~

### ৫। ACL

~~~bash
getfacl file.txt
~~~

### ৬। Detailed metadata

~~~bash
stat file.txt
~~~

### ৭। Process user

~~~bash
ps aux
~~~

Server application-এর ক্ষেত্রে process/service কোন user হিসেবে চলছে সেটি গুরুত্বপূর্ণ।

---

# ১৩। sudo, Root ও Least Privilege

`sudo` elevated privilege-এ command চালাতে ব্যবহৃত হয়।

~~~bash
sudo chown root:root file.txt
~~~

অপ্রয়োজনে সব command-এ `sudo` ব্যবহার করা উচিত নয়। বিশেষ করে:

~~~bash
sudo npm install
sudo composer install
sudo git ...
~~~

এগুলো ownership সমস্যা তৈরি করতে পারে।

## Root

`root` user-এর খুব উচ্চ privilege আছে। Production-এ application/service প্রয়োজন অনুযায়ী dedicated non-root user হিসেবে চালানো নিরাপদ পদ্ধতি।

## Principle of Least Privilege

> যে user/process-এর যতটুকু access দরকার, শুধু ততটুকুই দেওয়া উচিত।

---

# ১৪। Laravel, Nginx/PHP-FPM ও Docker

## Laravel

Application code এবং runtime-writable directory আলাদা করে ভাবুন।

সাধারণত Laravel application process-এর writable access প্রয়োজন হতে পারে:

~~~text
storage/
bootstrap/cache/
~~~

সব project-এ:

~~~bash
chmod -R 777 .
~~~

দেওয়া উচিত নয়।

মূল ধারণা:

~~~text
Application code
    ↓
Mostly read-only

Runtime directories
    ↓
Application process-এর জন্য writable
~~~

## Nginx + PHP-FPM

সাধারণ architecture:

~~~text
Browser
   ↓
Nginx
   ↓
PHP-FPM
   ↓
Laravel
~~~

Permission troubleshoot করার সময়:

~~~text
File Owner
Group
Process User
~~~

এই তিনটি আলাদা করে দেখতে হবে।

## Docker

Docker-এ host এবং container-এর UID/GID mismatch হলে bind mount বা volume-এ permission সমস্যা হতে পারে।

~~~text
Host UID/GID
      +
Container UID/GID
      +
Bind Mount / Volume
      ↓
File Access
~~~

বিশেষ করে Laravel `storage/`, bind mount, Docker volume এবং `node_modules`-এর ক্ষেত্রে UID/GID গুরুত্বপূর্ণ।

---

# ১৫। SELinux/AppArmor

Traditional Linux permission:

~~~text
Owner
Group
Others
~~~

এর পাশাপাশি system-এ additional security policy থাকতে পারে:

~~~text
SELinux
AppArmor
~~~

তাই কখনও:

~~~text
chmod ঠিক
chown ঠিক
~~~

হওয়ার পরও:

~~~text
Permission denied
~~~

হতে পারে।

কারণ access decision-এ additional security policy প্রভাব ফেলতে পারে।

---

# ১৬। Permission Audit Commands

## stat

~~~bash
stat file.txt
~~~

Detailed metadata এবং permission দেখতে।

## namei

~~~bash
namei -l /var/www/project/storage/app.log
~~~

পুরো path-এর প্রতিটি component-এর permission দেখতে।

## find

Writable file খুঁজতে:

~~~bash
find /var/www -type f -perm -002
~~~

নির্দিষ্ট 777 permission খুঁজতে:

~~~bash
find /var/www -type f -perm 777
~~~

SUID:

~~~bash
find / -perm -4000 -type f 2>/dev/null
~~~

SGID:

~~~bash
find / -perm -2000 -type f 2>/dev/null
~~~

> System-wide `find /` অনেক result দিতে পারে এবং permission-related error দেখা স্বাভাবিক।

---

# ১৭। Best Practices

### ১। অপ্রয়োজনে 777 ব্যবহার করবেন না

~~~bash
chmod 777 file
~~~

এতে Owner, Group এবং Others সবাইকে full permission দেওয়া হয়।

### ২। Ownership আগে বুঝুন

~~~bash
ls -l file
~~~

তারপর প্রয়োজন অনুযায়ী `chown` / `chgrp` ব্যবহার করুন।

### ৩। File এবং Directory আলাদা করে ভাবুন

অনেক ক্ষেত্রে:

~~~text
Directory → 755
Regular file → 644
~~~

ব্যবহার করা হয়, তবে application requirement অনুযায়ী permission পরিবর্তন হতে পারে।

### ৪। Recursive command সাবধানে ব্যবহার করুন

~~~bash
chmod -R
chown -R
chgrp -R
~~~

ভুল path-এ চালানো বিপজ্জনক।

### ৫। ACL থাকলে ACL check করুন

~~~bash
getfacl file.txt
~~~

### ৬। Process identity বুঝুন

~~~text
File owner ≠ Application process user
~~~

হতে পারে।

---

# 🧠 Permission Troubleshooting Mental Model

কোনো file access করতে না পারলে:

~~~text
১. কে access করছে?
        ↓
২. Current user কে?
        ↓
৩. User কোন group-এর member?
        ↓
৪. File-এর owner কে?
        ↓
৫. File-এর group কী?
        ↓
৬. File-এর rwx কী?
        ↓
৭. Parent directory-গুলোতে x আছে?
        ↓
৮. ACL আছে?
        ↓
৯. Process কোন user হিসেবে চলছে?
        ↓
১০. SELinux/AppArmor policy আছে?
        ↓
Final Access Decision
~~~

## Quick Revision

| বিষয় | Command / Concept |
| --- | --- |
| Permission দেখা | `ls -l` |
| Detailed metadata | `stat` |
| Path permission | `namei -l` |
| Permission পরিবর্তন | `chmod` |
| Owner পরিবর্তন | `chown` |
| Group পরিবর্তন | `chgrp` |
| Default permission | `umask` |
| SUID | `chmod 4xxx` |
| SGID | `chmod 2xxx` |
| Sticky Bit | `chmod 1xxx` |
| ACL দেখা | `getfacl` |
| ACL পরিবর্তন | `setfacl` |
| Current user | `whoami` |
| UID/GID/Groups | `id` |
| User groups | `groups` |
| Permission search | `find` |

---

> **পরবর্তী ধাপ:** Linux User/Group Management-এর সঙ্গে File Permission combine করে user + group + ownership + chmod + ACL নিয়ে practical lab করা সবচেয়ে উপকারী হবে।


---

> [🏠](../) [⬅️ ০০। সংক্ষিপ্ত প্রশ্নোত্তর](../০০-সংক্ষিপ্ত-প্রশ্নোত্তর) [➡️ ০১.১। ইউজার ম্যানেজমেন্ট](../০১.১-ইউজার-ম্যানেজমেন্ট)
