# iHelpBD - ফেজ ১: ইউনিফাইড ওয়ালবোর্ড

***সময়: ১ থেকে ৮ সপ্তাহ | টিম: ৬ জন | লক্ষ্য: ৪ টা ওয়ালবোর্ড = ১ টা***

## ১. প্রজেক্ট ওভারভিউ

**বর্তমান সমস্যা:** ৪ টা ওয়ালবোর্ড, রিপোর্টে গরমিল, মাঝে মাঝে হ্যাক, বিজনেস টিমের সাথে হ্যান্ডওভার হয় না।
**ফেজ ১ এর লক্ষ্য:** `wallboard.ihelpbd.com/v2` - একটাই কোডবেস, ১০০% নির্ভুল রিপোর্ট, ২ সেকেন্ডে লাইভ আপডেট।

***টিম স্ট্রাকচার (৬ জন):***

* **UI টিম (২ জন) S1, S2:** Next.js 15, Tailwind, Socket.io Client
* **API টিম (২ জন) A1, A2:** Laravel 11, MySQL, ClickHouse, Redis, Kafka
* **প্ল্যাটফর্ম টিম (২ জন) P1 DevOps, P2 QA/Coordinator:** Docker, Nginx, QA, বিজনেস টিমের সাথে ব্রিজ

**টেক স্ট্যাক ফাইনাল:**
`Monorepo + Docker + Laravel API + Next.js UI + MySQL + ClickHouse + Redis + Socket.io`

## ২. ৮ সপ্তাহের বিস্তারিত টাইমলাইন

### **সপ্তাহ ১: অডিট এবং ফ্রিজ - Foundation**

***উদ্দেশ্য: আর কোনো নতুন ভার্সন বানানো যাবে না।***

* **A1 + A2 (৩ দিন):** ৪ টা ওয়ালবোর্ডের সব ফিচার, সব API, Asterisk CDR টেবিলের স্ট্রাকচার অডিট করে একটা Excel শিট বানানো। কোন ক্লায়েন্টে কোন ভার্সন চলে তার লিস্ট।
* **S1 (৩ দিন):** ৪ টা ওয়ালবোর্ডের স্ক্রিনশট নিয়ে কমন UI প্যাটার্ন বের করা।
* **P1 (২ দিন):** Monorepo তৈরি `ihelpbd-master-platform/`, `docker-compose.yml` (Laravel, MySQL, Redis, ClickHouse), GitLab CI, `.env.example` সেটআপ। পুরনো ৪ টা কোড `legacy/` ফোল্ডারে আর্কাইভ।
* **ডেলিভারেবল:** অডিট ডকুমেন্ট + ফ্রিজ নোটিশ + রেডি Monorepo

### সপ্তাহ ২: ডিজাইন ও API চুক্তি - Contract

***এই সপ্তাহে কোড না, শুধু চুক্তি। চুক্তি ছাড়া কোড লিখলে আবার ৫ম ভার্সন হবে।***

* **A1 (৫ দিন):**  API লেখা:
    `GET /api/v1/wallboard/live-summary`
    `GET /api/v1/wallboard/queues`
    `GET /api/v1/wallboard/agents/live`
    `GET /api/v1/reports/accuracy-check`
    এবং `spatie/laravel-permission` দিয়ে ৩ টা রোল ডিজাইন - `client_admin`, `supervisor`, `viewer`

* **S1 + S2 (৫ দিন):** UI তে ১ টা মাস্টার ওয়ালবোর্ড ডিজাইন। ৩ টা ভিউ:
    ১. Admin View - সব ক্লায়েন্টের সব কিউ
    ২. Supervisor View - শুধু তার টিমের এজেন্ট
    ৩. Client Admin View - শুধু তার কোম্পানির ডাটা

### **সপ্তাহ ৩-৪: ব্যাকএন্ড এর মূল কাজ - Report Accuracy Fix**

***এটাই সবচেয়ে গুরুত্বপূর্ণ ২ সপ্তাহ। রিপোর্টের গরমিল এখানেই চিরতরে বন্ধ হবে।***

* **A1 + A2 (১০ দিন):**
    ১. **CDR Processor Service:** Asterisk এর `cdr` টেবিল থেকে সরাসরি রিপোর্ট দেখানো বন্ধ। একটা Laravel Schedule Job প্রতি ৫ মিনিট পর পর চলবে। কাজ:
       - ডুপ্লিকেট `uniqueid` বাদ
       - `Asia/Dhaka` টাইমজোন ফিক্স
       - কলকে `inbound`, `outbound`, `missed`, `abandoned` এ ভাগ করা
       - পরিষ্কার ডাটা MySQL এবং দ্রুত রিপোর্টের জন্য ClickHouse এ পাঠানো

    ২. **Redis Caching:** লাইভ কিউ এর ডাটা ২ সেকেন্ডের জন্য ক্যাশ - `Cache::remember('live_queue:client_12', 2, ...)`। ১০০ জন একসাথে দেখলেও MySQL এ চাপ পড়বে না।

    ৩. **Role Base API:** Middleware দিয়ে চেক - Client A, Client B এর ডাটা দেখতে পারবে না।

* **P1 (সমান্তরালে):** ClickHouse + Redis + Laravel Queue Worker এর Docker সেটআপ ফাইনাল।

#### **সপ্তাহ ৫-৬: ফ্রন্টএন্ড লাইভ - Realtime Wallboard**

* **S1 + S2 (১০ দিন):**
    ১. `apps/wallboard` - Next.js 15 প্রজেক্ট সেটআপ, `ui-kit` থেকে কার্ড, টেবিল, চার্ট নিয়ে লেআউট।
    ২. **Realtime:** Laravel Reverb / Socket.io এর সাথে কানেক্ট। কিউতে কল বাড়লে, এজেন্ট `Free` থেকে `Busy` হলে পেজ রিলোড ছাড়াই সাথে সাথে আপডেট হবে।
    ৩. কম্পোনেন্ট: Live Queue Card, Agent Status Grid, SLA Meter, Today's Call Chart

* **A1 (সমান্তরালে):** Socket Event পাঠানো - `broadcast(new QueueUpdatedEvent($data))`

#### **সপ্তাহ ৭: ইন্টিগ্রেশন ও ডকারাইজেশন**

***সবাই মিলে (৫ দিন):***

* UI + API + Socket + ClickHouse একসাথে কানেক্ট

* `docker-compose up --build` দিলেই Wallboard + API + MySQL + Redis + ClickHouse চালু হবে

* `.env` যেন গিটে না যায়, API তে `throttle:60,1` রেট লিমিট বসানো

### **সপ্তাহ ৮: টেস্টিং, সিকিউরিটি ও লাইভ**

***এই সপ্তাহে কোনো নতুন ফিচার না, শুধু টেস্ট।***

* **P2 - QA (৭ দিন): Accuracy Test:** একটা ক্লায়েন্টের জন্য পুরনো ওয়ালবোর্ড এবং নতুন ওয়ালবোর্ড ৭ দিন পাশাপাশি চালানো। প্রতিদিন ২ টার রিপোর্ট Excel এ মিলিয়ে দেখা। ১০০% না মিললে লাইভ হবে না।

* **P1 - Security (৫ দিন):** হ্যাক বন্ধ করার জন্য - Nginx WAF, Fail2Ban, SSH Key ছাড়া লগইন বন্ধ, Docker Image Non-Root User এ চালানো।

* **P2 - Handover (২ দিন):**
১ টা  Doc - ওয়ালবোর্ড কিভাবে ব্যবহার করতে হয়

**৮ম সপ্তাহের শেষ দিনে ডেলিভারেবল:** `wallboard.ihelpbd.com/v2` লাইভ।

## ৩. প্রতিদিনের নিয়ম

সকাল ১০ টায় ১৫ মিনিট স্ট্যান্ডআপ। এই ৮ সপ্তাহের ডকুমেন্ট অনুযায়ী কাজ করলে আপনার ৪ টা ওয়ালবোর্ড ১ টা হয়ে যাবে এবং রিপোর্ট ১০০% মিলবে।
