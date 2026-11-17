# ডকার রোডম্যাপ ফর ২০২৭

তোমার রোডম্যাপটা ইতিমধ্যেই খুব ভালো এবং বাস্তবমুখী। ২০২৬ সালের শেষের দিকে (Docker Engine ২৯.x, Compose v৫.x) অবস্থা দেখে ২০২৭-এর জন্য কিছু গুরুত্বপূর্ণ আপডেট ও গভীরতা যোগ করেছি। মূল কাঠামো একই রাখা হয়েছে, শুধু আধুনিক বেস্ট প্র্যাকটিস, নতুন ফিচার এবং Laravel-কেন্দ্রিক জায়গাগুলো আরও শক্ত করা হয়েছে।

প্রতিদিন ১–২ ঘণ্টা দিলে এখনও ৭–৯ সপ্তাহে বেসিক থেকে প্রোডাকশন লেভেলে যাওয়া সম্ভব।

| ফেজ | বিষয় | সময় | শেষ হলে তুমি পারবে |
|-----|------|-----|-------------------|
| ফেজ ০ | প্রস্তুতি | ২ দিন | লিনাক্স + টার্মিনালে স্বাচ্ছন্দ্য |
| ফেজ ১ | বেসিক ধারণা | ১ সপ্তাহ | কন্টেইনার চালানো, থামানো, দেখা |
| ফেজ ২ | ইমেজ ও Dockerfile | ১ সপ্তাহ | নিজের ইমেজ বানানো (BuildKit সহ) |
| ফেজ ৩ | ভলিউম ও নেটওয়ার্ক | ৫ দিন | ডেটা বাঁচানো + কন্টেইনার জোড়া |
| ফেজ ৪ | Docker Compose | ১ সপ্তাহ | মাল্টি-কন্টেইনার অ্যাপ চালানো |
| ফেজ ৫ | রিয়েল Laravel প্রজেক্ট | ১ সপ্তাহ | পুরো Laravel স্ট্যাক ডকারাইজ করা |
| ফেজ ৬ | অপটিমাইজেশন ও নিরাপত্তা | ৫–৭ দিন | ছোট, দ্রুত, নিরাপদ ইমেজ |
| ফেজ ৭ | রেজিস্ট্রি + CI/CD | ১ সপ্তাহ | অটো বিল্ড + ডেপ্লয় |
| ফেজ ৮ | অর্কেস্ট্রেশন | ১–২ সপ্তাহ | Kubernetes বেসিক বোঝা |
| ফেজ ৯ | প্রোডাকশন | চলমান | মনিটরিং, লগিং, স্কেলিং |
| ফেজ ১০ | অ্যাডভান্সড | প্রয়োজনমতো | Buildx, Scout, Rootless ইত্যাদি |

## ফেজ ০: প্রস্তুতি (আপডেটেড)

- Linux বেসিক: `ls, cd, pwd, mkdir, cat, nano/vim, chmod, chown, ps, top/htop, df, free`
- ফাইল পারমিশন + ইউজার/গ্রুপ ধারণা খুব জরুরি (পরে non-root ইউজার বানাতে লাগবে)
- টার্মিনালে আউটপুট পড়তে পারা
- PHP + Laravel বেসিক জানা থাকলেই যথেষ্ট

**প্র্যাকটিস:** Ubuntu-তে Docker Engine ২৯.x ইনস্টল করো (অফিসিয়াল ডকস অনুসারে)। `docker run hello-world` চালিয়ে দেখো। এরপর `docker version` ও `docker compose version` চেক করো।

## ফেজ ১: বেসিক ধারণা (সবচেয়ে গুরুত্বপূর্ণ)

যা শিখবে:

- Docker কী, কেন লাগে, VM-এর সাথে পার্থক্য
- Image vs Container, Layer, Registry, Daemon, Client
- Host kernel share, Namespace ও cgroup-এর হালকা ধারণা
- ২০২৬/২৭-এ containerd image store ডিফল্ট হয়ে গেছে — এটা জেনে রাখো

কমান্ডগুলো হাতে বসাও:

```bash
docker --version
docker pull nginx
docker run --name web1 -d -p 8080:80 nginx
docker ps / docker ps -a
docker logs web1
docker stop/start/restart web1
docker rm web1
docker images
docker system df
docker system prune
```

**মাইলস্টোন:** চোখ বন্ধ করে Image ও Container-এর পার্থক্য বলতে পারবে এবং যেকোনো ইমেজ দিয়ে কন্টেইনার চালিয়ে ব্রাউজারে দেখাতে পারবে।

## ফেজ ২: ইমেজ ও Dockerfile (BuildKit যুগ)

যা শিখবে:

- Layer ক্যাশিং কীভাবে কাজ করে
- সব নির্দেশিকা: `FROM, RUN, COPY, ADD, WORKDIR, EXPOSE, ENV, ARG, CMD, ENTRYPOINT, USER, HEALTHCHECK`
- `CMD` vs `ENTRYPOINT`
- `COPY` vs `ADD` (সাধারণত COPY ব্যবহার করো)
- `.dockerignore` বাধ্যতামূলক
- **BuildKit** এখন ডিফল্ট — `RUN --mount=type=cache` দিয়ে প্যাকেজ ক্যাশ (apt/composer/npm) রাখা
- Multi-stage build
- `docker buildx` এর প্রাথমিক ধারণা

**প্র্যাকটিস:**

1. সাধারণ HTML সাইটের Dockerfile
2. PHP ৮.৩/৮.৪ + প্রয়োজনীয় এক্সটেনশন সহ Dockerfile (Alpine বেস পছন্দনীয়)
3. Build করে `docker history` ও সাইজ দেখো

**মাইলস্টোন:** নিজের কোড থেকে ইমেজ বানিয়ে কন্টেইনার চালাতে পারবে।

## ফেজ ৩: ভলিউম ও নেটওয়ার্ক

ভলিউম:

- Named volume, Bind mount, Anonymous volume
- Database-এর জন্য Named volume, Development-এ Bind mount
- `docker volume` কমান্ডগুলো

নেটওয়ার্ক:

- Bridge (ডিফল্ট), Host, None, Overlay
- Custom bridge নেটওয়ার্কে সার্ভিস নাম দিয়ে যোগাযোগ
- `EXPOSE` শুধু ডকুমেন্টেশন — পোর্ট আসলে `-p` দিয়ে খোলে

**মাইলস্টোন:** কন্টেইনার মুছে ফেললেও ডেটা থাকে এবং দুই কন্টেইনার নাম ধরে একে অপরকে ডাকতে পারে।

## ফেজ ৪: Docker Compose (v5.x)

যা শিখবে:

- `compose.yaml` (version ফিল্ড আর লাগে না)
- services, volumes, networks, configs, secrets
- `depends_on` + `condition: service_healthy` + healthcheck
- `profiles` (dev/prod আলাদা করা)
- `docker compose watch` (কোড চেঞ্জ হলে অটো রিলোড — ডেভেলপমেন্টের জন্য দারুণ)
- `docker compose up -d --build`, `down`, `down -v` (সাবধান!), `logs -f`, `ps`, `stats`

**প্র্যাকটিস:** Nginx + PHP-FPM + MySQL + phpMyAdmin + Redis এর পূর্ণ compose ফাইল।

**মাইলস্টোন:** এক কমান্ডে পুরো স্ট্যাক চালু/বন্ধ করতে পারবে।

## ফেজ ৫: রিয়েল Laravel প্রজেক্ট (তোমার জন্য সবচেয়ে গুরুত্বপূর্ণ)

- App (PHP-FPM) + Nginx + MySQL + Redis + Queue Worker + Scheduler
- `.env` এ `DB_HOST=db`, `REDIS_HOST=redis`
- Storage ও cache-এর জন্য volume
- Development-এ bind mount + `compose watch`
- Production-এ multi-stage + non-root user
- Composer ও npm build কন্টেইনারের ভিতরে বা multi-stage-এ

**মাইলস্টোন:** ল্যাপটপে PHP/MySQL ইনস্টল না থাকলেও শুধু Docker দিয়ে পুরো Laravel প্রজেক্ট চলবে।

## ফেজ ৬: অপটিমাইজেশন ও নিরাপত্তা (২০২৭ ফোকাস)

অপটিমাইজেশন:

- `php:8.3-fpm-alpine` বা distroless-স্টাইল
- Multi-stage + BuildKit cache mount
- লেয়ার কমানো, ক্যাশ মুছে ফেলা
- `docker scout` বা Trivy দিয়ে স্ক্যান

নিরাপত্তা:

- Non-root user (`USER` নির্দেশিকা)
- সিক্রেট কখনো Dockerfile বা image-এ না রাখা
- Docker Scout / Trivy দিয়ে vulnerability স্ক্যান
- অপ্রয়োজনীয় পোর্ট বাইরে না খোলা
- Image pinning (digest ব্যবহার)

**প্র্যাকটিস:** Laravel ইমেজকে ৭০০–৮০০ MB থেকে ১৫০–২৫০ MB-এর নিচে নামিয়ে আনো।

## ফেজ ৭: রেজিস্ট্রি ও CI/CD

- Docker Hub / GHCR / AWS ECR
- ট্যাগিং স্ট্র্যাটেজি: `v1.2.3` + commit SHA (latest এড়িয়ে চলো)
- GitHub Actions: build → test → scout scan → push
- Buildx দিয়ে multi-platform (amd64 + arm64)
- VPS-এ `docker compose pull && docker compose up -d`

## ফেজ ৮: অর্কেস্ট্রেশন

- Docker Swarm-এর হালকা ধারণা (এখন কম ব্যবহার হয়)
- Kubernetes: Pod, Deployment, Service, Ingress, ConfigMap, Secret, PVC
- Minikube / Kind / k3d দিয়ে লোকালে একটা Laravel অ্যাপ ডেপ্লয়

**মাইলস্টোন:** একটা Docker image-কে Kubernetes Deployment হিসেবে চালাতে পারবে।

## ফেজ ৯: প্রোডাকশন

- Restart policy, Healthcheck, Resource limits (`cpus`, `mem_limit`)
- Log driver + rotation
- Volume backup + DB dump
- Nginx reverse proxy + Let’s Encrypt (Certbot বা Traefik/Caddy)
- Monitoring: Prometheus + Grafana বা সহজ টুল

## ফেজ ১০: অ্যাডভান্সড (প্রয়োজনমতো)

- Buildx + Bake
- Multi-platform build
- Docker Scout + SBOM + Attestations
- Rootless Docker
- Compose Jobs (experimental)
- Docker-in-Docker (CI-এর জন্য)
- Network গভীরভাবে (DNS, service discovery)

## প্রজেক্ট ক্রম (বাধ্যতামূলক)

1. Nginx দিয়ে static site  
2. PHP + MySQL ছোট অ্যাপ  
3. Laravel + MySQL + Redis + Nginx পূর্ণ স্ট্যাক  
4. Queue worker + Scheduler আলাদা সার্ভিস  
5. GitHub Actions → auto build → VPS deploy (SSL সহ)

## সাধারণ ভুল (২০২৭-এও একই)

- কন্টেইনারে ডেটা রাখা
- পাসওয়ার্ড Dockerfile/image-এ লেখা
- সবসময় `latest` ট্যাগ
- `docker compose down -v` দিয়ে প্রোডাকশন ডেটা মুছে ফেলা
- Root ইউজার দিয়ে চালানো
- বিশাল ইমেজ বানানো

## রিসার্চ নিয়ম

প্রতিটি ফেজে:

1. অফিসিয়াল ডকস (`docs.docker.com`) পড়ো
2. একটা ভালো হাতে-কলমে ভিডিও দেখে সাথে সাথে প্র্যাকটিস করো
3. নিজের ভাষায় নোট লেখো (বইয়ের জন্য দারুণ হবে)
