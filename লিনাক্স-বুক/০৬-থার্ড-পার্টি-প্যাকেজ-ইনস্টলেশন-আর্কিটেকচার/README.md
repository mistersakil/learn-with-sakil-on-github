> [🏠](../) [⬅️ ০৫। লিনাক্স থিম কাস্টমাইজেশন](../০৫-লিনাক্স-থিম-কাস্টমাইজেশন) [➡️ ০৭। কমান্ড লাইন অপশন](../০৭-কমান্ড-লাইন-অপশন)

# ০৬। থার্ড-পার্টি প্যাকেজ ইনস্টলেশন আর্কিটেকচার

> Ubuntu ও Debian-ভিত্তিক Linux সিস্টেমে Third-Party Software কীভাবে নিরাপদে ইনস্টল, যাচাই, আপডেট এবং পরিচালনা করা হয় তার পূর্ণাঙ্গ গাইড।

---

## সূচিপত্র

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

## থার্ড-পার্টি প্যাকেজ কী?

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

## Linux Package Management Architecture

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

## APT কী?

APT এর পূর্ণরূপ: Advanced Package Tool

এটি Ubuntu ও Debian-এর Package Manager।

### APT-এর কাজ

APT মূলত ৫টি কাজ করে:

১। Package খুঁজে বের করা `apt search nginx`

২। Package Install করা `sudo apt install nginx`

৩। Package Update করা `sudo apt upgrade`

৪। Package Remove করা `sudo apt remove nginx`

৫। Dependency Resolve করা

যদি nginx-এর জন্য:

```text
libssl
libpcre
zlib
```

প্রয়োজন হয়, APT স্বয়ংক্রিয়ভাবে সেগুলোও Install করে।

---

## Repository কী?

Repository হলো Software Storage Server। এখান থেকে Linux Package Download করে।

### Repository Structure

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

#### dists/

Metadata থাকে।

উদাহরণ:

```text
jammy
noble
bookworm
stable
testing
```

#### pool/

Actual Package File থাকে।

```text
chrome.deb
edge.deb
docker.deb
```

#### Release

Repository-এর Summary File।

#### Release.gpg

Digital Signature File।

## GPG Key কী?

GPG মানে: GNU Privacy Guard

এটি Package Authenticity Verify করার জন্য ব্যবহৃত হয়।

### কেন GPG Key প্রয়োজন?

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

### Verification Process

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

### ASCII Armored Key

অনেক Key থাকে:

```text
microsoft.asc
docker.asc
```

এগুলো Text Format।

### gpg --dearmor

Linux Binary Keyring ব্যবহার করে।

```bash
gpg --dearmor
```

Text Key কে Binary Key-তে রূপান্তর করে।

---

## Generic 4-Step Installation Workflow

Linux-এর প্রায় সব Third-Party Package Installation একই Pattern অনুসরণ করে।

### Step 1 — Install Dependencies

```bash
sudo apt update
sudo apt install wget curl gpg software-properties-common -y
```

#### কেন?

কারণ:

- wget
- curl
- gpg

এগুলো Repository Setup-এর জন্য প্রয়োজন।

### Step 2 — Import GPG Key

```bash
wget -qO- KEY_URL \
| sudo gpg --dearmor \
-o /usr/share/keyrings/package.gpg
```

#### Backend-এ কী হয়?

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

### Step 3 — Add Repository

```bash
echo "deb [signed-by=/usr/share/keyrings/package.gpg] REPO_URL stable main" \
| sudo tee /etc/apt/sources.list.d/package.list
```

#### Backend-এ কী হয়?

নতুন Source File তৈরি হয়:

```text
/etc/apt/sources.list.d/
```

উদাহরণ:

```text
docker.list
edge.list
vscode.list
```

---

### Step 4 — Refresh & Install

```bash
sudo apt update
sudo apt install package-name
```

#### apt update কী করে?

অনেকে মনে করে:

```bash
apt update
```

Software Update করে। আসলে তা নয়। এটি:

```text
Repository Metadata Refresh
```

করে।

---

### Workflow

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

### Cache Location

```bash
/var/lib/apt/lists/
```

### apt install কী করে?

```bash
sudo apt install nginx
```

দিলে:

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

### Download Location

```bash
/var/cache/apt/archives/
```

---

## .deb Package Anatomy

Ubuntu Package-এর Extension:

```text
.deb
```

একটি .deb Package-এর ভিতরে থাকে:

```text
control.tar.gz
data.tar.gz
debian-binary
```

### control.tar.gz

Package Metadata:

```text
Name
Version
Dependencies
Maintainer
```

### data.tar.gz

Actual Software Files।

## Practical Example — Microsoft Edge

### Install Dependencies

```bash
sudo apt update
sudo apt install wget curl gpg -y
```

### Add GPG Key

```bash
curl https://packages.microsoft.com/keys/microsoft.asc \
| gpg --dearmor \
| sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

এই কমান্ডটি লিনেক্সে কোনো নিরাপদ ও অফিশিয়াল সোর্স (যেমন: Microsoft) থেকে সফটওয়্যার ইনস্টল করার জন্য তাদের সিকিউরিটি কি (Security Key/GPG Key) সিস্টেমে যুক্ত করার একটি স্ট্যান্ডার্ড পদ্ধতি।
সহজ ভাষায়, এখানে | (পাইপ) চিহ্নের মাধ্যমে ৩টি আলাদা কমান্ডকে একসাথে জোড়া দেওয়া হয়েছে। প্রথম কমান্ডের আউটপুট দ্বিতীয়টিতে এবং দ্বিতীয়টির আউটপুট তৃতীয়টিতে পাঠানো হয়েছে।
নিচে প্রতিটি অংশের স্টেপ-বাই-স্টেপ ব্যাখ্যা দেওয়া হলো:

#### স্টেপ ১: curl https://packages.microsoft.com/keys/microsoft.asc

* curl হলো টার্মিনালের একটি টুল যা ইন্টারনেট থেকে ডেটা বা ফাইল ডাউনলোড করতে ব্যবহৃত হয়।
* এখানে এটি মাইক্রোসফটের অফিশিয়াল ওয়েবসাইট থেকে তাদের সিকিউরিটি কি ফাইলটি (microsoft.asc) ডাউনলোড করছে।
* এই ফাইলটি সাধারণত মানুষের পড়ার উপযোগী টেক্সট ফরম্যাটে (ASCII-armored) থাকে।

#### স্টেপ ২: gpg --dearmor

* | (Pipe) চিহ্নের কাজ হলো প্রথম স্টেপের ডাউনলোড করা ডেটাগুলোকে সরাসরি এই দ্বিতীয় স্টেপের কাছে পাঠিয়ে দেওয়া।
* gpg (GNU Privacy Guard) হলো লিনেক্সের ক্রিপ্টোগ্রাফি বা সিকিউরিটি টুল।
* --dearmor অপশনটি প্রথম স্টেপ থেকে আসা মানুষের পড়ার উপযোগী টেক্সট ফাইলটিকে (ASCII) রূপান্তর করে কম্পিউটারের পড়ার উপযোগী বাইনারি ফরম্যাটে (Binary format) নিয়ে যায়। লিনেক্স সিস্টেম সিকিউরিটি কি প্রসেস করার জন্য এই বাইনারি ফরম্যাটটিই পছন্দ করে। 

#### স্টেপ ৩: sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null

* আবার | (Pipe) চিহ্নের মাধ্যমে স্টেপ ২-এর তৈরি হওয়া বাইনারি ডেটাগুলোকে এই শেষ স্টেপে পাঠানো হয়।
* sudo ব্যবহার করা হয়েছে কারণ আমরা একটি সিস্টেম ডিরেক্টরিতে ফাইলটি সেভ করছি, যার জন্য অ্যাডমিনিস্ট্রেটর বা রুট (Root) পারমিশন প্রয়োজন।
* tee কমান্ডটি একটি বিশেষ টুল যা একসাথে দুটি কাজ করে: এটি ইনপুট হিসেবে পাওয়া ডেটাগুলোকে নির্দিষ্ট ফাইলে সেভ করে এবং একই সাথে স্ক্রিনেও প্রিন্ট করে দেখায়। এখানে এটি ডেটাগুলোকে /usr/share/keyrings/microsoft-edge.gpg নামের নতুন একটি ফাইলে সেভ করছে।
* > /dev/null অংশটি ব্যবহার করা হয়েছে স্ক্রিনের আউটপুট বন্ধ করার জন্য। যেহেতু tee কমান্ড ফাইল সেভ করার পাশাপাশি স্ক্রিনেও হাবিজাবি বাইনারি লেখা দেখাতো, তাই > /dev/null দিয়ে সেই স্ক্রিন আউটপুটকে লিনেক্সের একটি "ডিজিটাল ডাস্টবিন"-এ পাঠিয়ে গায়েব করে দেওয়া হয়েছে। ফলে টার্মিনাল একদম পরিষ্কার বা ক্লিন থাকে।


#### সংক্ষেপে পুরো লাইনের কাজ:
ইন্টারনেট থেকে মাইক্রোসফটের সিকিউরিটি কি ডাউনলোড করা হলো (curl), সেটাকে কম্পিউটারের বোঝার সুবিধার্থে বাইনারিতে রূপান্তর করা হলো (gpg --dearmor), এবং সবশেষে রুট পারমিশন নিয়ে স্ক্রিনে কোনো হিজিবিজি লেখা না দেখিয়ে সরাসরি লিনেক্সের নিরাপদ কি-রিং ফোল্ডারে ফাইলটি সেভ করা হলো (sudo tee ... > /dev/null)।
এই কি-টি যুক্ত করার পর আপনার লিনেক্স সিস্টেম নিশ্চিত হতে পারে যে, এরপর আপনি যখনই Microsoft Edge ব্রাউজার ইনস্টল বা আপডেট করবেন, সেটি আসল মাইক্রোসফটের কাছ থেকেই আসছে, কোনো হ্যাকারের কাছ থেকে নয়।


##### ১. এখানে \ (Backslash) এর অর্থ কী?
লিনেক্স টার্মিনালে \ (ব্যাকস্ল্যাশ) ব্যবহার করা হয় "Line Continuation" বা লাইন চালিয়ে যাওয়ার নির্দেশ হিসেবে।

* কেন ব্যবহার করা হয়: টার্মিনালে যখন কোনো কমান্ড অনেক লম্বা হয়ে যায়, তখন এক লাইনে পুরোটা দেখতে বা পড়তে সমস্যা হয়। কমান্ডটিকে ভেঙে পরবর্তী লাইনে নেওয়ার জন্য এবং লিনেক্সকে এটা বোঝানোর জন্য যে—"কমান্ডটি এখনো শেষ হয়নি, পরের লাইনের অংশটিও একই কমান্ডের অংশ", এই \ চিহ্নটি ব্যবহার করা হয়।
* কীভাবে কাজ করে: আপনি যখন \ দিয়ে Enter চাপবেন, টার্মিনাল কমান্ডটি রান (Run) না করে পরবর্তী লাইনে ইনপুট নেওয়ার জন্য অপেক্ষা করবে।

যদি আপনি \ ব্যবহার না করতে চান, তবে পুরো কমান্ডটি কোনো স্পেস বা ব্রেক ছাড়া এক লাইনে এভাবেও লিখতে পারেন (উভয়ই একদম এক কাজ করবে):

```code
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

### ২. wget ব্যবহার করে এর অল্টারনেটিভ (Alternative) সমাধান
curl এবং tee ব্যবহার না করে শুধুমাত্র wget এবং gpg দিয়ে এই কাজটি করার চমৎকার কিছু অল্টারনেটিভ বা বিকল্প উপায় রয়েছে। নিচে ৩টি সহজ পদ্ধতি দেওয়া হলো:

#### বিকল্প পদ্ধতি ১: এক লাইনে (wget + gpg + রিলিজ ডিরেক্টরি)
curl এর জায়গায় wget ব্যবহার করে এক লাইনে করার সবচেয়ে সহজ উপায় হলো এটি। এখানে wget -qO- কমান্ডটি ফাইলটি হার্ডডিস্কে সেভ না করে সরাসরি আউটপুটটি পরের কমান্ডে পাস করে দেয়:

wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null

(এখানে -q মানে Quiet/সাইলেন্টলি ডাউনলোড হবে এবং -O- মানে ফাইলটি সেভ না হয়ে টার্মিনাল আউটপুটে চলে যাবে)

#### বিকল্প পদ্ধতি ২: স্টেপ-বাই-স্টেপ (আলাদা ফাইল ডাউনলোড করে)
আপনি যদি পাইপ (|) বা tee ব্যবহার করতে না চান এবং ধাপে ধাপে ফাইল ডাউনলোড করে সিস্টেমে সেভ করতে চান, তবে এভাবে করতে পারেন:

***স্টেপ ক:*** প্রথমে wget দিয়ে সরাসরি ফাইলটি আপনার বর্তমান ফোল্ডারে ডাউনলোড করুন:

`wget https://packages.microsoft.com/keys/microsoft.asc`

***স্টেপ খ:*** এবার ডাউনলোড হওয়া ফাইলটিকে gpg দিয়ে রূপান্তর করে সরাসরি নির্দিষ্ট সিস্টেম ডিরেক্টরিতে সেভ করুন:

`sudo gpg --dearmor -o /usr/share/keyrings/microsoft-edge.gpg microsoft.asc`

(এখানে -o অপশনটি দিয়ে আমরা আউটপুট ফাইলের পাথ বা লোকেশন নির্দিষ্ট করে দিয়েছি, তাই আলাদা করে tee ব্যবহার করতে হয়নি)

***স্টেপ গ (ঐচ্ছিক):*** কাজ শেষ হয়ে গেলে মূল ডাউনলোড করা টেক্সট ফাইলটি ডিলিট করে দিতে পারেন:

`rm microsoft.asc`

#### বিকল্প পদ্ধতি ৩: সবচেয়ে আধুনিক ও ক্লিন পদ্ধতি (wget দিয়ে সরাসরি সেভ)
আধুনিক লিনেক্স সিস্টেমে (যেমন নতুন Ubuntu/Debian) gpg --dearmor ছাড়াও সরাসরি সিকিউরিটি কি রীড করা যায়। আপনি wget দিয়ে সরাসরি ফাইলটি ডাউনলোড করে সিস্টেমের কি-রিং ফোল্ডারে সেভ করে রাখতে পারেন এভাবে:

```code
sudo wget -O /usr/share/keyrings/microsoft-edge.asc \
https://packages.microsoft.com/keys/microsoft.asc
```


### অ্যাড রিপোসিটোরি - Add Repository

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main" \
| sudo tee /etc/apt/sources.list.d/microsoft-edge.list
```

এই কমান্ডটি লিনেক্স (বিশেষ করে Ubuntu বা Debian ভিত্তিক সিস্টেমে) Microsoft Edge ব্রাউজার ইনস্টল করার জন্য তার অফিশিয়াল সফটওয়্যার সোর্স বা রিপোজিটরি (Repository) যুক্ত করার কাজ করে।
সহজ কথায়, আগের কম্যান্ডে আমরা মাইক্রোসফটের সিকিউরিটি চাবি (Key) সিস্টেমে রেখেছিলাম, আর এই কমান্ডের মাধ্যমে লিনেক্সকে বলে দেওয়া হচ্ছে যে—"ভবিষ্যতে Microsoft Edge ডাউনলোড বা আপডেট করতে হলে কোন ইন্টারনেট অ্যাড্রেসে যেতে হবে।"
এখানেও \ (Line Continuation) দিয়ে কমান্ডটি ভাঙা হয়েছে এবং | (Pipe) চিহ্ন দিয়ে দুটি অংশকে জোড়া দেওয়া হয়েছে। নিচে প্রতিটি অংশের বিস্তারিত ব্যাখ্যা দেওয়া হলো:

#### স্টেপ ১: echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] https://packages.microsoft.com/repos/edge stable main"

echo কমান্ডের কাজ হলো এর ভেতরে ডাবল কোটেশনের (" ") মধ্যে থাকা পুরো লেখাটিকে টার্মিনালের আউটপুট হিসেবে প্রিন্ট করা বা বের করে দেওয়া।
কোটেশনের ভেতরের পুরো লেখাটি হলো একটি লিনেক্স রিপোজিটরি কনফিগারেশন লাইন। এর প্রতিটি অংশ আলাদা অর্থ বহন করে:

***deb:*** এটি লিনেক্সকে জানায় যে এই সোর্সটি একটি প্রি-কম্পাইলড বা রেডি-মেড সফটওয়্যার প্যাকেজ (Debian Package) প্রদান করবে, যা সরাসরি ইনস্টল করা সম্ভব।

***[arch=amd64]:*** এটি আপনার কম্পিউটারের প্রসেসর আর্কিটেকচার নির্দিষ্ট করে দেয়। এর মানে হলো সফটওয়্যারটি শুধুমাত্র 64-bit (Intel/AMD) কম্পিউটারের জন্য ডাউনলোড করা হবে।

***signed-by=/usr/share/keyrings/microsoft-edge.gpg:*** এটি অত্যন্ত গুরুত্বপূর্ণ একটি সিকিউরিটি সেটিং। এটি লিনেক্সের প্যাকেজ ম্যানেজারকে (apt) নির্দেশ দেয় যে—"এই সোর্স থেকে যখনই কোনো সফটওয়্যার ডাউনলোড করবে, তখন তা অবশ্যই এই নির্দিষ্ট ফোল্ডারে থাকা .gpg ডিজিটাল চাবি দিয়ে ভেরিফাই বা যাচাই করে নেবে।" (এটি আমরা ঠিক আগের কমান্ডে তৈরি করেছিলাম)।

***https://packages.microsoft.com/repos/edge:*** এটি মাইক্রোসফটের অফিশিয়াল সার্ভারের মূল লিংক বা ইউআরএল (URL), যেখানে Edge ব্রাউজারের সব ফাইল জমা আছে।

***stable main:*** stable মানে হলো আপনি ব্রাউজারের কোনো পরীক্ষামূলক বা বেটা সংস্করণ নয়, বরং সম্পূর্ণ ত্রুটিমুক্ত এবং চূড়ান্ত রিলিজ সংস্করণটি ডাউনলোড করতে চান। আর main হলো সেই সার্ভারের ভেতরের মূল ডিরেক্টরি বা ক্যাটাগরি।

#### স্টেপ ২: | sudo tee /etc/apt/sources.list.d/microsoft-edge.list

***Pipe(|):*** এটি স্টেপ ১-এর echo কমান্ড থেকে জেনারেট হওয়া পুরো টেক্সট লাইনটিকে সরাসরি এই দ্বিতীয় অংশের কাছে ট্রান্সফার করে দেয়।

***sudo:*** যেহেতু আমরা সিস্টেমের কনফিগারেশন ফাইল পরিবর্তন করছি, তাই রুট বা অ্যাডমিনিস্ট্রেটর পারমিশন নিশ্চিত করার জন্য sudo ব্যবহার করা হয়েছে।

***tee:*** আগেই যেমন জেনেছেন, tee কমান্ড ইনপুট হিসেবে পাওয়া ডেটাকে স্ক্রিনে দেখানোর পাশাপাশি ফাইলে সেভ করে। এখানে এটি echo থেকে পাওয়া টেক্সট লাইনটিকে সরাসরি /etc/apt/sources.list.d/microsoft-edge.list নামক একটি নতুন ফাইলে লিখে বা সেভ করে দেয়।

***.list ফাইল কী?:*** লিনেক্সের apt প্যাকেজ ম্যানেজার যখনই কোনো সফটওয়্যার ইনস্টল করতে যায়, তখন সে /etc/apt/sources.list.d/ ফোল্ডারের ভেতরের সব .list ফাইল চেক করে দেখে যে ইন্টারনেটে নতুন কোনো সফটওয়্যারের দোকান বা সোর্স যুক্ত হয়েছে কিনা। এখানে আমরা মাইক্রোসফটের জন্য আলাদা একটি ফাইল তৈরি করে দিলাম।

#### সংক্ষেপে পুরো লাইনের কাজ:
আমরা একটি টেক্সট তৈরি করলাম (echo) যাতে মাইক্রোসফটের সার্ভার লিংক ও সিকিউরিটি চাবির লোকেশন লেখা আছে এবং পাইপের মাধ্যমে রুট পারমিশন নিয়ে সেই টেক্সটটিকে লিনেক্সের নিজস্ব সফটওয়্যার সোর্স লিস্ট ডিরেক্টরিতে microsoft-edge.list নামে একটি ফাইল বানিয়ে সেভ করে দিলাম (sudo tee)।


### Install Edge

```bash
sudo apt update
sudo apt install microsoft-edge-stable -y
```

আপনার লিনেক্স সিস্টেমে Microsoft Edge সোর্স ফাইল এবং সিকিউরিটি কি (Key) যুক্ত করার পর, এই দুটি কমান্ডই হলো ব্রাউজারটি কম্পিউটারে সফলভাবে ডাউনলোড এবং ইনস্টল করার চূড়ান্ত ধাপ।
নিচে প্রতিটি কমান্ড এবং তার ভেতরের অপশনগুলোর বিস্তারিত ব্যাখ্যা দেওয়া হলো:

#### ১. sudo apt update
এই কমান্ডটি আপনার কম্পিউটারে কোনো সফটওয়্যার ইনস্টল করে না, বরং এটি ইনস্টল করার পূর্বপ্রস্তুতি।

***sudo:*** লিনেক্সের যেকোনো সিস্টেম-লেভেল পরিবর্তন বা সফটওয়্যার তালিকা আপডেট করার জন্য অ্যাডমিনিস্ট্রেটর বা রুট (Root) পারমিশন লাগে। sudo সেই ক্ষমতা দেয়।

***apt:*** এটি হলো Advanced Package Tool। এটি Debian, Ubuntu বা Linux Mint-এর মতো ডিস্ট্রিবিউশনগুলোর নিজস্ব অফিসিয়াল সফটওয়্যার ম্যানেজার (যেমন মোবাইলের Play Store বা App Store)।

***update:*** এই অপশনটি লিনেক্সকে নির্দেশ দেয় তার ডিরেক্টরিতে থাকা সমস্ত সফটওয়্যার সোর্সের (Repositories) তালিকায় গিয়ে খোঁজাখুঁজি করতে যে, ইন্টারনেটে কোনো নতুন অ্যাপ বা আপডেট এসেছে কিনা।

##### কেন এই কমান্ডটি দেওয়া জরুরি?
পূর্ববর্তী কমান্ডগুলোতে আমরা মাইক্রোসফটের একটি নতুন সোর্স লিস্ট ফাইল (microsoft-edge.list) সিস্টেমে যুক্ত করেছিলাম। কিন্তু আপনার প্যাকেজ ম্যানেজার (apt) এখনো জানে না যে এই নতুন ফাইলটি যুক্ত হয়েছে। আপনি যখন sudo apt update দেবেন, তখন সে ওই ফাইলটি রিড করবে এবং মাইক্রোসফটের সার্ভার থেকে Edge ব্রাউজারের লেটেস্ট ভার্সনের একটি তালিকা আপনার কম্পিউটারে এনে সেভ করবে। এটি না করলে পরবর্তী ইনস্টল কমান্ডটি কাজ করবে না।

#### ২. sudo apt install microsoft-edge-stable -y
এই কমান্ডটি চূড়ান্তভাবে ইন্টারনেট থেকে ফাইল ডাউনলোড করে আপনার কম্পিউটারে ইনস্টল করে।

***sudo apt:*** যথারীতি রুট পারমিশন নিয়ে প্যাকেজ ম্যানেজারকে কল করা হলো।

***install:*** এটি apt-এর একটি অ্যাকশন কমান্ড, যা লিনেক্সকে নির্দিষ্ট কোনো সফটওয়্যার প্যাকেজ ডাউনলোড এবং ইনস্টল করার নির্দেশ দেয়।

***microsoft-edge-stable:*** এটি হলো মূল সফটওয়্যারটির প্যাকেজ নাম (Package Name)। লিনেক্সের সার্ভারে Microsoft Edge-কে এই নামেই রেজিস্টার করা আছে। stable অংশটি নিশ্চিত করে যে আপনি ব্রাউজারের সম্পূর্ণ ত্রুটিমুক্ত এবং অফিসিয়াল ফাইনাল সংস্করণটি ইনস্টল করছেন।

***-y (বা --yes) ফ্ল্যাগ:*** এটি একটি অত্যন্ত দরকারী শর্টকাট। সাধারণ নিয়ম অনুযায়ী, আপনি যখন কোনো সফটওয়্যার ইনস্টল করতে যান, তখন টার্মিনাল আপনাকে দেখায় যে অ্যাপটি কত মেগাবাইট (MB) জায়গা নেবে এবং আপনার কাছে সম্মতি জানতে চায়—Do you want to continue? [Y/n]। তখন কীবোর্ডে y লিখে এন্টার দিতে হয়। কমান্ডের শেষে সরাসরি -y লিখে দিলে টার্মিনাল নিজে থেকেই সেই সম্মতি (Yes) দিয়ে দেয়, ফলে মাঝপথে আর কোনো ইন্টারঅ্যাকশন ছাড়াই পুরো ইনস্টলেশন প্রক্রিয়াটি স্বয়ংক্রিয়ভাবে সম্পন্ন হয়ে যায়।


#### সংক্ষেপে পুরো প্রক্রিয়া (Summary)

```tree
[ Step 1: sudo apt update ] 
       │
       ▼ ইন্টারনেটের সব সোর্স থেকে লেটেস্ট অ্যাপের লিস্ট আপডেট করা হলো (মাইক্রোসফট এজ সহ)
       │
[ Step 2: sudo apt install microsoft-edge-stable -y ]
       │
       ▼ কোনো পারমিশন না চেয়েই স্বয়ংক্রিয়ভাবে এজ ব্রাউজার ডাউনলোড ও ইনস্টল হয়ে গেল!
```

কমান্ড দুটি সফলভাবে রান করার পর আপনার লিনেক্স অ্যাপ মেন্যুতে Microsoft Edge-এর আইকনটি চলে আসবে।



### Practical Example — VS Code

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

### Practical Example — Docker

Docker Official Repository ব্যবহার করে Install করা উচিত।

কারণ Ubuntu Repository-এর Version অনেক সময় Outdated থাকে।

---

## Snap vs Flatpak vs AppImage vs APT

| Feature | APT | Snap | Flatpak | AppImage |
|----------|----------|----------|----------|----------|
| System Integration | ✅ | ✅ | ✅ | ⚠️ |
| Auto Update | ✅ | ✅ | ✅ | ❌ |
| Isolation | ❌ | ✅ | ✅ | ❌ |
| Speed | ✅ | ⚠️ | ✅ | ✅ |
| Portable | ❌ | ❌ | ❌ | ✅ |

---

## সেরা অনুশীলন - Best Practices

#### Official Documentation Follow করুন

ভালো: `docs.docker.com`

খারাপ: `random-blog.xyz`

#### সবসময় HTTPS ব্যবহার করুন

`https://`

#### GPG Key Verify করুন

`gpg --show-keys file.gpg`

#### Unused Repository Remove করুন

বর্তমান Repository List:

```bash
ls /etc/apt/sources.list.d/
```

---

## সমস্যা সমাধানের গাইড - Troubleshooting Guide

#### NO_PUBKEY Error

```text
NO_PUBKEY XXXXX
```

সমাধান:

```bash
Re-import GPG Key
```

#### 404 Not Found

```text
Repository Not Found
```

সমাধান:

Repository URL Verify করুন।

#### Broken Packages

```bash
sudo apt --fix-broken install
```

#### Dependency Error

```bash
sudo apt install -f
```

#### Package Lock Error

```text
Could not get lock
```

সমাধান:

অন্য Package Manager Process বন্ধ করুন।

---

## Real World Examples

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

## Key Takeaways

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