# লিনাক্স ইউজার ম্যানেজমেন্ট ---

1. [ইউজারের প্রকারভেদ](#১-লিনাক্সে-ইউজারের-প্রকারভেদ)
2. [গ্রুপ কী?](#২-গ্রুপ-কী)
3. [গুরুত্বপূর্ণ কনফিগারেশন ফাইল](#৩-ইউজার-ম্যানেজমেন্টের-গুরুত্বপূর্ণ-ফাইলসমূহ)
4. [জরুরি কমান্ডসমূহ](#৪-জরুরি-কমান্ডসমূহ)
5. [ইউজার পারমিশন (chmod, chown)](#৫-ইউজার-পারমিশন-chmod-chown)
6. [sudo প্রিভিলেজ দেওয়া](#৬-একজন-ইউজারকে-sudo-প্রিভিলেজ-দেওয়া)
7. [/etc/passwd ফাইল বিশ্লেষণ](#৭-etcpasswd-ফাইলের-ব্যাখ্যা)
8. [পাসওয়ার্ড এজিং ও chage](#৮-পাসওয়ার্ড-এজিং-ও-chage-কমান্ড)
9. [su বনাম sudo](#৯-su-বনাম-sudo)
10. [বিশেষ পারমিশন বিট (SUID, SGID, Sticky Bit)](#১০-বিশেষ-পারমিশন-বিট)

---

## ১. লিনাক্সে ইউজারের প্রকারভেদ

লিনাক্স একটি **মাল্টি-ইউজার** অপারেটিং সিস্টেম — একসাথে একাধিক ব্যবহারকারী কাজ করতে পারেন।

| ইউজার টাইপ | UID | বৈশিষ্ট্য |
|------------|-----|-----------|
| **রুট (Root/Superuser)** | 0 | সিস্টেমের অ্যাডমিন; আনলিমিটেড ক্ষমতা |
| **সিস্টেম ইউজার** | 1–999 | সার্ভিস (Apache, MySQL) চালানোর জন্য; লগইন নেই |
| **সাধারণ ইউজার** | 1000+ | নিজের হোম ডিরেক্টরিতে সীমাবদ্ধ |

---

## ২. গ্রুপ কী?

অনেক ইউজারকে একসাথে ম্যানেজ করার ব্যবস্থা।

- **Primary Group:** ইউজার তৈরির সময় অটোমেটিক তার নামে তৈরি হয়
- **Secondary Group:** অতিরিক্ত পারমিশনের জন্য ইউজারকে অন্য গ্রুপে যুক্ত করা হয়

---

## ৩. ইউজার ম্যানেজমেন্টের গুরুত্বপূর্ণ ফাইলসমূহ

| ফাইল | কাজ |
|------|-----|
| `/etc/passwd` | ইউজারের নাম, UID, হোম ডিরেক্টরি, শেল |
| `/etc/shadow` | এনক্রিপ্টেড পাসওয়ার্ড ও এজিং তথ্য |
| `/etc/group` | গ্রুপের তালিকা ও মেম্বার |
| `/etc/gshadow` | গ্রুপের সুরক্ষিত পাসওয়ার্ড |

---

## ৪. জরুরি কমান্ডসমূহ

### ইউজার তৈরি ও ডিলিট:
```bash
sudo useradd username        # নতুন ইউজার
sudo passwd username         # পাসওয়ার্ড সেট
sudo userdel username        # ইউজার ডিলিট
sudo userdel -r username     # হোম ডিরেক্টরিসহ ডিলিট
```

### ইউজার মডিফাই:
```bash
sudo usermod -aG sudo rahim      # sudo গ্রুপে যোগ
sudo usermod -l newname oldname  # নাম পরিবর্তন
```

### গ্রুপ ম্যানেজমেন্ট:
```bash
sudo groupadd groupname      # গ্রুপ তৈরি
sudo groupdel groupname      # গ্রুপ ডিলিট
```

### ইউজার চেক:
```bash
whoami                       # বর্তমান ইউজ
id username                  # UID, GID, গ্রুপ
```

---

## ৫. ইউজার পারমিশন (chmod, chown)

### পারমিশনের প্রকার:

| পারমিশন | প্রতীক | সংখ্যা | ফাইলে | ডিরেক্টরিতে |
|---------|--------|--------|--------|--------------|
| Read | r | 4 | পড়া | তালিকা দেখা |
| Write | w | 2 | এডিট | তৈরি/ডিলিট |
| Execute | x | 1 | চালানো | প্রবেশ |

### তিন ধরনের ব্যক্তি:
- **u (User)** — মালিক
- **g (Group)** — গ্রুপ মেম্বার
- **o (Others)** — বাকি সবাই

### পারমিশন পড়া:
```
-rwxr-xr-- 1 rahim developers 1024 file.txt
│ └─┬─┘ └─┬─┘ └─┬─┘
│ owner  group others
ফাইল
```

### chmod (Numeric):
```bash
chmod 755 file.txt   # rwxr-xr-x (স্ক্রিপ্টের জন্য)
chmod 644 file.txt   # rw-r--r-- (সাধারণ ফাইল)
chmod 600 file.txt   # rw------- (প্রাইভেট ফাইল, SSH key)
chmod -R 755 folder/ # রিকার্সিভ
```

### chmod (Symbolic):
```bash
chmod u+x script.sh  # মালিকের execute
chmod g-w file.txt   # গ্রুপের write বাদ
chmod o=r file.txt   # অন্যদের শুধু read
```

### chown (মালিকানা):
```bash
sudo chown rahim file.txt              # মালিক বদল
sudo chown rahim:developers file.txt   # মালিক+গ্রুপ
sudo chgrp developers file.txt         # শুধু গ্রুপ
sudo chown -R rahim /path/             # রিকার্সিভ
```

---

## ৬. একজন ইউজারকে sudo প্রিভিলেজ দেওয়া

### পদ্ধতি ১ — গ্রুপে যোগ (সহজ):
```bash
sudo usermod -aG sudo rahim      # Ubuntu/Debian
sudo usermod -aG wheel rahim     # RHEL/CentOS
```
⚠️ `-aG` ব্যবহার করুন — শুধু `-G` দিলে আগের সব সেকেন্ডারি গ্রুপ মুছে যায়!

### পদ্ধতি ২ — sudoers ফাইল:
```bash
sudo visudo          # সরাসরি এডিট করবেন না, visudo ব্যবহার করুন
```
```
rahim ALL=(ALL:ALL) ALL
```
| অংশ | মানে |
|------|------|
| `rahim` | ইউজারনেম |
| `ALL=` | যেকোনো হোস্ট |
| `(ALL:ALL)` | যেকোনো ইউজার/গ্রুপ হিসেবে |
| `ALL` | যেকোনো কমান্ড |

### সীমিত অনুমতি:
```
rahim ALL=(ALL) NOPASSWD: /usr/bin/apt update
```

### যাচাই ও বাতিল:
```bash
sudo -l -U rahim           # অনুমতি দেখা
sudo deluser rahim sudo    # বাতিল করা
```

---

## ৭. /etc/passwd ফাইলের ব্যাখ্যা

```
rahim:x:1001:1001:Rahim Uddin:/home/rahim:/bin/bash
  1   2   3    4        5           6          7
```

| ফিল্ড | মান | ব্যাখ্যা |
|------|-----|---------|
| ১ | `rahim` | ইউজারনেম |
| ২ | `x` | পাসওয়ার্ড `/etc/shadow`-এ আছে |
| ৩ | `1001` | UID (রুট=0, সিস্টেম=1–999) |
| ৪ | `1001` | Primary গ্রুপের GID |
| ৫ | `Rahim Uddin` | GECOS (পুরো নাম/তথ্য) |
| ৬ | `/home/rahim` | হোম ডিরেক্টরি |
| ৭ | `/bin/bash` | শেল (`nologin` = লগইন বন্ধ) |

---

## ৮. পাসওয়ার্ড এজিং ও chage কমান্ড

### /etc/shadow এর গঠন:
```
rahim:$6$K8x...:19700:5:90:7:14:19800:-
  1       2      3   4  5  6  7    8   9
```

| ফিল্ড | মানে |
|------|------|
| ১ | ইউজারনেম |
| ২ | এনক্রিপ্টেড পাসওয়ার্ড (`$6$`=SHA-512, `!`=লক, `*`=লগইন অসম্ভব) |
| ৩ | শেষ পরিবর্তন (১৯৭০ থেকে দিন) |
| ৪ | Min days — কমপক্ষে কতদিন পর বদলানো যাবে |
| ৫ | Max days — কতদিনের মধ্যে বদলাতেই হবে |
| ৬ | Warn days — আগে সতর্কবার্তা |
| ৭ | Inactive — মেয়াদ শেষে লগইনের সুযোগ |
| ৮ | Expire — অ্যাকাউন্ট বন্ধের তারিখ |
| ৯ | সংরক্ষিত |

### chage কমান্ড:
```bash
sudo chage -l rahim              # সেটিংস দেখা
sudo chage -m 5 rahim            # Min days
sudo chage -M 90 rahim           # Max days
sudo chage -W 7 rahim            # সতর্কবার্তা
sudo chage -I 14 rahim           # Inactive
sudo chage -E 2024-12-31 rahim   # অ্যাকাউন্ট এক্সপায়ারি
sudo chage -E -1 rahim           # এক্সপায়ারি বাতিল
sudo chage -d 0 rahim            # প্রথম লগইনেই পাসওয়ার্ড বদলাতে বাধ্য
sudo chage -m 5 -M 90 -W 7 -I 14 rahim   # একসাথে সব
```

---

## ৯. su বনাম sudo

| বিষয় | `su` | `sudo` |
|------|------|--------|
| পাসওয়ার্ড | রুটের | নিজের |
| সুযোগ | পুরো সেশন | একটি কমান্ড |
| লগ | ট্রেস কঠিন | auth.log-এ রেকর্ড |
| নিরাপত্তা | কম | বেশি (অডিট+সীমিত) |

### su ব্যবহার:
```bash
su -          # রুট সেশন (সম্পূর্ণ পরিবেশসহ)
su - rahim    # রহিম হওয়া
```
⚠️ সবসময় `su -` (desh সহ) ব্যবহার করুন।

### sudo-এর দরকারি অপশন:
```bash
sudo -i            # রুটের সম্পূর্ণ লগইন শেল
sudo -s            # রুট শেল
sudo -u rahim ls   # রহিম হিসেবে চালানো
sudo !!            # শেষ কমান্ড sudo সহ
sudo -k            # টাইমস্ট্যাম্প রিসেট
```

📌 sudo পাসওয়ার্ড মনে রাখে ডিফল্ট **১৫ মিনিট**।

**শ্রেষ্ঠ অভ্যাস:** রুট লগইন বন্ধ রাখুন (`sudo passwd -l root`), সবাইকে sudo দিন।

---

## ১০. বিশেষ পারমিশন বিট

```
-rwsr-sr-t
   │   │  └─ t = Sticky Bit
   │   └──── s = SGID
   └──────── s = SUID
```

### 🔴 SUID (4)
চালালে **মালিকের (রুটের)** ক্ষমতা পায়।
```bash
ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root ... /usr/bin/passwd
```
→ সাধারণ ইউজার `passwd` চালিয়ে `/etc/shadow` আপডেট করতে পারে।

```bash
chmod u+s file    # বা chmod 4755 file
find / -perm -4000 -type f 2>/dev/null   # SUID ফাইল খোঁজা (অডিট)
```

### 🟡 SGID (2)
- **ফাইলে:** চালালে গ্রুপের ক্ষমতা
- **ডিরেক্টরিতে:** ভেতরের নতুন ফাইল অটোমেটিক সেই গ্রুপের হয় (টিম ফোল্ডারে দারুণ কাজে লাগে)

```bash
sudo groupadd developers
sudo mkdir /home/project
sudo chgrp developers /home/project
sudo chmod 2775 /home/project    # drwxrwsr-x
```

### 🔵 Sticky Bit (1)
ডিরেক্টরিতে থাকলে **শুধু নিজের ফাইল ডিলিট করা যায়**।
```bash
ls -ld /tmp
drwxrwxrwt 15 root root ... /tmp

chmod +t folder/   # বা chmod 1777
```

### সারসংক্ষেপ:

| বিট | ফাইলে | ডিরেক্টরিতে | সংখ্যা | উদাহরণ |
|-----|--------|-------------|--------|---------|
| SUID | মালিকের ক্ষমতা | অর্থহীন | 4 | `/usr/bin/passwd` |
| SGID | গ্রুপের ক্ষমতা | গ্রুপ-উত্তরাধিকার | 2 | টিম ফোল্ডার |
| Sticky | অপ্রচলিত | নিজের ফাইলেই ডিলিট | 1 | `/tmp` |

### বড় হাতের বনাম ছোট হাতের অক্ষর:
- `rws` (ছোট s) → বিশেষ বিট **কার্যকর** ✅
- `rwS` (বড় S) → execute নেই, বিট **অকার্যকর** ⚠️
- `rwt` / `rwT` → Sticky-তেও একই নিয়ম

```bash
chmod 4754 file   # -rwsr-xr-- ✅
chmod 4644 file   # -rwSr--r-- ❌ (কাজ করবে না)
```

---

## 📝 এক নজরে পুরো বিষয়:

```
ইউজার তৈরি   → useradd, passwd, userdel
গ্রুপ         → groupadd, groupdel, usermod -aG
তথ্য ফাইল     → /etc/passwd, /etc/shadow, /etc/group
পারমিশন      → chmod (755, 644), chown
অ্যাডমিন      → sudo, visudo, usermod -aG sudo
নিরাপত্তা     → chage, passwd -l, find -perm -4000
বিশেষ বিট    → SUID(4), SGID(2), Sticky(1)
```

---

আরও জানতে চাইলে:
- **UMask** — ডিফল্ট পারমিশন কীভাবে নির্ধারিত হয়
- **ACL** — উন্নত পারমিশন ব্যবস্থা
- **সিস্টেম অডিট** — বিপজ্জনক SUID ফাইল খুঁজে বের করা