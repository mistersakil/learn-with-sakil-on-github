# ০৯। APT বনাম Wget — লিনাক্সে সফটওয়্যার ইন্সটলেশনের প্রকৃত প্রক্রিয়া

লিনাক্স শেখার সময় নতুন ব্যবহারকারীরা প্রায়ই **APT** এবং **Wget**-কে একই ধরনের টুল মনে করে। কারণ অনেক টিউটোরিয়ালে দেখা যায়:

```bash
wget https://example.com/software.deb
sudo apt install ./software.deb
```

ফলে মনে হয় দুটোই সফটওয়্যার ইন্সটল করার জন্য ব্যবহৃত হচ্ছে।

বাস্তবে **APT** এবং **Wget** সম্পূর্ণ ভিন্ন স্তরে কাজ করে।

---

## একটি বাস্তব উদাহরণ

ধরো তুমি Google Chrome ইন্সটল করতে চাও।

তুমি প্রথমে Chrome-এর `.deb` ফাইল ডাউনলোড করবে:

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

এরপর সেটি ইন্সটল করবে:

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

এখানে:

* `wget` → ফাইল নামিয়েছে
* `apt` → সফটওয়্যার ইন্সটল করেছে

অর্থাৎ:

```text
Internet
    │
    ▼
 Wget
    │
    ▼
 .deb File
    │
    ▼
  APT
    │
    ▼
 Installed Software
```

---

## ১। APT - Advanced Package Tool কী?

APT হলো Debian এবং Ubuntu পরিবারের Linux Distribution-এর Package Management System। এটি Windows-এর Microsoft Store বা Android-এর Play Store-এর মতো কাজ করে।

### APT-এর কাজ:

* সফটওয়্যার ইন্সটল করা
* সফটওয়্যার আপডেট করা
* সফটওয়্যার রিমুভ করা
* Dependency ম্যানেজ করা
* Repository থেকে Package ডাউনলোড করা

### APT কোথা থেকে সফটওয়্যার আনে?

APT সাধারণত Repository থেকে সফটওয়্যার সংগ্রহ করে।

Repository হলো সফটওয়্যার সার্ভার। লিনাক্সের অফিশিয়াল রিপোজিটরি মূলত কিছু দূরবর্তী ক্লাউড সার্ভার (Remote Web Servers), যেখানে হাজার হাজার সফটওয়্যার প্যাকেজ আকারে জমা থাকে। লিনাক্স ডিস্ট্রিবিউশনগুলো (যেমন: Ubuntu, Debian) তাদের নিজস্ব অফিশিয়াল ডেটা সেন্টারে এই সার্ভারগুলো পরিচালনা করে। 

>আপনার কম্পিউটারের APT কীভাবে এই সার্ভারগুলোর ঠিকানা বোঝে এবং কাজ করে, তা নিচে ৩টি ধাপে ব্যাখ্যা করা হলো:

### সার্ভারের ঠিকানা কোথায় থাকে?
আপনার লিনাক্স সিস্টেমে নির্দিষ্ট কিছু কনফিগারেশন ফাইল থাকে, যেখানে এই অফিশিয়াল সার্ভারগুলোর লিংক (URL) আগে থেকেই লিখে দেওয়া থাকে। 

* প্রধান ফাইল: `cat /etc/apt/sources.list.d/ubuntu.sources`
* সহায়ক ডিরেক্টরি: `ls /etc/apt/sources.list.d/` (এখানে থার্ড-পার্টি বা বাইরের রিপোজিটরিগুলোর লিংক থাকে)।

আপনি যদি টার্মিনালে cat /etc/apt/sources.list.d/ubuntu.sources কমান্ডটি দেন, তবে দেখতে পাবেন সেখানে

```sources
Types: deb
URIs: http://bd.archive.ubuntu.com/ubuntu/
Suites: resolute resolute-updates resolute-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: http://security.ubuntu.com/ubuntu/
Suites: resolute-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

```


### APT কীভাবে কাজ করে (ধাপসমূহ)?
যখন আপনি আপনার কম্পিউটারে APT ব্যবহার করেন, তখন এটি মূলত ৩টি ধাপে রিপোজিটরি চিনে কাজ সম্পন্ন করে:

***ধাপ ১: আপডেট এবং ইনডেক্স ডাউনলোড***
`sudo apt update` আপনি যখন এই কমান্ডটি দেন, APT তার sources.list ফাইলে থাকা সার্ভার লিংকগুলোতে নক করে। সার্ভার থেকে সে সমস্ত সফটওয়্যারের একটি লেটেস্ট তালিকা (Index File) ডাউনলোড করে আপনার নিজের কম্পিউটারে (/var/lib/apt/lists/) সেভ করে রাখে। এই তালিকায় লেখা থাকে কোন সফটওয়্যারের কোন ভার্সন সার্ভারে আছে। 

***ধাপ ২: লোকাল খোঁজা*** 
`sudo apt install <software>` এখন আপনি কোনো সফটওয়্যার ইন্সটল করতে দিলে APT কিন্তু সরাসরি ইন্টারনেটে খোঁজে না। সে আপনার কম্পিউটারে মাত্র ডাউনলোড হওয়া ওই ইনডেক্স তালিকার মধ্যে সফটওয়্যারটি খোঁজে। যদি তালিকায় নামটি পাওয়া যায়, তবে সেখান থেকে সেটির নির্দিষ্ট ডাউনলোড লিংকটি (URL) বের করে নেয়। 

***ধাপ ৩: নিরাপত্তা যাচাই (Security & GPG Keys)***
APT যেকোনো সার্ভার থেকে অন্ধের মতো ফাইল ডাউনলোড করে না। প্রত্যেকটি অফিশিয়াল রিপোজিটরির একটি ডিজিটাল সিকিউরিটি চাবি বা GPG Key আপনার সিস্টেমে আগে থেকেই সংরক্ষিত থাকে। ডাউনলোড করার সময় APT মিলিয়ে দেখে যে সার্ভারের ফাইলটি আসল এবং নিরাপদ কি না। চাবি মিলে গেলেই কেবল সে ফাইলটি নামিয়ে ইন্সটল করে।

***সহজ কথায়:*** 
আপনার কম্পিউটার স্ক্রিনে কোনো দৃশ্যমান অ্যাপ স্টোর না থাকলেও, ব্যাকগ্রাউন্ডে টেক্সট ফাইলের ভেতরে থাকা অফিশিয়াল সার্ভার লিংকের মাধ্যমেই APT পুরো ইন্টারনেট দুনিয়া থেকে সঠিক সফটওয়্যারটি খুঁজে বের করে নেয়। 

### APT-এর সবচেয়ে ব্যবহৃত কমান্ড

#### Package List Update

```bash
sudo apt update
```

Repository-তে নতুন কোন সফটওয়্যার এসেছে তা জানে।

#### Installed Package Upgrade

```bash
sudo apt upgrade
```

সিস্টেমে থাকা সফটওয়্যার আপডেট করে।

#### Software Install

```bash
sudo apt install vlc
```

VLC Player ইন্সটল করে।

#### Package Remove

```bash
sudo apt remove vlc
```

সফটওয়্যার সরিয়ে দেয়।

#### Package Search

```bash
apt search docker
```

Repository-তে Docker আছে কিনা খুঁজে।

#### Package Information

```bash
apt show docker.io
```

Package-এর বিস্তারিত তথ্য দেখায়।

### APT-এর সবচেয়ে বড় শক্তি: Dependency Resolution

ধরো VLC চালানোর জন্য আরও ২০টি লাইব্রেরি দরকার।

APT স্বয়ংক্রিয়ভাবে:

```text
VLC
 ├─ libA
 ├─ libB
 ├─ libC
 └─ libD
```

সবকিছু একসাথে ইন্সটল করে।

তোমাকে আলাদা করে কিছু করতে হয় না।

---

## ২। Wget (World Wide Web Get)

### Wget কী?

Wget হলো Command-Line Download Manager। এটি শুধুমাত্র ইন্টারনেট থেকে ফাইল ডাউনলোড করে।

### এর কাজ:

* ফাইল ডাউনলোড
* ওয়েবসাইট মিরর করা
* ব্যাকগ্রাউন্ড ডাউনলোড
* Resume Download
* Recursive Download

### Wget কোথা থেকে ফাইল আনে?

```text
Internet
    │
    ▼
   URL
    │
    ▼
  Wget
    │
    ▼
 Local File
```

APT-এর মতো Repository-এর উপর নির্ভরশীল নয়। যে URL দেবে সেখান থেকেই ফাইল নামাবে।

### সাধারণ Download

```bash
wget https://example.com/file.zip
```

### অন্য নামে Save

```bash
wget -O chrome.deb https://example.com/file.deb
```

### Download Resume

```bash
wget -c https://example.com/bigfile.iso
```

### Background Download

```bash
wget -b https://example.com/file.iso
```

### Entire Website Download

```bash
wget --mirror https://example.com
```

### Wget-এর সীমাবদ্ধতা

ধরো:

```bash
wget https://example.com/vlc.deb
```

এখন তোমার কাছে শুধুমাত্র একটি ফাইল আছে।

```text
vlc.deb
```

এটি এখনও ইন্সটল হয়নি।

এখনও তোমাকে:

```bash
sudo apt install ./vlc.deb
```

অথবা

```bash
sudo dpkg -i vlc.deb
```

ব্যবহার করতে হবে।

---

## APT এবং Wget-এর মধ্যে মৌলিক পার্থক্য

| বিষয়                | APT                   | Wget               |
| -------------------- | --------------------- | ------------------ |
| পূর্ণ নাম            | Advanced Package Tool | World Wide Web Get |
| কাজ                  | Package Management    | File Download      |
| উৎস                  | Repository            | URL                |
| Dependency বোঝে      | ✅ হ্যাঁ               | ❌ না               |
| Software Install করে | ✅ হ্যাঁ               | ❌ না               |
| Update করতে পারে     | ✅ হ্যাঁ               | ❌ না               |
| Remove করতে পারে     | ✅ হ্যাঁ               | ❌ না               |
| যেকোনো ফাইল Download | ❌ সীমিত               | ✅ হ্যাঁ            |
| Website Mirror       | ❌ না                  | ✅ হ্যাঁ            |

---

## Linux Package Installation Architecture

তৃতীয়-পক্ষ সফটওয়্যার ইন্সটল করার সময় সাধারণত নিচের ধাপগুলো ঘটে:

```text
Step 1
Internet
    │
    ▼
  Wget

Step 2
Download Package
(.deb)

Step 3
APT / DPKG

Step 4
Dependency Resolution

Step 5
Installed Software
```

বাস্তব উদাহরণ:

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo apt install ./google-chrome-stable_current_amd64.deb
```

এখানে:

```text
wget  → Download
apt   → Install
```

---

## APT, DPKG এবং Wget-এর সম্পর্ক

নতুন Linux ব্যবহারকারীদের জন্য এটি বোঝা গুরুত্বপূর্ণ:

```text
Wget
  │
  └── Downloads File

DPKG
  │
  └── Installs .deb Package

APT
  │
  ├── Uses DPKG
  ├── Resolves Dependencies
  └── Manages Repositories
```

অর্থাৎ:

```text
APT
 └── DPKG
```

APT নিজেও ভিতরে DPKG ব্যবহার করে।

---

## সহজে মনে রাখার কৌশল

### Wget

ভাবো:

> "আমি শুধু ফাইল নামাই"

```bash
wget file
```

### DPKG

ভাবো:

> "আমি .deb প্যাকেজ ইন্সটল করি"

```bash
dpkg -i file.deb
```

### APT

ভাবো:

> "আমি পুরো সফটওয়্যার ম্যানেজ করি"

```bash
apt install package
```

---

# সারসংক্ষেপ

* **Wget** ইন্টারনেট থেকে ফাইল ডাউনলোড করে।
* **DPKG** `.deb` প্যাকেজ ইন্সটল করে।
* **APT** সফটওয়্যার ইন্সটল, আপডেট, রিমুভ এবং Dependency ম্যানেজ করে।
* APT হলো Package Manager, Wget হলো Downloader।
* Ubuntu-তে Third-Party Software Installation-এর সবচেয়ে সাধারণ পদ্ধতি হলো:

```bash
wget <url>
sudo apt install ./package.deb
```

এবং এই কারণেই Linux টার্মিনালে **Wget + APT** জুটি সবচেয়ে বেশি দেখা যায়।
