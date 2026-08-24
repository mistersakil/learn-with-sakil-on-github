> [🏠](../) [⬅️ ০৯। apt বনাম wget](../০৯-apt-বনাম-wget) [➡️ ১০। ওয়েব সার্ভার](../১০-ওয়েব-সার্ভার)

# ১০। ওয়েব সার্ভার

Linux, DevOps, Web Hosting এবং Web Application Deployment-এর জগতে প্রবেশ করতে হলে ওয়েব সার্ভার সম্পর্কে পরিষ্কার ধারণা থাকা অত্যন্ত গুরুত্বপূর্ণ। এই অধ্যায়ে আমরা ওয়েব সার্ভার কী, কেন প্রয়োজন, কীভাবে কাজ করে, Nginx ইনস্টলেশন, Virtual Host, Multiple Domain Configuration এবং Local Development Environment নিয়ে বিস্তারিত আলোচনা করব।

---

## সূচিপত্র

- ওয়েব সার্ভার কী?
- ওয়েব সার্ভার কেন প্রয়োজন?
- ওয়েব সার্ভার কীভাবে কাজ করে?
- HTTP Request এবং Response
- DNS এবং Domain Resolution
- Web Server বনাম Application Server
- জনপ্রিয় Web Server সমূহ
- Nginx পরিচিতি
- Ubuntu-তে Nginx ইনস্টলেশন
- Firewall Configuration
- Nginx Directory Structure
- Virtual Host (Server Block)
- Single Domain Configuration
- Multiple Domain Configuration
- Local Development Domain (.test)
- Hosts File Configuration
- Nginx Testing এবং Reload
- SSL ও HTTPS পরিচিতি
- Common Troubleshooting
- Practice Lab
- Summary

---

## ওয়েব সার্ভার কী?

ওয়েব সার্ভার হলো একটি বিশেষ সফটওয়্যার বা কম্পিউটার সিস্টেম যা ওয়েবসাইট এবং ওয়েব অ্যাপ্লিকেশনের ফাইল সংরক্ষণ করে এবং ব্যবহারকারীর অনুরোধ অনুযায়ী সেই ফাইলগুলো ব্রাউজারে পাঠায়।

সহজ ভাষায়:

আপনি যখন ব্রাউজারে কোনো ওয়েবসাইটের ঠিকানা লিখেন—

```text
https://example.com
```

তখন আপনার ব্রাউজার একটি Request পাঠায়।

ওই Request গ্রহণ করে Web Server।

তারপর Web Server প্রয়োজনীয় HTML, CSS, JavaScript, Image অথবা Dynamic Content তৈরি করে আপনার ব্রাউজারে পাঠিয়ে দেয়।

---

## বাস্তব জীবনের উদাহরণ

একটি রেস্টুরেন্ট কল্পনা করুন।

| রেস্টুরেন্ট | ওয়েব জগৎ |
|------------|-----------|
| কাস্টমার | Browser |
| ওয়েটার | Web Server |
| রান্নাঘর | Application |
| খাবার | Response |

আপনি খাবার অর্ডার দেন → ওয়েটার অর্ডার নেয় → রান্নাঘরে পাঠায় → খাবার এনে দেয়।

ঠিক একইভাবে:

```text
Browser
   ↓
Web Server
   ↓
Application
   ↓
Database
```

---

## ওয়েব সার্ভার কেন প্রয়োজন?

একটি ওয়েবসাইট বা ওয়েব অ্যাপ্লিকেশন চালানোর জন্য ওয়েব সার্ভার অপরিহার্য।

কারণ এটি—

### ১. ফাইল সংরক্ষণ করে

ওয়েবসাইটের:

- HTML
- CSS
- JavaScript
- Images
- Videos
- Fonts

সবকিছু সংরক্ষণ করে।

### ২. Request Handle করে

প্রতিদিন লক্ষ লক্ষ ব্যবহারকারী ওয়েবসাইটে প্রবেশ করে।

ওয়েব সার্ভার:

```text
Request গ্রহণ করে
↓
Process করে
↓
Response পাঠায়
```

### ৩. Security প্রদান করে

নিরাপত্তার জন্য:

- HTTPS
- SSL Certificate
- Rate Limiting
- Firewall
- Access Control

ব্যবহার করা হয়।

### ৪. Domain পরিচালনা করে

উদাহরণ:

```text
google.com
facebook.com
youtube.com
```

এই ডোমেইনগুলোকে IP Address-এর সাথে সংযুক্ত রাখে।

### ৫. Multiple Website Host করতে পারে

একটি সার্ভারে একাধিক ওয়েবসাইট চালানো সম্ভব।

উদাহরণ:

```text
192.168.1.10

├── company.com
├── blog.company.com
├── api.company.com
└── admin.company.com
```

---

## ওয়েব সার্ভার কীভাবে কাজ করে?

নিচের Flow-টি বুঝলে পুরো Web Architecture পরিষ্কার হয়ে যাবে।

```text
Browser
   │
   ▼
DNS
   │
   ▼
Web Server
   │
   ▼
Application
   │
   ▼
Database
```

| ধাপ / পর্যায় | নাম / অ্যাকশন | বিবরণ / বিষয়বস্তু |
| --- | --- | --- |
| **ধাপ ১** | **User Domain লিখে** | `[https://example.com](https://example.com)` টাইপ করা হয় |
| **ধাপ ২** | **DNS Lookup** | ডোমেইনকে IP-তে রূপান্তর করে (`example.com` $\rightarrow$ `203.0.113.10`) |
| **ধাপ ৩** | **Browser Request পাঠায়** | `GET / HTTP/1.1` (Host: `example.com`) পাঠায় |
| **ধাপ ৪** | **Web Server Request গ্রহণ করে** | Nginx, Apache বা LiteSpeed সার্ভার রিকোয়েস্ট গ্রহণ করে |
| **ধাপ ৫** | **Response পাঠায়** | HTML কোড পাঠায় (যেমন: `<h1>Hello World</h1>`) |
| **ধাপ ৬** | **Browser Page Render করে** | ব্যবহারকারী ওয়েবসাইট দেখতে পান (User sees website) |

---

## HTTP Request এবং Response

ওয়েব সার্ভারের সবচেয়ে গুরুত্বপূর্ণ কাজ হলো Request এবং Response পরিচালনা করা।

### HTTP Request

Browser থেকে Server-এর দিকে যায়।

উদাহরণ:

```http
GET /about HTTP/1.1
Host: example.com
```

### HTTP Response

Server থেকে Browser-এর দিকে আসে।

উদাহরণ:

```http
HTTP/1.1 200 OK
Content-Type: text/html
```

### সাধারণ Status Code

| Status Code | Message | Description |
| --- | --- | --- |
| **200** | OK | সফল Request |
| **301** | Moved Permanently | Permanent Redirect |
| **404** | Not Found | ফাইল পাওয়া যায়নি |
| **500** | Internal Server Error | সার্ভারের অভ্যন্তরীণ সমস্যা |

---

## DNS কী?

DNS = Domain Name System

DNS হলো ইন্টারনেটের ফোনবুক।

উদাহরণ:

```text
google.com
↓
142.250.193.78
```

মানুষ Domain মনে রাখে।

কম্পিউটার IP Address বোঝে।

DNS এই দুইয়ের মধ্যে সংযোগ তৈরি করে।

---

## Web Server বনাম Application Server

অনেকেই এই দুইটি বিষয় গুলিয়ে ফেলেন।

| Web Server | Application Server |
|------------|-------------------|
| Static File Serve করে | Dynamic Logic Execute করে |
| Nginx | Laravel |
| Apache | Django |
| Caddy | Node.js |

### উদাহরণ

```text
User
 ↓
Nginx
 ↓
Laravel
 ↓
MySQL
```

এখানে:

- Nginx = Web Server
- Laravel = Application
- MySQL = Database

---

## জনপ্রিয় Web Server সমূহ

| Web Server | বিবরণ / ধরন | মূল সুবিধা |
| --- | --- | --- |
| **Apache** | সবচেয়ে পুরনো ও জনপ্রিয় | Stable, Large Community, Shared Hosting Friendly |
| **Nginx** | বর্তমান সময়ে সবচেয়ে জনপ্রিয় | Fast, Lightweight, High Performance |
| **LiteSpeed** | Commercial (WordPress Hosting-এ জনপ্রিয়) | High Performance, WordPress Optimization |
| **Caddy** | Modern Web Server | Automatic HTTPS, Easy Configuration |

---

## Nginx পরিচিতি

Nginx (উচ্চারণ: Engine-X)

বর্তমানে:

- Netflix
- GitHub
- Dropbox
- Cloudflare

সহ অসংখ্য কোম্পানি ব্যবহার করে।

### Nginx এর সুবিধা

- কম RAM ব্যবহার
- High Concurrency
- Reverse Proxy
- Load Balancer
- SSL Support
- Fast Static File Serving

### Ubuntu-তে Nginx Installation
| ধাপ / পদক্ষেপ | Command / বিবরণ | Output / নোট |
| --- | --- | --- |
| **Repository Update** | `sudo apt update` | প্যাকেজ লিস্ট আপডেট করা |
| **Nginx Install** | `sudo apt install nginx -y` | Nginx সার্ভার ইনস্টল করা |
| **Version Check** | `nginx -v` | `nginx version: nginx/1.28.0` |


### Nginx Service Management

| অ্যাকশন | Command | উদ্দেশ্য / কাজ |
| --- | --- | --- |
| **Start** | `sudo systemctl start nginx` | Nginx সার্ভিস চালু করা |
| **Stop** | `sudo systemctl stop nginx` | Nginx সার্ভিস বন্ধ করা |
| **Restart** | `sudo systemctl restart nginx` | Nginx পুরোপুরি রিস্টার্ট করা |
| **Reload** | `sudo systemctl reload nginx` | ডাউনটাইম ছাড়া কনফিগারেশন রিলোড করা |
| **Status** | `systemctl status nginx` | সার্ভিসের বর্তমান অবস্থা দেখা |


### Nginx Directory Structure

Ubuntu-তে Nginx সাধারণত:

```text
/etc/nginx/
```

ডিরেক্টরির মধ্যে থাকে।

#### Structure

```text
/etc/nginx
├── conf.d
├── fastcgi.conf
├── fastcgi_params
├── koi-utf
├── koi-win
├── mime.types
├── modules-available
├── modules-enabled
├── nginx.conf
├── proxy_params
├── scgi_params
├── sites-available
├── sites-enabled
├── snippets
├── uwsgi_params
└── win-utf
```

আপনার Ubuntu-তে Nginx ইনস্টল করার পর `/etc/nginx/` ডিরেক্টরির ভেতরে যে ফাইল ও ফোল্ডারগুলো দেখছেন, সেগুলো Nginx-এর কনফিগারেশন, মডিউল, MIME টাইপ, Proxy, FastCGI এবং Virtual Host ব্যবস্থাপনার জন্য ব্যবহৃত হয়।

---

# 📂 Nginx Directory Structure

```text
/etc/nginx
├── conf.d
├── fastcgi.conf
├── fastcgi_params
├── koi-utf
├── koi-win
├── mime.types
├── modules-available
├── modules-enabled
├── nginx.conf
├── proxy_params
├── scgi_params
├── sites-available
├── sites-enabled
├── snippets
├── uwsgi_params
└── win-utf
```

---

# 🧠 Nginx Configuration Hierarchy

Nginx সাধারণত নিচের ক্রমে কনফিগারেশন লোড করে:

```text
nginx.conf
│
├── modules-enabled/*
│
├── conf.d/*.conf
│
└── sites-enabled/*
```

অর্থাৎ `nginx.conf` হচ্ছে মূল (Root) Configuration File।

---

# 📄 nginx.conf

এটি Nginx-এর প্রধান কনফিগারেশন ফাইল।

লোকেশন:

```bash
/etc/nginx/nginx.conf
```

দেখতে:

```nginx
user www-data;
worker_processes auto;

events {
    worker_connections 768;
}

http {

    include mime.types;

    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

---

## কাজ

* Nginx-এর Global Settings
* Worker Process
* Logging
* Compression
* Security Settings
* Virtual Host Loading

---

# 📁 sites-available

লোকেশন:

```bash
/etc/nginx/sites-available
```

এখানে সকল Virtual Host Configuration রাখা হয়।

উদাহরণ:

```text
sites-available
├── default
├── blog.test
├── api.test
└── shop.test
```

---

## Example

```nginx
server {
    listen 80;
    server_name blog.test;

    root /var/www/blog;
}
```

---

# 📁 sites-enabled

লোকেশন:

```bash
/etc/nginx/sites-enabled
```

এখানে শুধুমাত্র Active Website-এর Symlink থাকে।

উদাহরণ:

```text
sites-enabled
├── default
├── blog.test
└── api.test
```

---

## Symlink Example

```bash
sudo ln -s \
/etc/nginx/sites-available/blog.test \
/etc/nginx/sites-enabled/
```

---

## কেন এই ডিজাইন?

একটি Website Disable করতে:

```bash
sudo rm /etc/nginx/sites-enabled/blog.test
```

ফাইল ডিলিট করতে হয় না।

---

# 📁 conf.d

লোকেশন:

```bash
/etc/nginx/conf.d
```

এখানে Global Configuration রাখা হয়।

---

## Example

```text
conf.d
├── gzip.conf
├── security.conf
└── cache.conf
```

---

## Example

```nginx
gzip on;
gzip_types text/css application/javascript;
```

---

## Usage

পুরো সার্ভারের জন্য:

* Security Rules
* Cache Rules
* Rate Limiting
* Gzip

---

# 📁 snippets

লোকেশন:

```bash
/etc/nginx/snippets
```

Reusable Configuration Block রাখার জন্য।

---

## Example

```text
snippets
├── ssl.conf
├── php.conf
└── security.conf
```

---

## Include Example

```nginx
server {

    include snippets/security.conf;

}
```

---

## সুবিধা

একই কোড বারবার লিখতে হয় না।

---

# 📄 mime.types

লোকেশন:

```bash
/etc/nginx/mime.types
```

---

## MIME Type কী?

Browser-কে বলে দেয় ফাইলটি কোন ধরনের।

---

## Example

```text
index.html
```

Browser বুঝবে:

```http
Content-Type: text/html
```

---

## Example

```text
style.css
```

Browser বুঝবে:

```http
Content-Type: text/css
```

---

## Example

```text
app.js
```

Browser বুঝবে:

```http
Content-Type: application/javascript
```

---

# 📄 fastcgi.conf

FastCGI Application-এর জন্য।

সাধারণত:

```text
PHP-FPM
```

এর সাথে ব্যবহার হয়।

---

## Example

```nginx
location ~ \.php$ {

    include fastcgi.conf;

    fastcgi_pass unix:/run/php/php8.4-fpm.sock;

}
```

---

## কাজ

Nginx → PHP-FPM Data Pass করে।

---

# 📄 fastcgi_params

FastCGI-এর Environment Variable।

---

## Example

```nginx
include fastcgi_params;
```

---

এর মধ্যে থাকে:

```text
SCRIPT_NAME
QUERY_STRING
REQUEST_METHOD
CONTENT_TYPE
```

---

## Laravel / PHP Project-এ খুব গুরুত্বপূর্ণ

---

# 📄 proxy_params

Reverse Proxy-এর জন্য।

---

## Example

```nginx
location / {

    proxy_pass http://127.0.0.1:3000;

    include proxy_params;

}
```

---

## ব্যবহার

যখন Nginx-এর পেছনে:

```text
Node.js
Laravel Octane
Next.js
Express.js
NestJS
```

চলবে।

---

# 📄 scgi_params

SCGI Protocol-এর Parameter।

---

## SCGI কী?

Simple Common Gateway Interface

বর্তমানে খুব কম ব্যবহৃত হয়।

---

## সাধারণত

```text
Rarely Used
```

---

# 📄 uwsgi_params

Python Application-এর জন্য।

---

## Example

```text
Django
Flask
FastAPI
```

---

## Example

```nginx
location / {

    include uwsgi_params;

    uwsgi_pass unix:/run/uwsgi.sock;

}
```

---

# 📁 modules-available

লোকেশন:

```bash
/etc/nginx/modules-available
```

এখানে Installed Modules-এর Configuration থাকে।

---

## Example

```text
50-mod-http-image-filter.conf
50-mod-http-geoip.conf
```

---

# 📁 modules-enabled

লোকেশন:

```bash
/etc/nginx/modules-enabled
```

এখানে Active Modules-এর Symlink থাকে।

---

## Relationship

```text
modules-available
       │
       ▼
modules-enabled
```

---

## Enable Module

```bash
sudo ln -s \
/etc/nginx/modules-available/mod.conf \
/etc/nginx/modules-enabled/
```

---

# 📄 koi-utf

---

## কাজ

Character Encoding Mapping File

---

## Encoding

```text
KOI8-R → UTF-8
```

---

## ব্যবহার

Russian Language Support

---

# 📄 koi-win

---

## কাজ

```text
KOI8-R → Windows Encoding
```

Conversion Mapping।

---

## ব্যবহার

Legacy Russian System

---

# 📄 win-utf

---

## কাজ

```text
Windows-1251 → UTF-8
```

Encoding Conversion।

---

## ব্যবহার

পুরনো Windows Character Set Support।

---

# 🎯 বাস্তবে একজন Laravel Developer কী কী বেশি ব্যবহার করে?

আপনি Laravel + Ubuntu + Nginx ব্যবহার করলে সবচেয়ে বেশি কাজ করবেন:

```text
/etc/nginx/nginx.conf

/etc/nginx/sites-available/

/etc/nginx/sites-enabled/

/etc/nginx/snippets/

/etc/nginx/conf.d/

/etc/nginx/fastcgi.conf

/etc/nginx/fastcgi_params
```

---

# 📚 সহজে মনে রাখার ট্রিক

```text
nginx.conf
    │
    ├── conf.d
    │
    ├── sites-enabled
    │       │
    │       ▼
    │   sites-available
    │
    ├── snippets
    │
    ├── fastcgi*
    ├── proxy*
    ├── uwsgi*
    └── modules-enabled
```

### মনে রাখুন

* `nginx.conf` → Nginx-এর Master Configuration
* `sites-available` → Website Definitions
* `sites-enabled` → Active Websites
* `conf.d` → Global Configurations
* `snippets` → Reusable Configurations
* `fastcgi*` → PHP-FPM
* `proxy_params` → Reverse Proxy
* `uwsgi_params` → Python Apps
* `modules-*` → Nginx Modules
* `mime.types` → File Type Mapping

এগুলো ভালোভাবে বুঝে ফেললে Nginx-এর প্রায় ৮০–৯০% Configuration Structure আপনার আয়ত্তে চলে আসবে।


| ফাইল / ডিরেক্টরি | কাজ / বিবরণ |
| --- | --- |
| **nginx.conf** | মূল Configuration File |
| **sites-available** | সকল Virtual Host এখানে থাকে |
| **sites-enabled** | যেসব Virtual Host Active আছে |

---


## Firewall Configuration

Ubuntu-তে UFW ব্যবহার করা হয়।

| পদক্ষেপ / কাজ | Command | উদ্দেশ্য / নোট |
| --- | --- | --- |
| **Nginx Profiles দেখুন** | `sudo ufw app list` | UFW-তে উপলব্ধ Nginx প্রোফাইল তালিকা দেখতে |
| **Full Access Enable** | `sudo ufw allow 'Nginx Full'` | HTTP (80) ও HTTPS (443) উভয় পোর্ট চালু করতে |
| **Status Check** | `sudo ufw status` | ফায়ারওয়ালের বর্তমান অবস্থা দেখতে |

---

## Single Domain Configuration

| পদক্ষেপ | Command | বিবরণ / উদ্দেশ্য |
| --- | --- | --- |
| **Website Directory তৈরি** | `sudo mkdir -p /var/www/example.com` | ওয়েবসাইটের ফাইল রাখার জন্য ডিরেক্টরি তৈরি করা |
| **Ownership Change** | `sudo chown -R $USER:$USER /var/www/example.com` | ডিরেক্টরির মালিকানা বর্তমান ইউজারকে দেওয়া |
| **Index File তৈরি** | `nano /var/www/[example.com/index.html](https://example.com/index.html)` | নতুন index.html ফাইল তৈরি ও এডিট করা |

### Content

```html
<!DOCTYPE html>
<html>
<head>
<title>Example</title>
</head>
<body>
<h1>Hello Example Domain</h1>
</body>
</html>
```

---

## Virtual Host (Server Block)

Virtual Host হলো Nginx-কে বলে দেওয়া:

> কোন Domain কোন Folder ব্যবহার করবে।

### Configuration File

```bash
sudo nano /etc/nginx/sites-available/example.com
```

### Content

```nginx
server {

    listen 80;
    listen [::]:80;

    server_name example.com www.example.com;

    root /var/www/example.com;

    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

}
```

### Enable Site

```bash
sudo ln -s \
/etc/nginx/sites-available/example.com \
/etc/nginx/sites-enabled/
```

### Configuration Test

যেকোনো পরিবর্তনের পরে:

```bash
sudo nginx -t
```

সফল হলে:

```text
syntax is ok
test is successful
```

### Reload Nginx

```bash
sudo systemctl reload nginx
```

---

## Multiple Domain Configuration

একটি Nginx Server থেকে অসংখ্য Domain চালানো যায়।

### Example

```text
html1.test
html2.test
api.test
blog.test
```

### Domain 1

```bash
sudo mkdir -p /var/www/html/html1.test
```

```bash
sudo chown -R $USER:$USER /var/www/html/html1.test
```

```bash
nano /var/www/html/html1.test/index.html
```

#### Content

```html
<h1>html1.test Working</h1>
```

### Domain 2

```bash
sudo mkdir -p /var/www/html/html2.test
```

```bash
sudo chown -R $USER:$USER /var/www/html/html2.test
```

```bash
nano /var/www/html/html2.test/index.html
```

#### Content

```html
<h1>html2.test Working</h1>
```

---

### Domain 1 Virtual Host

```bash
sudo nano /etc/nginx/sites-available/html1.test
```

```nginx
server {

    listen 80;
    listen [::]:80;

    server_name html1.test;

    root /var/www/html/html1.test;

    index index.html;

}
```

### Domain 2 Virtual Host

```bash
sudo nano /etc/nginx/sites-available/html2.test
```

```nginx
server {

    listen 80;
    listen [::]:80;

    server_name html2.test;

    root /var/www/html/html2.test;

    index index.html;

}
```

### Enable Sites

```bash
sudo ln -s \
/etc/nginx/sites-available/html1.test \
/etc/nginx/sites-enabled/
```

```bash
sudo ln -s \
/etc/nginx/sites-available/html2.test \
/etc/nginx/sites-enabled/
```

### Test Configuration

```bash
sudo nginx -t
```

#### Restart

```bash
sudo systemctl restart nginx
```

### Local Development Domain (.test)

লোকাল ডেভেলপমেন্টে `.test` ডোমেইন ব্যবহার করা একটি ভালো অভ্যাস।

উদাহরণ:

```text
laravel.test
react.test
vue.test
api.test
```

### Hosts File Configuration

লোকাল মেশিনকে বলতে হবে:

```text
html1.test
```

আসলে:

```text
127.0.0.1
```

এই IP-তে যাবে।

### Hosts File Edit

```bash
sudo nano /etc/hosts
```

#### Add

```text
127.0.0.1 html1.test
127.0.0.1 html2.test
127.0.0.1 api.test
127.0.0.1 laravel.test
```

### Result

Browser-এ:

```text
http://html1.test
```

লিখলেই লোকাল সাইট খুলবে।

---

## 🔒 SSL ও HTTPS পরিচিতি

HTTP নিরাপদ নয়।

HTTPS ব্যবহার করতে হয়।

### HTTP

```text
http://example.com
```

### HTTPS

```text
https://example.com
```

### HTTPS সুবিধা

- Encryption
- Security
- SEO Benefit
- User Trust

---

## Common Troubleshooting

### 403 Forbidden

কারণ:

```text
Permission Problem
```

সমাধান:

```bash
sudo chown -R www-data:www-data /var/www
```

### 404 Not Found

কারণ:

```text
Wrong Root Path
```

চেক করুন:

```nginx
root /var/www/html/site;
```

### Nginx Start হচ্ছে না

চেক করুন:

```bash
sudo nginx -t
```

### Port Already Used

চেক করুন:

```bash
sudo ss -tulpn | grep :80
```


### Error Log দেখুন

```bash
sudo tail -f /var/log/nginx/error.log
```

---


## Summary

এই অধ্যায়ে আমরা শিখলাম:

✅ ওয়েব সার্ভার কী

✅ কেন ওয়েব সার্ভার প্রয়োজন

✅ HTTP Request ও Response

✅ DNS কীভাবে কাজ করে

✅ Web Server বনাম Application Server

✅ Apache, Nginx, LiteSpeed ও Caddy

✅ Ubuntu-তে Nginx Installation

✅ Firewall Configuration

✅ Virtual Host

✅ Multiple Domain Configuration

✅ Hosts File

✅ SSL ও HTTPS

✅ Common Troubleshooting

✅ Local Development Environment

এখন আপনি Ubuntu Linux-এ Nginx ব্যবহার করে Single Domain এবং Multiple Domain Web Server কনফিগার করতে সক্ষম।

---

> [🏠](../) [⬅️ ০৯। apt বনাম wget](../০৯-apt-বনাম-wget) [➡️ ১০। ওয়েব সার্ভার](../১০-ওয়েব-সার্ভার)