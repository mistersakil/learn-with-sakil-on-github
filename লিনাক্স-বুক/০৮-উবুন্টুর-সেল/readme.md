> [🏠](../) [⬅️ ০৭। কমান্ড লাইন অপশন](../০৭-কমান্ড-লাইন-অপশন) [➡️  ০৮। উবুন্টুর সেল](../০৮-উবুন্টুর-সেল)

# ০৮। উবুন্টুর সেল : Ubuntu-তে Shell-এর ধরন, Installation ও Configuration

> Ubuntu বা অন্যান্য Linux Distribution-এ Terminal ব্যবহারের মূল মাধ্যম হলো **Shell**।
> Shell ব্যবহারকারী এবং Linux Kernel-এর মধ্যে একটি Interface হিসেবে কাজ করে।
> আপনি Terminal-এ যে Command লিখেন, Shell সেটিকে Kernel-এর কাছে পাঠায় এবং ফলাফল আবার আপনার কাছে প্রদর্শন করে।

এই অধ্যায়ে আমরা Ubuntu-তে ব্যবহৃত জনপ্রিয় Shell, সেগুলোর Installation, Default Shell পরিবর্তন এবং Basic Configuration সম্পর্কে বিস্তারিত জানব।

## সূচিপত্র

* [Shell কী?](#shell-কী)
* [Ubuntu-তে জনপ্রিয় Shell-এর ধরন](#ubuntu-তে-জনপ্রিয়-shell-এর-ধরন)
* [বর্তমান Shell চেক করার উপায়](#বর্তমান-shell-চেক-করার-উপায়)
* [Installed Shell-এর তালিকা দেখার উপায়](#installed-shell-এর-তালিকা-দেখার-উপায়)
* [Zsh Installation](#zsh-installation)
* [Fish Installation](#fish-installation)
* [Default Shell পরিবর্তন](#default-shell-পরিবর্তন)
* [Temporary Shell Testing](#temporary-shell-testing)
* [Comparison Table](#comparison-table)
* [Summary](#summary)

---

## Shell কী?

Linux-এ Shell হলো একটি **Command Interpreter**।

ব্যবহারকারী Terminal-এ Command লিখলে Shell সেটি বুঝে Operating System-এর Kernel-এর কাছে পাঠায় এবং ফলাফল প্রদর্শন করে।

```text
User
  ↓
Shell
  ↓
Kernel
  ↓
Hardware
```

সহজভাবে বললে — Shell হলো Linux-এর সাথে কথা বলার ভাষা।

---

## Ubuntu তে জনপ্রিয় shell এর ধরন

Ubuntu-তে একাধিক Shell ব্যবহার করা যায়।

### Bash (Bourne Again Shell)

#### পরিচিতি

Ubuntu-এর Default Shell।

Terminal খুললেই সাধারণত Bash চালু হয়।

#### বৈশিষ্ট্য

* Ubuntu-এর Default Shell
* Stable এবং Mature
* Shell Scripting-এর জন্য সবচেয়ে জনপ্রিয়
* Tab Completion Support
* Command History Support
* প্রায় সব Linux System-এ পাওয়া যায়

#### ব্যবহার

* Server Administration
* DevOps
* Automation
* Shell Script Development


### 🔹 Dash (Debian Almquist Shell)

#### পরিচিতি

Ubuntu-এর `/bin/sh` সাধারণত Dash-এর দিকে Point করে।

#### বৈশিষ্ট্য

* Bash-এর তুলনায় Lightweight
* অত্যন্ত দ্রুত
* কম Memory ব্যবহার করে
* Interactive Feature কম

#### ব্যবহার

* System Boot Process
* Startup Script
* Internal System Task


### 🔹 Zsh (Z Shell)

#### পরিচিতি

বর্তমান সময়ের সবচেয়ে জনপ্রিয় Developer Shell।

#### বৈশিষ্ট্য

* Bash Compatible
* Advanced Auto Completion
* Command Correction
* Plugin Support
* Theme Support
* Oh My Zsh Integration

#### ব্যবহার

* Software Development
* DevOps
* Power Users
* Terminal Customization

### Fish (Friendly Interactive Shell)

#### পরিচিতি

একটি Modern এবং User-Friendly Shell।

#### বৈশিষ্ট্য

* Intelligent Auto Suggestion
* Syntax Highlighting
* Rich Completion
* Beginner Friendly
* Built-in Configuration

#### ব্যবহার

* Daily Terminal Usage
* New Linux Users
* Productivity Workflow

### Sh (Bourne Shell)

#### পরিচিতি

Unix-এর ঐতিহাসিক Shell।

### #বৈশিষ্ট্য

* Minimal Feature Set
* Legacy Compatibility
* POSIX Standard Reference

#### ব্যবহার

* Legacy Script
* Cross-Platform Compatibility

## বর্তমান Shell চেক করার উপায়

বর্তমানে কোন Shell ব্যবহার করছেন তা দেখতে:

```bash
echo $SHELL
```

উদাহরণ:

```bash
/bin/bash
```

অথবা:

```bash
/usr/bin/zsh
```

---

## Installed Shell-এর তালিকা দেখার উপায়

Linux System-এ কোন কোন Shell Available আছে তা দেখতে:

```bash
cat /etc/shells
```

উদাহরণ:

```text
/bin/sh
/bin/bash
/bin/rbash
/usr/bin/bash
/usr/bin/zsh
/usr/bin/fish
```

## Zsh Installation

### Step 1: Package Index Update

```bash
sudo apt update
```

### Step 2: Install Zsh

```bash
sudo apt install zsh -y
```

### Step 3: Verify Installation

```bash
zsh --version
```

উদাহরণ:

```text
zsh 5.9
```

### Step 4 (Optional): Install Oh My Zsh

Oh My Zsh হলো Zsh-এর সবচেয়ে জনপ্রিয় Framework।

Official Installation:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Fish Installation

### Step 1: Repository Update

```bash
sudo apt update
```

### Step 2: Install Fish

```bash
sudo apt install fish -y
```

### Step 3: Verify Installation

```bash
fish --version
```

উদাহরণ:

```text
fish, version 4.x
```

---

## Default Shell পরিবর্তন

### Step 1: Shell Path নিশ্চিত করুন

```bash
cat /etc/shells
```

সাধারণ Path:

| Shell | Path            |
| ----- | --------------- |
| Bash  | `/bin/bash`     |
| Zsh   | `/usr/bin/zsh`  |
| Fish  | `/usr/bin/fish` |


### Step 2: Shell পরিবর্তন করুন

#### Zsh-কে Default করতে

```bash
chsh -s /usr/bin/zsh
```

#### Fish-কে Default করতে

```bash
chsh -s /usr/bin/fish
```

#### Bash-এ ফিরে যেতে

```bash
chsh -s /bin/bash
```

### Step 3: Logout / Login

Terminal Restart করুন অথবা—

```text
Logout
    ↓
Login
```

---

## Temporary Shell Testing

কোনো Shell Permanent না করে শুধু Test করতে:

### Zsh

```bash
zsh
```

### Fish

```bash
fish
```

বর্তমান Shell থেকে বের হতে:

```bash
exit
```

---

## Comparison Table

| Feature              | Bash      | Dash | Zsh       | Fish     |
| -------------------- | --------- | ---- | --------- | -------- |
| Default Ubuntu Shell | ✅         | ❌    | ❌         | ❌        |
| Fast Startup         | ⚠️        | ✅    | ⚠️        | ⚠️       |
| Auto Completion      | ✅         | ❌    | ✅         | ✅        |
| Auto Suggestion      | ❌         | ❌    | Plugin    | ✅        |
| Theme Support        | Limited   | ❌    | ✅         | ✅        |
| Plugin System        | Limited   | ❌    | Excellent | Good     |
| Beginner Friendly    | Medium    | Low  | Medium    | High     |
| Scripting            | Excellent | Good | Excellent | Moderate |

---

## কোন Shell ব্যবহার করবেন?

### নতুন Linux User

**Fish**

কারণ:

* সহজ
* সুন্দর
* Auto Suggestion Built-in

### Developer

**Zsh + Oh My Zsh**

কারণ:

* Powerful
* Highly Customizable
* Productivity Features

### Server Administrator / DevOps Engineer

**Bash**

কারণ:

* Universal Support
* Stable
* Industry Standard

---

## Summary

এই অধ্যায়ে আমরা শিখলাম—

* Shell কী
* Bash, Dash, Zsh, Fish ও Sh-এর পার্থক্য
* Installed Shell Check করা
* Zsh Installation
* Fish Installation
* Default Shell পরিবর্তন
* Temporary Shell Testing

Linux Terminal দক্ষভাবে ব্যবহার করতে চাইলে Bash সম্পর্কে ভালো ধারণা থাকা জরুরি। আর Productivity ও Customization-এর জন্য Zsh এবং Fish বর্তমানে সবচেয়ে জনপ্রিয় পছন্দ।

> [🏠](../) [⬅️ ০৭। কমান্ড লাইন অপশন](../০৭-কমান্ড-লাইন-অপশন) [➡️  ০৮। উবুন্টুর সেল](../০৮-উবুন্টুর-সেল)
