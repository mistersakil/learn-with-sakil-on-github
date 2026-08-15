> [🏠](../) [⬅️ ০৫। লিনাক্স থিম কাস্টমাইজেশন](../০৫-লিনাক্স-থিম-কাস্টমাইজেশন) [➡️ ০৭। কমান্ড লাইন অপশন](../০৭-কমান্ড-লাইন-অপশন)

# 📦 ০৬। থার্ড-পার্টি প্যাকেজ ইনস্টলেশন আর্কিটেকচার

> Ubuntu ও Debian-ভিত্তিক Linux সিস্টেমে Third-Party Software কীভাবে নিরাপদে ইনস্টল, যাচাই, আপডেট এবং পরিচালনা করা হয় তার পূর্ণাঙ্গ গাইড।

---

## 📚 সূচিপত্র

- থার্ড-পার্টি প্যাকেজ কী?
- Linux Package Management Architecture
- APT কীভাবে কাজ করে?
- Repository কী?
- GPG Key ও Digital Signature
- Generic 4-Step Installation Workflow
- apt update এর Backend Workflow
- apt install এর Backend Workflow
- `.deb` Package Anatomy
- Microsoft Edge Practical Example
- VS Code Practical Example
- Docker Practical Example
- Snap vs Flatpak vs AppImage vs APT
- Security Best Practices
- Troubleshooting Guide
- Real World Examples
- Key Takeaways

---

# 🎯 থার্ড-পার্টি প্যাকেজ কী?

Ubuntu Repository-এর বাইরে থাকা যেকোনো Software Package-কে Third-Party Package বলা হয়।

উদাহরণ:

- Google Chrome
- Microsoft Edge
- Visual Studio Code
- Docker
- GitHub CLI
- Node.js
- Postman
- Brave Browser

Ubuntu-এর Official Repository-তে এগুলো সাধারণত থাকে না।

তাই Software Vendor নিজস্ব Repository প্রদান করে।

---

# 🏗️ Linux Package Management Architecture

Linux-এ Software Installation সাধারণত নিচের Architecture অনুসরণ করে:

```text
Developer Company
       │
       ▼
Official Repository
       │
       ▼
GPG Signature
       │
       ▼
APT Sources
       │
       ▼
apt update
       │
       ▼
Package Metadata
       │
       ▼
apt install
       │
       ▼
Installed Software
```

---

# 📦 APT কী?

APT এর পূর্ণরূপ:

```text
Advanced Package Tool
```

এটি Ubuntu ও Debian-এর Package Manager।

---

## APT-এর কাজ

APT মূলত ৫টি কাজ করে:

### ১। Package খুঁজে বের করা

```bash
apt search nginx
```

---

### ২। Package Install করা

```bash
sudo apt install nginx
```

---

### ৩। Package Update করা

```bash
sudo apt upgrade
```

---

### ৪। Package Remove করা

```bash
sudo apt remove nginx
```

---

### ৫। Dependency Resolve করা

যদি nginx-এর জন্য:

```text
libssl
libpcre
zlib
```

প্রয়োজন হয়,

APT স্বয়ংক্রিয়ভাবে সেগুলোও Install করে।

---

# 📂 Repository কী?

Repository হলো Software Storage Server।

এখান থেকে Linux Package Download করে।

---

## Repository Structure

```text
repository/
│
├── dists/
│
├── pool/
│
├── Release
│
├── InRelease
│
└── Release.gpg
```

---

## dists/

Metadata থাকে।

উদাহরণ:

```text
jammy
noble
bookworm
stable
testing
```

---

## pool/

Actual Package File থাকে।

```text
chrome.deb
edge.deb
docker.deb
```

---

## Release

Repository-এর Summary File।

---

## Release.gpg

Digital Signature File।

---

# 🔐 GPG Key কী?

GPG মানে:

```text
GNU Privacy Guard
```

এটি Package Authenticity Verify করার জন্য ব্যবহৃত হয়।

---

## কেন GPG Key প্রয়োজন?

ধরুন:

```text
Your PC
   │
Internet
   │
Repository
```

যদি মাঝপথে Attacker Package Modify করে?

Linux সেটি Detect করতে পারে GPG Signature-এর মাধ্যমে।

---

## Verification Process

```text
Package
   │
Signature
   │
Public Key
   │
Verification
   │
Install
```

---

## ASCII Armored Key

অনেক Key থাকে:

```text
microsoft.asc
docker.asc
```

এগুলো Text Format।

---

## gpg --dearmor

Linux Binary Keyring ব্যবহার করে।

```bash
gpg --dearmor
```

Text Key কে Binary Key-তে রূপান্তর করে।

---

# 🔄 Generic 4-Step Installation Workflow

Linux-এর প্রায় সব Third-Party Package Installation একই Pattern অনুসরণ করে।

---

# 🛠️ Step 1 — Install Dependencies

```bash
sudo apt update
sudo apt install wget curl gpg software-properties-common -y
```

---

## কেন?

কারণ:

- wget
- curl
- gpg

এগুলো Repository Setup-এর জন্য প্রয়োজন।

---

# 🔑 Step 2 — Import GPG Key

```bash
wget -qO- KEY_URL \
| sudo gpg --dearmor \
-o /usr/share/keyrings/package.gpg
```

---

## Backend-এ কী হয়?

```text
Download Key
      │
      ▼
Convert to Binary
      │
      ▼
Store in Keyring
```

---

# 🗂️ Step 3 — Add Repository

```bash
echo "deb [signed-by=/usr/share/keyrings/package.gpg] REPO_URL stable main" \
| sudo tee /etc/apt/sources.list.d/package.list
```

---

## Backend-এ কী হয়?

নতুন Source File তৈরি হয়:

```text
/etc/apt/sources.list.d/
```

---

উদাহরণ:

```text
docker.list
edge.list
vscode.list
```

---

# 🚀 Step 4 — Refresh & Install

```bash
sudo apt update
sudo apt install package-name
```

---

# 🔍 apt update কী করে?

অনেকে মনে করে:

```bash
apt update
```

Software Update করে।

আসলে তা নয়।

---

এটি:

```text
Repository Metadata Refresh
```

করে।

---

## Workflow

```text
Read Sources
      │
      ▼
Connect Repository
      │
      ▼
Download Metadata
      │
      ▼
Verify Signature
      │
      ▼
Update Local Cache
```

---

## Cache Location

```bash
/var/lib/apt/lists/
```

---

# 🔍 apt install কী করে?

```bash
sudo apt install nginx
```

দিলে:

---

```text
Find Package
      │
Resolve Dependency
      │
Download Package
      │
Verify Signature
      │
Extract Files
      │
Register Package
      │
Finish
```

---

## Download Location

```bash
/var/cache/apt/archives/
```

---

# 📦 .deb Package Anatomy

Ubuntu Package-এর Extension:

```text
.deb
```

---

একটি .deb Package-এর ভিতরে থাকে:

```text
control.tar.gz
data.tar.gz
debian-binary
```

---

## control.tar.gz

Package Metadata:

```text
Name
Version
Dependencies
Maintainer
```

---

## data.tar.gz

Actual Software Files।

---

# 🌐 Practical Example — Microsoft Edge

## Install Dependencies

```bash
sudo apt update
sudo apt install wget curl gpg -y
```

## Add GPG Key

```bash
curl https://packages.microsoft.com/keys/microsoft.asc \
| gpg --dearmor \
| sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

এই কমান্ডটি লিনেক্সে কোনো নিরাপদ ও অফিশিয়াল সোর্স (যেমন: Microsoft) থেকে সফটওয়্যার ইনস্টল করার জন্য তাদের সিকিউরিটি কি (Security Key/GPG Key) সিস্টেমে যুক্ত করার একটি স্ট্যান্ডার্ড পদ্ধতি।
সহজ ভাষায়, এখানে | (পাইপ) চিহ্নের মাধ্যমে ৩টি আলাদা কমান্ডকে একসাথে জোড়া দেওয়া হয়েছে। প্রথম কমান্ডের আউটপুট দ্বিতীয়টিতে এবং দ্বিতীয়টির আউটপুট তৃতীয়টিতে পাঠানো হয়েছে।
নিচে প্রতিটি অংশের স্টেপ-বাই-স্টেপ ব্যাখ্যা দেওয়া হলো:

### স্টেপ ১: curl https://packages.microsoft.com/keys/microsoft.asc

* curl হলো টার্মিনালের একটি টুল যা ইন্টারনেট থেকে ডেটা বা ফাইল ডাউনলোড করতে ব্যবহৃত হয়।
* এখানে এটি মাইক্রোসফটের অফিশিয়াল ওয়েবসাইট থেকে তাদের সিকিউরিটি কি ফাইলটি (microsoft.asc) ডাউনলোড করছে।
* এই ফাইলটি সাধারণত মানুষের পড়ার উপযোগী টেক্সট ফরম্যাটে (ASCII-armored) থাকে। [1, 2, 3, 4] 

### স্টেপ ২: gpg --dearmor

* | (Pipe) চিহ্নের কাজ হলো প্রথম স্টেপের ডাউনলোড করা ডেটাগুলোকে সরাসরি এই দ্বিতীয় স্টেপের কাছে পাঠিয়ে দেওয়া।
* gpg (GNU Privacy Guard) হলো লিনেক্সের ক্রিপ্টোগ্রাফি বা সিকিউরিটি টুল।
* --dearmor অপশনটি প্রথম স্টেপ থেকে আসা মানুষের পড়ার উপযোগী টেক্সট ফাইলটিকে (ASCII) রূপান্তর করে কম্পিউটারের পড়ার উপযোগী বাইনারি ফরম্যাটে (Binary format) নিয়ে যায়। লিনেক্স সিস্টেম সিকিউরিটি কি প্রসেস করার জন্য এই বাইনারি ফরম্যাটটিই পছন্দ করে। [5, 6, 7] 

### স্টেপ ৩: sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null

* আবার | (Pipe) চিহ্নের মাধ্যমে স্টেপ ২-এর তৈরি হওয়া বাইনারি ডেটাগুলোকে এই শেষ স্টেপে পাঠানো হয়।
* sudo ব্যবহার করা হয়েছে কারণ আমরা একটি সিস্টেম ডিরেক্টরিতে ফাইলটি সেভ করছি, যার জন্য অ্যাডমিনিস্ট্রেটর বা রুট (Root) পারমিশন প্রয়োজন।
* tee কমান্ডটি একটি বিশেষ টুল যা একসাথে দুটি কাজ করে: এটি ইনপুট হিসেবে পাওয়া ডেটাগুলোকে নির্দিষ্ট ফাইলে সেভ করে এবং একই সাথে স্ক্রিনেও প্রিন্ট করে দেখায়। এখানে এটি ডেটাগুলোকে /usr/share/keyrings/microsoft-edge.gpg নামের নতুন একটি ফাইলে সেভ করছে। [8] 
* > /dev/null অংশটি ব্যবহার করা হয়েছে স্ক্রিনের আউটপুট বন্ধ করার জন্য। যেহেতু tee কমান্ড ফাইল সেভ করার পাশাপাশি স্ক্রিনেও হাবিজাবি বাইনারি লেখা দেখাতো, তাই > /dev/null দিয়ে সেই স্ক্রিন আউটপুটকে লিনেক্সের একটি "ডিজিটাল ডাস্টবিন"-এ পাঠিয়ে গায়েব করে দেওয়া হয়েছে। ফলে টার্মিনাল একদম পরিষ্কার বা ক্লিন থাকে।


#### সংক্ষেপে পুরো লাইনের কাজ:
ইন্টারনেট থেকে মাইক্রোসফটের সিকিউরিটি কি ডাউনলোড করা হলো (curl), সেটাকে কম্পিউটারের বোঝার সুবিধার্থে বাইনারিতে রূপান্তর করা হলো (gpg --dearmor), এবং সবশেষে রুট পারমিশন নিয়ে স্ক্রিনে কোনো হিজিবিজি লেখা না দেখিয়ে সরাসরি লিনেক্সের নিরাপদ কি-রিং ফোল্ডারে ফাইলটি সেভ করা হলো (sudo tee ... > /dev/null)। [9] 
এই কি-টি যুক্ত করার পর আপনার লিনেক্স সিস্টেম নিশ্চিত হতে পারে যে, এরপর আপনি যখনই Microsoft Edge ব্রাউজার ইনস্টল বা আপডেট করবেন, সেটি আসল মাইক্রোসফটের কাছ থেকেই আসছে, কোনো হ্যাকারের কাছ থেকে নয়।

## Add Repository

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main" \
| sudo tee /etc/apt/sources.list.d/microsoft-edge.list
```

## Install Edge

```bash
sudo apt update
sudo apt install microsoft-edge-stable -y
```

# 💻 Practical Example — VS Code

```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc \
| gpg --dearmor \
| sudo tee /usr/share/keyrings/packages.microsoft.gpg > /dev/null
```

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" \
| sudo tee /etc/apt/sources.list.d/vscode.list
```

```bash
sudo apt update
sudo apt install code -y
```

---

# 🐳 Practical Example — Docker

Docker Official Repository ব্যবহার করে Install করা উচিত।

কারণ Ubuntu Repository-এর Version অনেক সময় Outdated থাকে।

---

# 📱 Snap vs Flatpak vs AppImage vs APT

| Feature | APT | Snap | Flatpak | AppImage |
|----------|----------|----------|----------|----------|
| System Integration | ✅ | ✅ | ✅ | ⚠️ |
| Auto Update | ✅ | ✅ | ✅ | ❌ |
| Isolation | ❌ | ✅ | ✅ | ❌ |
| Speed | ✅ | ⚠️ | ✅ | ✅ |
| Portable | ❌ | ❌ | ❌ | ✅ |

---

# 🛡️ Security Best Practices

## ✅ Official Documentation Follow করুন

ভালো:

```text
docs.docker.com
```

---

খারাপ:

```text
random-blog.xyz
```

---

## ✅ HTTPS ব্যবহার করুন

সবসময়:

```text
https://
```

---

## ✅ GPG Key Verify করুন

```bash
gpg --show-keys file.gpg
```

---

## ✅ Unused Repository Remove করুন

বর্তমান Repository List:

```bash
ls /etc/apt/sources.list.d/
```

---

# 🚨 Troubleshooting Guide

## NO_PUBKEY Error

```text
NO_PUBKEY XXXXX
```

সমাধান:

```bash
Re-import GPG Key
```

---

## 404 Not Found

```text
Repository Not Found
```

সমাধান:

Repository URL Verify করুন।

---

## Broken Packages

```bash
sudo apt --fix-broken install
```

---

## Dependency Error

```bash
sudo apt install -f
```

---

## Package Lock Error

```text
Could not get lock
```

সমাধান:

অন্য Package Manager Process বন্ধ করুন।

---

# 🌍 Real World Examples

| Software | Repository Required |
|-----------|-----------|
| Chrome | ✅ |
| Edge | ✅ |
| Docker | ✅ |
| VS Code | ✅ |
| GitHub CLI | ✅ |
| Node.js | ✅ |
| Brave | ✅ |
| Postman | Sometimes |

---

# 🧠 Key Takeaways

- Linux Software Installation-এর কেন্দ্রবিন্দু হলো APT।
- Third-Party Package Install করতে Repository যোগ করতে হয়।
- GPG Key Package Authenticity নিশ্চিত করে।
- `apt update` Package Database Refresh করে।
- `apt install` Package Install করে।
- `.deb` হলো Debian Package Format।
- Official Repository ব্যবহার করা সবচেয়ে নিরাপদ।
- Repository যোগ করলে ভবিষ্যতের Update স্বয়ংক্রিয়ভাবে পাওয়া যায়।

---

> [🏠](../) [⬅️ ০৫। লিনাক্স থিম কাস্টমাইজেশন](../০৫-লিনাক্স-থিম-কাস্টমাইজেশন) [➡️ ০৭। কমান্ড লাইন অপশন](../০৭-কমান্ড-লাইন-অপশন)