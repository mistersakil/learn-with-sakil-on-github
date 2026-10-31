> [🏠](../) [⬅️ ০৩। ডেভঅপস ডেভেলপমেন্ট এনভায়রনমেন্ট](../০৩-ডেভঅপস-ডেভেলপমেন্ট-এনভায়রনমেন্ট) [➡️ ০৪। wget-এর ব্যবহার](../০৪-wget-এর-ব্যবহার)

# ৩.১। লিনাক্স সার্ভার প্রাথমিক সেটআপ, SSH Key Login ও systemctl

এই অধ্যায়টি সরাসরি হার্ডওয়্যারে (কিবোর্ড-মনিটর দিয়ে) লিনাক্স সার্ভার ইন্সটল করার পর কী কী করতে হয়, তার পূর্ণাঙ্গ ও বিস্তারিত গাইড।
প্রোডাকশনে যাওয়ার আগে এই ধাপগুলো মাস্টার করা অপরিহার্য।

## ভূমিকা — কেন এই সেটআপ এত গুরুত্বপূর্ণ?

নতুন লিনাক্স সার্ভার ইন্সটল করার পরপরই তা ইন্টারনেটে ছেড়ে দেওয়া সবচেয়ে বড় ও সবচেয়ে সাধারণ ভুল।
ইন্টারনেটে সংযুক্ত হওয়ার **প্রথম ১০–৩০ মিনিটের মধ্যেই** হাজার হাজার হ্যাকার বট (botnet) SSH পোর্ট ২২-এ আক্রমণ শুরু করে।
তারা শুধু `root`, `admin`, `ubuntu` ইত্যাদি কমন ইউজারনেম আর সাধারণ পাসওয়ার্ড চেষ্টা করে।

অনেক নতুন সার্ভার মাত্র কয়েক ঘণ্টার মধ্যেই কম্প্রোমাইজড হয়ে যায়। তাই প্রোডাকশনে যাওয়ার আগে নিচের কাজগুলো **বাধ্যতামূলক**ভাবে সম্পন্ন করতে হবে।
এই অধ্যায়ে আমরা ধাপে ধাপে সবকিছু করবো — শুরুতে শুধু কিবোর্ড-মনিটর থাকলে কী করবেন, তারপর রিমোট থেকে নিরাপদ SSH Key Login, Firewall, Fail2Ban, systemctl এবং অটো আপডেট পর্যন্ত।

**⚠ গুরুত্বপূর্ণ সতর্কতা**
এই পুরো প্রক্রিয়া চলাকালীন কমপক্ষে **দুইটি টার্মিনাল** খোলা রাখুন। একটিতে কাজ করবেন, অন্যটিতে নতুন সেটিংস টেস্ট করবেন।
SSH পোর্ট পরিবর্তন বা PasswordAuthentication বন্ধ করার পর যদি নতুন কানেকশন কাজ না করে, তাহলে পুরনো সেশন থেকে ফিরে আসতে পারবেন।

---

## পর্ব ১: যখন হাতে শুধু কিবোর্ড আর মনিটর আছে (Physical Console)

অনেকেই মনে করেন SSH Key বানানোর জন্য আরেকটা কম্পিউটার লাগবে। যদি আপনার কাছে শুধু একটা মেশিন থাকে যেখানে সরাসরি লিনাক্স সার্ভার ইন্সটল করেছেন,
তাহলে আপনি এখন **Physical Console**-এ আছেন। এই অবস্থায় SSH Key বানানোর দরকার নেই। প্রথমে পাসওয়ার্ড দিয়েই কাজ চালান।

### ধাপ ১: লগইন ও বেসিক চেক

1. ইন্সটলের সময় যে ইউজার বানিয়েছিলেন, সেই ইউজার দিয়ে লগইন করুন।
2. আইপি অ্যাড্রেস দেখুন:

```bash
ip a
# অথবা আরও সহজভাবে
hostname -I
```

`hostname -I` সাধারণত প্রাইভেট আইপি দেখায় (যেমন `192.168.x.x` বা `10.x.x.x`)।
পাবলিক আইপি জানতে চাইলে `curl ifconfig.me` বা `curl ipinfo.io/ip` ব্যবহার করতে পারেন।

3. ইন্টারনেট কানেকশন আছে কিনা যাচাই করুন:

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

যদি `ping` কাজ না করে, তাহলে নেটওয়ার্ক কনফিগারেশন চেক করুন (`/etc/netplan/` ফোল্ডার)।
অনেক সময় DHCP থেকে আইপি না পেলে ম্যানুয়ালি সেট করতে হয়।

### ধাপ ২: SSH সার্ভিস ইন্সটল ও চালু করা

লিনাক্স সার্ভারের কিছু মিনিমাল ইন্সটলে `openssh-server` আগে থেকে থাকে না। তাই প্রথমেই ইন্সটল করে নিন:

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
```

`enable --now` মানে এখনই চালু + রিবুটের পরও অটো চালু থাকবে।

**টিপ**
এই অবস্থায় আপনি পাসওয়ার্ড দিয়েই SSH করতে পারবেন। Key তখনই লাগবে যখন অন্য কোনো ল্যাপটপ, মোবাইল বা অন্য সার্ভার থেকে দূর থেকে এই মেশিনে ঢুকতে চাইবেন।

---

## পর্ব ২: সার্ভার ইন্সটলের পর ৮টি প্রাথমিক ও গুরুত্বপূর্ণ কাজ

### ১. সিস্টেম আপডেট (সবচেয়ে প্রথম কাজ)

নতুন ইন্সটল করা সিস্টেমে অনেক পুরনো প্যাকেজ থাকে। প্রথমেই আপডেট করুন:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y
sudo apt autoclean
```

**কেন গুরুত্বপূর্ণ?**
অনেক সিকিউরিটি ভলনারেবিলিটি পুরনো প্যাকেজে থাকে। আপডেট না করলে হ্যাকার সহজেই এক্সপ্লয়ট করতে পারে।

### ২. রুট বাদ দিয়ে নতুন Sudo ইউজার তৈরি

**কখনোই সরাসরি root ইউজার দিয়ে কাজ করবেন না।** root অ্যাকাউন্ট ডিজেবল করে নতুন একজন sudo ইউজার বানান।

```bash
# নতুন ইউজার তৈরি (উদাহরণ: sakil)
sudo adduser sakil

# sudo গ্রুপে যোগ করা
sudo usermod -aG sudo sakil

# যাচাই করা
groups sakil
```

এখন নতুন ইউজারে সুইচ করে টেস্ট করুন:

```bash
su - sakil
sudo whoami   # আউটপুট: root হওয়া উচিত
```

**সতর্কতা**
নতুন ইউজার বানানোর পর পুরনো ইউজার (যদি থাকে) থেকে লগআউট করে নতুন ইউজার দিয়ে আবার লগইন করে নিশ্চিত হোন যে sudo কাজ করছে।
না হলে root অ্যাক্সেস হারাতে পারেন।

### ৩. SSH Key Login সেটআপ (সবচেয়ে নিরাপদ লগইন পদ্ধতি)

পাসওয়ার্ডের বদলে **Public-Private Key Pair** দিয়ে লগইন করা অনেক বেশি নিরাপদ।
Private Key আপনার কাছে থাকবে, Public Key সার্ভারে যাবে।

#### চাবি কোথায় বানাবেন?

**সবসময় আপনার নিজের ল্যাপটপ বা পিসিতে** চাবি বানাবেন — সার্ভারে নয়।
কারণ Private Key যদি সার্ভারে থাকে এবং সার্ভার হ্যাক হয়, তাহলে আক্রমণকারী আপনার অন্যান্য সার্ভারেও ঢুকতে পারবে।

#### নিজের কম্পিউটারে Key তৈরি (Linux / macOS / Windows PowerShell / WSL)

```bash
ssh-keygen -t ed25519 -C "sakil@server" -f ~/.ssh/id_ed25519_sakil
```

ব্যাখ্যা:

- `-t ed25519` → আধুনিক ও সবচেয়ে নিরাপদ অ্যালগরিদম (RSA-এর চেয়ে ভালো)
- `-C` → কমেন্ট (কেবল মনে রাখার জন্য)
- `-f` → ফাইলের নাম কাস্টম করতে চাইলে

চাইলে পাসফ্রেজ (passphrase) দিতে পারেন। পাসফ্রেজ দিলে প্রতিবার লগইনের সময় সেটা দিতে হবে, কিন্তু Private Key চুরি হলেও নিরাপদ থাকবে।

তৈরি হওয়া ফাইল:

- `~/.ssh/id_ed25519` → **Private Key** (কখনো শেয়ার করবেন না)
- `~/.ssh/id_ed25519.pub` → **Public Key** (এটা সার্ভারে যাবে)

#### Public Key সার্ভারে পাঠানোর সহজ উপায়

```bash
# নিজের পিসি থেকে
ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 22 sakil@TOMAR_SERVER_IP
```

যদি `ssh-copy-id` কাজ না করে (বিশেষ করে Windows-এ), তাহলে ম্যানুয়ালি করুন:

```bash
# ১. নিজের পিসিতে Public Key দেখুন
cat ~/.ssh/id_ed25519.pub

# ২. সার্ভারে লগইন করে
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
# এখানে Public Key-এর পুরো লাইন পেস্ট করুন

# ৩. পারমিশন ঠিক করুন
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

**টিপ**
একাধিক ডিভাইস থেকে লগইন করতে চাইলে প্রতিটি ডিভাইসের Public Key আলাদা লাইনে `authorized_keys` ফাইলে যোগ করুন।

### ৪. SSH Hardening (খুবই গুরুত্বপূর্ণ)

এখন SSH কনফিগারেশন ফাইল এডিট করুন:

```bash
sudo nano /etc/ssh/sshd_config
```

নিচের লাইনগুলো খুঁজে বের করে (বা যোগ করে) এইভাবে সেট করুন:

```bash
Port 2222                          # ডিফল্ট ২২ পরিবর্তন করুন
PermitRootLogin no                 # root লগইন সম্পূর্ণ বন্ধ
PasswordAuthentication no          # পাসওয়ার্ড লগইন বন্ধ
PubkeyAuthentication yes           # Key-based লগইন চালু
PermitEmptyPasswords no
MaxAuthTries 3
LoginGraceTime 30
ClientAliveInterval 300
ClientAliveCountMax 2
```

সেভ করে বেরিয়ে আসুন (`Ctrl+O` → Enter → `Ctrl+X`), তারপর রিস্টার্ট করুন:

```bash
sudo systemctl restart sshd
# অথবা নতুন নামে
sudo systemctl restart ssh
```

**⚠ অত্যন্ত গুরুত্বপূর্ণ সতর্কতা**
`PasswordAuthentication no` করার আগে **অবশ্যই** নতুন টার্মিনাল খুলে Key দিয়ে লগইন টেস্ট করুন।
যদি Key কাজ না করে এবং পাসওয়ার্ডও বন্ধ করে দেন, তাহলে সার্ভারে আর ঢুকতে পারবেন না (Physical Console ছাড়া)।

নতুন পোর্টে লগইন করার কমান্ড:

```bash
ssh -p 2222 sakil@TOMAR_SERVER_IP
```

### ৫. Firewall (UFW) সেটআপ

UFW (Uncomplicated Firewall) দিয়ে শুধু প্রয়োজনীয় পোর্ট খুলে দিন:

```bash
sudo apt install ufw -y          # যদি না থাকে

# SSH পোর্ট (নতুন পোর্ট)
sudo ufw allow 2222/tcp

# ওয়েব সার্ভারের জন্য
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# চাইলে নির্দিষ্ট আইপি থেকে শুধু SSH অনুমতি দিতে পারেন (আরও নিরাপদ)
# sudo ufw allow from 123.45.67.89 to any port 2222

# ফায়ারওয়াল চালু
sudo ufw enable

# স্ট্যাটাস দেখা
sudo ufw status verbose
```

**সতর্কতা**
UFW চালু করার আগে নিশ্চিত হোন যে SSH পোর্ট (2222) allow করা আছে। না হলে নিজেকেই লক আউট করে ফেলবেন।

### ৬. Fail2Ban ইন্সটল ও কনফিগার

Fail2Ban বারবার ভুল লগইন চেষ্টা করলে সেই আইপিকে অটোমেটিক ব্লক করে দেয়।

```bash
sudo apt install fail2ban -y
sudo systemctl enable --now fail2ban
```

ডিফল্ট কনফিগারেশনই বেশ ভালো কাজ করে। আরও কঠোর করতে চাইলে:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local
```

`[sshd]` সেকশনে এইগুলো সেট করতে পারেন:

```bash
[sshd]
enabled = true
port = 2222
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 3600          # ১ ঘণ্টা
findtime = 600          # ১০ মিনিটের মধ্যে
```

```bash
sudo systemctl restart fail2ban
sudo fail2ban-client status sshd
```

### ৭. টাইমজোন ঠিক করা

```bash
sudo timedatectl set-timezone Asia/Dhaka
timedatectl                          # যাচাই করুন
```

টাইমজোন ঠিক না থাকলে লগ ফাইল পড়তে অসুবিধা হয় এবং অনেক অ্যাপ্লিকেশনে সমস্যা তৈরি করে।

### ৮. অটোমেটিক সিকিউরিটি আপডেট

```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure -plow unattended-upgrades
```

এতে সিকিউরিটি আপডেট অটোমেটিক ইন্সটল হবে। চাইলে আরও কনফিগার করতে পারেন:

```bash
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
```

---

## পর্ব ৩: systemctl বিস্তারিত গাইড

`systemctl` হলো systemd-এর প্রধান ম্যানেজমেন্ট টুল। পুরনো `service` কমান্ডের জায়গা এখন `systemctl` নিয়েছে।

| কমান্ড | কাজ |
| --- | --- |
| `sudo systemctl status ssh` | সার্ভিস চলছে কিনা ও বিস্তারিত তথ্য দেখা |
| `sudo systemctl start ssh` | এখনই চালু করা (রিবুটের পর বন্ধ হয়ে যেতে পারে) |
| `sudo systemctl stop ssh` | এখনই বন্ধ করা |
| `sudo systemctl restart ssh` | বন্ধ করে আবার চালু করা |
| `sudo systemctl reload ssh` | সার্ভিস বন্ধ না করে শুধু কনফিগ রিলোড করা |
| `sudo systemctl enable ssh` | সার্ভার বুট হলে অটো চালু হবে |
| `sudo systemctl disable ssh` | অটো চালু বন্ধ করা |
| `sudo systemctl enable --now ssh` | এখনই চালু + বুটে অটো চালু (সবচেয়ে বেশি ব্যবহার হয়) |
| `sudo systemctl is-enabled ssh` | enable করা আছে কিনা চেক |
| `sudo systemctl is-active ssh` | এখন চলছে কিনা চেক |

### enable vs start — পার্থক্য বুঝুন

- **start** → শুধু এখনই চালু করে। রিবুট দিলে বন্ধ হয়ে যাবে।
- **enable** → বুটের সময় অটো চালু হওয়ার জন্য সেট করে। এখনই চালু করে না।
- **enable --now** → দুটোই একসাথে করে। এটাই সবসময় ব্যবহার করবেন।

### লগ দেখা (সমস্যা ডিবাগ করার জন্য)

```bash
# রিয়েল-টাইম লগ
sudo journalctl -u ssh -f

# শেষ ১০০ লাইন
sudo journalctl -u ssh -n 100

# আজকের লগ
sudo journalctl -u ssh --since today

# fail2ban লগ
sudo journalctl -u fail2ban -f
```

### আপনার সার্ভারে এই তিনটি সার্ভিস সবসময় enable থাকতে হবে

```bash
sudo systemctl enable --now ssh
sudo systemctl enable --now ufw
sudo systemctl enable --now fail2ban
```

---

## শেষ চেকলিস্ট — প্রোডাকশনে যাওয়ার আগে

- সিস্টেম সম্পূর্ণ আপডেট করা হয়েছে (`apt update && apt upgrade`)
- নতুন sudo ইউজার তৈরি করা হয়েছে এবং root লগইন বন্ধ
- SSH Key দিয়ে লগইন সফলভাবে কাজ করছে
- PasswordAuthentication বন্ধ করা হয়েছে
- SSH পোর্ট ডিফল্ট ২২ থেকে পরিবর্তন করা হয়েছে
- UFW চালু এবং প্রয়োজনীয় পোর্ট allow করা হয়েছে
- Fail2Ban চালু এবং কাজ করছে
- টাইমজোন Asia/Dhaka সেট করা হয়েছে
- unattended-upgrades চালু করা হয়েছে
- ssh, ufw, fail2ban — তিনটিই `enable --now` করা হয়েছে

**অভিনন্দন!**
এই চেকলিস্ট সম্পন্ন হলে আপনার লিনাক্স সার্ভার প্রোডাকশনের জন্য প্রায় **৯০% নিরাপদ**।
এরপর আপনি নিশ্চিন্তে Nginx, Docker, Laravel, Node.js বা যেকোনো অ্যাপ্লিকেশন ডিপ্লয়মেন্ট শুরু করতে পারেন।

