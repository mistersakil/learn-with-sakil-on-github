ওয়েব সার্ভার হলো একটি বিশেষ সফটওয়্যার বা কম্পিউটার যা ইন্টারনেটে থাকা ওয়েবসাইট বা ওয়েব অ্যাপ্লিকেশনের ফাইলগুলো জমা রাখে এবং ব্যবহারকারীর অনুরোধ অনুযায়ী তা প্রদর্শন করে। সহজ কথায়, যখন আপনি ব্রাউজারে কোনো ওয়েবসাইটের ঠিকানা লিখে এন্টার চাপেন, তখন এই ওয়েব সার্ভারই আপনার স্ক্রিনে সেই ওয়েবসাইটের পেজটি এনে হাজির করে।
নিচে এটি কেন প্রয়োজন এবং কীভাবে কনফিগার করতে হয় তা সহজ ভাষায় ধাপে ধাপে আলোচনা করা হলো:
------------------------------
## 💻 ওয়েব সার্ভার কেন লাগে?
১. ফাইল সংরক্ষণ ও ব্যবস্থাপনা: ওয়েবসাইটের সব কোড, ইমেজ, ভিডিও এবং ডাটাবেজ একটি নিরাপদ জায়গায় ২৪ ঘণ্টা চালু রাখতে এটি প্রয়োজন।
২. অনুরোধ প্রক্রিয়াকরণ (Handling Requests): ব্যবহারকারী যখন কোনো পেজ দেখতে চান (HTTP Request), সার্ভার তা প্রসেস করে সঠিক ফাইলটি ব্রাউজারে পাঠায় (HTTP Response)।
৩. নিরাপত্তা নিশ্চিতকরণ: ওয়েবসাইটকে হ্যাকিং বা ক্ষতিকর আক্রমণ থেকে বাঁচাতে ফায়ারওয়াল এবং SSL সার্টিফিকেট (HTTPS) নিয়ন্ত্রণ করে।
৪. ডোমেইন লিঙ্কিং: আপনার ওয়েবসাইটের নামকে (যেমন: example.com) আইপি অ্যাড্রেসের (IP Address) সাথে যুক্ত করে সচল রাখে।
------------------------------
## ⚙️ ওয়েব সার্ভার কীভাবে কনফিগার করতে হয়? (Nginx-এর উদাহরণ)
বর্তমানে সবচেয়ে জনপ্রিয় এবং দ্রুতগতির একটি ওয়েব সার্ভার হলো Nginx (ইঞ্জিন-এক্স)। নিচে একটি লিনাক্স (Ubuntu) সার্ভারে Nginx কনফিগার করার মৌলিক ধাপগুলো দেওয়া হলো:
## ধাপ ১: সার্ভার আপডেট ও Nginx ইনস্টল করা
প্রথমে আপনার লিনাক্স টার্মিনাল খুলে নিচের কমান্ডগুলো দিয়ে সার্ভার আপডেট করুন এবং Nginx ইনস্টল করুন:

sudo apt update
sudo apt install nginx

## ধাপ ২: ফায়ারওয়াল চেক করা
সার্ভারে ট্রাফিক প্রবেশের অনুমতি দিতে ফায়ারওয়াল কনফিগার করতে হবে:

sudo ufw allow 'Nginx Full'

## ধাপ ৩: ওয়েব সার্ভার সচল আছে কিনা পরীক্ষা করা
Nginx ঠিকঠাক চলছে কিনা তা দেখতে নিচের কমান্ডটি দিন:

systemctl status nginx

(এরপর আপনার সার্ভারের আইপি অ্যাড্রেস ব্রাউজারে লিখলে "Welcome to nginx" লেখা পেজটি দেখতে পাবেন)
## ধাপ ৪: আপনার ওয়েবসাইটের জন্য কনফিগারেশন ফাইল তৈরি
আপনার ডোমেইনের জন্য একটি নির্দিষ্ট ডিরেক্টরি এবং কনফিগারেশন ফাইল তৈরি করতে হবে:
১. ওয়েবসাইটের ফাইলের জন্য ফোল্ডার তৈরি:

sudo mkdir -p /var/www/your_domain/html

২. একটি নতুন কনফিগারেশন ফাইল খোলা:

sudo nano /etc/nginx/sites-available/your_domain

৩. ফাইলটিতে নিচের কোডটি পেস্ট করুন (আপনার ডোমেইন নাম দিয়ে পরিবর্তন করে নেবেন):

server {
    listen 80;
    server_name your_domain www.your_domain;

    root /var/www/your_domain/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

৪. ফাইলটি সেভ করে বের হয়ে যান (Ctrl+O, তারপর Enter, এবং Ctrl+X)।



 **উবুন্টু (Linux) লোকালহোস্টে একাধিক ডোমেইন সেটআপ করার সহজ গাইড**

এই গাইডের মাধ্যমে আপনি আপনার লোকাল পিসিতে Nginx ব্যবহার করে দুটো ডোমেইন (`[http://html1.test](http://html1.test)` এবং `[http://html2.test](http://html2.test)`) তৈরি করবেন।

---

### 📁 ধাপ ১: ওয়েবসাইট ফোল্ডার ও ফাইল তৈরি

প্রথমেই দুটি ওয়েবসাইটের ফাইল রাখার জন্য দুটি আলাদা ফোল্ডার তৈরি করতে হবে।

**১.১ ডোমেইন ১ (`html1.test`) এর জন্য:**

* **ফোল্ডার তৈরি:**
```bash
sudo mkdir -p /var/www/html/html1.test

```


* **পারমিশন পরিবর্তন (যাতে ফাইল এডিট করতে প্রবলেম না হয়):**
```bash
sudo chown -R $USER:$USER /var/www/html/html1.test

```


* **ইনডেক্স ফাইল তৈরি:**
```bash
nano /var/www/html/html1.test/index.html

```


ফাইলে নিচের কোডটি বসিয়ে `Ctrl + O`, তারপর `Enter`, এবং বের হওয়ার জন্য `Ctrl + X` চাপুন:
```html
<!DOCTYPE html>
<html>
<head><title>HTML1 Test</title></head>
<body><h1>সাফল্যের সাথে html1.test কনফিগার হয়েছে!</h1></body>
</html>

```



**১.২ ডোমেইন ২ (`html2.test`) এর জন্য:**

* **ফোল্ডার তৈরি:**
```bash
sudo mkdir -p /var/www/html/html2.test

```


* **পারমিশন পরিবর্তন:**
```bash
sudo chown -R $USER:$USER /var/www/html/html2.test

```


* **ইনডেক্স ফাইল তৈরি:**
```bash
nano /var/www/html/html2.test/index.html

```


ফাইলে নিচের কোডটি বসিয়ে সেভ করে বের হয়ে যান:
```html
<!DOCTYPE html>
<html>
<head><title>HTML2 Test</title></head>
<body><h1>সাফল্যের সাথে html2.test কনফিগার হয়েছে!</h1></body>
</html>

```



---

### ⚙️ ধাপ ২: Nginx সার্ভার ব্লক (Virtual Host) তৈরি

Nginx-কে বুঝিয়ে দিতে হবে যে কোনো ডোমেইনের রিকোয়েস্ট এলে সেটি কোন ফোল্ডারে পাঠাবে।

**২.১ প্রথম ডোমেইনের কনফিগারেশন:**

* ফাইল ওপেন করুন:
```bash
sudo nano /etc/nginx/sites-available/html1.test

```


* নিচের কোডটি সম্পূর্ণ পেস্ট করুন এবং সেভ করুন:
```nginx
server {
    listen 80;
    listen [::]:80;

    root /var/www/html/html1.test;
    index index.html index.htm;

    server_name html1.test www.html1.test;

    location / {
        try_files $uri $uri/ =404;
    }
}

```



**২.২ দ্বিতীয় ডোমেইনের কনফিগারেশন:**

* ফাইল ওপেন করুন:
```bash
sudo nano /etc/nginx/sites-available/html2.test

```


* নিচের কোডটি সম্পূর্ণ পেস্ট করুন এবং সেভ করুন:
```nginx
server {
    listen 80;
    listen [::]:80;

    root /var/www/html/html2.test;
    index index.html index.htm;

    server_name html2.test www.html2.test;

    location / {
        try_files $uri $uri/ =404;
    }
}

```



**২.৩ ফাইলগুলো অ্যাক্টিভ করা এবং সার্ভার টেস্ট করা:**

1. **সিমলিঙ্ক (Symlink) তৈরি:**
```bash
sudo ln -s /etc/nginx/sites-available/html1.test /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/html2.test /etc/nginx/sites-enabled/

```


2. **কনফিগারেশন টেস্ট:**
```bash
sudo nginx -t

```


*(যদি `syntax is ok` এবং `test is successful` দেখায়, তবে বুঝবেন সব ঠিক আছে।)*
3. **Nginx রিস্টার্ট:**
```bash
sudo systemctl restart nginx

```



---

### 🌐 ধাপ ৩: লোকাল Hosts ফাইল আপডেট

পিসিকে জানাতে হবে যে এই দুটি ডোমেইন বাইরে কোথাও খুঁজবে না, ইন্টারনাল IP (`127.0.0.1`)-এ রিডাইরেক্ট করবে।

1. Hosts ফাইল ওপেন করুন:
```bash
sudo nano /etc/hosts

```


2. ফাইলের একদম শেষ লাইনে নিচের দুটি লাইন যুক্ত করুন:
```text
127.0.0.1   html1.test www.html1.test
127.0.0.1   html2.test www.html2.test

```


3. ফাইলটি সেভ করে বের হয়ে যান (`Ctrl + O` -> `Enter` -> `Ctrl + X`)।

---

### 🚀 ধাপ ৪: ব্রাউজারে টেস্ট করা

আপনার ব্রাউজার (Chrome/Firefox) ওপেন করে নিচের অ্যাড্রেসগুলোতে ভিজিট করুন:

* `[http://html1.test](http://html1.test)`
* `[http://html2.test](http://html2.test)`

আপনি নির্দিষ্ট ওয়েবসাইটগুলোর তৈরি করা মেসেজগুলো স্ক্রিনে দেখতে পাবেন!