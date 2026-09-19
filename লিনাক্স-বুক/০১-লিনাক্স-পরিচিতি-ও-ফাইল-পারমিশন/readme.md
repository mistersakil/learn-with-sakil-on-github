# ০১। লিনাক্স-পরিচিতি ও ফাইল পারমিশন | Linux Overview & File Permissions

> [🏠](../) [⬅️ ০০। সংক্ষিপ্ত প্রশ্নোত্তর](../০০-সংক্ষিপ্ত-প্রশ্নোত্তর) [➡️ ০১.১। ইউজার ম্যানেজমেন্ট](../০১.১-ইউজার-ম্যানেজমেন্ট)

---

## 📑 সূচিপত্র

- [ভাগ ১: লিনাক্স পরিচিতি](#ভাগ-১-লিনাক্স-পরিচিতি)
  - [১.১। লিনাক্স কী?](#১১-লিনাক্স-কী)
  - [১.২। লিনাক্স সিস্টেমের স্তরগুলো](#১২-লিনাক্স-সিস্টেমের-স্তরগুলো)
  - [১.৩। Linux Kernel কী?](#১৩-linux-kernel-কী)
  - [১.৪। Kernel-এর প্রধান কাজ](#১৪-kernel-এর-প্রধান-কাজ)
    - [CPU Management](#cpu-management)
    - [Memory Management](#memory-management)
    - [Process Management](#process-management)
    - [Filesystem Management](#filesystem-management)
    - [Hardware Management](#hardware-management)
    - [Security Management](#security-management)
  - [১.৫। Linux Distribution কী?](#১৫-linux-distribution-কী)
  - [১.৬। সব Distribution কি একই Kernel ব্যবহার করে?](#১৬-সব-distribution-কি-একই-kernel-ব্যবহার-করে)
- [ভাগ ২: CLI, Shell ও Terminal](#ভাগ-২-cli-shell-ও-terminal)
  - [২.১। CLI কী?](#২১-cli-কী)
  - [২.২। Shell কী?](#২২-shell-কী)
  - [২.৩। জনপ্রিয় Linux Shell](#২৩-জনপ্রিয়-linux-shell)
  - [২.৪। CLI কীভাবে কাজ করে?](#২৪-cli-কীভাবে-কাজ-করে)
  - [২.৫। CLI বনাম GUI](#২৫-cli-বনাম-gui)
  - [২.৬। Kernel এবং CLI-এর সম্পর্ক](#২৬-kernel-এবং-cli-এর-সম্পর্ক)
- [ভাগ ৩: Linux Filesystem Hierarchy](#ভাগ-৩-linux-filesystem-hierarchy)
  - [৩.১। SSH দিয়ে Linux Machine-এ প্রবেশ](#৩১-ssh-দিয়ে-linux-machine-এ-প্রবেশ)
  - [৩.২। Root Directory-র গুরুত্বপূর্ণ অংশ](#৩২-root-directory-র-গুরুত্বপূর্ণ-অংশ)
- [ভাগ ৪: Help নেওয়া ও Man Page](#ভাগ-৪-help-নেওয়া-ও-man-page)
  - [৪.১। man Command](#৪১-man-command)
  - [৪.২। অন্যান্য Help Command](#৪২-অন্যান্য-help-command)
- [ভাগ ৫: Directory Management](#ভাগ-৫-directory-management)
  - [৫.১। Root-এর অর্থ](#৫১-root-এর-অর্থ)
  - [৫.২। গুরুত্বপূর্ণ Directory Command](#৫২-গুরুত্বপূর্ণ-directory-command)
  - [৫.৩। Dot (`.` এবং `..`)-এর অর্থ](#৫৩-dot-এবং--এর-অর্থ)
- [ভাগ ৬: File Management](#ভাগ-৬-file-management)
  - [৬.১। touch Command](#৬১-touch-command)
  - [৬.২। cat ও tac](#৬২-cat-ও-tac)
  - [৬.৩। head ও tail](#৬৩-head-ও-tail)
  - [৬.৪। অন্যান্য দরকারি Command](#৬৪-অন্যান্য-দরকারি-command)
- [ভাগ ৭: File Permission](#ভাগ-৭-file-permission)
  - [৭.১। Permission-এর মূল ধারণা](#৭১-permission-এর-মূল-ধারণা)
  - [৭.২। File Type ও Permission Format](#৭২-file-type-ও-permission-format)
  - [৭.৩। File বনাম Directory Permission](#৭৩-file-বনাম-directory-permission)
  - [৭.৪। Directory-এর x Permission](#৭৪-directory-এর-x-permission)
  - [৭.৫। ls -l Output](#৭৫-ls--l-output)
- [ভাগ ৮: chmod — Permission পরিবর্তন](#ভাগ-৮-chmod--permission-পরিবর্তন)
  - [৮.১। chmod Symbolic Mode](#৮১-chmod-symbolic-mode)
  - [৮.২। chmod Numeric / Octal Mode](#৮২-chmod-numeric--octal-mode)
  - [৮.৩। Common Modes](#৮৩-common-modes)
  - [৮.৪। Recursive Permission](#৮৪-recursive-permission)
- [ভাগ ৯: chown ও chgrp](#ভাগ-৯-chown-ও-chgrp)
- [ভাগ ১০: umask](#ভাগ-১০-umask)
- [ভাগ ১১: Special Permissions](#ভাগ-১১-special-permissions)
  - [১১.১। SUID](#১১১-suid)
  - [১১.২। SGID](#১১2-sgid)
  - [১১.৩। Sticky Bit](#১১3-sticky-bit)
- [ভাগ ১২: ACL](#ভাগ-১২-acl)
- [ভাগ ১৩: Permission Troubleshooting](#ভাগ-১৩-permission-troubleshooting)
- [ভাগ ১৪: sudo, Root ও Least Privilege](#ভাগ-১৪-sudo-root-ও-least-privilege)
- [ভাগ ১৫: Laravel, Nginx/PHP-FPM ও Docker](#ভাগ-১৫-laravel-nginxphp-fpm-ও-docker)
- [ভাগ ১৬: SELinux/AppArmor](#ভাগ-১৬-selinuxapparmor)
- [ভাগ ১৭: Permission Audit Commands](#ভাগ-১৭-permission-audit-commands)
- [ভাগ ১৮: Best Practices](#ভাগ-১৮-best-practices)
- [🧠 Permission Troubleshooting Mental Model](#-permission-troubleshooting-mental-model)
- [Quick Revision](#quick-revision)

---

# ভাগ ১: লিনাক্স পরিচিতি

## ১.১। লিনাক্স কী?

Linux শুধুমাত্র একটি Operating System নয়; এটি একটি বিশাল **Open Source Ecosystem**। অনেকেই Ubuntu, Fedora, Debian বা Kali Linux-কে Linux বলে থাকেন, কিন্তু প্রযুক্তিগতভাবে Linux-এর মূল অংশ হলো **Linux Kernel**।

সহজভাবে বুঝতে:

| উপাদান | ভূমিকা |
| --- | --- |
| **Linux Kernel** | Operating System-এর Engine |
| **Shell / CLI** | User-এর Control Panel |
| **Applications** | User যে Software ব্যবহার করে |

> 💡 **মনে রাখুন:** Linux Kernel হলো মূল ভিত্তি। এর উপর Shell, Package Manager, Desktop Environment, Libraries এবং Applications যোগ করে একটি পূর্ণাঙ্গ Linux Distribution তৈরি হয়।

---

## ১.২। লিনাক্স সিস্টেমের স্তরগুলো

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

**কীভাবে কাজ করে:**

যখন আপনি কোনো Application ব্যবহার করেন, Application সরাসরি Hardware-এর সাথে কথা বলে না। বরং:

```text
Application → Shell/GUI → Kernel → Hardware
```

এই ধাপগুলো অনুসরণ করে কাজ সম্পন্ন হয়।

**বাস্তব উদাহরণ:**

ধরুন আপনি `ls` command চালালেন। তখন:

1. Shell আপনার command গ্রহণ করে
2. Shell Kernel-এর কাছে request পাঠায়
3. Kernel Filesystem-এ গিয়ে file list পড়ে
4. Kernel Shell-কে result ফেরত দেয়
5. Shell result আপনার সামনে display করে

---

## ১.৩। Linux Kernel কী?

Kernel হলো যেকোনো Operating System-এর **Core Component** বা কেন্দ্রীয় অংশ।

কম্পিউটার চালু হওয়ার সময় Kernel প্রথম Memory-তে Load হয় এবং পুরো System পরিচালনার দায়িত্ব গ্রহণ করে।

Kernel-কে Operating System-এর **"মস্তিষ্ক"** বলা যায়।

বাস্তবে Kernel একটি **মধ্যস্থতাকারী (Mediator)** হিসেবে কাজ করে:

```text
Application
     │
     ▼
Linux Kernel
     │
     ▼
Hardware
```

**কেন Mediator দরকার?**

- Application Hardware-এর ভাষা বোঝে না
- Hardware-ও Application-এর ভাষা বোঝে না
- Kernel এই দুই পক্ষের মধ্যে **অনুবাদকের** মতো কাজ করে

---

## ১.৪। Kernel-এর প্রধান কাজ

### CPU Management

একই সময়ে Browser, VS Code, Docker, Terminal, Database ইত্যাদি অনেক Process চলতে পারে। Kernel নির্ধারণ করে:

- কোন Process আগে চলবে
- কতক্ষণ CPU ব্যবহার করবে
- কোন Process অপেক্ষা করবে

একে **Process Scheduling** বলা হয়।

**উদাহরণ:**

```bash
top
```

এই command দিয়ে দেখতে পারেন কোন process কত CPU ব্যবহার করছে।

---

### Memory Management

RAM একটি সীমিত Resource। Kernel নির্ধারণ করে:

- কোন Process কত RAM পাবে
- কোন Process Memory Release করবে
- Memory Shortage হলে কী হবে

**উদাহরণ:**

```bash
free -h
```

এই command system-এর free এবং used memory দেখায়।

**Memory Management-এর কাজ:**

- Dynamic memory allocate করা
- Free memory track করা
- Swap space ব্যবহার করা (RAM শেষ হয়ে গেলে)

---

### Process Management

Linux-এ চলমান প্রতিটি Program একটি **Process**। Kernel:

- Process তৈরি করে
- Process বন্ধ করে
- Process Monitoring করে

**উদাহরণ:**

```bash
ps aux
```

এই command current processes-এর snapshot দেখায়।

```bash
top
```

এই command real-time process monitoring করে।

#### 📊 top কমান্ড (Table of Processes)

`top` কমান্ডটি Linux-এর একটি **Real-time System Monitor**। এটি Windows-এর Task Manager-এর মতো কাজ করে। এটি প্রতি ৩ সেকেন্ড পর পর update হতে থাকে এবং system-এ কতটুকু Processor ও RAM ব্যবহার হচ্ছে তা live দেখায়।

**top কমান্ডের প্রধান কলামগুলোর অর্থ:**

| কলাম | অর্থ |
| --- | --- |
| **PID** (Process ID) | প্রতিটি running program-এর একটি unique নম্বর। কোনো program বন্ধ করতে এই ID লাগে। |
| **USER** | কোন user বা account থেকে এই program চালানো হচ্ছে। |
| **%CPU** | Program-টি Processor-এর কত শতাংশ ব্যবহার করছে। |
| **%MEM** | Program-টি RAM-এর কত শতাংশ জায়গা নিয়েছে। |
| **TIME+** | Program চালু হওয়ার পর থেকে মোট কতক্ষণ CPU time ব্যবহার করেছে। |
| **COMMAND** | Running program বা application-এর নাম। |

**💡 প্রয়োজনীয় শর্টকাট (top চলাকালীন কীবোর্ডে চাপুন):**

| শর্টকাট | কাজ |
| --- | --- |
| `M` | RAM (Memory) ব্যবহারের উপর ভিত্তি করে তালিকা সাজায় (Highest to Lowest)। |
| `P` | CPU ব্যবহারের উপর ভিত্তি করে তালিকা সাজায়। |
| `k` | কোনো নির্দিষ্ট process বন্ধ করতে (Kill) — PID নম্বর দিতে হবে। |
| `q` | top screen থেকে বের হয়ে সাধারণ Terminal-এ ফিরে আসে। |

#### 📸 ps aux কমান্ড (Process Status)

`ps` মানে হলো **Process Status**। `top`-এর মতো এটি live update হয় না, বরং এটি command দেওয়ার ঠিক সেই মুহূর্তের (**Snapshot**) system-এ চলমান সকল process-এর একটি বিশাল তালিকা একবারে print করে দেয়।

**aux ফ্ল্যাগ বা অপশনগুলোর অর্থ:**

| ফ্ল্যাগ | অর্থ |
| --- | --- |
| `a` | System-এ যত user আছে, সবার process একসাথে দেখায়। |
| `u` | Process-গুলোর বিস্তারিত তথ্য (CPU, Memory ব্যবহার, user-এর নাম) সহজ ভাষায় দেখায়। |
| `x` | Terminal ছাড়া background-এ স্বয়ংক্রিয়ভাবে চলা process (Daemon/Services)-ও তালিকায় যুক্ত করে। |

**💡 বাস্তব জীবনের ব্যবহার (Real-life Use Case):**

`ps aux` দিয়ে সাধারণত হাজার হাজার লাইনের তালিকা আসে। তাই কোনো নির্দিষ্ট program খুঁজে বের করতে এর সাথে `grep` command ব্যবহার করা হয়।

**উদাহরণ:** আপনার system-এ Chrome browser চলছে কিনা এবং তার PID কত, তা দেখতে:

```bash
ps aux | grep chrome
```

#### ⚖️ সংক্ষেপে top বনাম ps aux

| বৈশিষ্ট্য | top Command | ps aux Command |
| --- | --- | --- |
| কাজের ধরন | Live বা Real-time monitoring। | একটি নির্দিষ্ট মুহূর্তের snapshot। |
| আপডেট | স্বয়ংক্রিয়ভাবে প্রতি কয়েক সেকেন্ড পর পর refresh হয়। | একবারই output দেখায়, নিজে থেকে refresh হয় না। |
| মূল ব্যবহার | System slow হয়ে গেলে কোন app বেশি load নিচ্ছে তা তাৎক্ষণিক দেখতে। | কোনো নির্দিষ্ট process background-এ চলছে কিনা তা search করে বের করতে। |

---

### Filesystem Management

Hard Disk বা SSD-তে Data কোথায় সংরক্ষিত হবে তা Kernel নিয়ন্ত্রণ করে।

**Linux-এর জনপ্রিয় Filesystem:**

| Filesystem | বৈশিষ্ট্য |
| --- | --- |
| **ext4** | Linux-এর সবচেয়ে common এবং stable filesystem। |
| **xfs** | High-performance, বড় file server-এ ব্যবহৃত। |
| **btrfs** | Modern, snapshot ও self-healing support আছে। |
| **zfs** | Advanced, data integrity ও storage pool support। |

Kernel **File Read** এবং **Write** Operation পরিচালনা করে।

---

### Hardware Management

Kernel Keyboard, Mouse, SSD, GPU, Wi-Fi Card, Printer ইত্যাদির সাথে যোগাযোগ করে। এ কাজ **Driver**-এর মাধ্যমে সম্পন্ন হয়।

**কীভাবে কাজ করে:**

```text
Application → Kernel → Driver → Hardware
```

---

### Security Management

Linux Permission System, User Access Control এবং Process Isolation Kernel-এর মাধ্যমেই পরিচালিত হয়।

**উদাহরণ:**

```bash
chmod 755 file.txt
```

```bash
chown user:user file.txt
```

এই command-গুলো Kernel-এর Security Management-এর অংশ।

---

## ১.৫। Linux Distribution কী?

Kernel একা একটি পূর্ণাঙ্গ Operating System নয়। Kernel-এর সাথে Shell, Package Manager, Desktop Environment, Libraries এবং বিভিন্ন Software যুক্ত করে **Linux Distribution** তৈরি করা হয়।

**জনপ্রিয় Linux Distribution:**

| Distribution | Package Manager | বিশেষত্ব |
| --- | --- | --- |
| Ubuntu | `apt` | Beginner-friendly, Stable |
| Debian | `apt` | Stable, Long-term support |
| Fedora | `dnf` | New features, Cutting-edge |
| Rocky Linux | `dnf` | RHEL-compatible, Production |
| AlmaLinux | `dnf` | RHEL-compatible, Production |
| Arch Linux | `pacman` | Rolling release, Advanced |
| openSUSE | `zypper` | DevOps/Server |

---

## ১.৬। সব Distribution কি একই Kernel ব্যবহার করে?

মূল Source Code একই হলেও Distribution ভেদে **Kernel Version** এবং **Configuration** আলাদা হতে পারে।

**উদাহরণ:**

| Distribution | Kernel বৈশিষ্ট্য |
| --- | --- |
| Ubuntu | Stable এবং নতুন Hardware Support-এর মধ্যে ভারসাম্য রাখে। |
| Debian | দীর্ঘমেয়াদী Stability-এর দিকে বেশি গুরুত্ব দেয়। |
| Fedora | নতুন Feature দ্রুত গ্রহণ করে। |
| Arch Linux | Rolling Release Model অনুসরণ করে। |

> 💡 **মজার তথ্য:** Android-ও একটি Modified Linux Kernel ব্যবহার করে।

---

# ভাগ ২: CLI, Shell ও Terminal

## ২.১। CLI কী?

**CLI (Command Line Interface)** হলো Text-Based User Interface।

- **GUI**-তে আমরা Mouse ব্যবহার করি
- **CLI**-তে আমরা Command ব্যবহার করি

**উদাহরণ:**

```bash
mkdir project
cd project
ls -lah
```

**CLI-এর প্রধান সুবিধা:**

| সুবিধা | ব্যাখ্যা |
| --- | --- |
| **দ্রুত** | Mouse click-এর চেয়ে command type করা দ্রুত। |
| **Automation** | Script লিখে repetitive কাজ automate করা যায়। |
| **Remote Server** | SSH দিয়ে দূর থেকে server পরিচালনা করা যায়। |
| **কম Resource** | GUI-এর চেয়ে কম RAM/CPU ব্যবহার করে। |

---

## ২.২। Shell কী?

Shell হলো **User** এবং **Linux Kernel**-এর মধ্যবর্তী **Interpreter Program**।

আপনি Terminal-এ Command লিখলে Shell সেটিকে Process করে Kernel-এর কাছে পাঠায়।

**Workflow:**

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

**Shell-এর কাজ:**

1. User-এর command গ্রহণ করা
2. Command-টি parse করা
3. Kernel-এর কাছে request পাঠানো
4. Kernel থেকে result নিয়ে user-কে দেখানো

---

## ২.৩। জনপ্রিয় Linux Shell

| Shell | Description |
| --- | --- |
| **Bash** | Linux-এর সবচেয়ে জনপ্রিয় Shell (Bourne Again Shell)। |
| **Zsh** | Modern এবং Highly Customizable। |
| **Fish** | Beginner Friendly, Auto-suggestion সহ। |
| **Dash** | Lightweight, দ্রুত। |
| **Ksh** | Korn Shell, Unix-এ জনপ্রিয়। |

**বর্তমান Shell দেখুন:**

```bash
echo $SHELL
```

**Output উদাহরণ:**

```text
/bin/bash
```

---

## ২.৪। CLI কীভাবে কাজ করে?

ধরুন আপনি লিখলেন:

```bash
ls -lah
```

**তখন যা ঘটে:**

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

**ধাপে ধাপে:**

1. **User** → Terminal-এ `ls -lah` type করে Enter চাপে
2. **Shell** → Command গ্রহণ করে parse করে
3. **Shell** → Kernel-এর কাছে request পাঠায়
4. **Kernel** → Filesystem-এ গিয়ে file list পড়ে
5. **Kernel** → Result Shell-কে ফেরত দেয়
6. **Shell** → Result User-এর সামনে display করে

---

## ২.৫। CLI বনাম GUI

| CLI | GUI |
| --- | --- |
| Keyboard ভিত্তিক | Mouse ভিত্তিক |
| দ্রুত | সহজ |
| Automation Friendly | Beginner Friendly |
| কম Resource ব্যবহার করে | বেশি Resource ব্যবহার করে |
| Server Administration-এর জন্য আদর্শ | Desktop ব্যবহারের জন্য আদর্শ |

---

## ২.৬। Kernel এবং CLI-এর সম্পর্ক

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

**মূল কথা:**

- আপনি সরাসরি Kernel-এর সাথে কাজ করেন না
- আপনি Shell বা CLI ব্যবহার করেন
- Shell Kernel-এর সাথে যোগাযোগ করে
- Kernel Hardware-এর সাথে যোগাযোগ করে

> 💡 **এই Architecture বুঝে গেলে** Linux-এর Filesystem, Permission, Package Management, Process Management এবং DevOps সম্পর্কিত পরবর্তী অধ্যায়গুলো বোঝা অনেক সহজ হয়ে যায়।

---

# ভাগ ৩: Linux Filesystem Hierarchy

## ৩.১। SSH দিয়ে Linux Machine-এ প্রবেশ

SSH দিয়ে login করার পর সাধারণত authenticated user-এর home directory-তে থাকবেন।

**উদাহরণ:**

```text
root@Ubuntu:~#
```

**এখানে:**

| অংশ | অর্থ |
| --- | --- |
| `root` | Username |
| `Ubuntu` | Hostname |
| `~` | বর্তমান user-এর home directory |
| `#` | Root user হিসেবে login করা হয়েছে |

> 💡 **নোট:** `$` চিহ্ন সাধারণ user বোঝায়, `#` চিহ্ন root user বোঝায়।

**System-এর root directory-তে যেতে:**

```bash
root@Ubuntu:~# cd /
```

---

## ৩.২। Root Directory-র গুরুত্বপূর্ণ অংশ

Linux-এর root directory (`/`)-র গুরুত্বপূর্ণ অংশগুলো:

| Directory | কাজ |
| --- | --- |
| `/bin` | Linux command চালানোর binary/program/executable ফাইল। |
| `/boot` | Bootloader-এর প্রয়োজনীয় static file এবং kernel boot করার ফাইল। |
| `/cdrom` | CD/DVD media mount করার জন্য ব্যবহৃত directory (যদি থাকে)। |
| `/dev` | Device সম্পর্কিত special file; যেমন keyboard, mouse, disk ইত্যাদি। |
| `/etc` | System-wide configuration file রাখার প্রধান directory। |
| `/home` | সাধারণ user-দের home directory। |
| `/lib` | System-এর প্রয়োজনীয় shared library। |
| `/lib32` | 32-bit library (যদি system-এ থাকে)। |
| `/lib64` | 64-bit library। |
| `/lost+found` | Filesystem recovery-এর সময় পাওয়া orphaned file রাখার জায়গা। |
| `/media` | Removable media, যেমন USB/SD card-এর mount point। |
| `/mnt` | Temporary/manual mount করার জন্য ব্যবহৃত directory। |
| `/opt` | Optional বা third-party software রাখার জন্য। |
| `/proc` | Running process ও kernel-এর virtual information filesystem। |
| `/root` | Root user-এর home directory। |
| `/run` | বর্তমানে চলমান system/process-এর runtime data। |
| `/sbin` | System administration-এর binary/command। |
| `/snap` | Snap package-এর data ও mount point। |
| `/srv` | System যে service-related data পরিবেশন করে তার জন্য। |
| `/sys` | Kernel ও hardware সম্পর্কিত virtual information filesystem। |
| `/tmp` | Temporary file রাখার directory। |
| `/usr` | User-space applications, libraries, documentation ইত্যাদি। |
| `/var` | পরিবর্তনশীল data; যেমন log, cache, spool ইত্যাদি। |

> ⚠️ **মনে রাখুন:** `/` হলো system root directory, আর `/root` হলো root user-এর home directory। এরা完全不同।

---

# ভাগ ৪: Help নেওয়া ও Man Page

## ৪.১। man Command

`man` হলো system reference manual পড়ার interface।

**ব্যবহার:**

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

**কিছু গুরুত্বপূর্ণ অর্থ:**

| Command | কাজ |
| --- | --- |
| `man man` | `man` command-এর manual দেখায়। |
| `man lsblk` | Block device সম্পর্কিত manual। |
| `man sshd` | OpenSSH server daemon-এর manual। |
| `man mandb` | Manual page index তৈরি/আপডেট করার তথ্য। |
| `whereis mandb` | Binary, source এবং manual-এর অবস্থান খুঁজে দেয়। |
| `whatis mandb` | Command-এর সংক্ষিপ্ত description দেখায়। |
| `man 5 mandb` | নির্দিষ্ট manual section-এর entry দেখায়। |
| `cd --help` | Command-এর available option সম্পর্কে সাহায্য দেয়। |

## ৪.২। অন্যান্য Help Command

| Command | কাজ |
| --- | --- |
| `--help` | Command-এর সংক্ষিপ্ত help দেখায়। |
| `info` | GNU-র detailed documentation। |
| `apropos` | Keyword দিয়ে manual page খুঁজে বের করে। |

**উদাহরণ:**

```bash
ls --help
apropos "copy files"
```

---

# ভাগ ৫: Directory Management

## ৫.১। Root-এর অর্থ

Linux-এ প্রায় সবকিছুই file হিসেবে বিবেচিত হয়; directory-ও একটি বিশেষ ধরনের file structure।

| চিহ্ন | অর্থ |
| --- | --- |
| `/` | System root directory। |
| `/root` | Root user-এর home directory। |
| `~` | বর্তমানে login করা user-এর home directory। |

**উদাহরণ:**

```text
root@hostname:~#
```

এখানে `~` root user-এর home directory বোঝায়। আবার:

```text
sakil@hostname:~$
```

এখানে `~` Sakil user-এর home directory বোঝায়।

---

## ৫.২। গুরুত্বপূর্ণ Directory Command

| Command | কাজ |
| --- | --- |
| `pwd pathname` | বর্তমান working directory দেখায়। |
| `cd pathname` | Relative path ব্যবহার করে directory পরিবর্তন করে। |
| `cd /pathname` | Absolute path ব্যবহার করে directory পরিবর্তন করে। |
| `cd ~` | বর্তমান user-এর home directory-তে যায়। |
| `cd -` | আগের directory-তে ফিরে যায়। |
| `ls` | বর্তমান directory-র item দেখায়। |
| `ls /var/log` | নির্দিষ্ট directory-র item দেখায়। |
| `ls -l` | বিস্তারিত list দেখায়। |
| `ls -la` | Hidden file-সহ বিস্তারিত list দেখায়। |
| `ls -lah` | Hidden file-সহ human-readable size দেখায়। |
| `ls -ldh` | Directory-র নিজস্ব তথ্য human-readable format-এ দেখায়। |
| `ls -li` | Inode number-সহ list দেখায়। |
| `ll` | সাধারণত `ls -l`-এর alias। |
| `mkdir dirName` | একটি খালি directory তৈরি করে। |
| `mkdir -p parent/child` | Parent ও child directory একসাথে তৈরি করে। |
| `rmdir pathOfDir` | খালি directory মুছে দেয়। |
| `rm -rf parent/child` | Directory ও তার contents forcefully মুছে দেয়। |

> ⚠️ **সতর্কতা:** `rm -rf` অত্যন্ত সতর্কতার সঙ্গে ব্যবহার করুন। ভুল path দিলে গুরুত্বপূর্ণ data মুছে যেতে পারে।

**উদাহরণ:**

```bash
mkdir -p sakil/os/fedora sakil/os/debian
```

এটি একসাথে multiple nested directory তৈরি করে।

```bash
rmdir dir1 dir2 dir3
```

এটি একাধিক খালি directory মুছে দেয়।

---

## ৫.৩। Dot (`.` এবং `..`)-এর অর্থ

| চিহ্ন | অর্থ |
| --- | --- |
| `.` | বর্তমান directory-র reference। |
| `..` | Parent directory-র reference। |

**উদাহরণ:**

```bash
cd ..        # Parent directory-তে যাও
cd ../..     # দুই ধাপ parent directory-তে যাও
./script.sh  # Current directory-র script চালাও
```

---

# ভাগ ৬: File Management

## ৬.১। touch Command

```bash
touch filename
```

**কাজ:**

- একটি খালি file তৈরি করে
- File-এর timestamp update করে

**একাধিক file তৈরি:**

```bash
touch {1..100}.txt
```

এটি `1.txt` থেকে `100.txt` পর্যন্ত একাধিক file তৈরি করে।

**নির্দিষ্ট timestamp সেট:**

```bash
touch -t 202601011030 filename
```

---

## ৬.২। cat ও tac

**File-এর content দেখা:**

```bash
cat fileName
```

**নতুন content লিখে file তৈরি/overwrite:**

```bash
cat > fileName
```

> ⚠️ Existing content overwrite হয়ে যাবে। `Ctrl+C` দিয়ে prompt থেকে বের হওয়া যায়।

**EOF marker ব্যবহার:**

```bash
cat > file.txt << EOF
content
EOF
```

এখানে `EOF` একটি নির্ধারিত termination marker হিসেবে কাজ করে।

**Existing content রেখে নতুন content append:**

```bash
cat >> fileName
```

```bash
cat >> file.txt << EOF
new content
EOF
```

**File-এর content উল্টো ক্রমে দেখা:**

```bash
tac fileName
```

---

## ৬.৩। head ও tail

| Command | কাজ |
| --- | --- |
| `head fileName` | সাধারণত প্রথম ১০টি line দেখায়। |
| `head -5 fileName` | প্রথম ৫টি line দেখায়। |
| `head -5 fileName \| tac` | প্রথম ৫টি line উল্টো ক্রমে দেখায়। |
| `tail fileName` | সাধারণত শেষ ১০টি line দেখায়। |
| `tail -5 fileName` | শেষ ৫টি line দেখায়। |
| `tail -5 fileName \| tac` | শেষ ৫টি line উল্টো ক্রমে দেখায়। |

---
noc@dev@

## ৬.৪। অন্যান্য দরকারি Command

| Command | কাজ |
| --- | --- |
| `file filename` | File-এর ধরন শনাক্ত করে। |
| `mv source new` | File বা directory rename অথবা move করে। |
| `cp source dest` | File বা directory copy করে। |
| `cp -r /copy/from/* /destination/path/` | একটি ফোল্ডারের ভেতরের সবকিছু copy করে। |
| `cp -r /copy/from/. /destination/path/` | Hidden (`.filename`) file-সহ সবকিছু copy করে। |
| `hostnamectl` | Linux OS-এর নাম ও version দেখায়। |
| `hostnamectl set-hostname your-new-hostname` | Static hostname সেট করে। |
| `cat /proc/cpuinfo` | CPU-র তথ্য দেখায়। |
| `apt update && apt install nano -y` | Container বা Linux distribution-এ `nano` editor install করে। |

---

# ভাগ ৭: File Permission

Linux Administration এবং DevOps-এ **File Permission** অত্যন্ত গুরুত্বপূর্ণ। কোন user কোন file বা directory কীভাবে ব্যবহার করতে পারবে—তা permission এবং access-control system দ্বারা নির্ধারিত হয়।

---

## ৭.১। Permission-এর মূল ধারণা

প্রতিটি Linux file/directory-র permission **তিন ধরনের user**-এর জন্য নির্ধারিত হয়:

| User | Symbol | অর্থ |
| --- | --- | --- |
| Owner | `u` | File-এর owner |
| Group | `g` | File-এর group-এর সদস্য |
| Others | `o` | অন্য সবাই |

**Permission:**

| Permission | File | Directory |
| --- | --- | --- |
| `r` | File পড়া | Directory-র entry/name list করা |
| `w` | File-এর content পরিবর্তন | File create/delete/rename করা |
| `x` | Program execute করা | Directory traverse/enter করা |

---

## ৭.২। File Type ও Permission Format

**উদাহরণ:**

```text
-rwxr-xr--
drwxr-xr-x
lrwxrwxrwx
```

**প্রথম character file type:**

| Character | অর্থ |
| --- | --- |
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `c` | Character device |
| `b` | Block device |
| `s` | Socket |
| `p` | Named pipe |

**পরের ৯টি character:**

```text
rwx r-x r--
│   │   │
│   │   └── Others
│   └────── Group
└────────── Owner
```

**উদাহরণ:**

```text
-rwxr-xr--
```

| অংশ | Permission |
| --- | --- |
| Owner | `rwx` |
| Group | `r-x` |
| Others | `r--` |

---

## ৭.৩। File বনাম Directory Permission

**File-এর ক্ষেত্রে:**

```text
r = content পড়া
w = content পরিবর্তন
x = executable হিসেবে চালানো
```

**Directory-এর ক্ষেত্রে:**

```text
r = directory-র নাম list করা
w = entry create/delete/rename করা
x = directory traverse/enter করা
```

> ⚠️ **মনে রাখুন:** একই `rwx` permission file এবং directory-তে একই অর্থ বহন করে না।

---

## ৭.৪। Directory-এর x Permission

Directory-র `x` permission-এর অর্থ execute নয়; এখানে মূল অর্থ **traverse**।

**ধরো:**

```text
project/
└── secret/
    └── data.txt
```

`secret/`-এ `x` permission না থাকলে directory-তে প্রবেশ বা path traverse করা যাবে না।

**Directory-তে সাধারণত:**

```text
r + x → list + traverse
w + x → entry create/delete/rename
```

**Parent directory-গুলোর `x` permission-ও file access-এর জন্য গুরুত্বপূর্ণ।**

**Path troubleshooting:**

```bash
namei -l /home/sakil/project/app/file.txt
```

---

## ৭.৫। ls -l Output

```bash
ls -l
```

**উদাহরণ:**

```text
-rwxr-xr-- 1 root root 1200 May 2 02:52 devops.txt
```

**এখানে:**

| অংশ | অর্থ |
| --- | --- |
| `-rwxr-xr--` | File type + permission |
| `1` | Hard link count |
| `root` | Owner |
| `root` | Group |
| `1200` | Size |
| `May 2 02:52` | Modification time |
| `devops.txt` | File name |

**দরকারি command:**

```bash
ls -la
ls -lah
ls -ldh directory/
```
এই কমান্ড তিনটির একটি সংক্ষিপ্ত সারসংক্ষেপ নিচে দেওয়া হলো:
 
* ls -la: বর্তমান ডিরেক্টরির সব ফাইল ও ফোল্ডারের (লুকানো বা হিডেন ফাইলসহ) একটি বিস্তারিত তালিকা (পারমিশন, সাইজ, মালিকানা ইত্যাদি) দেখায়।
* ls -lah: এটিও আগের কমান্ডের মতোই সব ফাইলের বিস্তারিত তালিকা দেখায়, তবে এখানে ফাইলের সাইজগুলো মানুষের সহজে বোঝার উপযোগী ফরম্যাটে (যেমন: K, M, G বা কিলোবাইট, মেগাবাইট, গিগাবাইট) প্রদর্শন করে।
* ls -ldh directory/: নির্দিষ্ট একটি ফোল্ডারের (directory) ভেতরের ফাইলগুলো না দেখিয়ে, শুধু সেই ফোল্ডারটির নিজস্ব পারমিশন, সাইজ এবং বিস্তারিত তথ্য সহজে বোঝার উপযোগী ফরম্যাটে (Human-readable) দেখায়।

### Hard link count
এই লাইনে 1 সংখ্যাটি দিয়ে বোঝানো হচ্ছে যে devops.txt ফাইলটির জন্য সিস্টেমে মাত্র ১টি হার্ড লিঙ্ক (Hard Link) আছে।
সহজ ভাষায়, Hard Link Count বলতে বোঝায়:

* ফাইলের নাম বা শর্টকাটের সংখ্যা: লিনাক্সে একটি ফাইল আসলে ডিস্কের একটি নির্দিষ্ট জায়গায় (যাকে ইনোড বা inode বলা হয়) জমা থাকে। আর ফাইলের নামটি হলো সেই জায়গায় পৌঁছানোর একটি রাস্তা বা লিঙ্ক।
* ১ থাকার অর্থ: এই ফাইলটির ডাটা বা ইনোডকে নির্দেশ করার জন্য পুরো সিস্টেমে কেবল এই একটি নামই (devops.txt) আছে।
* সংখ্যাটি কখন বাড়ে?: আপনি যদি এই ফাইলটির আরেকটি হার্ড লিঙ্ক তৈরি করেন (যেমন: ln devops.txt backup.txt), তাহলে এই সংখ্যাটি ২ হয়ে যাবে। তখন দুটি ফাইলের নামই ডিস্কের একই ডাটাকে নির্দেশ করবে এবং একটিতে এডিট করলে অন্যটিও বদলে যাবে।
* ফোল্ডারের ক্ষেত্রে: সাধারণ ফাইলের ক্ষেত্রে এই সংখ্যা সাধারণত 1 দিয়ে শুরু হলেও, কোনো ফোল্ডার বা ডিরেক্টরির ক্ষেত্রে এই সংখ্যাটি কমপক্ষে ২ বা তার বেশি হয়। কারণ ফোল্ডারের ভেতরে থাকা নিজস্ব . (current directory) এবং সাব-ফোল্ডারের .. (parent directory) এই কাউন্টকে বাড়িয়ে দেয়।

---

# ভাগ ৮: chmod — Permission পরিবর্তন

## ৮.১। chmod Symbolic Mode

`chmod` permission পরিবর্তনের জন্য ব্যবহৃত হয়।

| Symbol | অর্থ |
| --- | --- |
| `u` | Owner |
| `g` | Group |
| `o` | Others |
| `a` | All |
| `+` | Permission যোগ |
| `-` | Permission সরানো |
| `=` | Permission নির্দিষ্টভাবে সেট |

**উদাহরণ:**

```bash
chmod o+wx devops.txt
chmod u+x devops.txt
chmod u-w devops.txt
chmod g=rw devops.txt
```

**একাধিক operation:**

```bash
chmod o-wx,u+w,g=rx devops.txt
```

**সম্পূর্ণ permission সেট:**

```bash
chmod u=rw,g=r,o= file.txt
```

---

## ৮.২। chmod Numeric / Octal Mode

**Permission-এর value:**

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

**কারণ:**

```text
r = 4
w = 2
x = 1
```

**তাই:**

```text
rwx = 4 + 2 + 1 = 7
rw- = 4 + 2 + 0 = 6
r-x = 4 + 0 + 1 = 5
r-- = 4 + 0 + 0 = 4
```

**উদাহরণ:**

```bash
chmod 754 file.txt
```

**এর অর্থ:**

| User | Value | Permission |
| --- | --- | --- |
| Owner | 7 | `rwx` |
| Group | 5 | `r-x` |
| Others | 4 | `r--` |

---

## ৮.৩। Common Modes

| Mode | Owner | Group | Others | ব্যবহার |
| --- | --- | --- | --- | --- |
| `755` | `rwx` | `r-x` | `r-x` | Executable file, Directory |
| `644` | `rw-` | `r--` | `r--` | Regular file |
| `700` | `rwx` | `---` | `---` | Private file/directory |
| `750` | `rwx` | `r-x` | `---` | Directory (group access) |
| `770` | `rwx` | `rwx` | `---` | Shared directory |

**উদাহরণ:**

```bash
chmod 755 file
chmod 644 file
chmod 700 file
chmod 750 directory
chmod 770 directory
```

---

## ৮.৪। Recursive Permission

```bash
chmod -R ...
```

`-R` মানে **recursive**। তবে blindভাবে `chmod -R 755 project/` করলে regular file-ও executable হয়ে যেতে পারে।

**কিছু পরিস্থিতিতে:**

```bash
chmod -R a+X project/
```

chmod -R a+X project/ কমান্ডটি project/ নামক ফোল্ডার এবং এর ভেতরের সব ফোল্ডারের জন্য বিশেষ এক ধরনের এক্সিকিউট (Execute) বা প্রবেশ করার অনুমতি (Permission) দেয়।
সহজ ভাষায় এর ব্যাখ্যা নিচে দেওয়া হলো:

* chmod: এটি ফাইল বা ফোল্ডারের পারমিশন (অনুমতি) পরিবর্তন করার মূল কমান্ড (Change Mode)।
* -R: এর মানে হলো Recursive। এটি ফোল্ডারের ভেতরের সমস্ত সাব-ফোল্ডার (Sub-folders) এবং ফাইলের ওপর একই নিয়ম প্রয়োগ করে।
* a+X: এখানে তিনটি অংশ রয়েছে:
* a (All): সিস্টেমের সব ধরনের ব্যবহারকারী (মালিক, গ্রুপ এবং অন্যান্য সবাই)-এর জন্য এই নিয়ম কাজ করবে।
   * +: অনুমতি বা পারমিশন যোগ (Add) করা হচ্ছে।
   * X (Capital X): এটি একটি বিশেষ এক্সিকিউট পারমিশন। এটি শুধুমাত্র ফোল্ডারগুলোর (Directories) জন্য এক্সিকিউট পারমিশন দেয় (যাতে ফোল্ডারে প্রবেশ করা যায়), কিন্তু সাধারণ ফাইলগুলোর পারমিশন পরিবর্তন করে না (যদি না ফাইলটিতে আগে থেকেই কোনো এক্সিকিউট পারমিশন দেওয়া থাকে)।

মূল সুবিধা: সাধারণ ছোট হাতের x ব্যবহার করলে ফোল্ডারের পাশাপাশি ভেতরের সব সাধারণ ফাইলও এক্সিকিউটেবল হয়ে যেত, যা নিরাপত্তার জন্য ঝুঁকিপূর্ণ। কিন্তু বড় হাতের X ব্যবহার করায় শুধুমাত্র ফোল্ডারগুলোতে প্রবেশের অনুমতি চালু হয়, সাধারণ ফাইলগুলো সুরক্ষিত থাকে।

---

# ভাগ ৯: chown ও chgrp

**File-এর owner/group দেখতে:**

```bash
ls -l file.txt
```

**Owner পরিবর্তন:**

```bash
sudo chown john file.txt
```

**Owner + Group:**

```bash
sudo chown john:developers file.txt
```

**শুধু Group:**

```bash
sudo chown :developers file.txt
```

**Recursive:**

```bash
sudo chown -R john:developers project/
```

> ⚠️ `-R` খুব সাবধানে ব্যবহার করতে হবে।

**Group পরিবর্তন:**

```bash
sudo chgrp developers file.txt
sudo chgrp -R developers project/
```
sudo chgrp কমান্ড দুটি কোনো ফাইল বা ফোল্ডারের গ্রুপ মালিকানা (Group Ownership) পরিবর্তন করার জন্য ব্যবহার করা হয়।
সহজ ভাষায় কমান্ড দুটির ব্যাখ্যা নিচে দেওয়া হলো:

* sudo chgrp developers file.txt: এই কমান্ডটি file.txt নামক নির্দিষ্ট ফাইলটির গ্রুপ মালিকানা পরিবর্তন করে developers নামক গ্রুপকে দিয়ে দেয়। এর ফলে ওই গ্রুপের সদস্যরা ফাইলের পারমিশন অনুযায়ী সেটি দেখতে বা এডিট করতে পারবে।
* sudo chgrp -R developers project/: এখানে -R (Recursive) ফ্ল্যাগ ব্যবহার করা হয়েছে। এর অর্থ হলো এটি project/ ফোল্ডারের পাশাপাশি এর ভেতরে থাকা সমস্ত সাব-ফোল্ডার এবং ফাইলের গ্রুপ মালিকানা একসাথে developers গ্রুপে পরিবর্তন করে দেবে।

কমান্ডের মূল অংশগুলোর কাজ:

* sudo: এটি অ্যাডমিনিস্ট্রেটর বা রুট (Root) প্রিভিলেজ নিয়ে কমান্ডটি রান করে, কারণ সাধারণ ব্যবহারকারী অন্য গ্রুপের কাছে মালিকানা হস্তান্তর করতে পারে না।
* chgrp: এটি গ্রুপ পরিবর্তনের মূল কমান্ড (Change Group)।

---

# ভাগ ১০: umask

নতুন file/directory তৈরি হওয়ার সময় default permission-এ `umask` গুরুত্বপূর্ণ।

**দেখুন:**

```bash
umask
```

**সাধারণ example:**

```text
0022
```

**সহজ conceptual model:**

```text
Directory base = 777
File base      = 666

777 - 022 → 755
666 - 022 → 644
```

**তাই সাধারণভাবে:**

| ধরন | Default Permission |
| --- | --- |
| Directory | `755` |
| File | `644` |

**Temporary পরিবর্তন:**

```bash
umask 027
```

> 💡 Permission calculation-কে সহজভাবে বোঝাতে subtraction model ব্যবহার করা হয়; Linux বাস্তবে permission bitmask ব্যবহার করে।

---

# ভাগ ১১: Special Permissions

Linux-এর তিনটি গুরুত্বপূর্ণ special permission:

```text
SUID
SGID
Sticky Bit
```

---

## ১১.১। SUID

Executable file-এ SUID থাকলে execution-এর সময় file owner-এর effective identity ব্যবহার করা যেতে পারে।

**উদাহরণ দেখতে:**

```bash
ls -l /usr/bin/passwd
```

SUID থাকলে owner execute position-এ `s` দেখা যেতে পারে:

```text
-rwsr-xr-x
```

এটি একটি ফাইলের স্পেশাল পারমিশন (Special Permission) স্ট্রিং। লিনাক্সে ফাইলের পারমিশন সাধারণত ১০টি ক্যারেক্টার দিয়ে প্রকাশ করা হয়।
এখানে -rwsr-xr-x এর বিস্তারিত ব্যাখ্যা নিচে দেওয়া হলো:
## ১. ১০টি ক্যারেক্টারের ভাগ (Breakdown)
পারমিশন স্ট্রিংটিকে ৪টি মূল অংশে ভাগ করা যায়:

* - (প্রথম ক্যারেক্টার): এটি নির্দেশ করে যে এটি একটি সাধারণ ফাইল (Regular File)। (ফোল্ডার হলে এখানে d থাকতো)।
* rws (পরের ৩টি): ফাইলের মালিকের (Owner/User) পারমিশন।
* x-r (পরের ৩টি): ফাইলের গ্রুপের (Group) পারমিশন।
* x-x (শেষের ৩টি): সিস্টেমের অন্যান্য সবার (Others) পারমিশন।

------------------------------
## ২. মূল রহস্য: বড় হাতের বা ছোট হাতের s (SUID)
মালিকের পারমিশনে সাধারণ rwx এর জায়গায় rws লেখা আছে। এই s এর মানে হলো ফাইলটিতে SUID (Set User ID) পারমিশন দেওয়া আছে।

* SUID কী?: যখন কোনো ফাইলে SUID সেট করা থাকে, তখন যেকোনো সাধারণ ব্যবহারকারী ফাইলটি রান (Execute) করলেও ফাইলটি এমনভাবে চলবে যেন স্বয়ং ফাইলের মালিক (Owner) সেটি রান করছে।
* বাস্তব উদাহরণ: লিনাক্সের /usr/bin/passwd ফাইলে এটি থাকে। এর ফলে একজন সাধারণ ইউজার নিজের পাসওয়ার্ড পরিবর্তন করার সময় সাময়িকভাবে রুট (Root) প্রিভিলেজ বা ক্ষমতা পায়, যা পাসওয়ার্ড ফাইলটি আপডেট করার জন্য প্রয়োজন।

------------------------------
## ৩. প্রতিটি গ্রুপের আলাদা পারমিশন টেবিল

| ব্যবহারকারী (User Type) | পারমিশন ক্যারেক্টার | সহজ অর্থ |
|---|---|---|
| Owner (মালিক) | rws | ফাইলটি পড়তে পারবে (r), লিখতে পারবে (w), এবং এটি SUID হিসেবে রান (s) হবে। |
| Group (গ্রুপ) | xr- | গ্রুপের সদস্যরা ফাইলটি পড়তে পারবে (r) এবং রান করতে পারবে (x), কিন্তু এডিট বা পরিবর্তন (-) করতে পারবে না। |
| Others (অন্যান্য) | x-x | (নোট: আপনার দেওয়া স্ট্রিংয়ে xr- ও x-x আছে, সাধারণত এটি r-x হয়)। এখানে x-x থাকলে অন্য ইউজাররা ফাইলটি পড়তে পারবে না, শুধু রান করতে পারবে। |


**Numeric value:**

```text
SUID = 4
```

**উদাহরণ:**

```bash
chmod 4755 file
```

> ⚠️ SUID security-sensitive। শুধু প্রয়োজনীয় ক্ষেত্রে ব্যবহার করুন।

---

## ১১.২। SGID

Directory-তে SGID team/shared directory-র জন্য খুব useful।

```bash
chmod g+s /project
```

**অথবা:**

```bash
chmod 2775 /project
```

**Numeric value:**

```text
SGID = 2
```

SGID directory-তে নতুন child file/directory সাধারণত parent-এর group inheritance পায়।

---

## ১১.৩। Sticky Bit

Shared writable directory-তে Sticky Bit ব্যবহার করা হয়।

**উদাহরণ:**

```bash
ls -ld /tmp
```

**সাধারণত:**

```text
drwxrwxrwt
```

শেষের `t` হলো Sticky Bit।

**Set:**

```bash
chmod +t shared/
```

**Remove:**

```bash
chmod -t shared/
```

**Numeric value:**

```text
Sticky Bit = 1
```

**উদাহরণ:**

```bash
chmod 1777 shared/
```

**Special permission values:**

| Permission | Value |
| --- | ---: |
| SUID | 4 |
| SGID | 2 |
| Sticky Bit | 1 |

---

# ভাগ ১২: ACL

Traditional permission model:

```text
Owner
Group
Others
```

কিন্তু নির্দিষ্ট একাধিক user-কে আলাদা permission দিতে হলে **ACL** ব্যবহার করা যায়।

**উদাহরণ:**

```text
rahim      → read/write
karim      → read-only
developers → read/write
others     → no access
```

**ACL দেখা:**

```bash
getfacl file.txt
```

**নির্দিষ্ট user-কে permission:**

```bash
setfacl -m u:rahim:rw file.txt
setfacl -m u:karim:r file.txt
```

**ACL entry remove:**

```bash
setfacl -x u:rahim file.txt
```

**সব ACL remove:**

```bash
setfacl -b file.txt
```

## ACL Mask

ACL-এর `mask` effective permission সীমাবদ্ধ করতে পারে। তাই শুধু user entry দেখে সিদ্ধান্ত নেওয়া উচিত নয়।

## Default ACL

Directory-র future child objects-এর জন্য:

```bash
setfacl -d -m g:developers:rwx project/
```

Team-based shared directory-তে এটি useful।

---

# ভাগ ১৩: Permission Troubleshooting

**Common error:**

```text
Permission denied
```

সরাসরি `chmod 777` না দিয়ে systematicভাবে troubleshoot করুন:

### ধাপ ১: Current user দেখুন

```bash
whoami
```

### ধাপ ২: UID/GID ও groups দেখুন

```bash
id
groups
```

### ধাপ ৩: File permission ও ownership দেখুন

```bash
ls -l file.txt
```

### ধাপ ৪: Parent path চেক করুন

```bash
namei -l /path/to/file.txt
```

### ধাপ ৫: ACL চেক করুন

```bash
getfacl file.txt
```

### ধাপ ৬: Detailed metadata দেখুন

```bash
stat file.txt
```

### ধাপ ৭: Process user দেখুন

```bash
ps aux
```

Server application-এর ক্ষেত্রে process/service কোন user হিসেবে চলছে সেটি গুরুত্বপূর্ণ।

---

# ভাগ ১৪: sudo, Root ও Least Privilege

`sudo` elevated privilege-এ command চালাতে ব্যবহৃত হয়।

```bash
sudo chown root:root file.txt
```

**অপ্রয়োজনে সব command-এ `sudo` ব্যবহার করা উচিত নয়।** বিশেষ করে:

```bash
sudo npm install
sudo composer install
sudo git ...
```

এগুলো ownership সমস্যা তৈরি করতে পারে।

## Root

`root` user-এর খুব উচ্চ privilege আছে। Production-এ application/service প্রয়োজন অনুযায়ী **dedicated non-root user** হিসেবে চালানো নিরাপদ পদ্ধতি।

## Principle of Least Privilege

> যে user/process-এর যতটুকু access দরকার, শুধু ততটুকুই দেওয়া উচিত।

---

# ভাগ ১৫: Laravel, Nginx/PHP-FPM ও Docker

## Laravel

Application code এবং runtime-writable directory আলাদা করে ভাবুন।

সাধারণত Laravel application process-এর writable access প্রয়োজন হতে পারে:

```text
storage/
bootstrap/cache/
```

**সব project-এ:**

```bash
chmod -R 777 .
```

**দেওয়া উচিত নয়।**

**মূল ধারণা:**

```text
Application code
    ↓
Mostly read-only

Runtime directories
    ↓
Application process-এর জন্য writable
```

## Nginx + PHP-FPM

**সাধারণ architecture:**

```text
Browser
   ↓
Nginx
   ↓
PHP-FPM
   ↓
Laravel
```

**Permission troubleshoot করার সময়:**

```text
File Owner
Group
Process User
```

এই তিনটি আলাদা করে দেখতে হবে।

## Docker

Docker-এ host এবং container-এর UID/GID mismatch হলে bind mount বা volume-এ permission সমস্যা হতে পারে।

```text
Host UID/GID
      +
Container UID/GID
      +
Bind Mount / Volume
      ↓
File Access
```

বিশেষ করে Laravel `storage/`, bind mount, Docker volume এবং `node_modules`-এর ক্ষেত্রে UID/GID গুরুত্বপূর্ণ।

---

# ভাগ ১৬: SELinux/AppArmor

**Traditional Linux permission:**

```text
Owner
Group
Others
```

এর পাশাপাশি system-এ additional security policy থাকতে পারে:

```text
SELinux
AppArmor
```

তাই কখনও:

```text
chmod ঠিক
chown ঠিক
```

হওয়ার পরও:

```text
Permission denied
```

হতে পারে।

কারণ access decision-এ additional security policy প্রভাব ফেলতে পারে।

---

# ভাগ ১৭: Permission Audit Commands

## stat

```bash
stat file.txt
```

Detailed metadata এবং permission দেখতে।

## namei

```bash
namei -l /var/www/project/storage/app.log
```

পুরো path-এর প্রতিটি component-এর permission দেখতে।

## find

**Writable file খুঁজতে:**

```bash
find /var/www -type f -perm -002
```

**নির্দিষ্ট 777 permission খুঁজতে:**

```bash
find /var/www -type f -perm 777
```

**SUID:**

```bash
find / -perm -4000 -type f 2>/dev/null
```

**SGID:**

```bash
find / -perm -2000 -type f 2>/dev/null
```

> ⚠️ System-wide `find /` অনেক result দিতে পারে এবং permission-related error দেখা স্বাভাবিক।

---

# ভাগ ১৮: Best Practices

### ১। অপ্রয়োজনে 777 ব্যবহার করবেন না

```bash
chmod 777 file
```

এতে Owner, Group এবং Others সবাইকে full permission দেওয়া হয়।

### ২। Ownership আগে বুঝুন

```bash
ls -l file
```

তারপর প্রয়োজন অনুযায়ী `chown` / `chgrp` ব্যবহার করুন।

### ৩। File এবং Directory আলাদা করে ভাবুন

অনেক ক্ষেত্রে:

```text
Directory → 755
Regular file → 644
```

ব্যবহার করা হয়, তবে application requirement অনুযায়ী permission পরিবর্তন হতে পারে।

### ৪। Recursive command সাবধানে ব্যবহার করুন

```bash
chmod -R
chown -R
chgrp -R
```

ভুল path-এ চালানো বিপজ্জনক।

### ৫। ACL থাকলে ACL check করুন

```bash
getfacl file.txt
```

### ৬। Process identity বুঝুন

```text
File owner ≠ Application process user
```

হতে পারে।

---

# 🧠 Permission Troubleshooting Mental Model

কোনো file access করতে না পারলে:

```text
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
```

---

# Quick Revision

| বিষয় | Command / Concept |
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

> **পরবর্তী ধাপ:** Linux User/Group Management-এর সঙ্গে File Permission combine করে user + group + ownership + chmod + ACL নিয়ে practical lab করা সবচেয়ে উপকারী হবে।

---

> [🏠](../) [⬅️ ০০। সংক্ষিপ্ত প্রশ্নোত্তর](../০০-সংক্ষিপ্ত-প্রশ্নোত্তর) [➡️ ০১.১। ইউজার ম্যানেজমেন্ট](../০১.১-ইউজার-ম্যানেজমেন্ট)