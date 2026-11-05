# অধ্যায়: একই Linux সার্ভারে একাধিক phpMyAdmin সংস্করণ পরিচালনা

> **বিষয়:** CentOS, Apache HTTP Server, PHP 8.2, phpMyAdmin, SELinux, নিরাপত্তা ও রোলব্যাক  
> **লক্ষ্য:** `/DB` এবং `/DB2`—দুটি পৃথক URL-এ দুটি phpMyAdmin ইনস্টলেশন চালানো।

## ১. অধ্যায়ের উদ্দেশ্য

এই অধ্যায় শেষে পাঠক পারবেন:

- পুরোনো phpMyAdmin ও PHP সংস্করণের অসামঞ্জস্য শনাক্ত করতে।
- Apache `Alias` ব্যবহার করে একই সার্ভারে একাধিক phpMyAdmin চালাতে।
- পুরোনো ইনস্টলেশন অক্ষত রেখে নতুন সংস্করণ স্থাপন করতে।
- PHP Runtime, Extension, Apache, SELinux ও File Permission যাচাই করতে।
- নিরাপদে পরীক্ষা, সমস্যা সমাধান ও রোলব্যাক করতে।

## ২. বাস্তব সমস্যার প্রেক্ষাপট

আলোচিত পরিবেশে phpMyAdmin **4.4.15.10** এবং PHP **8.2** ব্যবহারের কথা বলা হয়েছে। পুরোনো phpMyAdmin-এ SQL Tab কাজ করলেও Search Form-এ ত্রুটি দেখা দিতে পারে। সংস্করণ-অসামঞ্জস্য একটি সম্ভাব্য প্রধান কারণ; তবে **Apache/PHP Error Log না দেখে এটিকে নিশ্চিত Root Cause বলা যায় না**। PHP Memory Limit বাড়ানো সব ধরনের Compatibility Error সমাধান করে না।

CentOS 7, EPEL থেকে ইনস্টলেশন, Apache-এর নির্দিষ্ট Configuration Path এবং SELinux-এর অবস্থা এই আলোচনায় **অনুমানভিত্তিক**; বাস্তব সার্ভারে যাচাই করতে হবে। CentOS 7-এর নিয়মিত সমর্থন ২০২৪ সালের জুনে শেষ হয়েছে। তাই দীর্ঘমেয়াদে সমর্থিত OS-এ Migration পরিকল্পনা করা জরুরি।

### কাঙ্ক্ষিত অবস্থা

| URL | Apache Alias | Application Directory | সংস্করণ |
|---|---|---|---|
| `https://server.example.com/DB` | `/DB` | `/usr/share/phpMyAdmin` | পুরোনো 4.4.15.10 |
| `https://server.example.com/DB2` | `/DB2` | `/usr/share/phpMyAdmin5` | নতুন 5.2.x |

> এখানে `server.example.com` একটি উদাহরণ। `1.1.1.1`-ও প্রকৃত সার্ভার IP ধরে নেওয়া যাবে না। বাস্তব Domain/IP ব্যবহার করুন। দুটি URL-এ **আলাদা Application** চলবে; একই MySQL/MariaDB Server-এ সংযোগ করলে একই Database দেখা যেতে পারে।

## ৩. আর্কিটেকচার

```text
Browser
   |
   | HTTPS
   v
Apache HTTP Server
   |
   +-- /DB  ------> /usr/share/phpMyAdmin   (legacy)
   |
   +-- /DB2 ------> /usr/share/phpMyAdmin5  (new)
                          |
                     PHP Runtime
                          |
                    MySQL / MariaDB
```

`Alias` URL Path-কে Filesystem Directory-এর সঙ্গে যুক্ত করে। এটি Database Copy করে না, নতুন Database Server তৈরি করে না, এবং স্বয়ংক্রিয়ভাবে পৃথক PHP Version-ও নির্ধারণ করে না। একই Apache VirtualHost-এ PHP Handler/FPM Configuration উভয় Application-এর Runtime নিয়ন্ত্রণ করতে পারে।

## ৪. পূর্বশর্ত ও যাচাই

Root/Sudo Access, SSH, Apache, প্রয়োজনীয় PHP Extensions, Backup Space এবং HTTPS/Access Control প্রস্তুত থাকতে হবে।

```bash
cat /etc/os-release
php -v
httpd -v
rpm -q phpMyAdmin
sudo grep -RniE 'Alias /DB|phpMyAdmin' /etc/httpd/conf.d/
php -m | grep -Ei 'mysqli|mbstring|curl|zip|gd|openssl'
getenforce
```

### Command-এর ব্যাখ্যা

| Command | উদ্দেশ্য |
|---|---|
| `cat /etc/os-release` | Linux Distribution ও Version জানা |
| `php -v` | CLI PHP Version দেখা; এটি Web PHP Version-এর নিশ্চয়তা নয় |
| `httpd -v` | Apache Version দেখা |
| `rpm -q phpMyAdmin` | RPM Package হিসেবে ইনস্টল আছে কি না জানা |
| `grep ... /etc/httpd/conf.d/` | Alias ও Configuration File খুঁজে পাওয়া |
| `php -m` | CLI PHP Extensions যাচাই |
| `getenforce` | SELinux-এর বর্তমান Mode জানা |

Web Server কোন PHP Runtime ব্যবহার করছে, তা Apache/FPM Configuration এবং প্রয়োজনে সীমিত-প্রবেশযোগ্য Diagnostic Page দিয়ে যাচাই করতে হবে। Diagnostic Page তৈরি করলে কাজ শেষে অবশ্যই মুছে ফেলুন।

### ত্রুটির প্রকৃত কারণ অনুসন্ধান

```bash
sudo tail -n 100 /var/log/httpd/error_log
sudo journalctl -u httpd -n 100 --no-pager
```

PHP-FPM ব্যবহার করলে সংশ্লিষ্ট PHP-FPM Service ও Log-ও দেখুন। Log-এ Fatal Error, Undefined Function, TypeError, Permission Denied, Missing Extension ইত্যাদি পাওয়া যেতে পারে।

## ৫. ব্যাকআপ ও পরিবর্তনের পরিকল্পনা

পুরোনো Application অক্ষত রেখে নতুন Installation করতে হবে। প্রথমে **প্রকৃত Path নিশ্চিত** করুন। নিচের উদাহরণ কেবল তখনই চালাবেন, যখন Path দুটি আপনার সার্ভারে আছে।

```bash
sudo mkdir -p /root/phpmyadmin-backup
sudo cp -a /etc/httpd/conf.d/phpMyAdmin.conf /root/phpmyadmin-backup/
sudo cp -a /usr/share/phpMyAdmin /root/phpmyadmin-backup/
```

Database-এর আলাদা Backup নিন; phpMyAdmin Directory-এর Backup Database Backup নয়। Production পরিবেশে বিদ্যমান Backup Policy অনুযায়ী `mysqldump`/`mariadb-dump`, Snapshot অথবা Managed Backup ব্যবহার করুন। Backup Restore পরীক্ষা করাও ভালো চর্চা।

### Change Plan

1. বর্তমান Installation ও Runtime যাচাই।
2. Apache Configuration, Application এবং Database Backup।
3. নতুন Archive Download ও Checksum/Signature যাচাই।
4. নতুন Directory-তে Install।
5. Config, Permission, SELinux ও Access Restriction।
6. `/DB2` Alias যোগ।
7. Configuration Test ও Apache Reload।
8. নতুন URL-এ Functional Test।
9. ব্যর্থ হলে শুধু নতুন Alias Disable করে Rollback।

## ৬. নতুন phpMyAdmin Download ও Install

এই আলোচনায় **5.2.3** উদাহরণ সংস্করণ হিসেবে ব্যবহৃত হয়েছে। Installation-এর দিন অফিসিয়াল [phpMyAdmin Downloads](https://www.phpmyadmin.net/downloads/) থেকে **সর্বশেষ Stable এবং আপনার PHP Version-এর সঙ্গে Compatible** Release যাচাই করবেন। Version বদলালে URL ও Filename-ও বদলাতে হবে।

```bash
cd /usr/share
sudo curl -fL \
  -o phpMyAdmin-5.2.3-all-languages.tar.gz \
  https://files.phpmyadmin.net/phpMyAdmin/5.2.3/phpMyAdmin-5.2.3-all-languages.tar.gz
```

অফিসিয়াল Release-এর প্রকাশিত SHA256-এর সঙ্গে যাচাই:

```bash
sha256sum /usr/share/phpMyAdmin-5.2.3-all-languages.tar.gz
```

প্রকাশিত Checksum-এর সঙ্গে **ম্যানুয়ালি মিলিয়ে নেওয়ার পর** Extract করুন:

```bash
cd /usr/share
sudo tar -xzf phpMyAdmin-5.2.3-all-languages.tar.gz
sudo mv phpMyAdmin-5.2.3-all-languages phpMyAdmin5
```

ফলে:

```text
/usr/share/
├── phpMyAdmin/      # পুরোনো ইনস্টলেশন
└── phpMyAdmin5/     # নতুন ইনস্টলেশন
```

পুরোনো Directory Rename, Overwrite অথবা Remove করবেন না।

## ৭. নতুন Application Configuration

```bash
cd /usr/share/phpMyAdmin5
sudo cp config.sample.inc.php config.inc.php
openssl rand -hex 16
sudo nano config.inc.php
```

উৎপন্ন ৩২-অক্ষরের Random Secret বসান:

```php
$cfg['blowfish_secret'] = 'YOUR_OWN_RANDOM_32_CHARACTER_SECRET';
```

এটি একটি Placeholder; প্রকৃত Random Value ব্যবহার করতে হবে। পুরোনো Config থেকে প্রয়োজনীয় Host, Port, Authentication এবং অন্যান্য Custom Settings পর্যালোচনা করে নতুন File-এ আনুন। পুরোনো সম্পূর্ণ Config Blind Copy করবেন না।

### Temporary Directory

```bash
sudo mkdir -p /usr/share/phpMyAdmin5/tmp
sudo chown apache:apache /usr/share/phpMyAdmin5/tmp
sudo chmod 700 /usr/share/phpMyAdmin5/tmp
```

`config.inc.php`-তে:

```php
$cfg['TempDir'] = '/usr/share/phpMyAdmin5/tmp';
```

Configuration File সুরক্ষিত করুন:

```bash
sudo chown root:apache /usr/share/phpMyAdmin5/config.inc.php
sudo chmod 640 /usr/share/phpMyAdmin5/config.inc.php
```

`apache` User/Group উদাহরণটি CentOS-এর সাধারণ Apache Setup অনুযায়ী; প্রকৃত Web/PHP-FPM User যাচাই করুন। **পুরো Application Directory-তে Apache-এর Write Ownership দেবেন না।**

## ৮. Apache-এ দ্বিতীয় Alias যোগ করা

পুরোনো `/DB` Alias পরিবর্তন করবেন না। নতুন File তৈরি করুন:

```bash
sudo nano /etc/httpd/conf.d/phpMyAdmin5.conf
```

**নিরাপদ প্রাথমিক উদাহরণ (Trusted IP সীমাবদ্ধ):**

```apache
Alias /DB2 /usr/share/phpMyAdmin5

<Directory "/usr/share/phpMyAdmin5">
    Options FollowSymLinks
    AllowOverride None
    DirectoryIndex index.php
    Require ip 192.0.2.10
</Directory>

<DirectoryMatch "^/usr/share/phpMyAdmin5/(?:setup|libraries|templates|vendor|tmp)(?:/|$)">
    Require all denied
</DirectoryMatch>

<FilesMatch "^(?:config\.inc\.php|composer\.(?:json|lock))$">
    Require all denied
</FilesMatch>
```

`192.0.2.10` Documentation-Only Example IP; আপনার **প্রকৃত Trusted Public IP বা VPN Subnet** বসাতে হবে। IP Restriction-এর পাশাপাশি HTTPS, Authentication এবং Vendor-এর Security Guidance অনুসরণ করুন। Apache DirectoryMatch/FilesMatch ব্যবহারের পাশাপাশি Web Root থেকে Sensitive Files ও Writable Directory-এর Direct Access কার্যকরভাবে বন্ধ আছে কি না পরীক্ষা করুন।

> **বিশেষ সতর্কতা:** Apache Alias, VirtualHost, PHP-FPM Handler এবং বিদ্যমান Access Rules একে অপরের সঙ্গে প্রভাব ফেলতে পারে। বর্তমান Apache Configuration দেখে প্রয়োজন অনুযায়ী নির্দেশনা মানিয়ে নিতে হবে। `Require all granted` দিয়ে Public Internet-এ Database Administration Interface উন্মুক্ত করবেন না।

### কেন আলাদা Configuration File?

- পুরোনো `/DB` Configuration অপরিবর্তিত থাকে।
- `/DB2` সহজে Disable করা যায়।
- নতুন ও পুরোনো Application-এর Access Rules পৃথক রাখা যায়।
- Troubleshooting ও Rollback সহজ হয়।

## ৯. SELinux ও File Permission

```bash
getenforce
```

`Enforcing` হলে SELinux Policy অনুযায়ী Application Read Access ও `tmp` Directory Write Access নিশ্চিত করতে হবে। প্রথমে বর্তমান Context দেখুন:

```bash
ls -Zd /usr/share/phpMyAdmin5
ls -Zd /usr/share/phpMyAdmin5/tmp
```

পূর্বনির্ধারিত SELinux Label পুনরুদ্ধারের জন্য:

```bash
sudo restorecon -RFv /usr/share/phpMyAdmin5
```

`restorecon` সব পরিবেশে Writable `tmp` Context ঠিক করবে—এমন নিশ্চয়তা নেই। প্রয়োজন হলে `semanage fcontext` দিয়ে কেবল Writable Directory-তে উপযুক্ত `httpd_sys_rw_content_t` Label স্থায়ীভাবে নির্ধারণ করতে হবে; তা করার আগে OS Policy, Existing Labels এবং PHP-FPM User যাচাই করুন। **SELinux বন্ধ করা সমাধান নয়।**

## ১০. Apache Test ও Reload

```bash
sudo apachectl configtest
```

প্রত্যাশিত ফল:

```text
Syntax OK
```

তারপর:

```bash
sudo systemctl reload httpd
```

Apache Reload ব্যর্থ হলে Log দেখুন:

```bash
sudo journalctl -u httpd -n 100 --no-pager
```

## ১১. কার্যকরী পরীক্ষা

| পরীক্ষা | `/DB` (পুরোনো) | `/DB2` (নতুন) |
|---|---|---|
| Login Page | আগের মতো আসে | নতুন Version দেখায় |
| Authentication | বিদ্যমান Login কাজ করে | Login কাজ করে |
| Database List | দেখা যায় | দেখা যায় |
| Browse | কাজ করে | কাজ করে |
| SQL Tab | কাজ করে | কাজ করে |
| Search | ত্রুটি থাকতে পারে | কাজ করা উচিত; পরীক্ষা আবশ্যক |
| Import/Export | বিদ্যমান আচরণ | কার্যকরভাবে পরীক্ষা করতে হবে |
| Logout/Session | স্বাভাবিক | স্বাভাবিক |

একই Host-এর দুটি Path-এ Browser Cookie/Session Settings একে অপরকে প্রভাবিত করতে পারে। দুই Version-এ Login/Logout সমস্যা হলে Cookie Path, Session Configuration, Reverse Proxy এবং phpMyAdmin Settings পর্যালোচনা করুন। প্রয়োজনে আলাদা Hostname ব্যবহার করা যেতে পারে।

### Database পরিবর্তন সম্পর্কে সতর্কতা

দুটি phpMyAdmin যদি একই Database Server-এ সংযুক্ত থাকে, একটিতে করা `UPDATE`, `DELETE`, `ALTER TABLE` ইত্যাদি অন্যটির দৃশ্যমান Database-এও প্রভাব ফেলবে। দুটি URL মানেই আলাদা Database নয়।

## ১২. Troubleshooting

| লক্ষণ | সম্ভাব্য কারণ | যাচাই/সমাধান |
|---|---|---|
| `/DB2`-এ 404 | Alias Load হয়নি, ভুল Path | `grep`, `httpd -S`, `configtest` |
| 403 Forbidden | Apache Access Rule বা SELinux | Error Log, `Require ip`, SELinux Audit |
| 500 Internal Server Error | PHP Fatal Error, Missing Extension, Permission | Apache/PHP-FPM Error Log |
| PHP Code Download হয় | PHP Handler Config ভুল | Apache PHP-FPM Integration যাচাই |
| Blank Page | PHP Error/Runtime Mismatch | PHP-FPM Log ও Extensions |
| Login Loop | Session/Cookie সমস্যা | Session Path, Cookie, HTTPS Settings |
| `mysqli` Missing | PHP Extension অনুপস্থিত | Web PHP Runtime-এর Modules যাচাই |
| Temp Directory Error | Ownership/SELinux | `ls -ld`, `ls -Z`, PHP User |
| Search Tab Crash | Compatibility বা অন্য PHP Error | Stack Trace/Logs বিশ্লেষণ |

নির্ণয়ের জন্য:

```bash
sudo tail -n 100 /var/log/httpd/error_log
sudo journalctl -u httpd -n 100 --no-pager
sudo apachectl -S
```

সব PHP Error Apache Log-এ নাও থাকতে পারে; PHP-FPM-এর সংশ্লিষ্ট Log দেখতে হবে।

## ১৩. Rollback: পুরোনো Version অক্ষত রেখে `/DB2` বন্ধ করা

নতুন Alias আলাদা File-এ থাকলে Rollback সহজ:

```bash
sudo mv /etc/httpd/conf.d/phpMyAdmin5.conf \
  /etc/httpd/conf.d/phpMyAdmin5.conf.disabled
sudo apachectl configtest
sudo systemctl reload httpd
```

এতে `/DB2` Alias বন্ধ হবে; পুরোনো `/DB` Configuration অপরিবর্তিত থাকবে। নতুন Application Directory আপাতত রেখে দিতে পারেন, যাতে পরবর্তীতে Debug করা যায়।

যদি আগে থেকেই পুরোনো `phpMyAdmin.conf` পরিবর্তন করে থাকেন, Backup থেকে Restore করতে হবে:

```bash
sudo cp -a /root/phpmyadmin-backup/phpMyAdmin.conf \
  /etc/httpd/conf.d/phpMyAdmin.conf
sudo apachectl configtest
sudo systemctl reload httpd
```

পুরোনো Directory বা RPM Package Delete করবেন না, যতক্ষণ না নতুন Installation স্থিতিশীল ও Migration Plan অনুমোদিত হয়।

## ১৪. Production Security Checklist

- [ ] phpMyAdmin-এ HTTPS বাধ্যতামূলক।
- [ ] Trusted IP/VPN/SSO বা সমতুল্য Access Restriction আছে।
- [ ] পুরোনো Unsupported phpMyAdmin Public Internet-এ উন্মুক্ত নয়।
- [ ] MySQL Root-এর পরিবর্তে প্রয়োজন অনুযায়ী Least-Privilege Account ব্যবহার করা হয়।
- [ ] `config.inc.php` Web থেকে সরাসরি পড়া যায় না।
- [ ] `setup`, `libraries`, `templates`, `vendor`, `tmp` ও অন্যান্য Sensitive Path-এর Exposure পরীক্ষা করা হয়েছে।
- [ ] PHP Runtime ও Extensions সমর্থিত এবং Patch করা।
- [ ] SELinux Enabled ও সঠিকভাবে Configured।
- [ ] Database Backup ও Restore Procedure পরীক্ষিত।
- [ ] Access/Error Logs Monitor করা হয়।
- [ ] CentOS 7 হলে সমর্থিত OS-এ Migration পরিকল্পিত।

## ১৫. গুরুত্বপূর্ণ ধারণা ও সাধারণ ভুল

**ভুল ১: `yum update` দিলেই সবসময় নতুন phpMyAdmin পাওয়া যাবে।**  
Package Repository-তে কোন Version আছে তার ওপর নির্ভর করে। পুরোনো Distribution-এ Latest Upstream Release নাও থাকতে পারে।

**ভুল ২: নতুন phpMyAdmin Install করতে পুরোনোটি মুছে ফেলতে হবে।**  
না। Side-by-Side Installation সম্ভব।

**ভুল ৩: `/DB` ও `/DB2` মানেই দুটি Database।**  
না। এগুলো দুটি Web Application URL; Database Connection Config অনুযায়ী একই বা ভিন্ন Database Server হতে পারে।

**ভুল ৪: CLI `php -v` দেখেই Apache-এর PHP Version নিশ্চিত করা যায়।**  
না। Web Runtime পৃথক হতে পারে।

**ভুল ৫: `Require all granted` Production-এ নিরাপদ।**  
না। Database Administration Interface-এ কঠোর Access Restriction দরকার।

**ভুল ৬: `restorecon` চালালেই সব SELinux Permission ঠিক হয়।**  
না। Custom Path ও Writable Directory-তে অতিরিক্ত Policy/Label লাগতে পারে।

**ভুল ৭: পুরোনো phpMyAdmin-এর Search Crash-এর কারণ নিশ্চিতভাবে PHP Version।**  
এটি সম্ভাব্য কারণ; Root Cause নিশ্চিত করতে Error Log প্রয়োজন।

## ১৬. চূড়ান্ত সারাংশ

একই Linux Server-এ একাধিক phpMyAdmin সংস্করণ চালাতে Apache-এর পৃথক `Alias` যথেষ্ট হতে পারে। পুরোনো `/DB` অক্ষত রেখে নতুন `/DB2` যোগ করার পদ্ধতি কম ঝুঁকিপূর্ণ এবং Rollback সহজ। তবে **PHP Compatibility, File Permission, SELinux, HTTPS, Session Isolation এবং Database Security** যাচাই ছাড়া Production-এ Deploy করা উচিত নয়।

### আরও পড়ুন

- [phpMyAdmin Official Downloads](https://www.phpmyadmin.net/downloads/)
- [phpMyAdmin Documentation](https://docs.phpmyadmin.net/)
- [Apache mod_alias Documentation](https://httpd.apache.org/docs/2.4/mod/mod_alias.html)
- [Apache Access Control](https://httpd.apache.org/docs/2.4/howto/access.html)
- [PHP Supported Versions](https://www.php.net/supported-versions.php)

---

**অধ্যায়ের অনুশীলন:** একটি Test Server-এ `/DB2` Alias তৈরি করুন, Trusted IP Restriction দিন, Apache Configuration Test করুন, phpMyAdmin Login/Search যাচাই করুন এবং Rollback অনুশীলন করুন।
