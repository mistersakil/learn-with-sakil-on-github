> [🏠](../) [⬅️ ০৩। ডেভঅপস ডেভেলপমেন্ট এনভায়রনমেন্ট](../০৩-ডেভঅপস-ডেভেলপমেন্ট-এনভায়রনমেন্ট) [➡️ ০৫। লিনাক্স থিম কাস্টমাইজেশন](../০৫-লিনাক্স-থিম-কাস্টমাইজেশন)

# ০৪। wget-এর ব্যবহার

`wget` শেখার সবচেয়ে ভালো উপায় হলো বাস্তব একটি ফাইল ডাউনলোড করা। এই অধ্যায়ে প্রথমে `wget` কী, কেন ব্যবহার করা হয় তা জানব, তারপর Google Chrome ডাউনলোডের মাধ্যমে এর ব্যবহার শিখব।

---

## সূচিপত্র

1. wget কী?
2. wget ইনস্টল করা
3. wget-এর মৌলিক ব্যবহার
4. Google Chrome ডাউনলোড উদাহরণ
5. ডাউনলোড করা ফাইল যাচাই
6. নির্দিষ্ট ফোল্ডারে ফাইল সংরক্ষণ
7. সাধারণ সমস্যা ও সমাধান

---

## ১. wget কী?

`wget` হলো Linux-এর একটি Command-Line Download Utility।

**Full Meaning:** World Wide Web Get

এর মাধ্যমে টার্মিনাল থেকে সরাসরি ইন্টারনেটের ফাইল ডাউনলোড করা যায়।

### wget কেন ব্যবহার করা হয়?

- সফটওয়্যার ডাউনলোড করতে
- স্ক্রিপ্টের মাধ্যমে স্বয়ংক্রিয় ডাউনলোড করতে
- সার্ভারে GUI ছাড়া কাজ করতে
- বড় ফাইল Resume করে ডাউনলোড করতে
- নির্দিষ্ট URL থেকে ফাইল সংগ্রহ করতে

### Syntax

```bash
wget URL
```

উদাহরণ:

```bash
wget https://example.com/file.zip
```

## ২. wget ইনস্টল করা

Ubuntu-তে wget না থাকলে:

```bash
sudo apt update
sudo apt install wget
```

ইনস্টল হয়েছে কিনা যাচাই করুন:

```bash
wget --version
```

## ৩. wget-এর মৌলিক ব্যবহার

একটি ওয়েব ফাইল ডাউনলোড:

```bash
wget https://example.com/archive.zip
```

বর্তমান ডিরেক্টরিতে ফাইলটি সংরক্ষিত হবে।

ফাইলগুলো দেখতে:

```bash
ls -lh
```

## ৪. Google Chrome ডাউনলোড উদাহরণ

এখন একটি বাস্তব উদাহরণ দেখি। Ubuntu-এর জন্য Google Chrome `.deb` প্যাকেজ ডাউনলোড করতে:

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

wget URL থেকে ফাইলটি নামিয়ে বর্তমান ডিরেক্টরিতে সংরক্ষণ করবে।

## ৫. ডাউনলোড করা ফাইল যাচাই

ডাউনলোড সফল হয়েছে কিনা দেখুন:

```bash
ls -lh
```

প্রত্যাশিত আউটপুট:

```text
google-chrome-stable_current_amd64.deb
```

ফাইলের বিস্তারিত তথ্য:

```bash
file google-chrome-stable_current_amd64.deb
```

## ৬. নির্দিষ্ট ফোল্ডারে ফাইল সংরক্ষণ

ধরা যাক আপনি `software` ফোল্ডারে Chrome প্যাকেজ রাখতে চান।

```bash
mkdir -p ~/software
```

ফাইল সরিয়ে নিন:

```bash
mv google-chrome-stable_current_amd64.deb ~/software/
```

যাচাই করুন:

```bash
ls ~/software
```

তারপর:

```bash
cd ~/software
```

Chrome ইনস্টল করুন:

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

ইনস্টলেশন যাচাই:

```bash
google-chrome --version
```

## ৭. সাধারণ সমস্যা ও সমাধান

### wget: command not found

```bash
sudo apt update
sudo apt install wget
```

### Broken Dependency Error

```bash
sudo apt --fix-broken install
```

তারপর পুনরায়:

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

### Chrome চালু করা

```bash
google-chrome
```

অথবা:

```bash
google-chrome &
```

---

## সারসংক্ষেপ

এই অধ্যায়ে আপনি শিখেছেন:

- `wget` কী
- `wget` দিয়ে ফাইল ডাউনলোড করা
- Google Chrome `.deb` প্যাকেজ নামানো
- ডাউনলোড করা ফাইল যাচাই করা
- অন্য ফোল্ডারে সরানো
- Chrome ইনস্টল করা

পরবর্তী অধ্যায়ে আমরা আরও বাস্তব Linux Command ব্যবহার করব।

> [🏠](../) [⬅️ ০৩। ডেভঅপস ডেভেলপমেন্ট এনভায়রনমেন্ট](../০৩-ডেভঅপস-ডেভেলপমেন্ট-এনভায়রনমেন্ট) [➡️ ০৫। লিনাক্স থিম কাস্টমাইজেশন](../০৫-লিনাক্স-থিম-কাস্টমাইজেশন)ss