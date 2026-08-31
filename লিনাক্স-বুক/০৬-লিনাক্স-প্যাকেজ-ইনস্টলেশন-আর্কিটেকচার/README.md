> [🏠](../) [⬅️ ০৫। লিনাক্স থিম কাস্টমাইজেশন](../০৫-লিনাক্স-থিম-কাস্টমাইজেশন) [➡️ ০৭। কমান্ড লাইন অপশন](../০৭-কমান্ড-লাইন-অপশন)

# ০৬। লিনাক্স প্যাকেজ ইনস্টলেশন আর্কিটেকচার

> Ubuntu ও Debian-ভিত্তিক Linux সিস্টেমে Third-Party Software কীভাবে নিরাপদে ইনস্টল, যাচাই, আপডেট এবং পরিচালনা করা হয় তার পূর্ণাঙ্গ গাইড।

## সূচিপত্র

- থার্ড-পার্টি প্যাকেজ কী?
- Linux-এ প্যাকেজ ইনস্টল করার ৫টি প্রধান উপায়
- Linux Package Management Architecture
- APT কীভাবে কাজ করে?
- Repository কী?
- GPG Key ও Digital Signature
- Generic 4-Step Installation Workflow
- apt update ও apt install-এর Backend Workflow
- `.deb` Package Anatomy
- Practical Example (Microsoft Edge, VS Code, Docker)
- ইনস্টলেশন লোকেশন (Installation Location)
- থার্ড-পার্টি প্যাকেজ কাস্টমাইজেশন
- Snap vs Flatpak vs AppImage vs APT
- Security Best Practices
- Troubleshooting Guide
- Key Takeaways

---

## ১. থার্ড-পার্টি প্যাকেজ কী?

Ubuntu-এর Official Repository-এর বাইরে থাকা যেকোনো Software Package-কে **Third-Party Package** বলা হয়।

**উদাহরণ:**

- Google Chrome
- Microsoft Edge
- Visual Studio Code
- Docker
- GitHub CLI
- Node.js
- Postman
- Brave Browser

এগুলো সাধারণত Ubuntu-এর অফিসিয়াল রিপোজিটরিতে থাকে না। তাই Software Vendor নিজস্ব Repository প্রদান করে।

---

## ২. Linux-এ প্যাকেজ ইনস্টল করার ৫টি প্রধান উপায়

Linux-এ মূলত ৫টি প্রধান পদ্ধতিতে সফটওয়্যার ইনস্টল করা যায়:

### ২.১ Native Package Manager (ডিফল্ট প্যাকেজ ম্যানেজার)

সবচেয়ে নিরাপদ ও জনপ্রিয় পদ্ধতি। ডিস্ট্রিবিউশনের নিজস্ব রিপোজিটরি থেকে ইনস্টল করা হয়।

- **Debian/Ubuntu (APT):**  
  `sudo apt install package_name`
- **RedHat/Fedora/CentOS (DNF/YUM):**  
  `sudo dnf install package_name`
- **Arch Linux (Pacman):**  
  `sudo pacman -S package_name`

***Special Note:***

আপনি যদি আপনার টাইপিং অভ্যাসের কারণে apt কমান্ডই লিখতে চান, তবে একটি Alias তৈরি করে নিতে পারেন। এতে ব্যাকএন্ডে dnf কাজ করবে, কিন্তু আপনি টাইপ করবেন apt।

ধাপ ১: টার্মিনালে আপনার প্রোফাইল ফাইলটি খুলুন:

```bash
nano ~/.bashrc
```

ধাপ ২: ফাইলের একদম নিচে নিচের লাইনগুলো যোগ করুন:

```bash
alias apt='sudo dnf'
alias apt-get='sudo dnf'
```

ধাপ ৩: ফাইলটি সেভ করে বের হয়ে আসুন (Ctrl+O, তারপর Enter, এবং Ctrl+X)। এবার পরিবর্তনটি সক্রিয় করতে রান করুন:

```bash
source ~/.bashrc
```


### ২.২ Universal Package Managers

যেকোনো ডিস্ট্রোতে একই প্যাকেজ চালানোর জন্য ব্যবহার হয়। সব ডিপেন্ডেন্সি একসাথে প্যাক করে রাখে।

- **Snap** (Canonical/Ubuntu):  
  `sudo snap install package_name`
- **Flatpak**:  
  `flatpak install flathub package_name`
- **AppImage**: ইনস্টলেশনের প্রয়োজন নেই। ফাইল ডাউনলোড করে Executable পারমিশন দিয়ে ডাবল-ক্লিক করেই চালানো যায়।

### ২.৩ Source Code Compiling

প্যাকেজ ম্যানেজারে না পেলে সোর্স কোড থেকে বিল্ড করতে হয়।

**ধাপ:**

1. `.tar.gz` ফাইল এক্সট্রাক্ট করুন
2. `./configure`
3. `make`
4. `sudo make install`

### ২.৪ Binary Packages (ম্যানুয়াল বাইনারি)

অনেক কোম্পানি রেডিমেড `.deb` বা `.rpm` ফাইল দেয়।

- **Ubuntu/Debian:**  
  `sudo dpkg -i package.deb`
- **Fedora/RedHat:**  
  `sudo rpm -ivh package.rpm`

### ২.৫ Language-Specific Package Managers

প্রোগ্রামিং ল্যাঙ্গুয়েজের নিজস্ব ম্যানেজার।

- **Python:** `pip install package_name`
- **Node.js:** `npm install -g package_name`

---

## ৩. Linux Package Management Architecture

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

## ৪. APT কী?

**APT** = Advanced Package Tool  
Ubuntu ও Debian-এর প্রধান Package Manager।

### APT-এর প্রধান ৫টি কাজ:

1. Package খুঁজে বের করা → `apt search nginx`
2. Package Install করা → `sudo apt install nginx`
3. Package Update করা → `sudo apt upgrade`
4. Package Remove করা → `sudo apt remove nginx`
5. Dependency Resolve করা (স্বয়ংক্রিয়ভাবে)

---

## ৫. Repository কী?

Repository হলো Software Storage Server। এখান থেকে Package ডাউনলোড করা হয়।

### Repository Structure

```text
repository/
├── dists/          → Metadata (jammy, noble, bookworm ইত্যাদি)
├── pool/           → Actual Package Files (.deb)
├── Release         → Summary File
├── InRelease
└── Release.gpg     → Digital Signature
```

---

## ৬. GPG Key ও Digital Signature

**GPG** = GNU Privacy Guard  
Package Authenticity যাচাই করার জন্য ব্যবহৃত হয়।

### Verification Process

```text
Package → Signature → Public Key → Verification → Install
```

যদি মাঝপথে কেউ Package পরিবর্তন করে, GPG Signature দিয়ে Linux সেটা Detect করতে পারে।

**ASCII Armored Key** (যেমন `microsoft.asc`) টেক্সট ফরম্যাটে থাকে।  
`gpg --dearmor` দিয়ে Binary Keyring-এ রূপান্তর করা হয়।

---

## ৭. Generic 4-Step Installation Workflow (থার্ড-পার্টি প্যাকেজ)

প্রায় সব Third-Party Package এই একই প্যাটার্ন অনুসরণ করে।

### Step 1 — Dependencies ইনস্টল

```bash
sudo apt update
sudo apt install wget curl gpg software-properties-common -y
```

### Step 2 — GPG Key Import

```bash
wget -qO- KEY_URL | sudo gpg --dearmor -o /usr/share/keyrings/package.gpg
```

অথবা

```bash
curl KEY_URL | gpg --dearmor | sudo tee /usr/share/keyrings/package.gpg > /dev/null
```

**Backend:**

```
Download Key → Convert to Binary → Store in Keyring
```

### Step 3 — Repository যোগ করা

```bash
echo "deb [signed-by=/usr/share/keyrings/package.gpg] REPO_URL stable main" \
| sudo tee /etc/apt/sources.list.d/package.list
```

নতুন ফাইল তৈরি হয়: `/etc/apt/sources.list.d/`

### Step 4 — Refresh ও Install

```bash
sudo apt update
sudo apt install package-name -y
```

---

## ৮. apt update ও apt install-এর Backend Workflow

### apt update কী করে?

Software আপডেট করে না। এটি **Repository Metadata Refresh** করে।

```text
Read Sources → Connect Repository → Download Metadata → Verify Signature → Update Local Cache
```

**Cache Location:** `/var/lib/apt/lists/`

### apt install কী করে?

```text
Find Package → Resolve Dependency → Download Package → Verify Signature → Extract Files → Register Package → Finish
```

**Download Location:** `/var/cache/apt/archives/`

---

## ৯. `.deb` Package Anatomy

একটি `.deb` প্যাকেজের ভিতরে থাকে:

- **control.tar.gz** → Metadata (Name, Version, Dependencies, Maintainer)
- **data.tar.gz** → Actual Software Files
- **debian-binary**

---

## ১০. Practical Example — Microsoft Edge  
**(বিস্তারিত ধাপে ধাপে ব্যাখ্যাসহ)**

Microsoft Edge ইনস্টল করার সম্পূর্ণ প্রক্রিয়া ৪টি প্রধান ধাপে বিভক্ত। নিচে প্রতিটি ধাপের কমান্ড এবং তার বিস্তারিত ব্যাখ্যা দেওয়া হলো।

### ধাপ ১: Dependencies ইনস্টল
```bash
sudo apt update
sudo apt install wget curl gpg -y
```

### ধাপ ২: GPG Key যোগ করা (Add GPG Key)

```bash
curl https://packages.microsoft.com/keys/microsoft.asc \
| gpg --dearmor \
| sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

এই কমান্ডটি Microsoft-এর অফিসিয়াল সিকিউরিটি কী (GPG Key) সিস্টেমে যুক্ত করার স্ট্যান্ডার্ড পদ্ধতি।  
এখানে `|` (পাইপ) চিহ্নের মাধ্যমে ৩টি আলাদা কমান্ডকে একসাথে জোড়া দেওয়া হয়েছে।

#### স্টেপ-বাই-স্টেপ ব্যাখ্যা:

**স্টেপ ১: `curl https://packages.microsoft.com/keys/microsoft.asc`**
- `curl` টার্মিনালের একটি টুল যা ইন্টারনেট থেকে ডেটা বা ফাইল ডাউনলোড করে।
- এখানে Microsoft-এর অফিসিয়াল ওয়েবসাইট থেকে `microsoft.asc` ফাইলটি ডাউনলোড করা হচ্ছে।
- এই ফাইলটি মানুষের পড়ার উপযোগী টেক্সট ফরম্যাটে (ASCII-armored) থাকে।

**স্টেপ ২: `gpg --dearmor`**
- `|` (Pipe) প্রথম স্টেপের ডেটা সরাসরি এই স্টেপে পাঠায়।
- `gpg` (GNU Privacy Guard) হলো লিনাক্সের ক্রিপ্টোগ্রাফি/সিকিউরিটি টুল।
- `--dearmor` অপশনটি ASCII টেক্সট ফাইলকে কম্পিউটারের পছন্দের Binary ফরম্যাটে রূপান্তর করে।

**স্টেপ ৩: `sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null`**
- আবার `|` দিয়ে Binary ডেটা এই স্টেপে আসে।
- `sudo` → সিস্টেম ডিরেক্টরিতে লেখার জন্য Root পারমিশন।
- `tee` → ডেটা ফাইলে সেভ করে + স্ক্রিনে দেখায়।
- `> /dev/null` → স্ক্রিনের বাইনারি আউটপুট লুকিয়ে টার্মিনাল পরিষ্কার রাখে।

**সংক্ষেপে পুরো লাইনের কাজ:**  
ইন্টারনেট থেকে Microsoft-এর সিকিউরিটি কী ডাউনলোড → Binary-তে রূপান্তর → Root পারমিশন নিয়ে নিরাপদ Keyring ফোল্ডারে সেভ।

এই কী যুক্ত হওয়ার পর সিস্টেম নিশ্চিত হতে পারে যে Edge ইনস্টল/আপডেট হচ্ছে আসল Microsoft থেকে, কোনো হ্যাকার থেকে নয়।

#### `\ (Backslash)` এর অর্থ কী?
লিনাক্স টার্মিনালে `\` ব্যবহার করা হয় **Line Continuation**-এর জন্য।  
লম্বা কমান্ডকে ভেঙে পরের লাইনে নেওয়ার সুবিধা দেয়।  
এক লাইনে লিখলেও একই কাজ হয়:
```bash
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```

#### wget ব্যবহার করে অল্টারনেটিভ পদ্ধতি

**বিকল্প ১: এক লাইনে (সবচেয়ে সহজ)**
```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | sudo tee /usr/share/keyrings/microsoft-edge.gpg > /dev/null
```
- `-q` → Quiet (সাইলেন্ট)
- `-O-` → ফাইল সেভ না করে সরাসরি আউটপুট পাস করে

**বিকল্প ২: স্টেপ-বাই-স্টেপ (পাইপ ছাড়া)**
```bash
wget https://packages.microsoft.com/keys/microsoft.asc
sudo gpg --dearmor -o /usr/share/keyrings/microsoft-edge.gpg microsoft.asc
rm microsoft.asc   # ঐচ্ছিক
```

**বিকল্প ৩: আধুনিক ও ক্লিন পদ্ধতি**
```bash
sudo wget -O /usr/share/keyrings/microsoft-edge.gpg \
https://packages.microsoft.com/keys/microsoft.asc
```

---

### ধাপ ৩: Repository যোগ করা (Add Repository)

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/microsoft-edge.gpg] \
https://packages.microsoft.com/repos/edge stable main" \
| sudo tee /etc/apt/sources.list.d/microsoft-edge.list
```

এই কমান্ডটি Microsoft Edge-এর অফিসিয়াল Repository সিস্টেমে যুক্ত করে।  
আগের ধাপে কী রাখা হয়েছে, এখন সিস্টেমকে বলা হচ্ছে — “Edge ডাউনলোড করতে হলে এই ঠিকানায় যেতে হবে।”

#### স্টেপ-বাই-স্টেপ ব্যাখ্যা:

**স্টেপ ১: `echo "deb [arch=amd64 signed-by=...] https://... stable main"`**
- `echo` → উদ্ধৃতিতে থাকা টেক্সট প্রিন্ট করে।
- প্রতিটি অংশের অর্থ:
  - `deb` → Pre-compiled Debian Package
  - `[arch=amd64]` → শুধুমাত্র ৬৪-বিট (Intel/AMD) সিস্টেমের জন্য
  - `signed-by=/usr/share/keyrings/microsoft-edge.gpg` → এই কী দিয়ে Verify করতে হবে
  - `https://packages.microsoft.com/repos/edge` → Microsoft-এর অফিসিয়াল সার্ভার
  - `stable main` → স্থিতিশীল (Stable) রিলিজ ভার্সন

**স্টেপ ২: `| sudo tee /etc/apt/sources.list.d/microsoft-edge.list`**
- `|` → echo-এর আউটপুট পাঠায়
- `sudo` → Root পারমিশন
- `tee` → ফাইলে সেভ করে
- `.list` ফাইল → APT এই ফোল্ডারের সব `.list` ফাইল চেক করে নতুন সোর্স খুঁজে বের করে

**সংক্ষেপে:**  
Microsoft-এর সার্ভার লিংক + সিকিউরিটি কীর লোকেশন লিখে `/etc/apt/sources.list.d/` ফোল্ডারে একটি নতুন `.list` ফাইল তৈরি করা হলো।

---

### ধাপ ৪: Install Edge

```bash
sudo apt update
sudo apt install microsoft-edge-stable -y
```

#### ১. `sudo apt update`
- কোনো সফটওয়্যার ইনস্টল করে না।
- নতুন যোগ করা Repository-এর Metadata (প্যাকেজ তালিকা) ডাউনলোড করে।
- এই কমান্ড না দিলে পরের ইনস্টল কমান্ড কাজ করবে না।

#### ২. `sudo apt install microsoft-edge-stable -y`
- `install` → প্যাকেজ ডাউনলোড ও ইনস্টল করে
- `microsoft-edge-stable` → প্যাকেজের অফিসিয়াল নাম
- `-y` → সব প্রশ্নের উত্তর স্বয়ংক্রিয়ভাবে “Yes” দেয় (মাঝপথে থামে না)

**সংক্ষেপে প্রক্রিয়া:**
```text
[ sudo apt update ]
       │
       ▼ সব সোর্স থেকে লেটেস্ট তালিকা আপডেট (Microsoft Edge সহ)
       │
[ sudo apt install microsoft-edge-stable -y ]
       │
       ▼ স্বয়ংক্রিয়ভাবে ডাউনলোড ও ইনস্টল সম্পন্ন
```

কমান্ড সফল হলে অ্যাপ মেনুতে Microsoft Edge-এর আইকন চলে আসবে।

---

## ১১. Practical Example — VS Code
```bash
wget -qO- https://packages.microsoft.com/keys/microsoft.asc \
| gpg --dearmor \
| sudo tee /usr/share/keyrings/packages.microsoft.gpg > /dev/null

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/packages.microsoft.gpg] \
https://packages.microsoft.com/repos/code stable main" \
| sudo tee /etc/apt/sources.list.d/vscode.list

sudo apt update
sudo apt install code -y
```

## ১২. Practical Example — Docker
Docker Official Repository ব্যবহার করুন (Ubuntu-এর ডিফল্ট ভার্সন প্রায়ই পুরনো হয়)।

---

## ১৩. ইনস্টলেশন লোকেশন

| পদ্ধতি                    | ডিফল্ট লোকেশন                                      |
|---------------------------|----------------------------------------------------|
| Source Code               | `/usr/local/bin/` ও `/usr/local/lib/`             |
| Proprietary (.deb/.rpm)   | `/opt/` (যেমন `/opt/google/chrome`)               |
| Snap                      | `/snap/bin/` + `~/snap/`                          |
| Flatpak                   | `/var/lib/flatpak/` বা `~/.local/share/flatpak/`  |
| pip / npm                 | `/usr/local/bin/` বা `~/.local/bin/`              |

---

## ১৪. কাস্টমাইজেশন
- `--prefix` দিয়ে Source Code-এর লোকেশন পরিবর্তন
- `PATH` Environment Variable যোগ
- `~/.config/` ও `~/.local/share/` থেকে কনফিগারেশন এডিট
- Symlink তৈরি: `sudo ln -s /opt/... /usr/local/bin/myapp`

---

## ১৫. Snap vs Flatpak vs AppImage vs APT

| Feature            | APT | Snap | Flatpak | AppImage |
|--------------------|-----|------|---------|----------|
| System Integration | ✅  | ✅   | ✅      | ⚠️       |
| Auto Update        | ✅  | ✅   | ✅      | ❌       |
| Isolation          | ❌  | ✅   | ✅      | ❌       |
| Speed              | ✅  | ⚠️   | ✅      | ✅       |
| Portable           | ❌  | ❌   | ❌      | ✅       |

---

## ১৬. Security Best Practices
- Official Documentation ফলো করুন
- সবসময় HTTPS ব্যবহার করুন
- GPG Key Verify করুন
- Unused Repository মুছে ফেলুন

---

## ১৭. Troubleshooting
| সমস্যা              | সমাধান                          |
|---------------------|---------------------------------|
| NO_PUBKEY           | GPG Key আবার Import             |
| 404 Not Found       | Repository URL যাচাই            |
| Broken Packages     | `sudo apt --fix-broken install`|
| Dependency Error    | `sudo apt install -f`           |
| Package Lock        | অন্য Process বন্ধ করুন          |

---

## ১৮. Key Takeaways
- APT হলো Linux Package Management-এর কেন্দ্রবিন্দু।
- Third-Party Package ইনস্টল করতে Repository + GPG Key প্রয়োজন।
- `apt update` → Metadata Refresh করে।
- `apt install` → প্যাকেজ ইনস্টল করে।
- Official Repository সবচেয়ে নিরাপদ।
- Edge/VS Code/Docker ইত্যাদির জন্য উপরের ৪-ধাপ Workflow অনুসরণ করুন।

---

> [🏠](../) [⬅️ ০৫। লিনাক্স থিম কাস্টমাইজেশন](../০৫-লিনাক্স-থিম-কাস্টমাইজেশন) [➡️ ০৭। কমান্ড লাইন অপশন](../০৭-কমান্ড-লাইন-অপশন)