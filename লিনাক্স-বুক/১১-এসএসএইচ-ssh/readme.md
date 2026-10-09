# অধ্যায়: SSH (Secure Shell) — নিরাপদ রিমোট অ্যাক্সেসের পূর্ণাঙ্গ গাইড

> **লক্ষ্য:** এই অধ্যায় শেষে আপনি SSH কী, কেন প্রয়োজন, কীভাবে কাজ করে, কীভাবে কনফিগার ও ব্যবহার করবেন, টানেলিং, জাম্প হোস্ট, হার্ডেনিং এবং GitHub-এ SSH-এর প্রোডাকশন-গ্রেড ব্যবহার সম্পর্কে পূর্ণ ধারণা পাবেন।

---

## সূচিপত্র

1. ভূমিকা ও ইতিহাস
2. SSH কী? কেন Telnet/FTP বাদ দেবেন?
3. SSH এর আর্কিটেকচার - ৩টি লেয়ার
4. SSH হ্যান্ডশেক - ভেতরে কী ঘটে?
5. অথেনটিকেশন মেথডসমূহ
6. SSH কী টাইপ - Ed25519 vs RSA vs ECDSA
7. SSH কী জেনারেট - `ssh-keygen` ডিপ ডাইভ
8. ফাইল স্ট্রাকচার ও পারমিশন
9. SSH এজেন্ট - `ssh-agent` ও `ssh-add`
10. SSH ক্লায়েন্ট কনফিগ - `~/.ssh/config` এর জাদু
11. সার্ভারে কী ইনস্টল করা - `ssh-copy-id` ও `authorized_keys`
12. সার্ভার কনফিগ - `/etc/ssh/sshd_config`
13. GitHub-এ SSH - সিঙ্গেল ও মাল্টিপল অ্যাকাউন্ট
14. ফাইল ট্রান্সফার - SCP, SFTP, Rsync over SSH
15. SSH টানেলিং - Local (-L), Remote (-R), Dynamic (-D)
16. জাম্প হোস্ট ও Bastion - `ProxyJump`
17. প্রোডাক্টিভিটি টিপস - Multiplexing, KeepAlive, Debugging
18. SSH হার্ডেনিং ও নিরাপত্তা
19. ট্রাবলশুটিং - কমন এরর ও সমাধান
20. সুবিধা-অসুবিধা
21. সারাংশ ও অনুশীলনী

---

### ১। ভূমিকা ও ইতিহাস

১৯৯৫ সালে **Tatu Ylönen** ফিনল্যান্ডে SSH প্রোটোকল তৈরি করেন, কারণ তার বিশ্ববিদ্যালয়ের নেটওয়ার্কে পাসওয়ার্ড স্নিফিং হচ্ছিল। তখন রিমোট লগইনের জন্য Telnet, rlogin, rsh ব্যবহার হতো — সব প্লেইনটেক্সট।

- **SSH-1 (1995):** প্রথম ভার্সন, নিরাপত্তা ত্রুটি ছিল
- **SSH-2 (2006, RFC 4251-4254):** বর্তমান স্ট্যান্ডার্ড, সম্পূর্ণ নতুন ও নিরাপদ। আজ আমরা যা ব্যবহার করি তা SSH-2।

Linux-এ এর ওপেন সোর্স ইমপ্লিমেন্টেশন হলো **OpenSSH** (`ssh` ক্লায়েন্ট ও `sshd` সার্ভার)।

### ২। SSH কী?

**SSH (Secure Shell)** একটি ক্রিপ্টোগ্রাফিক নেটওয়ার্ক প্রোটোকল যা অনিরাপদ নেটওয়ার্কের উপর **এনক্রিপ্টেড, অথেনটিকেটেড ও ইন্টিগ্রিটি-প্রোটেক্টেড** চ্যানেল তৈরি করে।

**Telnet/FTP vs SSH:**

| বিষয় | Telnet / FTP | SSH |
|---|---|---|
| ডেটা ট্রান্সমিশন | প্লেইনটেক্সট | এনক্রিপ্টেড |
| পাসওয়ার্ড | নেটওয়ার্কে দৃশ্যমান | কখনোই প্লেইনটেক্সটে যায় না |
| ব্যবহার | শুধু লগইন/ফাইল | লগইন, ফাইল, টানেলিং, Git, পোর্ট ফরওয়ার্ড |

### ৩। SSH এর আর্কিটেকচার - ৩টি লেয়ার

RFC অনুযায়ী SSH এর ৩টি মূল লেয়ার:

1. **Transport Layer:** সার্ভার অথেনটিকেশন, কী এক্সচেঞ্জ (Diffie-Hellman), সিমেট্রিক এনক্রিপশন (AES, ChaCha20), MAC দিয়ে ইন্টিগ্রিটি। এখানেই Perfect Forward Secrecy নিশ্চিত হয়।
2. **User Authentication Layer:** ক্লায়েন্টকে প্রমাণ করে। মেথড: publickey, password, keyboard-interactive, gssapi।
3. **Connection Layer:** একটি এনক্রিপ্টেড টানেলকে একাধিক চ্যানেলে ভাগ করে — shell, exec, subsystem (sftp), TCP forwarding।

### ৪। SSH হ্যান্ডশেক কীভাবে কাজ করে?

প্রতিবার `ssh user@host` দিলে ভেতরে যা ঘটে:

1. **TCP Handshake:** ক্লায়েন্ট পোর্ট ২২ (ডিফল্ট) এ TCP কানেকশন করে।
2. **Version Exchange:** উভয় পক্ষ `SSH-2.0-OpenSSH_8.9` এর মতো ভার্সন স্ট্রিং বিনিময় করে।
3. **Algorithm Negotiation:** কোন Key Exchange, Host Key, Encryption, MAC ব্যবহার হবে তা ঠিক হয়। `ssh -Q key` দিয়ে লিস্ট দেখতে পারবেন।
4. **Key Exchange (KEX):** Diffie-Hellman দিয়ে একটি **Session Key** তৈরি হয়। এই কী কখনো নেটওয়ার্কে যায় না।
5. **Server Authentication:** সার্ভার তার Host Key দিয়ে নিজের পরিচয় প্রমাণ করে। ক্লায়েন্ট `~/.ssh/known_hosts` এ চেক করে।
6. **User Authentication:** publickey / password দিয়ে ইউজার প্রমাণ হয়।
7. **Encrypted Session:** এরপর সব ডেটা সিমেট্রিক এনক্রিপশনে (AES-256-GCM) চলে।

### ৫। অথেনটিকেশন মেথডসমূহ

| মেথড | কীভাবে কাজ করে | কখন ব্যবহার |
|---|---|---|
| **publickey** | প্রাইভেট কী দিয়ে চ্যালেঞ্জ সাইন করা | সবচেয়ে নিরাপদ, অটোমেশনের জন্য বাধ্যতামূলক |
| **password** | পাসওয়ার্ড এনক্রিপ্টেড চ্যানেলে যায় | সাময়িক, প্রোডাকশনে বন্ধ রাখা উচিত |
| **keyboard-interactive** | সার্ভার একাধিক প্রশ্ন করে (PAM, OTP) | MFA এর জন্য |
| **hostbased** | হোস্ট ভিত্তিক ট্রাস্ট | প্রায় ব্যবহার হয় না |

### ৬। কী টাইপ তুলনা

```bash
ssh -Q key
ssh -Q sig
```

| টাইপ | সাইজ | নিরাপত্তা | পারফরম্যান্স | সাপোর্ট |
|---|---|---|---|---|
| **Ed25519** | 256-bit | আধুনিক, 128-bit security | সবচেয়ে দ্রুত | OpenSSH 6.5+ (2014+) - **রেকমেন্ডেড** |
| **RSA** | 2048/4096-bit | নিরাপদ (4096 হলে) | ধীর | সব জায়গায় চলে |
| **ECDSA** | 256/384/521 | NIST কার্ভ, বিতর্ক আছে | দ্রুত | OpenSSH 5.7+ |

**সিদ্ধান্ত:** নতুন কী হলে সবসময় `ed25519`। শুধু খুব পুরোনো সার্ভার (CentOS 6, Cisco) এর জন্য RSA 4096।

### ৭। SSH কী জেনারেট - ডিপ ডাইভ

```bash
# সবচেয়ে আধুনিক ও রেকমেন্ডেড উপায়
ssh-keygen -t ed25519 -f ~/.ssh/github_ed25519 -C "sakil@laptop-2024"

# পুরোনো সার্ভারের জন্য
ssh-keygen -t rsa -b 4096 -f ~/.ssh/legacy_rsa -C "legacy@server"
```

**অপশন ব্যাখ্যা:**

- `-t`: টাইপ
- `-b`: বিট সাইজ (RSA এর জন্য)
- `-f`: ফাইলের পাথ। কাস্টম নাম দিলে একাধিক কী ম্যানেজ করা সহজ।
- `-C`: কমেন্ট, সাধারণত ইমেইল। কী চিনতে সুবিধা হয়।
- `-p`: পুরোনো কী-এর পাসফ্রেজ পরিবর্তন
- `-y`: প্রাইভেট কী থেকে পাবলিক কী বের করা

**পাসফ্রেজ:** অবশ্যই দিন। পাসফ্রেজ দিলে প্রাইভেট কী AES দিয়ে এনক্রিপ্টেড থাকে। `ssh-agent` ব্যবহার করলে বারবার দিতে হবে না।

### ৮। ফাইল স্ট্রাকচার ও পারমিশন - খুব গুরুত্বপূর্ণ

SSH পারমিশন ভুল হলে কাজ করবে না।

```bash
ls -la ~/.ssh/
# -rw------- 1 sakil sakil  464 github_ed25519      # প্রাইভেট কী - 600
# -rw-r--r-- 1 sakil sakil  103 github_ed25519.pub  # পাবলিক কী - 644
# -rw-r--r-- 1 sakil sakil  888 known_hosts
# -rw------- 1 sakil sakil 1200 config

# সঠিক পারমিশন সেট করা
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*_ed25519 ~/.ssh/id_rsa ~/.ssh/config
chmod 644 ~/.ssh/*.pub ~/.ssh/known_hosts
chmod 600 ~/.ssh/authorized_keys # সার্ভারে
```

### ৯। SSH Agent

এজেন্ট আপনার প্রাইভেট কী RAM-এ ডিক্রিপ্টেড অবস্থায় রাখে।

```bash
# এজেন্ট চালু
eval "$(ssh-agent -s)"
# Agent pid 1234

# কী যোগ করা
ssh-add ~/.ssh/github_ed25519
# Enter passphrase...

# লিস্ট দেখা
ssh-add -l

# সব কী মুছে ফেলা
ssh-add -D

# macOS / Ubuntu-তে স্থায়ী করতে ~/.bashrc বা ~/.zshrc তে যোগ করুন
```

**আধুনিক বিকল্প:** `keychain` বা Gnome Keyring ব্যবহার করলে লগআউটের পরেও মনে রাখে।

### ১০। SSH ক্লায়েন্ট কনফিগ - `~/.ssh/config`

বারবার `-i` লেখা বন্ধ করুন। এটি DevOps ইঞ্জিনিয়ারের সবচেয়ে শক্তিশালী টুল।

```ssh-config
# ~/.ssh/config

# GitHub Personal
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_ed25519
    IdentitiesOnly yes

# GitHub Work - মাল্টিপল অ্যাকাউন্টের জন্য
Host github-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_work_ed25519
    IdentitiesOnly yes

# Production Server
Host prod
    HostName 203.0.113.10
    User ubuntu
    Port 2222
    IdentityFile ~/.ssh/prod_ed25519

# সব হোস্টের জন্য কমন সেটিং
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    Compression yes
```

এখন শুধু `ssh prod` বা `ssh github.com` লিখলেই হবে!

**মাল্টিপল GitHub অ্যাকাউন্টের জন্য Git Clone:**

```bash
git clone git@github.com:personal/repo.git
git clone git@github-work:work-org/work-repo.git
```

### ১১। সার্ভারে কী ইনস্টল করা

**সহজ উপায় - `ssh-copy-id`:**

```bash
ssh-copy-id -i ~/.ssh/prod_ed25519.pub ubuntu@203.0.113.10
# এটি সার্ভারের ~/.ssh/authorized_keys এ পাবলিক কী যোগ করে দেবে
```

**ম্যানুয়াল উপায়:**

```bash
cat ~/.ssh/prod_ed25519.pub | ssh ubuntu@203.0.113.10 "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

### ১২। সার্ভার কনফিগ - `/etc/ssh/sshd_config`

প্রোডাকশন সার্ভারে অবশ্যই হার্ডেন করুন:

```bash
sudo nano /etc/ssh/sshd_config

Port 2222
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no
UsePAM yes
X11Forwarding no
AllowUsers ubuntu deploy
MaxAuthTries 3
LoginGraceTime 30

# পরিবর্তনের পর রিস্টার্ট
sudo systemctl restart sshd
sudo systemctl status sshd
```

### ১৩। GitHub-এ SSH - সম্পূর্ণ ফ্লো

**Step 1 - কী তৈরি ও এজেন্টে যোগ:**

```bash
ssh-keygen -t ed25519 -f ~/.ssh/github_ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/github_ed25519
```

**Step 2 - পাবলিক কী কপি:**

```bash
cat ~/.ssh/github_ed25519.pub
# বা Linux এ
xclip -sel clip < ~/.ssh/github_ed25519.pub
```

**Step 3 - GitHub এ যোগ:**
Settings -> SSH and GPG keys -> New SSH key -> Paste -> Add

**Step 4 - টেস্ট:**

```bash
ssh -T git@github.com
# Hi mistersakil! You've successfully authenticated...
```

**Step 5 - ব্যবহার:**

```bash
git clone git@github.com:mistersakil/learn-with-sakil-on-github.git
# পুরোনো HTTPS রিপোকে SSH এ কনভার্ট
git remote set-url origin git@github.com:mistersakil/repo.git
```

### ১৪। ফাইল ট্রান্সফার

```bash
# SCP - সহজ কপি
scp -i ~/.ssh/prod_ed25519 file.txt ubuntu@203.0.113.10:/home/ubuntu/
scp -r ./myapp ubuntu@203.0.113.10:/var/www/

# SFTP - ইন্টারেক্টিভ
sftp -i ~/.ssh/prod_ed25519 ubuntu@203.0.113.10
# sftp> put file.txt
# sftp> get file.txt

# Rsync over SSH - সবচেয়ে ভালো (Delta transfer)
rsync -avz -e "ssh -i ~/.ssh/prod_ed25519 -p 2222" ./myapp/ ubuntu@203.0.113.10:/var/www/myapp/
```

### ১৫। SSH টানেলিং - DevOps এর সুপার পাওয়ার

**1. Local Forwarding (-L):** লোকাল পোর্ট -> রিমোট সার্ভিস। রিমোট DB লোকালে দেখা।

```bash
# রিমোট MySQL (3306) কে লোকাল 3307 এ আনা
ssh -L 3307:localhost:3306 ubuntu@prod-server
# এখন localhost:3307 এ কানেক্ট করলে প্রোডাকশন DB
```

**2. Remote Forwarding (-R):** রিমোট পোর্ট -> লোকাল সার্ভিস। লোকাল সাইট পাবলিক দেখানো।

```bash
# লোকাল 3000 পোর্টকে সার্ভারের 9000 এ
ssh -R 9000:localhost:3000 ubuntu@prod-server
```

**3. Dynamic Forwarding (-D):** SOCKS Proxy।

```bash
ssh -D 1080 ubuntu@prod-server
# এখন ব্রাউজারে SOCKS Proxy localhost:1080 সেট করুন, সব ট্রাফিক সার্ভার দিয়ে যাবে
```

### ১৬। Jump Host / Bastion Host

প্রাইভেট নেটওয়ার্কের সার্ভারে ঢুকতে:

```bash
# পুরোনো উপায়
ssh -J ubuntu@bastion-host ubuntu@private-server

# আধুনিক ও রেকমেন্ডেড - ~/.ssh/config এ
Host private-server
    HostName 10.0.1.10
    User ubuntu
    IdentityFile ~/.ssh/prod_ed25519
    ProxyJump bastion-host

Host bastion-host
    HostName 203.0.113.5
    User ubuntu
    Port 2222
    IdentityFile ~/.ssh/prod_ed25519

# এখন শুধু
ssh private-server
```

### ১৭। প্রোডাক্টিভিটি টিপস

**Multiplexing:** একবার কানেকশন হলে পরেরবার দ্রুত।

```ssh-config
Host *
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h-%p
    ControlPersist 600
```

**Debugging:**

```bash
ssh -vvv -i ~/.ssh/key ubuntu@host # ৩টি v = ফুল ডিবাগ
```

**KeepAlive:** কানেকশন ঝুলে যাওয়া বন্ধ করে।

```ssh-config
ServerAliveInterval 60
```

### ১৮। SSH হার্ডেনিং - প্রোডাকশন চেকলিস্ট

**সার্ভার সাইড:**

1. `PermitRootLogin no`
2. `PasswordAuthentication no` - শুধু কী
3. পোর্ট পরিবর্তন: `Port 2222`
4. `AllowUsers` / `AllowGroups` দিয়ে লিমিট
5. Fail2Ban ইনস্টল:

    ```bash
    sudo apt install fail2ban -y
    sudo systemctl enable fail2ban
    #/etc/fail2ban/jail.local এ sshd = enabled
    ```

6. Firewall: `sudo ufw allow 2222/tcp` এবং `sudo ufw enable`
7. MFA: Google Authenticator PAM

**ক্লায়েন্ট সাইড:**

1. পাসফ্রেজ ব্যবহার করুন
2. প্রাইভেট কী কখনো শেয়ার করবেন না, Git-এ কমিট করবেন না, স্ক্রিনশট দেবেন না
3. `~/.ssh` পারমিশন 700/600 মেইনটেইন করুন
4. পুরোনো কী রোটেট করুন (প্রতি 6-12 মাস)
5. `ssh-audit` টুল দিয়ে সার্ভার চেক করুন

### ১৯। ট্রাবলশুটিং

| এরর | কারণ ও সমাধান |
|---|---|
| `Permission denied (publickey)` | পাবলিক কী `authorized_keys` এ নেই, পারমিশন ভুল, বা `IdentitiesOnly yes` নেই। `ssh -vvv` দিয়ে দেখুন |
| `WARNING: UNPROTECTED PRIVATE KEY FILE!` | `chmod 600` করুন |
| `Host key verification failed` | সার্ভার রি-ইনস্টল হয়েছে। `ssh-keygen -R hostname` দিয়ে পুরোনো এন্ট্রি মুছুন |
| `Too many authentication failures` | অনেক কী ট্রাই করছে। `IdentitiesOnly yes` ব্যবহার করুন |
| `Connection timed out` | ফায়ারওয়াল / পোর্ট ভুল / Security Group ব্লক |

### ২০। সুবিধা-অসুবিধা

**সুবিধা:** এন্ড-টু-এন্ড এনক্রিপশন, কী-ভিত্তিক অটোমেশন, সব প্ল্যাটফর্মে সাপোর্ট, টানেলিং, Git, SFTP, ওপেন সোর্স।
**অসুবিধা:** প্রাথমিক সেটআপ নতুনদের জন্য জটিল, প্রাইভেট কী হারালে ঝুঁকি, কর্পোরেট ফায়ারওয়াল পোর্ট 22 ব্লক করতে পারে।

### ২১। সারাংশ

SSH হলো Linux অ্যাডমিনিস্ট্রেশনের মেরুদণ্ড। Telnet এর মতো অনিরাপদ প্রোটোকল থেকে SSH এ আসা মানে প্লেইনটেক্সট থেকে এনক্রিপ্টেড দুনিয়ায় আসা। **Ed25519** কী, **ssh-agent**, **~/.ssh/config**, এবং **হার্ডেনিং** — এই চারটি আয়ত্ত করলে আপনি প্রোডাকশন-রেডি।

### ২২। অনুশীলনী

1. `ed25519` কী তৈরি করে `ssh-agent` এ যোগ করুন।
2. `~/.ssh/config` এ `github.com` ও `prod` নামে দুটি হোস্ট বানান।
3. `ssh-copy-id` ছাড়া ম্যানুয়ালি `authorized_keys` সেটআপ করুন।
4. Local Forwarding দিয়ে রিমোট MySQL লোকালে ফরওয়ার্ড করুন।
5. একটি Bastion Host দিয়ে প্রাইভেট সার্ভারে `ProxyJump` কনফিগার করুন।
6. `sshd_config` এ `PermitRootLogin no` ও `PasswordAuthentication no` সেট করে Fail2Ban চালু করুন।
7. দুটি ভিন্ন GitHub অ্যাকাউন্ট (personal/work) একই মেশিনে SSH দিয়ে কনফিগার করুন।
