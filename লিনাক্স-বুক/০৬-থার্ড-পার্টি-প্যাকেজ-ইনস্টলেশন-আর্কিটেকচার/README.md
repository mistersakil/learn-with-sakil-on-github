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

---

## Add GPG Key

```bash
curl https://packages.microsoft.com/keys/microsoft.asc \
| gpg --dearmor \
| sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

---

## Add Repository

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main" \
| sudo tee /etc/apt/sources.list.d/microsoft-edge.list
```

---

## Install Edge

```bash
sudo apt update
sudo apt install microsoft-edge-stable -y
```

---

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