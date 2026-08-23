আপনার দেওয়া গাইডটি এমনিতেই বেশ গুছানো, তবে কিছু কমান্ড এবং ধাপ শিক্ষানবিসদের জন্য আরও সহজ ও বিস্তারিত করে নিচে উপস্থাপন করা হলো। যাতে কোনো ভুল ছাড়া এক বসাতেই সেটআপ সম্পন্ন করা যায়।

---

🌐 **উবুন্টু (Linux) লোকালহোস্টে একাধিক ডোমেইন সেটআপ করার সহজ গাইড**

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