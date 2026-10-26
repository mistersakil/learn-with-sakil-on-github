# Linux Administration Roadmap — বাংলায় সম্পূর্ণ গাইড

আপনার Laravel + React Development এবং Linux/DevOps শেখার লক্ষ্য বিবেচনা করে এই roadmap-টি এমনভাবে সাজানো ভালো, যাতে আপনি শুধু Linux command মুখস্থ না করে বাস্তবে server configure, application deploy, troubleshoot এবং automate করতে পারেন।

এই roadmap-এর মূল ধারণা হলো: প্রথমে Linux-এর ভিত্তি, তারপর system administration, এরপর networking ও troubleshooting, এবং সবশেষে containerization ও advanced DevOps।

ছবিতে অনেকগুলো বিষয় একসঙ্গে দেখানো হয়েছে। তবে সব বিষয় ছবির ক্রমানুসারে শেখার প্রয়োজন নেই। কিছু বিষয়ের prerequisite আছে, তাই সঠিক sequence-এ শেখা গুরুত্বপূর্ণ।

## ১. কোনটার পর কোনটা শিখবেন?

## পর্যায় ১ — Linux Fundamentals

প্রথমে শুরু করুন

Linux কীভাবে কাজ করে, terminal, directory, command এবং file permission শিখবেন।

সময়: ২–৩ সপ্তাহ

## পর্যায় ২ — Users, Processes ও Services

User/group, sudo, process management, systemd এবং package management শিখবেন।

সময়: ২–৩ সপ্তাহ

## পর্যায় ৩ — Storage, Boot ও Logs

Disk, filesystem, mount, memory, boot process, logs এবং server health বুঝবেন।

সময়: ২ সপ্তাহ

## পর্যায় ৪ — Networking ও Troubleshooting

IP, DNS, ports, routing, SSH, firewall এবং Nginx reverse proxy শিখবেন।

সময়: ২–৩ সপ্তাহ

## পর্যায় ৫ — Shell/Bash Programming

Variables, conditions, loops, functions, debugging ও automation শিখবেন।

সময়: ২ সপ্তাহ

## পর্যায় ৬ — Containerization

Docker, container runtime, resource limits, cgroups এবং container networking শিখবেন।

সময়: ৩–৪ সপ্তাহ

এটি একটি প্রস্তাবিত sequence। কিছু বিষয় পাশাপাশি শিখতে পারবেন, বিশেষ করে Bash, networking এবং বাস্তব server administration।

## ২. প্রতিটি পর্যায়ে কী কী শিখবেন?

### Phase 1: Navigation Basics ও Basic Commands

[How To Learn Linux: A Comprehensive Guide for Beginners](https://images.openai.com/static-rsc-4/tabCgwxVpwzlEVuijuyP3nep0wDS3rd8UihfFcszWpFRu0kI_jTTUuVhUkNB_KwK4j4BGkG-HDvs4EzXqX7MLO_gOq1RtjcbzxSn5fcBgOHZHd6Du5ovNMt3PVOK_P2AYPnGjvY90phjYM1LFtdwxV-yrQym6x2jN9nf4q963GA?purpose=inline)

[Linux Directory Structure (Linux File system hierarchy)](https://images.openai.com/static-rsc-4/LPBo68jP-gP8tL6bi8GwtI1Du6P7RlyegvNh1N2lEjMI4KVrk68tXikkB4BgQvvtEd2XSrOLzcODlsC0d85Gy-pQClHCo6_4g3Y94VglqqowjMJeWSsc3gAnTqAd4ujeVsKKGdQzrAIe4OqdUo7YDkrKClvIjkAdq90D_pCL4Hc?purpose=inline)

[The Best Linux Commands: A Comprehensive Guide for All Users](https://images.openai.com/static-rsc-4/Gr8oUGW9kZup-rsTw-uc_WnCL_eLEYZzt7TCgK9Zln4c-BduJaizfFzpOtD4KRE6HWQXql9O5CKin5qSbJDx40yDoa6I4sakBaPd2ySYjaZNfDXoXhb1_tKx-kki34qTTeOjrvAbUgytfsLRTSC6mxp7pHiI6AQLtjQDJCOCqDE?purpose=inline)

6

এখান থেকেই শুরু করবেন। Linux server-এ কাজ করার জন্য এটি সবচেয়ে গুরুত্বপূর্ণ foundation।

প্রথমে এই ক্রম অনুসরণ করুন:

01

Linux ও Terminal পরিচিতি

Kernel, shell, terminal, distribution এবং root directory (/)

02

Navigation Commands

pwd, ls, cd, tree, absolute path ও relative path

03

File ও Directory Management

mkdir, touch, cp, mv, rm, rmdir

04

File Content পড়া ও খোঁজা

cat, less, head, tail, grep, find

05

Command Help ও History

man, --help, history, which, type, command -v

06

Redirection ও Pipes

stdout, stdin, stderr, >, >>, 2>, |

এরপর শিখবেন:

- Editing Files: Nano বা Vim। শুরুতে Nano সহজ; Vim পরে শিখলেও হবে।
- Command Path: executable কোথায় আছে, `PATH` কীভাবে কাজ করে।
- Environment Variables: `PATH`, `HOME`, `USER`, `export`।
- Super User: `sudo`, root account ও privileged command।

প্র্যাকটিস হিসেবে `/var/www/`-এর মতো একটি directory structure তৈরি করুন, file copy ও rename করুন এবং `grep` দিয়ে log থেকে নির্দিষ্ট error খুঁজুন।

### Phase 2: Users, Groups, Permissions ও Process Management

এই পর্যায়ে আপনি বুঝবেন Linux server-এ কার কী access থাকবে এবং কোন application কীভাবে চলবে।

[What happens when you type “ls -l \*.c” in the shell? | by Yuri Quezada | Medium](https://images.openai.com/static-rsc-4/4EDrfJeuoMXIBmeGJTKHXL-isYoQnsv53ZYGaN4PBXWzpVYiAAaWcvRz4s7W-9GD6YfA--XOmYZTYZt6V9dBUFUa3dkZq6sIv9wprc2-azI1WZxcr9_1gHAM3Ef9u5Jh3M9NGRoHCLYNQybiuai4K0a03uqf2H3Yrg3j8lE2pQo?purpose=inline)

২.১ User & Group Management

`useradd`, `usermod`, `userdel`, `groupadd`, `id`, `groups`, `passwd`, `sudo`।

Practice: নতুন user ও group তৈরি, group membership এবং sudo access পরীক্ষা।

[How To Change File or Directory Permissions via the Linux Terminal | Tom's Hardware](https://images.openai.com/static-rsc-4/TVGT_nquT8ieQo9P2_fXZgMyFg3i5tF76gqssmtvKRqoNgD_v9JFCK2tHAr0WndNfBfyXhu6gM0WaesJO-gjuJoLt-wSOc8pk9G5UWBFtfa7LGz3in3x4fP8rSJYw-prwJPszS8MdHOqmGO-SjRojBwPfYtfFWdoQ7oteiVkeAA?purpose=inline)

২.২ File Permissions

`chmod`, `chown`, `chgrp`, `umask`, `rwx`, octal permission এবং `setgid`।

Practice: Nginx, PHP-FPM ও Laravel storage-এর ownership এবং permissions বোঝা।

[Show All Running Processes in Linux](https://images.openai.com/static-rsc-4/uNLOKRVvaE1fLdI-wQ30Hl7yPc-AZ3dvKuzY8SB6uwB57SqLE5d_h5wqzEh7BgSclqPtB_2LENqDsGU4TDlZt-x77kTUN69Fm0pBnSez_FL4dWO8Dikc0txhKdCtk2jLw1ZzKdCvyZgFIDShNT_Z7pFTTfx1QAaJc9h7WtdYkcM?purpose=inline)

২.৩ Process Management

`ps`, `top`, `htop`, `pgrep`, `kill`, `nice`, `renice`, `jobs`, `bg`, `fg`।

Practice: চলমান process খোঁজা, resource usage দেখা এবং নিরাপদে process বন্ধ করা।

এরপর শিখবেন Service Management (systemd):

- `systemctl status`
- `systemctl start` / `stop` / `restart`
- `systemctl enable` / `disable`
- `journalctl -u service-name`
- Custom systemd service তৈরি।

এখানে একটি গুরুত্বপূর্ণ পার্থক্য মনে রাখবেন: Process হলো চলমান program; Service হলো সাধারণত systemd দ্বারা পরিচালিত কোনো background workload। সব process service নয়।

### Phase 3: Package Management, Storage ও Booting

৩.১ Package Management

Ubuntu-তে এই ক্রমে শিখুন:

1. `apt update` ও `apt upgrade`
2. `apt search`, `apt show`
3. `apt install`, `apt remove`
4. `dpkg -l`, `dpkg -L`
5. Package repository এবং Snap

মনে রাখবেন, `apt update` মূলত package index refresh করে; এটি নিজে installed package upgrade করে না।

৩.২ Disks and Filesystems

- `df -h`, `du -sh`, `lsblk`, `blkid`
- Filesystem, inode ও mount point
- `/etc/fstab`
- Partition ও disk management
- LVM, swap এবং নতুন disk যুক্ত করা।

শুরুতে read-only command দিয়ে disk বুঝুন। Partition format বা production disk পরিবর্তনের আগে backup ও recovery plan নিশ্চিত করুন।

৩.৩ Booting Linux

- BIOS/UEFI ও bootloader
- GRUB-এর ভূমিকা
- Kernel initialization
- `systemd` startup
- Boot failure troubleshooting
- `journalctl -b` এবং boot logs।

৩.৪ Server Review

প্রতিদিন server health যাচাইয়ের জন্য শিখুন:

| কী পরীক্ষা করবেন | Command                |
| ---------------- | ---------------------- |
| Uptime ও load    | `uptime`               |
| CPU ও processes  | `top`                  |
| Memory           | `free -h`              |
| Disk space       | `df -h`                |
| Disk usage       | `du -sh /path`         |
| Failed services  | `systemctl --failed`   |
| System logs      | `journalctl -p err -b` |

### Phase 4: Networking ও Troubleshooting

আপনার server deployment কাজের জন্য এই অংশ বিশেষভাবে গুরুত্বপূর্ণ।

[TCP/IP Model: Understanding How Communication Happens Over the Internet | by Pubudu Madushan | Medium](https://images.openai.com/static-rsc-4/iWyVV8hV5TzYBuSG4Pq8sxzhZa4arsR4opNeSVUGAUBgO8mJ3ZhAWpcneCzi_9NQw_tF-5v2jOBQBKhBcv8chR2VfM3JZ6iYwHQd_1PdxQXfAQXq5gxIAGgAJnyj7UqcU7WCoZSL9AgDO2HkiE7twrSkaERW8xKwOCT6WKIKTR8?purpose=inline)

৪.১ Networking Fundamentals

TCP/IP, IP address, subnetting, gateway, ports, routing, ARP এবং Ethernet।

[CNAME 조회 — CNAME 레코드 확인 | DNS Robot | DNS Robot](https://images.openai.com/static-rsc-4/zHWKPUfNaPGph1FVRDZAQRjfgst4u7GoEuKHr91XeS-tXu15BTcy4Bi8j1ZfhC4BYXpqvkiPOr6ZRmTFacwinutXH6Ard2_ECEKiIjrP2IeQOtvevP_qU5QoAZ9Uoe9LYDP9NWITNhws4ZNSfXXob1fSJC96nhGT9A4xXYOlbyw?purpose=inline)

৪.২ DNS ও Connectivity

DNS resolution, A/AAAA/CNAME records, `dig`, `nslookup`, `getent hosts`।

[Connecting to your Cloud Server – Documentation](https://images.openai.com/static-rsc-4/kDFMm_erQbLapkuC-PWqzeenobsYPvwubwTcn7jbKxPG2MEuDEqSrTzsUGNffGaiQViLLbSUQk-uKnpyHGm18_4TwAJefWkHWjNvZ2Uygk705Ie7OoyZ_No1XAV6X9Ehb7vTdVjjKEl_ZrLGhvI8D2NgZ1XiFuJVaCM1GVmw7og?purpose=inline)

৪.৩ SSH ও File Transfer

SSH keys, `ssh`, `scp`, `sftp`, `rsync`, SSH configuration এবং secure access।

[Understanding NGINX: The Backbone of Modern Web Infrastructure](https://images.openai.com/static-rsc-4/Be_fngU_IDKFAJvf-psGCOQXVtj1RT6WGl5zAXauXSC1bzWd1xBnVradHOepI0iD3fKvN14ibLwsd8k67q-QtDmaZ2JhT8YKUMjTbZP9JDWUjxsMpwSWJgIaWCeZj5gJGi8x-K7hW9Iwlkse2Ri0I8SICe456ndqIh6rkvctZGM?purpose=inline)

৪.৪ Firewall ও Reverse Proxy

UFW, `ss -tulpn`, `curl`, HTTP/HTTPS, TLS certificates এবং Nginx reverse proxy।

এরপর শিখবেন `ping`, `traceroute`, `mtr`, `tcpdump`, `ip route`, `ip addr`, `netstat`/`ss` এবং packet analysis।

আপনার বাস্তব server troubleshooting-এ একটি গুরুত্বপূর্ণ উদাহরণ হলো: application-এর port-এ `curl` করে HTTP 200 পাওয়া গেলেও browser-এ পুরো page কাজ নাও করতে পারে। তখন DNS, reverse proxy headers, WebSocket upgrade, frontend asset এবং application-এর internal URL configuration পরীক্ষা করতে হয়।

### Phase 5: Shell/Bash Programming

এই অংশে এসে আপনি repetitive system administration কাজ automate করতে পারবেন।

শেখার ক্রম:

1. Shell variables ও environment variables
2. Quoting, command substitution ও exit status
3. `if`, `case` ও conditionals
4. `for`, `while` ও loops
5. Functions ও reusable scripts
6. `grep`, `awk`, `sed`, `cut`, `sort`, `uniq`
7. Error handling, logging ও debugging
8. Cron এবং scheduled automation।

প্র্যাকটিস প্রজেক্ট: একটি Bash script লিখুন, যেটি server-এর disk usage পরীক্ষা করবে, নির্দিষ্ট threshold ছাড়ালে log লিখবে এবং প্রয়োজনে notification পাঠাবে।

### Phase 6: Containerization ও Docker

[Why Are Companies Using Docker? A Real-World Case Study on the Rise of Containerization](https://images.openai.com/static-rsc-4/PsWzhQrOusvSDe2gUoO7cLaauO6kQ1oVRKoSynpJrVhjEkOat32ZFL9600aZah-3Z3fT384ZhX27FN8EDO-vtQ34sVLyPMeLQWzkCTxDYBqBdJn4nEqUIgIEIfAYtV9zEZ_ZC-wrz2n8WhHNEQ5Ioc_YAPDauxvg8UOYiVDhLas?purpose=inline)

[linkedin.com](https://www.linkedin.com/pulse/why-companies-using-docker-real-world-case-study-rise-devv-saxena-swb1c?utm_source=chatgpt.com)

Linux-এর fundamentals, permissions, processes, networking এবং storage বুঝে Docker শুরু করুন।

এই ক্রমে এগোন:

1. Virtual machine বনাম container
2. Container runtime, Docker Engine ও OCI-এর ধারণা
3. Docker images, containers, volumes ও networks
4. Dockerfile এবং image build
5. Port mapping, DNS ও container networking
6. Container logs, health checks ও restart policies
7. CPU/memory limits, cgroups ও resource monitoring
8. Docker Compose
9. Nginx reverse proxy দিয়ে Dockerized application প্রকাশ করা
10. Container security, non-root user এবং persistent data backup।

এরপর Kubernetes শিখতে পারবেন। তবে Kubernetes শুরু করার আগে Docker ও Linux networking-এর বাস্তব অভিজ্ঞতা নেওয়া ভালো।

## ৩. আপনার জন্য ১২ সপ্তাহের স্টাডি প্ল্যান

এই পরিকল্পনাটি সপ্তাহে প্রায় ৮–১২ ঘণ্টা অনুশীলন ধরে তৈরি।

### আপনার অগ্রগতি

0/12 সপ্তাহ

সপ্তাহ ১: Terminal ও Navigation

Filesystem, pwd, ls, cd, file operations

সপ্তাহ ২: File Content ও Shell Basics

grep, find, pipes, redirection, PATH, environment variables

সপ্তাহ ৩: Users ও Permissions

Users, groups, sudo, chmod, chown, ACL-এর পরিচিতি

সপ্তাহ ৪: Processes ও Services

ps, top, signals, systemd, journalctl

সপ্তাহ ৫: Packages ও Software

APT, repositories, install/remove, updates

সপ্তাহ ৬: Storage ও Boot

Disk, filesystem, mount, swap, boot logs

সপ্তাহ ৭: Networking Fundamentals

IP, subnet, ports, DNS, routing

সপ্তাহ ৮: SSH ও Nginx

SSH keys, firewall, TLS, reverse proxy

সপ্তাহ ৯: Troubleshooting

Logs, uptime, memory, packet inspection-এর ভিত্তি

সপ্তাহ ১০: Bash Scripting

Variables, conditions, loops, functions, automation

সপ্তাহ ১১: Docker Fundamentals

Images, containers, volumes, networks, Dockerfile

সপ্তাহ ১২: Deployment Project

Docker Compose, Nginx, HTTPS, logs ও backup

১২ সপ্তাহে এই roadmap-এর ব্যবহারিক ভিত্তি তৈরি হবে। Linux administration-এ দক্ষতা পেতে একই বিষয় production-like lab-এ বারবার অনুশীলন করতে হবে।

## ৪. কোন বিষয়গুলো পরে শিখবেন?

ছবিতে থাকা সব topic সমান অগ্রাধিকারের নয়।

| বিষয়                                  | কখন শিখবেন                            |
| -------------------------------------- | ------------------------------------- |
| Vim-এর advanced features               | Fundamentals-এর পরে                   |
| Archiving ও compression                | প্রথম ২–৩ সপ্তাহের মধ্যে              |
| Hard links ও soft links                | File management-এর সময়               |
| Advanced `awk`, `sed`, `tr`            | Bash ও text processing-এর সময়        |
| Process forking ও priorities           | Process management-এর পরে             |
| Inodes ও LVM                           | Storage phase-এ                       |
| Netfilter ও packet analysis            | Networking fundamentals-এর পরে        |
| Container runtime, cgroups ও `ulimits` | Docker basics-এর পরে                  |
| Kubernetes                             | Docker ও networking-এ দক্ষ হওয়ার পরে |

## ৫. হাতে-কলমে শেখার জন্য তিনটি প্রজেক্ট

[Configuring Nginx, PHP, MySQL, PHPMyAdmin on Ubuntu 20.04 (A to Z) — with Project Deployment. | by Meher Ullah Khan Raj | Medium](https://images.openai.com/static-rsc-4/Z1Oi_BZsHavmd1FVbXHacdAq82l8nZN7ZohSvwIKdWXYL5qDbnNpk9PY6zpr17G_OfAytAm96Xzi0i36rO8OVg3f0kY9H8_gGrsOjXiyVuFEMFnA5cbpd2utgAJrtsXO_frQZhEd5GRmfmkdRUvJByxcO-ORhLrS7IB0sXzYCGU?purpose=inline)

Project 1 — Linux Server Setup

Ubuntu server-এ non-root user, SSH key, Nginx, firewall, permissions ও log monitoring configure করুন।

[What Happens When You Type a URL and Press Enter | by Al Amin | Medium](https://images.openai.com/static-rsc-4/3DU7_0sKtnPG2zhFkBmHtedV1WManIQtwbek9N95qbn-G0-Em5QxA2jwQDdExwAM86pvArjT0QLNOSbs7JnujaBLgaUDRAK9ZeRbUaWj46sISVorsSXfXXZGt9H7zpknKNBg9mDyztIQ_LVYqOiWuN3V88AQt_Dwxgjb7bVzIyw?purpose=inline)

Project 2 — Laravel Deployment

Laravel application deploy করুন, PHP-FPM ও Nginx configure করুন, database connection, queue worker, systemd এবং scheduler চালু করুন।

[Laravel : Teknologi Virtualisasi Menggunakan Docker | by Dana Irwanda | Medium](https://images.openai.com/static-rsc-4/J_2rtjddE6xfNHkj0CQgSPxF_0ORjLe1vW3lSJ0BexiDfyKF1PeLDinnGBrLnnin2fs8Mqn0ePKkxUo7V_sPpVlF-CGTPt9QDy2FJXVGorzKSp3oYHOhuFZKu1aDXRrZDSc-nAdPNIIKg_x_kMH9cYLOqxYadSSXREcaU4hCx3I?purpose=inline)

Project 3 — Dockerized Application

Docker Compose দিয়ে application, database ও reverse proxy চালু করুন। Volume, network, logs, health checks এবং backup পরীক্ষা করুন।

## আমার চূড়ান্ত পরামর্শ

আপনার লক্ষ্য যদি Linux Administration থেকে DevOps, Deployment ও Automation-এ যাওয়া হয়, তাহলে এই মূল sequence অনুসরণ করুন:

Linux Basics → Files & Permissions → Users & Groups → Processes → systemd → Package Management → Storage & Boot → Networking → Troubleshooting → Bash → Docker → Kubernetes।

প্রতিটি পর্যায়ে command মুখস্থ করার চেয়ে বাস্তবে ব্যবহার করা বেশি গুরুত্বপূর্ণ। নিজের Ubuntu laptop-এ একটি lab environment তৈরি করে অনুশীলন করুন; production server-এ পরীক্ষা করার আগে lab-এ পরিবর্তনগুলো যাচাই করুন।