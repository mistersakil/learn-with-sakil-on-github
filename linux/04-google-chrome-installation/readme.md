# 🐧 Ubuntu-তে Google Chrome ইন্সটলেশন এবং ফাইল ম্যানেজমেন্ট গাইড

এই গাইডে Ubuntu Linux-এ টার্মিনাল ব্যবহার করে Google Chrome ডাউনলোড, ইন্সটল এবং ডাউনলোড করা `.deb` ফাইল সঠিক লোকেশনে ম্যানেজ করার পদ্ধতি আলোচনা করা হয়েছে।

---

# 📚 সূচিপত্র

1. Google Chrome ডাউনলোড
2. Chrome ইন্সটলেশন
3. `wget` কী?
4. ভুল লোকেশনে ডাউনলোড হওয়া ফাইল সরানো
5. Chrome চালু করা
6. Chrome আপডেট রাখা
7. Troubleshooting

---

# 🚀 ১. Google Chrome ডাউনলোড

Google Chrome-এর অফিসিয়াল `.deb` প্যাকেজ ডাউনলোড করতে নিচের কমান্ড ব্যবহার করুন:

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

ফাইলটি বর্তমান ডিরেক্টরিতে ডাউনলোড হবে।

ডাউনলোড হয়েছে কিনা যাচাই করতে:

```bash
ls -lh
```

---

# 📦 ২. Google Chrome ইন্সটলেশন

ডাউনলোড সম্পন্ন হলে:

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

Ubuntu প্রয়োজনীয় dependency স্বয়ংক্রিয়ভাবে ইন্সটল করবে।

ইন্সটলেশন যাচাই করতে:

```bash
google-chrome --version
```

---

# 🧠 ৩. `wget` কী?

`wget` হলো Linux-এর একটি জনপ্রিয় Command-Line Download Utility।

### Full Meaning

**World Wide Web Get**

### ব্যবহার

- ইন্টারনেট থেকে ফাইল ডাউনলোড
- ব্যাকগ্রাউন্ড ডাউনলোড
- Resume Support
- Automation Scripts
- Server Management

---

# 📁 ৪. ভুল লোকেশনে ডাউনলোড হওয়া ফাইল সরানো

ধরা যাক Chrome প্যাকেজ ভুলবশত `.ssh` ফোল্ডারে ডাউনলোড হয়েছে।

```text
~/.ssh/google-chrome-stable_current_amd64.deb
```

## Step 1: নতুন ফোল্ডার তৈরি

```bash
mkdir -p ~/software
```

## Step 2: ফাইল স্থানান্তর

```bash
mv ~/.ssh/google-chrome-stable_current_amd64.deb ~/software/
```

## Step 3: ফাইল যাচাই

```bash
cd ~/software && ls
```

প্রত্যাশিত আউটপুট:

```text
google-chrome-stable_current_amd64.deb
```

---

# 💻 ৫. নতুন লোকেশন থেকে Chrome ইন্সটল

```bash
cd ~/software
sudo apt install ./google-chrome-stable_current_amd64.deb
```

---

# 🌐 ৬. Chrome চালু করা

```bash
google-chrome
```

অথবা

```bash
google-chrome &
```

---

# 🔄 ৭. Chrome আপডেট রাখা

```bash
sudo apt update && sudo apt upgrade
```

Chrome ইন্সটল হওয়ার সময় Google Repository স্বয়ংক্রিয়ভাবে যুক্ত হয়, তাই সিস্টেম আপডেট করলেই Chrome-ও আপডেট হবে।

---

# 🛠️ Troubleshooting

### `wget: command not found`

```bash
sudo apt update
sudo apt install wget
```

### Dependency Error

```bash
sudo apt --fix-broken install
```

তারপর আবার:

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

### Chrome ওপেন না হলে

```bash
google-chrome --disable-gpu
```

---

# ✅ উপসংহার

এই গাইড অনুসরণ করে আপনি Google Chrome ডাউনলোড, ইন্সটল, আপডেট এবং `.deb` প্যাকেজ ম্যানেজমেন্ট সম্পর্কে বাস্তব অভিজ্ঞতা অর্জন করতে পারবেন। এটি Linux Beginner থেকে Intermediate পর্যায়ের ব্যবহারকারীদের জন্য একটি গুরুত্বপূর্ণ প্র্যাকটিক্যাল গাইড।