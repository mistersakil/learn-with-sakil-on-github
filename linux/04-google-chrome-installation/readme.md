# 🐧 উবুন্টুতে (Ubuntu) টার্মিনালের মাধ্যমে গুগল ক্রোম ইন্সটলেশন এবং ফাইল ম্যানেজমেন্ট গাইড

এই গাইডটিতে উবুন্টু অপারেটিং সিস্টেমে টার্মিনাল ব্যবহার করে গুগল ক্রোম (Google Chrome) ব্রাউজার ডাউনলোড, ইন্সটল এবং ভুল লোকেশনে ডাউনলোড হওয়া ফাইল সঠিক ফোল্ডারে স্থানান্তরের সম্পূর্ণ প্রক্রিয়া সহজ বাংলায় আলোচনা করা হয়েছে।

---

## 🚀 ১. গুগল ক্রোম ইন্সটল করার মূল কমান্ড

```bash
wget https://google.com
sudo apt install ./google-chrome-stable_current_amd64.deb
```

## 🧠 `wget` কী এবং কেন প্রয়োজন?

- **Full Meaning:** World Wide Web Get
- ইন্টারনেট থেকে টার্মিনালের মাধ্যমে ফাইল ডাউনলোড করার জন্য ব্যবহৃত একটি কমান্ড-লাইন টুল।
- রিজিউমেবল ডাউনলোড, অটোমেশন এবং স্ক্রিপ্টিংয়ের জন্য খুবই উপকারী।

---

## 📁 ২. ভুল লোকেশন থেকে ফাইল সঠিক ডিরেক্টরিতে স্থানান্তর

```bash
mkdir -p ~/software
mv ~/.ssh/google-chrome-stable_current_amd64.deb ~/software/
cd ~/software && ls
```

---

## 🔍 ৩. কমান্ডগুলোর ব্যাখ্যা

### `mkdir -p ~/software`
- `mkdir` = নতুন ফোল্ডার তৈরি করে
- `-p` = প্রয়োজন হলে প্যারেন্ট ফোল্ডারও তৈরি করে
- `~` = ইউজারের Home Directory

### `mv ~/.ssh/google-chrome-stable_current_amd64.deb ~/software/`
- `mv` = ফাইল বা ফোল্ডার স্থানান্তর করে
- প্রথম অংশ Source Location
- দ্বিতীয় অংশ Destination Location

### `cd ~/software && ls`
- `cd` = ডিরেক্টরি পরিবর্তন
- `&&` = প্রথম কমান্ড সফল হলে দ্বিতীয়টি চালায়
- `ls` = বর্তমান ডিরেক্টরির ফাইল তালিকা দেখায়

---

## 💻 ৪. নতুন লোকেশন থেকে ক্রোম ইন্সটল ও রান করা

```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
google-chrome
```

---

💡 **টিপস:** `sudo apt update && sudo apt upgrade` নিয়মিত চালালে Chrome-সহ সিস্টেমের অন্যান্য প্যাকেজও আপডেট থাকবে।