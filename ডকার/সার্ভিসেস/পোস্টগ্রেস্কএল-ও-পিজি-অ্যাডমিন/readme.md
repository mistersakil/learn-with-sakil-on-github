# অধ্যায়: Docker দিয়ে PostgreSQL ও pgAdmin সেটআপ এবং Nginx-এর সাথে সম্পর্ক

***ভূমিকা***

লিনাক্স সার্ভারে ডাটাবেস ম্যানেজমেন্টের জন্য PostgreSQL একটি শক্তিশালী ও জনপ্রিয় ওপেন সোর্স ডাটাবেস। অনেক সময় আমরা phpMyAdmin-এর মতো গ্রাফিক্যাল ইউজার ইন্টারফেস (GUI) চাই। PostgreSQL-এর জন্য সবচেয়ে ভালো GUI হলো **pgAdmin**।

এই অধ্যায়ে আমরা শিখব কীভাবে Docker Compose ব্যবহার করে PostgreSQL এবং pgAdmin একসাথে চালাতে হয়। পাশাপাশি বুঝব, Nginx ডিজেবল করার পরেও কেন এই সার্ভিসগুলো চলতে থাকে।

## ১. Docker দিয়ে PostgreSQL + pgAdmin চালানো

***প্রয়োজনীয়তা***

- Docker এবং Docker Compose ইনস্টল থাকতে হবে।

চেক করার কমান্ড:

```bash
docker --version
docker compose version
```

***ধাপ ১: প্রজেক্ট ফোল্ডার তৈরি***

```bash
mkdir postgres-pgadmin
cd postgres-pgadmin
```

***ধাপ ২: docker-compose.yml ফাইল তৈরি***

```bash
nano docker-compose.yml
```

নিচের কনফিগারেশনটি লিখুন:

```yaml
services:
  postgres:
    image: postgres:16
    container_name: postgres_db
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: mysecretpassword
      POSTGRES_DB: mydb
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

  pgadmin:
    image: dpage/pgadmin4
    container_name: pgadmin
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: sakil.diu.cse@gmail.com
      PGADMIN_DEFAULT_PASSWORD: admin123
    ports:
      - "5050:80"
    depends_on:
      - postgres

volumes:
  postgres_data:
```

ফাইল সেভ করে বেরিয়ে আসুন।

***ধাপ ৩: কনটেইনার চালু করা***

```bash
docker compose up -d
```

চলমান কনটেইনার দেখতে:

```bash
docker ps
```

***ধাপ ৪: pgAdmin-এ প্রবেশ***
ব্রাউজারে যান:

```text
http://localhost:5050
```

অথবা সার্ভারের আইপি দিয়ে:

```text
http://your-server-ip:5050
```

লগইন তথ্য:

- Email: `sakil.diu.cse@gmail.com`
- Password: `admin123`

***ধাপ ৫: PostgreSQL সার্ভার কানেক্ট করা***

1. pgAdmin-এ **Add New Server** ক্লিক করুন।
2. **General** ট্যাবে নাম দিন (যেমন: Local PostgreSQL)।
3. **Connection** ট্যাবে নিচের তথ্য দিন:

| ফিল্ড                  | মান                |
|------------------------|--------------------|
| Host name/address      | `postgres`         |
| Port                   | `5432`             |
| Maintenance database   | `mydb`             |
| Username               | `postgres`         |
| Password               | `mysecretpassword` |

**গুরুত্বপূর্ণ:** Host-এ `localhost` না দিয়ে সার্ভিস নাম `postgres` দিতে হবে। কারণ Docker নেটওয়ার্কের ভিতরে কনটেইনারগুলো একে অপরকে সার্ভিস নাম দিয়ে চিনে।

---

### ২. Nginx ডিজেবল করার পরেও কেন PostgreSQL ও pgAdmin চলছে?

অনেক ব্যবহারকারী Nginx বন্ধ করার পরেও দেখেন যে pgAdmin এখনও অ্যাক্সেস করা যাচ্ছে। এটা সম্পূর্ণ স্বাভাবিক।

***কারণ ব্যাখ্যা:***

| সার্ভিস       | Nginx-এর উপর নির্ভরশীলতা | কীভাবে চলে                  |
|---------------|---------------------------|-----------------------------|
| PostgreSQL    | নেই                       | Docker কনটেইনার             |
| pgAdmin       | নেই                       | Docker কনটেইনার + পোর্ট ম্যাপিং |
| Nginx         | আলাদা সার্ভিস             | রিভার্স প্রক্সি হিসেবে কাজ করে |

- Docker Compose-এ আমরা পোর্ট ম্যাপিং করেছি (`5050:80`)। এর ফলে হোস্ট মেশিনের ৫০৫০ পোর্ট সরাসরি pgAdmin কনটেইনারের সাথে যুক্ত হয়।
- Nginx শুধুমাত্র একটি রিভার্স প্রক্সি। এটি বন্ধ করলেও Docker কনটেইনারগুলো স্বাধীনভাবে চলতে থাকে।
- তাই `http://server-ip:5050` দিয়ে সরাসরি pgAdmin অ্যাক্সেস করা যায়।

***চেক করার কমান্ড:***

```bash
# চলমান কনটেইনার দেখা
docker ps

# ৫০৫০ পোর্টে কে শুনছে
sudo ss -tulnp | grep 5050
```

---

### ৩. দরকারি কমান্ডসমূহ

| কাজ                            | কমান্ড                          |
|--------------------------------|---------------------------------|
| কনটেইনার চালু করা              | `docker compose up -d`          |
| কনটেইনার বন্ধ করা              | `docker compose down`           |
| ভলিউমসহ সব মুছে ফেলা           | `docker compose down -v`        |
| শুধু pgAdmin বন্ধ করা          | `docker stop pgadmin`           |
| লগ দেখা                        | `docker compose logs -f`        |
| কনটেইনার স্ট্যাটাস             | `docker ps`                     |

---

### ৪. সারাংশ

- Docker Compose দিয়ে PostgreSQL এবং pgAdmin খুব সহজে একসাথে চালানো যায়।
- pgAdmin হলো PostgreSQL-এর জন্য phpMyAdmin-এর সমতুল্য GUI টুল।
- এই সেটআপ Nginx-এর উপর নির্ভর করে না। তাই Nginx ডিজেবল করলেও সার্ভিসগুলো চলতে থাকে।
- পোর্ট ম্যাপিং (`5050:80`) থাকার কারণে সরাসরি ব্রাউজার থেকে অ্যাক্সেস করা যায়।
- ডাটা `postgres_data` ভলিউমে সেভ থাকে, তাই কনটেইনার মুছে ফেললেও ডাটা নষ্ট হয় না (যতক্ষণ `-v` ফ্ল্যাগ ব্যবহার না করা হয়)।

এই অধ্যায়ের মাধ্যমে আপনি লিনাক্স সার্ভারে Docker ভিত্তিক PostgreSQL পরিবেশ তৈরি ও ম্যানেজ করতে পারবেন।
