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


###  Dash (Debian Almquist Shell)

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


### Zsh (Z Shell)

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

Zsh (Z Shell) হলো একটি শক্তিশালী এবং অত্যন্ত কাস্টমাইজযোগ্য কমান্ড-লাইন শেল। নিচে Zsh-এর ইতিহাস, ইন্সটলেশন, কনফিগারেশন এবং ব্যবহারের সমস্ত তথ্য বিস্তারিতভাবে দেওয়া হলো।

#### কেন Zsh ব্যবহার করবেন? (Key Features)

* Auto-completion: এটি অত্যন্ত স্মার্ট। ট্যাব (Tab) টিপলে কমান্ড, ফ্ল্যাগ এবং ফাইল পাথের অপশন দেখায় এবং অ্যারো কি (Arrow Keys) দিয়ে সিলেক্ট করা যায়।
* Spelling Correction: কমান্ড বানানে ভুল হলে এটি স্বয়ংক্রিয়ভাবে সঠিক কমান্ডটি সাজেস্ট করে।
* Shared History: একসাথে একাধিক টার্মিনাল উইন্ডো খোলা থাকলেও সবগুলোর হিস্ট্রি একসাথে শেয়ার হয়।
* Themes & Plugins: হাজার হাজার থিম এবং প্লাগইন ব্যবহার করে টার্মিনালের চেহারা ও কার্যক্ষমতা বদলে ফেলা যায়।

#### Zsh-কে ডিফল্ট শেল বানাতে(Changing to Zsh)
ইন্সটল করার পর Zsh-কে ডিফল্ট শেল বানাতে নিচের কমান্ডটি দিন:

chsh -s $(which zsh)

(নোট: এখানে $(which zsh) আপনার সিস্টেমে Zsh-এর সঠিক পাথটি স্বয়ংক্রিয়ভাবে খুঁজে নেয়। কমান্ডটি দেওয়ার পর আপনার ইউজার পাসওয়ার্ড দিতে হবে। এরপর টার্মিনাল বন্ধ করে আবার খুলুন।)

#### কনফিগারেশন এবং Oh My Zsh (Configuration)

Zsh-এর আসল ক্ষমতা প্রকাশ পায় এর কনফিগারেশনের মাধ্যমে। Zsh-এর মূল কনফিগারেশন ফাইলটি হলো আপনার হোম ডিরেক্টরিতে থাকা একটি হিডেন ফাইল, যার নাম .zshrc। Zsh-কে সহজে ম্যানেজ করার জন্য সবচেয়ে জনপ্রিয় ফ্রেমওয়ার্ক হলো Oh My Zsh। এটি ইন্সটল করলে টার্মিনাল দেখতে সুন্দর হয় এবং অনেক চমৎকার ফিচার চালু হয়।

### Oh My Zsh ইন্সটল করার কমান্ড:

আপনার দেওয়া কমান্ডটি 

```code
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### কমান্ডটির লাইন বাই লাইন (অংশ ধরে) ব্যাখ্যা
কমান্ডটি মূলত ৩টি প্রধান অংশে বিভক্ত। ভেতরের দিক থেকে শুরু করে বাইরের দিকে এর কাজ সম্পন্ন হয়:

##### ১. curl -fsSL https://.../install.sh (স্ক্রিপ্ট ডাউনলোড করা)

* curl: এটি টার্মিনালের একটি টুল, যা ইন্টারনেট থেকে ডেটা বা ফাইল ডাউনলোড বা আপলোড করতে ব্যবহৃত হয়।
* -fsSL: এগুলো হলো কার্ল-এর ৪টি ফ্ল্যাগ, যা ডাউনলোড প্রক্রিয়াটিকে নিরাপদ ও ব্যাকগ্রাউন্ডে সম্পন্ন করে:
* -f (Fail silently): সার্ভারে কোনো ত্রুটি (যেমন: ৪MD4 Error) থাকলে টার্মিনালে কোনো আবর্জনা আউটপুট না দেখিয়ে সরাসরি ফেইল করবে।
   * -s (Silent mode): ডাউনলোড করার সময় কোনো প্রোগ্রেস বার (কত পার্সেন্ট ডাউনলোড হলো) দেখাবে না। টার্মিনাল শান্ত থাকবে।
   * -S (Show error): সাইলেন্ট মুডে থাকলেও যদি কোনো বড় ধরনের ক্র্যাশ বা নেটওয়ার্ক এরর হয়, তবে সেটি স্ক্রিনে দেখাবে।
   * -L (Location/Redirect): নির্দিষ্ট করা লিঙ্কটি যদি অন্য কোনো লিঙ্কে রিডাইরেক্ট (স্থানান্তর) হয়, তবে কার্ল স্বয়ংক্রিয়ভাবে নতুন লিঙ্কটি অনুসরণ করবে।
* https://.../install.sh: এটি মূল স্ক্রিপ্ট ফাইলের ইউআরএল (URL), যা ইন্টারনেটে হোস্ট করা আছে।

##### ২. $( ... ) (কমান্ড সাবস্টিটিউশন)

* টার্মিনালের ভাষায় একে বলা হয় Command Substitution। এর কাজ হলো বন্ধনীর ভেতরে থাকা কমান্ডটি (এখানে curl দিয়ে ফাইল ডাউনলোড করা) আগে রান করা এবং ডাউনলোডের ফলে যে স্ক্রিপ্ট বা কোড পাওয়া যাবে, তা সরাসরি বাইরে পাঠিয়ে দেওয়া (ফাইল আকারে পিসিতে সেভ না করে)।
* 

##### ৩. sh -c (কোড এক্সিকিউট করা)

* sh: এটি হলো সিস্টেমের একদম বেসিক শেল (Bourne Shell)।
* -c (Command): এই ফ্ল্যাগটি সিস্টেমকে বলে যে, এর পরে কোনো ফাইলের নাম নেই, বরং সরাসরি কিছু "কমান্ড টেক্সট" বা স্ক্রিপ্ট দেওয়া হচ্ছে, যা তাৎক্ষণিকভাবে রান করতে হবে।
* পুরোটির সমন্বয়: sh -c তার পেছনে থাকা $(...) থেকে প্রাপ্ত সম্পূর্ণ ইন্সটলেশন কোডটিকে লুফে নেয় এবং আপনার কম্পিউটারে রান করিয়ে দেয়।

***এক লাইনে সারসংক্ষেপ:*** কমান্ডটি ইন্টারনেট থেকে ইন্সটলেশন স্ক্রিপ্টটি কোনো ফাইল হিসেবে হার্ডডিস্কে সেভ না করে, সরাসরি মেমোরিতে এনে রান করিয়ে দেয়। 

এই কমান্ডের $ এবং একদম শেষের - (ড্যাশ) চিহ্নের ভূমিকা খুবই গুরুত্বপূর্ণ। নিচে দুটির নিখুঁত ও সহজ ব্যাখ্যা দেওয়া হলো:


##### ১. $ চিহ্নের অর্থ ও কাজ
এই কমান্ডে ব্যবহৃত $( ... ) অংশটিকে লিনাক্সের ভাষায় Command Substitution (কমান্ড প্রতিস্থাপন) বলা হয়।

* কাজ: যখন টার্মিনাল কোনো কমান্ডের শুরুতে $ এবং বন্ধনী () দেখে, তখন সে বন্ধনীর ভেতরে থাকা কমান্ডটিকে (এখানে wget...) সবার আগে ব্যাকগ্রাউন্ডে রান করে।
* ফলাফল: বন্ধনীর ভেতরের কমান্ডটি রান হওয়ার পর যে আউটপুট বা কোড তৈরি হয়, $ চিহ্নটি সেই পুরো আউটপুটটিকে টেনে এনে বাইরের sh -c কমান্ডের কাছে জমা দেয়।

***সহজ উদাহরণ:***
মনে করুন আপনি লিখলেন echo $(date)। এখানে $ চিহ্নের কারণে আগে date কমান্ডটি রান হয়ে আজকের তারিখ ও সময় বের হবে, তারপর সেই সময়টি echo কমান্ডের মাধ্যমে স্ক্রিনে প্রিন্ট হবে। ঠিক একইভাবে, এখানে wget দিয়ে ডাউনলোড হওয়া কোডগুলো $ চিহ্নের মাধ্যমে sh কমান্ডের হাতে তুলে দেওয়া হচ্ছে।

##### ২. -O - অংশের শেষের - (ড্যাশ) চিহ্নের অর্থ ও কাজ
wget কমান্ডের সাধারণ স্বভাব হলো ইন্টারনেট থেকে কোনো ফাইল ডাউনলোড করে সেটি কম্পিউটারের হার্ডডিস্কে একটি নির্দিষ্ট ফাইল হিসেবে সেভ করা। কিন্তু আমরা এখানে ফাইলটি সেভ করতে চাই না, সরাসরি রান করতে চাই। এখানেই ম্যাজিক করে এই ড্যাশ চিহ্নটি।

* -O (Capital O): এটি একটি ফ্ল্যাগ, যার পূর্ণরূপ Output File। এর কাজ হলো wget-কে বলে দেওয়া যে, ডাউনলোড করা ফাইলটি কোন নামে বা কোথায় সেভ করতে হবে। যেমন: -O my_script.sh লিখলে ফাইলটি my_script.sh নামে সেভ হতো।
* শেষের - (Hyphen/Dash): লিনাক্স এবং ইউনিক্স সিস্টেমে এই একক ড্যাশ - চিহ্নের একটি বিশেষ অর্থ আছে। এর মানে হলো Standard Output (stdout)।

###### পুরোটার সমন্বয় (-O -):

যখন আপনি -O ফ্ল্যাগের পর কোনো ফাইলের নাম না দিয়ে একটি ড্যাশ - চিহ্ন বসিয়ে দেন, তখন আপনি wget-কে বলছেন— "ভাই, ফাইলটি কম্পিউটারের মেমোরিতে (হার্ডডিস্কে) কোনো ফাইল আকারে সেভ করো না। বরং ডাউনলোড হওয়া মাত্রই এর ভেতরের লেখা বা কোডগুলোকে সরাসরি টার্মিনালের স্ক্রিনে (Standard Output) ছুঁড়ে মারো।"

আর যখনই wget সেই কোডগুলোকে স্ক্রিনে ছুঁড়ে মারে, তখন ঠিক তার বাইরে ওত পেতে থাকা $( ) এবং sh -c সেই কোডগুলোকে লুফে নেয় এবং আপনার পিসিতে ইন্সটলেশন শুরু করে দেয়।

***এক লাইনে সারসংক্ষেপ:***
-O - এর শেষের ড্যাশটি ফাইলটিকে কম্পিউটারে সেভ হতে বাধা দিয়ে সরাসরি ওপেন করে দেয়, আর $ চিহ্নটি সেই ওপেন হওয়া কোডগুলোকে ইন্সটলারের (sh) কাছে চালান করে দেয়।
আপনার কি লিনাক্স বা ম্যাক টার্মিনালের পাইপলাইন (|) বা ইনপুট-আউটপুট রিডাইরেকশন নিয়ে আরও জানার আগ্রহ আছে, নাকি আপনি এখন Zsh থিম ও কাস্টমাইজেশনের দিকে এগোতে চান? আপনার পছন্দ জানালে সেই অনুযায়ী বিস্তারিত বলতে পারি!



#### এই কমান্ডের বিকল্পসমূহ (Alternatives)
যদি আপনার সিস্টেমে curl ইন্সটল করা না থাকে বা এই পদ্ধতি পছন্দ না হয়, তবে নিচের বিকল্পগুলো ব্যবহার করতে পারেন:

##### বিকল্প ১: Wget ব্যবহার করে (সবচেয়ে সাধারণ বিকল্প)

আপনার পিসিতে যদি curl-এর বদলে wget ডাউনলোড টুল থাকে, তবে এই কমান্ডটি দিতে পারেন:

```code
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

```code
bash -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"

```

এখানে -O - এর অর্থ হলো ফাইলটি পিসিতে সেভ না করে সরাসরি স্ক্রিনে আউটপুট (পাপিং) হিসেবে পাঠানো, যা পরবর্তীতে sh রান করে।

##### বিকল্প ২: ম্যানুয়াল ডাউনলোড ও রান (সবচেয়ে নিরাপদ পদ্ধতি)

ইন্টারনেট থেকে সরাসরি কোড রান করা অনেক সময় অনিরাপদ মনে হতে পারে। তাই আগে কোডটি পিসিতে ডাউনলোড করে দেখে নিয়ে তারপর রান করার নিয়ম:

   1. প্রথমে স্ক্রিপ্টটি ডাউনলোড করুন:
   
   ```code
   curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh 
   \ -o install.sh
   ```
   
   2. চাইলে nano install.sh লিখে কোডটি নিজে পড়ে দেখতে পারেন।
   3. এরপর স্ক্রিপ্টটিকে রান করুন:
   
   ```code 
   sh install.sh
   ```
   
   4. কাজ শেষ হলে ফাইলটি মুছে দিন: rm install.sh

#### বিকল্প ৩: গিট ক্লোন পদ্ধতি (Manual Git Clone)
কোনো স্ক্রিপ্ট রান না করে সরাসরি গিট রিপোজিটরি থেকে ডাউনলোড করে ম্যানুয়ালি সেটআপ করার নিয়ম:

##### ১. ওহ মাই জেডশেল ডিরেক্টরিতে ক্লোন করুন

```git
git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
```

##### ২. তাদের দেওয়া রেডিমেড টেমপ্লেটটি কনফিগারেশন ফাইল হিসেবে কপি করুন

```cp
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

#### গুরুত্বপূর্ণ কনফিগারেশন পরিবর্তন (How to configure):
কনফিগারেশন ফাইলটি এডিট করতে টার্মিনালে লিখুন:

nano ~/.zshrc


* থিম পরিবর্তন (Themes): ফাইলে ZSH_THEME="robbyrussell" অংশটি খুঁজে পাবেন। আপনি চাইলে এটিকে পরিবর্তন করে ZSH_THEME="agnoster" বা অন্য কোনো থিম দিতে পারেন।
* প্লাগইন যোগ করা (Plugins): ফাইলে plugins=(git) অংশটি খুঁজে পাবেন। এর ভেতরে স্পেস দিয়ে আরও প্লাগইন যোগ করতে পারেন, যেমন: plugins=(git docker python node).

কনফিগারেশন ফাইলে কোনো পরিবর্তন করার পর তা টার্মিনালে কার্যকর করতে এই কমান্ডটি দিতে হয়:

source ~/.zshrc


#### দুটি সেরা প্লাগইন (অবশ্যই ব্যবহার করা উচিত)
টার্মিনাল ব্যবহারের অভিজ্ঞতা সুপারফাস্ট করতে এই দুটি এক্সটার্নাল প্লাগইন ইন্সটল করে নিতে পারেন:
১. Zsh Syntax Highlighting (কমান্ড সঠিক হলে সবুজ, ভুল হলে লাল দেখাবে):

git clone https://github.com ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

২. Zsh Auto-suggestions (আগে টাইপ করা কমান্ড আবছাভাবে সাজেস্ট করবে):


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

### Details of chsh -s /usr/bin/zsh 

এই কমান্ডটি আপনার লিনাক্স (Linux) বা ম্যাক (macOS) টার্মিনালের ডিফল্ট শেল পরিবর্তন করে Zsh (Z Shell) সেট করে।
সহজ ভাষায় প্রতিটি অংশের বিস্তারিত ব্যাখ্যা নিচে দেওয়া হলো:

* chsh: এর পূর্ণরূপ হলো Change Shell। এটি অপারেটিং সিস্টেমের একটি বিল্ট-ইন কমান্ড, যা ব্যবহারকারীর বর্তমান লগইন শেল পরিবর্তন করতে ব্যবহৃত হয়।
* -s: এটি একটি ফ্ল্যাগ বা অপশন। এর পূর্ণরূপ হলো Shell। chsh কমান্ডকে নির্দেশ দেওয়ার জন্য এটি ব্যবহার করা হয় যে, আপনি নতুন কোনো শেল পাথ (Path) সেট করতে যাচ্ছেন।
* /usr/bin/zsh: এটি হচ্ছে আপনার সিস্টেমে থাকা Zsh শেলের মূল লোকেশন বা ফাইল পাথ (Absolute Path)। এই পাথের ফাইলটি রান করার মাধ্যমেই Zsh চালু হয়।

সারসংক্ষেপ: পুরো কমান্ডটির মানে দাঁড়ায়— "আমার বর্তমান টার্মিনাল শেল পরিবর্তন (chsh) করে নতুন শেল হিসেবে (-s) জিশেলকে (/usr/bin/zsh) ডিফল্ট হিসেবে সেট করো।"
প্রয়োজনীয় টিপস:

* কমান্ডটি কার্যকর করার পর পরিবর্তনটি দেখতে টার্মিনাল পুরোপুরি বন্ধ করে আবার চালু করতে হবে।
* কিছু সিস্টেমে নিরাপত্তার জন্য এই কমান্ডটি দেওয়ার পর আপনার কম্পিউটারের পাসওয়ার্ডটি টাইপ করতে হতে পারে।
* টার্মিনালে echo $SHELL লিখে রান করে নিশ্চিত হতে পারেন যে এটি পরিবর্তন হয়েছে কি না।

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
