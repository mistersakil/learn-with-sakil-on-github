# লিনাক্স শেখার সূচিপত্র

[রোডম্যাপ](https://roadmap.sh/linux)

> Linux শেখার জন্য ধাপে ধাপে বাংলা গাইড।

## পাঠসমূহ

- [০০। সংক্ষিপ্ত প্রশ্নোত্তর](./০০-সংক্ষিপ্ত-প্রশ্নোত্তর)
- [০১। লিনাক্স পরিচিতি](./০১-লিনাক্স-পরিচিতি)
- [০১.১। ইউজার ম্যানেজমেন্ট](./০১.১-ইউজার-ম্যানেজমেন্ট)
- [০২। শেল এক্সপ্যানশন](./০২-শেল-এক্সপ্যানশন)
- [০৩। ডেভঅপস ডেভেলপমেন্ট এনভায়রনমেন্ট](./০৩-ডেভঅপস-ডেভেলপমেন্ট-এনভায়রনমেন্ট)
- [০৪। wget-এর ব্যবহার](./০৪-wget-এর-ব্যবহার)
- [০৫। লিনাক্স থিম কাস্টমাইজেশন](./০৫-লিনাক্স-থিম-কাস্টমাইজেশন)
- [০৬। লিনাক্স প্যাকেজ ইনস্টলেশন আর্কিটেকচার](./০৬-লিনাক্স-প্যাকেজ-ইনস্টলেশন-আর্কিটেকচার)
- [০৭। কমান্ড লাইন অপশন](./০৭-কমান্ড-লাইন-অপশন)
- [০৮। উবুন্টুর শেল](./০৮-উবুন্টুর-শেল)
- [০৯। APT বনাম Wget](./০৯-apt-বনাম-wget)
- [১০। ওয়েব সার্ভার](./১০-ওয়েব-সার্ভার)

---

## লিনাক্স সিস্টেম অ্যাডমিনিস্ট্রেশন মাস্টারি রোডম্যাপ

> **লক্ষ্য:** একজন দক্ষ ও পূর্ণাঙ্গ Linux System Administrator হওয়া।
>
> **পটভূমি:** Laravel + React Developer | ১ বছরের DevOps/Linux অভিজ্ঞতা
>
> **মূল ফোকাস:** Deployment, Automation, Cloud এবং Security
>
> **সম্ভাব্য সময়কাল:** ৬-১২ মাস

## রোডম্যাপের সূচিপত্র

- [ধাপ ১: লিনাক্সের ভিত্তি মজবুত করা](#-ধাপ-১-লিনাক্সের-ভিত্তি-মজবুত-করা-১-২ মাস)

- [ধাপ ২: মধ্যবর্তী পর্যায়ের সিস্টেম অ্যাডমিনিস্ট্রেশন](#-ধাপ-২-মধ্যবর্তী-পর্যায়ের-সিস্টেম-অ্যাডমিনিস্ট্রেশন-২-৩ মাস)

- [ধাপ ৩: উন্নত সিস্টেম অ্যাডমিনিস্ট্রেশন ও অটোমেশন](#-ধাপ-৩-উন্নত-সিস্টেম-অ্যাডমিনিস্ট্রেশন-ও-অটোমেশন-৩-৪ মাস)

- [ধাপ ৪: ক্লাউড ও ডেভঅপসের সমন্বয়](#-ধাপ-৪-ক্লাউড-ও-ডেভঅপসের-সমন্বয়-৩-৪ মাস)

- [ধাপ ৫: দক্ষতা উন্নয়ন, সার্টিফিকেশন ও বাস্তব প্রজেক্ট](#ধাপ-৫-দক্ষতা-উন্নয়ন-সার্টিফিকেশন-ও-বাস্তব-প্রজেক্ট-চলমান)

- [শেষ কথা](#শেষ-কথা)

---

## ধাপ ১: লিনাক্সের ভিত্তি মজবুত করা (১-২ মাস)

এই ধাপে Linux-এর মৌলিক ধারণা, কমান্ড লাইন, ফাইল সিস্টেম, ইউজার, পারমিশন, প্রসেস, নেটওয়ার্ক এবং সিস্টেম সার্ভিস সম্পর্কে শক্ত ভিত্তি তৈরি করতে হবে।

- Linux ডিস্ট্রিবিউশন ইনস্টল ও ব্যবহার: Ubuntu LTS, Rocky Linux/AlmaLinux
- ফাইল সিস্টেমের কাঠামো (FHS) বোঝা: `/etc`, `/var`, `/home`, `/opt`, `/proc`, `/sys`
- ইউজার ও গ্রুপ ব্যবস্থাপনা: `useradd`, `usermod`, `groupadd`, `passwd`, `sudoers`
- ফাইলের পারমিশন ও মালিকানা: `chmod`, `chown`, `umask`, ACL
- প্যাকেজ ব্যবস্থাপনা: `apt`, `dnf`, `yum`, `rpm`, `dpkg`
- প্রসেস ব্যবস্থাপনা: `ps`, `top`, `htop`, `kill`, `nice`, `renice`
- Systemd: `systemctl`, `journalctl`, service, timer, target
- ডিস্ক ও স্টোরেজের মৌলিক ধারণা: `lsblk`, `df`, `du`, `mount`, `/etc/fstab`
- নেটওয়ার্কিংয়ের মৌলিক ধারণা: `ip`, `ss`, `ping`, `curl`, `dig`, `traceroute`
- SSH: key generation, `ssh`, `scp`, `rsync`, SSH hardening
- টেক্সট প্রসেসিং: `grep`, `awk`, `sed`, `cut`, `sort`, `uniq`
- Bash scripting-এর মৌলিক ধারণা: variable, loop, condition, function, argument
- Cron ও Timer: `crontab` এবং `systemd timer` দিয়ে নির্ধারিত কাজ স্বয়ংক্রিয়ভাবে চালানো
- লগ বিশ্লেষণ: `/var/log`, `journalctl`, `logrotate`

---

## ধাপ ২: মধ্যবর্তী পর্যায়ের সিস্টেম অ্যাডমিনিস্ট্রেশন (২-৩ মাস)

এই ধাপে Linux server পরিচালনা, storage, networking, web server, database, Docker এবং security সম্পর্কে বাস্তব প্রশাসনিক দক্ষতা তৈরি করতে হবে।

- উন্নত ইউজার ও পারমিশন ব্যবস্থাপনা: PAM, LDAP-এর মৌলিক ধারণা, sudo policy
- উন্নত স্টোরেজ ব্যবস্থাপনা: LVM, RAID, swap, disk quota
- Backup ও Restore: `tar`, `rsync`, `borg`, `restic`, automated backup
- মনিটরিং টুল: `vmstat`, `iostat`, `sar`, `netdata`, `htop`
- পারফরম্যান্স টিউনিং: `sysctl`, `ulimit`, kernel parameter
- উন্নত নেটওয়ার্কিং: netplan/NetworkManager, DNS, DHCP, routing
- ফায়ারওয়াল: `ufw`, `firewalld`, `iptables`/`nftables`-এর মৌলিক ধারণা
- ওয়েব সার্ভার: Nginx/Apache, PHP-FPM, Laravel deployment
- ডেটাবেজ অ্যাডমিনিস্ট্রেশন: MySQL/PostgreSQL-এর মৌলিক প্রশাসন, backup, restore
- Docker-এর মৌলিক ধারণা: image, container, volume, network, `docker-compose`
- Git ও GitHub: branch, pull request, CI/CD-এর মৌলিক ধারণা
- অটোমেশনের শুরু: Bash script দিয়ে বারবার করতে হয় এমন কাজ কমানো
- Linux security-এর মৌলিক ধারণা: `fail2ban`, SSH hardening, firewall rule
- Linux troubleshooting: boot issue, service fail, disk full, network down

---

## ধাপ ৩: উন্নত সিস্টেম অ্যাডমিনিস্ট্রেশন ও অটোমেশন (৩-৪ মাস)

এই ধাপে বড় ও production-grade infrastructure পরিচালনা, configuration automation, orchestration, monitoring, security এবং high availability সম্পর্কে দক্ষতা তৈরি করতে হবে।

- Configuration Management: Ansible **(অবশ্যই শিখতে হবে)**, Puppet/Chef-এর পরিচিতি
- Infrastructure as Code: Terraform-এর মৌলিক ধারণা
- CI/CD Pipeline: GitHub Actions, GitLab CI, Jenkins
- Container Orchestration: Kubernetes-এর মৌলিক ধারণা, Helm, `kubectl`
- Monitoring ও Logging: Prometheus, Grafana, Loki/ELK
- Security Hardening: SELinux/AppArmor, CIS Benchmark, auditd
- High Availability: HAProxy, Keepalived, load balancing, clustering
- উন্নত নেটওয়ার্কিং: VLAN, VPN, `tcpdump`, Wireshark
- Automation Scripting: Python/Go দিয়ে Linux automation **(ঐচ্ছিক, তবে উপকারী)**
- Web Stack Optimization: Nginx reverse proxy, caching, SSL/TLS
- Database High Availability: replication, failover, performance tuning
- Incident Response: log analysis, root cause analysis, documentation

---

## ধাপ ৪: ক্লাউড ও ডেভঅপসের সমন্বয় (৩-৪ মাস)

Linux Administration-এর সঙ্গে Cloud এবং DevOps দক্ষতা যুক্ত করে production infrastructure পরিচালনার জন্য প্রস্তুত হতে হবে।

- একটি ক্লাউড প্ল্যাটফর্ম বেছে নিন: AWS / Azure / GCP
- Cloud Compute: EC2, VPC, IAM, S3, RDS
- Cloud Linux: EC2 user data, AMI, autoscaling, security group
- Cloud Automation: Terraform + Ansible একসাথে ব্যবহার
- Managed Kubernetes: EKS / AKS / GKE
- Cloud Monitoring: CloudWatch, Prometheus, Grafana
- Laravel/React-এর জন্য CI/CD: build, test, deploy automation
- Cloud Security: IAM policy, secrets management, encryption
- Cloud Cost Optimization: right-sizing, reserved instance, budgeting
- Hybrid/On-premise + Cloud: VPN, Direct Connect, hybrid DNS

---

## ধাপ ৫: দক্ষতা উন্নয়ন, সার্টিফিকেশন ও বাস্তব প্রজেক্ট (চলমান)

এই ধাপটি নির্দিষ্ট সময়ে শেষ হয় না। Linux Administration-এর দক্ষতা ধরে রাখতে নিয়মিত বাস্তব সমস্যা সমাধান, নতুন প্রযুক্তি শেখা এবং production-like project করা গুরুত্বপূর্ণ।

### সার্টিফিকেশন প্রস্তুতি

- RHCSA — Red Hat Certified System Administrator
- LFCS — Linux Foundation Certified System Administrator
- CompTIA Linux+

### Home Lab তৈরি

- Proxmox ব্যবহার করে virtual machine তৈরি
- একাধিক Linux VM দিয়ে lab environment তৈরি
- Docker environment তৈরি
- Kubernetes cluster তৈরি ও পরিচালনা
- Monitoring ও logging stack তৈরি
- বিভিন্ন failure scenario তৈরি করে troubleshooting অনুশীলন

### বাস্তব প্রজেক্ট

- Laravel + React application Linux server-এ deploy করা
- CI/CD pipeline তৈরি করা
- Nginx + PHP-FPM configuration করা
- MySQL database backup ও restore automation করা
- SSL/TLS এবং domain configuration করা
- Application ও server monitoring যুক্ত করা
- Automated deployment এবং rollback ব্যবস্থা তৈরি করা

### Open Source Contribution

- Linux/DevOps tools-এ ছোটখাটো contribution করা
- Documentation improve করা
- Bug report ও issue তৈরি করা
- GitHub Pull Request করার অভ্যাস তৈরি করা

### Troubleshooting Mastery

নিয়মিত নিচের সমস্যাগুলো নিজে diagnose ও সমাধান করার অনুশীলন করুন:

- Boot সমস্যা
- Kernel সমস্যা
- Network connectivity সমস্যা
- Disk full সমস্যা
- Permission সমস্যা
- Service failure
- High CPU usage
- High memory usage
- Database connection সমস্যা
- SSL/TLS সমস্যা
- DNS সমস্যা
- Application deployment সমস্যা

### Documentation

প্রতিটি গুরুত্বপূর্ণ শেখা বিষয়, troubleshooting process, command এবং configuration-এর নোট GitHub-এ সংরক্ষণ করুন। এতে শেখার পাশাপাশি ভবিষ্যতে নিজের personal knowledge base তৈরি হবে।

### Community ও নিয়মিত শেখা

- Linux/DevOps forum ও community-তে অংশগ্রহণ
- Discord ও অন্যান্য technical community অনুসরণ
- Stack Overflow-এর সমস্যা ও সমাধান পড়া
- Linux/DevOps blog, podcast ও YouTube channel অনুসরণ
- নতুন release ও official documentation নিয়মিত পড়া

---

## শেষ কথা

Laravel + React-এর মতো application development background থাকলে Linux Administration শেখার সময় শুধু Linux command মুখস্থ করার পরিবর্তে **Deployment, Automation, Cloud এবং Security**—এই চারটি বিষয়ের সঙ্গে Linux-কে ব্যবহারিকভাবে শেখা বেশি কার্যকর।

আপনার লক্ষ্য হওয়া উচিত এমন একজন engineer হওয়া, যিনি শুধু application তৈরি করতে পারেন না, বরং সেই application-এর **server, deployment, networking, monitoring, security এবং automation**-ও বুঝতে ও পরিচালনা করতে পারেন।

এই roadmap ধারাবাহিকভাবে অনুসরণ করে **৬-১২ মাস** বাস্তব অনুশীলন করলে Linux Administration-এ একটি শক্ত ভিত্তি তৈরি করা সম্ভব। এরপর production troubleshooting, automation এবং infrastructure design-এর মাধ্যমে ধীরে ধীরে mastery-এর দিকে এগিয়ে যেতে পারবেন।

> **লক্ষ্য:** Application Developer যিনি Linux Administration, DevOps ও Cloud বোঝেন — এই skill combination-কে একজন production-ready engineering skill set-এ পরিণত করা।

**ফাইলের নামের প্রস্তাব:** `linux-admin-roadmap-bangla.md`

**প্রস্তাবিত repository:** `learn-with-sakil-on-github/linux-admin-roadmap`
